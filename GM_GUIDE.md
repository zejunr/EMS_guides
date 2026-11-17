# Energy Market Game

## Economic Intuition, Assumptions, and KPI Methodology

**Edition:** 17 November 2026 — browser and iOS platform, including three-block
bidding, euro leaderboard scoring, and optional Market Coach feedback.
Individual games retain their saved settings, which may differ from the defaults.

## 1. Purpose and scope

The Energy Market Game is a classroom simulation in which each student or
team manages a Generator Company (GENCO). Repeated auctions connect students'
offers and investment decisions with dispatch, market prices, and profitability.
Students can examine why a high price does not necessarily produce a high profit and why
equal starting capacity does not guarantee equal economic opportunities.

This guide explains the purpose of the features, their economic assumptions,
the model and KPI formulas, and how to interpret the results. For controls,
session workflows, scheduling, announcements, and code recovery, use the
[GM Interface Guide](GM_INTERFACE_GUIDE.md).

The implemented scenarios are **Normal Market**, **LNG Crisis**, and **Capacity
Investment**. They share the same clearing engine. Each scenario changes the
relevant inputs or decisions rather than introducing a different market model.

| Scenario | Teaching question |
|---|---|
| Normal Market | How do quantity and price offers affect dispatch, uniform prices, and profit? |
| LNG Crisis | How does a gas-generation cost shock affect the merit order and the returns of different portfolios? |
| Capacity Investment | Can the revenues from changing a fleet justify its recurring capacity charges under changing market conditions? |

Optional demand growth supplies an uncertain demand trend within these
scenarios. Market Coach helps students interpret their own saved results.
Independent games support separate experiments; appended phases support a
continuous sequence with the same participants and cumulative history.

## 2. Game structure, timing, and units

A **game** contains its participants, portfolios, phases, and result history.
A **phase** applies one scenario over a sequence of market periods. A **period**
is one market-clearing auction. New phases contain six periods, with displayed
period numbering restarting at 1. Some older games may contain fewer periods.

Each period is cleared independently. There are no ramping constraints,
intertemporal operating decisions, or energy inventories linking auctions.
However, installed capacity, scenario settings, and cumulative reporting can
carry information or decisions across periods.

Capacity, offered quantity, and dispatch are expressed in **MW**. Bid prices
and marginal costs are expressed in **€/MWh**. Financial calculations use a
one-hour period convention:

$$
E_{g,t}=p_{g,t}\Delta t,\qquad \Delta t=1\text{ hour}.
$$

Here, $p_{g,t}$ is generator dispatch in MW and $E_{g,t}$ is generated energy
in MWh. Thus, a dispatch of 100 MW corresponds to 100 MWh for settlement.
Displayed time labels identify auctions; they do not change this one-hour
convention.

## 3. Initial portfolios and generation costs

Each GENCO in a newly initialized game receives exactly **400 MW** of installed
capacity, distributed across one to three generators. A starting portfolio
does not repeat a technology. Equal installed capacity does not imply equal
costs, technology mixes, or profit opportunities.

### 3.1 Portfolio generation

The generator count is initially drawn with probabilities of 25%, 50%, and
25% for one, two, and three generators, respectively. The system selects
feasible combinations of technologies and allocates capacity so that each
portfolio totals 400 MW.

The technology-specific limits and marginal-cost sampling ranges are:

| Technology | Capacity range per initial generator | Base marginal-cost draw |
|---|---:|---:|
| Nuclear | 300–700 MW | Uniform over 1.5–11.5 €/MWh |
| Coal | 150–400 MW | Uniform over 15–35 €/MWh |
| Gas | 100–300 MW | Uniform over 70–90 €/MWh |
| Oil | 40–150 MW | Uniform over 150–170 €/MWh |

The common 400 MW portfolio total further restricts feasible combinations;
the individual ranges are not independent capacity entitlements.

Candidate portfolios also pass a quality screen. Define a reference margin:

$$
M_i=\sum_{g\in G_i}P_g^{base}\max(100-c_g^{base},0),
$$

where $G_i$ is the set of generators owned by GENCO $i$. The quality score is:

$$
Q_i=0.45\frac{\sum_{g\in G_i}P_g^{base}}{400}
+0.55\frac{M_i}{20{,}000}.
$$

Candidates are accepted when $0.70\leq Q_i\leq1.35$. The reference price of
100 €/MWh and margin of 20,000 € are initialization parameters, not forecasts
of clearing prices or guaranteed earnings. Because candidate portfolios are
screened, the accepted portfolios are not unrestricted independent uniform
draws. The stored generation seed makes initialization reproducible.

### 3.2 Persistence and effective parameters

Base capacities and marginal costs are stored after assignment. They are not
redrawn each period. The clearing and financial calculations use the
generator's **effective capacity** and **effective marginal cost** for the
relevant period.

In Normal Market, these are ordinarily the installed capacity and base cost.
LNG Crisis changes gas marginal costs, while finalized investment adds
generators. Period-specific records preserve the conditions used for
historical results.

## 4. Supply offers and physical withholding

Every generator submits between **one and three quantity–price blocks** in
each period, in every scenario.

For generator $g$, block $k$, and period $t$:

- $q^{bid}_{g,k,t}$ is the offered quantity in MW;
- $b_{g,k,t}$ is the offered price in €/MWh;
- $P^{max}_{g,t}$ is the generator's effective capacity.

Offer quantities satisfy:

$$
q^{bid}_{g,k,t}\geq0,\qquad
\sum_k q^{bid}_{g,k,t}\leq P^{max}_{g,t}.
$$

Quantities may be decimal values. Each block requires a price within the
game's saved bid limits, even when its quantity is zero. The default limits
are 0–4,000 €/MWh.

Students may offer less than their full capacity. This permits **physical
withholding**: installed capacity need not equal offered capacity. The
difference between a bid price and marginal cost also permits price markups.
Block numbers identify offers; they do not determine merit order.

For example, a 200 MW generator can offer:

| Block | Offered quantity | Bid price |
|---|---:|---:|
| 1 | 80 MW | 120 €/MWh |
| 2 | 50 MW | 80 €/MWh |
| 3 | 30 MW | 200 €/MWh |

The total offered quantity is 160 MW, leaving 40 MW unoffered. The 80 €/MWh
block is evaluated before the 120 €/MWh block.

A submission covers the student's complete portfolio and may be replaced
until bidding closes. Replacing a submission also removes blocks omitted
from the replacement. Actual market clearing uses the saved blocks.

## 5. Demand formulation

Demand is generated by the system rather than submitted by student buyers.
It consists of an **inelastic component** and a **price-responsive elastic
component**.

### 5.1 Reference demand

**With the optional demand-growth setting switched off**, the saved demand
multiplier $\alpha_t$ determines maximum demand in period $t$:

$$
D_t^0=\alpha_t C_{ref},
$$

where $C_{ref}$ is the applicable reference installed capacity in MW.
$D_t^0$ is the maximum quantity represented by the demand curve, not a
requirement that this quantity must be served.

For 40 initial GENCOs:

$$
C_{ref}=40\times400=16{,}000\text{ MW}.
$$

At $\alpha_t=0.80$, maximum demand is 12,800 MW.

With growth switched off, the default six-period multipliers $\alpha_t$ for
Normal and LNG phases are
**1.02, 0.65, 0.80, 0.90, 0.95, and 0.75**. These are fixed initialization
settings, not random draws. They do not imply a specified probability of
scarcity. Existing games retain their saved values.

When the GM enables the extra growth option, the affected demand quantities
instead follow the saved growth path in Section 5.4. The fixed period factors
do not continue to determine each of those periods independently.

### 5.2 Inelastic and elastic components

Under the default formulation, in both demand modes:

$$
D_{IN,t}^{max}=0.8D_t^0,\qquad D_{EL,t}^{max}=0.2D_t^0.
$$

The inverse demand curve gives willingness to pay for total quantity $q$.
For the inelastic component, where $0\leq q\leq0.8D_t^0$:

$$
P_t(q)=1{,}000.
$$

For the elastic component, where $0.8D_t^0<q\leq D_t^0$:

$$
P_t(q)=1{,}000\frac{D_t^0-q}{0.2D_t^0}.
$$

Prices in this expression are in €/MWh. Demand above $D_t^0$ is not
represented.

The first 80% is called inelastic because its requested quantity does not
adjust gradually with price within its willingness-to-pay range. In the
implemented optimization, however, it is a demand block valued at
**1,000 €/MWh**, not an equality constraint requiring full service.
Consequently, some of this component can remain unserved when supply is
insufficient or offers are too expensive.

The remaining 20% has willingness to pay declining linearly from 1,000 to
0 €/MWh. The 1,000 €/MWh valuation is a demand parameter; it is neither a
fixed settlement price nor the generator bid-price ceiling.

### 5.3 Demand updates and scarcity

**With growth switched off**, individual period demand can be edited while
the period is waiting or bidding is open; closing bidding locks it.

**With growth switched on**, the saved path replaces individual demand edits.
Its baseline controls and locking rules are described in Section 5.4.
Both modes use the same saved period demand for actual and perfect-competition
clearing.

Maximum demand exceeding installed capacity does not by itself imply
unserved inelastic demand: the elastic component may reduce consumption.
Similarly, actual offered capacity can be less than installed capacity
because of withholding. Capacity may constrain total consumption and allow
demand-side willingness to pay to set the price even when high-value demand
is served. These distinctions matter when interpreting scarcity.

Investment purchases do not automatically expand demand in either mode.
Late-admission recalculation applies to eligible capacity-scaled periods,
not to saved growth-managed periods. See Sections 15 and 16.

### 5.4 Optional demand growth: 3% expected increase

**Demand growth is an extra option that the GM must turn on.** It is available
in Normal Market, LNG Crisis and Capacity Investment. New games start with it
switched off; selecting a scenario alone does not enable it. Once enabled in
a game, its later appended phases automatically continue that path. Games
with growth off keep the capacity-scaled settings described in Section 5.1.

The purpose is to introduce an upward demand trend with uncertainty, so that
students must consider whether their existing generation capacity will remain
adequate. This gives capacity expansion an economic purpose without making
demand rise automatically whenever students invest. The 3% value is a teaching
calibration per game period, not an estimate of annual electricity-demand growth
or a forecast of the market-clearing price.

The GM chooses a starting demand baseline as a percentage of recorded starting
capacity. Writing that baseline as $D_{base}$:

$$
D_{base}=\beta C_{start},\qquad
g_t\sim U(-0.02,0.08),\qquad
D_t^0=D_{t-1}^0(1+g_t).
$$

Here, $\beta$ is the GM's chosen baseline fraction, $C_{start}$ is the recorded
starting capacity, and $g_t$ is an independent growth draw. For a newly enabled
path, the first draw applies to Period 1 using $D_{base}$ as its predecessor.
Older saved paths may instead retain Period 1 as their anchor and start drawing
from Period 2; the saved growth table identifies the actual starting point.

The expected rate is the midpoint of the distribution:

$$
E[g_t]=\frac{-0.02+0.08}{2}=0.03.
$$

This is **3% expected growth per transition**, not exactly 3% in every period or
in every game's realized average. A period can have negative growth. Increases
compound: after $n$ independent draws, expected demand is
$D_{base}(1.03)^n$. Six draws imply approximately 19.4% expected cumulative
growth from the pre-draw baseline; five draws imply approximately 15.9%. Count
the draws actually used rather than assuming the total increase is 3%.

The saved quantities are maximum, zero-price demand, not necessarily the
quantity that clears. Both the 80% high-value component and the 20% flexible
component grow together. Their valuation prices and the common clearing model
are unchanged. Actual and perfect-competition clearing use the same demand path.

Growth rates and demand quantities are drawn once and saved. Restarting the
server does not redraw them. Adding another phase to the same game continues
from the preceding phase's final demand and applies a new draw to its first
period, even though the displayed period number restarts at 1. Adding supply
through investment or late admission does not inflate this saved demand path.

Growth-managed periods use the saved absolute quantities. An unused baseline
can be rescaled before its first affected period opens without redrawing rates.
A baseline taken from a released period remains fixed. Once growth bidding
begins, later phases extend the path without revising completed results.
Scarcity and profitable investment are possible outcomes, not guaranteed ones.
For enabling, saving, and restoring a path, see the
[demand controls](GM_INTERFACE_GUIDE.md#7-choose-the-demand-settings).

## 6. Discretization of elastic demand

Graphs present a continuous demand curve, but the clearing model approximates
the elastic component with $L$ equal-sized blocks. Each has quantity:

$$
\Delta d_t=\frac{0.2D_t^0}{L}.
$$

The willingness to pay of elastic block $l$ is evaluated at its midpoint:

$$
\pi_l=1{,}000\left(1-\frac{l-0.5}{L}\right),
\qquad l=1,\ldots,L.
$$

With the default $L=20$, block values are 975, 925, …, 25 €/MWh. Each elastic
block represents 1% of maximum demand. The inelastic block is represented
separately at 1,000 €/MWh.

Clearing therefore uses a stepped approximation of the continuous curve.
The number of increments controls its resolution; a point on the drawn
continuous curve need not coincide exactly with the discrete auction outcome.

## 7. Actual market-clearing model

The market is a single-node, uniform-price auction. The same linear
optimization model is used in all scenarios.

In addition to the offer notation in Section 4, define:

- $p_{g,k,t}$: accepted supply from block $k$, in MW;
- $p_{g,t}=\sum_k p_{g,k,t}$: total generator dispatch;
- $d_{IN,t}$: served inelastic demand, in MW;
- $d_{l,t}$: served elastic demand block $l$, in MW.

The objective maximizes **declared economic surplus**:

$$
\max\quad
1{,}000d_{IN,t}+\sum_l\pi_l d_{l,t}
-\sum_g\sum_k b_{g,k,t}p_{g,k,t}.
$$

The constraints are:

$$
\sum_g\sum_k p_{g,k,t}=d_{IN,t}+\sum_l d_{l,t},
$$

$$
0\leq p_{g,k,t}\leq q^{bid}_{g,k,t},
$$

$$
0\leq d_{IN,t}\leq0.8D_t^0,
$$

$$
0\leq d_{l,t}\leq\frac{0.2D_t^0}{L}.
$$

Submitted quantities are fixed inputs to this optimization and must already
satisfy the aggregate capacity restriction in Section 4.

The objective subtracts **bid expenditure at offered prices**, not production
cost at actual marginal costs. Under strategic bidding, its value should
therefore not be interpreted as welfare calculated using true generation
costs. Marginal costs enter financial reporting and the perfect-competition
benchmark separately.

### Equal-price allocation

After the total traded quantity is determined, supply is allocated in
ascending bid-price order. A partially accepted equal-price group is
allocated proportionally to the quantities offered in that group.

For equally priced offers of 60 MW and 40 MW, acceptance of 50 MW from the
group produces dispatches of 30 MW and 20 MW. The allocation is not random
and does not favor a particular block number or student.

### Zero quantities and no trade

A zero-MW block remains a recorded offer but adds no width to the supply
curve and receives no dispatch. If every supply quantity is zero, the market
records no trade. More generally, trade can also be zero when supply and
demand valuations do not support an exchange.

With no trade, actual dispatch, revenue, variable production cost, and
operating profit are zero. Investment capacity charges may still apply.

## 8. Market-clearing price and settlement

The market-clearing price (MCP), denoted $\lambda_t$, is obtained from the
dual value of the energy-balance constraint, using the solver's consistent
sign convention.

All dispatched supply is paid the same MCP. This is a uniform-price
settlement, not a pay-as-bid settlement. The MCP is not necessarily identical
to the highest accepted supply bid: elastic demand and non-unique dual
solutions can affect the reported price.

When no trade occurs, the application reports **No trade**, with no MCP,
rather than assigning an artificial settlement price.

## 9. Revenue, production cost, and operating profit

For generator $g$:

$$
R_{g,t}=\lambda_t E_{g,t},
$$

$$
VC_{g,t}=c_{g,t}E_{g,t},
$$

$$
\Pi_{g,t}^{operating}=R_{g,t}-VC_{g,t}
=(\lambda_t-c_{g,t})E_{g,t}.
$$

Here, $c_{g,t}$ is the generator's effective marginal cost. All dispatched
blocks of a generator use this same cost. Their accepted quantities are
summed before generator-level financial reporting.

For GENCO $i$:

$$
E_{i,t}=\sum_{g\in G_i}E_{g,t},\qquad
\Pi_{i,t}^{operating}=\sum_{g\in G_i}\Pi_{g,t}^{operating}.
$$

Revenue and production cost are aggregated in the same way. Bid prices
influence dispatch and the MCP, but they are not substituted for marginal
cost in the production-cost calculation.

## 10. Perfect-competition benchmark

The benchmark clears the same period demand using one offer per available
generator, with:

$$
b_{g,t}^{PC}=c_{g,t},\qquad
q_{g,t}^{PC}=P_{g,t}^{max}.
$$

Thus, every generator offers its full effective capacity at its effective
marginal cost. The benchmark includes LNG cost adjustments and finalized
investment capacity.

It does not preserve actual block quantities, markups, or withholding.
Generators of non-submitting students remain included. The benchmark
calculates its own price, dispatch, revenue, cost, and operating profit.

This is a counterfactual in which **all generators** change their offers.
It is not a calculation in which only one student's offers are changed while
every other student's strategy remains fixed. The application may use
“truthful” in internal field names; in this guide, these outputs are called
perfect-competition results.

## 11. Strategic performance in euros

For GENCO $i$, the period's strategic-performance measure is:

$$
A_{i,t}=\Pi_{i,t}^{actual,operating}
-\Pi_{i,t}^{PC,operating}.
$$

Positive values indicate operating profit above the benchmark; negative
values indicate operating profit below it. Cumulative strategic performance
sums these monetary differences over the relevant recorded periods:

$$
A_{i,cumulative}=\sum_t A_{i,t}.
$$

This comparison does not establish that one student's decision caused the
difference, or that the student's strategy was optimal. The counterfactual
changes the whole market. Investment capacity charges are excluded from
this operating-profit comparison. The symbol $A$ distinguishes this
analytical indicator from the scenario-dependent leaderboard score $S$ in
Section 13.

## 12. KPI definitions and aggregation

| Indicator | Definition |
|---|---|
| Offered quantity | Sum of a generator's or company's submitted block quantities |
| Dispatch | Sum of accepted supply quantities, in MW |
| Dispatched energy | Dispatch multiplied by the one-hour period duration |
| Served demand | $d_{IN,t}+\sum_l d_{l,t}$ |
| Demand served (%) | $100(d_{IN,t}+\sum_l d_{l,t})/D_t^0$ |
| Generator utilization (%) | $100p_{g,t}/P^{max}_{g,t}$ |
| Revenue | MCP multiplied by dispatched energy |
| Variable production cost | Effective marginal cost multiplied by dispatched energy, summed over generators |
| Operating profit | Revenue minus variable production cost |
| Net profit | Operating profit minus applicable Investment capacity charges |
| Operating profit per MW of capacity | Operating profit divided by effective period portfolio capacity |
| Strategic performance | Actual minus PC operating profit, in euros |
| Difference versus PC per MW capacity | Actual minus PC capacity-normalized rates, as defined below |
| Cumulative operating profit | Sum of operating profits over the included periods |

These definitions describe the available calculations; not every diagnostic
appears on every screen. Capacity, offered quantity, and dispatch are distinct
and should not be used interchangeably.

### 12.1 Profit per MW of capacity

The reported normalized rate divides operating profit by effective portfolio
capacity for the period, rounded to two decimals. This keeps actual and
perfect-competition outcomes comparable even when dispatch is below available
capacity.

For generator or GENCO $x$ in period $t$, let $K_{x,t}$ be effective capacity:

$$R^{cap}_{x,t}=R_2\left(\frac{\Pi_{x,t}^{operating}}{K_{x,t}}\right),\qquad
R^{cap,PC}_{x,t}=R_2\left(\frac{\Pi_{x,t}^{PC}}{K_{x,t}}\right).$$

The signed difference used by the GM analytical KPI is calculated from the
underlying profits and rounded once:

$$\Delta^{cap}_{x,t}=R_2\left(\frac{\Pi_{x,t}^{operating}-\Pi_{x,t}^{PC}}{K_{x,t}}\right).$$

Leaderboard scoring is denominated in euros and defined separately in
Section 13; it does not sum these normalized KPI values.

With positive effective capacity, zero dispatch yields zero operating profit
and therefore a zero capacity-normalized rate. Both company and generator KPI
tables use the capacity-based definition. Independently rounded displayed
rates can differ by 0.01 from the normalized difference calculated from
underlying profits. Missing values are unavailable rather than imputed zeros.
Some interface columns retain the label "/MWh"; their implemented denominator
is effective available capacity, as defined here.

For example, zero actual operating profit compared with PC operating profit
of 16,000 € on 400 MW produces $\Delta^{cap}=-40$ €/MW of capacity.

### 12.2 Company rates versus generator rates

A company's rate uses its **total operating profit divided by total effective
capacity** for the period. It is not the sum or simple average of generator
rates. Before rounding, it is a capacity-weighted average of those rates.
Actual and PC calculations use the same effective capacities. A company's
comparison therefore need not equal an individual generator's comparison,
even when the monetary totals reconcile.

## 13. Leaderboard methodology

The current-period leaderboard ranks GENCOs by their euro score $S_{i,t}$ in
descending order. Higher signed values rank higher. The score uses the
scenario-specific formula in Section 13.1; the capacity-normalized analytical
KPIs in Section 12 remain available separately.

Cumulative ranking sums the euro scores:

$$
S_{i,cumulative}(T)=
\sum_{t\in\mathcal{R}(T)}S_{i,t},
$$

where $\mathcal{R}(T)$ contains released periods of the same game through the
selected period $T$, including earlier phases, whose cumulative-leaderboard
inclusion setting is enabled. The GM can exclude eligible periods for every
student, for example to keep a practice round out of the cumulative score.
Exclusion does not change accounts, market results, or current-period rankings.

This is a sum of period scores. It is not an average or a rate calculated by
dividing cumulative profit by cumulative energy. Normal and LNG scores use
operating profit. Investment scores use net profit after capacity charges
relative to the individual passive benchmark described below.

Ties use standard competition ranking: **1, 1, 3**. Missing comparisons are
unranked and placed after ranked entries. Zero actual dispatch does not by
itself prevent ranking when the required financial and benchmark results are
available.

In the current implementation, a company missing a comparison for any included
released period has no cumulative score. This can affect late entrants who
have no results for earlier periods.

The current-period score is defined once clearing is complete; cumulative
scores include released periods only. The information made available to
students is described in Section 19. To select scoring rounds, use the
[leaderboard controls](GM_INTERFACE_GUIDE.md#leaderboard).

### 13.1 Euro score by scenario

For Normal and LNG periods, the leaderboard score is the student's actual
operating profit minus the operating profit from a full-capacity,
perfect-competition dispatch using the same saved demand and effective costs:

$$
S_{i,t}=\Pi^{actual,operating}_{i,t}-\Pi^{PC,operating}_{i,t}.
$$

For Investment periods, the benchmark is an individual passive counterfactual.
It keeps the student's original game-start generators, makes no new capacity
purchases, offers that baseline at effective marginal cost, and leaves all
other students' saved offers and investments unchanged:

$$
S_{i,t}=\Pi^{actual,net}_{i,t}-\Pi^{passive}_{i,t}.
$$

The passive benchmark uses the same market-clearing engine and saved demand.
It is persisted when the period clears, so it is not redrawn after a restart.
Later Investment phases retain the same original baseline. A positive score
means the student's recorded outcome exceeded the selected benchmark; it is
not a claim that a different bid would necessarily have produced that score.

### 13.2 Why use an individual passive benchmark for Investment?

The benchmark asks: **Given what the other students actually did, did this
GENCO earn more through its bidding and investment decisions than it would
have earned by keeping its original portfolio and participating passively?**

This gives each student a reference tied to their own starting assets and the
market conditions they faced. The educational objective is to examine the
profitability of their decisions. Purchasing capacity earns no score by
itself: the resulting revenues must cover production costs and the applicable
capacity charges sufficiently to outperform passive participation.

For each student and Investment round, the backend constructs a separate
hypothetical market. That student's purchased generators are excluded, and
their original generators offer their full baseline capacity at the effective
marginal costs for that round. Every other student's actual offer blocks and
invested capacity remain as recorded, including saved missing-bid defaults.
Demand and the period's cost conditions stay fixed. Holding classmates'
decisions fixed anchors the calculation to the decisions actually made in
that classroom round; it does not assume how they might have reacted to a
different investment decision.

The backend clears this hypothetical market again. Removing purchased capacity
and changing offers can affect the clearing price and dispatch, so passive
profit uses the hypothetical market's own price and accepted quantities.
Passive profit is revenue minus production costs, with no charges for new
capacity because the passive student purchases none. Actual net profit
includes the student's applicable investment capacity charges.

A positive Investment score means actual net profit exceeded passive profit;
a negative score means it fell short. A student who buys no capacity and
offers the original portfolio in full at effective marginal cost should have
a score of approximately zero, subject to numerical precision. The score
evaluates bidding and investment together. It cannot attribute the difference
solely to an investment decision or identify an optimal strategy.

**Accumulating scores across rounds** lets the GM assess a sequence of
decisions over the experiment. Capacity can earn different returns as demand,
costs, and classmates' offers change. An investment can fall behind the
passive outcome in one round and exceed it in a later round. The cumulative
leaderboard adds those signed euro differences across the selected released
rounds, including applicable capacity charges in each Investment round. It
does not guarantee that investing will pay off. The original passive baseline
is retained across later Investment phases, and the GM can exclude practice
rounds through the cumulative-period controls.

## 14. Normal Market and LNG Crisis assumptions

### 14.1 Normal Market

Normal Market uses assigned portfolios and stored base marginal costs, without
an LNG adjustment or a new Investment-phase capacity charge. Actual clearing
uses student offers; the PC benchmark uses full effective capacity and cost.

### 14.2 LNG Crisis

LNG Crisis applies a common period-specific adjustment to gas generators:

$$
c_{g,t}=c_g^{base}+\Delta_t^{gas}.
$$

For nuclear, coal, and oil generators, marginal cost remains unchanged:

$$
c_{g,t}=c_g^{base}.
$$

Original cost differences between gas generators are retained. Their capacities
do not change. The default adjustment schedule is:

| Period | Gas-cost adjustment |
|---|---:|
| 1 | +0 €/MWh |
| 2 | +0 €/MWh |
| 3 | +40 €/MWh |
| 4 | +100 €/MWh |
| 5 | +160 €/MWh |
| 6 | +60 €/MWh |

The GM may change the adjustment and announcement before bidding opens.
Opening bidding fixes both for that period. Later configuration changes do
not revise previous results.

The adjustment affects portfolio costs, production costs, and PC offers.
Actual clearing continues to use student bid prices. The gas-cost adjustment
itself does not change demand or available capacity. Optional growth can operate
alongside the gas-cost shock when explicitly enabled by the GM.

This is a generation-cost shock, not a physical LNG model. Heat rates, fuel
conversion, plant efficiencies, and LNG trading are not represented.

New crisis news is revealed only when the current period becomes available
and its gas adjustment is positive. Future announcements are hidden.
Previously revealed news remains accessible in history.

### 14.3 Narrative context and student information

Crisis news gives a shared narrative for the current cost shock while each
student's portfolio shows their own effective costs. This supports discussion
of why an unchanged coal or nuclear generator may earn a different return
when gas costs rise. Future settings are withheld so students encounter the
scenario progressively.

Custom GM announcements can provide further classroom context. Their text
has no numerical effect on demand, costs, capacity, or the clearing model.
Publishing a statement does not change those inputs; the saved market
configuration remains authoritative. For publication and editing, use the
[announcement controls](GM_INTERFACE_GUIDE.md#14-publish-student-announcements).

## 15. Capacity Investment assumptions

An Investment phase has an investment stage followed by six market periods.
Students choose additional capacity by technology. Each positive technology
purchase creates a new generator; it does not enlarge an existing generator.

The teaching purpose is to move from operating an assigned fleet to choosing
its size and technology mix. Extra capacity can earn revenue but can also
change the market price and leave capacity unused. Recurring charges give
both the amount invested and the resulting market conditions financial
consequences. Investment is assessed through profitability, not a reward
for expanding supply alone.

For the standard initial baseline of 400 MW:

$$
C_i^{new,max}=5\times400=2{,}000\text{ MW}.
$$

A student starting with only that portfolio can therefore reach 2,400 MW in
total. This is not a universal total-capacity ceiling for every inherited
portfolio or sequence of Investment phases; the additional-purchase limit is
based on the eligible original baseline, excluding earlier acquired capacity.

There are no fixed purchase increments, loans, financing budgets, or upfront
payments. A zero-investment decision is valid. Each student's technology
offers have marginal costs sampled and stored when the investment phase is
initialized, using the technology cost ranges in Section 3. Students see those
costs before committing to a quantity. Editing a quantity does not redraw them.

Finalization creates the purchased generators and fixes the investment
decisions. They become available for actual bidding, subject to students'
offered quantities, and enter the PC benchmark at full effective capacity.

### Investment demand reference

Purchases add supply; they do not automatically create more demand. The demand
rule depends on whether the GM has enabled the extra growth option.

**With demand growth switched off**, demand ordinarily uses the total installed
capacity recorded when the Investment phase is initialized, before new purchases:

$$
D_t^0=\alpha_t C_{ref}^{pre-investment}.
$$

The GM can edit $\alpha_t$ before or during open bidding, until closure. Late
admission has a separate capacity-scaled recalculation rule in Section 16.

**With optional demand growth switched on**, saved absolute demand quantities
follow Section 5.4. A newly appended Investment phase continues from the previous
phase's final demand, with a further growth draw for Investment Period 1 and
each later period. It does not reset demand to a new percentage of the enlarged
fleet. Purchases and late admission increase supply without rescaling the saved
growth path, and the GM cannot edit individual growth-managed periods.

Growth can therefore increase pressure on the system if insufficient capacity
is added. Whether capacity is actually scarce, and whether an investment earns
enough to cover its charges, still depends on the baseline, realized growth,
available supply and submitted offers.

### Capacity charges and net profit

Acquired capacity is charged once per market period of its Investment phase,
regardless of dispatch:

| Technology | Capacity charge |
|---|---:|
| Nuclear | 32 €/MW per period |
| Coal | 16 €/MW per period |
| Gas | 5 €/MW per period |
| Oil | 2 €/MW per period |

Let $k_g$ be the applicable technology charge in €/MW per period, and
$P_g^{new}$ the acquired capacity in MW. Then:

$$
I_{g,t}=k_gP_g^{new},\qquad
I_{i,t}=\sum_{g\in G_i^{new}}I_{g,t}.
$$

GENCO net profit is:

$$
\Pi_{i,t}^{net}=\Pi_{i,t}^{operating}-I_{i,t}.
$$

For example, 100 MW of new gas capacity incurs 500 € per Investment market
period, including a period with zero dispatch. These are simplified teaching
charges, not annualized capital costs or an upfront purchase payment.
They are excluded from operating profit and deducted once to obtain actual
net profit. The Investment score uses that net profit in Section 13; it does
not deduct the same charge a second time.

## 16. Enrollment changes and their economic effects

Initial enrollment determines the GENCOs receiving starting portfolios.
Late requests remain pending until GM approval; a request alone does not
alter supply or demand.

Approval is permitted between periods, after active results have been released,
and only if a future waiting period exists. Each approved student receives a
new 400 MW portfolio and becomes active from the next waiting period.
Existing portfolios and historical results remain unchanged.

**With demand growth switched off**, eligible future waiting periods whose
demand is unlocked are recalculated using:

$$
D_t^{0,new}=\alpha_t C_{system}^{updated},
$$

with the saved $\alpha_t$ and updated total installed capacity. This also applies
to eligible Investment periods, overriding their ordinary pre-investment
reference for those recalculated quantities. The GM is informed which periods
were affected.

**With optional demand growth switched on**, approval leaves the saved absolute
demand quantities and growth rates unchanged. The newcomer increases available
supply without creating additional demand; demand relative to capacity can
therefore fall. Capacity-scaled recalculation must not be assumed for these
growth-managed periods.

Late students do not receive retrospective bids, profits, or benchmark results.
Their absence from earlier results may leave their cumulative leaderboard score
unavailable.

## 17. Missing-bid fallback

The administrative missing-bid fallback creates one block for each generator
without any saved offer when closure with defaults is selected:

$$
q_{g,1,t}^{default}=P_{g,t}^{max},\qquad
b_{g,1,t}^{default}=0.
$$

Existing submitted blocks are retained. A deliberately submitted zero-MW
offer counts as a submission and is not replaced.

Defaulting a missing offer at zero changes actual market supply; it is not
an imputation of the student's intended strategy. The generator still incurs
its effective marginal cost if dispatched and can make a loss. The PC
benchmark continues to include full capacity at effective marginal cost.

For classroom interpretation, distinguish an intentionally submitted offer
from one inserted through this administrative fallback.
The [closure instructions](GM_INTERFACE_GUIDE.md#close-bidding) describe how
to wait for submissions or accept the fallback.

## 18. Portfolio inheritance and experiment continuity

Appending a phase preserves the same participants, portfolios, and cumulative
game history. New scenario conditions apply prospectively; completed results
are not recalculated.

Creating an independent game from a released-period snapshot copies eligible
portfolio ownership, capacities, technologies, and base costs, with optional
destination-only base-cost overrides. The source game may still be ongoing.

The independent game has fresh auctions and financial history. Earlier bids,
profits, and temporary LNG adjustments are not copied. Previously acquired
generators become inherited capacity without reapplying their earlier
acquisition charges. Their capacity does not enlarge the original baseline
used to calculate the additional-investment limit.

These operations support different teaching designs: continuation within one
cumulative history, or a separate experiment starting from a chosen portfolio.
They do not guarantee identical results, because demand settings and student
offers may differ.

## 19. Information, release, and confidentiality

The GM controls auction timing, scenario configuration, clearing, and results
release. Clearing makes an outcome available for GM inspection; release makes
it available to students.

Students can inspect their own effective generation costs before bidding.
Future LNG settings are not disclosed in advance. After release, students see
their own financial outcomes and block-level results.

The student order book may display anonymous market blocks, without other
students' identities, generator associations, or generator totals.
The leaderboard separately discloses names and ranks, but not numeric scores
or other students' financial results. The GM can inspect named bids after
clearing.

Notifications, news history, and screen navigation communicate game state.
They do not change auction inputs, allocations, or profit formulas.
Opening a notification is not evidence of submitting a decision.

## 20. Market Coach: purpose, indicators, and interpretation

Market Coach connects a student's recorded offers with their dispatch and
financial accounts. Its teaching purpose is to help students distinguish
offer prices from production costs, installed capacity from accepted supply,
and operating profit from net profit. It gives observations for discussion,
not a recommended bidding strategy or an assessment of bidding skill.

### 20.1 Availability and selection of observations

Feedback uses the student's own released records and appears only when the
per-game option is enabled. The
[Market Coach controls](GM_INTERFACE_GUIDE.md#13-enable-market-coach) explain
how to enable it and where students find it.

The Julia backend applies fixed rules to the student's own saved results.
With sufficient source data, it returns two core observations and at most
one additional observation, in this order:

| Observation | When it appears | Teaching purpose |
|---|---|---|
| Offers and dispatch | Every eligible released result | Relate offer markups, offered quantity, dispatch, and unused capacity |
| Operating outcome | Every eligible released result; capacity charges are added when positive | Reconcile revenue, variable costs, operating profit, and net profit |
| Gas-generation cost shock | An LNG Crisis period with a positive saved adjustment on at least one of the student's own gas generators | Explain the student's period-specific costs and their accounting effect |
| Previous-period comparison | Only if the gas observation is absent and the comparison conditions below are met | Describe changes in the student's own outcomes across released periods |

The gas observation takes priority over the previous-period comparison, so
students receive **two or three observations**, rather than an exhaustive
list of every possible statement. If necessary source results are missing,
the coach does not invent values or substitute another student's results.
No AI, random selection, or external analysis service is used.

### 20.2 Offer markups and below-cost offers

For GENCO $i$, let $G_i$ contain its generators available in period $t$.
Using the notation from Section 4, total offered quantity is:

$$
O_{i,t}=\sum_{g\in G_i}\sum_k q^{bid}_{g,k,t}.
$$

When $O_{i,t}>0$, the quantity-weighted offer markup is:

$$
\overline{m}_{i,t}=
\frac{\sum_{g\in G_i}\sum_k
q^{bid}_{g,k,t}\left(b_{g,k,t}-c_{g,t}\right)}{O_{i,t}}.
$$

Here, $c_{g,t}$ is the generator's **effective marginal cost for that period**,
including an applicable LNG adjustment. The indicator is measured in €/MWh
and weights **offered quantities**, not dispatched quantities. Zero-MW blocks
have no weight. If all saved quantities are zero, the coach states that no
quantity-weighted markup can be calculated; it does not report a fabricated zero.

For example, a generator with marginal cost 80 €/MWh offering 30 MW at
90 €/MWh and 70 MW at 120 €/MWh has a weighted markup of:

$$
\overline{m}=\frac{30(90-80)+70(120-80)}{100}=31\text{ €/MWh}.
$$

The coach also totals the positive offered quantities whose prices are below
their generators' effective marginal costs:

$$
O^{below}_{i,t}=
\sum_{\substack{g\in G_i,\,k:\ b_{g,k,t}<c_{g,t}}}q^{bid}_{g,k,t}.
$$

This figure describes **offers**, not necessarily accepted energy or losses.
In a uniform-price market, an accepted offer is paid the clearing price,
which can exceed both the bid and marginal cost. Consequently, a below-cost
bid does not by itself establish a negative production margin. A positive
average markup also does not prove that every block was offered above cost.

These calculations include the period's recorded offers, including any
administrative missing-bid defaults. The coach does not infer whether a
zero-price offer was a deliberate strategy or a missing-bid fallback.

### 20.3 Dispatch, utilization, and unused capacity

Define effective portfolio capacity and actual portfolio dispatch as:

$$
K_{i,t}=\sum_{g\in G_i}P^{max}_{g,t},\qquad
p_{i,t}=\sum_{g\in G_i}p_{g,t}.
$$

For positive capacity, utilization, unused capacity, and unoffered capacity are:

$$
u_{i,t}=100\frac{p_{i,t}}{K_{i,t}},\qquad
U_{i,t}=\max(K_{i,t}-p_{i,t},0),\qquad
W_{i,t}=\max(K_{i,t}-O_{i,t},0).
$$

For example, a 400 MW portfolio offering 300 MW and dispatching 180 MW has
45% utilization, 220 MW unused, and 100 MW unoffered. The unused total includes
both unoffered capacity and offered capacity that did not clear.

Unused capacity alone does not identify the cause of low dispatch or prove
that offering more would have increased profit. Different offers can change
the clearing price and the allocation of dispatch. No alternative clearing
is performed by Market Coach.

### 20.4 Operating profit, negative margins, and investment charges

The operating observation reconciles the accounts defined in Section 9.
It identifies a generator with a negative realized production margin
when it has positive dispatch, a known clearing price, and:

$$
\lambda_t<c_{g,t}.
$$

For example, 50 MWh sold at 70 €/MWh by a generator costing 90 €/MWh yields
3,500 € revenue, 4,500 € variable cost, and an operating loss of 1,000 €.
Only the student's own affected generators are named. A company can have
positive total operating profit while an individual generator makes a loss.
With zero dispatch, operating revenue and variable generation costs are zero.

When an Investment capacity charge is positive, the same observation also
reports the stored charge and net result using Section 15's accounting.

For example, 2,000 € operating profit with a 3,000 € capacity charge gives a
net loss of 1,000 €. Charges apply to eligible acquired capacity even when
unused, so zero dispatch need not imply zero net loss. The coach describes
these accounts; it does not change the leaderboard calculation in Section 13.

### 20.5 LNG cost shocks: accounting effect versus counterfactual effect

For a student's gas generator, the coach uses the frozen base and effective
costs for the selected period:

$$
c_{g,t}=c_g^{base}+\Delta_{g,t}^{gas}.
$$

It reports those costs and the positive adjustment for the student's affected
gas units. At the **recorded actual dispatch**, the adjustment's contribution
to gas production costs is:

$$
C_{i,t}^{shock}=\sum_{g\in G_i^{gas}}
\Delta_{g,t}^{gas}E_{g,t}.
$$

For an 80 €/MWh gas unit with a +100 €/MWh adjustment and 50 MWh dispatched,
effective marginal cost is 180 €/MWh and the adjustment accounts for 5,000 €
of its recorded variable cost. An undispatched gas unit has zero such cost
contribution despite its higher marginal cost.

This is an **accounting decomposition at observed output**, not an estimate
that the student would have earned 5,000 € more without the crisis. Removing
the shock could change offers, dispatch, and the market price. Market Coach
does not simulate that alternative market. It also does not model a physical
LNG fuel price or fuel-to-electricity conversion.

No gas observation appears for a zero adjustment or for a student without an
affected gas generator. Historical feedback uses that period's saved cost
snapshots, not the current or a future period's configuration.

### 20.6 Comparison with the student's previous period

When no gas observation is selected, the coach considers the student's most
recent earlier **released** result in the **same phase**. It compares only
when total effective capacity is numerically unchanged. Let $s$ denote that
earlier period. The changes are:

$$
\delta\Pi_{i,t}=\Pi_{i,t}^{operating}-\Pi_{i,s}^{operating},\qquad
\delta u_{i,t}=u_{i,t}-u_{i,s}.
$$

An observation is included if either the absolute operating-profit change is
at least 1 € or the absolute utilization change is at least 1 percentage point.
The utilization change is a **percentage-point difference**, not percentage
growth relative to the earlier utilization.

For example, moving from 25% to 50% utilization is an increase of 25 percentage
points. The coach names the earlier period and reports the observed changes.
It does not attribute them solely to improved or worsened bidding: demand,
costs, and the rest of the market can also change between auctions.

There is no comparison across phases, with future or unreleased results, or
with another student. A first released result may therefore have no trend
observation. The comparison uses released history independently of whether a
period is included in the cumulative leaderboard; excluding a practice round
from ranking does not erase its results.

### 20.7 Numerical conventions, confidentiality, and teaching limits

Coach figures are formatted to at most two decimal places. A small numerical
tolerance of $10^{-7}$ is used when testing positive quantities and below-cost
or negative-margin conditions; effectively zero quantities are not weighted.
Capacity equality for the trend rule uses a numerical approximate-equality
check. These presentation rules do not revise stored accounts or KPI formulas.

Feedback is calculated on reading the results using fixed rules. For the same
saved data and setting, the observations are deterministic. The student's
authenticated session determines whose accounts are read. Coaching text does
not expose another student's offers, generator ownership, portfolio, profit,
identity, or leaderboard position. Ordinary result-graph and leaderboard
visibility remains as described in Section 19.

Market Coach supplies no optimal offer, profit forecast, counterfactual market
result, or personalized ranking explanation. It does not change bids, demand,
clearing, settlement, or scores. The GM can use the observations as prompts
for discussing the accounts and graph, rather than as a verdict on strategy.

## 21. Interpretation and model boundaries

Normal Market illustrates the effects of prices and offered quantities under
stable generation costs. LNG Crisis introduces a controlled gas-cost shock.
Capacity Investment introduces persistent supply expansion with explicit
period charges.

The game is a simplified teaching model rather than a complete representation
of an operating electricity market. It excludes transmission congestion and
losses, start-up and shutdown costs, unit commitment, ramping, minimum stable
generation, minimum up/down times, storage, balancing, ancillary services,
fuel hedging, bilateral contracts, and capital financing.

Equal initial capacity and portfolio screening do not guarantee equal economic
opportunities. Benchmark differences do not establish individual causality.
Scarcity, market power, and welfare should therefore be discussed with
reference to the saved demand, available supply, actual offers, and the
counterfactual used.

For classroom procedures and troubleshooting, see the
[GM Interface Guide](GM_INTERFACE_GUIDE.md).
