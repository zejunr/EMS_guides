# Energy Market Game

## A teaching and practical guide for Game Masters

**Audience:** professors, lecturers, and teaching assistants running a classroom game.
**Edition:** 16 November 2026 — browser and iOS platform, including three-block bidding
and cumulative leaderboard period selection.

This guide explains what each Game Master (GM) feature is for, the learning
activity it supports, and how to use it in class. Start with the teaching
question, then choose the controls that help students investigate it.
No command-line or programming knowledge is required to run a session on an
already configured server. The platform administrator supplies your website
address and GM account. The companion [economic reference](GM_GUIDE.md) gives
the formal assumptions and KPI formulas.

## 1. What the game does

Each student or team manages a generation company, called a **GENCO**. They
inspect their generators, submit electricity supply offers, and receive results
after each auction. As GM, you decide when enrollment, bidding, investment, and
results become available.

The purpose is to connect individual decisions with system-wide outcomes.
Students can examine how an offer affects the chance of dispatch, why the
market price differs from a generator's cost, and why a high selling price
does not necessarily produce a high profit. Repeated periods let them revise
their decisions after seeing evidence.

The game supports three scenarios:

| Scenario | Classroom focus | Additional GM controls |
|---|---|---|
| Normal Market | Establish how offers, demand, dispatch, and uniform pricing interact | Demand settings |
| LNG Crisis | Explore how a gas-cost shock can change the merit order and outcomes for gas and other generators | Gas-cost adjustment and market news for each period |
| Capacity Investment | Explore the trade-off between more selling capacity, market conditions, and recurring capacity charges | Open, close, and finalize an investment window |

**Demand growth is an extra option, switched off by default**, that the GM can
enable within any of these scenarios. It introduces 3% expected growth with
random variation; it is not a fourth scenario. See Section 7.

A **game** contains participants and their history. A **phase** is a sequence
of auctions under one scenario. A **period** is one auction. New phases use
six periods; period numbering starts again at 1 in each phase.

Use phases to develop a teaching sequence within one class, and independent
games to compare separate experiments. The available features support
discussion; the leaderboard alone cannot explain why a strategy succeeded.

## 2. Before the class

Choose the question students should investigate before choosing the scenario.
For example, use Normal Market to introduce the difference between offered
quantity and dispatch, LNG Crisis to study a cost shock, or Capacity Investment
to discuss whether extra capacity earns enough to cover its charge. A short
practice round helps students learn submission and results navigation before
you interpret their performance.

Obtain these from the platform administrator:

- The **GM website address**, ending in `/admin`.
- Your GM account or account-creation invitation.
- The **student website address**, ending in `/play`, and any iOS installation instructions.

Confirm the website works on the network your students will use. Run a short
practice game before the first class. A browser is sufficient for students;
the iOS application is another way to access the same game when configured
for the same server.

For an initial session, Normal Market with student self-registration is a
straightforward starting point. Decide in advance how you will handle absent
students and missing bids.

## 3. Sign in and find your way around

The dashboard separates managing the live auction from examining its history.
This lets you discuss a completed period while retaining control over the
next classroom activity.

Open the supplied `/admin` address and select **Sign in** using your GM
credentials. Accept an account invitation first if the administrator sent one.
The separate `/platform-admin` website manages GM accounts; it is not needed
for everyday teaching.

| Interface area | What you use it for |
|---|---|
| Game sessions sidebar | Select an existing game, or choose **+ New game** |
| Game header | Check the game name, status, and shared Game Code |
| Suggested next action | Advance the live game, for example opening bidding or releasing results |
| Enrollment panel | Check participants, recover codes, and correct registrations while enrollment is open |
| Game progress | Select a phase, then a period |
| Scenario panel | Configure LNG Crisis or manage investment, when applicable |
| Live bidding status | Check which companies have submitted complete bids |
| Advanced controls | Edit demand, access permitted manual actions, or delete a game |
| Results sub-tabs | Review **GENCO KPIs**, **Market graph**, and **Leaderboard** after clearing |

Selecting an old period only changes what you are viewing. It does not reopen
its bidding. Check the phase and period named in any confirmation before
advancing the live game.

Use the account button near the top of the page to change your password, or
**Sign out** to finish. Contact the platform administrator if you forget your
GM login; student Player Codes cannot recover a GM account.

## 4. Create a game

A separate game gives each class its own participants, portfolios, and result
history. Scenario selection determines the economic question being explored;
bid limits define the range of permitted offers. Explain those rules before
the first auction so students can make decisions with the same information.

1. Choose **+ New game**, or the **+** beside Game sessions.
2. Enter a descriptive **Game name**, such as `Energy Economics — Seminar 1`.
3. Select a **Market scenario**.
4. Choose how students will join, as described below.
5. Set the number or maximum number of players.
6. Review **Minimum bid** and **Maximum bid**. The default limits are
   0–4,000 €/MWh. These are offer-price limits, not guaranteed clearing prices.
7. Select **Create game**.

The game now appears in the sidebar with its own Game Code. Creating another
game creates a separate experiment; it does not erase earlier games.

### Option A: Student self-registration

Use this when students should join with their own display names and minimal
individual administration by the GM.

Choose **Student self-registration** and set **Maximum players**. Students
open the student app, enter the Game Code and their chosen display name, and
receive a private Player Code. Ask them to save it for signing in again.

### Option B: Player Codes from the GM

Use this when you want to prepare a fixed participant or team list and control
which invitation is assigned to each group.

Choose **Player Codes from the GM**. Specify the number of players, or enter
one custom player/team label per line. Custom labels replace the automatically
generated labels. Save the invitation list and distribute each private code
only to its intended student or team.

## 5. Enroll students and assign portfolios

Enrollment establishes who participates before supply and demand are
initialized. Equal initial total capacity gives students the same starting
scale, while different technologies and costs create different economic
positions. This allows discussion of why equal capacity does not mean equal
dispatch or equal profit opportunities.

Share the student URL or QR code and the **Game Code**. The Game Code identifies
the class and may be displayed publicly. A **Player Code** identifies a student
and must remain private. Do not project a screen or CSV containing those codes.

Watch the joined count and participant list. Before enrollment closes, use:

- **Edit name** to correct a display name.
- **Remove** to delete an incorrect registration so the student can register again.
- **Regenerate code**, when offered for an unjoined invitation, to replace that invitation's code.
- **Download current codes CSV**, when available, to keep a recovery copy securely.

When the intended students have joined, select the suggested enrollment-closing
action and confirm. Closing enrollment assigns each participating GENCO an
initial **400 MW portfolio**, split across one to three generators. Students
can then inspect their capacities and marginal costs. Different portfolios
can have different technologies and costs despite having equal total capacity.

Ordinary registration editing/removal is restricted after this step. Use the
late-registration procedure in Section 11 for students who missed enrollment.

## 6. Run each bidding period

The separate controls give you time to explain the situation, collect
decisions, and inspect outcomes before discussing them with the class.

The usual sequence is:

**Configure → Open bidding → Monitor → Close bidding → Clear market → Review → Release results**

### Configure

Use configuration to define the experiment for this auction. Demand affects
how much electricity buyers are willing to purchase; scenario settings affect
the relevant costs or installed capacity.

Select the intended phase and period. With growth off, review or edit demand
under **Advanced controls**. With the extra growth option on, review the saved
**Demand growth** table instead; individual demand editing is unavailable.
For LNG Crisis, also save the period's gas-cost adjustment and announcement
before opening bidding. Complete the investment stage first in Investment mode.

### Open bidding

Opening bidding starts the decision stage. Students can experiment with both
the price they request and the quantity they offer. Multiple blocks let them
offer different parts of one generator's capacity at different prices; offering
less than capacity allows them to explore physical withholding.

Use the suggested **Open bidding** action and confirm. Students can enter and
revise their offers until bidding closes. You can close manually or set an
optional timer in **Period schedule**. Students see an active timer's deadline
in their own device's timezone.

Each generator can offer one to three quantity–price blocks. Every block needs
a price, even at zero quantity. The combined quantity cannot exceed available
capacity, but students may deliberately offer less. Blocks are cleared by
price, regardless of their displayed order.

For example, a 200 MW generator may offer 80 MW at 120 €/MWh and 100 MW at
90 €/MWh, leaving 20 MW unoffered. The cheaper block is evaluated first.

### Monitor submissions

Submission status helps you distinguish a saved decision from a student who
is still working or has not submitted. It supports classroom pacing without
revealing named offers before the auction clears.

Use **Live bidding status** to see which companies are complete. Status refreshes
periodically, so allow a few seconds after a student submits. Typing into an
input does not save an offer; the student must submit it. A submitted zero-MW
offer is a valid decision and counts toward completion.

### Close bidding

Closure creates a common decision deadline and freezes the auction inputs.
The missing-bid confirmation gives you a deliberate choice between waiting
for participation and proceeding with the announced fallback rule.

Select **Close bidding** and confirm. Students can no longer edit offers.
For capacity-scaled demand, closing also locks the period demand.
A growth-managed period already uses the saved, individually noneditable path.

If bids are missing, a warning identifies the incomplete companies. Choose:

- **Cancel:** keep bidding open and ask students to submit.
- **OK:** close bidding and insert a full-available-capacity offer at
  **0 €/MWh for every generator without a saved bid**.

Existing submitted blocks, including intentional zero-MW offers, are retained.
Explain the fallback rule before class: it affects supply and prices, and a
defaulted generator can lose money when the clearing price is below its cost.

The fallback keeps the auction running; it is not a neutral replacement for a
student's decision. It can affect other players' dispatch and profit as well
as the defaulted player's outcome. The perfect-competition benchmark still
uses effective marginal costs, so interpret comparisons in light of any
fallback offers.

### Clear, review, and release

Separating clearing from release gives you a pause to check the outcome and
prepare a discussion. You can identify an illustrative generator or offer
before students receive their results.

Select **Clear market**. The system calculates actual and perfect-competition
outcomes. Clearing makes results available to you; it does not yet publish
them to students.

Review the results tabs, then select **Release results**. Students can now see
their released results and updated leaderboard. Open the next period when
ready and repeat the sequence.

| Period status | Meaning |
|---|---|
| Waiting | Bidding has not opened |
| Bidding | Students may submit or revise offers |
| Bidding closed | Offers and demand are locked; ready for clearing |
| Clearing | The market calculation is in progress |
| Cleared | The GM can inspect results; students are still waiting for release |
| Results released | Students can view the period's results |

Wait for an action to finish and the status to update before repeating it.

### Optional automatic progression

**Purpose:** keep a class moving through a planned series of auctions without
requiring the GM to close, clear, release, and open every period individually.
Leave timers off when you want time for discussion or manual review before release.
Existing games have no timers until you save one.

1. Select the phase in **Game progress**, then use the separate **Period schedule** panel.
2. Check **Display timezone**, initially your PC timezone. You can enter another
   IANA timezone, such as `Europe/Athens`. Changing this display does not move an
   existing deadline; it shows the same instant in the chosen timezone.
3. Enable **Use timer** only for the periods you want automated. Enter a separate
   clearing date/time for each. Turn it off for manual progression.
4. Save the schedule. Deadlines must be in the future and increase in period order.
   Ambiguous or nonexistent times at daylight-saving changes are rejected; choose
   another local time. Bid limits must allow the 0 €/MWh missing-bid fallback.
5. Review and save all upcoming LNG configurations. Automatic opening locks the
   next period's saved gas costs and announcement. Complete and finalize any
   Investment stage before opening its first bidding period.
6. Open the first period manually. Saving a timer does not open it.

At each active deadline, the server closes bidding, adds one full-effective-capacity
block at **0 €/MWh** for each generator without an offer, clears actual and
perfect-competition markets, releases results, and opens the next period **in
the same phase**. Existing offers, including intentional zero-MW blocks, are
preserved. The final period stops this sequence; a new phase is never created
automatically. An unscheduled next period can open automatically, but its closure,
clearing, and release remain manual. Previous released results, leaderboards,
and enabled Market Coach feedback remain available in student results history.

The panel shows each timer's state, last completed step, completion time, and
number of defaulted company submissions. Use **Cancel timer** to return a pending
or paused period to manual progression. Unchecking **Use timer** and saving has
the same effect. Closing bidding manually also cancels that period's timer;
it does not automatically clear or release results.

**Recovery:** the server must be running for timers to execute. While stopped,
they remain stored. After restart, deadlines that passed while it was stopped
become **Paused — GM action required**. The server does not automatically close,
clear, release results, or open the next period for those timers. The GM can
close, clear, release, and open the next period manually, or save a new future
deadline to resume automatic progression. Paused open periods accept bids until
the GM closes them or a newly saved deadline passes. Later deadlines are not shifted.

For example, if Period 1 was open with a 10:00 deadline, Period 2 was waiting
with an 11:00 deadline, and the server restarts at 12:00, both timers pause.
Period 1 remains open, and Period 2 remains waiting. The GM decides when to continue.
Timers with future deadlines remain enabled.

A transition that had already started before the interruption is marked failed
for GM review. Its completed steps remain saved. Select **Retry transition** to
resume it explicitly, or cancel the timer and finish using manual controls.

If a market operation fails, the transition stops and the panel shows the error.
Resolve the cause and select **Retry transition**. Recovery inspects saved market
state and resumes the first incomplete step, rather than duplicating bids or
results. A running transition cannot be edited or cancelled midway. Notification
delivery failures do not undo completed market actions. Maintenance mode suspends
timer execution; if the server stays running, due timers resume automatic
processing when maintenance ends. Restarting it uses the manual-review policy above.

## 7. Choose the demand settings

Demand controls let you change market tightness and explore how the same
generation fleet behaves under different buying conditions. The game offers
the usual capacity-scaled demand settings and an **extra demand-growth option**.
New games start with growth off; choosing LNG Crisis or Capacity Investment
does not enable it. Once the GM enables a path, later phases in that same game
continue it automatically.

### A. Demand growth switched off: edit each period's demand

Select a phase and period, expand **Advanced controls**, and find the demand
editor. Set **Demand fraction (alpha)**, review **LP increments**, and select
**Save demand**. Confirm that the saved demand values update.

Alpha scales maximum demand relative to the applicable capacity reference.
For example, alpha 0.80 with a 4,000 MW reference gives 3,200 MW maximum demand.
It does not fix the quantity ultimately traded: clearing also depends on offers
and willingness to pay. LP increments controls the resolution of the flexible
demand approximation; 20 is the default.

For these periods, demand is editable before or during open bidding and locks
when bidding closes. Investment purchases do not automatically enlarge its
reference. Approving late students separately recalculates eligible future,
unlocked capacity-scaled demand; review the GM confirmation.

### B. Optional demand growth: turn on an uncertain upward trend

Use this extra option when the class should face increasing pressure on
existing capacity, making the decision to invest meaningful. The choice of 3%
is a teaching calibration per period, not an annual electricity-demand forecast.

1. Close enrollment and generate portfolios.
2. Select the intended phase before its Period 1 opens bidding.
3. In **Demand growth**, choose **Baseline (% of starting capacity)** and review
   its MW value. This is demand before the first growth draw.
4. Select **Enable growth from Period 1** and inspect the saved rates and demand
   quantities. The option can be used in any of the three scenarios.

Each independent rate is drawn between -2% and +8%, whose midpoint is 3%.
Demand compounds from the previous quantity. Individual periods can shrink,
and the realized average is not forced to equal 3%. Both demand components grow
together; their valuation prices do not rise by 3%. The displayed maximum demand
also need not equal the quantity eventually traded.

The path is drawn once and saved. Adding another phase to the same game
continues from the preceding phase's final demand, with a further draw for the
new phase's Period 1. Investment and late admission add supply without increasing
the saved growth demand. This makes scarcity possible, not certain.

Individual demand editors are unavailable for growth-managed periods. Before
the first affected period opens, **Save baseline** can rescale the path while
keeping its saved rates, and **Restore previous demand schedule** switches
growth off. A baseline from an already released period remains fixed. Once the
first growth period opens bidding, the path is fixed. The saved table is
authoritative for older games whose first growth draw may be Period 2.

Both demand modes use the same 80% high-value / 20% flexible split and the same
period demand for actual and perfect-competition clearing. Students do not see
the future growth schedule. See the [economic reference](GM_GUIDE.md#54-optional-demand-growth-3-expected-increase)
for the growth formula and assumptions.

## 8. Run an LNG Crisis phase

Use this scenario to investigate how a change in one fuel's generation cost
can affect the entire electricity market. Students with gas capacity must
reconsider their offers; students with other technologies can observe changes
in their competitive position even when their own costs are unchanged.
Market news gives students a shared explanation for the current shock, while
their portfolios show the costs relevant to their own decisions.

The control represents an adjustment to gas-fired electricity generation
costs in €/MWh. It does not model a physical LNG price or convert a fuel price
into electricity costs.

The **Six-period crisis schedule** shows the phase's gas-cost adjustments.
Before opening each period:

1. Select that period.
2. Set **Gas marginal-cost adjustment**.
3. Edit **Student market news title** and **Student market announcement** if needed.
4. Select **Save crisis settings**.
5. Open bidding when ready.

The adjustment is added to every gas generator's original marginal cost.
For example, an 80 €/MWh gas generator with a +100 €/MWh adjustment costs
180 €/MWh in that period. Other technologies are unaffected.

The adjustment and announcement lock when bidding opens. Students receive
the current announcement only when the adjustment is positive; a zero
adjustment suppresses it. Future announcements are not revealed in advance.
Students see the effective generation costs needed to make their offers.

## 9. Run a Capacity Investment phase

Use investment to move from operating an assigned fleet to choosing its size
and technology mix. Students can investigate whether additional capacity is
worth acquiring when more supply can also change dispatch and market prices.
Recurring capacity charges make unused investments economically consequential.

After enrollment or phase initialization, use **Investment window**:

1. Select **Open investment window**.
2. Ask students to review technology costs and submit their desired additional capacities.
3. Review saved decisions, additional MW, and capacity charges in the GM table.
4. Select **Close investment window** when decisions are complete.
5. Review the decisions, then select **Finalize investments**.
6. Proceed with the six bidding periods using the normal controls.

Zero investment is valid. Finalization creates the purchased generators and
makes decisions permanent. Students see each technology offer's marginal cost
before confirming. The initial 400 MW baseline permits up to 2,000 MW of
additional purchases per student under the current rules.

Capacity charges apply to acquired capacity each Investment market period,
even without dispatch. Distinguish **Operating profit** from **Net profit**:
net profit subtracts those charges. The perfect-competition benchmark includes
the finalized additional generators.

## 10. Read and discuss results

The three results tabs answer different teaching questions: the KPI tables
show what each company earned, the graph helps explain how the market reached
that outcome, and the leaderboard compares performance against the benchmark.
Use them together when discussing a high or low score.

Select a cleared or released period under its phase, then choose a results tab.
Historical selection does not alter the recorded auction.

### GENCO KPIs

Company totals summarize the student's outcome; generator details help
explain its source. For example, a low-profit company might have offered too
little, failed to obtain dispatch, or dispatched a costly generator at a loss.
Comparing quantity offered, dispatch, price, and marginal cost helps separate
these explanations. In Investment mode, examine net profit as well as
operating profit to account for the capacity charge.

Use **Filter student / GENCO** to inspect the whole class or one participant.
The company table summarizes each GENCO; the generator table explains its
individual units. Summary panels follow the selected filter.

| Measure | Interpretation |
|---|---|
| Capacity | Available generation capacity |
| Quantity offered | Sum of submitted block quantities |
| Dispatch | Quantity actually accepted by clearing |
| Revenue | Clearing price multiplied by dispatched energy |
| Generation cost | Effective marginal cost multiplied by dispatched energy |
| Operating profit | Revenue minus generation cost |
| Capacity charge / Net profit | Investment charges and operating profit after those charges |
| Perfect-competition results | Alternative market using full capacity offered at effective marginal cost |
| Strategic difference | Actual minus perfect-competition operating profit, in euros |
| Difference vs perfect competition / MWh | Difference in operating profit normalized by effective period capacity; see the explanation below |

Each period uses a one-hour convention, so 1 MW dispatched is treated as
1 MWh for financial calculations. Revenue is therefore the clearing price
multiplied by dispatch, and generation cost uses effective marginal cost.

The normalized profit KPI uses **effective available capacity**, rather than
dispatched energy, as its denominator for both actual and perfect-competition
results. Some interface labels retain “/MWh”; the economic reference calls
this measure operating profit per MW of capacity. The company calculation
uses its total operating profit and total effective capacity. Generator
calculations use each generator's own profit and capacity.

For example, a company with 400 MW of capacity, actual operating profit of
32,000 €, and perfect-competition operating profit of 20,000 € has normalized
rates of 80 and 50 €/MW of capacity. Its comparison score is +30. Compute
the difference from the underlying profits and then round to two decimals;
subtracting independently rounded displayed rates can differ by 0.01.

Positive or zero comparison differences are green; negative differences are
red. With positive capacity, zero dispatch gives zero operating profit and
a zero normalized rate. Missing data remains unavailable. These comparisons describe
performance relative to a different market outcome; they do not prove that
one student's decision caused the difference.

### Market graph

Use the graph to connect the company accounts to the market mechanism.
Students can see why a lower-priced block clears before a higher-priced one,
where demand meets supply, and why an offer can remain unaccepted. Comparing
actual supply with perfect-competition supply illustrates the combined effect
of submitted offers relative to full-capacity offers at marginal cost.

Inspect demand, submitted supply, and clearing coordinates. Hover over, focus,
or tap an offer for details; the table lists the individual blocks. Zero-MW
blocks remain in the table but have no width on the curve.

Use the controls to show or hide demand, actual supply, player identities,
marginal costs, and perfect-competition supply. Zoom the axes and drag to pan;
reset the view to return to the full market. The perfect-competition curve
reveals cost ordering visually even if numeric cost labels are hidden.

Named bid inspection is available to the GM only after clearing. Students'
market graph is anonymous. Consider which information you want to project
during classroom discussion.

### Leaderboard

The leaderboard provides a relative-performance measure for repeated rounds.
Its purpose is to support reflection on decisions, alongside the graph and
accounts. A high score means a better outcome relative to this benchmark;
it does not, by itself, establish superior bidding skill. Portfolios, other
students' offers, and missing-bid defaults also affect outcomes.

Ranking uses the signed difference between actual and perfect-competition
operating profit divided by effective period capacity. Higher values rank higher.

- **Current period:** that period's difference.
- **Cumulative:** the sum of the differences for included, released periods
  through the selected point, including earlier phases of the same game.

Use **Cumulative ranking periods** to exclude a practice or otherwise
unsuitable round for every student. Select the eligible periods and save.
This changes cumulative ranking only: the excluded period's accounts,
current-period ranking, and auction results remain available. When viewing
an earlier period, later periods do not contribute to its cumulative score.
Explain the scoring-period selection to the class so everyone understands
which rounds count.

Ties use ranks 1, 1, 3. A cleared but unreleased period can appear in your
current-period ranking before it enters the released cumulative total.
Missing comparisons are unranked; late entrants without earlier results may
therefore have no cumulative rank.

Students see names and ranks, with a search function, but not the numeric
leaderboard scores or other students' financial results. They can revisit
their own released financial results by phase and period.

## 11. Admit a late student

Late registration lets you include an additional participant without restarting
the class. Approval between auctions gives the newcomer a portfolio for a
future period while preserving completed outcomes. With demand growth off,
approval recalculates eligible future capacity-scaled demand and identifies
the affected periods. With the extra growth option on, saved growth-demand
quantities stay unchanged: the new student adds supply without creating demand.

After normal enrollment closes, enable **Late registration** in the game
dashboard. The student requests entry through the student app and waits for
your approval.

Approve requests between periods, after the preceding results are released,
with a future waiting period still available. Approval is blocked while an
auction is bidding, closed, clearing, or awaiting results release.

Select **Approve** for each accepted student. The system assigns a new 400 MW
portfolio and activates it for the next waiting period. Multiple students
can be approved. Review the confirmation for any recalculated future waiting,
unlocked periods. Growth-managed demand is preserved rather than recalculated.
Previous results and existing portfolios are preserved in both cases.

Use **Reject** to decline a pending request. Late admission does not give the
student retrospective results or bids for periods already completed.

## 12. Continue or repeat an experiment

### Continue with the same students and cumulative history

Use another phase when students should experience a new scenario as a
continuation of the same game, retaining their participants and history.

After completing the latest phase, choose the next scenario and **Add another
phase**. Participants retain their codes and portfolios, and previous results
remain in the game. The new phase starts its displayed period count at 1.

If optional demand growth is enabled, its first period continues from the
previous phase's final demand with another growth draw. The demand path does
not reset when the scenario changes, and investment does not inflate it.
Games with growth off keep their usual phase-specific demand configuration.

### Create a separate experiment from an existing portfolio

Use portfolio inheritance when you want a new experiment to begin from an
existing fleet rather than a newly drawn one. This helps compare scenarios
with a common starting portfolio, although students' decisions and other
settings can still differ. A separate game gives the experiment its own
result and cumulative-score history.

Choose **Create Scenario from Existing** in the sidebar:

1. Select a source game and a period whose results have been released.
2. Review its portfolio snapshot.
3. Choose **Create an independent game** and the destination scenario.
4. Enter the new name and bid limits; optionally edit copied marginal costs.
5. Confirm creation and distribute the new Game Code.

The source game may still be ongoing. Students keep their Player Codes but
must sign in with the new Game Code. The destination has fresh periods and
results; earlier bids, profits, and temporary LNG adjustments are not copied.
An independent experiment starts a separate cumulative history.

## 13. Common classroom questions

| Situation | What to check or do |
|---|---|
| A student cannot join | Check the student URL, Game Code, enrollment method, enrollment state, and player limit. |
| A name is wrong | Use Edit name while enrollment is open. |
| A student lost their Player Code | Use the available GM recovery record or saved code CSV; distribute it privately. |
| A late student sees a waiting message | Review their request in Late registration and approve between periods. |
| A student typed bids but is incomplete | Ask them to submit their complete portfolio; wait for the status refresh. |
| Bidding closure warns about missing offers | Cancel to wait, or deliberately accept the full-capacity, zero-price fallback. |
| Students cannot see results after clearing | Select Release results. Clearing and release are separate actions. |
| A control is disabled | Check the selected phase/period and any required preceding action, such as investment finalization. |
| Gas costs cannot be edited | They lock when bidding opens. Configure the next period before opening it. |
| Demand cannot be edited | With growth off, it locks at bidding closure. With growth on, individual edits are unavailable; the source baseline is editable only before the first growth period opens and only if it has not already been used. |
| The app says the server is unavailable | Check your connection and contact the platform administrator if the service is unreachable. |
| You want to stop for a break | Leave the game in a deliberate state; closing the GM browser does not close bidding or stop the server. |

## 14. Finish a session

Keep the completed history when students will revisit decisions or when you
want to compare another scenario using the same portfolios. Deletion is for
records you no longer need.

After the last period, release results and allow time for students to review
them. Leave the game available if you want to revisit its history or use its
portfolio for another experiment. Sign out when using a shared computer.

**Delete game**, under Advanced controls, permanently removes that game and
its associated records after confirmation by Game Code. It is not the normal
way to finish a class. Ask the platform administrator about retaining or
backing up records before deleting anything needed for teaching or research.

## A one-period reminder

1. Select phase and period; review the saved growth demand if enabled, or save
   the individual demand settings if growth is off. Save any scenario settings.
2. Open bidding.
3. Check submission status.
4. Close bidding and resolve any missing-bid warning.
5. Clear market.
6. Review GENCO KPIs, Market graph, and Leaderboard.
7. Release results.
8. Discuss, then proceed to the next period.

For the mathematical model, full KPI formulas, and economic assumptions,
consult the companion [Economic framework and KPI methodology guide](GM_GUIDE.md).
