---
layout: page
title: Ames Housing Prices- paper
description: MSDS 6371 Project 1 Paper
date: 2022-02-01 17:39:00
# img: assets/img/12.jpg
importance: 1
category: school
redirect: /assets/pdf/MSDS6371FinalProject.pdf
---

Redirecting to the project paper.

{::nomarkdown}
{% assign jupyter_path = "assets/jupyter/Lab01.ipynb" | relative_url %}
{% capture notebook_exists %}{% file_exists assets/jupyter/Lab01.ipynb %}{% endcapture %}
{% if notebook_exists == "true" %}
    {% jupyter_notebook jupyter_path %}
{% else %}
    <p>Sorry, the notebook you are looking for does not exist.</p>
{% endif %}
{:/nomarkdown}