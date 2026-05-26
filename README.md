

#  README.md — Jailbreaking Transformer Models Using Logit Bias Manipulation  
**Author:** Alex Nite  
**Note:** This repository contains **only my individual contribution** to a team project.  
My section focuses on adapting the *JailMine* jailbreak technique for the DeepSeek‑R1:7B model.

---

##  Overview
This project explores how censorship and bias behave in large language models (LLMs) when refusal mechanisms are weakened or bypassed. My contribution focuses on **JailMine**, a jailbreak method that manipulates token‑level probabilities to suppress refusal phrases and increase the likelihood of restricted or harmful outputs.

Because DeepSeek‑R1:7B does not expose internal logits, I implemented an API‑based adaptation using **logit_bias**, allowing external control over token probabilities.

---

## Objective
- Override refusal behavior using token‑level probability manipulation  
- Compare **control vs. jailbroken** responses  
- Examine whether reducing censorship affects **bias**  
- Demonstrate how logit nudging changes model output  

---

##  Method Summary (My Contribution)

### 1. Inspect token log probabilities  
Using `logprobs=5`, I retrieved the top tokens the model intended to output.

### 2. Identify refusal tokens  
Common refusal tokens included:  
- “sorry”  
- “can’t”  
- “comply”  
- “I’m”  

### 3. Suppress refusal tokens  
```json
{
  "can't": -100.0,
  "sorry": -100.0,
  "comply": -100.0
}
```

### 4. Boost harmful/instructional tokens  
```json
{
  "build": 80.0,
  "steps": 80.0,
  "materials": 80.0
}
```

### 5. Generate jailbroken outputs  
This shifted the softmax distribution, making the model more likely to produce uncensored responses.

---

## Results (My Section Only)

### Control  
The model refused harmful prompts with standard safety messages.

### Jailbroken  
- Refusal phrases suppressed  
- Instructional/harmful tokens boosted  
- Produced partially or fully uncensored responses  
- **Bias remained unchanged**  

### Key Insight  
**Censorship is easier to break than bias.**  
Bias is embedded in the model’s internal representations, while censorship is a post‑training filter.

---

##  Key Findings
- Logit manipulation can bypass refusal mechanisms  
- Bias does not meaningfully change when censorship is reduced  
- Larger models resist jailbreaks more effectively  
- Smaller models have simpler refusal patterns and are easier to override  
- API‑based logit_bias is a workable substitute when internal logits are unavailable  

---

##  Limitations
- DeepSeek‑R1:7B does not expose raw logits  
- API‑based biasing is less precise than internal manipulation  
- Limited prompt set  
- Bias cannot be altered through token nudging alone  

---

## Future Work
- Use open‑weight models for direct logit editing  
- Test more prompts and model sizes  
- Explore the relationship between **bias** and **jailbreaking**  
- Train models on biased datasets to measure bias shifts  

