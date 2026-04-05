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
- `weights_int4_FINAL.bin` is not committed to Git due GitHub per-file size limits and must be transferred separately.
- `model_contract.json` and `tokenizer/` are included for deployment compatibility and migration reference.
- Base L2 anchoring proof for `.bit`/`.bin` is included under `for_other_fpgas/BASE_L2_ANCHORING.md`.

## Scope
This is a deployment artifact repository.
It is not a training, distillation, or Vivado source repository.

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
