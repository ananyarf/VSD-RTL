# VSD-RTL

# Daily Task Log — 26 October 2026

## Task 1: RTL Design Environment Setup Using GitHub Codespaces

### Objective
To set up an RTL design and simulation environment using GitHub Codespaces and install the essential tools required for Verilog-based digital design.

### Tools
- GitHub Codespaces
- VS Code Web
- Linux terminal
- Yosys — RTL synthesis
- Icarus Verilog — Verilog simulation
- GTKWave — waveform visualization

### Work Performed
1. Created a GitHub Codespace for the VSD RTL repository.
2. Accessed the repository workspace through VS Code in the browser.
3. Updated the package lists using `sudo apt update`.
4. Installed the RTL tools using the following command:

   ```bash
   sudo apt install -y yosys iverilog gtkwave
   ```

5. Attempted to verify the tool installations using their version commands.

### Outcome
Worked on preparing a cloud-based RTL design environment with the essential synthesis, simulation, and waveform-viewing tools.

---

## Task 2: Ubuntu VLSI Machine Setup Using a Preconfigured VDI

### Objective
To configure and access a preconfigured Ubuntu virtual machine containing the VLSI development environment, following the provided setup instructions.

### Reference Material
- VDI download: [VSD Squadron FPGA Mini](https://forgefunder.com/~kunal/vsdsquadron_fpga_mini.zip)
- VirtualBox downloads: [Oracle VirtualBox](https://www.virtualbox.org/wiki/Download_Old_Builds_6_1)

### Setup Procedure
1. Reviewed the system storage requirements and ensured that sufficient disk space was available.
2. Downloaded the provided VDI archive and extracted its contents.
3. Opened Oracle VirtualBox and configured a virtual machine for the supplied Ubuntu disk image.
4. Selected the existing VDI file as the virtual machine's hard disk.
5. Configured the virtual machine and started it.

### Outcome
Setting up a dedicated Ubuntu-based VLSI environment using a preconfigured virtual machine image.

### Purpose
A preconfigured VLSI machine provides a consistent software environment for semiconductor design and verification tasks without requiring every tool to be installed and configured individually.

---

## Summary

Today's work focused on preparing two development environments:

1. **Cloud-based RTL environment:** GitHub Codespaces with Yosys, Icarus Verilog, and GTKWave.
2. **Local VLSI environment:** Ubuntu virtual machine using a preconfigured VDI image.

These setups provide a foundation for future RTL design, simulation, synthesis, and VLSI-related learning activities.
