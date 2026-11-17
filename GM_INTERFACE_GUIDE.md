# Energy Market Game

## Game Master Interface and Operating Guide

**Audience:** professors, lecturers, and teaching assistants running a session.
**Edition:** 17 November 2026 — browser and iOS platform.

Use this guide to prepare and operate a game: which controls to use, in what
order, and how to handle common classroom situations. It describes the current
interface by control name, so you can find a feature without relying on its
exact position on the page.

The companion [GM Guide](GM_GUIDE.md) explains the teaching purpose, economic
assumptions, formulas, and interpretation of results. This operating guide
links to that material rather than repeating it.

## 1. Before the class

Obtain these from the platform administrator:

- Your GM website address, ending in `/admin`.
- Your GM account or account-creation invitation.
- The student website address, ending in `/play`, and any iOS installation
  instructions.

Check access using the network and devices your students will use. Run a
short practice game before the first class. Decide how students will join,
which scenario to use, and whether progression will be manual or timed.

Read the [scenario overview](GM_GUIDE.md#1-purpose-and-scope) when choosing
your teaching sequence. No command-line knowledge is required to run a
session on an already configured server.

## 2. Sign in and navigate

Open the supplied `/admin` address and sign in with your GM credentials.
If you received an account invitation, complete registration through that
invitation first. The separate `/platform-admin` website manages GM accounts.

| Control or panel | Operation |
|---|---|
| Game sessions | Select an existing game or choose **+ New game** |
| Game header | Check the selected game's name, status, and Game Code |
| Suggested next action | Perform the next available game action |
| Enrollment | Check registrations, correct names, and access available code records |
| Game progress | Select the phase and period to inspect |
| Demand growth | Enable a path, set its baseline, or inspect saved demand |
| Period schedule | Configure optional timed progression |
| Scenario controls | Save LNG settings or manage an Investment window |
| Live bidding status | Check submission completion |
| Student announcements | Publish, edit, or remove a message |
| Market Coach | Enable or disable private results feedback |
| Advanced controls | Edit individual demand, use permitted manual actions, or delete a game |
| Results sub-tabs | Open **GENCO KPIs**, **Market graph**, or **Leaderboard** |

Selecting a historical period changes the displayed results; it does not
reopen bidding. Check the phase and period shown in an action's confirmation.
Period numbers restart at 1 in each phase.

Use the account button to change your password and **Sign out** when finished.
If you forget your GM login, contact the platform administrator.

## 3. Create a game

1. Choose **+ New game**, or the **+** beside Game sessions.
2. Enter a descriptive **Game name**.
3. Select **Normal Market**, **LNG Crisis**, or **Capacity Investment**.
4. Choose one of the enrollment methods below.
5. Set the player count or maximum number of players.
6. Review **Minimum bid** and **Maximum bid**.
7. Select **Create game** and confirm it appears in the sidebar.

### Student self-registration

Choose **Student self-registration** and set **Maximum players**. Share the
student URL or QR code and the Game Code. Students enter that code and a
display name; the app gives each student a private Player Code to save.

### Player Codes from the GM

Choose **Player Codes from the GM**. Set the number of players or enter one
player/team label per line. Download the available code list and give each
private Player Code only to its intended student or team.

## 4. Complete enrollment

Watch the joined count and participant list. While enrollment is open:

- Use **Edit name** to correct a display name.
- Use **Remove** for an incorrect registration; the student can then register again.
- Use **Regenerate code**, when available for an unjoined invitation, to replace it.
- Use **Download current codes CSV**, when available, to keep a private recovery copy.

The shared Game Code identifies the class. Player Codes are private login
credentials: do not project the code list or CSV.

When the intended students have joined, select the enrollment-closing action
and confirm. Wait for portfolio generation to complete, then ask students to
check that Portfolio opens. Ordinary registration editing and removal are
restricted after enrollment closes. Use [late registration](#11-admit-a-late-student)
for students who missed this stage.

## 5. Run a bidding period manually

The sequence is:

**Configure → Open bidding → Monitor → Close bidding → Clear market → Review → Release results**

### Configure

Select the phase and period in **Game progress**. Review demand using
[Section 7](#7-choose-the-demand-settings). For LNG, save the period settings
using [Section 8](#8-run-an-lng-crisis-phase). For Investment, finalize the
investment stage using [Section 9](#9-run-a-capacity-investment-phase).

### Open bidding

Choose **Open bidding** and confirm. Ask students to enter their offers and
submit their complete portfolio. They can edit and resubmit while bidding
remains open. Student cards allow adding or removing blocks up to the
per-generator limit; submission errors must be corrected before saving.

For block limits, quantity constraints, and the economic meaning of offers,
see [GM Guide, Section 4](GM_GUIDE.md#4-supply-offers-and-physical-withholding).

### Monitor submissions

Use **Live bidding status** to find incomplete companies. Allow a few seconds
for a new submission to appear. Entering values in an input does not save a
bid: the student must submit it. A submitted zero-MW offer counts as a saved
decision.

### Close bidding

Select **Close bidding**. If offers are missing, the confirmation identifies
the incomplete companies:

- **Cancel:** keep bidding open.
- **OK:** close with a full-available-capacity, zero-price offer for each
  generator without a saved bid.

Previously submitted blocks, including intentional zero-MW offers, are kept.
Explain your missing-bid policy before students play; see the
[fallback assumptions](GM_GUIDE.md#17-missing-bid-fallback) for its effects.

### Clear, review, and release

1. Select **Clear market** and wait for the status to become **Cleared**.
2. Inspect the results sub-tabs described in [Section 10](#10-inspect-results).
3. Select **Release results** when ready for students to view them.
4. Open the next period when ready and repeat the sequence.

Clearing alone does not publish results. Wait for each action and its status
update before repeating it.

| Status | Operational meaning |
|---|---|
| Waiting | Configure the period before opening bidding |
| Bidding | Students can submit or revise offers |
| Bidding closed | Offers are locked; clearing is the next step |
| Clearing | Wait for the calculation to finish |
| Cleared | Review results, then release them |
| Results released | Students can view results; continue when ready |

## 6. Optional timed progression

Leave timers off for manual operation. To schedule a phase:

1. Select the phase in **Game progress** and find **Period schedule**.
2. Check **Display timezone**, initially detected from your PC. Override it
   with an IANA timezone such as `Europe/Athens` if required.
3. Enable **Use timer** for the required periods and enter each clearing
   date/time. Leave other periods untimed.
4. Save the schedule. Deadlines must be future times and increase in period
   order. Choose another time if a daylight-saving ambiguity is reported.
5. Confirm bid limits allow the zero-price fallback. Review and save upcoming
   LNG configurations, and finalize Investment decisions when applicable.
6. Open the phase's first period manually. Saving a timer does not open it.

At an active deadline, the server closes bidding with the missing-bid fallback,
clears the market, releases results, and opens the next period in the same
phase. Automatic opening locks the next period's saved LNG configuration.
The final period ends progression; the system does not create a new phase.
An untimed next period may open automatically but needs manual closure.
Students see the active deadline in their device's timezone.

Changing the display timezone shows an existing deadline in another timezone;
it does not move the deadline. The schedule panel displays status, progress,
completion time, and defaulted-submission counts.

### Change or cancel a timer

Edit an eligible pending deadline and save. Use **Cancel timer**, or uncheck
**Use timer** and save, to return the period to manual operation. Closing
bidding manually also cancels that period's timer; it does not automatically
clear or release results. A running transition cannot be edited or cancelled.

### Recover after downtime or a failed action

The backend must be running for timers to execute. After a restart, deadlines
missed while it was stopped become **Paused — GM action required**. Open
bidding stays open and waiting periods stay waiting. Choose either:

- Complete closure, clearing, release, and next-period opening manually.
- Save a new future deadline to resume automatic progression.

Paused open periods accept bids. Later deadlines are not shifted automatically.
For example, if Period 1's 10:00 deadline and Period 2's 11:00 deadline pass
while the server is stopped, a 12:00 restart pauses both schedules. It does
not run the missed auctions.

If a transition had started before interruption, or an action fails, inspect
the reported error and completed steps. Resolve the cause and select
**Retry transition**, or cancel the timer and finish manually. Saved steps
are retained. Notification failures do not undo completed market actions.
Maintenance mode suspends timers; if the backend stays running, due timers
resume when maintenance ends. A restart uses the pause policy above.

## 7. Choose the demand settings

### A. Edit demand separately for each period

With demand growth off:

1. Select a phase and period.
2. Expand **Advanced controls** and find the demand editor.
3. Set **Demand fraction (alpha)** and review **LP increments**.
4. Select **Save demand** and check that the saved values update.

Individual demand can be edited while the period is waiting or bidding is
open. Closing bidding locks it. For the meaning of alpha and increments,
see [demand formulation](GM_GUIDE.md#5-demand-formulation) and
[discretization](GM_GUIDE.md#6-discretization-of-elastic-demand).

### B. Enable and manage demand growth

Growth is optional and initially off. To enable it:

1. Close enrollment and wait for portfolios to be generated.
2. Select the intended phase before its Period 1 opens bidding.
3. In **Demand growth**, set **Baseline (% of starting capacity)** and review
   the displayed starting MW.
4. Choose **Enable growth from Period 1**.
5. Inspect the saved rates and demand quantities in the table.

Before the first affected period opens, use **Save baseline** to change an
eligible baseline while retaining the saved rates. Use **Restore previous
demand schedule** to switch growth off. A baseline from an already released
period cannot be edited. Older games may show their first growth draw at
Period 2; follow their saved table.

After the first affected bidding period opens, the path is fixed. Individual
demand editors are unavailable for its periods. When appending a phase,
review the continued path rather than trying to enable a second one.

The [demand-growth explanation](GM_GUIDE.md#54-optional-demand-growth-3-expected-increase)
contains the distribution, compounding, baseline, and economic assumptions.

## 8. Run an LNG Crisis phase

In **Six-period crisis schedule**, before opening each period:

1. Select the period.
2. Set **Gas marginal-cost adjustment** in €/MWh.
3. Edit **Student market news title** and **Student market announcement**.
4. Select **Save crisis settings**.
5. Open bidding when ready.

The adjustment and announcement lock when bidding opens. Check the adjustment
when expecting students to see LNG news: a zero value suppresses that period's
announcement. Future messages are not shown early.

For a message you want to publish immediately, use
[Student announcements](#14-publish-student-announcements) instead. The
[LNG assumptions](GM_GUIDE.md#142-lng-crisis) explain the generation-cost model.

## 9. Run a Capacity Investment phase

Use **Investment window**:

1. Select **Open investment window**.
2. Ask students to review the displayed technology offers and submit their
   desired additional capacities; zero investment is permitted.
3. Review saved decisions, additional MW, and capacity charges in the GM table.
4. Select **Close investment window** when decisions are complete.
5. Review the decisions, then select **Finalize investments**.
6. Wait for the acquired generators to appear, then open bidding.

Finalization makes decisions permanent. Use the ordinary period controls for
the remaining auctions. In results, inspect **Net profit** and the Investment
leaderboard score as well as operating profit.

For purchase limits, capacity charges, and the passive benchmark's purpose,
see [Investment assumptions](GM_GUIDE.md#15-capacity-investment-assumptions)
and [leaderboard methodology](GM_GUIDE.md#13-leaderboard-methodology).

## 10. Inspect results

Select a cleared or released period in **Game progress**, then choose one of
the results sub-tabs. Changing the selection does not change the saved auction.

### GENCO KPIs

Use **Filter student / GENCO** to inspect the class or a participant. Company
totals, generator rows, and summary panels follow the selected filter. The
generator bid cells list the saved blocks separately.

Signed comparison values use green for zero/positive and red for negative.
Unavailable data displays **N/A**. Some normalized analytical columns retain
“/MWh” in their interface labels; use the definitions in
[GM Guide, Section 12](GM_GUIDE.md#12-kpi-definitions-and-aggregation) when
explaining them. The separate leaderboard displays euro scores.

### Market graph

Hover over, keyboard-focus, or tap an offer to inspect its details. Use the
filters to show or hide demand, submitted supply, player identities, marginal
costs, and perfect-competition supply. Use the bid table to inspect individual
blocks, including zero-MW offers that have no visible curve width.

Zoom the axes, drag to pan, and reset the view to return to the full market.
Before projecting the graph, check whether player identities and cost details
are visible. The perfect-competition curve still shows cost levels visually
when numeric cost labels are hidden.

### Leaderboard

Select the period to inspect **Current-period ranking** and **Cumulative
ranking**. The GM sees signed euro scores and the applicable benchmark label.
Students see names and ranks, with search, but no numeric scores.

To exclude a practice round:

1. Find **Cumulative ranking periods**.
2. Select the eligible periods that should count through the displayed period.
3. Select **Save cumulative periods** and wait for confirmation.

This selection applies to every student. It changes cumulative ranking only;
the period's results and current-period ranking remain accessible. To include
a later released period, select that later period first. A cleared period's
current ranking is available to the GM before release; its cumulative
contribution waits for release.

For score formulas, ties, missing comparisons, and interpretation, use
[GM Guide, Section 13](GM_GUIDE.md#13-leaderboard-methodology).

## 11. Admit a late student

1. Enable **Late registration** in the game dashboard after normal enrollment closes.
2. Ask the student to request entry through the student app.
3. Wait until the preceding results are released and a future waiting period exists.
4. Review the pending request and select **Approve** or **Reject**.
5. For approval, review the activation period and any demand-recalculation notice.
6. Ask the accepted student to refresh and inspect their portfolio.

Approval is blocked while an auction is bidding, closed, clearing, or awaiting
release. Multiple students can be approved between periods. Follow each
confirmation's demand notice, including whether growth demand was preserved.
For the economic effects on supply, demand, and historical scores, see
[GM Guide, Section 16](GM_GUIDE.md#16-enrollment-changes-and-their-economic-effects).

## 12. Continue or repeat a game

### Append another phase

After the latest phase finishes, select the next scenario and **Add another
phase**. Check that participants, portfolios, and previous results remain
accessible. Review the new phase's demand and scenario settings before opening
bidding; its displayed period count starts at 1.

### Create an independent game from a portfolio snapshot

Choose **Create Scenario from Existing**:

1. Select the source game and a period whose results are released.
2. Review the portfolio snapshot.
3. Choose **Create an independent game** and the destination scenario.
4. Enter its name and bid limits; optionally edit copied marginal costs.
5. Confirm creation and distribute the new Game Code.

The source game can still be ongoing. Students sign in using the new Game
Code and their retained Player Codes. Check you have selected the destination
game before running its auctions.

For what is inherited and how histories remain separate, see
[GM Guide, Section 18](GM_GUIDE.md#18-portfolio-inheritance-and-experiment-continuity).

## 13. Enable Market Coach (Not very usefull currently)

Select the game and find **Market Coach**. Check **Enable Market Coach** and
wait for **Market Coach enabled.** Uncheck it to disable feedback for that game.

After results are released, students can expand **Market Coach** in that
period's Results, including history. If it is absent, confirm the setting
saved, the student is in the correct game, and the period is released.

Use [GM Guide, Section 20](GM_GUIDE.md#20-market-coach-purpose-indicators-and-interpretation)
for the feedback rules and how to discuss the observations.

## 14. Publish student announcements

In **Student announcements**:

1. Enter **Title**, then **Message**.
2. Select **Publish announcement** and wait for confirmation.
3. Ask students to open **News**; the new message has an unread badge.

Publication is immediate for that game. A custom announcement does not open
a popup. The GM list shows titles and publication details.

To revise a message, select **Edit**, change the fields, and choose **Save
changes**. **Cancel edit** leaves the published version unchanged. A saved
revision becomes unread again for students. Use **Remove** and confirm to
withdraw a message from their active news history.

LNG announcements are configured separately through
[the crisis controls](#8-run-an-lng-crisis-phase).

## 15. Help students recover a Player Code

Students can recover their Player Code by email after verifying their recovery address.
Have students verify their address while they still have access:

1. In Portfolio or the enrollment waiting screen, find **Player Code recovery**.
2. Enter an email and choose **Send verification code**.
3. Enter the emailed six-digit code in **Email verification code** on that same
   screen, then choose **Confirm email**.
4. Check that the app confirms the email is verified.

The verification code expires after 10 minutes. After five incorrect attempts,
use **Send a new verification code**. This short verification code is separate
from the Player Code used to sign in.

If the student later loses access, use **Forgot Code** on the login screen,
enter the Game Code and previously verified email, and check that inbox and
spam folder. The email contains the existing Game Code and Player Code;
the student enters them in **I HAVE A CODE**. There is no recovery link to open.

If the address was never verified, delivery is unavailable, or a legacy code
cannot be retrieved, use the private GM recovery record or saved code CSV.
Unrecognized or unverified addresses receive the same generic request message;
that message alone does not confirm an email was sent. Ask the administrator
for help if a verified address repeatedly receives nothing.

## 16. Troubleshoot a classroom session

| Situation | Check or action |
|---|---|
| A student cannot join | Check URL, Game Code, enrollment method, state, and player limit. |
| A name is wrong | Use **Edit name** while enrollment is open. |
| A late student sees a waiting message | Review the request between periods using Section 11. |
| A student typed bids but remains incomplete | Ask them to submit the complete portfolio and allow status to refresh. |
| Closing warns about missing offers | Cancel to wait, or explicitly accept the fallback. |
| Students cannot see cleared results | Select **Release results**. |
| A control is disabled | Check the phase/period and required prior actions, such as investment finalization. |
| Gas settings cannot be edited | Configure an unopened period; settings lock when bidding opens. |
| Demand cannot be edited | Check the saved demand mode and eligibility in Section 7. |
| A timer paused after restart | Set a new future deadline or continue manually using Section 6. |
| A student lost their code or cannot verify email | Follow Section 15; contact the administrator for delivery problems. |
| The server is unavailable | Check connectivity and contact the platform administrator. |
| You need a break | Check active timers and bidding; closing your browser does not stop them. |

## 17. Finish a session

Release the final results and allow students to review them. Keep the game if
you will revisit its history or use its portfolio as a source. Sign out on
shared computers.

To delete a game, use **Delete game** under **Advanced controls** and confirm
its Game Code. This permanently removes its associated records. Confirm any
required retention or backup arrangements with the administrator first.

## A one-period reminder

1. Select the phase and period; review demand and save scenario settings.
2. Open bidding.
3. Check submission status.
4. Close bidding and resolve any missing-bid confirmation.
5. Clear the market and wait for **Cleared**.
6. Review **GENCO KPIs**, **Market graph**, and **Leaderboard**.
7. Release results.
8. Continue when ready, or check the next active timer.
