# Day 3 ~ Observation

*Flow for the session (aim for ~60 mins, observer can drop off any time)*

## Learning objective
- Trigger a GitHub Action workflow that runs tests on a pull request and controls whether the branch can merge.


## 5 mins – Set the scene

*Camera on, clean background, confident tone.*

Morning everyone. Yesterday you wrote and tested your code, and you aimed for 70 percent coverage.

**Quick recap of yesterday (tests and coverage).**

Today we’ll take that a step further by automating those tests so they run every time we make a change.

### Learning objective
By the end of this session, you’ll have a GitHub Action that runs your tests automatically and only allows a merge if they pass.

Before we automate this:

- <mark>what could go wrong if we merge code manually without any checks?</mark>
- Add one word in chat that describes the risk.

<mark>So instead of running tests by hand, we’ll let GitHub do it for us.</mark>

**Exceeds:** applied connections, learner centred (verbal), professional etiquette.

---

## 5 mins – Micro-demo

**You show the CI YAML file**

- point out it's **renamed** so GitHub Actions is ignoring it.

<mark>Why do you think the file name matters?</mark>

**Exceeds:** questioning techniques, assessment for learning.

### Show the YAML structure briefly

Scroll through and point out:

- on: → the trigger (push, pull_request)
- jobs: → what actually runs (tests, coverage)

GitHub only looks for workflows inside a hidden folder called `.github/workflows`

</mark>Before I rename and push it, quick prediction: when will it run?</mark>

Type in chat: SAVE, PR, or BOTH.

---

## 15 mins – Learner activity (core learning)

1. Rename the file correctly
2. Save to File (or push to GitHub if on the VM)
3. Open a PR to merge `day2` branch

You circulate and coach by asking *scaffold* questions:

- “What does the workflow check here?”
- “How will you know it ran?”

Support different paces:

- If a learner is stuck: prompt with clues
- If ahead: ask them to explain to someone else (peer learning)

What you say

- Let’s see this working in someone else’s repo.
- Can one of you share your screen and walk us through your Pull Request and the Actions run?
- Tell us what happened step by step.

Follow up with:

- What shows you that the merge is allowed?
- If the tests had failed, what would you expect to see?

**Exceeds:** learner centred, recognising differences, peer learning, personalised support.

---

## 10 mins – Stretch and challenge

You introduce two outcomes:

* Coverage Fail > 70% (✅ allows merge)
* Coverage Fail < 70% (❌ blocks merge)


If your workflow’s already green, great — you’re ready for a challenge.

Choose one of these:

- 1️⃣ Add a status badge,
- 2️⃣ Edit the trigger, or
- 3️⃣ Try breaking a test.

Let’s see what happens!


Ask *prediction questions* before running:

<mark>What do you think will happen when we try to merge?</mark>

Learners test both scenarios.

“Want to show off your passing pipeline? Add a build-status badge to your README.”

Stretch option for fast finishers:

- Add a build status badge to the README.

```markdown
![CI](https://github.com/<org>/<repo>/actions/workflows/ci.yml/badge.svg)
```

They can copy the snippet from their Actions → … → Create status badge menu

Explain briefly: That badge updates automatically each time your workflow runs.

*This reinforces the continuous part of CI/CD and gives an instant visual payoff.*

### Option 2 – Explore a different trigger

Ask:

“How could we make this only run on pull requests, not on every push? Try editing the on: block.”

They can comment out or change:

```yaml
on:
  pull_request:
    branches: [ main ]
```

*This shows how the workflow reacts to different events – an excellent deeper-thinking moment.*

### Option 3 – Add an intentional failure

“What happens if one test fails? Try breaking one line of code and running it again.”


**Exceeds:** stretch and challenge.

---

## 10 mins – Applied connection activity

Ask:

<mark>Where in your workplace would a CI check prevent mistakes?</mark>

Encourage examples:

- “Broken SQL deployed to prod”
- “Accidental deletion”
- “Wrong dataset version”

**Exceeds:** applied connections, learners sharing real world.

---

## 10 mins – Second demo (you break stuff)

You:

1. Create a new branch
2. Add deliberately failing code/tests
3. Open a PR
4. Let pipeline fail visually

Ask: <mark>What just saved us here?</mark>

Assessment for learning: 

<mark>Ask learners to explain the failure in their own words (chat or verbal)</mark>

---

## 5 mins – Quick exit ticket (proof of learning)

In chat:

<mark>Type one sentence: what does CI/CD stop from happening?</mark>

**Exceeds:** assessment for learning, learner centred.

---

## How this hits the “Exceeds Expectations” rubric

| Principle               | Evidence from your session                              |
| ----------------------- | ------------------------------------------------------- |
| Professional etiquette  | Camera on, confident demo, fluent use of GitHub Actions |
| Learner centred         | They rename CI file and trigger pipeline themselves     |
| Recognising differences | Scaffold prompts + extension badge task                 |
| Stretch and challenge   | Predict outcomes; badge task; failure demo              |
| Assessment for learning | Prediction, peer support, exit ticket                   |
| Applied connections     | Workplace discussion about preventing real failures     |

## Quick checklist

- Camera on
- Speak slowly and check for understanding often
- Use reactions and chat intentionally
- Ask learners to screen share once (massive evidence)
