# Ver1.1 CPU最適化 提案メモ

対象: `Source/Logic/BassSynthVoice.h` の `renderNextBlock` と `Source/DSP/WavetableOscillator.h`
方針: **音（出力波形）を変えない**ことを最優先。実装は次フェーズ。

---

## 候補A（最優先・音は完全に同一）: Morph再計算を「変化したときだけ」にする

### 現状
`renderNextBlock` の毎サンプルのループ内（337〜349行付近）で、毎サンプル必ず

```cpp
oscillator.setMorphA(currentModeA, aA, sA);
oscillator.setMorphB(currentModeB, aB, sB);
oscillator.setMorphC(currentModeC, aC, sC);
```

を呼んでいる。`setMorphX` は内部で `precomputeMorphs()` を呼び、`exp` / `pow` / 除算を含む
重い前計算を **毎サンプル×3系統** 実行している。Morphが動いていない（ノブ固定・変調なし）
ときも毎回フル計算しているのが無駄。

### 対策
直前の `(mode, amt, shift)` を保持し、**値が変わったサンプルだけ** `setMorphX` を呼ぶ。
変調が止まって値が一定になれば前計算はスキップされる。出力は1ビットも変わらない。

```cpp
// メンバに前回値を保持（例）
float lastAA=1e9f,lastSA=1e9f,lastAB=1e9f,lastSB=1e9f,lastAC=1e9f,lastSC=1e9f;
int   lastModeA=-1,lastModeB=-1,lastModeC=-1;

// ループ内（setMorphX の置き換え）
if (currentModeA!=lastModeA || aA!=lastAA || sA!=lastSA) {
    oscillator.setMorphA(currentModeA, aA, sA);
    lastModeA=currentModeA; lastAA=aA; lastSA=sA;
}
// B, C も同様
```

効果: 変調なし時のMorph前計算コストがほぼ0に。リスク: なし（同値ならスキップするだけ）。

---

## 候補B（音は完全に同一）: 周波数の再計算頻度を下げる

### 現状
毎サンプル `oscillator.setFrequency(cf)`（362行付近）→ 内部で `recalculate()` を呼び、
voiceごとに `pow` / `cos` / `sin` / `sqrt` を計算してパン・アンプ・増分テーブルを作り直している。
グライド中以外は周波数が一定なのに毎サンプル作り直しているのが無駄。

### 対策
`setFrequency` 側で「前回と同じ周波数なら `recalculate()` をスキップ」する。

```cpp
void setFrequency(float freqHz) {
    if (freqHz == baseFreq) return;   // ★ 同一なら再計算しない
    baseFreq = freqHz; recalculate();
}
```

効果: グライドしていない通常発音時、`recalculate` のコストがほぼ0に。
リスク: なし（同一周波数なら結果が同じため）。
注意: `setUnison*` / `setStereoWidth` など他の `recalculate` 経路はそのまま残す（影響なし）。

---

## 候補C（「ごく僅かな差は許容」枠・任意）: 高コスト関数の近似

聴感上わからない範囲での近似。やるなら効果は大きいが、検証（試聴＋スペクトル確認）必須。

1. **ドリフトの sin**（WavetableOscillator 343/385行）: すでに32サンプルに1回だが、
   `std::sin` を多項式近似に置換可能。ごく微小なLFO形状差のみ。
2. **`dynamicPitchMult = std::pow(2.0f, ...)`**（299行）: 毎サンプルの `pow` を
   `exp2` あるいは差分更新に。ピッチ微差の可能性があるため候補Cに分類。

推奨順: **A → B を先に実装・試聴 → 余裕が無ければ C は見送り**。
A・Bだけでも変調・グライド未使用時の負荷が目に見えて下がるはず。

---

## 実装時のチェック
- A・B適用後、ノブ固定／変調あり／グライドあり の3パターンで出力波形が
  従来と一致することを確認（録音して差分、または同一プリセットで聴き比べ）。
- リアルタイムCPUメーターで idle時・1音・最大ユニゾン時を比較。
