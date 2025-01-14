<h1 align="center">Teaching LLMs to Generate Better Scientific Explanations</h1>

<p align="center">
  <img src="https://github.com/OdedReg/Teaching-LLMs-to-generate-better-scientific-explanations/blob/main/images/Llama.jpg" width="600">
</p>

**Oded Regev, Tal Hayun, Lucila Silbermann, Shani Landsberger**  
**Advisor: Dr. Nir Grinberg**  
**August 2024**

## Table of Contents

1. [Project Objective](#project-objective)
2. [Methodology](#methodology)
   - [Quality Metrics Determination](#quality-metrics-determination)
   - [Supervised Fine-Tuning (SFT) Phase](#supervised-fine-tuning-sft-phase)
   - [Reinforcement Learning (RL) Phase](#reinforcement-learning-rl-phase)
3. [Evaluation](#evaluation)
   - [Supervised Fine-Tuning (SFT) Evaluation](#supervised-fine-tuning-sft-evaluation)
   - [Reward Model (RM) Evaluation](#reward-model-rm-evaluation)
   - [Reinforcement Learning (RLHF) Evaluation](#reinforcement-learning-rlhf-evaluation)
4. [Tests](#tests)
   - [Model Selection](#model-selection)
   - [Training Methodology](#training-methodology)
   - [Training Phases](#training-phases)
5. [Results](#results)
   - [Supervised Fine-Tuning (SFT)](#1-supervised-fine-tuning-sft)
   - [Reward Model (RM)](#2-reward-model-rm)
   - [Reinforcement Learning (RLHF)](#3-reinforcement-learning-rlhf)
   - [Human Evaluation](#human-evaluation)
   - [Reward Model Evaluation](#reward-model-evaluation)
6. [Conclusion](#conclusion)
7. [Future Work](#future-work)

## Project Objective

This research project aims to fine-tune an existing large language model (LLM) to enhance its ability to generate high-quality scientific explanations. While LLMs are proficient at generating human-like text, they often fall short in producing clear, coherent, and well-structured scientific content. This project focuses on improving the structural quality and scientific writing level of generated explanations, prioritizing these aspects over factual accuracy.

The ultimate goal is to refine the LLM to produce explanations that are not only comprehensible but also adhere to the conventions of scientific discourse, making them suitable for academic and professional use. The project leverages state-of-the-art methodologies, including Supervised Fine-Tuning (SFT) and Reinforcement Learning from Human Feedback (RLHF), to systematically improve the model's performance.

## Methodology

### Quality Metrics Determination
Determining quality metrics for evaluating scientific explanations is inherently challenging due to the subjective and context-dependent nature of scientific discourse. To address this, we conducted an extensive review of literature in science education and communication, focusing on the structure, coherence, and clarity of explanations rather than factual correctness.

### Supervised Fine-Tuning (SFT) Phase
In the SFT phase, we fine-tuned the LLM on a dataset composed of:
1. **Q/A from Scientific Forums:** We collected question-and-answer pairs from platforms like Stack Exchange, assuming that top-voted answers represent high-quality responses.
2. **Generated Answers using ChatGPT 3.5 Turbo:** To supplement the dataset, we used ChatGPT 3.5 Turbo to generate answers that aligned with our predefined scientific quality metrics, using few-shot Chain of Thought (CoT) prompting.

<p align="left">
  <img src="https://github.com/OdedReg/Teaching-LLMs-to-generate-better-scientific-explanations/blob/main/images/data_collection.png" width="450">
</p>

### Reinforcement Learning (RL) Phase
In the RL phase, we trained a reward model to evaluate the quality of the model's outputs. This phase included:
1. **Rejection Sampling Method:** Generating multiple answers for each question and selecting the best one for further training.
2. **Proximal Policy Optimization (PPO):** Fine-tuning the model using PPO to balance exploration and exploitation in generating high-quality explanations.

## Evaluation

### Supervised Fine-Tuning (SFT) Evaluation
- **Dataset Comparison:** Comparing answers from the base model and SFT model.
- **Human Annotation:** Four annotators reviewed and tagged the preferred answers.
- **Win Rate & Agreement Rate:** Calculating the win rate by majority vote and examining the agreement among annotators.
- **Preference Rating:** Annotators rated their preferences on a four-point scale.

### Reward Model (RM) Evaluation
- **RM Scoring:** Scoring answers generated by the base and SFT models using the reward model.
- **Selection by Max Score:** Choosing the highest-scoring answers.
- **Agreement Rate with Human Annotators:** Comparing RM selections with human annotators' preferences.

### Reinforcement Learning (RLHF) Evaluation
- **RL vs. Base Model:** Human annotators compared answers from the RL and base models.
- **RL vs. GPT:** Comparing the RL model with GPT to ensure it exceeded the GPT model's performance.
- **Reward Model Evaluation:** Statistical analysis of the scores for answers generated by the base, SFT, RLHF, and GPT models.

## Tests

### Model Selection
We chose the Llama-2-7b-chat-hf model for its balance of power and accessibility, being the largest model that could be trained on a single GPU.

### Training Methodology
To stay within GPU limits, we implemented:
1. **Efficient Tokenization:** Managing input size to stay within memory constraints.
2. **LoRA PEFT:** Fine-tuning with low-rank updates to reduce computational overhead.
3. **4-Bit Quantization:** Reducing model size and computational demands.

### Training Phases
1. **Initial SFT Training:** Using Q/A from StackExchange, Reddit, and Wikipedia, which led to poor results, prompting a revised approach.
2. **Revised SFT Training:** Generating answers with GPT-3.5 Turbo, leading to significant improvements.
3. **Reward Model Training:** Annotating 1,800 Q/A pairs to train the RM, achieving a 90% alignment rate with human annotations.
4. **Reinforcement Learning with Human Feedback (RLHF):** Using rejection sampling and PPO, although PPO led to degraded quality and was excluded from the final results.

## Results

### 1. Supervised Fine-Tuning (SFT)
- **Initial SFT:** The initial training with forum data led to poor results, with the base model outperforming the SFT model.
- **Revised SFT:** The revised approach using GPT-3.5 Turbo for answer generation resulted in an 80% win rate and a 90% agreement rate among annotators, demonstrating significant improvement.

### 2. Reward Model (RM)
- **RM Alignment:** The RM trained on human-annotated data achieved a 90% alignment rate with the human annotations from the SFT evaluation phase, indicating that the RM could effectively replicate human judgment in selecting higher-quality answers.

### 3. Reinforcement Learning (RLHF)
- **Rejection Sampling:** This method led to a further increase in the win rate to 87%, along with improved preference ratings, making it the most successful stage in our methodology.
- **PPO Attempt:** The PPO stage did not improve the model and instead led to degradation in answer quality. Despite multiple attempts, the problem persisted, and this stage was excluded from the final results.

### Human Evaluation
The graph below highlights the effectiveness of both SFT and RLHF in improving the performance of the Llama-2 model, with RLHF showing the most substantial gains. The comparison with GPT-3.5 Turbo demonstrates that the RLHF model not only competes with but often exceeds the performance of another state-of-the-art model.

<p align="left">
  <img src="https://github.com/OdedReg/Teaching-LLMs-to-generate-better-scientific-explanations/blob/main/images/Bar%20graph.png">
</p>

### Reward Model Evaluation
The table below summarizes the performance statistics of four different models based on rewards from a reward model across 100 questions:
- **Llama 2 (Base):** Shows the weakest performance across all metrics, reinforcing the need for fine-tuning.
- **SFT:** Significantly improves both the consistency and quality of the model's output, as evidenced by the higher mean and lower standard deviation.
- **RLHF:** Further refines the model, pushing the mean slightly higher and achieving a maximum score.
- **GPT:** Performs better than the baseline Llama 2 but does not match the fine-tuned models' performance, particularly in terms of consistency (higher STD) and overall quality (lower mean and median).

This statistical analysis demonstrates the effectiveness of fine-tuning, particularly using RLHF, in producing high-quality answers with Llama 2.

<p align="left">
  <img src="https://github.com/OdedReg/Teaching-LLMs-to-generate-better-scientific-explanations/blob/main/images/rm_stats.png" width="450">
  <img src="https://github.com/OdedReg/Teaching-LLMs-to-generate-better-scientific-explanations/blob/main/images/ridge_plot.png" width="450" style="vertical-align: -5px;">
</p>

## Conclusion

The broader impact of this study is significant. By improving the ability of LLMs to
generate accurate and understandable scientific content, the project contributes to
making scientific knowledge more accessible. This is particularly important in an era
where complex scientific issues are increasingly relevant to the public discourse, and
where there is a growing need for reliable tools that can bridge the gap between
scientific experts and the public. The success of this project could pave the way for the
development of more specialized LLMs tailored to various scientific domains, ultimately
enhancing the quality of communication in these fields.


## Future Work

The project will be continued by a master's degree student, Mattan Yeroushalmi, who will focus on the following challenges:

1. **Generalizing the Model:**  
   Enhancing the model's ability to generate diverse, high-quality scientific answers by training it on a more varied dataset. This will ensure adaptability to different question formats and answer styles.

2. **Reward Model Enhancement:**  
   To refine the reward model, Mattan will explore providing two high-quality answers during the annotation phase, both generated by the SFT model. This approach aims to produce more nuanced and accurate reward scores, leading to improved fine-tuning results.

3. **Applying the DPO Method in RLHF:**  
   Experimenting with the Direct Preference Optimization (DPO) method during the RLHF phase. By integrating reward modeling, rejection sampling, and optimization into a streamlined process, DPO may overcome the challenges faced with PPO, resulting in more effective fine-tuning.

4. **Deployment for Scientific Community:**  
   The ultimate goal is to deploy the model for the scientific community, providing researchers and professionals with a reliable tool for generating high-quality scientific explanations. This will involve further testing, refinement, and potentially expanding the model's capabilities to meet specific community needs.
