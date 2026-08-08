# chip8-emu

A CHIP-8 emulator written in Go.

## Why this exists

This is a **learning project** not a polished product. I'm using it to go beyond
CS fundamentals and get real hands-on depth in Go, memory management, bit
manipulation, fetch-decode-execute loops, concurrency, and clean separation
between core logic and I/O.

The emulator core (CPU, memory, opcodes, timers) is written using only the Go
standard library. [Ebiten](https://ebiten.org/) is used purely for the
rendering/input/audio layer, kept deliberately separate from the emulator core.

Expect rough edges, incomplete features, and code that changes as I learn. If
you're also learning Go or emulator development, feel free to poke around,
but treat this as a work-in-progress study project, not a reference
implementation.

## What is CHIP-8?

CHIP-8 is a simple, interpreted programming language / virtual machine from
the 1970s, originally designed to make it easier to write games for 8-bit
microcomputers. It's widely used as a first emulator project because the
instruction set is small (~35 opcodes) but it's still a fully real, playable
system. Games like Pong, Tetris, and Space Invaders exist as CHIP-8 ROMs.

## Status

- [ ] Core CPU/memory struct
- [ ] Fetch-decode-execute loop
- [ ] Opcode implementations
- [ ] 60Hz timers
- [ ] Ebiten display rendering
- [ ] Keyboard input
- [ ] Passes standard CHIP-8 test ROM
- [ ] Plays real games (Pong, Tetris, etc.)
- [ ] Sound

## Running

```bash
go run main.go
```

(ROM loading path/flag to be added as the project progresses.)

## ROMs

This repo does not include copyrighted game ROMs. Public-domain CHIP-8 ROMs
and test ROMs are available from various community archives online.

## Dependencies

- Go standard library (core emulator logic)
- [`github.com/hajimehoshi/ebiten/v2`](https://github.com/hajimehoshi/ebiten) — window, rendering, input, audio

## References

- Cowgod's CHIP-8 Technical Reference (the standard opcode/architecture guide)