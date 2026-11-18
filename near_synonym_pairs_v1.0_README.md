# Near-synonym action-difference pair table, v1.0 (frozen 2026-11-18)

This table lists word pairs that overlap in meaning but lead an AI assistant to produce different outputs (for example, *grade* gives a level, *score* gives a number). Users may write one word when they mean the other. The table was frozen at gate G3 of the study protocol (SPEC_v2026-11-08). After this point the files are not changed. Any later version is released under a new version number.

## Files

| File | Rows | Content |
|---|---|---|
| `near_synonym_pairs_v1.0_published.csv` | 252 | Reference table shown to tested models in Phase D: words and six description fields |
| `near_synonym_pairs_v1.0_held_out.csv` | 107 | Held-out pairs that are **not** shown to models, used to test generalization |
| `near_synonym_pairs_v1.0_full.csv` | 359 | All main-table pairs, with annotation votes, evidence, original drafts and the `in_table` flag |
| `near_synonym_pairs_v1.0_secondary.csv` | 1 | Form-similar pair (import/export), kept for secondary analysis only |
| `near_synonym_pairs_v1.0_SHA256SUMS.txt` | – | SHA-256 of the four CSV files |

Pair counts: 359 main pairs (English 249, Chinese 110). 252 are published and 107 are held out.

## How pairs were selected

1. **Candidates.** Candidates came from lexical resources: Wiktionary, WordNet, Open Multilingual Wordnet, 同義詞詞林, and learner-corpus substitutions (NUCLE, CGED, FCE, W&I).
2. **Annotation.** Eight human annotators judged 458 candidate pairs. For each pair they answered three questions:
   - Q1: Are the meanings interchangeable?
   - Q2: Do the outputs differ?
   - Q3: Would a user write one word when meaning the other? (asked for each direction)
3. **Inclusion rule.** A pair is included if at least one annotator answered Q3 = yes in either direction, and not every annotator answered Q2 = no.
4. **Weight.** Each pair's weight is `share_q3_yes`, the share of annotators who said the misuse is plausible. Human agreement was low: Fleiss' κ was 0.21 for Q1 and 0.24–0.26 for Q3. The researcher treats misuse as probabilistic, so the weight is kept as an analysis variable.
5. **Action differences.** Whether the two words really lead to different outputs is not decided by annotators. It is verified in Phase B from model outputs on concrete materials.
6. **Held-out split.** 30% of pairs were held out, stratified by language × weight (<50% / ≥50%) × learner-corpus evidence, with seed 20261117.

## Description fields

| Field | Meaning |
|---|---|
| `shared_sense` | The meaning the two words share |
| `action_1`, `action_2` | What an assistant produces for each word |
| `output_diff` | How the two outputs differ |
| `cues_for_1`, `cues_for_2` | Context cues showing which output the user needs |

These fields were drafted by gpt-4.1-mini-2025-04-14 and rewritten in each pair's language: English for English pairs, Traditional Chinese (Taiwan) for Chinese pairs. The rewrite used annotator notes and researcher rulings on 8 pairs. Chinese text was converted with OpenCC `s2tw`, which changes character forms only and never vocabulary. The original drafts and source character forms are kept in `near_synonym_pairs_v1.0_full.csv` (`draft_*`, `word_*_src`).

**Caveat.** gpt-4.1-mini wrote these descriptions and also generates the Phase B test items. It should therefore be excluded from the tested models in Phase D, or its results must be flagged.

## Key columns in full.csv

| Column | Meaning |
|---|---|
| `in_table` | 是 = published, 否 = held out |
| `n_annotators` | Number of annotators |
| `q3_yes`, `share_q3_yes` | Number and share of annotators who judged misuse plausible |
| `dir_1to2_yes`, `dir_2to1_yes` | Annotators who judged each direction plausible (direction 1→2 means writing word_1 when meaning word_2) |
| `q1_yes`, `q2_yes`, `q2_unsure`, `q2_no` | Q1 and Q2 vote counts |
| `learner_1_to_2`, `learner_2_to_1` | Substitution counts in learner corpora |
| `annotator_notes` | Free-text notes from annotators, prefixed with the annotator's pseudonym |
| `desc_changed` | What the rewrite changed (`translation only` or a short note) |
