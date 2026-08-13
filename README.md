# SSSAM: Public Result Preview

This repository snapshot is a research preview accompanying a manuscript under review. It publishes the final prediction maps and high-level pseudocode so that the reported outputs can be inspected and independently evaluated. Full training/inference code and model weights are intentionally withheld during peer review and are planned for release after acceptance.

This is therefore a **public evidence package**, not yet a complete open-source implementation.

## Contents

```text
SSSAM/
├── PSEUDOCODE.md
├── RESULTS.md
├── ARTIFACT_COMMITMENT.sha256
├── RESULTS_MANIFEST.sha256
├── metrics/
└── results/
    ├── RGB-D/
    ├── RGB-T/
    └── RGB-NIR/
```

The prediction maps are the final method outputs used for the current paper draft:

- RGB-D: 3,044 maps on DES, LFSD, NJU2K, NLPR, SIP, SSD, and STERE.
- RGB-T: 4,321 maps on VT821, VT1000, and the VT5000 test split.
- RGB-NIR: 780 zero-shot maps produced with the RGB-T model without RGB-NIR fine-tuning.

All maps retain their benchmark filenames. RGB-T and RGB-NIR maps are saved at their corresponding ground-truth resolution. The ZIP file under `results/RGB-NIR/` is an exact archive of its `Images/` directory and is included only for convenient download.

## Integrity verification

From the repository root, verify every published result and metric file with:

```bash
sha256sum --check RESULTS_MANIFEST.sha256
```

`ARTIFACT_COMMITMENT.sha256` records hashes of the private final model source and checkpoints. The private files are not part of this preview, so that file is a timestamped commitment rather than a locally verifiable manifest. When the complete implementation is released, the disclosed artifacts can be checked against these hashes.

## Scope and use

The pseudocode describes the scientific data flow but deliberately omits implementation-level details such as tensor dimensions, layer schedules, expert definitions, preprocessing, initialization, and checkpoint conversion. It is documentation and cannot be executed.

Please cite the accompanying paper when using these results. See `NOTICE.md` for the status of this preview.
