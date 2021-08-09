# Объяснение вывода моделей с SHAP

[SHAP](https://github.com/slundberg/shap) - инструмент, позволяющий объяснять вывод моделей машинного обучения.

Он основан на векторах Шепли из теории игр и их расширениях \(подробное объяснение можете увидеть в [документации SHAP](https://shap.readthedocs.io/en/latest/example_notebooks/overviews/An%20introduction%20to%20explainable%20AI%20with%20Shapley%20values.html)\).

## Запуск SHAP

Вы можете легко установить SHAP с помощью [PyPI](https://pypi.org/project/shap) или [conda-forge](https://anaconda.org/conda-forge/shap) и затем запускать его вместе с платформой, используя Jupyter Notebooks.

### Быстрый старт

Готовый Jupyter notebook с SHAP уже есть в нашем [демо-проекте](https://github.com/neuro-inc/mlops-demo-oss-dogs).

Просто следуйте шагам, описанным в `readme` этого проекта, чтобы потом иметь возможность быстро проверить работу SHAP с моделями, сфокусированными на классификации изображений. 

### Использование SHAP с вашими проектами

[Oфициальная документация SHAP](https://shap.readthedocs.io/en/latest/index.html) предоставляет подробные и простые руководства по запуску SHAP в разных средах. 

Так как наша платформа [интегрирована с Jupyter Notebooks](../../web/working-with-jupyter/jupyter-notebooks.md), вам нужно будет всего лишь последовать соответствующим руководствам, чтобы запустить SHAP.

