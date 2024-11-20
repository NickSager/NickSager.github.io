---
layout: page
title: Clustering - MNIST
description: Applying clustering techniques for image recognition
img: assets/img/mnist-digits-crop.jpg
importance: 4
category: school
---
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/mnist-digits.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<!-- <div class="caption">
    Handwritten Digits from the MNIST dataset
</div> -->

In this project, we were assigned to use clustering techniques on a large dataset and evaluate how well they performed at a task. I thought it would be interesting to try clustering on the MNIST [dataset](http://yann.lecun.com/exdb/mnist/), which is a collection of 70,000 handwritten digits commonly used for image recognition and evaluating machine learning algorithms. This dataset presented the opportunity to gain exposure to some image processing, and to use clustering algorithms for something unusual. My idea for this project wasn't to try to beat the state of the art in image classification, but to see whether the I could group digits in a meaningful way without training labels. The idea is that if one had a large collection of symbols, an unsupervised technique like clustering might be a useful preprocessing step. Images could be grouped by similarity, and then manually labeled for training a more sophisticated model for classification.

This actually worked pretty well. When used for prediction on the test set, Spectral Clustering groups digits with 73% accuracy. Neural networks regularly achieve 97%+ accuracy on this dataset, but clustering digits without labels can still make generating labels significantly more efficient.

I am particularly proud of the section on parallelizing a grid search for hyperparameter tuning on the DBSCAN model. Sci-kit Learn does not have a predict method for most clustering algorithms, so I had to search the parameter space with nested `for` loops. To do this efficiently, I used [dask](https://www.dask.org) to parallelize the search. Python would normally do this serially using a single thread on a single core, so this is a huge improvement. I always enjoy finding ways to better use the computing resources I have. Additionally, because some of the models lack the predict method, there isn't a way to assign unseen data to the clusters. To solve this, I fit a KNN model to the training data and assigned labels, and used that model to predict labels for the test data. Again, this was unconventional use of ML techniques but makes sense for the proposed use case. Below is the notebook for this project.




{::nomarkdown}
{% assign jupyter_path = "assets/jupyter/Clustering-MNIST.ipynb" | relative_url %}
{% capture notebook_exists %}{% file_exists assets/jupyter/Clustering-MNIST.ipynb %}{% endcapture %}
{% if notebook_exists == "true" %}
    {% jupyter_notebook jupyter_path %}
{% else %}
    <p>Sorry, the notebook you are looking for does not exist.</p>
{% endif %}
{:/nomarkdown}
