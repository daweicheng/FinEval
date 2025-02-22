<div align="center">
  <img src="docs/en/_static/image/FinEval.jpg" width="500px"/>
  <br />
  <br />

[![license](https://img.shields.io/badge/License-Apache--2.0-blue.svg)](https://github.com/SUFE-AIFLM-Lab/FinEval/blob/main/LICENSE)

[🌐Website](https://fineval.readthedocs.io/en/latest/index.html) |
[🤗Hugging Face](https://huggingface.co/datasets/SUFE-AIFLM-Lab/FinEval) |
[📃Paper](https://arxiv.org/abs/2308.09975)

English | [简体中文](README_zh-CN.md)

</div>


Welcome to **FinEval**

Large language models (LLMs) have demonstrated exceptional performance in various natural language processing tasks, yet their efficacy in more challenging and domain-specific tasks remains largely unexplored. This paper presents FinEval, a benchmark specifically designed for the financial domain knowledge in the LLMs.
  
FinEval is a collection of high-quality multiple-choice questions covering **finance,economy, accounting, and certificate**. It includes 4,661 questions spanning 34 different academic subjects. To ensure a comprehensive model performance evaluation, FinEval employs various methods, including zero-shot, few-shot, answer-only,and chain-of-thought prompts. Evaluating state-of-the-art Chinese and English LLMs on FinEval, the results show that only GPT-4 has an average accuracy rate close to 70%, indicating significant growth potential for LLMs in the financial domain knowledge. Our work offers a more comprehensive financial knowledge evaluation benchmark, utilizing data of mock exams and covering a wide range of evaluated LLMs.

<div align="center">
  <img src="docs/en/_static/image/subjects.png" width="1000px" height="475px"/>
  <br />
  <br /></div>


## Contents

- [Performance Leaderboard](#performance-leaderboard)
- [Installation](#installation)
- [Evaluation](#evaluation)
- [Dataset Preparation](#dataset-preparation)
- [Supporting New Datasets and Models](#supporting-new-datasets-and-models)
- [How to Submit](#how-to-submit)
- [Citation](#citation)

## Performance Leaderboard

We divide the evaluation into **Answer Only** and **Chain of Thought**. For examples of prompts for both methods, please refer to [zero-shot for Answer Only](/docs/en/prompt/zero_shot.md), [few-shot for Answer Only](/docs/en/prompt/few_shot.md), and [Chain of Thought](/docs/en/prompt/cot.md).

Below is the average accuracy(%) on the test split. We report the average accuracy over the subjects within each category. "Average" column indicates the average accuracy over all the subjects. Notably, we only report the results from each model under the best setting, which is determined by the highest average accuracy achieved among four settings (i.e., zero- and few-shot learning with and without CoT):

| Model                  | Size    | Finance | Economy | Accounting | Certificate | Average |
|------------------------|---------|:-------:|:-------:|:----------:|:-----------:|:-------:|
| GPT-4                  | unknown |  71.0   |  74.5   |    59.3    |    70.4     |  68.6   |
| CFGPT-v2-13B           | 13B     |  57.3   |  56.2   |    51.2    |    57.6     |  55.6   |
| ChatGPT                | 175B    |  59.3   |  61.6   |    45.2    |    55.1     |  55.0   |
| Qwen-7B                | 7B      |  54.5   |  54.4   |    50.3    |    55.8     |  53.8   |
| Qwen-Chat-7B           | 7B      |  51.5   |  52.1   |    44.5    |    53.6     |  50.5   |
| Baichuan-13B-Base      | 13B     |  52.6   |  50.2   |    43.4    |    53.5     |  50.1   |
| Baichuan-13B-Chat      | 13B     |  51.6   |  51.1   |    41.7    |    52.8     |  49.4   |
| ChatGLM2-6B            | 6B      |  46.5   |  46.4   |    44.5    |    51.5     |  47.4   |
| InternLM-7B            | 7B      |  49.0   |  49.2   |    40.5    |    49.4     |  47.1   |
| InternLM-Chat-7B       | 7B      |  48.4   |  49.1   |    40.8    |    49.5     |  47.0   |
| LLaMA-2-Chat-70B       | 70B     |  47.1   |  46.7   |    41.5    |    45.7     |  45.2   |
| Falcon-40B             | 40B     |  45.4   |  43.2   |    35.8    |    44.8     |  42.4   |
| Baichuan-7B            | 7B      |  44.9   |  41.5   |    34.9    |    45.6     |  42.0   |
| LLaMA-2-Chat-13B       | 13B     |  41.6   |  38.4   |    34.1    |    42.1     |  39.3   |
| Ziya-LLaMA-13B-v1      | 13B     |  43.3   |  36.9   |    34.3    |    41.2     |  39.3   |
| Bloomz-7b1-mt          | 7B      |  41.4   |  42.1   |    32.5    |    39.7     |  38.8   |
| LLaMA-2-13B            | 13B     |  39.5   |  38.6   |    31.6    |    39.6     |  37.4   |
| ChatGLM-6B             | 6B      |  38.8   |  36.2   |    33.8    |    39.1     |  37.2   |
| Chinese-Llama-2-7B     | 7B      |  37.8   |  37.8   |    31.4    |    36.7     |  35.9   |
| Chinese-Alpaca-Plus-7B | 7B      |  30.5   |  33.4   |    32.7    |    38.5     |  34.0   |
| moss-moon-003-sft      | 16B     |  35.6   |  34.3   |    28.7    |    35.6     |  33.7   |
| LLaMA-2-Chat-7B        | 7B      |  35.6   |  31.8   |    31.9    |    34.0     |  33.5   |
| LLaMA-2-7B             | 7B      |  34.9   |  36.4   |    31.4    |    31.6     |  33.4   |
| AquilaChat-7B          | 7B      |  34.2   |  31.3   |    29.8    |    36.2     |  33.1   |
| moss-moon-003-base     | 16B     |  32.2   |  33.1   |    29.2    |    30.7     |  31.2   |
| Aquila-7B              | 7B      |  27.1   |  31.6   |    32.4    |    33.6     |  31.2   |
| LLaMA-13B              | 13B     |  33.1   |  29.7   |    27.2    |    33.6     |  31.1   |
| Falcon-7B              | 7B      |  28.5   |  28.2   |    27.5    |    27.4     |  27.9   |

## Installation

Below are the steps for quick installation. For detailed instructions, please refer to the [Installation Guide](docs/en/get_started/install.md).

 ```python
    conda create --name fineval_venv python=3.8
    conda activate fineval_venv
 ```

```python
    git clone https://github.com/SUFE-AIFLM-Lab/FinEval
    cd FinEval
    pip install -r requirements.txt
 ```   


## Dataset Preparation

Download the dataset using Hugging Face datasets. Run the command to **manually download** and decompress, run the following command in the Fineval/code project directory, and rename it to data, and prepare the dataset to the FinEval/code/data directory.

```text
cd code/data
wget https://huggingface.co/datasets/SUFE-AIFLM-Lab/FinEval/resolve/main/FinEval.zip
unzip FinEval.zip
```

After the dataset is decompressed, the file format is as follows:

- -----data
   ----dev: The dev set for each subject contains five demonstration examples and explanations provided by the few-shot assessment
   ----val: The val set is mainly used for the self-test model score, and the score can be obtained directly
   ----test: used for the final evaluation of the model, the answers of the test set will not be made public, users are required to submit the evaluation results of `submission.json`, and the obtained scores will participate in the final leaderboard


## Evaluation

Please read [Get started quickly](/docs/en/get_started/quick_start.md) to learn how to run an evaluation task.

## Supporting New Datasets and Models

If you need to incorporate a new dataset for evaluation, please refer to [Add a dataset](/docs/en/advanced_guides/new_dataset.md).

If you need to load a new model, please refer to [Add a Model](/docs/en/advanced_guides/new_model.md).

## How to Submit

The location for saving the assessment results is: `output_path=$PROJ_HOME/output_dir/${exp_name}/$exp_date`. Within this folder, the `submission.json` file is generated automatically. Users only need to submit this file.

Instructions for the saving location can be found in the [How to run](/docs/en/user_guide/how_to_run.md) section.

```text
## The key inside each subject is the "id" field in the dataset
{
    "banking_practitioner_qualification_certificate": {
        "0": "A",
        "1": "B",
        "2": "B",
        ...
    },
    
    "Subject Name":{
    "0":"Answer1",
    "1":"Answer2",
    ...
    }
    ....
}
```

You can submit the generated `submission.json` file to zhang.liwen@shufe.edu.cn by email.


## Citation

```bibtex
@misc{2308.09975,
Author = {Liwen Zhang and Weige Cai and Zhaowei Liu and Zhi Yang and Wei Dai and Yujie Liao and Qianru Qin and Yifei Li and Xingyu Liu and Zhiqiang Liu and Zhoufan Zhu and Anbo Wu and Xin Guo and Yun Chen},
Title = {FinEval: A Chinese Financial Domain Knowledge Evaluation Benchmark for Large Language Models},
Year = {2023},
Eprint = {arXiv:2308.09975},
}
```
