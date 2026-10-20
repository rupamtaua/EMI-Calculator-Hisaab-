# Hisaab – Calculator Hub

A fast, good-looking collection of money and everyday calculators in a single HTML file. No build step, no framework, no backend. Open the file in a browser and it works.

Built with plain HTML, CSS and JavaScript. Designed for both desktop and mobile, with a white and green theme.

## Features

- 18 calculators across four categories
- Results update as you type
- Desktop layout with a sidebar, mobile layout with a floating bottom navigation bar
- Indian digit grouping for amounts (for example ₹25,00,000)
- Donut chart for principal versus interest, plus a year-by-year table for EMI
- Friendly validation messages instead of broken numbers
- Works offline once the page has loaded (the only external request is the Google Font, which falls back to system fonts)
- Respects reduced-motion settings and has visible keyboard focus

## Calculators

| Category | Calculators |
|---|---|
| Loans | EMI, Loan Prepayment, Loan Eligibility |
| Savings | FD, RD, SIP, Lumpsum, PPF, Inflation |
| Interest & tax | Simple Interest, Compound Interest, GST, Discount, Percentage |
| Everyday | BMI, Age, Fuel Cost, Bill Split |

## Getting started

1. Download `calculator-hub.html`.
2. Open it in any modern browser (Chrome, Edge, Firefox, Safari).

That is all. To host it, upload the file to any static host (GitHub Pages, Netlify, Vercel, an S3 bucket). Rename it to `index.html` if your host expects that.

## How the page is organised

Everything lives in one file, in this order:

1. **CSS** – colour tokens at the top of `:root`, then layout, components and the mobile rules.
2. **Calculator definitions** – one `add({...})` block per calculator.
3. **Icons** – a small `ICONS` object of inline SVG paths.
4. **UI code** – hash-based routing, the sidebar and bottom bar, and the result renderer.

Routes use the URL hash, so every calculator has its own link:

- `#home` – dashboard
- `#g0` to `#g3` – a category page
- `#emi`, `#fd`, `#sip` and so on – a calculator

## Adding a calculator

Add a block like this in the calculator section, then add its icon to `CALC_ICON`.

```js
add({
  id: "cagr", group: "Savings", title: "CAGR Calculator",
  sub: "Yearly growth rate between two values.",
  fields: [
    {k: "start", label: "Starting value (₹)", v: 100000},
    {k: "end",   label: "Ending value (₹)",   v: 250000},
    {k: "y",     label: "Years",              v: 6}
  ],
  calc({start, end, y}) {
    if (start <= 0 || y <= 0) return {error: "Enter a positive value and duration."};
    const rate = (Math.pow(end / start, 1 / y) - 1) * 100;
    return {stats: [["CAGR", num(rate) + "%", 1]]};
  }
});
```

Field options:

- `k` – the key passed to `calc`
- `label` – text shown above the input
- `v` – default value
- `step` – optional, for decimal inputs
- `type` – optional: `"select"` (with `options: [[value, label], ...]`) or `"date"`

What `calc` can return:

- `error` – a message shown instead of results
- `stats` – a list of `[label, value, isHighlighted]`
- `split` – two `[label, amount]` pairs, drawn as a donut chart
- `table` – `{head: [...], rows: [[...], ...]}`

## Formulas

| Calculator | Formula |
|---|---|
| EMI | `P × r × (1+r)^n / ((1+r)^n − 1)`, where `r` is the monthly rate and `n` the number of months |
| FD / Compound interest | `P × (1 + R/(100×f))^(f×t)`, where `f` is compounding periods per year |
| RD | Each monthly deposit compounds quarterly for its remaining months |
| SIP | `P × ((1+i)^n − 1) / i × (1+i)`, deposits made at the start of each month |
| Lumpsum | `P × (1 + R/100)^t` |
| PPF | Yearly deposit plus a full year of interest, repeated each year |
| Simple interest | `P × R × t / 100` |
| Loan eligibility | Affordable EMI is `income × limit% − existing EMIs`, converted back to a loan amount |
| BMI | `weight / height²` (height in metres) |

## Customising the look

The colours are variables at the top of the stylesheet. Change them to re-theme the whole app.

```css
:root {
  --g700: #0f7a43;   /* deep green */
  --g500: #22c55e;   /* bright green */
  --g100: #dcfce7;   /* light green */
  --bg:   #f3faf6;   /* page background */
}
```

The currency symbol is set in the `inr` helper near the top of the script.

## Good to know

- Results are estimates for planning. Banks and funds may round, compound or charge fees differently, so check the final numbers with your lender or provider.
- Investment return calculators (SIP, Lumpsum) use the rate you enter. Real returns are not guaranteed.
- Interest rate defaults (for example PPF at 7.1%) are only starting values. Update them to the current rate.
- The inflation and loan eligibility calculators are simple models and do not replace professional advice.

## Browser support

Any current version of Chrome, Edge, Firefox or Safari, on desktop and mobile.

## License

Use it, change it and ship it however you like.
