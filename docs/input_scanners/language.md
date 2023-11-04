# Language Scanner

This scanner identifies and assesses the authenticity of the language used in prompts.

## Attack

With the rise of sophisticated LLMs, there has been an increase in attempts to manipulate or "confuse" these models.
Some common tactics employed by users to attack LLMs include:

- **Jailbreaks and Prompt Injections in different languages**. For example, by utilizing unique aspects of the Japanese
  language to try and confuse the model. Paper: [Multilingual Jailbreak Challenges in Large Language Models](https://arxiv.org/abs/2310.06474)
- **Encapsulation & Overloading**: Using excessive code or surrounding prompts with a plethora of special characters to
  overload or trick the model.

The Language Scanner is designed to identify such attempts, assess the authenticity of the language used.

## How it works

At its core, the scanner leverages the capabilities of [lingua-py](https://github.com/pemistahl/lingua-py) library.
The primary function of the scanner is to analyze the input prompt, determine its language, and check if it's in the
list. It supports the [following languages](https://github.com/pemistahl/lingua-py#3-which-languages-are-supported).

## Usage

```python
from llm_guard.input_scanners import Language

scanner = Language(valid_languages=["en"], all_languages=["en", "de", "es", "it"], low_accuracy_mode=True)  # Add other valid language codes (ISO 639-1) as needed
sanitized_prompt, is_valid, risk_score = scanner.scan(prompt)
```

!!! info

    In high accuracy mode, the language detector consumes approximately 800 MB of memory if all language models are loaded. In low accuracy mode (default), memory consumption is reduced to approximately 60 MB.

## Optimization

1. Use `low_accuracy_mode` if possible.
2. Specify languages you'd need to analyze using `all_languages` parameter.

## Benchmarks

Environment:

- Platform: Amazon Linux 2
- Python Version: 3.11.6

Run the following script:

```sh
python benchmarks/run.py input Language
```

Results:

| Instance                | Time taken, s | Characters per Second | Total Length Processed |
|-------------------------|---------------|-----------------------|------------------------|
| inf1.xlarge (AWS)       | 0.4           | 34.98                 | 14                     |
| m5.large (AWS)          | 0.36          | 37.9                  | 14                     |
| g5.xlarge (AWS) **GPU** | 0.314         | 44.63                 | 14                     |
