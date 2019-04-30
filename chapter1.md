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

The idea of the Fourier transform is to split a signal down in sine and cosine (Sinus und Kosinus) parts with different frequencies. In the following, we will assume that:
- $n = 2^N$ represents the number of data in the signal. The algorithm if fast Fourier transform is most efficient, if the length of the data is a power of 2.
- The sampling rate of the signal, i.e. the number of measurements per second, is measured in Hz (Hertz), and given by samp_rate.
- $f_k = (k-1)/n \cdot samp_rate$ is the frequency for Fourier coefficient $a_k$.  
- Each Fourier coefficient $a_k$ shows the weight of the frequency $f_k$. The coefficients have a real part and an imaginary part.
- A Fourier Transformation will return the Fourier coefficients as an array, beginning with index 1: the coefficient a_1 belonging to frequency $f_k=0$, i.e., a constant that is equal to the sum of all data.

Now we assume that we have a signal with **$n=2^{15}$ data points** and a sampling rate of **64 Hz**. So the Fast-Fourier-Transform (FFT) of our signal will return 2^15 Fourier coefficients $a_k$. The first coefficient $a_1$ is the sum (or, in other implementations of FFT, the average) of all data, representing the vertical offset of the signal. The next $2^{14}$ coefficients, $a_k$, represent the oscillations with frequencies $f_k = k / n * samp_rate$. Because of the way FFT deals with complex numbers, the coefficients are mirrored in the following $2^{14}-1$ coefficients, $a_k$ corresponding to $a_{n-k+2}$. Based on the so-called Nyquist theorem the maximal frequency we can detect is half of the sampling rate, here 32 Hz, and always represented by the center coefficient $a_{n/2+1}$, which is not mirrored. $a_1$ is also not mirrored.

What is the frequency step (or distance) between each of the Fourier coefficients?

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
1. First we need our random numbers (last exercise)! Generate a new list of 2^15 normally distributed random numbers with average 0, and standard deviation 0.5. Save it to `numbers`
2. Calculate the FFT of this random time series by using the function [fft()](https://www.rdocumentation.org/packages/stats/versions/3.5.3/topics/fft). Save the result in `r_numbers_fft`! 
3. As mentioned before, we need only the first half of Fourier coefficients (1 to 2^14+1), which we can do with indices in brackets `[start_index:end_index]`. Note that 'end_index' must be enclosed in brackets, if it is to be calculated, e.g. by 2^14+1. 
4. Since it is difficult to plot complex numbers, we take the absolute of our Fourier coefficients. You can do it by `abs()` -- it also works for a whole list of complex numbers. Save the result in `y`

5. Create a list of frequencies from 0 to 32 Hz with steps of 32/2^14. Use `seq(start, end, step)` and save the result in `x`

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
