<h1 align="center">Arcade Space Shooter: mkeykernel Edition</h1>

<p align="center">
  <a href="https://en.wikipedia.org/wiki/C_(programming_language)">
    <img src="https://img.shields.io/badge/C-A8B9CC?style=for-the-badge&logo=c&logoColor=white" alt="C">
  </a>
  <a href="https://en.wikipedia.org/wiki/X86_instruction_listings">
    <img src="https://img.shields.io/badge/x86_Assembly-6E4C13?style=for-the-badge" alt="x86 ASM">
  </a>
  <a href="https://github.com/arjun024/mkeykernel">
    <img src="https://img.shields.io/badge/mkeykernel-OS-orange?style=for-the-badge" alt="mkeykernel">
  </a>
  <a href="https://en.wikipedia.org/wiki/VGA-compatible_text_mode">
    <img src="https://img.shields.io/badge/VGA-Text_Mode-blue?style=for-the-badge" alt="VGA Text">
  </a>
  <a href="LICENSE">
    <img src="https://img.shields.io/badge/license-MIT-darkred?style=for-the-badge" alt="License">
  </a>
</p>

<p align="center">
  A high-performance x86 <b>Arcade Space Shooter</b> built upon the <b>mkeykernel</b> framework developed by <b>Arjun Sreedharan</b>. This project leverages the bare-metal capabilities of mkeykernel—including custom interrupt handling and direct video memory manipulation—to implement a fully responsive space combat simulation directly in the system's VGA text buffer.
</p>

<p align="center">
  <a href="#mkeykernel-architecture">
    <img src="https://img.shields.io/badge/Architecture-222222?style=flat" />
  </a>
  <span> ° </span>
  <a href="#project-structure">
    <img src="https://img.shields.io/badge/Structure-222222?style=flat" />
  </a>
  <span> ° </span>
  <a href="#arcade-mechanics">
    <img src="https://img.shields.io/badge/Mechanics-222222?style=flat" />
  </a>
  <span> ° </span>
  <a href="#technical-specifications">
    <img src="https://img.shields.io/badge/Specs-222222?style=flat" />
  </a>
  <span> ° </span>
  <a href="#deployment--installation">
    <img src="https://img.shields.io/badge/Deploy-222222?style=flat" />
  </a>
</p>

---
<br>

<h2 align="center">mkeykernel Architecture</h2>

This project is deeply integrated with the architecture defined in the **mkeykernel** project, a minimal x86 kernel designed for educational exposure to hardware-level programming:

1.  **Framework Foundation:** Utilizes the base kernel implementation by Arjun Sreedharan, featuring a Multiboot-compliant header and a 32-bit Protected Mode environment initialized via `kernel.asm`.
2.  **Interrupt Descriptor Table (IDT):** Implements a custom IDT to register the `keyboard_handler`. This allows the game to capture and process physical scancodes from the keyboard controller (Port `0x60`) with zero abstraction.
3.  **VGA Text Buffer Pipeline:** Directly writes memory patterns to `0xb8000`. By treating the text buffer as a 2D coordinate space (80x25), the engine renders high-speed ASCII sprites and UI elements with minimal CPU overhead.
4.  **Hardware Port I/O:** Employs Assembly wrappers for `in` and `out` instructions to communicate with the **Programmable Interrupt Controller (PIC)**, ensuring proper IRQ mapping and EOI (End of Interrupt) signaling.

---
<br>

<h2 align="center">Project Structure</h2>

```
ArcadeSpaceShooter_mkeykernel/
├── LICENSE                                   # MIT License
├── README.md                                 # Project documentation
│
├── mkeykernel/                               # Kernel Source & Engine
│   ├── kernel.c                              # Integrated Game Logic (Credit: mkeykernel)
│   ├── kernel.asm                            # Assembly Entry (Arjun Sreedharan)
│   ├── keyboard_map.h                        # Scancode Definitions
│   ├── link.ld                               # Linker Script for x86 Positioning
│   ├── binary_x86/                           # Compiled kernel binaries
│   └── ...                                   # Object artifacts
│
└── Screenshots/                              # Engine Previews in Simulation
    ├── mkeykernel_v1.PNG
    ├── mkeykernel_v2.PNG
    └── mkeykernel_v3.PNG
```

---
<br>

<h2 align="center">Arcade Mechanics</h2>

- **ASCII Sprite Modeling**: Detailed multi-character shuttle designs (`/ ]\`, `< ( ] >`) rendered via direct video memory offsets.
- **Asynchronous Input Loop**: High-frequency polling of the keyboard status port to manage movement and firing simultaneously with the main loop.
- **Deterministic Spawning**: Implements a custom Pseudo-Random Number Generator (PRNG) using an LCG algorithm for consistent enemy waves.
- **AABB Collision Logic**: Precise Hit-Box detection calculated across the 80x25 character grid.
- **Color Optimization**: Utilizes VGA attribute bytes to render sprites in high-contrast white (0x0F) against a void background.

### System Commands
<table align="center">
  <tr>
    <th align="center">Action</th>
    <th align="center">Physical Key</th>
  </tr>
  <tr>
    <td align="center">Vertical Ascend</td>
    <td align="center"><b>Arrow UP</b></td>
  </tr>
  <tr>
    <td align="center">Vertical Descend</td>
    <td align="center"><b>Arrow DOWN</b></td>
  </tr>
  <tr>
    <td align="center">Weapon Deployment</td>
    <td align="center"><b>Space</b></td>
  </tr>
  <tr>
    <td align="center">System Shutdown</td>
    <td align="center"><b>Q</b></td>
  </tr>
</table>

---
<br>

<h2 align="center">Technical Specifications</h2>

<table align="center">
  <tr>
    <th align="center">Component</th>
    <th align="center">Specification</th>
  </tr>
  <tr>
    <td align="center"><b>Base Kernel</b></td>
    <td align="center">mkeykernel (Arjun Sreedharan)</td>
  </tr>
  <tr>
    <td align="center"><b>CPU Mode</b></td>
    <td align="center">32-bit Protected Mode (x86)</td>
  </tr>
  <tr>
    <td align="center"><b>Video Base</b></td>
    <td align="center">0xB8000 (Color Text Buffer)</td>
  </tr>
  <tr>
    <td align="center"><b>Interrupts</b></td>
    <td align="center">IDT / IRQ 1 (Keyboard)</td>
  </tr>
  <tr>
    <td align="center"><b>Linker Format</b></td>
    <td align="center">ELF i386 (Linker Mapping)</td>
  </tr>
</table>

---
<br>

<h2 align="center">Deployment & Installation</h2>

### 1. Build Environment
You require `nasm` for assembly compilation and a C cross-compiler (e.g., `gcc` targeting `elf_i386`) for the kernel logic.

### 2. Acquisition
```bash
git clone https://github.com/Zer0-Bug/ArcadeSpaceShooter_mkeykernel.git
```
```bash
cd ArcadeSpaceShooter_mkeykernel/mkeykernel
```

### 3. Compilation
Link the assembly and C components using the provided script:
```bash
nasm -f elf32 kernel.asm -o kasm.o
gcc -m32 -c kernel.c -o kc.o
ld -m elf_i386 -T link.ld -o kernel kasm.o kc.o
```

### 4. Execution (QEMU)
Launch the standalone kernel directly in your emulator:
```bash
qemu-system-i386 -kernel kernel
```

---
<br>

<h2 align="center">Contribution</h2>

Contributions are always appreciated. Open-source projects grow through collaboration, and any improvement—whether a bug fix, new feature, documentation update, or suggestion—is valuable.

To contribute, please follow the steps below:

1. Fork the repository.
2. Create a new branch for your change:  
   `git checkout -b feature/your-feature-name`
3. Commit your changes with a clear and descriptive message:  
   `git commit -m "Add: brief description of the change"`
4. Push your branch to your fork:  
   `git push origin feature/your-feature-name`
5. Open a Pull Request describing the changes made.
<br>
All contributions are reviewed before being merged. Please ensure that your changes follow the existing code style and include relevant documentation or tests where applicable.

---
<br>
<p align="center">
  <a href="mailto:777eerol.exe@gmail.com">
    <img src="https://cdn.simpleicons.org/gmail/D14836" width="40" alt="Email">
  </a>
  <span> × </span>
  <a href="https://www.linkedin.com/in/eerolexe/">
    <img src="https://upload.wikimedia.org/wikipedia/commons/c/ca/LinkedIn_logo_initials.png"
         width="40"
         alt="LinkedIn">
  </a>
</p>

---

<p align="center" style="margin-top:10px; letter-spacing:4px;">
  ∞
</p>
