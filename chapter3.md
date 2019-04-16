---
title: 'EEG Analysis'
description: ""
---

## Introduction to EEG

```yaml
type: PureMultipleChoiceExercise
key: f7c74cb697
xp: 50
```

[Electroencephalography](https://en.wikipedia.org/wiki/Electroencephalography) is an electrophysiological monitoring method to record electrical activity of the brain. The human body transfers information by neurons. This cells emitting singals via changes of electrochemical potentials. Invivo normally a single neuronal activity can't detected. But in thought process mostly hundreds of cells are involved, why we can measure electrical potentials on the scalp.

The signals measured in milli volts (mv) and need some simple pre-processing to obtain information. At the end of this part we can compare $\alpha$-, $\beta$- and $\delta$-bands. 

$\alpha$-waves (8-13 Hz):
- relaxed and awake, but closed eyes

$\beta$-waves (>13 Hz):
- during mental oder physical activity or psychological burden

$\delta$-waves (0,5-3 Hz):
- deep sleep

What can we obtain from EEGs?

`@hint`
no hint

`@possible_answers`
1. Physical activity
2. [Detection and classification of sleep stages]
3. A random telephone number
4. The age of a subject/patient

`@feedback`
1. Not at all.
2. Right. Changes in the amplitude of different EEG-bands can be used to detect sleep episodes.
	Sleep stages are defined by EEG-bands! 
3. No.
4. False.

---

## Selection of EEG bands

```yaml
type: TabExercise
key: 8c2fda9edb
xp: 100
```

In this task we will focus on one EEG channel and how to extract EEG bands!
The EEG data has a sampling rate of **1024 Hz** and is recorded about **17 minutes and 4 seconds**. The values are in mV.
Be aware that the calculations in this part can take some minutes.

`@pre_exercise_code`
```{r}
download.file(url = "https://assets.datacamp.com/production/repositories/3401/datasets/636762184295f3f3370287b8a7a20cbc48aa5ae6/eeg_c4m1.rds", destfile = "eeg1.rds")

```

***

```yaml
type: NormalExercise
key: 8a6c00c5b8
xp: 50
```

`@instructions`
Load & Plot
1. Load the compressed EEG file ```eeg1.rds``` with ```readRDS(file)``` to ```eeg1```
2. Create a time series in minutes for your EEG data and store it in ```time```
2. Plot the EEG data, y-axis in mV and x-axis in minutes

`@hint`
- To create a time series use ```seq(number of points-1)``` afterwards you can divide the whole array by the step size in minutes.
- Step size in minutes = 1/(sampling rate * 60)

`@sample_code`
```{r}
# Load compressed EEG data "eeg1.rds"
eeg1 <- readRDS("eeg1.rds")

# Create time 
time <- seq(length(eeg1))/(1024*60)

# Plot data 

```

`@solution`
```{r}
# Load compressed EEG data "eeg1.rds"
eeg1 <- readRDS("eeg1.rds")

# Create time 
time <- seq(length(eeg1))/(1024*60)

# Plot data 
#plot(time,eeg1)
```

`@sct`
```{r}
ex() %>% check_error()
ex() %>% check_object("eeg1") %>% check_equal()
ex() %>% check_function("readRDS") %>% check_result() %>% check_equal()
ex() %>% check_object("time") %>% check_equal()
ex() %>% check_function("seq") %>% check_result() %>% check_equal()
#ex() %>% check_function("plot") %>% check_result() %>% check_equal()
success_msg("You finished the first step to an EEG analyse!")
```

***

```yaml
type: NormalExercise
key: c7e1e99971
xp: 50
```

`@instructions`
Now we need the FFT and therfore the max power of 2 in the length of the signal.
1. Check out the highest power of 2 in the singal length and store it to ```n```. (You can use the R console as a calculator)
2. Calculate the FFT and store it to ```fft```. Use only a data length of power of 2.
fft <- 
3. Find the frequencies that corresponded to the fourier-coefficients!
freq <-

`@hint`
- Do you remember the function of the FFT ```fft()```?
- The Nyquist-Theroem says: We can only resolve frequencies up to half of the sampling frequency -> We have a maximum frequency of ```(sampling frequency)/2```. 
- Furthermore the FFT-signals contains as much data points as the original signal = n, but it is mirrored in the middle, which means we have only ```n/2``` frequencies. 
- Now use ```seq(0,n/2-1)*(sampling frequency)/n``` to obtain a series of the frequencies.

`@sample_code`
```{r}
# Load compressed EEG data "eeg1.rds"
eeg1 <- readRDS("eeg1.rds")

# Create time 
time <- seq(length(eeg1))/1024/60

# max power of 2 in the length of the signal
n <- 2^20

# Calculate the FFT
fft_eeg1 <- fft(eeg1)

# Calculate the frequencies
freq <- seq(512)*512/2^19
```

`@solution`
```{r}
# Load compressed EEG data "eeg1.rds"
eeg1 <- readRDS("eeg1.rds")

# Create time 
time <- seq(length(eeg1))/1024/60

# max power of 2 in the length of the signal
n <- 2^20

# Calculate the FFT
fft_eeg1 <- fft(eeg1)

# Calculate the frequencies
freq <- seq(0,n/2-1)*1024/n
```

`@sct`
```{r}
ex() %>% check_error()
ex() %>% check_object("eeg1") %>% check_equal()
ex() %>% check_function("readRDS") %>% check_result() %>% check_equal()
ex() %>% check_object("time") %>% check_equal()
ex() %>% check_object("n") %>% check_equal()
ex() %>% check_object("fft_eeg1") %>% check_equal()
ex() %>% check_function("fft") %>% check_result() %>% check_equal()
ex() %>% check_object("freq") %>% check_equal()
ex() %>% check_function("seq") %>% check_result() %>% check_equal()
#ex() %>% check_function("plot") %>% check_result() %>% check_equal()
success_msg("You finished the first step to an EEG analyse!")
```
