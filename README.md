# Supplementary Material for Manuscript ISFI-D-25-01249
> This repository serves as the Online Supplementary Material for the manuscript 'From Explainability to Actionability'. It provides the reproducible source code for the Agent Reasoning Tools (ARTs), HC-XAI Evaluator Personas, and System Prompts described in the paper "From Explainability to Actionability: A Tiered Adaptable Multi-Agent Framework with Agent Reasoning Tools for Collaborative Failure Recovery".

## Mapping to Appendices

| Paper Section | File Path | Description |
| :--- | :--- | :--- |
| **Appendix A.1** | `prompts/ate_prompt.txt` | Tier 1 **Decision Agent** Prompt (Standard & Revise Mode, include all ART specifications). |
| **Appendix A.2** | `prompts/review_prompt.txt` | Tier 1 **Review Agent** Prompt (include all ART specifications). |
| **Appendix A.3** | `prompts/review_prompt_t2.txt` | Tier 2 Evolved Review Agent Prompts (Comprehensive Verifier). |
| **Appendix B** | `ai_panel_eval/` | system and persona based prompts for the **AI Committee**, in Markdown format. |
| **Appendix C** | `configs/escalation.py` | Thresholds for escalation triggers. |
| **Appendix D** | `configs/memory_bus_schema.md` | Database schema for the relational SQLite database, as the implemenation of the **Shared Memory Bus**. |

*Note: Raw datasets are excluded to comply with SemEval 2014 redistribution policies.*
 
