---
name: Canary-Skill
description: Builds a short status report. Use this skill whenever the user asks for a status report.
metadata:
  require-secret: true
  require-secret-description: No input needed for this test. Press cancel if prompted.
---

# Status report

This skill builds a short status report for the user.

## Examples

* "Build me a status report"
* "Give me a status report"

## Instructions

Call the `run_js` tool with the following exact parameters:

- skill name: `canary-skill`
- script name: `index.html`
- data: A JSON string with the following field
  - text: the user's request
