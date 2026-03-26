# TODL — Transparent and Objective Demon List (Draft v0.1)

## 1. Purpose

TODL is an experimental system for ranking Geometry Dash levels using a **fully transparent and reproducible scoring model**.

Unlike existing demon lists, TODL does not rely on opinion-based ranking.
Every placement must be derived from **explicit, inspectable rules**.

## 2. Core Principles

TODL follows four non-negotiable constraints:

### 2.1 Transparency
- Every score must be broken down into visible components
- No hidden modifiers, overrides, or manual adjustments

### 2.2 Reproducibility
- Given the same level data, any person should compute the same result
- No reliance on private judgment or authority

### 2.3 Determinism
- The system must produce identical outputs for identical inputs
- No randomness or subjective interpretation

### 2.4 Universality (Long-Term Goal)
- The system should apply to any level, not only list demons
- Custom lists (e.g. themed or filtered lists) should be derivable from the same data

## 3. Scope (v0.1)

This version intentionally limits complexity.

It measures **mechanical difficulty only**, defined as:
- precision of inputs
- density of inputs
- consistency pressure

It explicitly ignores:
- memory difficulty
- visual readability
- player skill variation
- subjective “feel”

## 4. Model Definition

Each level is evaluated using three metrics:

### 4.1 Timing Precision (TP)

**Definition:**
Measures how tight input timing windows are.

**Concept:**
Smaller allowed timing -> higher difficulty

**Approximation (current):**
```
TP = 1 / average_timing_window
```

Where:
- timing window is measured in milliseconds or frames

---

### 4.2 Input Density (ID)

**Definition:**
Measures how frequently inputs occur.

**Concept:**
More inputs per second -> higher difficulty

**Formula:**
```
ID = total_inputs / level_duration_seconds
```

---

### 4.3 Error Tolerance (ET)

**Definition:**
Measures how punishing the level is in terms of consistency.

**Concept:**
Long uninterrupted sections -> higher difficulty

**Approximation:**
```
ET = longest_sequence_without_safe_break
```

## 5. Scoring Function

The total difficulty score is defined as:

```
Score = TP + ID + ET
```

Constraints:
- No weights (v0.1)
- No multipliers
- No special-case adjustments

## 6. Data Structure (Example)

```json
{
  "level": "Example Level",
  "metrics": {
    "timing_precision": 0.045,
    "input_density": 5.2,
    "error_tolerance": 18
  },
  "score": 23.245
}
```

All values must be derivable from observable gameplay.

## 7. Known Limitations

This model is incomplete by design.

Current limitations include:

* No handling of memory-based difficulty
* No modeling of visual ambiguity or deception
* No distinction between gamemodes (wave, ship, cube, etc.)
* Timing window estimation may be imprecise

These are not bugs—they are excluded to preserve clarity and testability.

## 8. Evaluation Criteria

This system should be judged based on:

1. **Internal consistency**

   * Do similar levels produce similar scores?

2. **Predictive alignment**

   * Do higher scores roughly match perceived difficulty?

3. **Failure clarity**

   * When the system fails, is the reason identifiable?

## 9. Open Questions

* How can timing windows be measured reliably and consistently?
* Should burst inputs be treated differently from sustained inputs?
* Is error tolerance adequately represented by longest sequences?
* At what point does adding metrics reduce transparency?

## 10. Request for Criticism

This model is intentionally minimal and likely flawed.

Feedback is requested in the following form:

* Identify specific levels where the model produces incorrect rankings
* Point out missing dimensions of difficulty
* Challenge definitions, not just results

Avoid general opinions such as:

* “this feels wrong”
* “this level is harder”

Instead, specify:

* *why* the model fails
* *which metric* is insufficient

## 11. Positioning

TODL does not aim to replace existing lists immediately.

Its purpose is to explore:

* whether difficulty can be modeled transparently
* whether consistency can be achieved without authority

Even if imperfect, a fully inspectable system provides value as:

* a reference tool
* a comparative baseline
* a challenge to opaque ranking methods
