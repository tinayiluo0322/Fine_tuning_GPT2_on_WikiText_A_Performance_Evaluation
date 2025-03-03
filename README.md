# **Fine-Tuning GPT-2 on WikiText-2 - A Performance Evaluation**  

### *Luopeiwen Yi*  
---

## **1. Introduction**  

GPT-2 is a **transformer-based large language model** developed by OpenAI for text generation tasks. While the pre-trained GPT-2 model has been trained on a diverse corpus, **fine-tuning** it on a domain-specific dataset like **WikiText** allows it to specialize in a particular style or topic.

This experiment evaluates **three different versions of GPT-2** to compare their **text generation ability, perplexity scores, and computational efficiency**:  
1. **Pre-trained GPT-2** *(Original OpenAI model, without fine-tuning)*  
2. **Fully Fine-tuned GPT-2** *(Fine-tuned on WikiText-2 with all model parameters updated)*  
3. **LoRA Fine-tuned GPT-2** *(Fine-tuned using Low-Rank Adaptation (LoRA) for efficiency)*  

The goal is to evaluate how fine-tuning affects GPT-2’s text generation ability, perplexity scores, and computational efficiency.

We also analyze whether fine-tuning improves or degrades text relevance, and whether a lower perplexity score truly correlates with better text quality.

---

## **2. Experiment Setup**  

### **2.1 Dataset: WikiText-2**  
- **Training Set:** Used to train the model  
- **Validation Set:** Used for model evaluation during fine-tuning  
- **Test Set:** Used to assess model generalization ability  

### **2.2 Fine-Tuning Methods**  
We tested **two fine-tuning approaches**:  
- **Full Fine-Tuning:** Updates **all** model parameters, allowing for **maximum adaptation** to the dataset.  
- **LoRA Fine-Tuning:** Modifies **only a subset of parameters**, making the process **faster and memory-efficient** while retaining pre-trained knowledge.  

### **2.3 Evaluation Metrics**  
- **Text Generation Quality:** Subjective analysis of output relevance and coherence  
- **Perplexity:** Measures how well the model predicts text (**lower is better**)  
- **Training Time & Memory Efficiency:** Comparison of computational costs  

### **2.4 Experiment Setup & Hardware**  
- **Base Model:** `"gpt2"` (OpenAI’s pre-trained GPT-2 model)  
- **Fine-Tuning:** Hugging Face `Trainer` API  
- **Hardware:** Google Colab with L4 GPU  

---

## **3. Results**  

### **3.1 Comparison Table of Fine-Tuning Methods**  

We prompted each model with:
"GPT-2 is a language model based on transformers developed by OpenAI"

| Model | **Generated Text** (First 100 Tokens) | **Perplexity (Lower is Better)** | **Training Time** | **Memory Usage** |
|--------|--------------------------------------|--------------------------------|----------------|----------------|
| **Pre-trained GPT-2** | GPT-2 is a language model based on transformers developed by OpenAI, which allows for rapid and efficient transformation of data sets. OpenAI's transformers are based on the Open Data Framework, which provides a powerful tool to build data sets. In this article, I will cover the data structures, features, and behavior of OpenAI's transformers and how they are used. | **49.60 (highest)** | No training | None |
| **Fully Fine-tuned GPT-2** | GPT-2 is a language model based on transformers developed by OpenAI, which is designed to classify vertebrate species using the functional traits of the species. The model has been used in many vertebrate taxonomical analyses, including vertebrate taxonomy and the identification of taxonomic units such as phyla. | **32.77 (lowest perplexity)** | **Longest (13:00 min with L4 GPU)** | **High (all layers trained)** |
| **LoRA Fine-tuned GPT-2** | GPT-2 is a language model based on transformers developed by OpenAI, a global company dedicated to creating and developing intelligent robots and artificial intelligence. OpenAI has developed a number of different models and systems that interact with a variety of different sensors and processors. | **36.99 (improved but not as good as full fine-tuning)** | **Faster Than Fully Finetuned (09:55 min with L4 GPU)** | **Low (only LoRA adapters trained)** |

---

## **4. Conclusion & Discussion**  

### **4.1 Pre-trained GPT-2: General but Inconsistent**  
- **Text:** Produces general, somewhat repetitive AI-related text.  
- **Perplexity:** **49.60 (highest)** → Indicates **lower confidence and more variance** in predictions.  
- **Interpretation:**  
  - **Maintains broad generalization ability** but lacks topic specificity.  
  - Higher perplexity suggests **more diverse but less structured output**.  

---

### **4.2 Fully Fine-Tuned GPT-2: Strong Adaptation but Topic Drift**  
- **Text:** Generates **highly confident but off-topic** text about vertebrate taxonomy.  
- **Perplexity:** **32.77 (lowest perplexity)** → Model is **very confident** in predictions, but text loses **AI focus**.  
- **Training Time:** **13:00 min (Longest)**  
- **Interpretation:**  
  - **Severe overfitting** → The model memorizes WikiText-2 instead of retaining general AI knowledge.  
  - **Low perplexity does not guarantee better text quality**—it **loses topic relevance** despite confident predictions.  
  - **High computational cost makes this approach inefficient** for many practical use cases.  

---

### **4.3 LoRA Fine-Tuned GPT-2: Best Balance Between Adaptation & Generalization**  
- **Text:** Generates **AI-related content**, discussing OpenAI, AI, and robotics.  
- **Perplexity:** **36.99 (higher than full fine-tuning but much lower than pre-trained GPT-2)**  
- **Training Time:** **09:55 min (Faster than full fine-tuning)**  
- **Interpretation:**  
  - **Preserves pre-trained knowledge better than full fine-tuning.**  
  - **Balances adaptation and efficiency**, improving text relevance without completely overwriting GPT-2’s original knowledge.  
  - **LoRA fine-tuning achieves strong improvements while significantly reducing computational cost.**  

---

### **4.4 Trade-offs Between the Models**  

| Model | **Key Strength** | **Key Weakness** |
|--------|----------------------|---------------------------|
| **Pre-trained GPT-2** | **Generalization ability, retains diverse knowledge** | High perplexity, lacks domain-specific adaptation |
| **Fully Fine-Tuned GPT-2** | **Lowest perplexity, strongly adapted to WikiText-2** | **Severe topic drift (overfits to fine-tuning data)** |
| **LoRA Fine-Tuned GPT-2** | **Best balance between adaptation and generalization** | Slightly higher perplexity than full fine-tuning |

---

### **4.5 The Relationship Between Perplexity and Text Quality**  
- **Fully fine-tuned GPT-2 has the lowest perplexity (32.77), but the generated text is irrelevant.**  
- **LoRA fine-tuned GPT-2 has slightly higher perplexity (36.99) but produces more relevant and meaningful text.**  
- **Pre-trained GPT-2 has the highest perplexity (49.60), leading to more variance in predictions.**  

**Key Takeaways:**  
- **Fine-tuning must balance perplexity reduction and knowledge retention.**  
- **Overfitting (as seen in fully fine-tuned GPT-2) leads to a loss of general knowledge.**  

---

### **5. Conclusion: Which Model is Best?**  

| Model | **Best Use Case** |
|--------|------------------|
| **Pre-trained GPT-2** | General-purpose text generation with broad knowledge retention |
| **Fully Fine-Tuned GPT-2** | Domain-specific adaptation when topic relevance is not a concern |
| **LoRA Fine-Tuned GPT-2** | Best trade-off between efficiency, adaptation, and topic relevance |


### **6. Final Thought: Fine-Tuning Must Be Done Carefully**  
**Fine-tuning is not always beneficial**—without proper dataset selection, fine-tuning can cause **knowledge loss and topic drift rather than meaningful improvements**.  

**LoRA fine-tuning proves to be the most effective approach**, offering a balance between **efficient learning, topic relevance, and knowledge retention** while avoiding the computational cost of full fine-tuning.  
