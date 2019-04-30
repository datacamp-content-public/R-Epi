---
title: 'Advanced tasks'
description: ""
---

## Generate long-term correlated data

```yaml
type: NormalExercise
key: 6743cda87e
xp: 100
```

Long-term correlated random numbers can be generated via Fourier filtering. We start with iid Gaussian distributed data. Then FFT is used to transfer the data into the frequency space, where each Fourier coefficient must be multiplied by the inverse square-root if the corresponding frequency. This procedure must also be applied to the mirror coefficients. After inverse FFT, we obtain the desired (artificial) long-term correlated data, specifically: 1/f noise.

`@instructions`
1. Generate iid Gaussian distributed data with zero mean and unit variance, n=2^12.
2. Apply FFT.
3. Multiply all Fourier coefficients by sqrt(frequency). Do not forget the mirror part.
4. Apply inverse FFT
5. Plot the data.

Why does one get the impression that there are trends in some parts of the time series? 

How does the appearance change if an exponent smaller or larger than one is added to the term sqrt(frequency)?

`@hint`


`@pre_exercise_code`
```{r}

```

`@sample_code`
```{r}

```

`@solution`
```{r}

```

`@sct`
```{r}

```

---

## Prepare a double-logarithmic plot

```yaml
type: NormalExercise
key: a47975b5c5
xp: 100
```

The detection and analysis of long-term correlations and scaling behavior is based on studying double-logarithmic plots of the power spectrum.

`@instructions`
1. Calculate the power spectrum (= square of absolute value of the Fourier transform) of the long-term correlated data from the previous exercise.
2. Plot the logarithm of the power versus the logarithm of the frequency. Note that the zero-frequency point must be omitted in such plots, since log(0) = -Infinity.
3. Include a straight line with slope -1 into your plot, so that one can compare with the scaling behavior expected for 1/f noise.

Check the procedure for time series with scaling behavior different from 1/f noise (see previous excercise).

`@hint`


`@pre_exercise_code`
```{r}

```

`@sample_code`
```{r}

```

`@solution`
```{r}

```

`@sct`
```{r}

```
