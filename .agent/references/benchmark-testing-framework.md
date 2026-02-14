# Benchmark Testing Framework

**Purpose:** Standardized procedure for comparing workflow performance. Use this framework whenever you need to benchmark, evaluate improvements, or compare workflow variants.

**When to use:**
- Enhancing or refactoring an existing workflow
- Comparing two workflow approaches
- Validating that changes improve output quality
- Regression testing after workflow modifications

---

## 1. Methodology

### 1.1 Test Design Principles

- **Controlled comparison:** Same inputs, different workflow instructions
- **Empty context:** Each evaluation starts from empty agent context (no prior knowledge)
- **Multiple samples:** Minimum 2 test samples to test generalizability
  - 1 real-world sample (actual project data)
  - 1 synthetic sample (designed to test different challenges)
- **Blind measurement:** Criteria defined before running tests, not after seeing results

### 1.2 Sample Selection

| Sample | Source | Purpose |
|--------|--------|---------|
| Sample 1 | Real data from project | Tests real-world performance |
| Sample 2 | Synthetic (AI-generated) | Tests edge cases, different challenges |
| Sample N | Additional as needed | Tests specific scenarios |

**Synthetic sample guidelines:**
- Represent a different persona/scenario than the real sample
- Include known weaknesses the workflow should catch
- Include challenges the real sample doesn't cover
- Document the design rationale

### 1.3 Control vs Treatment

| Group | Description |
|-------|-------------|
| **Control** (Current) | Run using the existing workflow exactly as documented |
| **Treatment** (Enhanced) | Run using the proposed new/modified workflow |

Both groups must process the same samples with the same scope parameters.

---

## 2. Measurement Criteria

### 2.1 Standard Metrics Template

Every benchmark must define metrics before execution. Use this template and adapt as needed:

#### Binary Metrics (0/1)
Presence/absence of specific output patterns.

```
| # | Metric Name | Description | Control | Treatment |
|---|-------------|-------------|---------|-----------|
| 1 | [Pattern]   | [What to look for] | 0/1 | 0/1 |
```

#### Count Metrics
Number of specific items in the output.

```
| # | Metric Name | Description | Control | Treatment |
|---|-------------|-------------|---------|-----------|
| 1 | [Item]      | [What to count] | N | N |
```

#### Percentage Metrics
Ratios calculated from output analysis.

```
| # | Metric Name | Description | Control | Treatment |
|---|-------------|-------------|---------|-----------|
| 1 | [Ratio]     | [How calculated] | X% | X% |
```

### 2.2 Example: CV Evaluation Metrics

For reference, the CV evaluation benchmark used these 10 criteria:

| # | Metric | Type | Description |
|---|--------|------|-------------|
| 1 | Positioning Diagnosis | 0/1 | One-line gap framing present? |
| 2 | Evidence Citations | count | Exact CV quotes referenced |
| 3 | Rewrite Examples | count | Concrete replacement text provided |
| 4 | Score Impact Estimates | 0/1 | Numerical improvement estimates? |
| 5 | Contradiction Detection | count | Inconsistencies identified |
| 6 | Current Role Priority | 0/1 | Most recent role analyzed deepest? |
| 7 | Career Arc Analysis | 0/1 | Career trajectory mapped? |
| 8 | Top N Actions + Impact | 0/1 | Ranked actions with estimates? |
| 9 | Total Actionable Items | count | Specific, implementable suggestions |
| 10 | Specificity Score | % | Ratio of specific-to-generic suggestions |

---

## 3. Execution Protocol

### Step 1: Prepare

1. Create benchmark directory:
   ```
   docs/[feature]/benchmark/
   ├── samples/           # Test samples
   ├── current-workflow/   # Control group output
   └── enhanced-workflow/  # Treatment group output
   ```
2. Define metrics (from Section 2)
3. Create or select test samples (from Section 1.2)
4. Document any modified framework/reference files in `enhanced-workflow/`

### Step 2: Execute Control Group

For each sample:
1. Start from empty context (no prior analysis)
2. Follow the current workflow exactly as documented
3. Save output to `current-workflow/eval-sample-N.md`
4. Optionally generate HTML report: `current-workflow/eval-sample-N.html`

### Step 3: Execute Treatment Group

For each sample:
1. Start from empty context (no prior analysis)
2. Follow the enhanced/modified workflow with all proposed changes
3. Save output to `enhanced-workflow/eval-sample-N.md`
4. Optionally generate HTML report: `enhanced-workflow/eval-sample-N.html`

### Step 4: Measure

1. Score each evaluation output against the defined metrics
2. Calculate per-sample scores for both groups
3. Calculate averages across samples
4. Calculate deltas (Treatment - Control)

### Step 5: Report

Generate `benchmark-results.html` (or `.md`) with:
- Methodology summary
- Per-sample metrics table
- Averaged metrics table
- Quality Index scores
- Side-by-side examples
- Trade-off analysis
- Verdict

---

## 4. Quality Index Formula

The Quality Index provides a single weighted score for apples-to-apples comparison.

### Calculation

```
Quality Index = (Binary Score × 0.40) + (Volume Score × 0.30) + (Specificity × 0.30)
```

Where:
- **Binary Score** = (total binary metrics achieved / total binary metrics possible) × 10
- **Volume Score** = normalize count metrics to 0-10 scale based on reasonable expectations
- **Specificity** = specificity percentage normalized to 0-10 scale

### Normalization for Volume Metrics

Define "reasonable expectations" per metric before testing:

| Metric | Floor (0) | Ceiling (10) |
|--------|-----------|--------------|
| Evidence Citations | 0 | 30+ |
| Rewrite Examples | 0 | 15+ |
| Actionable Items | 0 | 20+ |

Formula: `Score = min(10, (actual / ceiling) × 10)`

### Interpretation

| Quality Index | Rating | Meaning |
|---------------|--------|---------|
| 9.0 - 10.0 | Exceptional | Comprehensive, evidence-based, highly actionable |
| 7.0 - 8.9 | Strong | Good quality with minor gaps |
| 5.0 - 6.9 | Adequate | Functional but missing key patterns |
| 3.0 - 4.9 | Weak | Significant quality gaps |
| 0.0 - 2.9 | Poor | Generic, lacks specificity |

---

## 5. Trade-Off Analysis

Every benchmark report must include trade-off analysis:

| Dimension | Control | Treatment | Verdict |
|-----------|---------|-----------|---------|
| Output length | X lines | Y lines | [Acceptable/Concerning] |
| Token cost | ~Xk tokens | ~Yk tokens | [Acceptable/Concerning] |
| Generation time | ~Xs | ~Ys | [Acceptable/Concerning] |
| Cognitive load | [Low/Med/High] | [Low/Med/High] | [Acceptable/Concerning] |
| Consistency guarantee | [Description] | [Description] | [Better/Worse/Same] |

---

## 6. Decision Framework

### When to Adopt Changes

| Condition | Action |
|-----------|--------|
| Quality Index improves ≥20% with acceptable trade-offs | ✅ Adopt |
| Quality Index improves ≥20% but trade-offs are concerning | ⚠️ Adopt with modifications |
| Quality Index improves <20% | 🔶 Consider if trade-offs justify |
| Quality Index decreases | ❌ Reject |

### When to Create Separate Variants

If the treatment workflow has significantly different trade-offs (e.g., 3× token cost), consider creating two variants instead of replacing:
- **Quick variant:** Lightweight, fast iteration
- **Deep variant:** Comprehensive, resource-intensive

This was the approach taken for `/evaluate-cv-quick` vs `/evaluate-cv-deepdive`.

---

## 7. Benchmark Report Template

```markdown
# Benchmark: [Current Feature] vs [Enhanced Feature]

## Methodology
- Samples: [N] ([real/synthetic breakdown])
- Evaluations: [N] ([samples × workflows])
- Metrics: [N] objective criteria
- Context: Empty (fresh agent per evaluation)

## Results Summary
| Metric | Control (avg) | Treatment (avg) | Delta |
|--------|--------------|-----------------|-------|
| ...    | ...          | ...             | ...   |
| **Quality Index** | **X.X** | **X.X** | **+XX%** |

## Trade-Off Analysis
[Per Section 5]

## Side-by-Side Examples
[Show 2-3 concrete before/after output comparisons]

## Verdict
[Adopt / Adopt with modifications / Reject]
[Justification citing specific metrics]
```

---

_This framework is a project context reference. It is loaded by AI agents when performing benchmark tests or workflow comparisons._
