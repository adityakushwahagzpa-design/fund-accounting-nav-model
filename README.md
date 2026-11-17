# Fund Accounting & Daily NAV Model

This is an Excel model of what a fund accountant does every business day: value a mutual fund, work out its NAV, and run the checks and reconciliations that come before sign-off. It covers November 2026 for a fictional hybrid fund with about ₹71 crore in net assets.

**All the securities, prices, trades and investor flows are fictional, made up for practice.** None of the issuers are real companies.

**File:** [NAV_Fund_Accounting_Model.xlsx](NAV_Fund_Accounting_Model.xlsx)

## What's in it

| | |
|---|---|
| NAV dates | 20 business days (2–30 Nov 2026), plus the 30-Oct opening NAV. 24-Nov is a market holiday |
| Securities | 20 stocks and 5 bonds (government, state, PSU and corporate) |
| Trades | 55 equity trades, settled T+1 |
| Corporate actions | 4 cash dividends, a 1:2 stock split and a 1:2 bonus issue |
| Investor transactions | 138 subscriptions and redemptions, each processed at its day's NAV |
| Formulas | About 4,000. Every number outside the blue input cells is a formula |

## How the NAV is built each day

1. **Holdings** are kept on a trade-date basis. On a split or bonus ex-date, the holding before the ex-date is multiplied by the ratio, and any trades after the ex-date are added on top.
2. **Market value** is the holding times the closing price. Bond prices are quoted per ₹100 of face value.
3. **Accrued interest** builds up on each bond until its coupon date and then resets. Two bonds pay their coupons this month, and that cash goes into the cash book.
4. **Receivables and payables:** sale proceeds and purchase costs sit here until the trade settles. A dividend becomes a receivable on its ex-date and turns into cash on its pay date.
5. **Expenses** (1.20% TER) accrue for every calendar day, so Monday's NAV carries three days of expenses.
6. **NAV per unit** is net assets *before* the day's subscriptions and redemptions, divided by the units in issue at the start of the day. Investor transactions are then turned into units at that NAV.

## Results

- **NAV:** from ₹31.4000 to ₹32.0071. That is a fund return of +1.93% for the month, against +1.49% for the benchmark.
- **Checks:** 6 days were flagged for review and 7 exceptions were logged and cleared.
  - Malabar Cement rose 9.2% on its results. That one move lifted NAV by about 0.38%, which pushed the day outside the 0.30% benchmark tolerance.
  - The price drops on the split and bonus ex-dates were exactly offset by the larger holdings.
  - A thinly traded PSU bond showed the same price twice in the month. Both times it was checked against the valuation agency's price.
- **Cash reconciliation at 30-Nov:** two items explain the gap between the fund's books and the custodian. One is a ₹1,180 bank charge the fund hadn't booked yet. The other is a ₹1,09,500 subscription that reached the bank after its cut-off.
- **Holdings reconciliation at 30-Nov:** all 25 holdings match once two timing items are allowed for:
  - the three trades done on 30-Nov, which settle on 1-Dec
  - 69,000 bonus shares that the custodian only receives on 2-Dec
- **Settlement over the holiday:** trades done on 23-Nov settle on 25-Nov, not 24-Nov, because 24-Nov is a market holiday.

## How I checked it

I rebuilt the same calculation independently in Python. All 21 NAVs, unit balances and cash balances in the workbook match it exactly.

## Sheets

| Sheet | What's in it |
|---|---|
| Summary | Month at a glance, daily NAV table and chart |
| Inputs / Calendar | TER, opening units and cash, tolerances, NAV dates, holidays and benchmark returns |
| Securities / Prices | Security master and daily closing prices |
| Trades | Trade blotter: settlement dates (WORKDAY with holidays), charges and net amounts |
| Corp_Actions | Dividends, split and bonus, with each dividend's entitlement taken from the holdings |
| Capital_Activity | 138 investor transactions, with the NAV applied and the units issued or redeemed |
| Holdings / Market_Value / Accrued_Interest | Daily grids for all 25 securities |
| Cash | Cash book on a settlement basis |
| NAV | Assets, liabilities, NAV per unit and unit movements for each day |
| NAV_Checks | Daily review checks and the exceptions log |
| Cash_Recon / Position_Recon | Fund books against the custodian at month-end |

## Simplifications

- Subscriptions and redemptions settle in cash on the NAV date.
- Interest accrues on an actual/365 basis.
- There are no bond trades, taxes or exit loads, and accrued expenses aren't paid out during the month.
- The benchmark returns are illustrative.
