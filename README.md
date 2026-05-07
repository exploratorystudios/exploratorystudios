<h1 align="center">Exploratory Studios</h1>

<p align="center">
  <img src="https://raw.githubusercontent.com/exploratorystudios/exploratorystudios/main/logo.gif" alt="Exploratory Studios Logo">
</p>

<p align="center">
  <em>The limitation is where the interesting part begins.</em>
</p>

<p align="center">
  <a href="https://github.com/exploratorystudios/TILM2"><img src="https://img.shields.io/badge/TILM2-Language_Model_on_TI--84_CE-67e8f9?style=flat&labelColor=0891b2&logo=python&logoColor=cffafe" alt="TILM2"></a>
  &nbsp;&nbsp;
  <a href="https://github.com/exploratorystudios/HermesOptimus-v3"><img src="https://img.shields.io/badge/HermesOptimus_V3-Neural_Net_on_a_TI--84-f9a8d4?style=flat&labelColor=db2777&logo=python&logoColor=fce7f3" alt="HermesOptimus V3"></a>
  &nbsp;&nbsp;
  <a href="https://github.com/exploratorystudios/Tetra-Tennis-Golf"><img src="https://img.shields.io/badge/Tetra_Tennis_Golf-25_Lines_of_Tetris-c4b5fd?style=flat&labelColor=7c3aed&logo=python&logoColor=ede9fe" alt="Tetra Tennis Golf"></a>
</p>
<p align="center">
  <a href="https://github.com/exploratorystudios/shmup-golf"><img src="https://img.shields.io/badge/Shmup_Golf-7_Lines_of_Shmup-fca5a5?style=flat&labelColor=dc2626&logo=python&logoColor=fee2e2" alt="Shmup-Golf"></a>
  &nbsp;&nbsp;
  <a href="https://github.com/exploratorystudios/QuickChat"><img src="https://img.shields.io/badge/QuickChat-Local_LLM_Client-93c5fd?style=flat&labelColor=2563eb&logo=qt&logoColor=dbeafe" alt="QuickChat"></a>
  &nbsp;&nbsp;
  <a href="https://github.com/exploratorystudios/DitherPal"><img src="https://img.shields.io/badge/DitherPal-Image_%26_Video_Dithering-fcd34d?style=flat&labelColor=d97706&logo=python&logoColor=fef3c7" alt="DitherPal"></a>
  &nbsp;&nbsp;
  <a href="https://github.com/exploratorystudios/gemspire"><img src="https://img.shields.io/badge/Gemspire-Match--3_Roguelike-6ee7b7?style=flat&labelColor=059669&logo=javascript&logoColor=d1fae5" alt="Gemspire"></a>
</p>

---

I build things that probably shouldn't work as well as they do. Games in single-digit line counts, neural networks on graphing calculators, tools that do something genuinely useful and stay out of your way. The hardware constraints aren't obstacles I'm working around. They're usually where the interesting design decisions happen.

---

## Machine Learning

### [TILM2](https://github.com/exploratorystudios/TILM2)

> A syllable-level autoregressive language model that runs on a TI-84 Plus CE.

Trained in Python with NumPy, deployed entirely on-calculator. The model uses phonetic syllable tokenization with a 198-dimensional hidden layer factored into six parallel output heads (onset, nucleus, coda, stress, word boundary, and role) with a 10-syllable context window and separate 16-dimensional discourse and 8-dimensional word state that carry meaning across generation. The architecture is built around the TI-84's matrix constraints and ~150KB of RAM. H1 contributions are precomputed ahead of time so the calculator adds vectors instead of multiplying full matrices at each step. A full generation run takes 2.5 to 3 hours. You have to keep an eye on it for garbage collection prompts. I find that adds something.

### [HermesOptimus V3](https://github.com/exploratorystudios/HermesOptimus-v3)

> A neural network word classifier deployed on a TI-84 Plus Silver Edition.

A 30-50-12 feedforward network trained in Python with NumPy, deployed as TI-BASIC with weights exported to calculator-native matrix files. Classifies and autocorrects 24 four-letter words (typos, scrambled letters, missing characters) entirely on a graphing calculator with 56KB of RAM. Uses 12-bit binary output encoding matched via Hamming distance instead of softmax, because the hardware made softmax impractical and Hamming distance turned out to be the smarter call anyway.

---

## Code Golf

Complete, playable games in as few lines of Python as possible. No semicolons. Each line is one logical statement. Each ships with a readable reference version alongside the compressed one, so you can understand it, look at the compressed version again, and hopefully feel something different the second time.

### [Tetra Tennis Golf](https://github.com/exploratorystudios/Tetra-Tennis-Golf)

> Full Tetris in 25 lines of Python.

All seven tetrominoes, hold, ghost piece, hard drop, rotation, next-piece preview, and progressive difficulty, running in your terminal, cross-platform, in a single function called `g`. The board uses Unicode half-block characters to pack double vertical resolution into each terminal row. "Tetris" is trademarked. "Tennis" is there for the vibes.

### [Shmup-Golf](https://github.com/exploratorystudios/shmup-golf)

> A horizontal shoot-em-up in 7 lines of Python.

You are two characters. The world scrolls past you. A procedurally generated cave tries to kill you, three enemy types try to kill you, and spikes try to kill you. A powerup occasionally makes you invincible and triggers rapid autofire. The function signature alone contains the entire cross-platform input system. Line 3 is `while 1:`.

---

## Tools

### [QuickChat](https://github.com/exploratorystudios/QuickChat)

> A desktop chat client for local language models through Ollama.

A full-featured PySide6 application for running local LLMs privately. Real-time reasoning visualization, vision input, conversation forking, LaTeX rendering, and adaptive streaming. Dark and light themes, persistent chat history, Markdown and JSON export. No telemetry. Everything stays on your machine.

### [DitherPal](https://github.com/exploratorystudios/DitherPal)

> High-performance image and video dithering.

Five dithering algorithms (Floyd-Steinberg, Jarvis-Judice-Ninke, Bayer, Rosette halftone, and Text Pattern), all JIT-compiled through Numba for up to 500x speedups. Supports images, animated GIFs, and video. KMeans color palette extraction, memory-aware chunked processing for large files, and a clean PyQt6 interface.

---

## Games

### [Gemspire](https://github.com/exploratorystudios/gemspire)

> A match-3 roguelike in one HTML file.

Fully browser-based, vanilla JavaScript, no frameworks, no build step. Match gems across a 7×7 board, collect relics between floors, and spend crystals on permanent upgrades across runs. Over 20 relics, a prestige economy, hard mode, and enough depth to keep pulling you back in, all in a single self-contained file.

---

<p align="center">
  <em>The constraint does not limit the work. It becomes the work.</em>
</p>
