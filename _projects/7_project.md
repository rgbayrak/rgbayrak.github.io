---
layout: page
title: Multi-API Literature Query Tool
description: Search the scientific literature across multiple academic APIs in one workflow
img: assets/img/vu/malq.png
importance: 4
category: weekday
---


The Multi-API Literature Query (MALQ) Tool solves this by letting you search multiple academic databases in a single workflow, combining broad coverage with reproducible search strategies. I built it for myself because it made it easy to **catch relevant papers in one reproducible workflow**. That personal need turned into a tool that I hope would help if you are doing broad or interdisciplinary literature discovery.


<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/vu/malq.png" title="<MALQ> pipeline (created with Gemini)" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

---

## What It Does

MALQ sends coordinated literature searches across several major scholarly APIs at once, including:

- PubMed
- Europe PMC
- Semantic Scholar
- OpenAlex
- Crossref
- CORE
- arXiv

---

## Why It’s Useful

MALQ improves **recall, breadth, and reproducibility** by making it easy to search across multiple ecosystems in parallel.

- literature reviews
- grant writing
- project scoping
- finding datasets and benchmark papers
- interdisciplinary discovery
- systematic and semi-systematic reviews
- building paper corpora for downstream NLP or RAG pipelines

---

## A Note on Limitations

MALQ is designed to cast a much wider net than searching a single database, but no automated literature search can guarantee perfect coverage.

Results vary depending on the wording of the query, API availability, provider-side ranking behavior, and rate limits. Some APIs also require keys for higher throughput or more reliable batching, while others may occasionally return incomplete metadata. 

In practice, I treat it as a **high-recall first pass**: a way to catch as much relevant work as possible early, then refine the search strategy based on what the first round reveals.

---

## Open Source

MALQ is open source and designed for practical use in real research workflows.

> **Code:** <a href="https://github.com/rgbayrak/harvest-paper">GitHub</a>

Whether you're preparing a systematic review, exploring a new topic, or building a paper corpus for machine learning, MALQ makes literature discovery faster, broader, and easier to reproduce.
