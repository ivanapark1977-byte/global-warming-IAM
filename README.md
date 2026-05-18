# Climate–Economy IAM Dashboard

A browser-based, first-order **Integrated Assessment Model** spanning 2024–2100. Couples a four-sector energy transition model with GDP dynamics, emissions accounting, climate physics, endogenous carbon removal, and supply curve feedbacks for oil and LNG.

**[Open the dashboard →](https://yourusername.github.io/iam-dashboard/)**
*(replace with your GitHub Pages URL after publishing)*

---

## What it does

The dashboard runs a full 2024–2100 climate-economy simulation in real time as you move sliders. It is designed to make the relative importance of different policy levers and scientific uncertainties visible at a glance — not to produce precise forecasts.

Three preset scenarios are included:

| Scenario | IEA analogue | Central T₂₁₀₀ |
|---|---|---|
| No Policy | CPS | ~3.0°C |
| Current Policy | STEPS | ~2.9°C |
| Strong Action | NZE (partial) | ~2.1°C |

All parameters are calibrated to IEA WEO 2025 and IPCC AR6 anchor points at 2024.

---

## Files

| File | Description |
|---|---|
| `index.html` | The interactive dashboard — open this in any browser |
| `iam_model_documentation.html` | Full technical documentation: all equations, parameter sources, calibration targets, uncertainty analysis |
| `iam_user_manual.html` | User manual: how to operate the dashboard, what each slider does, shortcomings, and how the model diverges from IEA scenarios |
| `IAM_Critique_r8.pdf` | Independent critique report with tornado diagram and Monte Carlo S-curves |

---

## How to use

**Online (GitHub Pages):** Click the link at the top of this README.

**Locally:** Clone or download the repository, then open `index.html` directly in a browser. No build step, no server, no dependencies to install.

```bash
git clone https://github.com/yourusername/iam-dashboard.git
cd iam-dashboard
open index.html   # macOS
# or double-click index.html in your file explorer
```

---

## Model structure

The model runs an annual time-step loop. Each year:

1. Supply curve prices update from the previous year's energy demand
2. Each sector computes a cost-advantage signal (fossil vs clean LCOE)
3. Fossil shares shift according to the signal, transition responsiveness, and deployment caps
4. Energy intensity improves via AEEI, electrification, and price effects
5. Total emissions computed from fuel-specific carbon intensities
6. CDR deploys endogenously once the carbon tax exceeds its levelised cost
7. Temperature relaxes toward equilibrium with a 15-year ocean thermal lag
8. GDP grows at the base rate minus climate damages and transition drag

### Sectors

| Sector | Energy weight | Initial fossil share |
|---|---|---|
| Power | 40% | 60% |
| Transport | 22% | 92% |
| Buildings | 16% | 46% |
| Industry | 22% | 74% |

### Key climate parameters

- **TCRE:** 0.45 °C/TtCO₂ central (IPCC AR6 median); slider range 0.26–0.78
- **Thermal inertia:** τ = 15 yr (1-box ocean model)
- **TCRE non-linearity correction:** applied above 2,000 GtCO₂ cumulative (r9)
- **2024 baseline temperature:** 1.29°C above 1850–1900 (NASA GISS)

---

## Sliders

### Policy sliders (decisions)
| Slider | Range | Effect |
|---|---|---|
| Carbon tax growth | $0–20/tCO₂/yr | Annual increment added to all sector carbon prices |
| Renewable subsidy | $0–40/MWh | Subtracted from clean LCOE in power sector |
| Grid deployment ceiling | 0.2–3.5%/yr | Hard cap on annual fossil share reduction in power |
| Green H₂ subsidy | $0–80/MWh | Reduces green hydrogen cost in industry sector |

### Uncertainty sliders (contested science)
| Slider | Range | Central | Source |
|---|---|---|---|
| TCRE | 0.26–0.78 °C/TtCO₂ | 0.45 | IPCC AR6 WG1 §5.5 |
| Transition responsiveness | 0.40–1.20 | 0.70 | Technology adoption literature |
| Fossil carbon intensity | 0.068–0.079 tCO₂/GJ | 0.072 | IEA WEO 2025 |
| CDR ceiling | 3.5–14 GtCO₂/yr | 7.0 | Expert elicitation |

---

## Known limitations

- **Single-region world.** Global aggregate only — no regional heterogeneity.
- **DICE-style damages.** Burke et al. (2015) coefficients are applied as level effects, not growth-rate effects. The High-end option understates Burke-framework cumulative damages.
- **No mandate-driven deployment.** The model relies on carbon price signals and cost economics; EV mandates, building codes, and appliance standards are not modelled. This is why Current Policy gives ~2.9°C rather than IEA STEPS's ~2.7°C.
- **CDR scale.** Strong Action does not reach 1.5°C because the model's CDR deployment (cost-triggered, 30-yr ramp) is far below IEA NZE assumptions (7.6 GtCO₂/yr by 2050).
- **No tipping elements.** Permafrost feedback, ice sheet instability, and Amazon dieback are excluded. Tail risk above ~3.5°C is likely understated.

See the [user manual](iam_user_manual.html) §10–11 for a full treatment of shortcomings and a detailed comparison with IEA scenarios.

---

## Calibration targets (2024)

| Target | Value | Model output |
|---|---|---|
| CO₂ emissions | 37.4 GtCO₂ | ✓ 37.4 GtCO₂ |
| Global temperature | 1.29°C | ✓ 1.29°C |
| Energy intensity | 7.2 EJ/T$ | ✓ 7.2 EJ/T$ |
| Global GDP | $90.5T (2015 PPP) | ✓ $90.5T |
| Global car fleet | 1.50B vehicles | ✓ 1.50B |
| EV fleet | 55M vehicles | ✓ 55M |

---

## Data sources

- IEA World Energy Outlook 2025
- IEA CO₂ Emissions from Fuel Combustion 2024
- IEA Global EV Outlook 2024
- IPCC Sixth Assessment Report (AR6) WG1 and WG3
- NASA GISS Surface Temperature Analysis 2024
- IMF World Economic Outlook 2024
- BloombergNEF New Energy Outlook 2024
- Burke et al. (2015) — damage function coefficients
- Nordhaus DICE-2023 — damage function calibration

---

## Revision history

- **r9 (May 2026):** TCRE non-linearity correction; continuous non-CO₂ abatement scaling replacing binary policy switch; Burke label corrected to High-end†; GDP damage multiplier scope documented; gas flex premium made penetration-dependent
- **r8:** Thermal inertia (1-box ocean model); damage function label corrections; CDR cost corrected; storage learning rate updated to 18%/doubling; energy intensity floor added

---

*For research and educational use. Not a substitute for GCAM, MESSAGE-GLOBIOM, or REMIND.*
