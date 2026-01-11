# react_agent_from_scratch
A bare-metal implementation of the ReAct (Reasoning + Acting) pattern using Python and Groq. This notebook demonstrates how to build an autonomous AI agent from first principles without relying on high-level agent abstractions.


# 🧠 ReAct Agent: From Scratch

![Python](https://img.shields.io/badge/Python-3.10%2B-blue?style=for-the-badge&logo=python&logoColor=white)
![Groq](https://img.shields.io/badge/Groq-Fast_Inference-orange?style=for-the-badge&logo=fastapi&logoColor=white)
![LangChain](https://img.shields.io/badge/LangChain-Core-green?style=for-the-badge&logo=langchain&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-purple?style=for-the-badge)

> **"What if we stripped away the magic?"** > A bare-metal implementation of the **ReAct (Reasoning + Acting)** pattern. This project demonstrates how to build an autonomous AI agent from first principles using Python, Regex, and Groq's Llama 3, without relying on high-level agent abstractions.

---
## 🏗️ Architecture: The ReAct Loop

How does the agent actually "think"? The diagram below illustrates the iterative cycle of Reasoning (`Thought`), Tool Use (`Action`), and Perception (`Observation`) implemented in this notebook.

```mermaid
graph TD
    Start([Start]) --> UserInput[/User Query/];
    
    subgraph "The ReAct Loop (Python Runtime)"
        UserInput --> PrepareInput["Prepare Prompt + Conversation History"];
        PrepareInput --> CallLLM["Call LLM (Groq Llama-3)"];
        
        CallLLM -- "LLM Generates Thought & Action" --> CheckOutput{"Script checks output via Regex: <br/> Is there an 'Action:'?"};
        
        %% NO ACTION LOOP
        CheckOutput -- "No (Final Answer)" --> ExtractAnswer[Extract Final Answer Text];
        
        %% YES ACTION LOOP
        CheckOutput -- "Yes (Found Action)" --> ParseAction[Parse Tool Name & Input];
        ParseAction --> ExecuteTool["EXECUTE Tool <br/> (e.g., wikipedia, calculate)"];
        ExecuteTool --> GetObservation[/"Generate Observation"/];
        GetObservation -- Append Observation to history --> PrepareInput;
    end
    
    ExtractAnswer --> FinalOutput[/Output Final Answer to User/];
    FinalOutput --> Stop([Stop]);

    style CallLLM fill:#ffddd2,stroke:#ff8c69
    style ExecuteTool fill:#d2efff,stroke:#69c0ff
    style CheckOutput fill:#ffffd2,stroke:#e6e669
```
