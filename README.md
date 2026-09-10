# HypoGen_LA



# Fine-Tuning Dataset for Hypothesis Generation in Learning Analytics

## Overview

Welcome to the official repository for our fine-tuning dataset and code for Large Language Models (LLMs), specifically designed for use with ChatGPT's API. The primary goal of this repository is to provide a high-quality dataset for fine-tuning LLMs and offer others the opportunity to leverage it in their own projects.

This repository includes:

- A **publicly available dataset** designed for fine-tuning purposes to researchers.
- **Sample code** for fine-tuning LLMs using the ChatGPT API.
- Documentation on how to utilize the dataset and implement fine-tuning with ease.

## Purpose

As generating meaningful hypotheses and interpreting educational data analytics can be challenging, this dataset is intended to help researchers and practitioners build better models by fine-tuning pre-trained LLMs like ChatGPT. The dataset consists of carefully curated records aimed at improving model performance in specific domains such as education, learning analytics, and more.

## What's Included

1. **Fine-Tuning Dataset**
   - Structured data formatted for ease of use in LLM fine-tuning.
   - Data includes: keywords, abstracts, and hypotheses extracted from educational research papers.
   
2. **Fine-Tuning Code**
   - Python code for fine-tuning an LLM using the ChatGPT API.
   - Clear instructions on how to prepare and train the model with our dataset.

## How to Use

### Dataset

1. Download the dataset from the `/data` folder.
2. The dataset is in CSV format and includes fields such as:
   - `Keywords`: Core terms extracted from the papers.
   - `Abstract`: A summary of the research paper.
   - `Hypothesis`: Hypotheses generated from the research context.

### Fine-Tuning

To fine-tune your model with this dataset, follow these steps:

1. **Install the necessary libraries**:
   ```bash
   pip install openai pandas jsonlines
   ```

2. **Set up the ChatGPT API**:
   You'll need an OpenAI API key to get started. Sign up on [OpenAI's website](https://beta.openai.com/signup) and follow their instructions to get your API key.

3. **Run the fine-tuning code**:

   ```python
   import openai
   import pandas as pd

   #
   ft_data = []

    with open("ExtractedHypoLA.json") as f:
        data = json.load(f)
        for i in range(len(data)):
            if "keywords" in data[i] and "GenHypo" in data[i]:
                msgs = {}
                msgs["messages"] = []
                q_entry = {}
                q_entry["role"] = "user"
                q_entry["content"] = f'Generate hypotheses about {data[i]["keywords"]}'
            
                a_entry = {}
                a_entry["role"] = "assistant"
                a_entry["content"] = data[i]["ExtractedHypo"]
    
                msgs["messages"].append(q_entry)
                msgs["messages"].append(a_entry)
                ft_data.append(msgs)
            else:
                print(f"Warning: 'keywords' or 'GenHypo' not found in entry {i}. Skipping this entry.")
    
    with jsonlines.open("ftdata_HypoLA.jsonl", "w") as writer:
        writer.write_all(ft_data)
   
       # Configuration OpenAI API key configuration
    os.environ["OPENAI_API_KEY"] = "your-OpneAI-API-key"
    
    client = OpenAI(
        api_key=os.environ["OPENAI_API_KEY"],
    )
    
    client.files.create(
      file=open("/path_to_prepared_dataset/ftdata_HypoLA.jsonl", "rb"),
      purpose="fine-tune"
    )


   # Your OpenAI API key
   openai.api_key = 'your_api_key'

   # Fine-tuning configuration
       client = OpenAI(
        api_key=os.environ["OPENAI_API_KEY"],
    )
    
    client.fine_tuning.jobs.create(
      training_file="your-file-XXXXXXXXXXXXXXXXXXXXXXXX", 
      #model = "gpt-4o-2024-08-06"
      #model = "gpt-4o-mini-2024-07-18"
      #model = "gpt-4-0613"
      #model="gpt-3.5-turbo-0125"
    )

  
   ```

   Make sure your dataset is properly formatted before fine-tuning.

## Contributing

We welcome contributions from the community! If you have suggestions, improvements, or additional datasets you'd like to share, feel free to create a pull request or open an issue.

---

**Disclaimer**: Please make sure to follow OpenAI’s fine-tuning guidelines and respect ethical AI practices when using the provided dataset and code.

## License and Terms of Use

This dataset is provided for academic and non-commercial research purposes only, particularly for research in artificial intelligence, Learning Analytics, and related fields.

Commercial use is strictly prohibited.

Redistribution or modification is allowed for non-commercial academic research, **provided that appropriate attribution and citation are given**. In particular:

- If you redistribute the dataset (in whole or in part), adapt it, or include it in a derivative dataset,  
  you must clearly acknowledge the original “HypoGen_LA” dataset in your documentation.
- If you use the dataset in a publication, you must cite the associated paper and/or this repository  
  (e.g., by citing the “HypoGen_LA: Fine-Tuning Dataset for Hypothesis Generation in Learning Analytics” dataset and its authors).

By using this dataset, you agree to these terms.

Please note that the authors reserve the right to update, restrict access to, or withdraw this repository and its contents at any time, for example in response to copyright, contractual, institutional, or ethical considerations. Users who depend on this dataset for their research are encouraged to keep a local copy that complies with these terms.

---
##Human evaluation data (Takami, Majumdar & Flanagan, IEEE Access 2026)
This repository distributes the HypoGen_LA hypothesis-generation and fine-tuning dataset. It does not contain the participant-level human evaluation records (495 pairwise judgments by 5 learning-analytics experts and 6 K-12 practitioners) analysed in the paper. Because the evaluation involved a small number of participants, these records are not distributed publicly.

De-identified data necessary to reproduce the reported human-evaluation analyses (item-level A/B choices, difficulty flags, criteria selections, and the A/B model-pair mapping) may be made available by the corresponding author upon reasonable request, subject to the applicable participant-consent, research-ethics, and data-protection requirements.
---

##Note on terminology
The hypotheses in this dataset were generated by GPT-4o based on the abstracts of published LAK papers (Prompt 1 of the paper: "Based on the following abstract, generate a research hypothesis"). They were not extracted verbatim from the source papers. Earlier versions of this README described them as "extracted"; that wording has been corrected. See the Correction to the paper (IEEE Access, 2026) for details.

---

## Citation

If you use this dataset, code, or repository in your research, please cite the following paper:

> K. Takami, R. Majumdar, and B. Flanagan,  
> “When Even Experts Disagree: Human-in-the-Loop Evaluation of LLM-Generated Hypotheses for Learning Analytics,”  
> *IEEE Access*, vol. 14, pp. 106842–106860, 2026.  
> doi: 10.1109/ACCESS.2026.3708316

```bibtex
@article{takami2026when,
  author={Takami, Kyosuke and Majumdar, Rwitajit and Flanagan, Brendan},
  journal={IEEE Access},
  title={When Even Experts Disagree: Human-in-the-Loop Evaluation of LLM-Generated Hypotheses for Learning Analytics},
  year={2026},
  volume={14},
  pages={106842--106860},
  doi={10.1109/ACCESS.2026.3708316}
}


