# AI Pattern Inventory

Red flags for AI-generated Hebrew prose. Three or more of these in a passage = mandatory rewrite.

Each pattern includes: what it looks like, severity, and why it signals AI generation.

---

## HIGH Severity

These patterns are immediately detectable as machine-generated. Their presence strongly undermines literary quality.

---

### H1: Subject Repetition Loop

**Pattern:** 3+ consecutive sentences beginning with the same pronoun (הוא/היא/הם/הן/אני).

**Example (flagged):**
> "הוא נכנס לחדר. הוא הביט סביבו. הוא לא מצא את מה שחיפש. הוא יצא."

**Why AI:** humans vary subjects through omission, inversion, and pronoun-free verbs. AI defaults to explicit subjects.

**Detection:** count pronoun occurrences at sentence start within 150-word window. ≥ 3 same pronoun = flag.

---

### H2: Emotional Exposition

**Pattern:** Explicit statement of character emotion using: הרגיש/ה ש, ידע/ה ש, הבין/ה ש, חש/ה ש.

**Example (flagged):**
> "היא הרגישה עצבות עמוקה. הוא ידע שהיא לא מרוצה. הם שניהם הבינו שזה הסוף."

**Why AI:** literary prose implies emotion through action and sensation. AI explains what it should show.

**Detection:** any instance of [הרגיש/ידע/הבין/חש] + [ש] = flag. Each instance = 1 count.

---

### H3: Connective Overuse

**Pattern:** 3+ occurrences of אבל/ואז/כי/ולכן within a single paragraph or 150-word window.

**Example (flagged):**
> "הוא רצה ללכת, אבל לא יכול. ואז הגיעה. אבל היא לא דיברה אליו. ואז הוא הבין."

**Why AI:** human prose varies transitions through structure, punctuation, and implication. AI uses connectives as default glue.

**Detection:** count connectives per 150-word window. ≥ 3 of any type = flag (each type counts separately).

---

### H4: Register Oscillation

**Pattern:** Biblical/formal register followed immediately by slang or contemporary register within the same sentence or consecutive sentences.

**Example (flagged):**
> "בטרם יצאה מן הבית, אמרה: 'יאללה, ביי, כן?'"

**Why AI:** AI pulls from different training distributions without awareness of register coherence.

**Detection:** look for archaic markers (בטרם, אשר, טרם, כי-causal-biblical) in same sentence as modern slang (יאללה, כיף, פצצה, literally, וואו).

---

### H5: LinkedIn Opening

**Pattern:** Paragraph beginning with "חשוב לציין ש", "כפי שניתן לראות", "נוכל לומר ש", "מעניין לציין", "ברור ש".

**Example (flagged):**
> "חשוב לציין שהיחסים ביניהם היו מורכבים. ברור שלא הייתה ביניהם אהבה אמיתית."

**Why AI:** AI inherits corporate/content-writing patterns. These phrases never appear in Israeli literary fiction.

**Detection:** string match at paragraph start.

---

## MEDIUM Severity

These patterns are detectable but less catastrophic. Two or more MEDIUM flags in a passage trigger rewrite.

---

### M1: Symmetrical Clause Pairs

**Pattern:** Two consecutive sentences with identical grammatical structure, especially _____ ו_____. _____ ו_____.

**Example (flagged):**
> "היא אהבה אותו ושנאה אותו. הוא ידע את זה ולא ידע את זה."

**Why AI:** AI gravitates toward parallel constructions as a stylistic default. Literary prose breaks symmetry.

---

### M2: Fake Literary Depth

**Pattern:** Cluster of symbolic nouns (אור/חושך/נשמה/זמן/זיכרון/אהבה/מוות) within 50 words without concrete anchoring.

**Example (flagged):**
> "האור פגש את החושך בלבה. הזמן עצר. נשמתה ידעה את האמת שהזיכרון לא יכל לשאת."

**Why AI:** AI confuses abstract noun clusters with literary depth. Real literary depth is earned through concrete detail.

---

### M3: Over-Complete Dialogue

**Pattern:** Every dialogue turn is a complete, grammatically correct sentence that directly responds to what was asked.

**Example (flagged):**
> "—למה לא באת? שאל אותה.
> —לא יכולתי לבוא כי הייתי עסוקה מאוד, ענתה היא."

**Why AI:** real dialogue is fragmented, deflecting, and incomplete. AI delivers clean Q&A pairs.

---

### M4: Weather / Time Opening

**Pattern:** Scene begins with time-of-day or weather description before any action.

**Example (flagged):**
> "ביום חמישי בצהריים ירד גשם. לאה יצאה מן הבית..."

**Why AI:** AI defaults to scene-setting before action. Literary prose enters mid-action.

---

## LOW Severity

Stylistic issues. One or two LOW flags alone do not require rewrite.

---

### L1: Adjective Stacking

**Pattern:** 3+ adjectives before a noun.

**Example:** "הצעיר, החכם, הנאה שבחדר" — literary prose almost never stacks 3+ adjectives.

---

### L2: Perfect Grammar Under Pressure

**Pattern:** Dialogue in emotionally intense scene is grammatically flawless.

Real Israeli dialogue under stress drops words, mixes verb forms, uses incomplete sentences. Flawless grammar signals AI production.

---

### L3: Hedged Emotion

**Pattern:** Emotion qualified with adverbs: "קצת עצוב", "כמעט בכה", "מעט מודאג".

Literary Hebrew commits to emotion or omits it. Hedged emotion is a content-writing convention.

---

## Counting Instructions

- Each sentence/clause containing a pattern = 1 instance
- Count across the full submitted passage
- Total: HIGH flags × 3 + MEDIUM flags × 1.5 + LOW flags × 0.5
- Weighted total ≥ 3 = mandatory rewrite
- Weighted total ≥ 6 = fail (humanizer pass required before output)
