# 🦠 Foodborne Pathogen Safety Tool

A comprehensive, interactive food safety reference built as a single self-contained HTML page. No frameworks, no dependencies, no build step — just open it in a browser.

This tool was born from a simple question: *how long does it actually take to kill a pathogen at a given temperature?* That question led to a deep dive into USDA and FDA food safety data, thermal death kinetics, freeze destruction protocols, and the science behind the danger zone. The result is four interconnected tools that cover the full lifecycle of food safety — from cooking to cooling to storage.

---

## What It Does

### 🔥 Heat Inactivation
Visualizes the thermal kill curves for 10 common foodborne pathogens (Salmonella, Listeria, E. coli O157:H7, Norovirus, Campylobacter, and others) across a temperature range of 135°F to 180°F. The chart uses first-order thermal death kinetics with D-values and z-values sourced from peer-reviewed food science literature. A click-to-zoom detail view reveals what's actually happening in the "instant kill zone" at 160–165°F, where most pathogens are destroyed in under a second.

### ❄️ Freeze Inactivation
Covers 6 parasites — Anisakis simplex, Trichinella spiralis, Toxoplasma gondii, Taenia, Cyclospora, and Giardia — and the hold times required for destruction at temperatures from 20°F down to -30°F. This tab makes an important distinction that most people miss: freezing kills parasites, but it does NOT kill bacteria. Salmonella, E. coli, and Listeria survive freezing and reactivate upon thawing.

### ⏱️ Holding / Danger Zone
An interactive safety gauge that answers the most common real-world food safety question: *"my food's been sitting out — is it still safe?"* Select a holding temperature, set the elapsed time, and get an immediate SAFE / USE NOW / DISCARD verdict based on FDA time-and-temperature guidelines. The gauge incorporates the 2-hour rule (1 hour above 90°F) and the 4-hour discard threshold, with thresholds that adjust based on the selected temperature.

### 🧊 Leftover Storage Guide
A quick-reference lookup for 18 food types organized into four categories: Cooked Meats & Proteins, Prepared Dishes, Cold & Raw, and Dairy & Baked Goods. Each entry shows fridge life, freezer life, and reheat temperature with a practical safety tip. Highlights include the Bacillus cereus warning for cooked rice (the #1 leftover people get wrong), the botulism risk for foil-wrapped baked potatoes, and the hard-vs-soft cheese mold rules.

---

## Design Decisions

The tool went through 30 iterations to get here. Some of the key pivots:

**Log scale vs. linear for heat inactivation.** The original Gemini-generated chart used a log scale, which made every pathogen look like a gentle slope. Switching to a linear y-axis revealed the dramatic exponential decay — but crushed all the detail below 5 minutes into an invisible band. The solution: a linear overview with a click-to-zoom log detail view for the instant kill zone.

**Line chart vs. gauge for the danger zone.** The first attempt (V7) used a multi-line bacterial growth chart with CFU counts on a log scale. It was scientifically accurate and completely confusing. The gauge (V8) answers the same question in one glance without requiring any microbiology knowledge.

**Cooling curve vs. storage lookup.** The original cooling tab showed an FDA two-stage cooling curve (135→70°F in 2 hours, 70→41°F in 4 more hours). Technically correct, but irrelevant for most home cooks — normal portions cool fine in a standard fridge. The storage lookup (V20) answers the question people actually have: *"how long are these leftovers good for?"* The batch cooling guidance is tucked away in a collapsible section for the rare case where it matters.

**Temperature button colors.** The danger zone temp selector went through three iterations. Pure safety colors (green/red) lost the temperature context. Pure temperature colors (blue→red gradient) lost the safety signal. The final version (V17) uses temperature-gradient colors for unselected buttons but flips to green when a safe temperature (40°F or 140°F) is selected — communicating both dimensions at once.

---

## Data Integrity

All pathogen data is modeled using first-order thermal death kinetics: `log₁₀(t) = log₁₀(D_ref) − (T − T_ref) / z`, where D_ref is the decimal reduction time at reference temperature and z is the temperature change required for a 10-fold change in D-value. Parameters are approximated from published sources for 6.5–7 log reductions.

The tool uses the USDA's consumer-facing danger zone range (40°F–140°F) rather than the FDA Food Code's commercial range (41°F–135°F), providing a wider safety margin for home use. Both standards are documented in the tool's footnotes.

### Primary Sources

- USDA FSIS Appendix A — Lethality performance standards for cooked meat products
- FDA Food Code 2022 — Sections on cooking, cooling, reheating, parasite destruction, and time/temperature control
- FDA Fish & Fishery Products Hazards and Controls Guidance (4th Edition)
- Murphy et al. (2002) — D- and z-values for Salmonella and Listeria in meat products, *Journal of Food Protection*
- McMinn et al. (2018) — Validation of FSIS Appendix A across roast beef, turkey, and ham
- Sánchez-Recillas et al. (2022) — Thermal inactivation in raw milk products, *Journal of Food Protection*
- Ratkowsky et al. (1982) — Temperature-dependent bacterial growth modeling, *Journal of Bacteriology*
- Dubey (2004) — Toxoplasma freeze inactivation, *Veterinary Parasitology*
- Kotula et al. (1991) — Freezing effects on Toxoplasma gondii, *Journal of Food Protection*
- USDA ARS Pathogen Modeling Program & ComBase
- ICMSF — *Microorganisms in Foods 5: Characteristics of Microbial Pathogens*

Full citations with additional context are available in the collapsible sources section within the tool itself.

---

## Technical Details

- **Single file** — everything (HTML, CSS, JavaScript) in one self-contained `.html` file
- **Zero dependencies** — no frameworks, no CDN imports, no build tools
- **~800 lines** — compact enough to audit, substantial enough to be useful
- **Theming** — full light/dark mode via CSS custom properties
- **Unit conversion** — °F/°C toggle converts all displayed temperatures in real time
- **SVG rendering** — all charts drawn programmatically with vanilla JS and SVG elements
- **Interactive** — hover tooltips, clickable chart zones, draggable sliders, clickable gauge arc

---

## Built With AI, Verified By Humans

This tool was written with AI assistance (Anthropic Claude) and reviewed, tested, and validated by a human throughout every iteration. While AI was used to accelerate development — generating code, structuring data models, and drafting content — every data point, threshold, and safety guideline in this tool traces back to published, peer-reviewed research or official U.S. government food safety standards (USDA FSIS, FDA Food Code). No values were fabricated, estimated from training data, or assumed. The thermal death kinetics (D-values, z-values), freeze destruction timelines, danger zone thresholds, and storage durations were all cross-referenced against the primary sources listed above. The AI wrote the code; the science comes from the citations.

---

## Disclaimer

This tool is for educational and reference purposes only and does not guarantee food safety. Always use your own judgment — if food looks, smells, or feels off, discard it regardless of what any chart says. When in doubt, throw it out. See the full disclaimer within the tool for details.

---

## Future

A fifth tab for a **Pathogen Reference** is planned — pick a pathogen or a food type and see associated risks, symptoms, incubation periods, duration of illness, and high-risk populations. This would close the loop between *what to do* (tabs 1–4) and *why it matters* (tab 5).
