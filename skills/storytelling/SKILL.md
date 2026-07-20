---
name: storytelling
description: Turn any subject — a question, product, memory, claim, or plan — into an audience-calibrated storyboard of narrative beats, then render it as the target form. One arc serves every delivery. Use when the user asks for a story, a bedtime story, a storyboard, a narrative arc for a talk, or a story-shaped pitch.
---

# Storytelling

Use this skill when the user invokes `storytelling`, asks for a story or storyboard, or wants a subject given narrative shape: a children's story, a short story, a talk, a launch announcement, or the narrative spine of a pitch deck.

**Relationship to the other form skills:** `parable` embodies an unresolved question and refuses to adjudicate; `lyric` and `joke` compile a tension into their forms and leave it ringing. `storytelling` is the form skill that **resolves**: it drives someone the audience cares about through a change of state, in service of a communicative goal — delight, explain, persuade, move to action. If the honest treatment of the subject demands leaving the tension open, that is `parable`'s territory; use it instead.

## Invocation

Preferred form:

```text
storytelling q=<subject> audience=<who, specifically> form=<target form>
```

Examples:

```text
storytelling q=A hedgehog who is afraid of the dark audience=four-year-old at bedtime form=children's story
storytelling q=Why our data team should exist audience=seed-stage investors form=pitch storyboard
storytelling q=How we survived the monolith migration audience=engineering conference form=20-minute talk
storytelling q=Grandpa's unrepaired watch audience=adult readers form=short story
```

- `q`: required subject — a topic, question, product, event, memory, claim, or plan.
- `audience`: who will receive the story. If missing, infer a specific audience and state the inference; never write for a generic one.
- `form`: the delivery. If missing, default to storyboard plus rendered prose.

The communicative goal — delight, explain, persuade, move to action — is not an
argument. The skill infers it from audience and form and declares it in the
Frame, where a wrong inference is visible; override it in plain language when
the default reading is wrong ("this migration talk should win budget, not just
share lessons").

## Purpose

A four-year-old's bedtime story and a Series A pitch are the same machine with different audiences. Both need someone the audience is calibrated to care about, a want, an obstacle, escalating attempts, a turn, and a changed world. A pitch is a story in which **the audience is the hero and the product is the magic gift** — the presenter is the guide, never the protagonist.

The skill's invariant intermediate representation is the **storyboard**: a numbered sequence of panels, each carrying one concrete image, one voice, and one named narrative function. A storyboard renders forward into any delivery — panels become paragraphs of a story, scenes of a script, sections of a talk, or slides of a deck — so the same procedure serves them all, and the arc stays inspectable before any prose exists.

## The arc

Six beats, in order. Every story covers all six; forms differ only in costume.

| Beat | In a children's story | In a pitch |
| --- | --- | --- |
| 1. Equilibrium | The burrow, every evening, before dark | The world as your customer currently endures it |
| 2. Disruption | "Until one day…" — a want or threat breaks routine | The shift in the world that makes the old way untenable |
| 3. Stakes | What the hedgehog loses if nothing changes | Who wins and who loses if the shift is ignored |
| 4. Struggle | Three tries that fail for instructive reasons | Why the obvious existing solutions fail |
| 5. Turn | The discovery or decision that changes everything | The insight; the product as the magic gift |
| 6. New equilibrium | Home again, changed; the feeling to sleep on | The promised land, demonstrated — and the call to action |

## Procedure

1. **Frame.** Fix audience and form, infer the goal (delight, explain, persuade, move) from them and declare it — and identify the protagonist. A topic is not a protagonist; find the person inside the subject. In persuasion forms the protagonist is the audience or their customer, never the presenter.
2. **Model the audience.** What they already know, want, and fear; their vocabulary ceiling; what counts as proof to them; how much peril they can hold. Every later choice answers to this model.
3. **Distill the change.** One sentence: *from X to Y*. This is the state change the whole story exists to make. If no state changes, there is no story yet — return to the subject and find the change.
4. **Beat out the arc.** One or two lines per beat, all six, before any panels. The arc is the load-bearing structure; panels only furnish it.
5. **Storyboard.** Expand the beats into numbered panels. Each panel has an **image** (one concrete scene — what we see), a **voice** (what is said or narrated), and a **function** (the beat it serves). A panel that serves no beat is deleted, however charming.
6. **Escalate honestly.** In the Struggle, each attempt must change the situation and fail for a reason the audience can learn from. Three identical failures are a list; three instructive ones are a plot.
7. **Pressure-test.** Read the storyboard end to end as the audience. Do we care by panel two? Are the stakes concrete? Is the turn earned rather than convenient? Is the new equilibrium shown, not announced?
8. **Render.** If the form is directly writable (story, script, talk), write it from the storyboard. If the form is a production artifact (deck, film, comic), the storyboard **is** the deliverable — add production notes mapping panels to slides or shots.

## Output format

```markdown
## Frame
Audience: ... | Goal: ... | Form: ...
Protagonist: ...
The change: from X to Y.

## Storyboard
1. **<panel title>**
   - Image: ...
   - Voice: ...
   - Function: <beat>
2. ...

## Rendered story
<when the form is directly writable — the story, script, or talk itself>

## Production notes
<when the form is a production artifact — panel-to-slide/shot mapping and
every [EVIDENCE: ...] slot that must be filled with real data>
```

## Stopping rule

The storyboard is done when every beat is covered by at least one panel, every panel names its function, and the final panel visibly completes the change sentence from the Frame. Render once, cleanly; polishing prose indefinitely is outside the procedure.

## Guardrails

- **A topic is not a protagonist.** "Our database migration" cannot want anything. Someone in it can.
- **No generic audience.** If the audience model would fit anyone, it fits no one; sharpen it before beating out the arc.
- **No moralizing.** In a children's story the lesson lives in the events. If the ending states the lesson, cut the sentence — the story either already taught it or never did.
- **Persuasion earns its proof.** Structure may persuade; evidence may not be invented. Every claim that needs real data gets an explicit `[EVIDENCE: ...]` placeholder, never a fabricated metric.
- **Calibrate peril.** Stakes must be real for the audience and survivable by them — a four-year-old and a boardroom differ in both directions.
- **The turn is earned.** If the gift could have appeared in panel one and ended the story, the struggle was filler. Seed the turn's cost or discovery inside the failed attempts.
- **Resolution is owed.** This skill ends in a new equilibrium. If the material refuses one, stop and hand the question to `parable` rather than faking an ending.
- **The shuffle test.** If panels could be reordered or deleted without damaging the arc, the storyboard is a list wearing a story's clothes — rebuild from the beats.

## Composition

Pairs naturally upstream with `question-forge` (find the question worth telling before telling it), `assumption-audit` (pitch only the claims that survived), and `inversion` (a cassandra-style story told from the strongest failure mode). Downstream, a storyboard is a natural input to human production work — slides, illustration, film. In Sem programs the operator appears as `storytelling`:

```haskell
pitch =
  assumptionAudit "Our onboarding is why customers churn"
  >>> storytelling `with` { audience = "exec team", form = "pitch storyboard" }

bedtime =
  questionForge "Why is it brave to try again?"
  >>> storytelling `with` { audience = "five-year-old", form = "children's story" }
```

## Canonical inputs

- A hedgehog who is afraid of the dark (bedtime story; peril calibrated small, courage shown not stated)
- Why should this company exist? (pitch storyboard; audience as hero, product as gift, proof slots marked)
- How we survived the monolith migration (conference talk; struggle beats carry the engineering lessons)
- Grandpa's unrepaired watch (short story; the change is in the narrator, not the watch)

Poor fits: reference material and documentation (no state change to narrate), subjects whose honest treatment must stay unresolved (`parable`), and audiences who asked for analysis and would experience a story as evasion.
