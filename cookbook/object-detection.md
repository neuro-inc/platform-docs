---
description: >-
  This page is slightly outdated. Our technical writers are updating it right
  now.
---

# Object Detection

[Run on Neu.ro](https://apps.neu.ro/ml-recipes/object-detection)

[Explore on GitHub](https://github.com/neuromation/ml-recipe-object-detection)

This recipe contains the code and procedures that are required in order to add new object classes into the object detection model. In object recognition, the addition of new classes is computationally expensive and involves fine-tuning to ensure the model performs well on both old and new classes. Usually, this involves a trade-off between the cost of labeling new datasets \(new objects in different environments and combinations\), model training time \(cost of GPU\), and ML Engineering time spent on experimentation and development. To make this recipe interactive, we are providing a reduced version of the [Common Objects in Context \(COCO\)](http://cocodataset.org) dataset that contains only classes that represent retail items. This dataset is further reduced to 100 images to reduce re-training time.

## Quick Start

#### 0. Sign up

#### 1. Install CLI and log in

```text
pip install -U neuromation
neuro login
```

#### 2. Run the recipe

```text
git clone git@github.com:neuromation/ml-recipe-object-detection.git
cd ml-recipe-object-detection
make setup
make jupyter
```

