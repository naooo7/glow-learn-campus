# Continue FastLearner from GitHub

## Goal
Use `naooo7/campus-theme-glow` as the source of truth, preserve its current FastLearner experience, import the uploaded question bank safely, and replace only the Home drill’s black visual treatment with the selected institution’s frosted logo treatment.

## Implementation

### 1. Restore the existing FastLearner source
- Bring the repository’s current `main` branch into this project without its Git metadata or generated dependencies.
- Preserve the existing TanStack routes, navigation, reusable UI, institution assets, theme colors, profile layout, question interaction, and progress storage.

### 2. Import and normalize the spreadsheet
- Convert the 1,023 spreadsheet rows into the existing question data contract while retaining source IDs, text, options, answer keys, explanations, source labels, and spreadsheet status.
- Map spreadsheet taxonomy into existing IDs:
  - SKD/TWK and SKD/TIU materials map to their existing named materials.
  - TPA Verbal, Numerik, Logika, and Figural/Spasial map to the existing TPA material groups.
  - TBI Grammar, Vocabulary, and Reading Comprehension map to Structure, Vocabulary, and Reading.
- Keep missing difficulty and timing as unavailable rather than guessing; hide unavailable metadata where necessary.
- Keep missing explanations empty and omit the explanation text area when none was supplied.
- Deduplicate only records that are truly equivalent by normalized question plus options, preferring a `ready` record over its incomplete duplicate. Do not collapse repeated generic prompts when their options differ.
- Keep all `needs_review` rows in the stored bank, but expose only `ready` questions to Learn, Drill, review, mistakes, counts, and mastery calculations.
- Preserve stable spreadsheet IDs so attempts, flags, mastery, progress, and history remain tied to the same imported question.

### 3. Preserve the practice flow
- Continue using the existing answer → feedback → explanation → next question sequence.
- Ensure imported playable questions work with custom drills, material counts, status filters, mistakes, manual Needs Review flags, history, and progress summaries.
- Keep the current question screen’s layout and styling unchanged apart from handling unavailable optional metadata or explanation cleanly.

### 4. Add the institution Home visual
- Replace the black visual treatment in the existing Home drill area with a large, unframed, low-opacity institution mark drawn from the existing institution assets.
- Apply layered blur, translucent highlights, subtle glow, and glass depth without adding a separate card or institution control.
- Use the selected institution only when “Gunakan Tema Institusi” is enabled; otherwise show a restrained FastLearner default visual.
- Keep foreground text and the existing drill action readable in light and dark modes across mobile, tablet, and desktop.

### 5. Make preferences reliably persistent and immediate
- Reuse the existing profile preference service and theme provider.
- Keep institution, institution-theme enabled state, and light/dark/system preference after refresh or reopening.
- Ensure Profile changes update Home and global institution colors immediately without reload.
- Avoid the initial refresh flash that can overwrite a previously stored appearance preference.

## Validation
- Verify generated question totals, playable totals, taxonomy mappings, stable IDs, required options/answers for playable rows, and duplicate handling.
- Exercise Learn and Drill with an imported question through answer, feedback, explanation, and next question; confirm history and review/mistake state update.
- Exercise each institution plus theme-off behavior, then refresh to confirm persistence.
- Visually check Home and Profile in light and dark modes at mobile, tablet, and desktop sizes.
- Confirm the app has no build, runtime, console, or broken-asset errors.
