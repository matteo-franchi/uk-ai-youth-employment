# Pre-analysis plan

*Written before looking at any post-2022 data. The date of the commit that adds this file is the proof. Everything below is fixed: it will be followed whatever the results.*

## Question

Did the release of ChatGPT (30 November 2022) reduce employment of young workers in UK sectors most exposed to large language models?

This project revisits an essay I wrote at the University of Warwick (2025), which reported a 70% fall in entry-level hiring. That result came from very small survey cells and from one specification out of several. This plan fixes the analysis in advance.

## Part 1. Why the original result was fragile (Labour Force Survey)

- **Data:** Quarterly Labour Force Survey, 30 calendar quarters from January–March 2019 to April–June 2026 (UK Data Service). Microdata are not redistributed in this repository.
- **Cell:** the essay's definition. Aged 21–25, in the current job for less than 3 months, working in Information & Communication (J), Finance & Insurance (K), Professional, Scientific & Technical (M), Construction (F) or Accommodation & Food (I). Variable names to be confirmed from the LFS user guide.
- **Output:** the **unweighted** number of respondents in the cell, by quarter and sector group. No weighted estimates, no regression.
- **Also reported:** the naive difference-in-differences recomputed from Table 1 of the essay, and the fact that the essay's regression (Table 2) cannot be reproduced from it.

## Part 2. What the data show now (HMRC payroll data)

- **Data:** HMRC, *UK payrolled employments by nationality, region, industry, age and sex, July 2014 to December 2025* (Open Government Licence). Administrative data covering all PAYE employments.
- **Window:** all available data, July 2014 to December 2025. Nothing is dropped, COVID included.
- **Frequency:** quarterly (average of the three monthly counts).
- **Counts used:** nationality "Total", female + male.
- **Treated sectors:** J, K, M. **Control sectors:** F, I. Same as the essay.
- **Outcome (triple difference):** for each sector and quarter,
  `y = log(employments aged 18–24) − log(employments aged 25–34)`.
  This removes shocks that hit a whole sector (for example interest rates). Comparing treated with control sectors then removes shocks that hit all young workers.
- **Treatment date:** ChatGPT was released on 30 November 2022. The first full quarter after release, January–March 2023, is the first post-treatment quarter.
- **Reference quarter:** October–December 2022, the last quarter before treatment. All coefficients are measured relative to it.
- **Model (event study):** `y` regressed on sector fixed effects, quarter fixed effects, sector × calendar-quarter fixed effects (to absorb each sector's usual seasonal pattern, since the data are not seasonally adjusted), and one treated × quarter coefficient for every quarter except the reference. The coefficients are shown in a single chart.
- **Uncertainty:** heteroskedasticity-robust standard errors. With only five sectors, confidence intervals are indicative, and this is stated in the results.

## Decision rule for pre-trends

- **Test:** on the quarters before treatment, estimate a linear trend in the treated-minus-control gap. If its slope differs from zero at the 5% level, pre-trends are taken to diverge.
- **If pre-trends do not diverge:** post-treatment coefficients are read as the estimated effect, with the limits stated below.
- **If pre-trends diverge:** the pre-treatment linear trend (estimated on July 2014 – December 2022) is extended into the post period, and the post-treatment coefficients are compared with that line.
  - If they fall faster than the line: "an acceleration compatible with an effect of LLMs, not proof of one".
  - If they do not: "no sign of an effect".
- In both cases, no alternative specification is used to change the conclusion.

## Known limits, stated in advance

- HMRC counts jobs held (a stock), not new hires. It measures employment by age, not entry-level status.
- Age bands differ between the two parts: 21–25 in the LFS, 18–24 in HMRC.
- Sector is the employer's industry, not the worker's occupation.
- Other shocks in 2023 (tech layoffs, high interest rates) can coincide with the treatment date.

## Anything else

Any analysis not described here will be labelled **exploratory** and will not change the main conclusion.
