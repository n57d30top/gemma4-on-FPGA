# Gemma 4 FPGA Artifact Bundle

This repository contains runtime artifacts for FPGA inference deployment.

## Included Files
- `for_other_fpgas/` (grouped portability pack with `.bit`, `.bin`, contract, tokenizer, hashes)
- `RELEASE_CHECKLIST.md` (release gate)
- `MODEL_PROVENANCE.md` (provenance template)
- `LICENSE` / `NOTICE` (legal files)

## Other FPGA Pack
Use `for_other_fpgas/` as the bundle for porting/bring-up work on other FPGA targets.

Important:
- `omni_titan_agi_top.bit` is board-specific and may not run unchanged on a different FPGA board.
- `weights_int4_FINAL.bin` is committed as 3 split parts (`weights_int4_FINAL.bin.part01..03`) to satisfy GitHub size limits.
- `model_contract.json` and `tokenizer/` are included for deployment compatibility and migration reference.
- Base L2 anchoring proof for `.bit`/`.bin` is included under `for_other_fpgas/BASE_L2_ANCHORING.md`.

## Scope
This is a deployment artifact repository.
It is not a training, distillation, or Vivado source repository.

## Architecture Rationale
Why this stack was chosen for FPGA deployment:

- HDC (Hyperdimensional Computing):
  - Robust against noisy activations and quantization artifacts.
  - Friendly for compact, integer-heavy hardware representations.
- Mamba-style sequence modeling:
  - Streaming-oriented sequence handling with low-latency state updates.
  - Better fit for edge inference than large attention-heavy baselines in this hardware class.
- KAN-style function blocks:
  - Good expressivity per parameter for compressed/quantized deployment paths.
  - Practical for LUT- and fixed-point-oriented hardware mappings.
- NPU core cluster:
  - Deterministic parallel compute path on FPGA fabric.
  - Throughput scales with replicated cores and fixed datapaths.

## Main Challenges
- Training compute ceiling:
  - Current available hardware cannot sustain enough epochs for reliable quality improvements.
- Prompt/template sensitivity:
  - Gemma-family chat formatting must be exact, otherwise output quality collapses.
- Tokenizer/contract pinning:
  - Any tokenizer drift or contract mismatch can break semantic correctness.
- Hardware/software boundary:
  - Quantization, memory layout, and deployment guards must remain strictly aligned.
- Artifact logistics:
  - Large weights exceed single-file GitHub limits and require split packaging.

## Performance Snapshot
Current observed runtime snapshot from bring-up testing:

- Output speed (short response): 16 words in 0.036112 s
- Equivalent throughput: about 443 words/s
- Practical shorthand used during testing: about 450 tokens/s on short outputs

TOPS note:
- A formally validated end-to-end TOPS benchmark is not yet published in this artifact bundle.
- Until a reproducible TOPS benchmark script is added, throughput should be interpreted via measured latency/words-per-second above.

## Training Status
Training and further iteration of `weights_int4_FINAL.bin` are currently paused.

Reason:
- The available hardware is too weak to run enough epochs at the required quality level.
- Continuing under current hardware limits would produce low-quality or unstable results and waste compute time.

Current strategy:
- We publish the existing artifact bundle for reproducible deployment.
- Future `.bin` training will resume only when stronger training hardware is available.

## Quick Start
1. Verify checksums:
   - `cd for_other_fpgas`
   - `sha256sum -c SHA256SUMS`
2. Program bitstream on target board.
3. Load weights and tokenizer.
4. Start API/runtime using `model_contract.json`.

## Compatibility
- Target board: `<set exact board name>`
- Runtime stack: `<set exact runtime versions>`
- API contract mode: `<set exact mode>`

## Legal
This repository is intended to comply with Gemma terms and Gemma 4 license requirements.
See:
- `LICENSE` (Dual-Use Non-Commercial for project-owned materials)
- `NOTICE`
- `MODEL_PROVENANCE.md`
- `RELEASE_CHECKLIST.md`

Important:
- Third-party components keep their original licenses.
- Gemma-related materials remain subject to https://ai.google.dev/gemma/terms
