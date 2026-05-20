---
name: scene-composer
description: >
  Shape individual scenes for cinematic pacing, sensory grounding, and subtext.
  Apply when opening, writing, or transitioning between scenes. Enforce in-media-res
  entry, subtext-first structure, and tension through sentence rhythm.
  Triggers on: "scene", "open scene", "write scene", "scene transition", "scene break",
  "cinematic", "blocking", "scene opening", "scene ending", "scene structure", "pacing",
  "tension", "atmosphere", "sensory", "dialogue scene", "action scene", "interior scene",
  "new scene", "cut to", "enter scene", "establish scene", "scene description".
allowed-tools: Read
---

# Scene Composer

Rules for building individual scenes with cinematic structure, sensory grounding, and subtext.

**Step 1: Load reference file**

Read `${CLAUDE_SKILL_DIR}/references/scene-patterns.md` before composing any scene.

---

## 1. Entry: In Media Res

**Every scene begins in the middle of something.**

The first sentence must be action, sensation, or mid-dialogue. Never:
- Time-of-day: "It was three in the afternoon when..."
- Weather setup: "The rain had been falling since morning..."
- Character introduction: "John was a man who..."
- Establishing shot: "The office was on the fifth floor..."

Instead:
- Start with the verb in motion: "The door opened before he knocked."
- Start mid-dialogue: "'You're late.' She didn't look up."
- Start with sensation: "The coffee was still burning when she swallowed it."
- Start with aftermath: "He was already outside when he heard the shot."

The reader enters a moment already in progress. They catch up.

---

## 2. Sensory Anchoring

**Ground the reader physically before any emotion or inner thought.**

Order of scene elements:
1. Sensory detail (smell, sound, texture, temperature, light — pick one)
2. Physical setting (only the details relevant to the scene's action)
3. Character action
4. Interiority or emotion (only after the physical world is established)

Avoid: opening with a character's emotional state. Start with what their senses
encounter. Let emotional state emerge from behavior, not be announced before it.

One sensory detail is often enough. Three sensory details in one paragraph is a catalog.

---

## 3. Subtext Over Exposition

**Dialogue implies. Narration does not explain what dialogue already implies.**

If a character says something and the narrator then explains what it meant — delete the
narrator's explanation.

Test: remove the explanatory sentence. Does the reader lose information they cannot
recover from context and dialogue? If yes, keep. If no, cut.

Subtext lives in:
- What a character does not answer
- What they do instead of what they're asked
- The gap between what they say and what we see them doing
- Silence — noted by the narrator without interpretation

---

## 4. Pacing via Sentence Length

**Sentence length is the primary pacing instrument.**

| Situation | Sentence Length |
|-----------|----------------|
| Tension building | Short (4–7 words) |
| Action peak | Shortest (1–4 words, fragment possible) |
| Interiority / memory | Long (18–40 words) |
| Waiting | Medium-long with slowed verbs |
| Description | Medium (8–14 words) |
| Scene close | Variable — end on image or action, not summary |

Scene peaks use the writer's shortest sentences. A climactic moment written in 30-word
sentences is a pacing failure.

---

## 5. Cinematic Blocking

**Characters exist in space. Movement carries meaning. Silence is a beat.**

Before writing a scene, establish:
- Where each character is (spatial relationship — distance matters)
- What they are doing with their hands or body (idle action vs. charged action)
- What the camera would see (limit narration to observable fact)

Silence is a beat. Write it as action: "She didn't answer." Full stop. Not "She didn't
answer because she didn't know what to say." The reader supplies the reason; the narrator
supplies the silence.

Movement means something. A character who walks to the window instead of answering is
communicating. Show the movement. Don't explain what it communicates.

---

## 6. Scene Closure

**A scene closes before the reader is ready for it to end.**

Scene close options:
- Open image (a physical detail that implies but does not resolve)
- Interrupted action (scene ends before the action completes — reader continues it mentally)
- Recontextualized detail (something established early returns with changed meaning)

Never close on:
- A resolution sentence ("And so she knew that things would be different now")
- A summary of what the scene was about
- A character explicitly naming what they've learned

The scene ends on the body, not the mind.

---

## Application

Apply these rules in order:
1. Identify scene entry — revise if it begins with context-setting or weather
2. Check first paragraph for sensory anchor
3. Scan for explanatory narration after dialogue — delete
4. Read silently for rhythm — break uniform meter
5. Verify characters have spatial positions throughout
6. Check scene close — if it resolves cleanly, cut back one beat

Reference `scene-patterns.md` for working examples of each technique.
