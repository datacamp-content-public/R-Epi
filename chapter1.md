---
title: 'Short Introduction to FFT'
description: ""
free_preview: true
---

## Random Numbers

```yaml
type: NormalExercise
key: 847996f3f9
lang: r
xp: 100
skills: 1
```

This is a first task to get familiar with R and time series analysis.
We will start with some easy tasks. For time series analysis we need some data. So let's create some random numbers. Therefor we can use the command [runif(n)](https://www.rdocumentation.org/packages/compositions/versions/1.40-2/topics/runif). The argument ```n``` should be the number of numbers you would like to generate. By default you will get shuffeld numbers between 0 and 1, uniform distributed.
Another possibility is to pull random numbers from a normal distribution. [rnorm(n,mean,std)](https://www.rdocumentation.org/packages/stats/versions/3.5.3/topics/Normal) where n is the number of numbers, mean the mean of the distribution with standard deviation std. 
You can show your data by using square brackets ```array[start:end]```.

**Advices for R beginners**

- You can assign values to variables by ```<-``` but also ```=```. Most people uses ```<-```. But be carful for arguments in functions only ```=``` can be used.
- If you need additional information or if you search function you can use [www.rdocumentation.org](https://www.rdocumentation.org/)
- Our you can type e.g. ?runif, in the R Consol (lower window on the right siede) and a help window will pop up.
- The **indices starts in R with 1** like in Fortran and not with 0 like in python or C! 
- To raise the power in R use ```n^p```

`@instructions`
1. Generate 2^15 random numbers from a uniform distribution between 0 to 1 by using ```runif()``` and save it to ```u_numbers```

2. Generate 2^15 random numbers from a normal distribution with mean 0.5 and std 0.15. Use ```rnorm()``` and save the result to ```n_numbers```

3. Print the first 10 values of both lists

4. Let's plot your data in a histogram!

`@hint`


`@pre_exercise_code`
```{r}

```

`@sample_code`
```{r}
# Write your own code here!
# Create a list of 2^15 random numbers
u_numbers <-

# Create a list of 2^15 normal distributed numbers
n_numbers <-

# Print the first 10 values of both lists (replace _)
u_numbers[_:_]
n_numbers[_:_]

# Lets plot your histograms (do not change!)
plot(hist(n_numbers),col='blue')
hist(u_numbers,col=rgb(1, 0, 0, alpha=0.5),add=T)
```

`@solution`
```{r}
# Write your own code here!
# Create a list of 2^15 random numbers
u_numbers <- runif(2^15)

# Create a list of 2^15 normal distributed numbers
n_numbers <- rnorm(2^15,0.5,0.15)

# Print the first 10 values of both lists (replace _)
u_numbers[1:10]
n_numbers[1:10]

# Lets plot your histograms (do not change!)
plot(hist(n_numbers),col='blue')a
hist(u_numbers,col=rgb(1, 0, 0, alpha=0.5),add=T)
```

`@sct`
```{r}
ex() %>% check_error()
ex() %>% check_function("runif") %>% check_arg("n") %>% check_equal()
ex() %>% check_object("u_numbers")
ex() %>% check_function("rnorm") %>% {
  check_arg(.,"n") %>% check_equal()
  check_arg(.,"mean") %>% check_equal()
  check_arg(.,"sd") %>% check_equal()
  }
ex() %>% check_object("n_numbers")
ex() %>% check_code("u_numbers[1:10]",fixed=TRUE)
ex() %>% check_code("n_numbers[1:10]",fixed=TRUE)
ex() %>% check_function("plot",not_called_msg="Did you remove the plot-function?")
ex() %>% check_function("hist",not_called_msg="Did you remove the hist-function?")
success_msg("Good job! You finished the first task of our Workshop!")
```

---

## FFT spectrum

```yaml
type: NormalExercise
key: d9ac7746ab
xp: 100
```

We assume that we have a signal with 2^15 data points and a sampling rate of 64 Hz. So the Fast-Fourier-Transformation (FFT) of our signal will return 2^15 complex numbers. Because of the nature of the FFT the signal is mirrored, we need only the first half. Based on the Nyquist Theorem we can detect frequencies 

At the end we have 2^14 - 1 frequencies in steps of 32/2^14 from 0 to (2^14-1)*32/2^14 = 32-32/2^14.

`@instructions`
Use your random numbers from the exercise bevore and calculate the FFT-spectrum of your "randome-time-series" of 2^15 datapoints!
But let's to it step by step: 
1. First we need our random numbers (last exercise)! Create a new list of 2^15 random numbers and save it in `r_numbers`
2. Calculate the FFT of this random time-series by using the function `fft()`. Save the result in `r_numbers_fft`! As mentioned bevore we need only the absolute there for we will use the function `abs()` and we need only the first half of the real part of the fft-signal, which we can do with indices in brackets `[start_index:end_index]`. Save the result in `y`!
 
At this point we have already our y-values also called fourier-coefficients for the FFT-spectrum. Each fourier-coefficient respresents a frequeuncy. Now we will calculate this frequencies:
3. Create a list of frequencies from 0 to 32 Hz with steps of 32/2^14. Use `seq(start,end, step)` and save the result in `x`

2. Plot the FFT-spectrum (x-axis = frequencies, y-axis = FFT-coefficients)! Use `plot(x=x-values, y=y-values)`!

`@hint`
`no` hint given at the moment ... we need time to do ```so```

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

# Calculate the absolute and take only the second half of the fft-result
y <-

# Calculate the frequencies of each fourier coefficient.
x <-

# Plot the spectogram
```

`@solution`
```{r}
# Have a lot of fun with this funny exercise.
# Creat a list of 2^15 random numbers
r_numbers <- runif(2^15)

# Calculate the FFT of r_numbers
r_numbers_fft <- fft(r_numbers) 

# Calculate the absolute and take only the second half of the fft-result
y <- abs(r_numbers_fft)[0:2^14]

# Calculate the frequencies of each fourier coefficient.
x <- seq(32)*32/2^14 #seq(0, 32-(32/2^14), by = (32/2^14))

# Plot the spectogram
#plot(x=x,y=y)
```

`@sct`
```{r}
ex() %>% check_error()
ex() %>% check_object("r_numbers") %>% check_equal()
ex() %>% check_object("r_numbers_fft") %>% check_equal()
ex() %>% check_function("fft") %>% check_result() %>% check_equal()
ex() %>% check_object("y") %>% check_equal()
ex() %>% check_function("Re") %>% check_result() %>% check_equal()
ex() %>% check_object("x") %>% check_equal()
ex() %>% check_function("seq") %>% check_result() %>% check_equal()
ex() %>% check_function("plot") %>% check_result() %>% check_equal()
success_msg("Great!")

```

---

## Insert exercise title here

```yaml
type: NormalExercise
key: a042f2b0b5
xp: 100
```



`@instructions`


`@hint`


`@pre_exercise_code`
```{r}
x = c(1,2,3,4,5)
```

`@sample_code`
```{r}
y = x[1:2]
```

`@solution`
```{r}
y = x[1:2]
```

`@sct`
```{r}
ex() %>% check_code("y = x[1:2]",fixed=TRUE)
```
