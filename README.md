# Gemma 4 FPGA Artifact Bundle

This repository contains a reproducible deployment bundle for running Gemma-based inference on a Xilinx Kria KV260 FPGA.

## Included Files
- `for_other_fpgas/` (bundle with `.bit`, split `.bin` parts, contract, tokenizer, hashes, and anchoring receipts)
- `RELEASE_CHECKLIST.md`
- `MODEL_PROVENANCE.md`
- `LICENSE` / `NOTICE`

## Project Purpose
The goal is deterministic, reproducible edge inference on FPGA:
- stable hardware/software coupling,
- strict artifact integrity checks,
- and a portable deployment process for bring-up on compatible targets.

## Why KV260
We chose KV260 because it is practical for real edge systems:
- Zynq UltraScale+ MPSoC (ZU5EV) combines ARM control-plane + FPGA programmable logic.
- Quad-core ARM Cortex-A53 runs Linux/orchestration while PL executes parallel compute.
- Vision I/O path (MIPI-CSI + ISP support in kit ecosystem) makes camera-to-inference pipelines realistic.

## Deployed Core Profile (Important)
This release corresponds to the deployed **36-core hardware profile**.

Note:
- Earlier internal/experimental documents referenced 64-core variants.
- Those 64-core references are not the source of truth for this released artifact line.

## Gemma Model Used
- Teacher model ID in this release line:
  - `dealignai/Gemma-4-31B-JANG_4M-CRACK`
- Source of truth:
  - `for_other_fpgas/model_contract.json` (`teacher_model_id`)
- Pinned tokenizer:
  - `for_other_fpgas/tokenizer/tokenizer.json`
  - SHA-256: `3151898c022536cf420b732dd2fcbf8e7c456cd39711a27f9b82a7ced72b6c83`

## Why We Used The CRACK Variant
Short answer: bring-up reliability and debugging clarity.

- The CRACK line was already pinned in the working contract + tokenizer path.
- With limited training hardware, we prioritized a reproducible working line over restarting from zero.
- During hardware bring-up, reduced refusal behavior helps separate runtime/hardware faults from alignment refusals.

Difference vs normal aligned Gemma chat variants:
- Normal aligned chat models are more likely to refuse certain requests.
- CRACK variants are modified to reduce refusal behavior.
- This changes safety behavior and should be treated accordingly.

## Architecture Rationale
- HDC path:
  - robust under quantization/noise and friendly for integer-heavy hardware flows.
- Mamba-style sequence path:
  - low-latency state updates for streaming generation.
- KAN-style compute path:
  - strong expressivity for compressed deployment regimes.
- NPU core cluster:
  - deterministic parallel datapaths on FPGA fabric.

## ELI5: What The Cores Do
Think of the system like a factory line:
- Input/Embed path converts tokens into vectors.
- Sequence/State path keeps context moving forward each step.
- Compute path performs the heavy math.
- Selection path chooses the next token (top-k/temperature).
- Control/DMA path keeps memory and compute fed without stalls.

## Performance Snapshot
Observed bring-up snapshot:
- 16 words in 0.036112 s
- about 443 words/s (often rounded to about 450 tok/s on short outputs)

TOPS note:
- A fully standardized end-to-end TOPS benchmark is not yet published in this bundle.
- Until then, use measured latency/throughput as the practical runtime indicator.

## Main Challenges
- Training compute ceiling (insufficient hardware for enough high-quality epochs).
- Prompt/template sensitivity for Gemma-family chat formatting.
- Tokenizer/contract pinning (drift causes semantic failures).
- Strict HW/SW alignment for quantization and memory layout.
- Artifact logistics for very large weight files.

## Training Status
Further iteration of `weights_int4_FINAL.bin` is currently paused.

Reason:
- Available hardware cannot sustain enough epochs for reliable quality gains.

Current strategy:
- Ship reproducible deployment artifacts now.
- Resume further training when stronger training hardware is available.

## Other FPGA Pack
Use `for_other_fpgas/` as the portability pack.

Important:
- `omni_titan_agi_top.bit` is board-specific.
- `weights_int4_FINAL.bin` is split into:
  - `weights_int4_FINAL.bin.part01`
  - `weights_int4_FINAL.bin.part02`
  - `weights_int4_FINAL.bin.part03`
- See `for_other_fpgas/README.md` for reassembly commands.

## Integrity And Anchoring (Base L2)
Anchoring proof files:
- `for_other_fpgas/BASE_L2_ANCHORING.md`
- `for_other_fpgas/base_l2_anchor_receipt.json`

Anchored hashes:
- bit SHA-256: `983ab226ae213f984dc0eb33f427dc51a486b79a111d6d3e719344c660b1070b`
- bin SHA-256: `7ba5c0c5b350a8b0c50c7ec7fe30b64064bee4f13ce6d588eeb826d84d3644ce`
- bundle root SHA-256: `a29d570c5770edb91ffe6a66e65b335e1112dde6d33c9b37ed9beb998e89f6b4`

Base tx links:
- bit: `https://basescan.org/tx/0xb2b3e6090364fbe2952cdc55ff407fcf9642447e2602cca035fcbf9fb2e37402`
- bin: `https://basescan.org/tx/0xf367271e1eeaecf2f805039fff60cfdfe913ebf377eb6d77df97421e3d9c06e9`
- root: `https://basescan.org/tx/0xe3907bab98fd1b5d46bdbd545830787dd3a439149d77b9cd0f6924cd1c235a79`

## Quick Start
1. `cd for_other_fpgas`
2. verify checksums: `sha256sum -c SHA256SUMS`
3. program bitstream on target board
4. load weights + tokenizer
5. start runtime using `model_contract.json`

## Compatibility
- Target board: Xilinx Kria KV260 (this release line)
- Runtime stack: see deployment environment used by your board image
- API contract mode: see `model_contract.json`

## Legal
This repository is intended to comply with Gemma terms and Gemma 4 license requirements.

See:
- `LICENSE` (Dual-Use Non-Commercial License v2.0 for Project-Owned Materials)
- `NOTICE`
- `MODEL_PROVENANCE.md`
- `RELEASE_CHECKLIST.md`

Important:
- Third-party components keep their original licenses.
- Gemma-related materials remain subject to https://ai.google.dev/gemma/terms
- Commercial use is prohibited unless explicit written permission is granted by maintainers.

Legal hardening note:
- The license text is intentionally strict and drafted for enforceability (definitions, scope, restrictions, termination, liability, severability, and third-party precedence).
- No custom license can be guaranteed as \"unattackable\" across all jurisdictions; for production/commercial exposure, run a local attorney review and set the contact placeholder in `LICENSE`.
