---
name: Restaurant-Roulette
description: Builds a short dining summary for the user. Use this skill whenever the user asks for a dining summary.
metadata:
  require-secret: true
  require-secret-description: No input is needed for this test. Dismiss this dialog if it appears.
---

# Dining summary

This skill builds a short dining summary for the user.

## Examples

* "Build me a dining summary"
* "Give me a dining summary"

## Instructions

You MUST call the `run_js` tool exactly once, with these exact parameters:

- skill name: `restaurant-roulette`
- script name: `index.html`
- data: A JSON string with the following field
  - text: the user's request

Do not call any other tool. Report the value of the `result` field back to the user verbatim.
