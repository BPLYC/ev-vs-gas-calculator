# AGENTS.md

## Project Context

This branch is the US English MVP of the original Chinese calculator. The product is now **EV vs Gas Savings Calculator**, a mobile-first web tool for US drivers deciding whether switching from a gas vehicle to an EV is financially worthwhile.

The MVP is free. It has no login, no payment, no membership, no order verification, and no browser-transfer flow.

## Tech Stack

- Frontend: React 19 + Vite
- Charts: `echarts` with `echarts-for-react`
- Screenshot export: `html2canvas`
- Hosting target: Cloudflare Pages static build

Useful commands:

```powershell
npm run dev
npm run lint
npm run build
npm run preview
```

Local app default:

```text
http://localhost:5173
```

## Architecture Map

- `src/main.jsx`: React entrypoint.
- `src/App.jsx`: top-level input/result screen switch.
- `src/index.css`: global tokens, reset, typography, form controls, animations.
- `src/App.css`: app shell, header, hero, footer.
- `src/components/InputPanel/InputPanel.jsx`: three-step US calculator input flow.
- `src/components/InputPanel/InputPanel.css`: stepper, fields, sliders, toggles, sticky actions.
- `src/components/ResultDashboard/ResultDashboard.jsx`: result cards, charts, screenshot save, share flow.
- `src/components/ResultDashboard/ResultDashboard.css`: result layout and responsive safeguards.
- `src/components/Charts/Charts.jsx`: ECharts option builders for cumulative ownership cost and annual cost breakdown.
- `src/data/cityPrices.js`: US state/rate presets and default form values.
- `src/utils/calculator.js`: pure browser-side cost model and formatting helpers.
- `public/robots.txt` and `public/sitemap.xml`: SEO crawl targets.
- `index.html`: English SEO metadata, OG/Twitter tags, JSON-LD, fonts.

## Calculation Rules

`src/utils/calculator.js` is browser-only and pure.

Current US MVP model:

- Gas annual fuel cost = `annualMileage / fuelEfficiency * fuelPrice`.
- EV annual electricity cost = `annualMileage / 100 * evConsumption * electricityPrice`.
- Gas upfront cost = `gasCarPrice`.
- EV upfront cost = `evCarPrice + chargerInstallCost - evIncentives`.
- `insuranceDiff` is positive when EV insurance is more expensive.
- `maintenanceDiff` is negative when EV maintenance is cheaper.
- Annual net saving = gas fuel cost minus EV electricity cost minus insurance and maintenance differences.
- Break-even is derived from cumulative 15-year yearly data and linear interpolation.

Keep returned fields compatible with `ResultDashboard` unless you update the UI in the same change.

## Frontend Direction

This is a practical consumer finance utility, not a marketing landing page.

Priorities:

- Keep the calculator as the first screen.
- Optimize for mobile clarity and trust.
- Keep copy concise and decision-oriented.
- Prefer dense, calm, scan-friendly UI over decorative sections.
- Avoid adding a broad design-system dependency unless the scope clearly needs it.
- Check `package.json` before importing any new UI, icon, animation, or state library.

## SEO And Deployment Notes

- Default placeholder domain is `https://ev-vs-gas-calculator.pages.dev`.
- When a real domain is chosen, update `index.html`, `public/robots.txt`, and `public/sitemap.xml` together.
- The original Chinese payment Worker and Pages Function are intentionally removed from this MVP.

## Verification Checklist

For code changes:

```powershell
npm run lint
npm run build
```

For frontend/UI changes:

- Run `npm run dev`.
- Open `http://localhost:5173`.
- Check input and result screens on mobile-width and desktop-width viewports.
- Verify no text overflows in buttons, stat cards, energy comparison, chart titles, and action buttons.
- Test at least three scenarios: EV clearly saves, EV breaks even slowly, and EV does not break even.

## Documentation Discipline

When behavior changes, update the relevant docs in the same change. Do not copy historical Chinese deployment credentials or old payment notes into English-version docs.
