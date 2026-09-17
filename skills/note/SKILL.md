---
name: note
description: 'Use when the user prefixes mid-task feedback or additional thinking with "note:" or explicitly invokes the note skill.'
---

# Note

Treat the note as input to the active task, not a replacement task or a request to end the turn.

1. Preserve the active objective, remaining work, and completion criteria. Fold actionable feedback into that work at the next sensible point; do not abandon the current step just to answer the note.
2. Preserve the user's intent: apply clear requests, but treat tentative thinking as context rather than a decided change. Keep unrelated future ideas out of the current scope.
3. Acknowledge only when useful, in one short commentary message, then keep working. Otherwise incorporate the note silently. An acknowledgment must not be a final response.
4. Ask for clarification only when the ambiguity blocks progress; continue independent work while awaiting an answer.
5. Finish the original task and its required checks before the final response. Receiving or addressing a note is never, by itself, a reason to stop—even when the note is easy to resolve. Honor an explicit instruction to stop, pause, or replace the task.

**User:** `note: that border should be thicker`

**Good:** Brief commentary: “I’ll fold that in.” Then continue implementation, adjust the border, and finish the outstanding checks.

**Bad:** Final response: “Sure, I’ve made the border thicker.” Leave the original task unfinished.
