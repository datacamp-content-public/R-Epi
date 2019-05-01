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

In the last example we could not actually obtain results from our Fourier spectra. Therefore we will now investigate real respiratory data. To load the data we use here [scan()](https://www.rdocumentation.org/packages/base/versions/3.5.3/topics/scan). The data is a time series measured with a temperature sensor placed at the nostrils. This is a common way to record respiratory activity.

`@instructions`
Let's start with loading your data. 
1. Load the data ```respiration.dat``` to ```data``` by using ```scan()```. The first argument is the path. The file is located in your current directory. 
2. The data has a sampling rate of 4 Hz. Create a time series of the time and store it to ```time```. You can check the length of a vector by ```length(vector)```.   
3. Now plot the data to see what it looks like. You can set the x-label and y-label by adding ```xlab=x-label``` and ```ylab=y-label``` to the arguments of ```plot()```.
4. The plot from task 3 is not quite nice. Let's plot again, but only the second minute of data and with a **line plot**, by adding ```"l"``` (minuscle of L) to the arguments of ```plot()```. But first create the ```start``` and ```end``` index for the second minute, and then use it in the ```plot``` command.

`@hint`
- 4 Hz = 4 values per second => step = 0.25
- Do you remember the ```seq(start,end,step)``` command to create the time series of the time?!  
- If the lengths of ```data``` and ```time``` do not match, you need to reduce maybe the time vector - you can do this by subtracting one step (0.25) from the step argument in ```seq(start,end,by=step)```.
- You can plot by ```plot(x=time-values,y=data-values,xlab='x-label',ylab='y-label')```.

`@pre_exercise_code`
```{r}
download.file(url = "https://assets.datacamp.com/production/repositories/3401/datasets/d191ac1f6ae2fda3392c4d41b892ba8bd2822bf3/atmung.dat", destfile = "respiration.dat")

```

`@sample_code`
```{r}
# Load the file "respiration.dat"
data <- 

# Create time vector
time <- 

# Plot the data


# Define start and end
start <-
end <-

# Plot only the second minute as line-plot

```

`@solution`
```{r}
# Load the file "respiration.dat"
data <- scan("respiration.dat")

# Create time
time <- seq(0,length(data)/4-0.25,0.25)

# Plot the data
plot(x=time,y=data)

# Define start and end
start <- 240
end <- 480

# Plot only the second minute as a line-plot
plot(x=time[start:end],y=data[start:end],"l")
```

`@sct`
```{r}
ex() %>% check_function("scan") %>% check_arg("file") %>% check_equal()
ex() %>% check_object("data") %>% check_equal()

ex() %>% check_function("seq") %>% {
  check_arg(.,"from")
  check_arg(.,"to")
  check_arg(.,"by")  
} %>% check_equal()
ex() %>% check_object("time") %>% check_equal()

ex() %>% check_object("start") %>% check_equal()
ex() %>% check_object("end") %>% check_equal()

ex() %>% check_function("plot",index=1) %>% check_result() %>% check_equal()
ex() %>% check_function("plot",index=2) %>% check_result() %>% check_equal()

ex() %>% check_error()
success_msg("You plotted a very nice graph!")
```

---

## Physiology of respiratory activity

```yaml
type: PureMultipleChoiceExercise
key: b986615cd9
xp: 50
```

What is a normal respiratory frequency? (no physical acitvity)

`@hint`
Take some breaths ;-)

`@possible_answers`
1. 10 Hz
2. [0.3 Hz]
3. 1 Hz 
4. 0.01 Hz

`@feedback`
1. Do you really breathe 10 times per second?? 
2. Correct! Normally, the respiration frequency 0.15 and 0.4 Hz (at least according to the 1996 guidelines for heart rate variability analysis).
3. Close. During physical activities you can reach up to 1 Hz.
4. I am scared, are you taking only a breath each 100 seconds?

---

## Frequency spectrum of our respiration data

```yaml
type: NormalExercise
key: 4617f36b93
xp: 100
```

In the last programming exercise we got this plot. ![](https://assets.datacamp.com/production/repositories/3401/datasets/3ef69998a7434569bb3df0dc7e33b09c2b827e65/respiration.png). Now we would like to know the respiration frequency -- and we can determine it by fourier-transformation. The data is still available and has a **sampling rate of 4 Hz**.

`@instructions`
The respiration data is still stored in ```data```.
1. You probably remember that the FFT works best with data of lengths equal to a power of 2. Set ```n``` as lowest possible power of 2 so that the data still fits into the array. Maybe you have to check the length of data first. Hint: You can take the console as a calculator, too.

2. Calculate the fast-Fourier-transform of the data and save the Fourier coefficients to ```fft_data```. Use a power of 2 as number of data points. Zero entries will be added to the array, if you specify a range that exceeds the actual length of the data.

3. Save the absolute of the Fourier coefficients to ```abs_fft_data```, representing the frequencies from 0 to 2 Hz and no mirror coefficients.

4. Calculate the corresponding frequencies of the Fourier coefficients and store them to ```freq```.

5. Plot the frequency spectrum.

`@hint`


`@pre_exercise_code`
```{r}
path_data = "https://assets.datacamp.com/production/repositories/3401/datasets/d191ac1f6ae2fda3392c4d41b892ba8bd2822bf3/atmung.dat"
data = scan(path_data)


```

`@sample_code`
```{r}
# set length of data (power of 2!)
n <- 2^10

# Calculate the fourier-coefficients
fft_data <- fft(data[1:n])

# Calculate absolute of fft and take the first halfe
abs_fft_data <- abs(fft_data[1:((n/2)+1)])

# Calculate frequencies
freq <- seq(0, 2, 2/2^9)

# Plot the spectogram
plot(x=freq,y=abs_fft_data,"l")
```

`@solution`
```{r}
# set length of data (power of 2!)
n <- 2^10

# Calculate the fourier-coefficients
fft_data <- fft(data[1:n])

# Calculate absolute of fft and take the first halfe
abs_fft_data <- abs(fft_data[1:((n/2)+1)])

# Calculate frequencies
freq <- seq(0, 2, 2/2^9)

# Plot the spectogram
plot(x=freq,y=abs_fft_data,"l")
```

`@sct`
```{r}
ex() %>% check_error()
ex() %>% check_object("n") %>% check_equal()
ex() %>% check_object("fft_data") %>% check_equal()
ex() %>% check_function("fft") %>% check_result() %>% check_equal()
ex() %>% check_object("abs_fft_data") %>% check_equal()
ex() %>% check_object("freq") %>% check_equal()
ex() %>% check_function("plot") %>% check_result() %>% check_equal()
success_msg("Check out your frequency spectrum. What is the respiration frequency? And what represents the other peak? Is it plausible?")
```

---

## What does the frequency spectrum tell us?

```yaml
type: PureMultipleChoiceExercise
key: bcc8e7e8a1
xp: 50
```

![](https://assets.datacamp.com/production/repositories/3401/datasets/e9a75a9779ec5042622026a258642cf8d64b8e62/respiration_spectrum.png)
Let us check our frequency spectrum of the respiration data again. 
What is the respiration frequency?

`@hint`


`@possible_answers`
1. 0.01 Hz
2. [0.22 Hz]
3. 0.44 Hz

`@feedback`
1. No.
2. Right!
3. This is the first harmonic of the respiration frequency, but not the actual respiration frequency.
