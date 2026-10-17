# superhvc-sys

System runtime and stdlib for the Super Home Video Computer (SFC/SNES)

## SNES Hardware Architecture

### Central Processing Unit (CPU)
Powered by the Ricoh 5A22, a custom 16-bit processor derived from the Western Design Center (WDC) 65C816. It runs at variable bus speeds between 1.79 MHz and 3.58 MHz and handles general game logic, controller polling, DMA/HDMA transfers, and overall system coordination.

### Picture Processing Unit (PPU)
Composed of two closely integrated IC blocks (S-PPU) dedicated to managing the console's rich graphical output:
- **Resolutions**: Typically 256x224 or 256x239 pixels, with support for high-resolution and interlaced modes up to 512x476.
- **Backgrounds & Sprites**: Supports up to 4 simultaneous background layers and 128 hardware sprites (with a limit of 32 sprites per scanline).
- **Display Modes**: Includes 8 different background modes, most notably Mode 7, which provides hardware-accelerated affine transformations for rotation, scaling, and pseudo-3D perspective effects.
- **Graphics Memory**: 
  - 64 KB of Video RAM (VRAM) for tile maps and patterns
  - 512 bytes of Colour Generator RAM (CGRAM) for a 32,768-color palette space (256 active on-screen)
  - 544 bytes of Object Attribute Memory (OAM) for sprite metadata

### Audio Processing Unit (APU)
An independent audio subsystem featuring an S-SMP (SPC700 8-bit core) and S-DSP. It operates autonomously using 64 KB of dedicated audio RAM and supports 8 simultaneous sample voices once initialized by the main CPU.

### System Memory
128 KB of main Work RAM (WRAM) used for game states, object tracking, variables, and temporary buffers.

## Instruction Set Architecture (ISA): WDC 65C816

The SNES instruction set is rooted in the legendary 6502 architecture via the 65C816 core, expanding it significantly into a full 16-bit environment.

### Dual Operating Modes
- **Emulation Mode**: Upon system reset, the CPU boots in a 6502-compatible mode to maintain backward compatibility.
- **Native Mode**: Unlocks the processor's full 16-bit register capabilities and extended 24-bit memory mapping.

### Registers & Width Control
- **Accumulator (A) and Index Registers (X, Y)**: Can dynamically toggle between 8-bit and 16-bit widths using processor status flags ($m$ for the accumulator and $x$ for index registers).

### 24-Bit Addressing
Utilizes an 8-bit Program Bank Register (PB) and Data Bank Register (DB) combined with 16-bit base addresses to access up to 16 MB of total memory space.

### Stack Pointer (SP) & Direct Page (DP)
The zero-page and stack pointer are upgraded to 16 bits and can be relocated anywhere within Bank 0.

### Advanced Addressing & Operations
Introduces powerful instructions and addressing modes such as:
- Stack Relative
- Absolute Long (full 24-bit direct access)
- Block move commands (MVN, MVP) for rapid memory manipulation

## Sources & Reference Links
- [Super Nintendo Hardware: CPU, Graphics, Sound & Chips (Retrolize)](https://retrolize.co.uk/blogs/retro-blog/super-nintendo-superfamicom-snes-an-inside-look)
- [WDC 65C816 Architecture Overview (Wikipedia)](https://en.wikipedia.org/wiki/WDC_65C816)
- [65816 Assembly Programming for the SNES (ChibiAkumas)](https://www.chibiakumas.com/6502/snes.php?noui=1)
- [Introduction to 65816 Assembly (X65 Docs)](https://docs.x65.zone/2/08_assembly/)