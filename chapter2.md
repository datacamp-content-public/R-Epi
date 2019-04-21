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
1. Load the data ```respiration.dat``` to ```data``` by using ```scan()```. The first argument is the path. The file is located in your current directory. 
2. The data has a sampling rate of 4 Hz. Create a time series of the time and store it to ```time```. You can check the length of a vector by ```length(vector)```.   
3. Now plot the data to see how the data looks like. If you like, you can set the x-label and y-label with arguments ```xlab=x-label``` and ```ylab=y-label``` to the arguments of ```plot()```.
4. The plot from task 3 isn't quit well. Let's plot again, but only the second minute of data and with a **line plot**, by adding ```"l"``` (minuscle of L) to the arguments of ```plot()```. But first create the ```start``` and ```end``` index for the second minute and then use it in the ```plot``` command.

`@hint`
- 4 Hz = 4 values per second => step = 0.25
- Do you remember the ```seq(start,end,step)``` command to create the time series of the time?!  
- If the lengths of ```data``` and ```time``` doesn't match, you need to reduce maybe the time vector - you can do this by subtracting one step (0.25) from the step argument in ```seq(start,end,by=step)```.
- You can plot by ```plot(x=time-values,y=data-values,xlab='x-label',ylab='y-label')```.

`@pre_exercise_code`
```{r}
download.file(url = "https://assets.datacamp.com/production/repositories/3401/datasets/d191ac1f6ae2fda3392c4d41b892ba8bd2822bf3/atmung.dat", destfile = "respiration.dat")

```

`@sample_code`
```{r}
# Load the file "respiration.dat"
data <- 

# Create time
time <- 

# Plot the data


# define start and end



# Plot only the first houre as line-plot

```

`@solution`
```{r}
# Load the file "respiration.dat"
data <- scan("respiration.dat")

# Create time
time <- seq(0,length(data)/4-0.25,by=0.25)

# Plot the data
plot(x=time,y=data)

# define start and end
start <- 240
end <- 480

# Plot only the first houre as line-plot
plot(x=time[start:end],y=data[start:end],"l")
```

`@sct`
```{r}

ex() %>% check_function("scan") %>% check_result() %>% check_equal()
ex() %>% check_object("data") %>% check_equal()
ex() %>% check_function("seq") %>% check_result() %>% check_equal()
ex() %>% check_object("time") %>% check_equal()
ex() %>% check_object("start") %>% check_equal()
ex() %>% check_object("end") %>% check_equal()
ex() %>% check_error()
ex() %>% check_function("plot",index=1) %>% check_result() %>% check_equal()
ex() %>% check_function("plot",index=2) %>% check_result() %>% check_equal()
success_msg("You plotted a very nice graph!")
```

---

## Insert exercise title here

```yaml
type: PureMultipleChoiceExercise
key: b986615cd9
xp: 50
```

What is a normal respiration frequency? (no physical acitvity)

`@hint`
Take some breaths ;-)

`@possible_answers`
1. 10 Hz
2. [0.3 Hz]
3. 1 Hz 
4. 0.001 Hz

`@feedback`
1. Do you really breathe 10 times a second?? 
2. Correct! Normally the respiration frequency 0.15 and 0.5 Hz.
3. Close. During physical activities you can reach up to 1 Hz.
4. I am scared, are you taking only a breath every 1000 seconds?

---

## Frequency spectrum of our respiration data

```yaml
type: NormalExercise
key: 4617f36b93
xp: 100
```

[Bild](https://assets.datacamp.com/production/repositories/3401/datasets/3ef69998a7434569bb3df0dc7e33b09c2b827e65/respiration.png) In the last exercise we got this plot. Now we would like to know the respiration frequency - and we can calculate determine it by fourier transformation

`@instructions`
The respiration data is still stored in ```data```.
1. You remember the FFT works best with data lengths of the power of 2. Set ```n``` as highest possible power of 2. So maybe you have to check the length of data first. Hint: You can take the console as a calculator too.

2. Calculate the fourier-transformation of the data and save fourier-coefficients to ```fft```. Use a power of 2 as number of data points. 

3. Save the absolute of the fourier-coefficients to ```abs_ff```.

4. Calculate the corresponding frequencies of the fourier-coefficients and store them to ```freq```

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
n <- 

# Calculate the fourier-coefficients
fft <-  

# Calculate absolute of fft and take the first halfe
abs_fft <- 

# Set sampling frequency fs
fs <- 

# Calculate frequencies
freq <- 

# Plot the spectogram as lineplot


```

`@solution`
```{r}
# set length of data (power of 2!)
n <- 2^10

# Calculate the fourier-coefficients
fft <- fft(data[0:n])/n 

# Calculate absolute of fft and take the first halfe
abs_fft <- abs(fft[0:(n/2)])

# Set sampling frequency fs
fs <- 4

# Calculate frequencies
freq <- seq(n/2)*fs/n

# Plot the spectogram
plot(x=freq,y=abs_fft,'l')
```

`@sct`
```{r}
ex() %>% check_error()
ex() %>% check_object("n") %>% check_result() %>% check_equal()
ex() %>% check_object("fft") %>% check_equal()
ex() %>% check_function("fft") %>% check_result() %>% check_equal()
ex() %>% check_object("abs_fft") %>% check_result() %>% check_equal()
ex() %>% check_object("fs") %>% check_result() %>% check_equal()
ex() %>% check_object("frq") %>% check_result() %>% check_equal()
ex() %>% check_function("plot") %>% check_result() %>% check_equal()
success_msg("Check out your frequency spectrum. What is the respiration frequency? Is it plausible?")
```
