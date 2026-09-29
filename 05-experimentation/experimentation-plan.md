# A/B Experiment Brief, StreamLine (B2C)

## Parameters
| Parameter | Decision |
|---|---|
| Feature under test | Spotlight Curated Rail a hand-picked selection of titles that sits at the top of every subscriber's homepage |
| Persona | The Casual Browser |
| Expected outcome | will find something to watch faster |
| Primary success metric | +2 percentage point increase in the share of homepage sessions with a play start within 3 minutes |
| Baseline rate | 10% |
| Guardrail metric | weekly viewing hours per subscriber |
| Guardrail boundary | the 95% confidence interval's lower bound must stay above −2% relative to control |
| Second guardrail | · |
| Minimum Detectable Effect | + 2 |
| Sample size per arm | 4,311 |
| Traffic split | 50/50 |
| Test duration | 14 days |
| Significance threshold | .05 |

## Control vs. Variant
- **Control (A):** The current homepage, with every existing row in its current order and no Spotlight rail
- **Variant (B):** The current homepage with a Spotlight rail inserted as row 1: exactly 8 title cards, fully visible without scrolling at 1280×720 (desktop) and 390×844 (mobile). All existing rows move down one position and are otherwise unchanged. Adult profiles only. The prototype control bar (profile switcher and A/B toggle) is removed from the test build.
- **Held constant (isolation check):** The content and order of all other rows, the recommendation and ranking algorithm, search, title artwork, pricing and promotions, app version, and the curated title list, which is frozen for the full test

## Hypothesis
> I believe that Spotlight Curated Rail a hand-picked selection of titles that sits at the top of every subscriber's homepage for The Casual Browser will result in will find something to watch faster, as measured by a + 2 change in +2 percentage point increase in the share of homepage sessions with a play start within 3 minutes within 14 days. We will protect weekly viewing hours per subscriber throughout the test.

## Shipping criteria
> We will **ship** if +2 percentage point increase in the share of homepage sessions with a play start within 3 minutes improves by ≥ + 2 at .05 and weekly viewing hours per subscriber does not reach the 95% confidence interval's lower bound must stay above −2% relative to control after 14 days.
> We will **iterate** if direction is positive but lift is below the MDE.
> We will **kill** if the primary metric shows no improvement or moves negatively.
> The read date is fixed at the end of 14 days, no results reviewed before this date.
