# Model Training Evolution Comparison

## Key Observations

- **Pre-trained models show varied performance.** While the "paraphrase" model consistently scores high on "求財運" (Seeking financial luck), the "ckiplab" model performs well on "家人生病" (Family member is sick). The "distiluse" model's scores are generally lower. This suggests that the choice of pre-trained model significantly impacts the results.
- Fine-tuned models generally outperform pre-trained models. Across all four questions, the fine-tuned versions, particularly those with larger datasets (v6, v5, v4), tend to achieve higher scores for the most relevant label. This demonstrates the effectiveness of fine-tuning for specific tasks.

- **Data augmentation and dataset size seem to improve performance.** Models trained on augmented datasets (v6-cross-2k, v4-binary) often have slightly higher scores compared to their non-augmented counterparts. Also, models trained on larger datasets (v6, v5) generally perform better. This highlights the importance of data quantity and quality in model training.

- **Binary vs. Multi-label training shows mixed results.** For some questions like "求財運," both binary and multi-label models perform similarly well. However, for other questions like "工作、升遷," there is more variation in performance and label predictions. This suggests that the choice between binary and multi-label training depends on the specific task and dataset characteristics.

- **v1-binary's low performance is likely due to limited data.** Trained on only 15 data points, v1-binary struggles to achieve high scores, especially for complex questions. This emphasizes the need for sufficient data for effective model training.

## Fine-tuning Configuration Comparison

| Version     | Data Size | Augmentation | Training Type | Epochs | Learning Rate | Batch Size | Warmup Ratio | Weight Decay |
| ----------- | --------- | ------------ | ------------- | ------ | ------------- | ---------- | ------------ | ------------ |
| v6-cross-2k | 2000      | Yes          | Cross         | 100    | 2e-5          | 8          | 0.1          | 0.05         |
| v6-cross-1k | 1000      | No           | Cross         | 50     | 2e-5          | 8          | 0.1          | 0.05         |
| v5-binary   | 1000      | No           | Binary        | 100    | 1e-5          | 8          | 0.1          | 0.05         |
| v4-binary   | 2000      | Yes          | Binary        | 100    | 1e-5          | 8          | 0.2          | 0.01         |
| v3-binary   | 150       | No           | Binary        | 100\*  | 2e-5          | 8          | 0.1          | 0.01         |
| v2-binary   | 150       | No           | Binary        | 100    | 2e-5          | 16         | N/A          | 0.01         |
| v1-binary   | 15        | No           | Binary        | 100    | 2e-5          | 16         | N/A          | 0.01         |

\*Early stopped at epoch 12

## Loss Metrics Comparison

| Version     | Train Loss | Val Loss |
| ----------- | ---------- | -------- |
| v6-cross-2k | 0.110400   | 0.605767 |
| v6-cross-1k | 0.179800   | 0.603712 |
| v5-binary   | 0.590500   | 3.511295 |
| v4-binary   | 0.102700   | 3.548775 |
| v3-binary   | 1.296900   | 3.635640 |
| v2-binary   | 0.090300   | 0.055652 |
| v1-binary   | 0.109500   | 0.068439 |

## Sample Prediction Scores Comparison

### "這是一段與感情相關的提問" (This is a question related to relationships)

| Version         | Top Label (Score) | Second Label (Score) | Third Label (Score) |
| --------------- | ----------------- | -------------------- | ------------------- |
| **ckiplab**     | **自身 (0.7377)** | 交易 (0.7210)        | 六甲 (0.7134)       |
| **paraphrase**  | **尋人 (0.4725)** | 自身 (0.3932)        | 求財 (0.3293)       |
| **distiluse**   | **婚姻 (0.0780)** | 尋人 (0.0650)        | 求財 (0.0511)       |
| **v6-cross-2k** | **婚姻 (0.5089)** | 疾病 (0.4724)        | 尋人 (0.4575)       |
| **v6-cross-1k** | **疾病 (0.5254)** | 婚姻 (0.5166)        | 尋人 (0.5075)       |
| **v5-binary**   | **婚姻 (0.5320)** | 疾病 (0.5233)        | 尋人 (0.5064)       |
| **v4-binary**   | **婚姻 (0.5320)** | 疾病 (0.5233)        | 尋人 (0.5064)       |
| **v3-binary**   | **疾病 (0.4218)** | 婚姻 (0.4105)        | 求財 (0.3415)       |
| **v2-binary**   | **疾病 (0.4218)** | 婚姻 (0.4105)        | 求財 (0.3415)       |
| **v1-binary**   | **婚姻 (0.1919)** | 家宅 (0.1358)        | 行人 (0.0847)       |

### "求財運" (Seeking financial luck)

| Version         | Top Label (Score) | Second Label (Score) | Third Label (Score) |
| --------------- | ----------------- | -------------------- | ------------------- |
| **ckiplab**     | **求財 (0.9002)** | 家宅 (0.7499)        | 田蠶 (0.7413)       |
| **paraphrase**  | **求財 (0.9539)** | 交易 (0.7383)        | 尋人 (0.6301)       |
| **distiluse**   | **求財 (0.8898)** | 尋人 (0.6541)        | 失物 (0.5772)       |
| **v6-cross-2k** | **求財 (0.8084)** | 家宅 (0.5507)        | 失物 (0.5341)       |
| **v6-cross-1k** | **求財 (0.8464)** | 家宅 (0.5849)        | 失物 (0.5737)       |
| **v5-binary**   | **求財 (0.8566)** | 家宅 (0.5942)        | 失物 (0.5853)       |
| **v4-binary**   | **求財 (0.8566)** | 家宅 (0.5942)        | 失物 (0.5853)       |
| **v3-binary**   | **求財 (0.8188)** | 失物 (0.3252)        | 家宅 (0.2961)       |
| **v2-binary**   | **求財 (0.8188)** | 失物 (0.3252)        | 家宅 (0.2961)       |
| **v1-binary**   | **自身 (0.3014)** | 交易 (0.0787)        | 疾病 (0.0613)       |

### "家人生病" (Family member is sick)

| Version         | Top Label (Score) | Second Label (Score) | Third Label (Score) |
| --------------- | ----------------- | -------------------- | ------------------- |
| **ckiplab**     | **疾病 (0.8383)** | 自身 (0.7947)        | 行人 (0.7845)       |
| **paraphrase**  | **疾病 (0.5064)** | 失物 (0.3659)        | 公訟 (0.2886)       |
| **distiluse**   | **疾病 (0.6435)** | 家宅 (0.4307)        | 婚姻 (0.4110)       |
| **v6-cross-2k** | **疾病 (0.7211)** | 婚姻 (0.5350)        | 家宅 (0.4917)       |
| **v6-cross-1k** | **疾病 (0.7418)** | 婚姻 (0.5514)        | 家宅 (0.5403)       |
| **v5-binary**   | **疾病 (0.7393)** | 尋人 (0.5723)        | 婚姻 (0.5671)       |
| **v4-binary**   | **疾病 (0.7393)** | 尋人 (0.5723)        | 婚姻 (0.5671)       |
| **v3-binary**   | **疾病 (0.6642)** | 婚姻 (0.4274)        | 家宅 (0.4214)       |
| **v2-binary**   | **疾病 (0.6642)** | 婚姻 (0.4274)        | 家宅 (0.4214)       |
| **v1-binary**   | **家宅 (0.1779)** | 移徙 (0.1030)        | 六畜 (0.0741)       |

### "工作、升遷" (Work and promotion)

| Version         | Top Label (Score) | Second Label (Score) | Third Label (Score) |
| --------------- | ----------------- | -------------------- | ------------------- |
| **ckiplab**     | **自身 (0.8424)** | 六甲 (0.8408)        | 田蠶 (0.8408)       |
| **paraphrase**  | **移徙 (0.4977)** | 求財 (0.4932)        | 行人 (0.4726)       |
| **distiluse**   | **行人 (0.4566)** | 尋人 (0.4455)        | 移徙 (0.3838)       |
| **v6-cross-2k** | **移徙 (0.6313)** | 尋人 (0.5586)        | 疾病 (0.5568)       |
| **v6-cross-1k** | **家宅 (0.6318)** | 田蠶 (0.6259)        | 尋人 (0.6226)       |
| **v5-binary**   | **自身 (0.6704)** | 家宅 (0.6558)        | 疾病 (0.6511)       |
| **v4-binary**   | **自身 (0.6674)** | 家宅 (0.6558)        | 疾病 (0.6511)       |
| **v3-binary**   | **六甲 (0.4494)** | 田蠶 (0.4494)        | 行人 (0.4493)       |
| **v2-binary**   | **六甲 (0.4494)** | 田蠶 (0.4494)        | 行人 (0.4493)       |
| **v1-binary**   | **家宅 (0.1311)** | 交易 (0.1201)        | 移徙 (0.1021)       |

**Note:** The bold labels indicate the highest score for each model and question combination.

<br>
<br>
<br>

# Model References (Pre-trained)

## **ckiplab/bert-base-chinese**

**Model**

- FM: ckiplab/bert-base-chinese
- BERT_tokenizer: BertTokenizer
- BERT_model: BertModel

**Results**

- Compute cosine similarity on Questions and 15 labels directly:

```
這是一段與感情相關的提問:==========
Label: 自身, Score: 0.7377
Label: 交易, Score: 0.7210
Label: 六甲, Score: 0.7134
求財運:==========
Label: 求財, Score: 0.9002
Label: 家宅, Score: 0.7499
Label: 田蠶, Score: 0.7413
家人生病:==========
Label: 疾病, Score: 0.8383
Label: 自身, Score: 0.7947
Label: 行人, Score: 0.7845
工作、升遷:==========
Label: 自身, Score: 0.8424
Label: 六甲, Score: 0.8408
Label: 田蠶, Score: 0.8408
```

## **paraphrase-multilingual-MiniLM-L12-v2**

**Model**

- FM: sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2
- BERT_model: SentenceTransformer
- BERT_tokenizer: model.encode directly

**Results**

- Compute cosine similarity on Questions and 15 labels directly:

```
這是一段與感情相關的提問:==========
Label: 尋人, Score: 0.4725
Label: 自身, Score: 0.3932
Label: 求財, Score: 0.3293
求財運:==========
Label: 求財, Score: 0.9539
Label: 交易, Score: 0.7383
Label: 尋人, Score: 0.6301
家人生病:==========
Label: 疾病, Score: 0.5064
Label: 失物, Score: 0.3659
Label: 公訟, Score: 0.2886
工作、升遷:==========
Label: 移徙, Score: 0.4977
Label: 求財, Score: 0.4932
Label: 行人, Score: 0.4726
```

## **distiluse-base-multilingual-cased-v2**

**Model**

- FM: sentence-transformers/distiluse-base-multilingual-cased-v2
- BERT_model: SentenceTransformer
- BERT_tokenizer: model.encode directly

**Results**

- Compute cosine similarity on Questions and 15 labels directly:

```
這是一段與感情相關的提問:==========
Label: 婚姻, Score: 0.0780
Label: 尋人, Score: 0.0650
Label: 求財, Score: 0.0511
求財運:==========
Label: 求財, Score: 0.8898
Label: 尋人, Score: 0.6541
Label: 失物, Score: 0.5772
家人生病:==========
Label: 疾病, Score: 0.6435
Label: 家宅, Score: 0.4307
Label: 婚姻, Score: 0.4110
工作、升遷:==========
Label: 行人, Score: 0.4566
Label: 尋人, Score: 0.4455
Label: 移徙, Score: 0.3838
```

# Model References (Fine-tuned)

## **v6-cross-2k-data-aug**

**Model**

- FM: ckiplab/bert-base-chinese
- BERT_tokenizer: BertTokenizer
- BERT_model: BertForSequenceClassification

**Background**

- Labels: 2k categories (augmented)
- Data: Enhanced with data augmentation
- Trainer: **BERTMultiLabelTrainer**
  - Parameters:
    - Train/val data splitting: Yes
    - learning_rate: 2e-5
    - per_device_train_batch_size: 8
    - per_device_eval_batch_size: 8
    - num_train_epochs: 100
    - optimizer: AdamW
    - warmup_ratio: 0.1
    - weight_decay: 0.05

**Results**

The numbers in the last epoch:

- train/loss: 0.110400
- val/loss: 0.605767
- sentences similarity:

```
這是一段與感情相關的提問:==========
Label: 婚姻, Score: 0.5089
Label: 疾病, Score: 0.4724
Label: 尋人, Score: 0.4575
求財運:==========
Label: 求財, Score: 0.8084
Label: 家宅, Score: 0.5507
Label: 失物, Score: 0.5341
家人生病:==========
Label: 疾病, Score: 0.7211
Label: 婚姻, Score: 0.5350
Label: 家宅, Score: 0.4917
工作、升遷:==========
Label: 移徙, Score: 0.6313
Label: 尋人, Score: 0.5586
Label: 疾病, Score: 0.5568
```

---

## **v6-cross-1k-data**

**Model**

- FM: ckiplab/bert-base-chinese
- BERT_tokenizer: BertTokenizer
- BERT_model: BertForSequenceClassification

**Background**

- Labels: 1k categories
- Data: Cross-validation used, non-augmented
- Trainer: **BERTMultiLabelTrainer**
  - Parameters:
    - Train/val data splitting: Yes
    - learning_rate: 2e-5
    - per_device_train_batch_size: 8
    - per_device_eval_batch_size: 8
    - num_train_epochs: 50
    - optimizer: AdamW
    - warmup_ratio: 0.1
    - weight_decay: 0.05

**Results**

The numbers in the last epoch:

- train/loss: 0.179800
- val/loss: 0.603712
- sentences similarity:

```
這是一段與感情相關的提問:==========
Label: 疾病, Score: 0.5254
Label: 婚姻, Score: 0.5166
Label: 尋人, Score: 0.5075
求財運:==========
Label: 求財, Score: 0.8464
Label: 家宅, Score: 0.5849
Label: 失物, Score: 0.5737
家人生病:==========
Label: 疾病, Score: 0.7418
Label: 婚姻, Score: 0.5514
Label: 家宅, Score: 0.5403
工作、升遷:==========
Label: 家宅, Score: 0.6318
Label: 田蠶, Score: 0.6259
Label: 尋人, Score: 0.6226
```

---

## **v5-binary-1k-data**

**Model**

- FM: ckiplab/bert-base-chinese
- BERT_tokenizer: BertTokenizer
- BERT_model: BertForSequenceClassification

**Background**

- Labels: Binary categories (1k data points)
- Data: Non-augmented
- Trainer: BERTTrainer
  - Parameters:
    - Train/val data splitting: Yes
    - learning_rate: 1e-5
    - per_device_train_batch_size: 8
    - per_device_eval_batch_size: 8
    - num_train_epochs: 100
    - optimizer: AdamW
    - warmup_ratio: 0.1
    - weight_decay: 0.05

**Results**

The numbers in the last epoch:

- train/loss: 0.590500
- val/loss: 3.511295
- sentences similarity:

```
這是一段與感情相關的提問:==========
Label: 婚姻, Score: 0.5320
Label: 疾病, Score: 0.5233
Label: 尋人, Score: 0.5064
求財運:==========
Label: 求財, Score: 0.8566
Label: 家宅, Score: 0.5942
Label: 失物, Score: 0.5853
家人生病:==========
Label: 疾病, Score: 0.7393
Label: 尋人, Score: 0.5723
Label: 婚姻, Score: 0.5671
工作、升遷:==========
Label: 自身, Score: 0.6704
Label: 家宅, Score: 0.6558
Label: 疾病, Score: 0.6511
```

---

## **v4-binary-2k-data-aug**

**Model**

- FM: ckiplab/bert-base-chinese
- BERT_tokenizer: BertTokenizer
- BERT_model: BertForSequenceClassification

**Background**

- Labels: Binary categories (2k data points, augmented)
- Data: Augmented
- Trainer: BERTTrainer
  - Parameters:
    - Train/val data splitting: Yes
    - learning_rate: 1e-5
    - per_device_train_batch_size: 8
    - per_device_eval_batch_size: 8
    - num_train_epochs: 100
    - optimizer: AdamW
    - warmup_ratio: 0.2
    - weight_decay: 0.01

**Results**

The numbers in the last epoch:

- train/loss: 0.102700
- val/loss: 3.548775
- sentences similarity:

```
這是一段與感情相關的提問:==========
Label: 婚姻, Score: 0.5320
Label: 疾病, Score: 0.5233
Label: 尋人, Score: 0.5064
求財運:==========
Label: 求財, Score: 0.8566
Label: 家宅, Score: 0.5942
Label: 失物, Score: 0.5853
家人生病:==========
Label: 疾病, Score: 0.7393
Label: 尋人, Score: 0.5723
Label: 婚姻, Score: 0.5671
工作、升遷:==========
Label: 自身, Score: 0.6674
Label: 家宅, Score: 0.6558
Label: 疾病, Score: 0.6511
```

---

## **v3-binary-150-data-split**

**Model**

- FM: ckiplab/bert-base-chinese
- BERT_tokenizer: BertTokenizer
- BERT_model: BertForSequenceClassification

**Background**

- Labels: Binary categories (150 data points)
- Data: Non-augmented
- Trainer: BERTTrainer
  - Parameters:
    - Train/val data splitting: Yes
    - learning_rate: 2e-5
    - per_device_train_batch_size: 8
    - per_device_eval_batch_size: 8
    - num_train_epochs: 100
    - optimizer: AdamW
    - warmup_ratio: 0.1
    - weight_decay: 0.01

**Results**

The numbers in the last epoch:
(Notes: Early stopped at 12th due to underperforming)

- train/loss: 1.296900
- val/loss: 3.635640
- sentences similarity:

```
這是一段與感情相關的提問:==========
Label: 疾病, Score: 0.4218
Label: 婚姻, Score: 0.4105
Label: 求財, Score: 0.3415
求財運:==========
Label: 求財, Score: 0.8188
Label: 失物, Score: 0.3252
Label: 家宅, Score: 0.2961
家人生病:==========
Label: 疾病, Score: 0.6642
Label: 婚姻, Score: 0.4274
Label: 家宅, Score: 0.4214
工作、升遷:==========
Label: 六甲, Score: 0.4494
Label: 田蠶, Score: 0.4494
Label: 行人, Score: 0.4493
```

---

## **v2-binary-150-data**

**Model**

- FM: ckiplab/bert-base-chinese
- BERT_tokenizer: BertTokenizer
- BERT_model: BertForSequenceClassification

**Background**

- Labels: Binary categories (150 data points)
- Data: Non-augmented
- Trainer: BERTTrainer
  - Parameters:
    - Train/val data splitting: No
    - learning_rate: 2e-5
    - per_device_train_batch_size: 16
    - per_device_eval_batch_size: 16
    - num_train_epochs: 100
    - optimizer: AdamW
    - weight_decay: 0.01

**Results**

The numbers in the last epoch:

- train/loss: 0.090300
- val/loss: 0.055652
- sentences similarity:

```
這是一段與感情相關的提問:==========
Label: 疾病, Score: 0.4218
Label: 婚姻, Score: 0.4105
Label: 求財, Score: 0.3415
求財運:==========
Label: 求財, Score: 0.8188
Label: 失物, Score: 0.3252
Label: 家宅, Score: 0.2961
家人生病:==========
Label: 疾病, Score: 0.6642
Label: 婚姻, Score: 0.4274
Label: 家宅, Score: 0.4214
工作、升遷:==========
Label: 六甲, Score: 0.4494
Label: 田蠶, Score: 0.4494
Label: 行人, Score: 0.4493
```

---

## **v1-binary-15-data**

**Model**

- FM: ckiplab/bert-base-chinese
- BERT_tokenizer: BertTokenizer
- BERT_model: BertForSequenceClassification

**Background**

- Labels: Binary categories (15 data points)
- Data: Non-augmented
- Trainer: BERTTrainer
  - Parameters:
    - Train/val data splitting: No
    - learning_rate: 2e-5
    - per_device_train_batch_size: 16
    - per_device_eval_batch_size: 16
    - num_train_epochs: 100
    - optimizer: AdamW
    - weight_decay: 0.01

**Results**

The numbers in the last epoch:

- train/loss: 0.109500
- val/loss: 0.068439
- sentences similarity:

```
這是一段與感情相關的提問:==========
Label: 婚姻, Score: 0.1919
Label: 家宅, Score: 0.1358
Label: 行人, Score: 0.0847
求財運:==========
Label: 自身, Score: 0.3014
Label: 交易, Score: 0.0787
Label: 疾病, Score: 0.0613
家人生病:==========
Label: 家宅, Score: 0.1779
Label: 移徙, Score: 0.1030
Label: 六畜, Score: 0.0741
工作、升遷:==========
Label: 家宅, Score: 0.1311
Label: 交易, Score: 0.1201
Label: 移徙, Score: 0.1021
```
