# Other FPGA Pack

This folder groups the core artifacts needed for bring-up on other FPGA targets.

Included:
- omni_titan_agi_top.bit
- model_contract.json
- tokenizer/
- SHA256SUMS
- BASE_L2_ANCHORING.md
- base_l2_anchor_receipt.json

Not committed to Git:
- weights_int4_FINAL.bin (excluded because GitHub enforces a per-file upload limit; see SHA256SUMS for expected hash)

Important:
- The `.bit` file is board-specific and may not run on different FPGA boards without a new build/port.
- The `.bin`, tokenizer, and contract are included as deployment references.

Anchoring:
- Hashes for `.bit` and `.bin` are anchored on Ethereum L2 Base.
- See `BASE_L2_ANCHORING.md` for tx links and `base_l2_anchor_receipt.json` for machine-readable proof.
