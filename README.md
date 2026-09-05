# shell-patcher

Replaces the shell binary in a PlayStation BIOS ROM with an arbitrary PS-EXE,
in the browser. One self-contained `.html` you can copy anywhere and open
offline. No server, no install, no network.

Written by spicyjpeg.

## What it does

Given a 512 KB BIOS image and a PS-EXE, it overwrites the shell region at
`0x18000` with a small self-relocating stub followed by the executable body.
The stub copies the body to the load address in the executable's header, flushes
the instruction cache through `A0(44h)`, sets `$gp` and `$sp`, and jumps to the
entry point. The shell region is 0x4c000 bytes, so that is the ceiling on stub
plus payload.

Two optional overrides are written before the copy runs, so they take effect
before any of the executable does:

- **Main RAM configuration**, stored to `0x1F801060`. See the
  [memory control](https://psx-spx.consoledev.net/memorycontrol) page for the
  values.
- **VRAM size**, sent as `GP1(09h)`. Bit 0 gates the second 1 MB bank: with it
  clear every Y coordinate wraps inside the lower megabyte, with it set the full
  10-bit Y range is addressable. Only meaningful on a board that has the upper
  bank populated.

## Deploying

There is no build step. `index.html` is the artifact; the Pages workflow uploads
it as it stands.
