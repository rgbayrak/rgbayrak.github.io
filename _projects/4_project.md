---
layout: page
title: PhysAI Research Suite
description: Tools for physiological signal analysis in neuroimaging
img: assets/img/work/physai_prev.jpg
importance: 1
category: work
related_publications: false
---

<a href="https://neurdylab.github.io/physai">PhysAI</a> is a research suite I contribute to, dedicated to advancing the analysis of physiological signals collected concurrently with functional neuroimaging data. The tools are aimed to support research in exploring the complex interactions between the brain and body.

<div class="row justify-content-sm-center">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/work/physai1.jpg" title="PhysAI overview" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Overview of the PhysAI research suite.
</div>

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/work/physai3.jpg" title="Reconstruction pipeline" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Core team.
</div>

---

## Reconstruction Methods

Reconstruction methods enable the extraction of low-frequency cardiac and respiratory fluctuations directly from fMRI data. This is particularly valuable when physiological recordings are unavailable or compromised, allowing researchers to enrich the analysis and interpretation of fMRI data even without dedicated peripheral measurements.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/work/physai2.jpg" title="Reconstruction example" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Publications all in one place.
</div>

---

## Quality Assurance Tools

PhysAI includes both manual and automated tools for quality assessment of physiological recordings acquired during MRI sessions.

### PhysioQA

Systemic physiological processes have traditionally been treated as noise in fMRI analysis, but they are increasingly recognized as meaningful signals linked to cognitive processes. Neuroimaging research now frequently incorporates concurrent recordings of peripheral physiology to enhance fMRI studies. However, the value of this data depends heavily on recording quality and expertise in data handling.

Quality assessment presents several challenges: it is time-consuming, and ratings can vary significantly between reviewers. Existing approaches include manual and template-based tools for peak detection quality (such as physiopy's peakdet and PhysIO) and automated exclusion based on statistical summary metrics. However, there remains a gap in automated methods that can provide rapid, effective quality determination at the whole-scan or windowed level.

PhysioQA addresses this gap with automated quality assessment for physiological recordings, reducing the burden on researchers while improving consistency across studies.

### Manual QA Tool

For cases requiring human judgment or detailed inspection, there is a manual quality assessment interface. This tool allows researchers to visually inspect physiological recordings, flag artifacts, and annotate regions of concern—complementing the automated pipeline when nuanced evaluation is needed.
