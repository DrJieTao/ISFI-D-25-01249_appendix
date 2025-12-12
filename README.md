# Supplementary Material for Manuscript ISFI-D-25-01249
> This repository contains the code artifacts, prompt definitions, and experimental logs referenced in the paper "From Explainability to Actionability: A Tiered Adaptable Multi-Agent Framework with Agent Reasoning Tools for Collaborative Failure Recovery".

## Mapping to Appendices

| Paper Section | File Path | Description |
| :--- | :--- | :--- |
| **Appendix A.1** | `prompts/ate_prompt.txt` | Tier 1 **Decision Agent** Prompt (Standard & Revise Mode, include all ART specifications). |
| **Appendix A.2** | `prompts/review_prompt.txt` | Tier 1 **Review Agent** Prompt (include all ART specifications). |
| **Appendix A.3** | `prompts/review_prompt_t2.txt` | Tier 2 Evolved Review Agent Prompts (Comprehensive Verifier). |
| **Appendix B** | `ai_panel_eval/` | system and persona based prompts for the **AI Committee**, in Markdown format. |
| **Appendix C** | `configs/escalation.py` | Thresholds for escalation triggers. |
| **Appendix D** | `configs/memory_bus_schema.md` | Database schema for the relational SQLite database, as the implemenation of the **Shared Memory Bus**. |
 
