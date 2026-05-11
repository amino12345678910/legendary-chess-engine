# LEGENDARY_ENGINE

**LEGENDARY_ENGINE** is a C++20 chess engine and training assistant project built around correctness, testing, iterative improvement, and UCI compatibility.

The engine is developed through multiple versions. Each version introduces specific improvements, is tested against known chess-engine validation positions, and is compared against previous versions before being considered stable.

The goal of the project is not only to create a playable chess engine, but to build a serious engineering workflow around chess programming: move generation, search, evaluation, testing, debugging, regression fixing, and version comparison.

---

## Overview

LEGENDARY_ENGINE is designed to run as a chess engine that can be used from the command line or loaded into a chess GUI that supports the **UCI protocol**.

The project focuses on:

- Correct legal move generation
- UCI engine compatibility
- Search and evaluation improvements
- Engine version testing
- Self-play comparison between versions
- Regression detection
- Long-term development toward a stronger chess engine and training assistant

---

## Current Repository Contents

```text
LEGENDARY_ENGINE/
│
├── engines/
│   ├── LEGENDARY_ENGINE_V1.exe
│   ├── LEGENDARY_ENGINE_V2.exe
│   ├── LEGENDARY_ENGINE_V2b.exe
│   ├── LEGENDARY_ENGINE_V3.exe
│   └── LEGENDARY_ENGINE_V3b.exe
│
├── tests/
│   └── test positions and validation files
│
├── scripts/
│   └── testing / match-running scripts
│
└── README.md
```

> The repository currently provides compiled Windows engine executables. Source code and full build instructions may be added as the project evolves.

---

## How to Run the Engine

### 1. Run from the terminal

Open PowerShell or Command Prompt inside the folder containing the engine executable.

Example:

```powershell
.\LEGENDARY_ENGINE_V3b.exe
```

The engine accepts standard UCI-style commands.

Basic command sequence:

```text
uci
isready
position startpos
go depth 10
quit
```

Example with moves:

```text
uci
isready
position startpos moves e2e4 e7e5 g1f3
go depth 8
```

Example with a custom FEN position:

```text
position fen rnbqkbnr/pppppppp/8/8/4P3/8/PPPP1PPP/RNBQKBNR b KQkq - 0 1
go depth 8
```

---

## How to Use With a Chess GUI

The engine can be loaded into any chess GUI that supports UCI engines.

Recommended GUIs:

- Arena Chess GUI
- Cute Chess
- Banksia GUI
- Lucas Chess
- Fritz / ChessBase-compatible UCI setup

General setup:

1. Open your chess GUI.
2. Go to engine management / add engine.
3. Select one of the `.exe` files from the `engines/` folder.
4. Choose the version you want to test.
5. Start analysis, self-play, or engine-vs-engine matches.

Example:

```text
Engine path:
engines/LEGENDARY_ENGINE_V3b.exe
```

---

## Engine Versions

LEGENDARY_ENGINE is developed using versioned releases. Each version represents a different development stage.

### V1 — Correctness Baseline

The first stable baseline version.

Main focus:

- Basic board representation
- Legal move generation
- Initial search structure
- Basic evaluation
- UCI command handling
- Perft validation

Purpose:

V1 exists as the correctness foundation. Before improving strength, the engine needs reliable move generation and stable behavior.

Status:

```text
Stable baseline
```

---

### V2 — Search Experiment Version

V2 introduced search and tactical improvements.

Main focus:

- Search improvement experiments
- Quiescence search experiments
- Static Exchange Evaluation ideas
- Better move ordering experiments
- Tactical stability testing

Purpose:

V2 was used to test whether more advanced search ideas improved engine strength without breaking stability.

Status:

```text
Experimental
```

Important note:

Not every improvement is automatically kept. Some search ideas can make an engine weaker if they prune too aggressively or create tactical blind spots. V2 is part of that trial-and-error process.

---

### V2b — Stable Search Revision

V2b is a corrected revision of V2.

Main focus:

- Keep useful improvements from V2
- Remove or reduce unstable pruning behavior
- Improve reliability
- Restore stable tactical behavior
- Compare performance against V1 and V2

Purpose:

V2b represents the idea that engine development is not only about adding features. Sometimes the best improvement is identifying a regression and removing the cause.

Status:

```text
Stable revision
```

---

### V3 — UCI and Testing Improvement Version

V3 focused more on engine usability and testing infrastructure.

Main focus:

- Better UCI behavior
- More reliable engine communication
- Improved testing workflow
- Better engine-vs-engine comparison
- Improved time-control handling experiments

Purpose:

A chess engine is not only its search algorithm. It also needs to communicate correctly with GUIs, handle commands properly, and be testable in repeatable ways.

Status:

```text
Testing-focused version
```

---

### V3b — Current Promoted Version

V3b is the current promoted version.

Main focus:

- More stable UCI behavior
- Improved testing reliability
- Better match-result handling
- More consistent engine-vs-engine evaluation
- Cleaner practical behavior compared to earlier experimental versions

Purpose:

V3b is intended to be the current main version for testing, GUI loading, and comparison against future versions.

Status:

```text
Current promoted build
```

---

## Development Methodology

LEGENDARY_ENGINE is built through controlled iteration rather than random feature stacking.

The development process follows this cycle:

```text
1. Implement a small improvement
2. Run correctness tests
3. Check for regressions
4. Compare against previous versions
5. Fix unstable behavior
6. Promote only stable versions
```

This approach is important because chess engines can become worse even after adding advanced techniques. A new feature must prove that it improves the engine without breaking correctness or stability.

---

## Testing Strategy

Testing focuses on correctness, stability, and practical strength.

### 1. Perft Testing

Perft is used to validate move generation.

It counts the number of legal positions reachable from a given position at a specific depth.

Example positions:

- Starting position
- Kiwipete position
- Tactical edge cases
- Castling positions
- Promotion positions
- En passant positions
- Check and checkmate positions

Example concept:

```text
position startpos
perft 4
```

Perft testing helps detect bugs in:

- Legal move generation
- Castling rules
- En passant rules
- Promotion handling
- Check detection
- Move legality
- Board state restoration

---

### 2. UCI Testing

The engine is tested with standard UCI commands.

Example:

```text
uci
isready
position startpos
go depth 10
quit
```

This verifies:

- Engine starts correctly
- UCI handshake works
- GUI communication works
- Search command works
- Engine returns a move
- Engine exits cleanly

---

### 3. Engine-vs-Engine Testing

Different versions are compared through self-play or GUI match testing.

Example comparison:

```text
LEGENDARY_ENGINE_V2b.exe vs LEGENDARY_ENGINE_V3b.exe
```

This helps detect whether a new version is actually stronger or just more complex.

Testing focuses on:

- Win/loss/draw results
- Stability
- Crashes
- Illegal moves
- Time losses
- Tactical mistakes
- Regression from previous versions

---

### 4. Regression Testing

When a new feature causes weaker behavior or instability, it is treated as a regression.

Regression examples:

- Engine crashes
- Illegal move output
- Failing perft result
- Worse tactical performance
- Broken UCI communication
- Time-management failure

A version is not promoted until major regressions are fixed.

---

## Technical Architecture

The engine is planned around standard chess-engine components.

### Board Representation

Responsible for storing the current chess position.

Typical responsibilities:

- Piece placement
- Side to move
- Castling rights
- En passant square
- Halfmove clock
- Fullmove number

---

### Move Generation

Responsible for generating legal chess moves.

Move generation must correctly handle:

- Normal piece moves
- Captures
- Checks
- Pins
- Castling
- En passant
- Promotions
- Legal move filtering

Correct move generation is the foundation of the engine.

---

### Search

Search is responsible for exploring possible future moves and choosing the best one.

Planned and experimental techniques include:

- Minimax / Negamax
- Alpha-beta pruning
- Quiescence search
- Move ordering
- Iterative deepening
- Transposition tables
- Killer move heuristic
- History heuristic
- Null-move pruning
- Late move reductions

---

### Evaluation

Evaluation estimates how good a position is.

Basic evaluation ideas:

- Material balance
- Piece-square tables
- King safety
- Pawn structure
- Mobility
- Passed pawns
- Bishop pair
- Rook activity
- Tactical threats

The evaluation function is expected to evolve across versions.

---

### UCI Protocol

The engine is designed to communicate through UCI.

Core commands:

```text
uci
isready
ucinewgame
position
go
stop
quit
```

UCI compatibility allows the engine to work with external chess GUIs and testing tools.

---

## Roadmap

LEGENDARY_ENGINE is planned as a long-term versioned project.

### V4 — Stronger Search

Planned improvements:

- Improved move ordering
- Killer move heuristic
- History heuristic
- Iterative deepening
- Transposition table support
- Better quiescence search
- Aspiration windows
- More stable time management

Goal:

```text
Improve tactical strength and search efficiency.
```

---

### V5 — Better Evaluation

Planned improvements:

- Improved piece-square tables
- Pawn structure evaluation
- King safety
- Passed pawn evaluation
- Bishop pair bonus
- Mobility scoring
- Rook activity
- Endgame-aware evaluation ideas

Goal:

```text
Make the engine evaluate positions more intelligently.
```

---

### V6 — Training Assistant Features

Planned improvements:

- Analyze mode
- Move recommendation explanations
- Mistake detection
- Position review
- Human-readable feedback
- Training-oriented analysis output

Goal:

```text
Turn the engine from only a chess player into a learning and training assistant.
```

---

### Future Versions

Long-term ideas:

- Opening book support
- Syzygy tablebase support
- Stronger benchmarking system
- Elo tracking between versions
- NNUE-style evaluation experiments
- Better GUI integration
- Cross-platform builds
- Web-based analysis interface

---

## How This Project Is Built

LEGENDARY_ENGINE is built through experimentation, testing, and correction.

The engine development process includes:

- Building an initial working version
- Running tests to find errors
- Fixing bugs through trial and correction
- Comparing versions against each other
- Removing changes that cause regressions
- Keeping improvements that prove stable
- Planning future versions based on test results

This makes the project an engineering exercise, not only a chess project.

The main principle is:

```text
Correctness first. Strength second. Stability always.
```

---

## Tech Stack

- C++20
- UCI protocol
- Windows executable builds
- PowerShell testing scripts
- Chess GUI compatibility
- Engine-vs-engine testing workflow

---

## Current Status

Current promoted version:

```text
LEGENDARY_ENGINE_V3b
```

Main current focus:

- Stable engine execution
- UCI compatibility
- Testing workflow
- Version comparison
- Preparing for stronger search improvements in future versions

---

## Known Limitations

The current version is still under active development.

Known limitations may include:

- Evaluation is still basic compared to mature chess engines
- Search is still evolving
- Some advanced pruning techniques are experimental
- No opening book yet
- No tablebase support yet
- No NNUE evaluation yet
- Source code/build instructions may not yet be fully published

---

## Usage Example

Run the engine:

```powershell
.\LEGENDARY_ENGINE_V3b.exe
```

Send UCI commands:

```text
uci
isready
position startpos
go depth 10
```

Expected behavior:

```text
bestmove <move>
```

Example:

```text
bestmove e2e4
```

Actual move depends on the engine version, search depth, and evaluation.

---

## Recommended Use

Use the latest promoted version for normal testing:

```text
LEGENDARY_ENGINE_V3b.exe
```

Use earlier versions for comparison:

```text
LEGENDARY_ENGINE_V1.exe
LEGENDARY_ENGINE_V2.exe
LEGENDARY_ENGINE_V2b.exe
LEGENDARY_ENGINE_V3.exe
```

Recommended testing workflow:

```text
1. Load two versions into a chess GUI
2. Run engine-vs-engine games
3. Compare stability and results
4. Check for crashes or illegal moves
5. Promote only the stronger stable version
```

---

## Project Direction

The long-term direction of LEGENDARY_ENGINE is to become:

1. A stronger chess engine
2. A technical chess analysis tool
3. A training assistant for players
4. A structured engine-development project with serious testing and version control

The project is still evolving, and each version is designed to move the engine closer to that goal.

---

## License

This project is currently a personal educational and experimental chess-engine project.

A formal license may be added later.
