# Confidence-Gated Adaptive Fusion for Multimodal Biometric Identification

Code and experiments for *"Confidence-Gated Adaptive Fusion for Multimodal Biometric Identification: Correct Modality Trust Without Guaranteed Accuracy Gains Under Severe Degradation."*

## Abstract

Multimodal biometric identification systems — combining fingerprint, iris, and voice here — are assumed more robust than single-modality systems, but that robustness depends heavily on how modalities are fused, especially when one or more is degraded or unavailable at inference. This work investigates whether per-sample, confidence-gated modality weighting offers a measurable benefit over fixed-weight fusion under missing-modality conditions, using a closed-set system trained on a 204-identity dataset built from SOCOFing (fingerprint), IITD (iris), and a VoxCeleb1 subset (voice). An initial adaptive gating mechanism failed to outperform a fairly-trained naive baseline; three causes were identified and addressed: curriculum sampling, confidence calibration, and a supervised gate objective. The resulting gate achieves near-perfectly decisive and correct modality trust — but a six-run pooled statistical evaluation shows this correct gating does not translate into better fused accuracy: under the two most severe degradation conditions, adaptive fusion is significantly outperformed by naive fixed-weight fusion. This dissociation between gate correctness and downstream accuracy is the main finding of this study.

## Repository structure

```
notebooks/
├── data\_preparation/            # dataset splitting / reconciliation scripts
├── 01\_unimodal\_training/        # fingerprint, iris, voice expert training
├── 02\_fusion\_training/
│   ├── adaptive\_naive\_fusion\_main.ipynb
│   └── seed\_runs/               # 6 independently-seeded reproducibility runs
├── 03\_statistical\_analysis/     # pooled McNemar significance testing
├── 04\_explainability/           # Integrated Gradients per modality
└── archive\_development\_history/ # earlier ablation-stage runs (feed Table 8)
```

## Setup

```bash
pip install -r requirements.txt
```

Notebooks were run on Kaggle (dual NVIDIA T4 GPUs, mixed precision). No GPU-specific code paths are required to adapt them elsewhere, but training time will vary.

## Datasets

Not included in this repository — obtain them directly:

* **Fingerprint (SOCOFing):** public Kaggle dataset.
* **Iris (IIT Delhi Iris Database):** request access via the database's official portal.
* **Voice (VoxCeleb1):** Hugging Face mirror or original VoxCeleb1 source.

See `notebooks/data\_preparation/` for the exact splitting and 204-identity virtual-subject reconciliation procedure used across all three modalities.

## Reproducing the results

1. **Prepare data** — run the notebooks in `data\_preparation/`.
2. **Train unimodal experts** — run the three notebooks in `01\_unimodal\_training/`, one per modality.
3. **Train fusion models** — run `02\_fusion\_training/adaptive\_naive\_fusion\_main.ipynb`, then each notebook in `seed\_runs/` for the multi-seed reproducibility analysis.
4. **Statistical testing** — run both notebooks in `03\_statistical\_analysis/` to reproduce the pooled McNemar significance results.
5. **Explainability** — run the notebooks in `04\_explainability/` for Integrated Gradients attribution per modality.

## Key results

|Modality|Top-1|Top-3|Top-5|
|-|-|-|-|
|Fingerprint (SOCOFing)|99.25%|99.97%|99.97%|
|Iris (IITD)|99.51%|99.75%|99.75%|
|Voice (VoxCeleb1 subset)|92.41%|97.20%|98.19%|


Fused (all modalities present): **100%** top-1 identification accuracy.

Under the two most severe single-modality-survival conditions, naive fixed-weight fusion significantly outperforms the adaptive gate (six-run pooled McNemar test, p < 0.05), despite the gate allocating >99.9% of trust to the correct surviving modality.

## Citation

```
```

## License

MIT

## Acknowledgments

This work was conducted under the supervision of Prof. Pawan Kumar Singh, Department of Information Technology, Jadavpur University.

