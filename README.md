# Market Strategy Advantage Library

## Purpose

This folder is the permanent repository for every market strategy, signal, feature, or combination of signals that demonstrates some evidence of predictive or trading advantage during experimentation.

The purpose of this library is **not only to store winning strategies**.

It must preserve:

- strong standalone signals,
- weak but real signals,
- promising but statistically uncertain signals,
- successful combinations of individually weak signals,
- failed combinations,
- and the strongest total model currently discovered.

The library should make it possible for later agents to reuse previous discoveries instead of repeatedly rediscovering the same effects.

---

# Folder Structure

```text
market_strategies/
│
├── README.md
│
├── weak/
│
├── unsure/
│
├── strong/
│
├── combined_confirmed/
│
├── combined_denied/
│
└── current_model/
```

---

# Core Principle

A signal should **never be discarded simply because it is not profitable enough by itself**.

Many useful market effects are individually too small to trade after transaction costs but become valuable when combined with other independent or complementary signals.

For this reason:

> Any signal showing reproducible predictive information should be preserved and classified.

The primary classification question is:

```text
Does the signal contain useful information?

        ↓

If yes, how strong is that information,
and can it currently survive realistic trading costs?
```

---

# 1. `weak/`

Store signals here when:

> A reproducible market relationship appears to exist, but the magnitude of the advantage is too small to reliably overcome realistic transaction costs, spread, slippage, or execution uncertainty.

Typical characteristics:

- directionally consistent effect,
- some evidence of predictive information,
- economically small edge,
- net expectancy approximately zero or negative after costs,
- signal may improve another model,
- useful as a conditioning variable,
- useful as a confirmation signal,
- useful for position sizing even if not useful for direct entries.

Example:

```text
EMA9 touch followed by slightly higher probability of a bounce.

Gross expected move: +3.2 bps
Estimated execution cost: 4.5 bps

Result:
Predictive information exists,
but standalone trading is uneconomic.
```

Classification:

```text
weak/
```

Do **not** delete weak signals.

Weak signals are explicitly candidates for later combination testing.

---

# 2. `unsure/`

Store signals here when:

> The observed advantage may be large enough to beat realistic costs, but statistical evidence is not strong enough yet to confidently conclude that the effect is real.

Typical causes:

- insufficient sample size,
- wide confidence intervals,
- rare market condition,
- unstable results between periods,
- promising out-of-sample result but too few observations,
- positive expectancy but marginal statistical significance,
- strong economic magnitude but uncertain robustness.

Example:

```text
Signal expectancy after costs: +14 bps
Number of independent observations: 57
p-value: 0.11

Potentially very useful,
but evidence is currently insufficient.
```

Classification:

```text
unsure/
```

These signals should receive special attention in future experiments because additional observations or combination with related signals may resolve the uncertainty.

---

# 3. `strong/`

Store signals here when:

> The signal has a statistically credible, economically meaningful advantage that survives realistic transaction costs and slippage.

A strong strategy should ideally demonstrate:

- positive net expectancy after costs,
- adequate sample size,
- statistical significance or comparably convincing evidence,
- robustness across reasonable parameter changes,
- robustness across time,
- out-of-sample or walk-forward validation,
- no obvious single-period dependence,
- no obvious look-ahead leakage,
- plausible market mechanism,
- realistic execution assumptions.

The exact statistical threshold should depend on the experiment, but the researcher should generally seek evidence comparable to:

```text
p < 0.05
```

while recognizing that p-values alone are not sufficient.

The classification should consider the full evidence:

```text
Statistical strength
+
Economic magnitude
+
Robustness
+
Out-of-sample behavior
+
Execution realism
```

Example:

```text
Gross expectancy: +19 bps
Estimated cost: 5 bps
Net expectancy: +14 bps
N: 4,320
p-value: 0.003

Profitable across:
2021
2022
2023
2024
2025
2026

Walk-forward results remain positive.
```

Classification:

```text
strong/
```

---

# Classification Matrix

Use the following basic classification logic:

| Predictive Evidence | Can Beat Costs | Statistical Confidence | Folder |
|---|---:|---:|---|
| Yes | No | Any reasonable evidence | `weak/` |
| Yes / Likely | Yes | Insufficient | `unsure/` |
| Yes | Yes | Strong | `strong/` |
| No reproducible evidence | No | No | Do not promote into the advantage library |

A signal does **not** need to generate a complete profitable trading strategy to deserve preservation.

If it predicts:

- direction,
- volatility,
- continuation,
- reversal,
- probability of MA bounce,
- probability of MA penetration,
- magnitude,
- timing,
- relative performance,
- market regime,
- or some other useful conditional distribution,

it may still be valuable.

---

# Combination Testing

Weak and unsure signals should periodically be tested together.

This is a major purpose of the library.

A signal that is weak individually may become highly valuable when conditioned on another signal.

For example:

```text
EMA9 touch
        +
high relative strength
        +
market breadth improving
        +
volume absorption
```

may be much stronger than any of those variables individually.

Agents should therefore search for:

- additive effects,
- nonlinear interactions,
- conditional effects,
- confirmation relationships,
- cancellation relationships,
- regime dependencies,
- signal sequencing,
- lead/lag relationships,
- ensemble effects,
- position-sizing information.

---

# 4. `combined_confirmed/`

Store combinations here when:

> Multiple previously identified signals produce a statistically and economically meaningful advantage when combined.

The inputs may come from:

```text
weak/
unsure/
strong/
```

although the primary purpose is discovering useful interactions among `weak/` and `unsure/` signals.

Examples:

```text
weak_signal_17
+
weak_signal_42
+
unsure_signal_08

→ strong combined edge
```

or:

```text
EMA proximity
+
relative strength
+
volume acceleration
+
index divergence

→ materially higher bounce probability
```

A confirmed combination should document exactly which underlying signals were used.

---

# 5. `combined_denied/`

Store combinations here when:

> A combination was explicitly tested and failed to produce meaningful additional advantage.

This folder is extremely important.

Do not simply forget failed combinations.

Recording failures prevents future agents from repeatedly testing the same interaction.

Examples:

```text
signal_A + signal_B
```

Result:

```text
No improvement.
Interaction denied.
```

or:

```text
signal_A + signal_B + signal_C
```

Result:

```text
Apparent in-sample improvement disappeared out of sample.
Combination denied.
```

The folder therefore acts as the project's **negative knowledge base**.

---

# 6. `current_model/`

This folder contains the:

# CURRENT STRONGEST TOTAL COMBINATION

It represents the best complete model discovered by the project so far.

It should combine whichever signals, models, filters, regime variables, execution rules, and sizing logic currently produce the strongest robust performance.

Only the current champion model belongs here.

When a better total model is discovered:

```text
OLD MODEL
    ↓
archive / corresponding strategy records

NEW MODEL
    ↓
current_model/
```

The current model should be updated only when the challenger demonstrates a meaningful improvement under fair testing.

Do not replace the current model merely because another model produces slightly better in-sample results.

Prefer improvements that survive:

- out-of-sample testing,
- walk-forward testing,
- transaction costs,
- slippage,
- different market regimes,
- parameter perturbation,
- and reasonable execution assumptions.

---

# Strategy Promotion Flow

The general research pipeline is:

```text
NEW EXPERIMENT
      │
      ▼
Evidence of predictive advantage?
      │
 ┌────┴────┐
 │         │
NO        YES
 │         │
Stop      ▼
       Can reliably
       beat costs?
         │
      ┌──┴───┐
      │      │
     NO     YES
      │      │
      ▼      ▼
    WEAK   Statistically
           convincing?
               │
          ┌────┴────┐
          │         │
         NO        YES
          │         │
          ▼         ▼
       UNSURE     STRONG
```

Then:

```text
WEAK
   \
    \
UNSURE -----> COMBINATION TESTING
    /               │
STRONG              │
                    ▼
          ┌───────────────────┐
          │                   │
     Combination works   Combination fails
          │                   │
          ▼                   ▼
combined_confirmed/    combined_denied/
          │
          ▼
Potential inclusion
in total model
          │
          ▼
current_model/
```

---

# Required Strategy Record

Every stored signal should have enough information for another agent to reproduce it.

At minimum record:

```text
Name
ID
Date tested
Signal category
Source experiment
Market / universe
Timeframe
Holding horizon
Signal definition
Entry logic
Exit logic
Filters
Parameters
Sample size
Gross expectancy
Estimated costs
Estimated slippage
Net expectancy
Hit rate
Effect size
Statistical significance
Confidence interval
Sharpe if applicable
Maximum drawdown if applicable
Out-of-sample result
Regime behavior
Robustness tests
Known weaknesses
Possible interactions
Final classification
Reason for classification
```

If applicable also preserve:

- PnL curve,
- parameter sensitivity plots,
- conditional return distributions,
- feature importance,
- probability curves,
- calibration plots,
- trade logs,
- code,
- configuration,
- datasets or dataset references.

---

# Recommended Strategy Package

Each strategy should preferably receive its own directory.

Example:

```text
strong/
└── ema9_relative_strength_bounce/
    ├── README.md
    ├── results.json
    ├── trades.parquet
    ├── pnl.csv
    ├── config.json
    ├── charts/
    └── code/
```

The strategy `README.md` should explain the result in human-readable form.

Machine-readable metrics should also be stored whenever practical so future agents can aggregate the entire library automatically.

---

# Signal IDs

Every signal should receive a permanent unique ID.

For example:

```text
SIG_MA_0001
SIG_MA_0002
SIG_RS_0001
SIG_VOL_0001
SIG_BREADTH_0001
SIG_CORR_0001
SIG_MICRO_0001
```

Combinations should reference those IDs:

```text
COMBO_0042

Inputs:
SIG_MA_0017
SIG_RS_0008
SIG_VOL_0023
```

This makes it possible to track relationships between hundreds or thousands of experiments.

---

# Do Not Double Count Related Signals

When combining signals, agents must recognize that several signals may measure nearly the same underlying phenomenon.

For example:

```text
5-minute relative strength
10-minute relative strength
15-minute relative strength
```

may contain highly overlapping information.

An apparent improvement from adding correlated versions of the same feature should not automatically be treated as three independent confirmations.

Test:

- feature correlation,
- incremental predictive value,
- incremental PnL,
- conditional information gain,
- marginal contribution to the full model.

---

# Costs and Slippage

All classifications involving profitability must use realistic execution assumptions.

At minimum consider:

```text
bid/ask spread
slippage
market impact where relevant
commissions if applicable
borrow costs for shorts
financing costs
option spreads
latency/execution limitations
turnover
```

Gross alpha and executable alpha must remain separate concepts.

A signal can have:

```text
real predictive alpha
```

while having:

```text
no standalone executable alpha
```

Such a signal belongs in `weak/`, not in the trash.

---

# Statistical Significance

Do not mechanically classify signals using a single p-value.

Consider:

- sample size,
- effect size,
- confidence intervals,
- multiple hypothesis testing,
- regime stability,
- cross-sectional consistency,
- temporal consistency,
- out-of-sample behavior,
- parameter sensitivity,
- autocorrelation between observations.

Thousands of experiments may eventually be run in this project.

Therefore false discoveries are a major risk.

The more hypotheses tested, the stronger the validation requirements should become.

---

# Prefer Robust Regions Over Exact Parameters

A strategy that works only at:

```text
threshold = 1.8734
```

but fails at:

```text
1.7
1.8
1.9
2.0
```

is suspicious.

A strategy showing good behavior across a broad parameter region is much more credible.

Always prefer:

```text
stable parameter regions
```

over:

```text
single optimized peaks
```

---

# Regime Dependence

A signal does not need to work in every market regime.

A powerful regime-specific signal can still be extremely valuable.

If an effect works only during:

```text
high volatility
low volatility
bull markets
bear markets
high dispersion
low dispersion
high correlation
low correlation
risk-on
risk-off
opening session
closing session
```

document that explicitly.

Do not average away useful conditional effects.

These regime dependencies themselves may later become signals.

---

# Combination Priority

When searching for combinations, prioritize signals that appear to contain **different information**.

For example:

```text
price structure
+
relative strength
+
volume
+
market breadth
+
cross-asset information
```

is generally more interesting than:

```text
EMA8
+
EMA9
+
EMA10
+
EMA11
```

unless testing demonstrates otherwise.

Complementary signals have greater potential to produce meaningful ensemble improvements.

---

# Promotion to `current_model/`

A new combination should challenge the existing current model directly.

Compare using identical:

- dates,
- instruments,
- costs,
- capital assumptions,
- execution assumptions,
- risk limits.

Evaluate metrics such as:

```text
net return
Sharpe ratio
Sortino ratio
maximum drawdown
Calmar ratio
profit factor
tail behavior
turnover
capacity
regime stability
out-of-sample performance
```

The goal is not simply:

```text
maximum historical return
```

The goal is:

```text
maximum robust executable risk-adjusted alpha.
```

---

# Preserve Discoveries

Never overwrite a useful discovery simply because a newer model exists.

The individual signal library is the project's accumulated knowledge.

A future experiment may reveal that an old weak signal becomes extremely powerful under a newly discovered condition.

Therefore:

> Preserve the components even after they have been incorporated into larger models.

---

# Final Rule

Every market effect discovered by the research system should eventually answer four questions:

```text
1. Is the effect real?

2. Is the effect economically meaningful?

3. Can it survive realistic execution costs?

4. Does it improve the total model when combined with other information?
```

Its location in this repository should make the answer immediately visible:

```text
weak/
    Real information, currently too small to trade alone.

unsure/
    Potentially tradeable information, evidence still insufficient.

strong/
    Statistically credible and economically tradeable standalone edge.

combined_confirmed/
    A tested combination whose joint effect is useful.

combined_denied/
    A tested combination that failed validation.

current_model/
    The strongest robust total combination discovered so far.
```

The ultimate purpose of the entire repository is to continuously transform isolated market observations into an increasingly powerful, validated, and executable market model.