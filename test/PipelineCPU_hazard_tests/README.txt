PipelineCPU hazard test programs — Logisim-evolution

All ROM images are 16 words (32-bit) and use Logisim memory format `v2.0 raw`.
These are test stimuli, not pre-verified PASS results. Run each program on the PipelineCPU circuit and record actual results.

IMPORTANT: Use the PipelineCPU circuit, not SingleCycleCPU. Before each independent test:
1. Load the chosen `.hex` into the InstructionMemory ROM.
2. Stop the clock and reset the simulation with Ctrl+R; confirm the ROM still contains the intended first instruction.
3. Assert/release the CPU Reset if that is part of the project's startup procedure.
4. Run enough rising clock edges for the final instruction to reach WB. Do not assume the same edge count as SingleCycleCPU; inspect the pipeline stages/register writeback.
5. Record final RegisterFile and RAM values. A test passes only if all expected values match and the relevant hazard behavior is observed.

P1: direct RAW data dependencies. Expected x1=10, x2=15, x3=25, x4=26.
P2: store/load then immediate use. Expected RAM[0]=42, x2=42, x3=43, x4=53.
P3: taken BEQ. The instruction at address 0x0C writes x3=99 but must be squashed/skipped. Expected x3 remains 0 and x4=7.

For P1, if outputs are wrong, capture the pipeline stage values around the dependent instructions and do not mark PASS. For P2, capture evidence of LW, the immediate dependent ADDI, and final register/RAM values. For P3, capture PC/branch target or flush signal if available, plus final registers.
