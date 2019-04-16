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

## Load and plot EEG data

```yaml
type: NormalExercise
key: 14525283ea
xp: 100
```

In this and the following task we will focus on one EEG channel and how to extract EEG bands!

First we need to load the data.

The EEG data has a sampling rate of **1024 Hz** and is recorded about **17 minutes and 4 seconds**. The values are in mV.
Be aware that the calculations in this part can take some minutes.

`@instructions`
1. Load the compressed EEG file ```eeg1.rds``` with ```readRDS(file)``` to ```eeg1```
2. Create a time series in minutes for your EEG data and store it in ```time```
2. Plot the EEG data, y-axis in mV and x-axis in minutes

`@hint`
- To create a time series use ```seq(number of points-1)``` afterwards you can divide the whole array by the step size in minutes.
- Step size in minutes = 1/(sampling rate * 60)

`@pre_exercise_code`
```{r}
download.file(url = "https://assets.datacamp.com/production/repositories/3401/datasets/636762184295f3f3370287b8a7a20cbc48aa5ae6/eeg_c4m1.rds", destfile = "eeg1.rds")

```

`@sample_code`
```{r}
# Load compressed EEG data "eeg1.rds"
eeg1 <- 

# Create time 
time <- 

# Plot data 

```

`@solution`
```{r}
# Load compressed EEG data "eeg1.rds"
eeg1 <- readRDS("eeg1.rds")

# Create time 
time <- seq(length(eeg1))/(1024*60)

# Plot data 
plot(time,eeg1)
```

`@sct`
```{r}
ex() %>% check_error()
ex() %>% check_object("eeg1") %>% check_equal()
ex() %>% check_function("readRDS") %>% check_result() %>% check_equal()
ex() %>% check_object("time") %>% check_equal()
ex() %>% check_function("seq") %>% check_result() %>% check_equal()
ex() %>% check_function("plot") %>% check_result() %>% check_equal()
success_msg("You finished the first step to an EEG analyse!")
```

---

## Frequency spectrum of EEG data

```yaml
type: NormalExercise
key: 3bfc4232e8
xp: 100
```

Now we will calculate the FFT of your EEG data and plot the frequncy spectrum.

The EEG data has a sampling rate of **1024 Hz** and is recorded about **17 minutes and 4 seconds**. The values are in mV.
Be aware that the calculations in this part can take some minutes.

`@instructions`
Now we need the FFT and therfore the max power of 2 in the length of the signal.
1. Check out the highest power of 2 in the singal length and store it to ```n```. (You can use the R console as a calculator)
2. Calculate the FFT and store it to ```fft```. Use only a data length of power of 2.
3. Determine the frequencies (```freq```) that corresponded to the fourier-coefficients.

`@hint`
- Do you remember the function of the FFT ```fft()```?
- The Nyquist-Theroem says: We can only resolve frequencies up to half of the sampling frequency -> We have a maximum frequency of ```(sampling frequency)/2```. 
- Furthermore the FFT-signals contains as much data points as the original signal = n, but it is mirrored in the middle, which means we have only ```n/2``` frequencies. 
- Now use ```seq(0,n/2-1)*(sampling frequency)/n``` to obtain a series of the frequencies.```
- Plot: Absolute = ```abs()```, you can choose data ranges by square bracktes e.g.: data[1:10]

`@pre_exercise_code`
```{r}
download.file(url = "https://assets.datacamp.com/production/repositories/3401/datasets/636762184295f3f3370287b8a7a20cbc48aa5ae6/eeg_c4m1.rds", destfile = "eeg1.rds")
# Load compressed EEG data "eeg1.rds"
eeg1 <- readRDS("eeg1.rds")
# Create time 
time <- seq(length(eeg1))/1024/60
```

`@sample_code`
```{r}
# Your EEG data is still stored in eeg1
# max power of 2 in the length of the signal
n <- 

# Calculate the FFT
fft_eeg1 <- 

# Calculate the frequencies
freq <- 

# Plot the data, but use only the absolute of the first half of the fft

```

`@solution`
```{r}
# Your EEG data is still stored in eeg1
# max power of 2 in the length of the signal
n <- 2^20

# Calculate the FFT
fft_eeg1 <- fft(eeg1)

# Calculate the frequencies
freq <- seq(0,n/2-1)*1024/n

# Plot the data, but use only the absolute of the first half of the fft
plot(freq,abs(fft_eeg1[1:2^19]))
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
success_msg("You created successfull a frequency spectrum!")
```

---

## Bandpass filter

```yaml
type: NormalExercise
key: a2a114075b
xp: 100
```

Now we come to the highlight of this session, the bandpass filter or how to select specific frequencies of my signal and suppress other frequencies.

Lets start with the $\alpha$-band of an EEG. You remember it contains the frequency range from 8 to 13 Hz. The basic principle is now to use our frequency domain, and set all fourier coefficients to 0, whose frequencies are out of 8 to 13 Hz. But don't forget the mirrored part. 

The EEG data has a sampling rate of **1024 Hz** and is recorded about **17 minutes and 4 seconds**. The values are in mV.
Be aware that the calculations in this part can take some minutes.

`@instructions`
1. Define your minimum frequency and your maximum frequency of the $\alpha$-band.
2. Iterate over all frequencies ```freq``` and set the fourier coefficients to 0, whom not in the $\alpha$-band. Don't forget the mirrored part.

`@hint`
- ```for (i in 1:10) {}``` will iterate i from 1 to 10.
- ```|``` and ```&``` are the logical OR and AND of R 
- As the ```fft_eeg1``` is symmetric, you have to set ```fft_eeg1[i]``` and ```fft_eeg1[n-i]``` to 0.

`@pre_exercise_code`
```{r}
download.file(url = "https://assets.datacamp.com/production/repositories/3401/datasets/636762184295f3f3370287b8a7a20cbc48aa5ae6/eeg_c4m1.rds", destfile = "eeg1.rds")
# Load compressed EEG data "eeg1.rds"
eeg1 <- readRDS("eeg1.rds")
# max power of 2 in the length of the signal
n <- 2^20
# Calculate the FFT
fft_eeg1 <- fft(eeg1)
# Calculate the frequencies
freq <- seq(0,n/2-1)*1024/n
```

`@sample_code`
```{r}
# eeg1, n = 2^20, fft_eeg1 and freq still available
# Set your minimum frequency and your maximum frequency
freq_min <- 8
freq_max <- 13

# Set all fourier coefficients 0, which not between freq_min and freq_max
for (i in 1:(n/2)) {
  if ((freq[i]<freq_min) | (freq[i]>freq_max)) {
    # Set the fft_coefficients to zero, also in the mirrored part
    fft_eeg1[i] <- 0
    fft_eeg1[n-i] <- 0
    }
  }

# Check your result by plotting the fourier coefficients
#plot(fft_eeg1)

```

`@solution`
```{r}
# eeg1, n = 2^20, fft_eeg1 and freq still available
# Set your minimum frequency and your maximum frequency
freq_min <- 8
freq_max <- 13

# Set all fourier coefficients 0, which not between freq_min and freq_max
for (i in 1:(n/2)) {
  if ((freq[i]<freq_min) | (freq[i]>freq_max)) {
    # Set the fft_coefficients to zero, also in the mirrored part
    fft_eeg1[i] <- 0
    fft_eeg1[n-i] <- 0
    }
  }
  
plot(x=freq,y=abs(fft_eeg1[1:2^19]))
plot(x=abs(fft_eeg1))

```

`@sct`
```{r}
ex() %>% check_error()
ex() %>% check_object("eeg1") %>% check_equal()
#ex() %>% check_function("readRDS") %>% check_result() %>% check_equal()
ex() %>% check_object("n") %>% check_equal()
#ex() %>% check_object("fft_eeg1") %>% check_equal()
#ex() %>% check_function("fft") %>% check_result() %>% check_equal()
ex() %>% check_object("freq") %>% check_equal()
ex() %>% check_object("freq_min") %>% check_equal()
ex() %>% check_object("freq_max")  %>% check_equal()
# check for loop
ex() %>% check_for() %>% {
  check_cond(.) %>% {
    check_code(., "in")
    check_code(., "1")
    check_code(., "(n/2)")
  }
  check_body(.) %>%check_if_else(1) %>%  {
  check_cond(.) %>% {
    check_code(., "freq_min")
    check_code(., "|")
    check_code(., "freq_max")
  	}
  check_if(.) %>% check_code(.,"    fft_eeg1[i] <- 0") %>% check_equal() #{
    #check_code(.) %>% check_equal()
    #check_code(., c("i", "n-i"))
    #check_code(.,"fft_eeg1[i] <- 0") #%>% check_arg("x") %>% check_equal()
    #check_object(.,"fft_eeg1<-0") %>% check_equal()
    #check_code(.,"fft_eeg1[i]")
    #check_code(.,"fft_eeg1[n-i]")
    #}  
  }
}
#ex() %>% check_function("plot") %>% check_result() %>% check_equal()
success_msg("Nice! As you can see in the plot, you have only left frequencies between 8 and 13 Hz.\n In the next task we will look at the effect of this")
```
