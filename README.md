<h1 align="center">Space Shooter: Bare-Metal x86 Engine</h1>

<p align="center">
  <a href="https://en.wikipedia.org/wiki/C_(programming_language)">
    <img src="https://img.shields.io/badge/C-A8B9CC?style=for-the-badge&logo=c&logoColor=white" alt="C">
  </a>
  <a href="https://en.wikipedia.org/wiki/X86_instruction_listings">
    <img src="https://img.shields.io/badge/x86_Assembly-6E4C13?style=for-the-badge" alt="x86 ASM">
  </a>
  <a href="https://isocpp.org/">
    <img src="https://img.shields.io/badge/Protected_Mode-32--bit-blue?style=for-the-badge" alt="32-bit PM">
  </a>
  <a href="https://en.wikipedia.org/wiki/VGA-compatible_text_mode">
    <img src="https://img.shields.io/badge/VGA-Text_Mode-red?style=for-the-badge" alt="VGA Text">
  </a>
  <a href="LICENSE">
    <img src="https://img.shields.io/badge/license-MIT-darkred?style=for-the-badge" alt="License">
  </a>
</p>

<p align="center">
  A highly optimized <b>Bare-Metal Space Shooter</b> built from scratch as a standalone <b>x86 Operating System Kernel</b>. This project bypasses all traditional abstraction layers, utilizing direct <b>Interrupt Descriptor Table (IDT)</b> management and <b>VGA Text Buffer</b> manipulation to deliver an atmospheric arcade experience directly on top of the CPU.
</p>

<p align="center">
  <a href="#low-level-architecture">
    <img src="https://img.shields.io/badge/Architecture-222222?style=flat" />
  </a>
  <span> ° </span>
  <a href="#project-structure">
    <img src="https://img.shields.io/badge/Structure-222222?style=flat" />
  </a>
  <span> ° </span>
  <a href="#kernel-mechanics">
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

<h2 align="center">Low-Level Architecture</h2>

This project is a masterclass in custom kernel development, featuring a specialized architecture designed for real-time responsiveness:

1.  **Interrupt-Driven Input:** Implements a custom **IDT (Interrupt Descriptor Table)** to register a high-priority hardware interrupt handler for IRQ1 (Keyboard), ensuring zero input lag by reading scancodes directly from `0x60` data ports.
2.  **VGA Buffer Pipeline:** Renders game state by manipulating the hardware video memory at `0xb8000`. It utilizes a 25-line by 80-column character matrix with specialized byte offsets for lightning-fast ASCII sprite updates.
3.  **Assembly Bootstrapping:** A custom `kernel.asm` loader sets up the 32-bit Protected Mode environment, establishing the global stack and enabling the CPU to transition into the high-level C `kmain` entry point.
4.  **Hardware Port I/O:** Uses custom `read_port` and `write_port` assembly wrappers to communicate with the **Programmable Interrupt Controller (PIC)** and keyboard hardware status registers.

---
<br>

<h2 align="center">Project Structure</h2>

```
ArcadeSpaceShooter_mkeykernel/
├── LICENSE                                   # MIT License
├── README.md                                 # Project documentation
│
├── mkeykernel/                               # Bare-Metal Kernel Source
│   ├── kernel.c                              # Main Engine & Interrupt Logic
│   ├── kernel.asm                            # Assembly Entry & Environment Setup
│   ├── keyboard_map.h                        # Scancode to ASCII definitions
│   ├── link.ld                               # Linker Script (x86 positioning)
│   ├── binary_x86/
│   │   └── kernel                            # Compiled x86 Kernel Binary
│   └── kc.o / kasm.o                         # Compiled Object Files
│
└── Screenshots/                              # Engine Previews in Simulation
    ├── mkeykernel_v1.PNG
    ├── mkeykernel_v2.PNG
    └── mkeykernel_v3.PNG
```

---
<br>

<h2 align="center">Kernel Mechanics</h2>

- **ASCII Sprite Modeling**: Detailed multi-character shuttle designs (`/ ]\`, `< ( ] >`) rendered via direct video memory offsets.
- **Asynchronous Input Loop**: High-frequency polling of the keyboard status port to manage movement and firing simultaneously with the main loop.
- **Deterministic Spawning**: Implements a custom **Pseudo-Random Number Generator (PRNG)** using an LCG algorithm for consistent enemy waves.
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
    <td align="center"><b>Execution Layer</b></td>
    <td align="center">32-bit Protected Mode (Bare-Metal)</td>
  </tr>
  <tr>
    <td align="center"><b>Video Memory</b></td>
    <td align="center">0xB8000 (VGA Text Buffer)</td>
  </tr>
  <tr>
    <td align="center"><b>Input Protocol</b></td>
    <td align="center">Direct Port 0x60 / 0x64 I/O</td>
  </tr>
  <tr>
    <td align="center"><b>Interrupts</b></td>
    <td align="center">Custom IDT / IRQ1 Mapping</td>
  </tr>
  <tr>
    <td align="center"><b>Resolution</b></td>
    <td align="center">80 Columns x 25 Lines</td>
  </tr>
</table>

---
<br>

<h2 align="center">Deployment & Installation</h2>

### 1. Build Environment
You require `nasm` for assembly compilation and a C cross-compiler (e.g., `gcc` targeting `elf_i386`) for the kernel logic.

### 2. Physical/Virtual Deployment
1. Navigate to the `mkeykernel/` directory.
2. Compile and link using the provided linker script (`link.ld`):
   ```bash
   nasm -f elf32 kernel.asm -o kasm.o
   gcc -m32 -c kernel.c -o kc.o
   ld -m elf_i386 -T link.ld -o kernel kasm.o kc.o
   ```

### 3. Execution (QEMU)
Launch the standalone kernel in an emulator to observe the bare-metal logic:
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
