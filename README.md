<h1 align="center">Exploratory Studios</h1>

<p align="center">
  <em>The limitation is where the interesting part begins.</em>
</p>

<p align="center">
  <a href="https://github.com/exploratorystudios/Tetra-Tennis-Golf"><img src="https://img.shields.io/badge/Tetra_Tennis_Golf-25_lines_of_Tetris-8B5CF6?style=for-the-badge" alt="Tetra Tennis Golf"></a>
  <a href="https://github.com/exploratorystudios/shmup-golf"><img src="https://img.shields.io/badge/Shmup_Golf-7_lines_of_shmup-EF4444?style=for-the-badge" alt="Shmup-Golf"></a>
  <a href="https://github.com/exploratorystudios/QuickChat"><img src="https://img.shields.io/badge/QuickChat-local_LLM_client-3B82F6?style=for-the-badge" alt="QuickChat"></a>
  <a href="https://github.com/exploratorystudios/DitherPal"><img src="https://img.shields.io/badge/DitherPal-image_dithering-F59E0B?style=for-the-badge" alt="DitherPal"></a>
  <a href="https://github.com/exploratorystudios/gemspire"><img src="https://img.shields.io/badge/Gemspire-match--3_roguelike-10B981?style=for-the-badge" alt="Gemspire"></a>
  <a href="https://github.com/exploratorystudios/HermesOptimus-v3"><img src="https://img.shields.io/badge/HermesOptimus_V3-neural_net_on_a_TI--84-EC4899?style=for-the-badge" alt="HermesOptimus V3"></a>
</p>

---

I build software that treats constraints as creative material — games compressed into single-digit line counts, neural networks deployed on graphing calculators, desktop applications where performance isn't an afterthought but the whole conversation. Every project here is a small study in what happens when you take a limitation seriously, not as something to work around, but as the parameter that shapes the architecture itself.

The through-line is a question: *what survives compression?* A 25-line Tetris clone isn't a parlor trick. It's an excavation. Strip away every luxury and what remains are the mechanics that actually mattered — the ones you never noticed when the code had room to breathe. That is what I find interesting. The constraint reveals the essential geometry of the thing.

I work primarily in Python, occasionally in vanilla JavaScript, and once on a TI-84 Plus Silver Edition because the problem required it and the hardware had something to teach me.

---

## Code Golf

Complete, playable games compressed into as few lines of Python as possible. No semicolons. Each line is one logical statement. Each ships with a readable unobfuscated reference — so you can understand what the compressed version does, look at the compressed version again, and feel something different the second time.

### [Tetra Tennis Golf](https://github.com/exploratorystudios/Tetra-Tennis-Golf)

> Full Tetris in 25 lines of Python.

All seven tetrominoes, hold system, ghost piece, hard drop, soft drop, rotation, next-piece preview, progressive difficulty, and cross-platform input handling across Windows, Linux with Xlib, and a plain curses fallback. The entire game lives inside a single function called `g`, and that function's default arguments contain the complete input subsystem — a three-way platform-dependent ternary evaluated once at definition time.

Pieces are stored as bitmasks in 4x4 grids. Collision detection walks those bits against the board. The board itself stores two logical rows per visual row, because the rendering uses Unicode half-block characters (`▄` `▀` `█`) to pack double vertical resolution into each terminal cell. The ghost piece renders as `▒`, placed blocks as `█`, and the whole thing draws in a single `or`-chain expression that relies on `s.erase()` returning `None` to keep the chain alive. The hold system, hard drop, horizontal movement, rotation, and piece spawning all resolve simultaneously inside one `next(...)` expression with walrus operators.

"Tetris" is trademarked. "Code Golf" describes the exercise. "Tennis" is there for the vibes.

### [Shmup-Golf](https://github.com/exploratorystudios/shmup-golf)

> A horizontal shoot-em-up in 7 lines of Python.

You are `⠶⠄` on Linux, `▐-` on Windows. Two characters. That is the ship. It sits at column 3, and the world scrolls past it.

The cave ceiling and floor each wander by independent random walk every frame, clamped so neither takes more than a third of the screen. Three enemy types share a four-field structure (`[x, y, type, hp]`) but diverge in behavior: slow enemies (3 HP, 1 pixel per frame, sprites that visually degrade as damage accumulates), fast enemies (1 HP, 2 pixels per frame), and sine-wave enemies whose y-position recalculates every frame as `round(sin(x * 0.4) * 2)`, clamped to the open cave at their column. Spikes scroll in from the right edge — `│` body, `▼` or `▲` tip — and kill on contact.

Powerups drop from destroyed enemies at a 1-in-16 chance. Collecting one activates 120 frames of shield and rapid autofire. The HUD reads `RAPID` with lightning bolts, and the ship transforms into its final form: `⠶⠗`. An enemy bullet absorbed by the shield scores 10 points and zeroes it. That is the only defensive mechanic.

The starfield deserves its own mention: `W//4` stars at hash-derived fixed positions, scrolling at `t//13`, with braille sub-pixel animation on Linux that alternates `⠠` and `⠄` every 13 frames to simulate half-cell leftward movement within characters that do not actually support sub-positions. On Windows, they are `·`. Windows gets dots.

The function signature alone — line 1 — contains the complete input system for three platforms, a mutable cooldown tracker, and rising-edge detection for the fire button. Line 2 unpacks twelve variables while initializing the terminal, generating both terrain arrays, and conditionally calling `chcp 65001` on Windows so the Unicode renders instead of crashing. Line 3 is `while 1:`. Line 5 is the entire physics and collision engine in one expression. Line 6 draws the screen in nine layered passes. Line 7 is `__import__("curses").wrapper(g)`.

Scoring: 25 per enemy hit, 10 per bullet absorbed, 5 per frame overlapping a powerup, 1 per frame alive.

---

## Tools

### [QuickChat](https://github.com/exploratorystudios/QuickChat)

> A desktop chat client for local language models through Ollama.

Built with PySide6 for the interface, SQLAlchemy over SQLite for persistent conversation storage, and `qasync` to bridge Python's asyncio with Qt's event loop so the UI never freezes during inference. The Ollama client is fully asynchronous and caches model metadata in-memory to avoid repeated API calls.

The thinking mode system uses dual detection: parameter-based for models that support it through the Ollama API, and directive-based (`/think` `/no_think` tags) for GGUF models like Qwen3 and DeepSeek-R1 that handle reasoning through prompt directives. During generation, `<think>` tags are stripped in real time, separated from the response, and displayed in collapsible thought bubbles. Thinking can be toggled per-message.

Vision support auto-enables for multimodal models — LLaVA, MoonDream, LLaMA 3.2 Vision — and sends only the latest attached image to avoid context bloat while preserving thumbnails in history. LaTeX expressions render to cached PNG images via matplotlib's mathtext engine, requiring no TeX installation.

The streaming system tracks token arrival rate in characters per second and reveals text at 90% of that rate to stay just behind the stream. When the internal buffer exceeds 200 characters, it accelerates into catch-up mode. The result is a smooth, adaptive reveal that never stutters or jumps.

Conversation forking lets you right-click any message and branch into a new chat from that point, preserving the full history up to the fork. AI-generated titles name conversations automatically. Dark and light Messenger-inspired themes with animated sidebar transitions. Import and export in Markdown or JSON. No telemetry, no cloud dependency. Everything stays on your machine.

### [DitherPal](https://github.com/exploratorystudios/DitherPal)

> High-performance image and video dithering.

Five algorithms, each JIT-compiled through Numba in `nopython` mode with `fastmath=True` for SIMD-friendly vectorization:

- **Floyd-Steinberg** — classic error-diffusion with padded buffers that eliminate bounds checking, distributing quantization error across four neighboring pixels for smooth gradients.
- **Jarvis-Judice-Ninke** — two-row lookahead with a wider error spread kernel, capturing finer detail at the cost of more computation.
- **Bayer Ordered** — threshold matrix dithering at 2x2, 4x4, and 8x8 scales. Deterministic, parallelizable, and fast.
- **Rosette Halftone** — CMYK-style dot screening with rotation angles at 15, 75, and 45 degrees, generating the circular interference patterns found in commercial print.
- **Text Pattern** — custom text strings rendered as halftone masks, tiled across the image. Type anything. It becomes the dither.

Full-color mode uses KMeans clustering on image samples to extract dominant colors for an intelligent palette, with graceful fallback to grayscale if scikit-learn is unavailable. Custom palettes support 1 to 4 user-defined colors.

Video processing reads frames via OpenCV, batches them for parallel processing through ThreadPoolExecutor scaled to CPU core count, applies the selected dithering to each frame, and encodes to MP4 with H.264. Animated GIFs preserve per-frame durations. Upscaling uses a stepped approach — progressively doubling resolution in stages — with available-memory detection (via `psutil` when present) to prevent the process from exceeding system limits. The chunked rendering system splits oversized images into vertical strips, processes each independently, and stitches the result.

Built with PyQt6. Dark-themed interface with real-time before-and-after preview panels, drag-and-drop file loading, and a progress bar that reflects actual processing state. Output auto-saves to a `dithered/` subdirectory with descriptive filenames.

---

## Games

### [Gemspire](https://github.com/exploratorystudios/gemspire)

> A match-3 roguelike in one HTML file.

No frameworks. No build step. No dependencies. Approximately 7,000 lines of vanilla JavaScript, CSS, and HTML in a single self-contained file.

The board is 7x7 with five gem types — Fire, Ice, Nature, Lightning, and Void — each with base values between 15 and 40. Matches of three or more clear the gems, gravity fills the gaps, and cascades score with an escalating combo multiplier. Matches of four or more trigger explosions that destroy adjacent gems for a 50% score bonus. The board generates without pre-existing matches via a 50-attempt retry on placement.

The roguelike layer is where the depth lives. Players progress through floors with exponentially increasing target scores (`800 * 1.45^(floor-1)`), selecting one of three random relics between each floor. Over 20 relics across Common, Rare, Epic, and Legendary tiers introduce mechanics that reshape how the board plays:

- **Void Hunger** causes Void matches to consume adjacent gems at 2-3x value.
- **Storm Surge** makes Lightning matches strike every Lightning gem on the board, removing them all and boosting their value by +5 each.
- **Cascade Master** permanently increases the base multiplier by +1 every second cascade.
- **Dice of Fate** gives a 15% chance that moves cost nothing.
- **Quantum Mirror** doubles all cascade bonuses.

The crystal economy persists across runs via localStorage. Permanent upgrades range from 15 to 25,000 crystals. Prestige upgrades scale into the millions — Reality Breaker costs 2,000,000 and grants 3x starting multiplier. Hard mode unlocks at 1,000,000 crystals: 1000% higher targets, 40% lower gem values, one fewer move per floor. Run boosters and consumable abilities add tactical depth within individual runs. Cosmetic themes — neon gems, enhanced particles, space board — are separate from gameplay purchases.

Responsive to mobile with touch event handling, passive listeners for performance, and particle count reduction on lower-end devices. Audio through the HTML5 Audio API with 16 distinct sound effects and separate volume controls for music and SFX.

---

## Machine Learning

### [HermesOptimus V3](https://github.com/exploratorystudios/HermesOptimus-v3)

> A neural network word classifier deployed on a TI-84 Plus Silver Edition.

The architecture is 30-50-12. The input layer uses a hybrid encoding: 4 sorted positional letter values (normalized A through Z) capturing letter ordering, plus 26 binary presence flags (one per letter) capturing the character set. This dual representation means the network sees both *which* letters are present and *where* they tend to appear, making it robust to scrambling.

The hidden layer has 50 neurons with sigmoid activation. The output layer has 12 neurons — not one per word, but a binary encoding where each of the 24 dictionary words maps to a unique 12-bit pattern. Classification works by thresholding all 12 outputs at 0.5, producing a binary vector, and finding the dictionary word with the smallest Hamming distance. This is the key design choice: 12 output neurons instead of 24 reduces the weight matrix the calculator has to multiply through, and Hamming distance is trivial to compute in TI-BASIC compared to anything resembling softmax.

Training runs in Python with NumPy — 500,000 epochs at learning rate 0.02 with decay factor 0.3. Data augmentation generates five variants per word per epoch: the original, two with random character drops, and two with multi-letter adjacent substitutions. This teaches the network to recognize words through noise, typos, and missing characters.

The 24-word dictionary: BACK, DARK, EACH, FROM, JUST, BEEN, GOOD, MUCH, SOME, TIME, LIKE, ONLY, WORK, WAVE, ZERO, ZONE, VAIN, VAST, QUIZ, HELP, FIND, PLUS, YAWN, STOP.

Trained weights export through a pipeline: NumPy arrays to CSV via a conversion script, then to TI-native `.8xm` matrix and `.8xl` list files through TokenIDE, transferred to the calculator via TI Connect CE. The TI-BASIC program (`HERMESV3.8xp`, compiled from source through SourceCoder 3) performs the full forward pass on-device: two matrix multiplications (30x50 and 50x12), sigmoid activation, binary thresholding, and Hamming distance search. A progress bar renders during inference so you know the calculator is thinking and not frozen.

The visualization suite generates encoding comparisons, weight heatmaps, binary codebook diagrams, and accuracy breakdowns across original words, scrambled inputs, character substitutions, and adjacent-key typos. The calculator does not know it is doing machine learning. It simply runs the math.

---

<p align="center">
  <em>The constraint does not limit the work. It becomes the work.</em>
</p>
