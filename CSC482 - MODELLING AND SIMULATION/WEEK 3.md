# Statistics

Statistics is the science of collecting, organizing, compiling, computing, summarizing, analysing, and interpreting data used in decision making and passing information to the public.

## Uses of Statistics

1. Planning and budgeting.
    
2. Used to access the variation of a system.
    
3. It aids the studying of association between 2 objects or systems.
    
4. Disseminating research findings.
    

## Measures of Central Tendency

Mean, median, and mode are measures of central tendencies. They help you describe the center or typical value of a data set.

### Finding Mean, Median, and Mode

Given the data set: 1, 2, 2, 3, 4, 5, 2.

- **Mean:**
    
    $$\text{Mean} = \frac{1 + 2 + 3 + 2 + 4 + 5 + 2}{7} = \frac{19}{7} \approx 2.714$$
- **Median:** Arrange the data in order: 1, 2, 2, 2, 3, 4, 5. The middle value is 2.
    
    $$\text{Median} = 2$$
- **Mode:** The most frequent number is 2.
    
    $$\text{Mode} = 2$$

### When to Use Mean, Median, Mode

- **Mean:** Best for normal numerical data.
    
- **Median:** Best when data have extreme values.
    
- **Mode:** Best for the most common category or value.
    

## Grouped Data: Mean, Median, and Mode

Given the following frequency distribution of the height of a data set.

|   |   |   |   |   |
|---|---|---|---|---|
|**Height**|**Frequency (f)**|**Midpoint (x)**|**fx**|**Cumulative Frequency (CF)**|
|1-3|3|2|6|3|
|4-6|4|5|20|7|
|7-9|7|8|56|14|
|10-12|18|11|198|32|
|13-15|6|14|84|38|
|16-18|10|17|170|48|
|19-21|2|20|40|50|

**Formulas for Grouped Data**

- **Median Formula:**
    
    $$\text{Median} = L_m + \left[ \frac{\frac{n}{2} - F_c}{f_m} \right] \times c$$
- **Mode Formula:**
    
    $$\text{Mode} = L_m + \left[ \frac{f_1 - f_0}{2f_1 - f_0 - f_2} \right] \times c$$

_(Note: Based on the provided values:_ $L_m = 10$_,_ $n = 50$_,_ $F_c = 14$_,_ $f_m = 18$_,_ $c = 3$_,_ $f_0 = 7$_,_ $f_1 = 18$_,_ $f_2 = 6$_.)_

## Variance and Standard Deviation

Variance and standard deviation measure data dispersion. They show you how far data points spread from the mean (average).

- **Variance (**$\sigma^2$**):** The average of squared differences from the mean.
    
- **Standard Deviation (**$\sigma$**):** The square root of the variance. It returns the measurements to the original units.
    

You prefer standard deviation for data interpretation, while variance is crucial for further statistical analysis. In the interpretation of variance, lower values mean data is clustered near the mean. High values indicate greater spread.

### Variance Formula

$$\sigma^2 = \frac{\sum fx^2}{\sum f} - \left[ \frac{\sum fx}{\sum f} \right]^2$$$$\sigma = \sqrt{\sigma^2}$$

### Example 1: Variance and Standard Deviation

The following table shows student performance in an examination. Find the mean, median, mode, variance, and standard deviation.

|   |   |   |   |   |   |
|---|---|---|---|---|---|
|**Marks**|**f**|**x (midpoint)**|**x2**|**fx2**|**fx**|
|30-34|3|32|1024|3072|96|
|35-39|6|37|1369|8214|222|
|40-44|13|42|1764|22932|546|
|45-49|18|47|2209|39762|846|
|50-54|15|52|2704|40560|780|
|55-59|7|57|3249|22743|399|
|60-64|4|62|3844|15376|248|
|65-69|2|67|4489|8978|134|
|**Sum**|**68**|**349**||**161637**|**3271**|

**Calculating Variance (**$\sigma^2$**):**

$$\sigma^2 = \frac{\sum fx^2}{\sum f} - \left[ \frac{\sum fx}{\sum f} \right]^2$$$$\sigma^2 = \frac{161637}{68} - \left[ \frac{3271}{68} \right]^2$$$$\sigma^2 = 2377.0147 - (48.1029)^2$$$$\sigma^2 = 2377.0147 - 2313.8929 = 63.1218$$

**Calculating Standard Deviation (**$\sigma$**):**

$$\sigma = \sqrt{63.1218} \approx 7.9449$$

### Example 2: Class Interval Variance

|   |   |   |   |   |   |
|---|---|---|---|---|---|
|**Class**|**f**|**x (midpoint)**|**fx**|**x2**|**fx2**|
|1-10|5|5.5|27.5|30.25|151.25|
|11-20|8|15.5|124|240.25|1922|
|21-30|12|25.5|306|650.25|7803|
|31-40|7|35.5|248.5|1260.25|8821.75|
|41-50|3|45.5|136.5|2070.25|6210.75|
|**Sum**|**35**||**842.5**||**24908.75**|

**Calculating Variance (**$\sigma^2$**):**

$$\sigma^2 = \frac{\sum fx^2}{\sum f} - \left[ \frac{\sum fx}{\sum f} \right]^2$$$$\sigma^2 = \frac{24908.75}{35} - \left[ \frac{842.5}{35} \right]^2$$$$\sigma^2 = 711.6786 - (24.0714)^2$$$$\sigma^2 = 711.6786 - 579.4323 = 132.2463$$

**Calculating Standard Deviation (**$\sigma$**):**

$$\sigma = \sqrt{132.2463} \approx 11.4998$$

## Correlation Coefficient

The correlation coefficient is a number that tells you how strongly two variables are related and in what direction. In statistics, the common one is Pearson's correlation coefficient. It measures the linear relationship between 2 variables.

The result of a correlation coefficient is between -1 and +1.

- If the result is +1, it means you have a perfect positive correlation (both variables increase together).
    
- If the result is -1, it means you have a perfect negative correlation (inverse proportion).
    
- If the result is 0, it means there is a null linear relationship.
    

### Correlation Coefficient Formula

$$r = \frac{n\sum xy - \sum x \sum y}{\sqrt{[n\sum x^2 - (\sum x)^2][n\sum y^2 - (\sum y)^2]}}$$

### Example 1: Calculating Correlation

|   |   |   |   |   |
|---|---|---|---|---|
|**x**|**y**|**xy**|**x2**|**y2**|
|5|70|350|25|4900|
|7|62|434|49|3844|
|8|63|504|64|3969|
|10|65|650|100|4225|
|12|45|540|144|2025|
|15|50|750|225|2500|
|17|22|374|289|484|
|18|48|864|324|2304|
|20|40|800|400|1600|
|23|25|575|529|625|
|**Sum: 135**|**490**|**5841**|**2149**|**26476**|

Here, $n = 10$.

$$r = \frac{10(5841) - (135)(490)}{\sqrt{[10(2149) - (135)^2][10(26476) - (490)^2]}}$$$$r = \frac{58410 - 66150}{\sqrt{(21490 - 18225)(264760 - 240100)}}$$$$r = \frac{-7740}{\sqrt{(3265)(24660)}}$$$$r = \frac{-7740}{\sqrt{80514900}}$$$$r = \frac{-7740}{8973.0095} \approx -0.8626$$

### Example 2: Hours Studied vs. Exam Grade

The following table shows hours studied and exam grade. Variable $x$ represents hours studied, while $y$ represents exam grade.

|   |   |   |   |   |
|---|---|---|---|---|
|**x**|**y**|**xy**|**x2**|**y2**|
|1|40|40|1|1600|
|2|50|100|4|2500|
|3|60|180|9|3600|
|4|70|280|16|4900|
|5|80|400|25|6400|
|**Sum: 15**|**300**|**1000**|**55**|**19000**|

Here, $n = 5$.

$$r = \frac{5(1000) - (15)(300)}{\sqrt{[5(55) - 15^2][5(19000) - 300^2]}}$$$$r = \frac{5000 - 4500}{\sqrt{[275 - 225][95000 - 90000]}}$$$$r = \frac{500}{\sqrt{50 \times 5000}} = \frac{500}{\sqrt{250000}} = \frac{500}{500}$$$$r = 1$$

**OR**

Using the alternative formula:

$$r = \frac{\sum (x - \bar{x})(y - \bar{y})}{\sqrt{\sum (x - \bar{x})^2 \cdot \sum (y - \bar{y})^2}}$$

First, find the mean of $x$ and $y$:

$$\bar{x} = \frac{\sum x}{n} = \frac{15}{5} = 3$$$$\bar{y} = \frac{\sum y}{n} = \frac{300}{5} = 60$$

Then, complete the table:

|   |   |   |   |   |
|---|---|---|---|---|
|**x−xˉ**|**y−yˉ​**|**(x−xˉ)2**|**(y−yˉ​)2**|**(x−xˉ)(y−yˉ​)**|
|-2|-20|4|400|40|
|-1|-10|1|100|10|
|0|0|0|0|0|
|1|10|1|100|10|
|2|20|4|400|40|
|**Sum: 0**|**0**|**10**|**1000**|**100**|

Now apply the sums to the formula:

$$r = \frac{100}{\sqrt{10 \times 1000}} = \frac{100}{\sqrt{10000}} = \frac{100}{100}$$$$r = 1$$