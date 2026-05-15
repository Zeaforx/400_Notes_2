# Week 3: Statistics

## What is Statistics?

**Statistics** is the science of collecting, organising, compiling, computing, summarising, analysing, and interpreting data for the purpose of decision-making and passing information to the public.

In the context of modelling and simulation, statistics is essential because:

- Real-world data feeds into models as input.
- Simulation outputs are themselves statistical (especially in stochastic models).
- We need statistical tools to compare scenarios, draw conclusions, and quantify uncertainty.

## Uses of Statistics

1. **Planning and budgeting** — governments and businesses use statistics to project demand, allocate resources, and forecast revenue.
2. **Assessing the variation of a system** — measuring how much output fluctuates around its average tells us whether the system is stable.
3. **Studying associations between two objects or systems** — does class attendance correlate with exam scores? Does fuel price affect transport demand?
4. **Disseminating research findings** — research becomes trustworthy when supported by statistical evidence.

---

# Measures of Central Tendency

**Mean, median, and mode** are the three classical *measures of central tendency*. They describe the **centre** or **typical value** of a data set — answering the question, *"What is a representative single number for this data?"*

---

## Ungrouped Data

### Worked Example

Given the data set: **1, 2, 2, 3, 4, 5, 2**

#### Mean (Arithmetic Average)

Add all values and divide by the count:

$$\text{Mean} = \frac{1 + 2 + 3 + 2 + 4 + 5 + 2}{7} = \frac{19}{7} \approx 2.714$$

#### Median (Middle Value)

First, arrange in ascending order: **1, 2, 2, 2, 3, 4, 5**

The middle value (the 4th of 7 values) is **2**.

$$\text{Median} = 2$$

> If the data set has an *even* number of values, the median is the average of the two middle values.

#### Mode (Most Frequent Value)

The number 2 appears three times — more than any other. So:

$$\text{Mode} = 2$$

> A data set can have **no mode** (all values appear once), **one mode** (unimodal), or **multiple modes** (bimodal, multimodal).

---

### When to Use Each Measure

| Measure | Best For | Example |
|---------|----------|---------|
| **Mean** | Normal, symmetric numerical data without extreme outliers. | Average exam score in a class. |
| **Median** | Data with extreme values (outliers) or skewed distributions. | Average income (since billionaires distort the mean). |
| **Mode** | Categorical data, or finding the most common category. | Most popular shoe size sold in a shop. |

> **Real-world example of why this matters:** If Aliko Dangote walks into a room of 99 average Nigerians, the *mean* net worth jumps to millions of dollars — meaningless for describing a "typical" person in the room. The *median* barely moves, giving a more honest summary.

---

## Grouped Data: Mean, Median, and Mode

When data is presented in a frequency distribution (grouped into class intervals), the formulas change because individual values are no longer known — only the class each value falls into.

### Example Frequency Distribution: Height Data

| Height | Frequency ($f$) | Midpoint ($x$) | $fx$ | Cumulative Frequency (CF) |
|--------|------|------|------|-----|
| 1 – 3   | 3  | 2  | 6   | 3  |
| 4 – 6   | 4  | 5  | 20  | 7  |
| 7 – 9   | 7  | 8  | 56  | 14 |
| 10 – 12 | 18 | 11 | 198 | 32 |
| 13 – 15 | 6  | 14 | 84  | 38 |
| 16 – 18 | 10 | 17 | 170 | 48 |
| 19 – 21 | 2  | 20 | 40  | 50 |
| **Total** | **$\sum f = 50$** | | **$\sum fx = 574$** | |

The **midpoint** is the average of the lower and upper class limits (e.g., for class 1–3: midpoint = (1+3)/2 = 2). We treat all values in a class as if they sit at the midpoint.

### Mean of Grouped Data

$$\bar{x} = \frac{\sum fx}{\sum f} = \frac{574}{50} = 11.48$$

### Median Formula (Grouped)

$$\text{Median} = L_m + \left[ \frac{\frac{n}{2} - F_c}{f_m} \right] \times c$$

Where:
- $L_m$ = lower class boundary of the median class
- $n$ = total frequency
- $F_c$ = cumulative frequency *before* the median class
- $f_m$ = frequency of the median class
- $c$ = class width

The **median class** is the class containing the $(n/2)^{\text{th}}$ observation. Here, $n/2 = 25$, and the cumulative frequency just reaches/exceeds 25 in the class **10–12** (CF goes from 14 to 32). So the median class is 10–12.

Plug in: $L_m = 10$, $n = 50$, $F_c = 14$, $f_m = 18$, $c = 3$:

$$\text{Median} = 10 + \left[ \frac{25 - 14}{18} \right] \times 3 = 10 + \frac{11}{18} \times 3 = 10 + 1.833 = 11.83$$

### Mode Formula (Grouped)

$$\text{Mode} = L_m + \left[ \frac{f_1 - f_0}{2f_1 - f_0 - f_2} \right] \times c$$

Where:
- $L_m$ = lower class boundary of the modal class (class with highest frequency)
- $f_1$ = frequency of the modal class
- $f_0$ = frequency of the class *before* the modal class
- $f_2$ = frequency of the class *after* the modal class
- $c$ = class width

The **modal class** is 10–12 (frequency 18, the highest). So $L_m = 10$, $f_1 = 18$, $f_0 = 7$, $f_2 = 6$, $c = 3$:

$$\text{Mode} = 10 + \left[ \frac{18 - 7}{2(18) - 7 - 6} \right] \times 3 = 10 + \frac{11}{23} \times 3 = 10 + 1.435 = 11.43$$

---

# Variance and Standard Deviation

Where central tendency tells us about the *centre* of the data, **variance** and **standard deviation** tell us about the **spread** — how far data points typically lie from the mean.

## Why Spread Matters

Two data sets can have the same mean but behave very differently.

> **Example:** Two students both have an average of 70 over five exams.
> - Student A scored: 70, 70, 70, 70, 70 (very consistent).
> - Student B scored: 50, 60, 70, 80, 90 (highly variable).
>
> The mean alone hides this difference. The standard deviation reveals it.

## Definitions

- **Variance ($\sigma^2$)** — the *average of squared differences from the mean*. Squaring eliminates negative signs and emphasises large deviations.
- **Standard Deviation ($\sigma$)** — the *square root of variance*. Taking the square root returns the value to the original units (so a variance of 25 marks² becomes a standard deviation of 5 marks).

### Interpretation

- **Low variance / standard deviation** → data clusters tightly around the mean (consistent, predictable).
- **High variance / standard deviation** → data is widely spread (volatile, unpredictable).

We typically **report standard deviation** because it shares the units of the data and is easier to interpret. **Variance** is more useful for further statistical analysis (e.g., ANOVA, regression).

## Formula for Grouped Data

$$\sigma^2 = \frac{\sum fx^2}{\sum f} - \left[ \frac{\sum fx}{\sum f} \right]^2$$

$$\sigma = \sqrt{\sigma^2}$$

> This is the *population* variance formula. It is essentially $E[X^2] - (E[X])^2$ — the expected value of the square minus the square of the expected value.

---

## Worked Example 1: Student Performance

The following table shows student performance in an examination. Find the mean, variance, and standard deviation.

| Marks   | $f$ | $x$ (midpoint) | $x^2$ | $fx^2$ | $fx$ |
|---------|-----|------|--------|---------|------|
| 30 – 34 | 3   | 32 | 1,024 | 3,072  | 96   |
| 35 – 39 | 6   | 37 | 1,369 | 8,214  | 222  |
| 40 – 44 | 13  | 42 | 1,764 | 22,932 | 546  |
| 45 – 49 | 18  | 47 | 2,209 | 39,762 | 846  |
| 50 – 54 | 15  | 52 | 2,704 | 40,560 | 780  |
| 55 – 59 | 7   | 57 | 3,249 | 22,743 | 399  |
| 60 – 64 | 4   | 62 | 3,844 | 15,376 | 248  |
| 65 – 69 | 2   | 67 | 4,489 | 8,978  | 134  |
| **Total** | **68** | | | **161,637** | **3,271** |

### Mean

$$\bar{x} = \frac{\sum fx}{\sum f} = \frac{3{,}271}{68} \approx 48.10$$

### Variance

$$\sigma^2 = \frac{\sum fx^2}{\sum f} - \left[ \frac{\sum fx}{\sum f} \right]^2 = \frac{161{,}637}{68} - (48.10)^2$$

$$\sigma^2 = 2{,}377.01 - 2{,}313.89 = 63.12$$

### Standard Deviation

$$\sigma = \sqrt{63.12} \approx 7.94$$

**Interpretation:** Students scored on average about 48 marks, and most fall within roughly ±8 marks of that average. About 68% of students (under a normal-distribution assumption) scored between 40 and 56.

---

## Worked Example 2: Class Intervals

| Class   | $f$ | $x$ (midpoint) | $fx$ | $x^2$ | $fx^2$ |
|---------|-----|------|--------|---------|----------|
| 1 – 10  | 5  | 5.5  | 27.5  | 30.25   | 151.25   |
| 11 – 20 | 8  | 15.5 | 124   | 240.25  | 1,922    |
| 21 – 30 | 12 | 25.5 | 306   | 650.25  | 7,803    |
| 31 – 40 | 7  | 35.5 | 248.5 | 1,260.25| 8,821.75 |
| 41 – 50 | 3  | 45.5 | 136.5 | 2,070.25| 6,210.75 |
| **Total** | **35** | | **842.5** | | **24,908.75** |

### Mean

$$\bar{x} = \frac{842.5}{35} \approx 24.07$$

### Variance

$$\sigma^2 = \frac{24{,}908.75}{35} - (24.07)^2 = 711.68 - 579.43 = 132.25$$

### Standard Deviation

$$\sigma = \sqrt{132.25} \approx 11.50$$

**Interpretation:** This data set has a higher standard deviation (11.5) than the student-marks data (7.94), meaning it is *more spread out* relative to its centre.

---

# Correlation Coefficient

The **correlation coefficient** is a single number that tells you **how strongly two variables are related** and **in what direction**. The most common version is **Pearson's correlation coefficient**, which measures the *linear* relationship between two variables.

## Range and Meaning

The coefficient $r$ always falls in the range $[-1, +1]$:

| Value | Meaning |
|-------|---------|
| $r = +1$ | **Perfect positive correlation** — both variables increase together exactly in step. |
| $0 < r < 1$ | Positive correlation — they tend to increase together. |
| $r = 0$ | **No linear relationship** — knowing one tells you nothing linear about the other. |
| $-1 < r < 0$ | Negative correlation — when one increases, the other tends to decrease. |
| $r = -1$ | **Perfect negative correlation** — exact inverse relationship. |

### Rough Strength Guide

| $\lvert r \rvert$ | Strength |
|-------------------|----------|
| 0.0 – 0.2 | Very weak / none |
| 0.2 – 0.4 | Weak |
| 0.4 – 0.6 | Moderate |
| 0.6 – 0.8 | Strong |
| 0.8 – 1.0 | Very strong |

> **Important caution:** Correlation is **not causation**. Ice-cream sales and drowning rates are positively correlated, but ice cream does not cause drowning — both increase in summer. This is why correlation must be interpreted carefully.

## Formula

$$r = \frac{n\sum xy - \sum x \sum y}{\sqrt{[n\sum x^2 - (\sum x)^2][n\sum y^2 - (\sum y)^2]}}$$

Where $n$ is the number of paired observations.

---

## Worked Example 1: Calculating Correlation

| $x$ | $y$ | $xy$ | $x^2$ | $y^2$ |
|-----|-----|------|---------|---------|
| 5   | 70  | 350  | 25     | 4,900   |
| 7   | 62  | 434  | 49     | 3,844   |
| 8   | 63  | 504  | 64     | 3,969   |
| 10  | 65  | 650  | 100    | 4,225   |
| 12  | 45  | 540  | 144    | 2,025   |
| 15  | 50  | 750  | 225    | 2,500   |
| 17  | 22  | 374  | 289    | 484     |
| 18  | 48  | 864  | 324    | 2,304   |
| 20  | 40  | 800  | 400    | 1,600   |
| 23  | 25  | 575  | 529    | 625     |
| **$\sum x = 135$** | **$\sum y = 490$** | **$\sum xy = 5{,}841$** | **$\sum x^2 = 2{,}149$** | **$\sum y^2 = 26{,}476$** |

With $n = 10$:

$$r = \frac{10(5{,}841) - (135)(490)}{\sqrt{[10(2{,}149) - 135^2][10(26{,}476) - 490^2]}}$$

**Numerator:**
$$10(5{,}841) - (135)(490) = 58{,}410 - 66{,}150 = -7{,}740$$

**Denominator:**
$$10(2{,}149) - 18{,}225 = 21{,}490 - 18{,}225 = 3{,}265$$
$$10(26{,}476) - 240{,}100 = 264{,}760 - 240{,}100 = 24{,}660$$
$$\sqrt{3{,}265 \times 24{,}660} = \sqrt{80{,}514{,}900} \approx 8{,}973.01$$

**Result:**

$$r = \frac{-7{,}740}{8{,}973.01} \approx -0.8626$$

**Interpretation:** $r \approx -0.86$ indicates a **strong negative correlation** — as $x$ increases, $y$ tends to decrease in a fairly consistent linear way.

---

## Worked Example 2: Hours Studied vs. Exam Grade

Let $x$ represent hours studied and $y$ represent exam grade.

| $x$ | $y$ | $xy$ | $x^2$ | $y^2$ |
|-----|-----|------|---------|---------|
| 1   | 40  | 40   | 1      | 1,600   |
| 2   | 50  | 100  | 4      | 2,500   |
| 3   | 60  | 180  | 9      | 3,600   |
| 4   | 70  | 280  | 16     | 4,900   |
| 5   | 80  | 400  | 25     | 6,400   |
| **$\sum x = 15$** | **$\sum y = 300$** | **$\sum xy = 1{,}000$** | **$\sum x^2 = 55$** | **$\sum y^2 = 19{,}000$** |

With $n = 5$:

$$r = \frac{5(1{,}000) - (15)(300)}{\sqrt{[5(55) - 15^2][5(19{,}000) - 300^2]}}$$

$$r = \frac{5{,}000 - 4{,}500}{\sqrt{(275 - 225)(95{,}000 - 90{,}000)}} = \frac{500}{\sqrt{50 \times 5{,}000}} = \frac{500}{\sqrt{250{,}000}} = \frac{500}{500} = 1$$

**Interpretation:** $r = 1$ — a **perfect positive correlation**. Every extra hour of study adds exactly 10 marks. (Real exam data is never this clean — this is a textbook scenario.)

---

### Alternative Form: Using Deviations from the Mean

The same correlation can also be computed using deviations from the mean:

$$r = \frac{\sum (x - \bar{x})(y - \bar{y})}{\sqrt{\sum (x - \bar{x})^2 \cdot \sum (y - \bar{y})^2}}$$

First find the means:

$$\bar{x} = \frac{15}{5} = 3, \quad \bar{y} = \frac{300}{5} = 60$$

Then build the deviation table:

| $x - \bar{x}$ | $y - \bar{y}$ | $(x - \bar{x})^2$ | $(y - \bar{y})^2$ | $(x - \bar{x})(y - \bar{y})$ |
|------|------|------|------|------|
| -2   | -20  | 4    | 400  | 40   |
| -1   | -10  | 1    | 100  | 10   |
| 0    | 0    | 0    | 0    | 0    |
| 1    | 10   | 1    | 100  | 10   |
| 2    | 20   | 4    | 400  | 40   |
| **0** | **0** | **10** | **1,000** | **100** |

(Note: the deviation columns always sum to zero — a useful check.)

Apply the formula:

$$r = \frac{100}{\sqrt{10 \times 1{,}000}} = \frac{100}{\sqrt{10{,}000}} = \frac{100}{100} = 1$$

Same answer ✓ — the two formulas are mathematically equivalent.

---

## Why Statistics Matters for Modelling and Simulation

Bringing this back to the course theme:

- When **collecting input data** for a simulation, you compute means and standard deviations to describe the distributions feeding your model.
- When **analysing simulation output** (especially from stochastic models), you need statistics to summarise multiple runs and assess uncertainty.
- **Correlation** helps detect relationships between variables you may want to include in or exclude from your model.
- **Variance** is central to assessing whether a system is stable or volatile — a key question in queueing, manufacturing, finance, and reliability engineering.

In short: you cannot model what you cannot measure, and statistics gives us the language for measurement.
