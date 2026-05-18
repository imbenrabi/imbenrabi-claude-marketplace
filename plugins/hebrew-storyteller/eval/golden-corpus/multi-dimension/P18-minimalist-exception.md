---
passage_id: P18
target_dimension: rhythm_variety
ground_truth_score: 6
ground_truth_verdict: PASS
applicable_dimensions:
  - naturalness
  - ai_detectability
  - register_consistency
  - rhythm_variety
dialogue_present: false
tone_preset: minimalist-literary
tone_overrides: {}
notes: "Critical exception test — passage is intentionally monotone (all very short sentences), which would normally score 2/10 on rhythm. But the minimalist-literary preset's rubric override (in quality-rubric.md) says monotony is the style here. Judge should score rhythm ~6 (not 2) and overall PASS. If judge scores rhythm < 5, judge has not applied the preset override and FAILS calibration on this passage."
expected_top_issue_tags: []
---

בא הביתה. שתה מים. ישב. קם. הסתכל בחלון. ישב שוב. החשיכה נכנסה. לא הדליק.
