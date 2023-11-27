---
layout: page
title: Ames Housing Prices 2
description: TODO Change descriptions
img: # assets/img/beer.jpg
importance: 6
category: school
---

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        <p>
        This was one of the first term projects I completed in the MSDS Program. Notionally, we were to study data on the craft beers available in the US, and make recommendations to Budweiser on how they could approach that market. 
        </p>
        <p>
        We did the analysis entirely using R. It involved a lot of data cleaning and manipulation, an exploratory data analysis, and some statistical inference. While Python is more versatile and has better support for machine learning, I do enjoy using R for statistical analysis and quick visualization.
        </p>
        <p>
        This project was done as a pair team, which worked really well. The code and writeup below are largely my work, and the presentation (not shown) was mostly his.
        </p>
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.html path="assets/img/beer1.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Please see the notebook below for the full case study.
</div>

{::nomarkdown}
{% assign jupyter_path = "assets/jupyter/HousePricesCaseStudy2.ipynb" | relative_url %}
{% capture notebook_exists %}{% file_exists assets/jupyter/HousePricesCaseStudy2.ipynb %}{% endcapture %}
{% if notebook_exists == "true" %}
    {% jupyter_notebook jupyter_path %}
{% else %}
    <p>Sorry, the notebook you are looking for does not exist.</p>
{% endif %}
{:/nomarkdown}