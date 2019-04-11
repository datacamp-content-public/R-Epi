---
title: Respiration
description: ""
---

## Load and inspect data

```yaml
type: NormalExercise
key: 8dc2739bea
xp: 100
```

In the last example we couldn't actually see anything happens. Thats why we investigate in the next step real data. To load data we use here [scan()](https://www.rdocumentation.org/packages/base/versions/3.5.3/topics/scan). The data is a time series of a temperature sensor placed to the nostrils. This is a common way to record respiration. 



`@instructions`
Let's start with loading your data. 
1. Load the data from ```path_data``` to ```data``` by using ```scan()```. The first argument is the path. 
2. The data has a sampling rate of 4 Hz. Create a time series of the time and store it to ```time```. You can check the length of a vector by ```length(vector)```. Check the length of time and data, maybe you have to adjust your time series. 
3. Now plot the data to see how the data looks like. If you like, you can set the x-label and y-label with arguments ```xlab``` and ```ylab```.
4. The plot from task 2 isn't quit well. Let's plot again, but only the second hour of data and with a **line plot**, by adding ```"l"``` to the arguments of ```plot()```

`@hint`
- Do you remember the ```seq(start,end,step)``` command to create the time series of the time?!
- 4 Hz = 4 values per second => step = 0.25
- You can plot by ```plot(x=time-values,y=data-values,xlab='x-label',ylab='y-label')```.

`@pre_exercise_code`
```{r}
path_data = "https://assets.datacamp.com/production/repositories/3401/datasets/d191ac1f6ae2fda3392c4d41b892ba8bd2822bf3/atmung.dat"
```

`@sample_code`
```{r}
# Load the data
data <- 

# Create time
time <- 

# Plot the data

# define start and end
start <- 
end <- 

# Plot only the second houre as line-plot
```

`@solution`
```{r}
# Load the data
data <- scan(path_data)

# Create time
time <- seq(0,length(data)/4-0.25,0.25)

# Plot the data
plot(x=time,y=data)

# define start and end
start <- 240
end <- 480

# Plot only the second houre as line-plot
plot(x=time[start:end],y=data[start:end],"l")
```

`@sct`
```{r}
ex() %>% check_error()
ex() %>% check_function("scan") %>% check_result() %>% check_equal()
ex() %>% check_object("data") %>% check_equal()
ex() %>% check_function("seq") %>% check_result() %>% check_equal()
ex() %>% check_object("time") %>% check_equal()
ex() %>% check_object("start") %>% check_equal()
ex() %>% check_object("end") %>% check_equal()
ex() %>% check_function("plot") %>% check_result() %>% check_equal()
ex() %>% check_function("plot") %>% check_result() %>% check_equal()
success_msg("You plotted a very nice graph!")
```

---

## Frequency spectrum of our respiration data

```yaml
type: NormalExercise
key: 4617f36b93
xp: 100
```

In the following exercise we will calculate a frequency spectrum, to investigate the respiration frequency of our dataset.


`@instructions`
The respiration data is still stored in ```data```.
1. Calculate the fourier-transformation of the data and save fourier-coefficients to ```fft```. Use a power of 2 as number of data points. 

2. Save the absolute of the fourier-coeeficients to ```y```.

3. Calculate the corresponding frequencies of the fourier-coefficients and store them to ```freq```

4. Plot the frequency spectrum.

5. Correct the frequency spectrum by 1/frequency

`@hint`


`@pre_exercise_code`
```{r}
path_data = "https://assets.datacamp.com/production/repositories/3401/datasets/d191ac1f6ae2fda3392c4d41b892ba8bd2822bf3/atmung.dat"
data = scan(path_data)


```

`@sample_code`
```{r}
# Calculate the fourier-coefficients
fft <-

# Calculate the frequencies
freq <- 

# Plot the frequency spectrum


```

`@solution`
```{r}
# Calculate the fourier-coefficients
fft <- fft(data[0:2^10-1]) 

# Calculate the absolute and take only the second half of the fft-result
y <- abs(r_numbers_fft)[2^9:2^10]

# Calculate the frequencies of each fourier coefficient.
x <- seq(0, 32, by = (32/2^14))

# Plot the spectogram
plot(x=x,y=y)
```

`@sct`
```{r}

```
