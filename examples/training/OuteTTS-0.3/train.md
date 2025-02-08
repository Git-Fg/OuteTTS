# OuteTTS Version 0.3 Training Guide

## Overview

The OuteTTS v0.3 model follows a typical language model training pipeline:
- **Pre-Training**
- **Supervised Fine-Tuning (SFT)**
- **Direct Preference Optimization (DPO)**

If you plan to introduce new languages, you may want to follow the same training pattern.

## Fine-Tuning Guide

### 1. Creating a Dataset

Before fine-tuning, you need to create a dataset. Use the provided processing script to generate the necessary data format:

- **Script Location:**  
  `examples/training/OuteTTS-0.3/data_creation_example.py`

- Ensure your raw data is formatted as Parquet files.
- Your data must contain:
    - A `"transcript"` field of type string.
    - An `"audio"` field containing audio bytes in `"bytes"` .

The script will process your data and save the resulting files in Parquet format with full prompts.

### 2. Preparing Fine-Tuning Data

To fine-tune the model, you will need to create two types of split SFT (Supervised Fine-Tuning) data:
- **Input Completion**
- **Speaker Completion**

#### A. Input Completion

This step involves preparing data where the model predicts completions based on given inputs.

**Input:**
```text
<|im_end|>
<|text_start|>this<|space|>is<|space|>a<|space|>test<|period|><|text_end|>
<|audio_start|>
```

**Target:**
```text
this<|t_0.15|><|27|><|1789|><|379|><|1236|><|1465|><|1326|><|1584|><|889|><|183|><|1283|><|794|><|space|>
is<|t_0.09|><|1281|><|903|><|1521|><|319|><|230|><|1533|><|906|><|space|>
a<|t_0.07|><|1300|><|1258|><|581|><|1113|><|557|><|space|>
test<|period|><|t_0.51|><|1734|><|1510|><|419|><|391|><|334|><|859|><|1588|><|592|><|858|><|911|><|1726|><|1140|><|346|><|135|><|569|><|206|><|1183|><|1272|><|1329|><|1446|><|1556|><|1752|><|1748|><|454|><|870|><|1349|><|1364|><|719|><|1370|><|1088|><|1238|><|253|><|767|><|1273|><|224|><|414|><|1359|><|1450|>
<|audio_end|>
<|im_end|>
```

#### B. Speaker Completion

For speaker completion, fine-tune the model to adapt to a speaker's style by including a portion of the speaker's input. The input length should vary for example between 5-10 seconds, depending on your speaker data. Randomly selecting different portions is recommended over using a fixed segment length.

**Example Format:**

**Input:**
```text
<|im_end|>
<|text_start|>this<|space|>is<|space|>a<|space|>test<|period|><|text_end|>
<|audio_start|>
this<|t_0.15|><|27|><|1789|><|379|><|1236|><|1465|><|1326|><|1584|><|889|><|183|><|1283|><|794|><|space|>
is<|t_0.09|><|1281|><|903|><|1521|><|319|><|230|><|1533|><|906|><|space|>
```

**Target:**
```text
a<|t_0.07|><|1300|><|1258|><|581|><|1113|><|557|><|space|>
test<|period|><|t_0.51|><|1734|><|1510|><|419|><|391|><|334|><|859|><|1588|><|592|><|858|><|911|><|1726|><|1140|><|346|><|135|><|569|><|206|><|1183|><|1272|><|1329|><|1446|><|1556|><|1752|><|1748|><|454|><|870|><|1349|><|1364|><|719|><|1370|><|1088|><|1238|><|253|><|767|><|1273|><|224|><|414|><|1359|><|1450|>
<|audio_end|>
<|im_end|>
```

### 3. Training the Model

Because OuteTTS is built on a language model architecture, it can be tuned using any preferred language model training library or framework. Some popular options include:

- [TorchTune](https://github.com/pytorch/torchtune)
- [Unsloth](https://github.com/unslothai/unsloth)
- [Hugging Face Transformers Trainer](https://huggingface.co/docs/trl/sft_trainer)

### Fine-Tuning for French TTS

To fine-tune OuteTTS for French TTS, follow the steps outlined in the [Google Colab notebook](fine_tuning_french_tts.ipynb). This notebook provides comprehensive instructions for setting up the environment, preparing the data, training the model, and evaluating the results specifically for French TTS.
