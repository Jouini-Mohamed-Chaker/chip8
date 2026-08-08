## Project Roadmap

**Step 1 — Learn the machine**
Read a full CHIP-8 technical reference before writing anything: memory layout (4KB), registers V0–VF, index register I, program counter, stack, delay/sound timers, 64×32 display, 16-key hex keypad, and the complete opcode table.

**Step 2 — Core emulator package, no Ebiten yet**
Create a `chip8` package: struct for memory, registers, stack, PC, I, timers, a `[64*32]bool` (or `byte`) display buffer, and keypad state as a `[16]bool`. Write `LoadROM(path string)` to load a file into memory at 0x200. This package should be able to compile and be tested with zero knowledge that Ebiten will ever exist.

**Step 3 — Fetch-decode-execute skeleton**
One `Cycle()` method: fetch 2 bytes, increment PC, decode, switch on opcode groups, execute (stub bodies for now). This is the loop Ebiten will call every frame later.

**Step 4 — Implement opcodes in logical groups**
- Flow control (jumps, calls, returns, skip-if) — test these first with tiny hand-written test ROMs
- Register/arithmetic ops (loads, add, sub, bitwise, the 8XY_ group)
- Memory/index ops (I register loads, register range store/load)
- Display op DXYN — sprite XOR-drawing into your display buffer with collision detection into VF (this is the one Ebiten will actually visualize, so it's satisfying once step 6 is wired up)
- Input-check ops (skip-if-key-pressed/not-pressed) — stub against your `[16]bool` keypad array for now, Ebiten fills it in step 7

**Step 5 — 60Hz timers**
Delay/sound timers decrement at a fixed 60Hz independent of CPU cycle speed. Use a `time.Ticker` goroutine, or — cleaner once Ebiten's in the picture — just decrement them once per `Update()` call, since Ebiten's `Update()` already runs at a fixed logical rate (default 60 TPS), which conveniently matches CHIP-8's timer spec almost exactly.

**Step 6 — Wire up Ebiten's game loop**
Create your `Game` struct implementing Ebiten's three required methods:
- `Update()` — run N `Cycle()` calls per tick (N = your chosen instructions-per-frame), decrement timers
- `Draw(screen)` — read the chip8 display buffer, draw each "pixel" as a filled rectangle (or a scaled-up image) onto the Ebiten screen
- `Layout()` — return your logical screen size, let Ebiten handle window scaling
Test with a ROM that draws something static first (e.g. the IBM logo test ROM — a very common CHIP-8 first-render test).

**Step 7 — Keyboard input via Ebiten**
In `Update()`, use `ebiten.IsKeyPressed()` for the standard CHIP-8 key mapping (`1234/qwer/asdf/zxcv` on your real keyboard, standard 4x4 hex layout) and update your emulator's `[16]bool` keypad array each frame. Wire this into the input-check opcodes from step 4.

**Step 8 — Validate correctness**
Run a known CHIP-8 test ROM (there are ones purpose-built to validate every opcode and print PASS/FAIL results) and fix bugs until it fully passes. Don't skip this — "looks about right" hides real bugs that surface later as game-breaking glitches.

**Step 9 — Play real games**
Load public-domain ROMs (Pong, Tetris, Space Invaders, Breakout) and play them. Tune your instructions-per-frame constant until games feel right — original CHIP-8 hardware speed wasn't standardized, so this is a "feel" tuning step, not a bug.

**Step 10 — Sound**
Use Ebiten's audio package to play a simple tone while the sound timer is nonzero (a generated square wave is period-accurate to real CHIP-8 beeps and easy to synthesize yourself as a byte buffer — no audio file needed).

**Step 11 (optional polish) — Quality of life**
Pause/resume, load-ROM-from-file-picker or CLI arg, adjustable emulation speed, save states (dump/restore your CPU struct to disk), a simple debug overlay showing register values.