# Release Checklist (Gemma 4 Artifacts)

Date: 2026-04-05
Owner: <set owner>

## 1) License + Terms
- [ ] Confirm source model license path and version date.
- [ ] Include Gemma terms link in repo docs.
- [ ] Include Gemma 4 license reference (Apache 2.0 link).
- [ ] Add `NOTICE` file with required Gemma notice text.
- [ ] Ensure prohibited-use constraints are carried forward in Terms/README.

## 2) Artifact Integrity
- [ ] Add `omni_titan_agi_top.bit`.
- [ ] Add `weights_int4_FINAL.bin`.
- [ ] Add `model_contract.json`.
- [ ] Add exact tokenizer files (`tokenizer.json`, `tokenizer_config.json`, etc.).
- [ ] Generate `SHA256SUMS` for all shipped artifacts.
- [ ] Verify hashes on a clean machine before publish.

## 3) Reproducibility + Provenance
- [ ] Fill `MODEL_PROVENANCE.md` with:
  - [ ] upstream model ID
  - [ ] commit/revision hash
  - [ ] quantization/distillation method
  - [ ] tokenizer source + hash
  - [ ] contract hash
- [ ] Mark modified components clearly.
- [ ] State known limitations and supported boards.

## 4) Security + Privacy
- [ ] Ensure no private keys in repo.
- [ ] Ensure no hardcoded passwords/IP credentials in scripts.
- [ ] Ensure no internal-only network topology docs are exposed unintentionally.
- [ ] Ensure no personal data or logs with sensitive content are included.

## 5) Operational Readiness
- [ ] Provide minimal deployment instructions in README.
- [ ] Provide health-check example call.
- [ ] Provide rollback note (known-good artifact set).
- [ ] Publish support policy (best effort / no SLA).

## 6) Final Gate (Go/No-Go)
- [ ] Legal sign-off
- [ ] Security sign-off
- [ ] Technical sign-off
- [ ] Tag release (`vX.Y.Z`)
- [ ] Attach checksums and release notes

## Required Links
- Gemma Terms: https://ai.google.dev/gemma/terms
- Gemma 4 license page: https://ai.google.dev/gemma/apache_2
