# Model output explanation with SHAP

[SHAP](https://github.com/slundberg/shap) is a tool that provides the ability to explain outputs of machine learning models.

It's based on Shapley values from game theory and their related extensions (you can see a detailed explanation in the [SHAP documentation](https://shap.readthedocs.io/en/latest/example\_notebooks/overviews/An%20introduction%20to%20explainable%20AI%20with%20Shapley%20values.html)).

## Running SHAP

You can easily install SHAP via [PyPI](https://pypi.org/project/shap) or [conda-forge](https://anaconda.org/conda-forge/shap) and then run it in conjunction with platform using Jupyter Notebooks.

### Quick start

We already have a prepared Jupyter notebook with SHAP in our [Dogs demo project](https://github.com/neuro-inc/mlops-demo-oss-dogs).

Just follow the steps described in the project's `readme` and check how SHAP works with models focused on image classification tasks.&#x20;

### Using SHAP with your flows

The [official SHAP documentation](https://shap.readthedocs.io/en/latest/index.html) provides thorough and easy-to-follow guides on how to run SHAP in various environments.&#x20;

As platform is [integrated with Jupyter Notebooks](../../web/working-with-jupyter/jupyter-notebooks.md), running SHAP on the platform is just a matter of following the corresponding tutorials.
