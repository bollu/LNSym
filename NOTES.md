# Notes on Integrating Superoptimization into `LNSym`


```
.
├── Arm
│   ├── Attr.lean
│   ├── BitVec.lean
│   ├── Cfg -- control flow graph.
│   │   └── Cfg.lean
│   ├── Cosim.lean -- cosimulation.
│   ├── Decode -- decode bits into instruction.
│   │   ├── BR.lean -- branch
│   │   ├── DPI.lean -- data processing immediate
│   │   ├── DPR.lean -- data processing register
│   │   ├── DPSFP.lean -- data processing SIMD & FP
│   │   └── LDST.lean -- load/store.
│   ├── Decode.lean
│   ├── Exec.lean -- main file with large jump table to implementations in Insts/.
│   ├── FromMathlib.lean
│   ├── Insts
│   │   ├── BR
│   │   │   ├── Compare_branch.lean
│   │   │   ├── Cond_branch_imm.lean
│   │   │   ├── Hints.lean
│   │   │   ├── Insts.lean
│   │   │   ├── Uncond_branch_imm.lean
│   │   │   └── Uncond_branch_reg.lean
│   │   ├── Common.lean
│   │   ├── Cosim
│   │   │   ├── armsimulate
│   │   │   ├── disasm.sh
│   │   │   ├── platform_check.sh
│   │   │   ├── simulator.c
│   │   │   └── template.S
│   │   ├── DPI
│   │   │   ├── Add_sub_imm.lean
│   │   │   ├── Bitfield.lean
│   │   │   ├── Insts.lean
│   │   │   ├── Logical_imm.lean
│   │   │   ├── Move_wide_imm.lean
│   │   │   └── PC_rel_addressing.lean
│   │   ├── DPR
│   │   │   ├── Add_sub_carry.lean
│   │   │   ├── Add_sub_shifted_reg.lean
│   │   │   ├── Conditional_select.lean
│   │   │   ├── Data_processing_one_source.lean
│   │   │   ├── Data_processing_two_source.lean
│   │   │   ├── Insts.lean
│   │   │   └── Logical_shifted_reg.lean
│   │   ├── DPSFP
│   │   │   ├── Advanced_simd_copy.lean
│   │   │   ├── Advanced_simd_extract.lean
│   │   │   ├── Advanced_simd_modified_immediate.lean
│   │   │   ├── Advanced_simd_permute.lean
│   │   │   ├── Advanced_simd_scalar_copy.lean
│   │   │   ├── Advanced_simd_scalar_shift_by_immediate.lean
│   │   │   ├── Advanced_simd_shift_by_immediate.lean
│   │   │   ├── Advanced_simd_table_lookup.lean
│   │   │   ├── Advanced_simd_three_different.lean
│   │   │   ├── Advanced_simd_three_same.lean
│   │   │   ├── Advanced_simd_two_reg_misc.lean
│   │   │   ├── Conversion_between_FP_and_Int.lean
│   │   │   ├── Crypto_aes.lean
│   │   │   ├── Crypto_four_reg.lean
│   │   │   ├── Crypto_three_reg_sha512.lean
│   │   │   ├── Crypto_two_reg_sha512.lean
│   │   │   └── Insts.lean
│   │   └── LDST
│   │       ├── Advanced_simd_multiple_struct.lean
│   │       ├── Insts.lean
│   │       ├── Reg_imm.lean
│   │       ├── Reg_pair.lean
│   │       └── Reg_unscaled_imm.lean
│   ├── Insts.lean 
│   ├── Map.lean -- association lists.
│   ├── Memory.lean -- memory model.
│   ├── MemoryProofs.lean -- memory (non) interference proofs. Presents memory as key-value store.
│   ├── MinTheory.lean -- "A minimal theory, safe for all LNSym proofs". Proof automation.
│   ├── Separate.lean -- Defines what it means for memory regions to overlap and be separated.
│   ├── SeparateProofs.lean -- Proofs about overlapping. Currently entirely sorryd, waiting on automation support.
│   ├── State.lean -- processor state.
│   ├── Util.lean -- FIXME macro.
│   └── mem_separate_for_subset.smt2
├── Arm.lean -- toplevel.
├── CONTRIBUTING.md
├── LICENSE
├── Main.lean
├── Makefile
├── NOTES.md
├── Proofs
│   ├── MultiInsts.lean -- small example with multiple instructions + symbolic simulation.
│   ├── Proofs.lean -- example tests, symbolic simulation of Sha512 upto 4 bits.
│   ├── Sha512_block_armv8.lean
│   ├── Sha512_block_armv8_rules.lean
│   └── Test.lean
├── Proofs.lean
├── README.md
├── Specs -- specification implementation of hash / crypto fns.
│   ├── AESArm.lean
│   ├── AESCommon.lean
│   ├── GCM.lean
│   ├── SHA512.lean
│   ├── SHA512Common.lean
│   └── Specs.lean
├── Tactics
│   └── Sym.lean -- symbolic evaluation.
├── Tactics.lean
├── Tests
│   ├── AES-GCM
│   │   ├── AESGCMDecKernelProgram.lean
│   │   ├── AESGCMEncKernelProgram.lean
│   │   ├── AESGCMProgramTests.lean
│   │   ├── AESGCMSpecTest.lean
│   │   ├── AESHWCtr32EncryptBlocksProgram.lean
│   │   ├── AESHWEncryptProgram.lean
│   │   ├── AESHWSetEncryptKeyProgram.lean
│   │   ├── AESSpecTest.lean
│   │   ├── AESV8ProgramTests.lean
│   │   ├── GCMGhashV8Program.lean
│   │   ├── GCMGmultV8Program.lean
│   │   ├── GCMInitV8Program.lean
│   │   └── GCMProgramTests.lean
│   ├── Common.lean -- `revflat (x : List (BitVec n)) : BitVec (n * x.length)`
│   ├── Debug.lean -- `trace_run`, `log_run`, `run_until`.
│   ├── LDSTTest.lean -- unit tests for LDST instructions.
│   ├── SHA2
│   │   ├── SHA512ProgramTest.lean -- gigantic SHA512 program and tests that run this.
│   │   ├── SHA512SpecTest.lean
│   │   └── SHA512StandardSpecTest.lean
│   └── Tests.lean
├── Tests.lean
├── doc
│   └── add_new_inst.md
├── lake-manifest.json
├── lakefile.lean
└── lean-toolchain

18 directories, 115 files
```

#### Instruction Types

- `DPI, DPR`: data processing instructions for register and immediate.
- `DPSFP`: Data Processing (SIMD and FP) Instructions
- `LDST`: Load/Store

#### CFG.lean
- Forward and backward control flow graphs.
- Graph represented as an adjacency list (list of edges).
