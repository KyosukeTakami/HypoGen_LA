# HypoGen_LA

Here's a sample introductory page for your GitHub repository:

---

# Fine-Tuning Dataset for Hypothesis Generation in Learning Analystics

## Overview

Welcome to the official repository for our fine-tuning dataset and code for Large Language Models (LLMs), specifically designed for use with ChatGPT's API. The primary goal of this repository is to provide a high-quality dataset for fine-tuning LLMs and offer others the opportunity to leverage it in their own projects.

This repository includes:

- A **publicly available dataset** designed for fine-tuning purposes.
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

## License

This repository is licensed under the MIT License. See the `LICENSE` file for more details.

---

Feel free to adapt this page to fit your specific project needs!
