# react_agent_from_scratch
A bare-metal implementation of the ReAct (Reasoning + Acting) pattern using Python and Groq. This notebook demonstrates how to build an autonomous AI agent from first principles without relying on high-level agent abstractions.


graph TD
    Start([Start]) --> UserInput[/User Query/];
    
    subgraph "The ReAct Loop (Python Runtime)"
        UserInput --> PrepareInput[Prepare Prompt + History];
        PrepareInput --> CallLLM[Call LLM (Groq)];
        
        CallLLM -- LLM Generates "Thought" & "Action" --> CheckOutput{Script checks output via Regex: <br/> Is there an 'Action:'?};
        
        %% NO ACTION LOOP
        CheckOutput -- "No (Final Answer)" --> ExtractAnswer[Extract Final Answer Text];
        
        %% YES ACTION LOOP
        CheckOutput -- "Yes (Found Action)" --> ParseAction[Parse Tool Name & Input];
        ParseAction --> ExecuteTool[EXECUTE Tool <br/> (e.g., wikipedia() or calculate())];
        ExecuteTool --> GetObservation[/Generate "Observation"/];
        GetObservation -- Append Observation to history --> PrepareInput;
    end
    
    ExtractAnswer --> FinalOutput[/Output Final Answer to User/];
    FinalOutput --> Stop([Stop]);

    style CallLLM fill:#ffddd2,stroke:#ff8c69
    style ExecuteTool fill:#d2efff,stroke:#69c0ff
    style CheckOutput fill:#ffffd2,stroke:#e6e669
