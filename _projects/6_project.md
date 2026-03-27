---
layout: page
title: DeepPhysioRecon
description: Reconstructing physiological signals from fMRI dynamics
img: assets/img/publication_preview/dpr.png
importance: 1
category: weekday
---

Many fMRI studies lack physiological measurements, which limits both the interpretation and richness of the data. Natural fluctuations in breathing and heart rate provide windows into cognition, emotion, and health—and they also heavily influence fMRI signals. DeepPhysioRecon addresses this gap by decoding continuous variations in respiration and heart rate directly from whole-brain fMRI dynamics.

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/vu/dpr1.png" title="DeepPhysioRecon pipeline" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The DeepPhysioRecon pipeline extracts ROI time-series from fMRI data and uses a bidirectional LSTM network to jointly estimate respiration variation (RV) and heart rate (HR).
</div>

---

## The Problem

The brain and body are closely coupled. Functional MRI measures blood oxygenation, which is influenced not only by neural activity but also by physiological processes like breathing and heart rate. These low-frequency fluctuations (below 0.15 Hz) can account for substantial variance in fMRI signals—in some brain regions, up to 50% of the signal.

However, many existing fMRI datasets lack physiological recordings entirely, and even when recordings exist, they are often noisy or corrupted. This limits our ability to properly interpret fMRI results or study brain-body interactions.

---

## Our Approach

DeepPhysioRecon uses a bidirectional Long Short-Term Memory (LSTM) network to jointly predict respiration variation and heart rate from fMRI time-series data. The key insight is that RV and HR are coupled, so learning them together improves accuracy for both.

The framework works with conventional fMRI acquisitions (TR > 1s) and does not require fast multiband sampling or slice-based reconstructions. This makes it applicable to the vast majority of existing fMRI datasets.

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/vu/dpr2.png" title="Example predicted signals" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Left: Example of predicted (solid) vs. measured (dashed) RV and HR signals for one scan. Right: Model performance across cross-validation folds.
</div>

---

## Key Results

Models trained on resting-state fMRI data from the Human Connectome Project achieved median correlations of approximately 0.68 for RV and 0.63 for HR against measured physiological signals. The models generalized across experimental conditions, different subjects, and even different scanners and acquisition parameters.

The predicted signals can be used to account for physiological effects in fMRI analyses. We showed that removing predicted RV and HR from fMRI data alters functional connectivity maps in ways that closely match what happens when using measured physiological signals.

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/vu/dpr3.png" title="Variance explained" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Left: Percentage of fMRI variance explained by measured vs. predicted physiological signals. Right: Impact on functional connectivity matrices.
</div>

---

## Generalizability

We tested the models across seven different task conditions from HCP and an external dataset with different acquisition parameters (including TR of 2.1s vs. 0.72s). The resting-state trained models transferred well to task data, and task-trained models showed improved performance on held-out tasks.

This suggests that the relationship between fMRI and physiology is largely brain state-invariant, making pre-trained models broadly applicable.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/vu/dpr4.png" title="Generalization across datasets" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Model performance across different experimental conditions and datasets.
</div>

---

## Potential for Denoising

An unexpected finding: in cases where measured physiological recordings contained artifacts, the predicted signals appeared to smooth over these corruptions. This suggests DeepPhysioRecon may also be useful for cleaning up noisy physiological data, not just replacing missing data.

---

## Code and Data

DeepPhysioRecon is open source and available for use:

- **Code:** <a href="https://github.com/neurdylab/deep-physio-recon">GitHub</a>
- **Model weights:** <a href="https://huggingface.co/rgbayrak/deep-physio-recon">Hugging Face</a>
- **Paper:** <a href="https://doi.org/10.1162/IMAG.a.163">Imaging Neuroscience</a>

---

## Citation

Bayrak RG, Hansen CB, Salas JA, Ahmed N, Lyu I, Mather M, Huo Y, Chang C. DeepPhysioRecon: Tracing peripheral physiology in low frequency fMRI dynamics. *Imaging Neuroscience*. 2025;3. <a href="https://doi.org/10.1162/IMAG.a.163">10.1162/IMAG.a.163</a> 