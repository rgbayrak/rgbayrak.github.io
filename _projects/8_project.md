---
layout: page
title: Interactively Constructing Functional Brain Parcellations
description: 
img: assets/img/vu/pragma.png
importance: 3
category: weekday

---

**PRAGMA** is an interactive visualization system for constructing **scan-specific functional brain parcellations** from established population-level atlases.

While standard atlases are foundational in neuroimaging, they primarily encode **group-level functional organization** and often fail to capture **individual variability** or **state-dependent changes** in brain activity. PRAGMA addresses this gap by enabling domain experts to interactively refine and reorganize atlas parcels into individualized functional subdivisions.

The system combines **user-guided hierarchical clustering** with **linked visual views** that support reasoning about parcel similarity, hierarchy evolution, and correspondence to known atlases.

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid
            loading="eager"
            path="assets/img/vu/pragma-demo.gif"
            title="PRAGMA interactive demo"
            class="img-fluid rounded z-depth-1" %}
    </div>
</div>

---

### Abstract

A prominent goal of neuroimaging studies is mapping the human brain, in order to identify and delineate functionally meaningful regions and elucidate their roles in cognitive behaviors. These brain regions are typically represented by atlases that capture general trends over large populations. Despite being indispensable to neuroimaging experts, population-level atlases do not capture individual or state-dependent differences in functional organization. In this work, we present **PRAGMA**, an interactive visualization method that allows domain experts to derive **scan-specific parcellations from established atlases**. PRAGMA features a **user-driven hierarchical clustering scheme** for defining temporally correlated parcels at varying granularity.

The visualization design supports decisions about **when to expand, collapse, or merge parcels**, using a set of **linked and coordinated views** for:

* understanding the current hierarchy
* assessing intra-cluster variation
* relating emerging parcellations to established atlases

A user study with four neuroimaging experts demonstrated that PRAGMA can support exploration of **individualized and state-specific functional brain organization**, offering new insights into functional brain networks.

---

##### Overview

PRAGMA begins with an **established atlas prior** and allows experts to refine parcels using **temporally correlated fMRI signals**.


<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/vu/pragma.png" title="PRAGMA interactive workflow" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="caption">
    PRAGMA interface incorporates five complementary visuals: (A) abstract view of the hierarchical clustering of the brain through a node-link diagram, (B1) similarity of fMRI signal time-courses represented with a line plot with a shaded confidence interval, (B2) homogeneity glyphs within nodes, (C1) functional connectivity depicted as a chord diagram and (C2) anatomical location of parcels viewed on an orthographic template.
    </div>
</div>

The workflow typically follows:

1. **Load atlas-based parcels** for a scan
2. **Inspect temporal similarity structure**
3. **Interactively split or merge parcels**
4. **Evaluate intra-parcel variance**
5. **Compare refined regions to canonical atlas boundaries**
6. **Iterate until the desired granularity is reached**

This creates a hierarchy of increasingly specialized parcels that can reveal:

* subject-specific network organization
* task-state differences
* scan-specific deviations from atlas priors

---

## Methods Summary

PRAGMA combines a **Python-based clustering backend** with an **interactive D3.js visual analytics frontend** to support expert-guided functional brain parcellation.

The workflow begins with a reference atlas and scan-specific fMRI time series, from which parcel-level temporal correlations are computed. These similarity relationships drive a **hierarchical clustering pipeline**, allowing parcels to be recursively expanded, collapsed, or merged at different levels of granularity.

The backend handles:
- atlas and fMRI-derived connectivity preprocessing
- hierarchical clustering computations
- parcel similarity updates
- cluster tree management
- data serving through a lightweight Flask application

The frontend is built with **D3.js**, which powers the linked visual views used to inspect clustering hierarchies, compare parcel homogeneity, and relate emerging clusters back to the reference atlas.

To make the system easier to reproduce and share, the full application is **Dockerized**, and an accompanying **Observable notebook / D3 prototype** provides a lightweight browser-based way to explore the visualization design and clustering behavior interactively.

---

## A Note on Limitations

PRAGMA is best thought of as an **interactive hypothesis-generation and exploration tool**.

The resulting parcels still depend on:
- the starting atlas
- clustering thresholds
- fMRI signal quality
- expert judgment
- study-specific goals

Different researchers may make different decisions about when a parcel should be split, merged, or preserved. In practice, this flexibility is a strength: it allows the parcellation process to adapt to the scientific question rather than enforcing one universal clustering formalism.

---

##### Citation

If you use PRAGMA, please cite the IEEE VIS 2020 paper and link back to the original repository.

```bibtex
@inproceedings{bayrak2020pragma,
  title={PRAGMA: Interactively Constructing Functional Brain Parcellations},
  author={Bayrak, R. G., Hoang, N., Hansen, C. B., Chang, C., Berger, M.},
  booktitle={IEEE VIS},
  year={2020}
}
```

---

##### Links

* **Paper (arXiv):** [https://arxiv.org/abs/2009.01697](https://arxiv.org/abs/2009.01697)
* **Repo (GitHub):** [https://github.com/rgbayrak/PRAGMA/tree/master](https://github.com/rgbayrak/PRAGMA/tree/master)
* **Venue:** IEEE VIS 2020
