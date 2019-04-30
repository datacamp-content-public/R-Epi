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

This is a first task to get familiar with R and time series analysis. We will start with some easy tasks. 
For time series analysis we need some data. So let's generate random data. For this purpose, we can use the command [runif(n)](https://www.rdocumentation.org/packages/compositions/versions/1.40-2/topics/runif). The argument ```n``` should be the number of data you want to generate. You will get uncorrelated numbers (iid = identically and independently distributed), by default uniformly distributed between 0 and 1.
Another possibility is to generate iid data from a normal distribution, [rnorm(n,mean,std)](https://www.rdocumentation.org/packages/stats/versions/3.5.3/topics/Normal), where n is the number of data, mean the average value, and std is the standard deviation. 
You can list your data by using square brackets ```array[start:end]```.

**Advices for R beginners**

- You can assign values to variables by ```<-``` but also ```=``` would work. Most people use ```<-```. But be careful, for arguments in functions, only ```=``` can be used.
- If you need additional information or if you search for functions, you can use [www.rdocumentation.org](https://www.rdocumentation.org/)
- Our you can type, e.g., ?runif, in the R Console (the lower window on the right side of the screen), and a help window will pop up.
- Array **indices in R start with 1** (like in Fortran, unlike python or C, where the first element has index 0)! 
- To raise the power in R, use ```n^p```

`@instructions`
1. Generate 2^15 random numbers with a uniform distribution between 0 to 1 by using ```runif()``` and save it to ```u_numbers```

2. Generate 2^15 random numbers from a normal distribution with mean 0.5 and standard deviation 0.15. Use ```rnorm()``` and save the result to ```n_numbers```

3. Display the first 10 values of both lists

4. Let's plot your data in a histogram (the code has already been prepared for you)!

`@hint`
- Did you use ```runif(n)``` and ```rnorm(n,mean,sd)``` as described?
- Have you started with index 1?

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
plot(hist(n_numbers),col='blue')
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

## Fourier Series

```yaml
type: PureMultipleChoiceExercise
key: 045c484b8a
xp: 50
```

The idea of the Fourier series is to cut a signal down in sinus and cosinus parts. A Fourier series can be defined as follows:

Fourier series = $2\cdot a_0 + \sum _{i=1} ^{2^N-1} (a_i \cdot cos(i \cdot x) + b _i \cdot sin(i \cdot x))$ 

- $2^N$ represents the number of datapoints in the interval of the signal. 
- $i \cdot x$ describe different frequencies.
- $a$ and $b$ shows the weight of each frequency and called fourier coefficients where $a$ is the real part and $b$ the imaginar part of the complex fourier coefficients. This fourier coefficients are a measurment of appearence of frequencies.
- A Fourier Transformation will return the fourier coefficients!
- As you maybe now we can describe sinus with a cosinus function and vice versa which means the second half of the fourier coefficients is mirrored to the first half!

Now we assume that we have a signal with **$2^{15}$ data points** and a sampling rate of **64 Hz**. So the Fast-Fourier-Transformation (FFT) of our signal will return 2^15 fourier coefficients. The first coefficient $a_0$ is a constant representing more or less the offset of a signal. Because of the nature of the FFT the other $2^{15}-1 $ coefficients are mirrored at the $2^{14}+1^{th}$ data point. Based on the Nyquist Theorem (You don't need to know anything about it) the maximal frequency we can detect is half of the sampling rate, here 32 Hz!

Now the first $2^{14} + 1$ fourier coefficients representing 32 Hz. What is the Frequency step/distance between each Fourier coefficient?

`@hint`
- Hz = Hertz = 1/s, in this case data points per seconds!

`@possible_answers`
1. $\frac{32}{2^{14 + 1}}$ Hz
2. 1 Hz
3. [$\frac{32}{2^{14}}$]
4. $\frac{32}{2^{15} -1}$

`@feedback`
1. Close, but false! Remember the first coefficient is a constant, which means frequency = 0!
2. Did you read the instruction?
3. Good work! (Or just luck?!)
4. o.O False. Try again!

---

## FFT spectrum

```yaml
type: NormalExercise
key: d9ac7746ab
xp: 100
```

Now we calculate the FFT and plot a frequency spectrum!
We have a signal with **$2^{15}$ data points** and a sampling rate of **64 Hz**.

`@instructions`
1. First we need our random numbers (last exercise)! Create a new list of 2^15 normal distributed random numbers with mean of 0, and a standard deviation of 0.5. Save it in `numbers`
2. Calculate the FFT of this random time-series by using the function [fft()](https://www.rdocumentation.org/packages/stats/versions/3.5.3/topics/fft). Save the result in `r_numbers_fft`! 
3. As mentioned before we need only the first half of fourier coefficients (1 to 2^14+1), which we can do with indices in brackets `[start_index:end_index]`. It is difficult to plot complex numbers thats why we use the absolute of our fourier coefficients. You can do it by `abs()` (It works also about a whole list of complex numbers). Save the result in `y`!
 
4. Create a list of frequencies from 0 to 32 Hz with steps of 32/2^14. Use `seq(start,end, step)` and save the result in `x`

5. Plot the FFT-spectrum (x-axis = frequencies, y-axis = FFT-coefficients)! Use `plot(x=x-values, y=y-values)`!

`@hint`
- Do you remember the function ```rnorm(n,mean,sd)```?

`@pre_exercise_code`
```{r}

```

`@sample_code`
```{r}
# Have a lot of fun with this funny exercise.
# Creat a list of 2^15 normal distributed random numbers (mean = 0, sd = 0.5)
numbers <- 

# Calculate the FFT of r_numbers
numbers_fft <- 

# Calculate the absolute and take only the first half of the fft-result (replace _)
y <- abs(numbers_fft[_:_])

# Calculate the frequencies of each fourier coefficient.
x <- 

# Plot the spectogram

```

`@solution`
```{r}
# Have a lot of fun with this funny exercise.
# Creat a list of 2^15 normal distributed random numbers (mean = 0, sd = 0.5)
numbers <- rnorm(2^15,0,0.5)

# Calculate the FFT of r_numbers
numbers_fft <- fft(numbers) 

# Calculate the absolute and take only the first half of the fft-result
y <- abs(numbers_fft[1:(2^14+1)])

# Calculate the frequencies of each fourier coefficient.
x <- seq(0, 32, by = (32/2^14))

# Plot the spectogram
plot(x=x,y=y)
```

`@sct`
```{r}
ex() %>% check_object("numbers") #%>% check_equal()
ex() %>% check_object("numbers_fft") #%>% check_equal()
ex() %>% check_function("fft") %>% check_result() %>% check_equal()
ex() %>% check_object("y") %>% check_equal(incorrect_msg="Did you use round brackets for operations in the index area e.g. list[1:(2^2 +1)]?")
ex() %>% check_object("x") %>% check_equal()
ex() %>% check_function("seq") %>% check_result() %>% check_equal()
ex() %>% check_error()
ex() %>% check_function("plot") %>% check_result() %>% check_equal()
success_msg("Great!")

```
