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
To give a value to a variable use `<-`. Example: to give 10 to the variable x use `x<-10`.

`@pre_exercise_code`
```{r}

```

`@sample_code`
```{r}
# Write your own code here!
```

`@solution`
```{r}
numbers <- runif(2^15)
plot(hist(numbers))
```

`@sct`
```{r}
ex() %>% check_error()
ex() %>% check_object("numbers") %>% check_equal()
ex() %>% check_function("plot") %>% check_result() %>% check_equal()
success_msg("Good job! You have finished your first R-program!")
```

---

## FFT spectrum

```yaml
type: NormalExercise
key: d9ac7746ab
xp: 100
```

We asume we have a signal with 2^15 datapoints and a sampling rate of 64 Hz. So the FFT of our signal will return 2^15 complex numbers. For now we take only the **real part**. Because of the nature of the FFT the signal is mirrored, we need only one half of the real part. At the end we have 2^14 - 1 frequencies in steps of 32/2^14 from 0 to (2^14-1)*32/2^14 = 32-32/2^14.

`@instructions`
Use your random numbers from the exercise bevore and calculate the FFT-spectrum of your "randome-time-series" of 2^15 datapoints!
But let's to it step by step: 
1. First we need our random numbers (last exercise)! Create a new list of 2^15 random numbers and save it in `r_numbers`
2. Calculate the FFT of this random time-series by using the function `fft()`. Save the result in `r_numbers_fft`! As mentioned bevore we need only the **real part** there for we will use the function `Re()` and we need only the second halfe of the real part of the fft-signal, which we can do with indices in brackets `[start_index:end_index]`. Save the result in `y`!
 
1. Calculate the frequencies of the FFT-coefficients, use a sampling rate of 64 Hz!
2. Plot the FFT-spectrum (x-axis = frequencies, y-axis = FFT-coefficients)!

`@hint`
no hint given at the moment ... we need time to do so

`@pre_exercise_code`
```{r}

```

`@sample_code`
```{r}
# Have a lot of fun with this funny exercise.
# Creat a list of 2^15 random numbers
r_numbers <- 

# Calculate the FFT of r_numbers
r_numbers_fft <-  

# 
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
ex() %>% check_error()
ex() %>% check_object("r_numbers") %>% check_equal()
#ex() %>% check_function("plot") %>% check_result() %>% check_equal()
success_msg("Are you the next time-series-master?!")

```
