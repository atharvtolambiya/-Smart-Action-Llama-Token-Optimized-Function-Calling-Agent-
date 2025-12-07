# 🦙 Llama-3 Action Agent: Fine-Tuned for Function Calling

![Model](https://img.shields.io/badge/Model-Llama_3_8B-blue)
![Task](https://img.shields.io/badge/Task-Fine_Tuning-orange)
![Format](https://img.shields.io/badge/Output-TOON-green)

## 📌 Project Overview
This project transforms a standard conversational LLM into a strict **Action Engine** capable of driving downstream software pipelines.

While models like GPT-4 are powerful, they are designed for "Chat." When integrated into autonomous agents, their tendency to include conversational filler ("Sure, I can help with that...") breaks parsers and causes API failures.

**The Solution:** I fine-tuned **Llama-3-8B** to suppress natural language generation and output **only** executable function calls in a custom, token-efficient **TOON** format.

## 🛠️ Tech Stack
* **Base Model:** Meta Llama-3-8B
* **Fine-Tuning Framework:** Unsloth / PyTorch (LoRA Adapters)
* **Data Format:** TOON (Custom Optimized Schema)
* **Language:** Python

## ⚡ The Engineering Challenge
The core objective was to solve the **"Chatty LLM" Problem** in Agentic Workflows using a custom data structure.

### 1. The Problem (Standard LLM)
* **Input:** "Book a meeting with John for 2 PM."
* **Output:** "Certainly! I have found a function for that. Here is the code..."
* **Result:** ❌ **Pipeline Crash.** The downstream system cannot parse the conversational text.

### 2. The Solution (My Fine-Tuned Agent)
* **Input:** "Book a meeting with John for 2 PM."
* **Output:**
    ```text
    name: schedule_meeting
    arguments:
      attendee: John
      time: 14:00
    ```
* **Result:** ✅ **Success.** Clean, deterministic, executable output in TOON format.

## 📊 Results & Inference Demo
The model demonstrates high precision in mapping vague natural language intent to strict arguments.

### Model Output Example
<img width="1854" height="895" alt="Screenshot 2025-12-07 134355" src="https://github.com/user-attachments/assets/1cbec7f0-48c6-4b78-b5f3-cbca64ea09d8" />



**Test Case: Financial Calculation**
> **User Prompt:** "Calculate the mortgage for a loan of $450,000 at 3.5% interest for 15 years."

> **Model Response (Raw TOON Output):**
```text
name: calculate_mortgage
arguments:
  loan_amount: 450000
  interest_rate: 3.5
  loan_term: 15
````

## 🚀 How to Run This Model

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/your-username/llama3-action-agent.git](https://github.com/your-username/llama3-action-agent.git)
    ```
2.  **Install dependencies:**
    ```bash
    pip install torch transformers unsloth
    ```
3.  **Run Inference:**
    ```python
    from unsloth import FastLanguageModel

    # Load my fine-tuned adapter
    model, tokenizer = FastLanguageModel.from_pretrained(
        "your-username/llama3-function-calling-adapter"
    )

    prompt = "Get the stock price for Apple."
    inputs = tokenizer([prompt], return_tensors="pt").to("cuda")

    # The model will output only the TOON function call
    outputs = model.generate(**inputs, max_new_tokens=64)
    print(tokenizer.decode(outputs[0]))
    ```

## 🧠 Key Learnings

  * **Format Optimization:** Switched to **TOON** format for output structuring, improving token efficiency and lowering latency compared to standard JSON.
  * **Instruction Tuning:** Learned how to curate datasets that punish "conversational filler" and reward strict syntax adherence.
  * **LoRA (Low-Rank Adaptation):** Efficiently fine-tuned a 8B parameter model on consumer hardware without retraining all weights.

-----

*Created by [Atharv Tolambiya]*
