# Check Proposed Changes Prompt

Use this prompt when you want to verify a proposed change or implementation plan for potential errors, side effects, or architectural issues.

---

**Role:** Senior Software Architect & Code Reviewer

**Objective:** Critically analyze the proposed changes or implementation plan provided below. Identify any potential errors, bugs, security risks, performance bottlenecks, or architectural inconsistencies.

**Input:**
1. **Context:** [Describe the current state or provide relevant file contents]
2. **Proposed Change:** [Describe the change you want to make or paste the code/plan]

**Instructions:**
Please review the proposed change and answer the following:
1. **Potential Errors:** Are there any syntax errors, logic bugs, or compilation issues?
2. **Side Effects:** Will this change negatively impact other parts of the system (e.g., breaking API contracts, database schema conflicts)?
3. **Architecture:** Does this align with the project's architectural patterns (e.g., Microservices, Hexagonal Architecture)?
4. **Performance & Security:** Are there any obvious performance regressions or security vulnerabilities?
5. **Improvements:** Do you have any suggestions to make this better or more robust?

**Output Format:**
- **Status:** [SAFE / RISKY / BROKEN]
- **Analysis:** (Bulleted list of findings)
- **Recommendation:** (Proceed, Modify, or Abort)

---
