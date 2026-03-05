# ⏳ Workman's Clock (WMC)

A real-time, break-aware net earnings tracker that runs entirely in your browser — no server, no sign-up, no data leaves your device.

---

## ✨ Features

| Feature | Details |
|---|---|
| **Live earnings ticker** | Updates every second — per second (¢), per minute ($), per hour ($), and total today ($) |
| **Break-aware income** | Work intervals are split around unpaid breaks so you never over-count |
| **Multiple income streams** | Stack as many jobs/contracts as you like — all rates combine in real time |
| **One-time payments** | Bonuses, invoices, freelance payouts — added to your running total the moment they occur |
| **Recurring expenses** | Daily, weekly, or monthly costs are prorated and continuously subtracted |
| **Full breakdown panel** | Hit **Calculate** at any time to see a line-by-line breakdown of every number |
| **Field validation** | Missing or inconsistent fields are highlighted with an inline prompt before the calculation runs |

---

## 🚀 How to Use

### 1 · Open the app
Just open `index.html` in any modern browser — Chrome, Firefox, Safari, Edge all work.

### 2 · Fill in your income stream(s)
- **Rate ($/h)** — your gross hourly rate for that job
- **Start / End** — when your shift begins and ends (24-hour clock)
- **Break start / Break end** — unpaid break window (optional); leave blank if no break

Click **➕ add another income** to stack additional jobs or contracts.

### 3 · Add one-time payments (optional)
Click **➕ add one-time payment**, enter the dollar amount and the exact date & time.  
The payment is counted toward your totals once that moment is reached.

### 4 · Add recurring expenses (optional)
Click **➕ add expense**, enter the amount and pick **daily / weekly / monthly**.  
The app prorates the cost continuously throughout the day and subtracts it from your net.

### 5 · Watch the live panel
The dark header panel updates every second:

```
per second   per minute   per hour   earned today (net)
   0.07 ¢      4.17 $      25.00 $       73.45 $
```

### 6 · Get a full breakdown
Click the **🧮 Calculate & show breakdown** button.  
The app validates every field first — any missing or invalid entries are highlighted in red with a short explanation.  
If all fields are valid, a detailed breakdown expands below the button showing:

- Each income stream — shift hours, break deduction, and earned amount  
- One-time payments that have already occurred today  
- Total expenses (daily prorated amount deducted so far)  
- **Net earned today**

---

## 🔢 How the Maths Work

### Active minutes for an income stream

```
activeMinutes = work_segment_minutes_up_to_now
              = (shift_end - shift_start)
              - (break overlap with elapsed time)
```

Earnings for a stream so far today:

```
earned = (activeMinutes / 60) × hourlyRate
```

### Instant net rate

```
netHourly      = Σ(active hourly rates) − (totalDailyExpenses / 24)
netPerMinute   = netHourly / 60
netPerSecond   = netHourly / 3600
```

### Net earned today

```
netToday = Σ(income stream earnings)
         + Σ(one-time payments that occurred today, up to now)
         − (totalDailyExpense × fractionOfDayElapsed)
```

---

## 🤖 GitHub Actions Workflow

The repository includes a **workflow_dispatch** GitHub Actions workflow (`calculate.yml`) that lets you run a server-side Node.js calculation with the same maths the browser uses.

### Running the workflow

1. Go to **Actions → WMC Earnings Calculator** in the GitHub repository.
2. Click **Run workflow**.
3. Fill in the inputs:
   - `hourly_rate` — gross hourly rate ($/h)
   - `shift_start` — shift start time (`HH:MM`, 24-hour)
   - `shift_end` — shift end time (`HH:MM`, 24-hour)
   - `break_start` *(optional)* — break start (`HH:MM`)
   - `break_end` *(optional)* — break end (`HH:MM`)
   - `daily_expense` *(optional)* — total daily expenses ($)
4. Click **Run workflow** — the job will print a timestamped earnings breakdown to the log.

---

## 📁 File Structure

```
Wmc/
├── index.html          # The entire app — HTML + CSS + JS in one file
├── .github/
│   └── workflows/
│       └── calculate.yml   # GitHub Actions earnings calculator workflow
└── README.md           # This file
```

---

## 🛠️ Contributing / Development

1. Clone the repo
2. Open `index.html` in your browser — no build step, no dependencies
3. Edit, save, refresh — that's it

---

## 📄 License

MIT — do whatever you like with it.
