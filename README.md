![Downloads](https://img.shields.io/github/downloads/OTODESK4193/BassSynth/total.svg)

# BassSynth

**BassSynth** is a next-generation wavetable bass synthesizer plugin (VST3/AU) built with C++ and the JUCE framework. Designed for modern electronic music producers, it combines earth-shattering low-end with crystalline top-end textures, featuring an intuitive Ableton-style dark UI.

<img src="ScreenShots/Screenshot_1.png" width="600">

👉 **[Watch the Demo Video on X (動作デモ動画)](https://x.com/kijyoumusic/status/2047948478392021324?s=20)**

---
## 🚀 What's New in V1.2.0 (主な新機能)近日中にリリース予定！！

- **🎹 Polyphony Support (ポリフォニック化)**: Rebuilt the voice engine from monophonic to polyphonic, supporting up to **24 voices** for rich chords and pads. Includes a dedicated GUI slider to adjust the max voice count dynamically.
- **⚡ Advanced Modulation Matrix Sources (マトリクス変調ソースの拡張)**: Extended the modulation matrix with 3 powerful performance-oriented sources:
  - **Velocity (打鍵速度)**: Use note velocity as a modulator.
  - **Vel > N (Velocity閾値ゲート)**: Generates a ramp modulation value only when note velocity exceeds a configurable threshold `N` (1-127).
  - **Trig.Ran (トリガーランダム)**: Generates a unique random value per note on every trigger event.
- **⚙️ Modulator Config Panel (モジュレータ・コンフィグ搭載)**: Added dedicated options for the new performance sources, including **Smoothing Times** (adjustable lag from `0.5ms` to `200ms` to prevent clicks) and **Polarity Switch** (switchable between Unipolar and Bipolar mode).
- **🧪 Formula-Based Wavetable Generation (数式生成)**: Integrated a math formula editor panel to generate custom wavetable shapes using mathematical equations (e.g., Sine, Cos) in real-time. Wavetables can be exported as `.wav` or previewed instantly.
- **🌀 New FX Suite & Routing (空間系エフェクト＆順序入れ替え)**: Integrated high-quality **Dimension Chorus**, **Stereo Delay**, and **Diffusion Cloud Reverb (Simple Reverb)**. Features a dynamic routing editor where you can drag and swap the FX chain order (Slot 1, 2, 3) on the fly.
- **✨ Enhanced Modulators (LFOのTriangle波追加＆One-Shotモード)**: Added **Triangle LFO** shape (making 5 total shapes: Sine, Saw, Pulse, Random, Triangle) and extended trigger options with **One-Shot** mode (stopping after 1 cycle) alongside Free and Retrig.
- **📦 Hardcoded Factory Assets (バイナリ埋め込み)**: Embedded **29 factory presets** and **13 custom wavetables** directly into the plugin binary, eliminating external path dependencies for portability and speed.
- **🗃️ Wavetable Browser Integration (波形ブラウザ統合)**: Integrated the 13 built-in custom wavetables directly inside the `Factory -> Basic` tab in the browser for easy navigation and favoriting (★).
- **⚡ Real-Time Render Optimization (描画ラグの解消)**: Refactored the UI waveform render logic to bind parameters to the latest active voice. This resolves lag on the display and prevents thread contention.

---

⚠️ 警告：自己責任でご使用ください
本プラグインの設定によっては、突発的な大音量が発生する場合があります。聴覚や音響機器へのダメージについて、開発者は一切の責任を負いません。ボリュームを下げた状態での試聴と、マスターへのリミッター設置を強く推奨します。
## ✨ Key Features

### 🌊 Advanced Wavetable Engine
- **10 Math-Generated Factory Morphing Tables**: 64-frame fluid morphing waves (Basic Morph, PWM Sweep, Sync Sweep, Vowel Sweep, etc.) calculated in real-time at zero-latency.
- **Custom Wavetable Import**: Drag & Drop or browse your own `.wav` files with high-quality bandlimited interpolation.
- **Smart Browser**: Instantly filter through Factory, User, and Favorite (★) wavetables. Includes a real-time search bar.
- **Spectral Morphing**: 13 morph modes (Bend, PWM, Sync, Mirror, Vocode, Comb, etc.) across 3 independent morphing slots (A, B, C).

### 🎨 Color IR Engine & Sparkle Arp
- **Auto-Chord Learning**: Play a MIDI chord, and the synth learns it to generate a custom Impulse Response (IR) matching the harmony.
- **Convolution Resonator**: Apply 5 unique IR types (Crystal Saw, Shimmer PWM, Harmonic Bell, Stacked Shimmer, Crystal Pluck) for metallic or ethereal top-end layers.
- **Sparkle Arp**: A dedicated, dynamic arpeggiator that runs through the learned chord structure to create high-frequency rhythmic textures.

### 🎛️ Dual Filter & Shaping
- **State-Variable Filters**: Two independent filters (LPF, HPF, BPF, Notch, Comb, Analog LPF) that can be routed in Serial or Parallel.
- **Distortion Unit**: Built-in Drive, Sine Shaper, Downsampler (Rate), and Bitcrusher for aggressive tone shaping.

### 🧬 Modulation Powerhouse
- **MSEG (Multi-Stage Envelope Generator)**: 2 custom visual envelope editors for complex, repeating rhythmic shapes.
- **LFOs & Mod Envelopes**: 3 LFOs and 3 Mod Envelopes with comprehensive sync and trigger options.
- **10-Slot Matrix**: Route any modulator to Pitch, FM, Morph A/B/C, Filter Cutoff/Reso, or Master Gain with pinpoint accuracy.

### 🎚️ Master Dynamics
- **True OTT**: Built-in 3-band upward/downward multiband compression.
- **Soothe-Style Resonance Suppression**: Instantly tame harsh frequencies (Selectivity, Sharpness, Focus controls).
- **Zero-Latency Peak Limiter**: Transparent, latency-free brickwall limiter with adjustable ceiling to keep your mix clipping-free.

---

## 🛠 Installation

### 📖 User Manual
For detailed instructions on all parameters and advanced synthesis techniques, please refer to the:
👉 [**BassSynth Official User Manual (PDF)**](BassSynth_User_Manual.pdf)

### For Users (Download)
1. Download the latest release from the [Releases](https://github.com/OTODESK4193/BassSynth/releases) page. 
2. Move the `BassSynth.vst3` (Windows) to your DAW's plugin folder.
   - **Windows VST3**: `C:\Program Files\Common Files\VST3`


---

## 🎹 Quick Start Guide
1. **Browse Wavetables**: Click the `BROWSE` button top left. Use the search bar or click the `★` icon to favorite shapes.
2. **Learn a Chord (Color Mode)**: Click `COL` to open the Color panel. Hit `LEARN CHORD`, play a chord on your MIDI keyboard, and hear the harmonic resonance build up over your bass.
3. **Edit MSEGs**: Navigate to the `MSEGs` tab. Double-click to add/remove points, and Alt+Drag (or Option+Drag) to curve the lines.

---

## 📝 Version History

### V1.2.0 (Latest)
- **Polyphony Support**: Rebuilt the voice engine from monophonic to polyphonic, supporting up to **24 voices** (adjustable via UI slider) for rich chords and pads.
- **Advanced Modulation Matrix Sources**: Integrated 3 new performance-oriented modulation sources: **Velocity** (note velocity), **Vel > N** (velocity exceeds configurable threshold gate), and **Trig.Ran** (per-note random generator on key trigger).
- **Modulator Config Panel**: Added options to adjust **Smoothing Time** (`0.5ms` to `200ms` lag via 1st-order lowpass filter) and toggle **Polarity** (Unipolar/Bipolar) for each performance source.
- **Formula-Based Wavetable Generation**: Added a math formula editor panel to generate custom wavetable shapes mathematically (e.g., Sine, Cos) in real-time.
- **New FX Suite & Routing**: Integrated **Dimension Chorus**, **Stereo Delay**, and **Diffusion Cloud Reverb (Simple Reverb)** with support for dynamic routing order swap in the UI.
- **Enhanced Modulators**: Added new **Triangle LFO** shape (5 shapes total) and **One-Shot** LFO trigger mode (stops after 1 cycle).
- **Hardcoded Factory Presets**: Added **29 factory presets** (categorized into Bass, Bell, Color Presets, Guitar, Lead, PAD, Synth) embedded directly into the binary, with sequential numbering.
- **Built-in Wavetables**: Hardcoded **13 custom wavetables** (e.g., Circle VPS, Deep Saw, etc.) directly into the binary and integrated them into the `Factory -> Basic` list in the Wavetable Browser.
- **Real-time Waveform Rendering**: Refactored the UI waveform render logic to pull modulation data from the latest active voice, resolving lag on the display and preventing thread contention.
- **Version Bump**: Upgraded the project name, output filename, and header display name to `BassSynth1.2.0`.

---

## License

This project is licensed under the GNU Affero General Public License v3.0 (AGPLv3) - see the [LICENSE](LICENSE) file for details.
This software is built using the **JUCE 8** framework. In accordance with JUCE 8's open-source licensing terms, this entire project is distributed under the AGPLv3.


---

## 👨‍💻 Credits
- Developed by **OTODESK** [https://x.com/kijyoumusic](https://x.com/kijyoumusic)
- Built with [JUCE](https://juce.com/)
- UI Inspired by modern digital workstations.
