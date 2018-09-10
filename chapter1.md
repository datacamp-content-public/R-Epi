---
title: 'Task 1'
description: 'First Task'
---

## Random Numbers

```yaml
type: NormalExercise
key: 847996f3f9
lang: r
xp: 100
skills: 1
```

This is a first task to get familar with R and time series analysis.
We wanna start with some easy tasks. For time series analysis we need some data. So let's create some random numbers. Therefor we can use the command [runif()](https://www.rdocumentation.org/packages/compositions/versions/1.40-2/topics/runif). First argument should be the numbers of numbers you would like to generate. By default you will get shuffeld numbers between 0 and 1, but you can also add an intervall as argument. `runif(10,1,10)` will give you 10 random numbers out of 1 to 10.

`@instructions`
(i) Generate 2^15 random numbers from 0 to 1 by using `runif()` and save it to `numbers`

(ii) Plot a histogram of your random numbers using `plot(hist(#))`

`@hint`
To raise the power in R use `n^p`
To give a value to a variable use `<-`. To give 10 to the variable x use `x<-10`.

`@pre_exercise_code`
```{r}

```

`@sample_code`
```{r}
# Write your own code here!
```

`@solution`
```{r}
x <- runif(2^15)
#plot(hist(numbers))
```

`@sct`
```{r}
# Update this to something more informative.
test_error()
test_object("x",
            undefined_msg = "Make sure to define `x`!",
            incorrect_msg = "Have you correctly assigned 5 to `x`!")
success_msg("Nice! You got the first steps.")
```

---

## FFT spectrum

```yaml
type: NormalExercise
key: d9ac7746ab
xp: 100
```

We asume we have a signal with 2^15 datapoints and a sampling rate of 64 Hz. So the FFT of our signal will return 2^15 complex numbers. For now we take only the real part. Because of the nature of the FFT the signal is mirrored, we need only one half of the real part. At the end we have 2^14 - 1 frequencies in steps of 32/2^14 from 0 to (2^14-1)*32/2^14 = 32-32/2^14.

`@instructions`
1. Use your random numbers from the exercise bevore and calculate the FFT-spectrum of your "randome-time-series" of 2^15 datapoints!
2. Calculate the frequencies of the FFT-coefficients, use a sampling rate of 64 Hz!
3. Plot the FFT-spectrum (x-axis = frequencies, y-axis = FFT-coefficients)!

`@hint`
no hint given at the moment ... we need time to do so

`@pre_exercise_code`
```{r}

```

`@sample_code`
```{r}
# no test
```

`@solution`
```{r}
r_numbers<-runif(2^15)
y<-Re(fft(r_numbers))[2^14:2^15]
plot(y)
length(y)
fs<-64 #Hz
x<-seq(0, fs/2, by = (fs/2/2^14))
length(x)
plot(x=x,y=y)
```

`@sct`
```{r}

```
