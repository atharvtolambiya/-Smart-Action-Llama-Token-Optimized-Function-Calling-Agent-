# Llama-3 Action Agent (Function Calling)

## 🚀 The Problem
Standard LLMs (like GPT-4) are "chatty." When you ask them to trigger a software action (like "Check the weather"), they often reply with conversational filler ("Sure! Here is the weather function...") which breaks downstream software pipelines.

## 🛠️ The Solution
I fine-tuned **Llama-3-8B** to behave like a strict **Action Engine**.
- It does not speak English.
- It only speaks **Function Calls**.
- It uses a custom **YAML-based format** that is cleaner and more token-efficient than standard JSON.

## 📊 Results
The model learned to map natural language intent to executable code with high precision.

**User:** "Calculate the mortgage for a loan of $450,000 at 3.5% interest for 15 years."
**Model Output:**
name: calculate_mortgage
arguments:
  loan_amount: 450000
  interest_rate: 3.5
  loan_term: 15
<img width="778" height="542" alt="Screenshot 2025-12-04 172013" src="https://github.com/user-attachments/assets/c5c8fe2c-da28-4ea2-8c45-8986afb5f55c" />
