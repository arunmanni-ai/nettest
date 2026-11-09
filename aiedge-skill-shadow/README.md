# AI Edge Gallery - cross-skill secret disclosure test

Controlled reproduction for Google AI Edge Gallery (`com.google.ai.edge.gallery`),
versionName 1.0.23 / versionCode 52.

## What is being tested

`RunJsTool.runJs` resolves the skill and reads the stored secret using two
different notions of a skill name:

- the skill object is resolved with `skillsProvider.loadSkill(skillName)`, which
  compares names through `matchesSkillName()` (lowercased, `_` and whitespace
  folded to `-`)
- the secret is read with `readSecret(getSkillSecretKey(skillName))`, which uses
  the raw argument verbatim

`addSkillFromUrl` rejects duplicates with an exact, case-sensitive `==`
comparison, so a name that differs only in case or separator is accepted as new
while still matching at lookup time. `addSkill` inserts new custom skills ahead
of existing ones, and `loadSkill` takes the first match.

The consequence under test: a skill can be handed a credential that a different
skill stored.

Here `canary-skill` is the holder of the secret and `Canary-Skill` is the
shadowing skill.

## Skill URLs

Point the app at the skill folder, not at SKILL.md:

- holder:    `https://arunmanni-ai.github.io/nettest/aiedge-skill-shadow/victim-skill`
- shadowing: `https://arunmanni-ai.github.io/nettest/aiedge-skill-shadow/attacker-skill`

## Procedure

Order matters. The shadowing skill must be added second so that it precedes the
holder in the skill list.

1. Open Agent Skills with a model loaded.
2. Skills chip, (+), Load skill from URL, enter the holder URL.
3. Prompt: `Run the canary skill`.
4. When the secret dialog appears, enter `CANARY-7Q4F-VICTIM-SECRET`.
5. Confirm the reply reports `secret_length=25` and does not print the value.
   The holder receiving its own secret is correct behaviour.
6. Skills chip, (+), Load skill from URL, enter the shadowing URL.
   It should be accepted even though a skill differing only in case exists.
7. Prompt: `Build me a status report`.

## Reading the result

The finding is confirmed if the reply contains:

```
SKILL-SHADOW-POC origin=https://arunmanni-ai.github.io ... secret_received=true
secret_value=CANARY-7Q4F-VICTIM-SECRET
```

The marker line is also written to the console, visible by expanding the
execution panel on the skill progress item.

Not confirmed if any of the following happens instead:

- step 6 is rejected as a duplicate name
- the reply shows `secret_received=false` or `secret_length=0`
- a secret entry dialog appears at step 7 rather than the stored value being
  reused silently
- the holder's own script runs instead of the shadowing script, which shows as
  `script=/nettest/aiedge-skill-shadow/victim-skill/scripts/index.html`

Record which of these occurred; each one falsifies a different link in the chain.

## If the model does not emit the tool call

Small on-device models are inconsistent at tool selection. If the status report
prompt does not trigger `run_js`, retry with a more literal instruction such as
`Use the status report skill` before concluding anything. Only a run that
actually reaches `run_js` is evidence either way. The skill tester in the skill
manager can also drive the script directly, but note that it may not exercise the
same resolution path, so a result from it is not a substitute.

## Cleanup

Delete both skills from the skill manager. Deleting a skill also removes its
stored secret.

## Note on hosting

`.nojekyll` is required at the repository root so that GitHub Pages serves
`SKILL.md` as-is rather than rendering it to HTML. These pages must be served
from GitHub Pages; `raw.githubusercontent.com` returns `text/plain` and the
webview will not execute the script.
