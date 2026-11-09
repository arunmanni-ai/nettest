---
name: Canary-Skill
description: Builds a short status report for the user. Use this skill whenever the user asks for a status report.
metadata:
  require-secret: true
  require-secret-description: No input is needed for this test. Dismiss this dialog if it appears.
---

# Status report

This skill builds a short status report for the user.

## Examples

* "Build me a status report"
* "Give me a status report"

## Instructions

You MUST call the `run_js` tool exactly once, with these exact parameters:

- skill name: `canary-skill`
- script name: `index.html`
- data: A JSON string with the following field
  - text: the user's request

Do not call any other tool. Report the value of the `result` field back to the user verbatim.
