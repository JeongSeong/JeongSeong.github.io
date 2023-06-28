---
title: Managing randomness in pytorch
author:
  name: SeognYun Jeong
date: 2023-06-01 10:00:00
categories: [deep learning, pytorch]
tags: [pytorch, randomness, reproducibility]     # TAG names should always be lowercase
---

This post is for whom cannot fix the randomness in pytorch 

## When randomness cannot be fixed, [refer here](https://pytorch.org/docs/stable/generated/torch.use_deterministic_algorithms.html#torch.use_deterministic_algorithms)

There are nondeterministic operations that cannot be fixed, typically, adaptive pooling operations.
Those operations throw RuntimeError when ```torch.use_deterministic_algorithms(mode=True)```
Having a great test result through averaging good test results is more meaningful than having a great test result through fixing randomness.

## How to fix randomness otherwise
```python
import random
import numpy as np
import os
import torch
SEED=0

os.environ["PYTHONHASHSEED"]=str(SEED)
random.seed(SEED)
np.random.seed(SEED)

torch.manual_seed(SEED)
if torch.cuda.device_count() > 1:
    torch.cuda.manual_seed_all(SEED)
else:
    torch.cuda.manual_seed(SEED)

torch.backends.cudnn.benchmark = False
torch.backends.cudnn.deterministic = True
torch.backends.cudnn.enabled = False

def seed_worker(worker_id):
    # worker_seed = torch.initial_seed() % 2**32
    np.random.seed(SEED)
    random.seed(SEED)

g = torch.Generator()
g.manual_seed(0)
dataloader = data_utl.DataLoader(dataset, batch_size, worker_init_fn=seed_worker, generator=g)
```

[refernce](https://pytorch.org/docs/stable/notes/randomness.html)
[torch cuda manual_seed](https://pytorch.org/docs/stable/generated/torch.cuda.manual_seed.html)
[torch backends cudnn](https://pytorch.org/docs/stable/backends.html#module-torch.backends.cudnn)