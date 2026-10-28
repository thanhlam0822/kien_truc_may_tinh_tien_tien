# SingleCycleCPU 25-instruction evidence suite

Goal: demonstrate the 25 mandatory instructions on SingleCycleCPU with reproducible ROM programs and expected final states. The files are a test plan; they are NOT marked PASS until run in Logisim and observed outputs match.

## Included files
- `01_R_type.hex` and paired `.asm.txt`: all 10 R-type instructions.
- `02_I_type.hex` and paired `.asm.txt`: all 9 I-type arithmetic/logical instructions.
- `03_Memory_P13.hex` and paired `.asm.txt`: LW/SW plus dependent arithmetic.
- `04_Branch.hex` and paired `.asm.txt`: BEQ/BNE taken and not taken.
- `05_Jump.hex` and paired `.asm.txt`: JAL/JALR target and link-register checks.
- `SingleCycleCPU_coverage_map.txt`: coverage map.

## Loading a program into the 16-word instruction ROM
1. Save a backup copy of the current `.circ` project.
2. Select the `InstructionMemory` circuit in Logisim-evolution.
3. Right-click the ROM component and choose `Load Image...`.
4. Select one `.hex` file. The first line `v2.0 raw` is the memory-image format; each following value is a 32-bit instruction word at consecutive addresses.
5. Open `SingleCycleCPU`, reset simulation / assert Reset, then release Reset.
6. Execute the program one clock edge at a time. The branch/jump program follows the target PC; the instruction at each skipped address must not alter its destination register.
7. Read final registers and RAM against the paired `.asm.txt` file. If any expected value differs, record FAIL; do not adjust expected results to match the circuit.
8. Take a screenshot showing the CPU/PC and relevant register or RAM values. Record run result in the report table.

## Important
- ROM depth is 16 words (64 bytes) and address advances by 4 bytes per instruction. All images are padded to 16 words with NOP `0x00000013`.
- Stop after the final meaningful instruction and before PC wraps back to address 0.
- RegisterFile/DataMemory reset behavior can affect state between tests; use a fresh simulation or verify/clear the state before each program.
- Load each ROM image explicitly; do not assume the current ROM contents match a previous test.
- When finished, restore the agreed baseline ROM if desired, and save the project's canonical copy separately from any temporary test version.
