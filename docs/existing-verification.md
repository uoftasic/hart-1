# TinyQV: existing verification

For issue [#2](https://github.com/uoftasic/hart-1/issues/2).

This doc describes how [TinyQV](https://github.com/MichaelBell/tinyQV) is already tested. We're using that as our baseline. Before we design any peripherals, we want to improve the verification of the RISC-V core itself.

## Which version

We use the `ttsky25a` branch of TinyQV, commit `baf7d4d`, the version that was made into a real chip on Tiny Tapeout SKY25a. It's copied into `src/tinyQV/`.

TinyQV's `main` branch is older and slightly different (for example, it has 4 interrupt inputs instead of 16), so don't use it as the reference.

## What exists today

TinyQV is checked in three ways:

1. **cocotb tests** (Python, simulated with Icarus Verilog). There are 8 testbenches, one per block of the core: ALU, register file, counters, decoder, core, CPU, memory controller and QSPI controller. They contain 52 tests in total, mixing hand-written cases with random ones. Each test checks the results against a small Python model written into the test.
2. **Formal proofs** (SymbiYosys). These prove the ALU and shifter always give the right answer, and check the QSPI controller against a set of rules for the first 65 clock cycles. A proof for the multiplier exists but is switched off because it's slow.
3. **Lint** (Verilator). This scans all the Verilog for suspicious code.

In TinyQV's own repo, all three run automatically on every push.

## Results

From `src/tinyQV/` in hart-1, all 52 tests and the 3 formal checks pass. A full test run takes about 3 to 7 minutes, and almost all of that is two random tests running on the slow Icarus simulator.

## What's missing

- No test runs the whole core (CPU and memory controller together), and no test runs a real program.
- No independent reference model. Expected results come from small models written into the tests, not from a proper RISC-V simulator.
- No coverage, so we can't tell what has and hasn't been tested.
- The timer (`time.v`) and the top-level wrapper (`tinyqv.v`) have no tests of their own.
- The tests aren't yet set up to run automatically in hart-1.

## Next

Improve verification of the core first (issue [#1](https://github.com/uoftasic/hart-1/issues/1)): move the tests to UVM on Verilator, add a proper reference model, and add coverage. Peripheral design comes after.

## How to run

```bash
cd src/tinyQV/test && make && ! grep failure *results.xml
cd ../verify && ./verify.sh
```
