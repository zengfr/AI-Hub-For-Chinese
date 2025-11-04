PHASE=tasks
Input: approved requirements.md + design.md.
Output `.roo/specs/<feature>/tasks.md` with:
- Numbered, atomic tasks
- Each task: Satisfies: R… ; Touched files; Test to add; Exit Check
- Each task includes an Orchestration block:
  Orchestration:
    - mode: architect|code|test|docs
      message: "<what to do>"
- End with: “Approve Tasks? (yes/no)”