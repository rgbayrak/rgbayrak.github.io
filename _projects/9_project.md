---
layout: page
title: PhysAI - Viz
description: Interactive Time Series Visualization Tool for BIDS Compatible Physiological Data
img: assets/img/vu/physviz_prev.png
importance: 3
category: weekday

---

In this project, I have built an interactive visualization tool for **quality assessment of BIDS-compatible physiological recordings**. It is built to make it easier to inspect physiological traces—such as respiration, and cardiac recordings—together with auxiliary signals like framewise displacement, scan timing, and signal quality metrics. It combines reusable D3 interactions with BIDS-aware data loading, the viewer makes it easy to assess signal quality before downstream application steps.

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid
            loading="eager"
            path="assets/img/vu/physviz.gif"
            title="PRAGMA interactive demo"
            class="img-fluid rounded z-depth-1" %}
    </div>
</div>

---

#### Methods Summary

The viewer is built with a **TypeScript + D3.js frontend architecture**, bundled with **Vite** for fast local development and modular deployment.

The core visualization system uses:

- **D3.js** for SVG rendering, scales, axes, zoom, pan, and brush interactions
- **TypeScript** for strongly typed chart logic and reusable visualization components
- **Vite** for hot-reload development and lightweight packaging
- a **focus + context chart design** for overview-to-detail browsing
- viewport-aware rendering for dense physiological traces
- BIDS-aware TSV parsing and metadata-driven signal loading

This stack makes it possible to smoothly navigate long physiological recordings while preserving interaction control for quality assessment workflows.

---

#### Why It’s Useful

Reliable physiological recordings are critical for:

- brain-body coupling studies
- cardiac and respiratory modeling
- retrospective denoising

Yet these recordings are often difficult to inspect efficiently. This viewer improves **speed, interpretability, and confidence in physiological QC** by combining signal browsing, metadata awareness, and auxiliary quality metrics in one interface.

---

#### Preview

Below figures are provding a preview of the tool, showcasing key functionality.

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/vu/physviz_prev.png" title="BIDS physiology quality assessment viewer" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
Interactive visualization of **BIDS-compatible physiological recordings** with metadata-aware scan navigation, channel selection, and synchronized signal browsing for rapid quality assessment.
</div>

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/vu/physviz_sqi.png" title="BIDS physiology quality assessment viewer" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
Signal quality metrics can be calculated on the fly. Integrated quality assessment panel showing implemented signal quality metrics, alongside extensible placeholders for additional measures derived from prior physiology literature.
</div>

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/vu/physviz_fd.png" title="BIDS physiology quality assessment viewer" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
Auxiliary motion context. Motion (in the example above, it is framewise displacement (FD)) is visualized alongside physiological traces as an auxiliary signal, helping distinguish physiological artifacts from motion-related corruption during scan-level quality assessment.
</div>

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/vu/physviz_zoom.png" title="BIDS physiology quality assessment viewer" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
Interactive zoom and brushing. Linked focus+context views support smooth zooming, panning, and brush-based temporal selection, enabling rapid transition from whole-scan inspection to fine-grained artifact analysis.
</div>

---

#### Open Source

This project is open source and designed for practical **BIDS-compatible physiology QC workflows**.

> **Code:** <a href="https://github.com/rgbayrak/d3-timeseries">GitHub</a>

Whether you're validating physiology traces, curating datasets, or building neuroimaging QC pipelines, this viewer makes long-form physiological signal assessment faster, clearer, and easier to trust.
