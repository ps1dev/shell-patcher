
# PS1 BIOS shell patcher

A quick and dirty web-based tool to replace the shell (i.e. the code that
implements the startup screen, CD player and memory card manager) in an original
PlayStation BIOS ROM with an arbitrary PS1 executable, with optional support for
overriding the main RAM configuration and VRAM size prior to launching it.
[Hosted here](https://tools.psx.dev/shell-patcher), but it can also be used
offline by simply opening `index.html` in a browser.

This is not (and was never meant to be) a particularly robust tool. As such,
there are a few important limitations:

- only Sony's own BIOS ROMs are supported. The no$psx BIOS has no separate shell
  component and OpenBIOS does not place its shell at a fixed address in the ROM.
  Note that OpenBIOS can be trivially recompiled with a custom embedded
  executable in addition to or in place of the shell, so it doesn't make much
  sense to patch a built copy;
- the kernel is hardcoded to load the shell into RAM at `0x80030000`, but most
  PS1 binaries expect to run from `0x80010000` instead. In order to work around
  this a *very* crude self-relocation stub is generated and prepended to the
  executable body. This is slow (the relocation is a simple copy, not
  self-decompression) and imposes limits on where code can be loaded; expect to
  get overlapping region errors with executables larger than 128 KB. The proper
  way to solve this would be to patch the target address in kernel code, but
  that would in turn come with its own caveats.

**NOTE**: while this repository was set up and is currently managed by an AI
agent, the actual patcher was written entirely by hand by
[spicyjpeg](https://github.com/spicyjpeg) as a one-off in early 2026.

## License

As with most of spicyjpeg's other work, the patcher is released under the MIT
license. The color theme and graphical assets are from
[ps1-ram-tester](https://github.com/spicyjpeg/ps1-ram-tester).
