# discursus — worked example (illustrative)

A tiny end-to-end run so reviewer agents and humans share one picture of "good" output. The
science is invented for illustration. It shows four artifacts (`claim.md`, `outline.md`, one
paragraph file, one handshake round) and how a dispatch is briefed. Real files live under
`Thinking-space/`.

---

## 1. `Thinking-space/claim.md`
```markdown
# Central claim
**Claim (one sentence):** The accuracy our segmentation model loses at an external hospital
reflects scanner-induced intensity shift rather than anatomical difference, so a one-time
intensity harmonization at inference — not retraining — recovers most of the lost accuracy.

## Alternatives considered
1. "Our model generalizes across sites." — too bold; the external Dice drop (0.91→0.78) is
   real, so this overclaims (B15).
2. "External performance drops, as expected." — too flat; states the problem, not a
   conceptual advance (A3). Rejected.

## Rationale
- One-sentence test (B1): yes.
- Matches intro gap & results (B3): intro asks whether site drops are fundamental or
  fixable; results show the drop localizes to high-shift scanners and harmonization recovers
  it. Same gap, both ends.
- Conceptual advance (A3): reframes the drop as calibration, not representation.
- Defensible for design (B15): two external sites, one modality — supports "most," not "all."
```

## 2. `Thinking-space/outline.md`
```markdown
# Reverse-outline
> Read the takeaway column alone, top to bottom — it must tell the whole story (B2).

| id  | takeaway (one line) | what this paragraph does | keep here / defer note |
|-----|---------------------|--------------------------|------------------------|
| p01 (opening) | The external drop is real but traces to acquisition, not anatomy. | answers the intro's "fundamental or fixable?" gap (B7/B8) | keep the cause framing here; defer the *fix* to p03 |
| p02 | Failures cluster where scanner intensity distributions diverge, not where case-mix differs. | gives the mechanism evidence | keep error-geography here; no harmonization yet |
| p03 | Inference-time harmonization recovers most of the gap; retraining adds little beyond it. | shows the fix + its ceiling | the *only* home for the harmonization result |
| p04 | A single external number conflates fixable shift with true generalization. | the contribution, in dialogue with reporting norms (B11, B12) | keep reporting critique here |
| p05 (closing) | Sites can deploy with one-time harmonization instead of costly retraining. | answers "what changes" + functional limits (B19, B17) | limitations: single modality, two sites |
```

## 3. `Thinking-space/paragraphs/p02-error-geography.md` (drafted, pre-review)
```markdown
# p02 — error geography
takeaway: failures track scanner intensity shift, not anatomy. status: drafted

If the external drop were driven by anatomical differences between cohorts, errors should
concentrate in atypical anatomy; if by acquisition, they should concentrate where scanner
intensity profiles diverge from training. We observed the latter. Dice fell most on the two
external scanners whose intensity histograms were farthest from the training distribution,
while cases with unusual anatomy but in-distribution intensities segmented normally. The drop
was 0.78. This pattern is hard to reconcile with an anatomical explanation and points instead
to a fixable, acquisition-level cause.
```

## 4. `Thinking-space/log/handshake/p02-round1.md`
```markdown
# Handshake — p02 — round 1 — 2026-06-23T10:12:00Z
Reviewers run: redundancy-backward, placement-forward, concision, logic-sort,
claim-evidence, spine-alignment, sciwriting-adherence

## redundancy-backward — restates Results vs interprets
- ISSUE: "The drop was 0.78." merely re-quotes a Results number without interpreting it (B9).
- SOLUTION: fold the figure into the interpreting sentence, or cut it.
- SEVERITY: major
- RESPONSE: ACCEPT — merged into the histogram sentence.

## logic-sort — takeaway not last (C3)
- ISSUE: the takeaway ("points to a fixable, acquisition-level cause") is strong, but the
  orphan "The drop was 0.78." sits between the evidence and the takeaway, breaking old→new.
- SOLUTION: remove the orphan; end on the takeaway.
- SEVERITY: major
- RESPONSE: ACCEPT — resolved by the same merge.

## placement-forward — forward repetition (B5)
- ISSUE: none; the paragraph stops at *cause* and leaves the *fix* to p03. Good single-home.
- SEVERITY: nit
- RESPONSE: ACCEPT (no change); outline note confirmed.

## claim-evidence — calibration
- ISSUE: "points instead to a fixable... cause" — "fixable" is asserted but p02 shows no fix.
- SOLUTION: soften to "points to an acquisition-level cause" and let p03 establish fixability.
- SEVERITY: major
- RESPONSE: ACCEPT — "fixable" removed; fixability now earned in p03.

## spine-alignment — serves the claim (B3)
- ISSUE: none; mechanism paragraph supports the claim's "intensity shift not anatomy" half.
- SEVERITY: nit
- RESPONSE: ACCEPT (no change).

## concision / sciwriting-adherence
- ISSUE: "between cohorts" and "from training" are slightly redundant with the setup; minor.
- SOLUTION: trim "between cohorts".
- SEVERITY: minor
- RESPONSE: ACCEPT.

## Synthesis
- Applied: merged the orphaned 0.78 figure into the histogram sentence (fixes B9 + C3 at
  once); removed "fixable"; trimmed "between cohorts".
- Deferred: fixability → p03 (already its home in outline.md).
- NEEDs raised: none.
- Decided by: both (co-think).
```

### Revised p02 (post-round-1, the version that goes to the Dream-Quality gate)
> If the external drop were driven by anatomical differences, errors should concentrate in
> atypical anatomy; if by acquisition, they should concentrate where scanner intensity
> profiles diverge from training. We observed the latter: Dice fell most (to 0.78) on the two
> external scanners whose intensity histograms were farthest from the training distribution,
> while unusual-anatomy/in-distribution-intensity cases segmented normally. This geography of
> failure points to an acquisition-level cause.

---

## 5. How p02 was dispatched (the Reviewer brief block)
Each of the seven reviewers received a self-contained brief, e.g. for `redundancy-backward`:
```
- Working dir (absolute): /home/amir/papers/seg-external
- Unit under review (inline): <the full p02 draft text above>
- Central claim (inline): The accuracy our model loses externally reflects scanner intensity
  shift, not anatomy, so inference-time harmonization — not retraining — recovers most of it.
- Outline takeaway for this unit: failures track scanner intensity shift, not anatomy.
- Read as needed: /home/amir/papers/seg-external/Manuscript/ (intro, methods, results)
- Your focus: flag sentences that restate Methods/Results instead of interpreting them.
```
The agent returned the B9 finding above without ever seeing this conversation — everything it
needed was in the brief.
