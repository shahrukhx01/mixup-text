# mixup-text
This repository contains implementation of mixup strategy for text classification. The implementation is primarily based on the paper [Augmenting Data with Mixup for Sentence Classification: An Empirical Study
](https://arxiv.org/abs/1905.08941), although there is some difference.

Three variants of mixup are considered for text classification
1. Embedding mixup: Texts are mixed immediately after word embeedding
2. Hidden/Encoder mixup: Mixup is done prior to the last fully connected layer
3.  Sentence mixup: Mixup is done before softmax

# Run Supervised Training with Mixup Augmentation

```python
from tqdm import tqdm

SAMPLES_PER_CLASS = [50, 100, 150, 200, 250]
N_AUGMENT = [0, 2, 4, 8, 16]
DATASETS = ['bace', 'bbbp']
METHODS = ['embed', 'encoder', 'sent']
OUTPUT_FILE = 'eval_result_mixup_augment_v1.csv'
N_TRIALS = 20
EPOCHS = 20

for method in METHODS:
  for dataset in DATASETS:
      for SAMPLE in SAMPLES_PER_CLASS:
          for n_augment in N_AUGMENT:
              for i in tqdm(range(N_TRIALS)):
                  !python pseudo_label/main.py --dataset-name={dataset} --epoch={EPOCHS} \
                  --batch-size=16 --model-name-or-path=shahrukhx01/muv2x-simcse-smole-bert \
                  --samples-per-class={SAMPLE} --eval-after={EPOCHS} --train-log=0 --train-ssl=0 \
                  --out-file={OUTPUT_FILE} --n-augment={n_augment} --method={method}
                  !cat {OUTPUT_FILE}
```
