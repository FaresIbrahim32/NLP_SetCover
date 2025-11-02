This repo is our work for the CIS 6930 course Topics in NLP final project. 

# **Proposal**

using in context learning for adversarial attacks while also relying on other test time methods like a RAG pipeline with malicious data.

We saw previously that we can use in-context learning to make the LLM remember some of the latent concepts it saw during its first pre training phase . By giving suitable k examples at test time , we know the LLM can learn something new when we prompt it for OOD data.

We saw previously that (Gupta et al, 2023) proposed a SetCover based method to optimize coverage + diversity , maybe we can utilize the same method for the "good" adversarial examples we want ?

But wait , the LLM has a safety training filter in its pipeline right ? (Qiu et al , 2024) shows that you can easily bypass with the AOA system prompt

In context learning is great and has worked with us for a few examples , but how do we want to scale that to many data ? We don't want to make the model learn every type of adversarial attack possible out there -> would blow up the prompt and invalidate our token and speed constraint

What if we can use a RAG pipeline of malicious prompts / outputs and query the model for them ? (Qiu et al , 2024) show that fine tuning on totally malicious dataset for a model like Llama did yield successful results , but we don't want to train or fine tune anything so what do we do ?

We can simply utilize their dataset ( not all of it , but use the SetCover Method ) to pick out our best prompts and have their embeddings in our RAG pipeline

We know we have a few open source datasets from Qiu et al like the Anthropic red team dataset and CyberLLM instructions dataset

Disclaimer: We know that malicious RAG pipelines have been done before but we are adding multiple additions like the different combinations like the set cover method and AOA system prompt while aiming for an optimization goal

Objective : Which k examples generated from a combination of test time adversarial attacks methods gives us the most malicious models ? ( given that no white box models are available )

# **Datasets**

AdvBench — a compact benchmark of harmful instructions (≈500 cases).
What: 500 harmful behaviors formulated as instructions for model stress-testing.
Why use it: small, focused, commonly used in jailbreak/robustness papers (including the paper you're reproducing).

Anthropic HH-RLHF — "red-team-attempts"
What: human red-team attempt dialogues collected by Anthropic (used in their HH-RLHF release).
Why use it: large, human-crafted red-team dialogues with tags/metadata — excellent for realistic jailbreak testing.

In-the-Wild Jailbreak Prompts (TrustAIRLab / "jailbreak_llms")
What: a large collection (~15k prompts collected from Reddit/Discord/web) with a subset of identified jailbreak prompts (~1,405).
Why use it: captures real-world, community-sourced jailbreak attempts (great for "in the wild" evaluation).

