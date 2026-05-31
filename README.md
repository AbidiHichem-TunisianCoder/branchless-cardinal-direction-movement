# Branchless Cardinal Direction Movement — A Zero-Dependency Formula

**Author:** AbidiHichem-TunisianCoder  
**License:** CC BY 4.0  
**Origin:** Tunisia, 2026

---

## The Formula

```c
Δx = 1 - abs(1 - d)
Δy = abs(2 - d) - 1
```

Where `d ∈ {0, 1, 2, 3}` encodes the four cardinal directions.

---

## Verification

| d | Δx | Δy | Direction |
|---|----|----|-----------|
| 0 | 0  | +1 | Up        |
| 1 | +1 | 0  | Right     |
| 2 | 0  | -1 | Down      |
| 3 | -1 | 0  | Left      |

---

## Why This Matters

The universal standard solution uses lookup arrays:

```c
int dx[] = {0, 1, 0, -1};
int dy[] = {1, 0, -1, 0};
```

This formula eliminates that entirely.

**Properties:**
- Zero memory footprint
- Zero dependencies — no headers, no arrays, no constants
- Branchless when abs is branchless
- Works identically in C, C++, Java, Python, GLSL, HLSL,
  Metal, assembly, any language with abs
- No integer size assumptions, no two's complement dependency
- Works on 8-bit microcontrollers, GPUs, CPUs, any architecture

This is the **minimal possible solution** — you cannot encode
four cardinal directions with less.

---

## How It Works

Both components sample a **phase-shifted triangle wave**:

- `abs(1-d)` produces `1, 0, 1, 2` — a V-shape centered at d=1
- `1 - abs(1-d)` shifts it to `0, +1, 0, -1` ✓

- `abs(2-d)` produces `2, 1, 0, 1` — a V-shape centered at d=2
- `abs(2-d) - 1` shifts it to `+1, 0, -1, 0` ✓

The two components are the **same arithmetic pattern** applied
with different phase centers (1 and 2). This is not coincidence
— it is the underlying symmetry of the cardinal directions.

---

## GLSL Usage

```glsl
for (int d = 0; d < 4; d++) {
    int nx = x + (1 - abs(1 - d));
    int ny = y + (abs(2 - d) - 1);
    // sample neighbor at nx, ny
}
```

No array declarations. No memory access.
Pure ALU computation across all parallel threads.

---

## Origin

This formula was discovered in 2025 while coding a Pacman
pathfinding implementation in Scratch.

The constraint was personal: a refusal to use branching code
and no knowledge of lookup arrays. This forced a search for
a purely arithmetic solution.

The Δx component emerged first. The Δy component required
pushing past the obvious — nearly giving up — before the
constant 2 revealed the symmetry that completes the formula.

Web searches, Bit Twiddling Hacks, HAKMEM, Stack Overflow,
and AI language models all return lookup arrays as the
standard solution. This formula has no known prior publication.

---

## Applications

- Cellular automata on GPU (Conway's Game of Life, etc.)
- Grid pathfinding (A*, BFS, Dijkstra)
- Tile-based games (Pacman, roguelikes, Sokoban)
- Flood fill (image editors, terrain generation)
- Robotics and embedded navigation
- Image processing (convolution kernels)
- Wave Function Collapse procedural generation
- Any grid-based simulation

---

© 2026 AbidiHichem-TunisianCoder. Licensed under CC BY 4.0 — https://creativecommons.org/licenses/by/4.0/
