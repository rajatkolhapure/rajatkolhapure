# Rajat Kolhapure

Developer based in Pune, India.

I build software to understand how things work underneath all the ui. Most projects started because I ran into the tool that felt limited or paywalled and thought: *"why is this the way it is?"*

I prefer working implementations over complexity in the design, and complete systems over isolated demos. Still learning, experimenting, and regularly breaking things to see why they worked in the first place.

---

### Systems & Projects

#### [CBT Core](https://github.com/rajatkolhapure/cbt-core) + [NTA LaTeX Extractor](https://github.com/rajatkolhapure/nta-latex-extractor)
*An end-to-end test platform paired with an automated question paper digitization.*

Standard computer-based testing software often feels old and rigid to populate with complex questions. I built this two-part ecosystem to tackle both sides of the problem: running secure, responsive exams, and transforming messy examination papers into clean, machine readable data.

- **CBT Core**: Built with TypeScript and Node.js with a distinctive interface. Handles high concurrency exam delivery, stays persistence across network drops, and automated cheat detection.
- **NTA LaTeX Extractor**: A Python and computer vision pipeline designed to take in thousands of scanned JEE, NEET, and MHT-CET exam papers. Rather than relying on naive OCR that mangles formulas and diagrams, it pairs multimodal vision models with OpenCV ink-projection strat to cleanly extract mathematical expressions into LaTeX, format tables into semantic markup, and isolate diagrams with sub-pixel Lanczos4 anti-aliased cropping.

<details>
<summary>Technical breakdown & pipeline details</summary>

```
Raw Scanned PDF / Image
       │
       ├── Computer Vision Preprocessing
       │     ├── Ink projection profiling & column segmentation
       │     └── Deterministic diagram boundary isolation
       │
       ├── Dual Vision Inference Engine
       │     ├── Cloud: Multimodal LLM with automated retry & fallback
       │     └── Offline: Local GPU inference (Qwen2.5-VL)
       │
       ├── Normalization & Asset Export
       │     ├── Sub-pixel diagram upscaling 
       │     ├── LaTeX mathematical expression validation
       │     └── Dynamic subject classification (Physics / Chemistry / Math)
       │
       └── Verification & Ingestion
             ├── Rapid side-by-side keyboard-driven human review tool
             └── Direct schema push into CBT Core PostgreSQL database
```

**Status:** The extraction pipeline has processed thousands of real-world national entrance exam questions into structured JSON. CBT Core has an end-to-end working exam engine with live proctoring event dispatchers and a local verification suite, although it is not 100% accurate it still messes up some questions which are too complicated for it.
</details>

---

#### [Tele-Lens](https://github.com/rajatkolhapure/Tele-Lens)
*Watermark-free phone to desktop webcam over WebRTC.*

Built-in laptop cameras are notoriously shit, while commercial phone as webcam apps tend to slap watermarks over video, throttle resolution behind monthly subscriptions, or run proprietary blackbox drivers. I wanted an open alternative that treats your phone camera like a proper hardware sensor without artificial restrictions.

- **Mobile Client (Flutter / Dart)**: Interacts directly with Camera2 and AVFoundation to expose manual lens switching (standard, ultrawide, telephoto), exposure locks, ISO adjustments, manual focus, and a thermal-saver background mode.
- **Desktop Receiver (Python / PyQt6)**: A low latency receiving hub that accepts incoming feeds over local Wi-Fi (mDNS / Bonjour auto-discovery) or wired USB (automated ADB port forwarding).
- **Virtual Hardware Integration**: Feeds raw incoming video frames into DirectShow and OBS Virtual Camera backends via `pyvirtualcam`, making the phone appear as a native hardware webcam in Zoom, Google Meet, Teams, and Discord.

<details>
<summary>Technical breakdown & driver bridge</summary>

```
[ Smartphone: Flutter ]
  │  ├── Camera2 / AVFoundation Manual Controls
  │  └── WebRTC Video & Audio Encoder
  │
  │─── Wi-Fi (mDNS Zero-Config) or USB ADB Tunnel ───┐
                                                      ▼
                                            [ Desktop: Python / PyQt6 ]
                                              ├── Incoming Peer Gatekeeper
                                              ├── JSON-RPC Camera Control Channel
                                              └── pyvirtualcam Bridge
                                                      │
                                                      ▼
                                            [ Windows Virtual Camera ]
                                            (DirectShow / OBS Source)
```

**Status:** Complete working prototype for Android and Windows with local Wi-Fi and USB ADB transport, manual sensor control sliders, and OBS virtual camera output.
</details>

---

#### [Evolutionary Chess Engine](https://github.com/rajatkolhapure/chess)
*A step-by-step experiment in understanding game-tree search and machine evaluation from first principles.*

Instead of importing an existing engine or jumping straight into neural networks, I wanted to explore how machine chess thought evolved historically—starting from a pure greedy piece-counter and systematically building up search depth and tactical awareness.

- **Level 1 (Greedy)**: Material counting with basic PeSTO piece-square tables and game-phase tapering.
- **Level 2 (Minimax)**: Zero-sum game tree search with 3-ply lookahead.
- **Level 3 (Tactician)**: Alpha-Beta pruning, MVV-LVA (Most Valuable Victim - Least Valuable Attacker) move ordering, and Quiescence search to eliminate the horizon effect during mid-trade captures.
- **Level 4 (Deep Searcher)**: Zobrist hashing for 64-bit board transposition tables, Null-Move Pruning (NMP) for early branch cutoffs, and iterative deepening with fail-safe rollback state protection.

<details>
<summary>Evaluation details & search benchmarks</summary>

The engine implements a classical Stockfish-inspired positional evaluator:
- Tapered phase scoring between opening and endgame.
- Positional bonuses for bishop pairs and rook infiltration on open files and 7th ranks.
- Pawn structure evaluation tracking passed pawns, isolated pawns, and doubled pawn penalties.
- King safety metrics penalizing collapsed castling pawn shields.

**Status:** Levels 1 through 4 completed (~1900 Elo target). Search and evaluation pipelines benchmarked and verified with a local web interface for live analysis.
</details>

---

### What I'm exploring lately

- Multimodal document processing and heuristic computer vision pipelines.
- Virtual device drivers, audio loopbacks, and low-latency WebRTC data channels.
- Combinatorial search optimizations and adversarial game tree pruning.

---

### How I build

1. **Understand before abstracting**: I prefer tracing the raw protocol packets, byte arrays, or API calls before reaching for a high-level wrapper.
2. **Built to solve an actual annoyance**: If a tool already does what I need cleanly and without arbitrary limitations, I don't rebuild it. If it doesn't, I treat the gap as an excuse to learn how that class of software works.
3. **Tendency to overengineer**: A simple weekend script has a habit of growing into a full pipeline with diagnostic dashboards, stateful resumption, and custom test harnesses.

---

### Links & Contact

- **Website:** [rajatkolhapure.me](https://rajatkolhapure.me)
- **Live Demo:** [core.rajatkolhapure.me](https://core.rajatkolhapure.me)
- **Email:** [rajatkolhapure@gmail.com](mailto:rajatkolhapure@gmail.com)
- **GitHub:** [@rajatkolhapure](https://github.com/rajatkolhapure)
