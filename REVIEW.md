# Farm Profit Manager — Review & Recommendations

_Reviewed November 5, 2026. Focus: quick read on **profit & break-even** and **marketing position**, on **desktop and phone**, single user._

How I checked: read the code (layout, navigation, saving, dashboard, profit math, marketing) and clicked through the app on desktop and a phone-sized screen using the app's built-in sample data, with cloud saving blocked. **I did not look at your real data.**

---

## A. Bugs: numbers that can be wrong (fix first)

| # | Problem | Why it matters |
|---|---------|----------------|
| A1 | **The blended price goes too high if you've sold more bushels than you expect to grow.** In a test, contracts averaged $4.50 but corn showed a **$5.85** blended price. | A short crop is when you most need correct numbers, and that's when this happens. Revenue, net return, margin and break-even all come out too optimistic. |
| A2 | **The Marketing page leaves out the Non-GMO and Renwick premiums** when it values unsold soybeans. The Dashboard includes them. With sample data, Marketing showed **$581k** and the Dashboard **$633k** for the same beans. | Two pages give two different answers for the same beans. |
| A3 | **The Dashboard price strip labels the SB Commercial price as "Soy"** but counts contracted bushels for all soybeans. The Marketing Progress card also values all unsold soybeans at the commercial price. | This makes the numbers hard to read. Non-GMO and Renwick look worse than they are. |
| A4 | The "Refresh live prices" link on the Dashboard highlights **Application Plan** in the sidebar instead of Marketing. | Small, but confusing. |
| A5 | The Sale Decision tool uses fixed fallback numbers (870.4 ac, 229 bu, $4.48, $275 rent) and a simple (not acre-weighted) average yield. | Its suggested starting numbers can differ from the Dashboard. |

**A question for you (not necessarily a bug):** if a field has an Application Plan, its input cost comes **only** from that plan, and the crop budget is ignored for that field. Costs like seed, crop insurance, drying or interest won't count unless they're plan lines. Is that what you want?

---

## B. Phone problems

| # | Problem | Fix |
|---|---------|-----|
| B1 | **The Enterprise Summary is cut off on a phone.** You can see Acres, Yield and Price, but **Full B/E and Margin/Ac, the numbers you care most about, are off the screen.** | Show each crop as a card on phones: margin, break-even vs. price and % sold, in large text. |
| B2 | **You can't switch crop years on a phone.** The year picker is only in the desktop sidebar. | Add the year to the phone header. |
| B3 | **You can't sign out on a phone.** | Add Sign out under **More**. |
| B4 | **Inputs & Inventory is missing from the phone menu.** You can't reach that page on a phone at all. | Add it under **More**. |
| B5 | Much of the text is 9–11px (notes, labels, explanations) and hard to read in the truck. | Make the most important numbers bigger and move the explanations into tap-for-info pop-ups. |

---

## C. Making it quick to review and decide

The Dashboard has the right information, but it's packed with formulas and explanation text. It takes study to answer "where am I?"

**C1. Add a "Where I stand" strip at the top of the Dashboard.** For each crop, one line in plain language:

> **Corn**: projected **−$37/ac** · break-even **$4.62** vs. market **$4.46** (−$0.16) · **30% sold** · need **237 bu/ac** to break even (APH 229)

Show it green, amber or red at a glance. This would be the one view you check every day.

**C2. Add a "Needs attention" box.** It would show only what calls for action, such as:
- Prices more than 1 day old (with a refresh button)
- Market price is now above full break-even, so it may be time to sell (uses your existing ProfitWatch targets)
- Less than X% sold by a date you set
- Contracts that add up to more than expected production (see A1)
- An unsaved or failed save

**C3. Move the explanations out of the way.** Footnotes like "Blended price = (contracted bu × …" can be ⓘ tooltips. Keep plain labels: "Profit/acre", "Break-even", "Sold".

**C4. Put the Sale Decision tool on the Marketing page.** "Is this sale profitable?" is a marketing question but sits in Reports → second tab. A **"Check a sale"** button next to "+ Log Sale" fits better.

**C5. Make one price the main price.** The app shows futures, MTM, blended, budget price and settings price. The Dashboard should show **market price** and **your average sold price**, and keep the rest in details.

**C6. Simplify the menu.** There are 17 pages. Consider:
- Keep it simple day-to-day: **Dashboard · Marketing · Fields · Field Ops (App Plan + Tracker + Planting) · Money (Cash Flow, Loans, Balance Sheet) · Reports · Setup** (Budgets, Machinery, Overhead, Settings, Inputs)
- On the phone bottom bar, consider **Dashboard · Marketing · Grain · Tracker · More**. App Plan is mostly desk work.

**C7. Remove sign-up code you don't use.** The hidden "Create account" form isn't needed for one user. Removing it also closes a small security gap (see D2).

---

## D. Data safety and maintenance

| # | Item |
|---|------|
| D1 | The save system is careful: it won't save if loading failed, holds back big deletions, and catches edits from two devices at once. Good. |
| D2 | **Check Supabase settings:** make sure **new sign-ups are disabled** and the `farm_data` table only lets **your** account read and write. The public key in the page is normal for Supabase, but your data is protected only if those rules are set. I can't see those settings from the code. You'd check them in the Supabase dashboard. |
| D3 | Backups are a manual "Backup Data" download. Consider an automatic weekly backup reminder, or showing "Last backup: X days ago" on the Dashboard. |
| D4 | The whole app is a single 9,000-line file. That's fine for GitHub Pages, but changes get riskier over time. No action needed now; we'll test each change locally before pushing. |

---

## Suggested order

1. **Fix the number bugs** A1–A4 (small, high value).
2. **Fix the phone gaps** B2–B4 (small), then **B1** (phone crop cards).
3. **Build the "Where I stand" strip and "Needs attention" box** (C1, C2): the biggest gain in ease of use.
4. Clean up text, prices and menus (C3–C6) and the Supabase check (D2).
