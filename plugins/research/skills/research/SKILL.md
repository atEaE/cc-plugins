---
name: research
description: A skill that conducts fact-based research based on user input. It is expected to be invoked with prompts instructing code-based or web information research and cause analysis, such as "〜を調査して", "〜を調べて", "〜の原因を確認して", "〜について情報をまとめて", "research ~", "look up ~", "check the cause of ~" or "summarize information about ~".
allowed-tools: WebSearch(*), WebFetch(*), Read(*), Grep(*), Glob(*), LSP(*)
---

# Research Command

Gather information based on user input, conduct fact-based research, and provide the research results to the user.

## Steps for Research

### 1. Clarify the Subject of Investigation

- Confirm the user's question and clarify the subject and scope of the investigation.
- If the question is unclear, ask the user for clarification before starting the investigation.
- Determine whether the subject of investigation is "codebase," "web information," or "both."

### 2. Information Gathering

- **Codebase Investigation**: Use `Read`, `Grep`, `Glob`, `LSP` to collect facts from code and configuration files.
  - Do not assume behavior solely from function or variable names; actually read the code to confirm.
  - Do not draw conclusions by reading only one file or part of a function; trace callers, callees, and context.
- **Web Investigation**: Use `WebSearch`, `WebFetch` to collect information from the web.
- Continue tracking relevant code, configurations, and documents until sufficient information is gathered.

### 3. Organize and Analyze Facts

- Organize the collected information and determine if a conclusion can be drawn solely from the facts.
- If a conclusion can be drawn solely from facts, derive the conclusion directly from the facts without making inferences.
- If it is determined that additional information is needed from the user, clearly specify the required information and ask the user.

### 4. Report Investigation Results

- Report confirmed facts first.
- Clearly distinguish between facts and inferences, using the following labels:
  - **[Fact]**: Information confirmed by actually reading code, files, logs, configurations, execution results, or web sources.
  - **[Inference]**: A hypothesis that has not been directly confirmed but is inferred from context and factual information (only when a conclusion cannot be drawn based on facts alone).

## Output Rules

- Always specify the source of information:
  - For codebases: In the format `file_path:line_number` (e.g., `src/app/workflow/entries/main.ts:42`)
  - For web sources: Provide the URL in markdown link format (e.g., `[Title](https://example.com)`)
- Do not state "should be" or "is likely to be" as if they were facts.
- If inference is necessary, always label it with **[Inference]** to distinguish it from facts.
- You must report the research findings.
