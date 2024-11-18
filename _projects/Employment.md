---
layout: page
title: Employment Data Case Study
description: Business analysis with statistical inference
img: # assets/img/beer.jpg
importance: 6
category: school
---

<div class="row justify-content-sm-center">
    <div class="col-sm-7 mt-3 mt-md-0">
        <p>
        The objective of this project was to identify factors associated with employee turnover for a fictional company. We were then to build a model to predict attrition and make recommendations to the company leadership based on our findings.
        </p>
        <p>
        I completed this project with R. It includes an exploratory data analysis, visualizations, and some predictive modelling. I particularly like this project because of the more rigorous statistical inference done throughout, and think it's a good example of my ability to communicate technical concepts to a non-technical audience.
        </p>
        <p>
        See the notebook below for the full case study.
        </p>
    </div>
    <div class="col-sm-5 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/skyscraper.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<!-- <div class="caption">
    Please see the notebook below for the full case study.
</div> -->

{::nomarkdown}
{% assign jupyter_path = "assets/jupyter/CaseStudy2DDS.ipynb" | relative_url %}
{% capture notebook_exists %}{% file_exists assets/jupyter/CaseStudy2DDS.ipynb %}{% endcapture %}
{% if notebook_exists == "true" %}
    {% jupyter_notebook jupyter_path %}
{% else %}
    <p>Sorry, the notebook you are looking for does not exist.</p>
{% endif %}
{:/nomarkdown}
