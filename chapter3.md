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

[Electroencephalography](https://en.wikipedia.org/wiki/Electroencephalography) is an electro-physiological monitoring method to record electrical activity of the brain. The human body transfers information by neurons. This cells emitting signals via changes of electrochemical potentials. A single neuronal activity usually cannot be detected in-vivo. But hundreds or thousands of neurons are involved in neurological processes in the brain, so that we can measure their synchronous activity as electrical potentials on the scalp.

The signals are measured in micro volts (µV) and need some pre-processing to obtain information. At the end of this part we can compare activities in $\alpha$ , $\beta$  and $\delta$ bands. 

$\alpha$-waves (8-13 Hz):
- relaxed and awake, but closed eyes

$\beta$-waves (13-30 Hz):
- during mental or physical activity or psychological burden

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

The EEG data has a sampling rate of **1024 Hz** and is recorded for **17 minutes and 4 seconds**. The values are in µV.
Be aware that the calculations in this part can take some time.

`@instructions`
1. Load the compressed EEG file ```eeg.rds``` with ```readRDS(file)``` to ```eeg```
2. Create a time series in minutes for the horizontal axis of your EEG data and store it in ```time```. 
2. Plot the EEG data, y-axis in µV and x-axis in minutes

`@hint`
- To create a time series use ```seq(start,end,step)```. start = 0. end = (length(data)-1)/(step)
- Step size in minutes = 1/(sampling rate * 60)

`@pre_exercise_code`
```{r}
download.file(url = "https://assets.datacamp.com/production/repositories/3401/datasets/636762184295f3f3370287b8a7a20cbc48aa5ae6/eeg_c4m1.rds", destfile = "eeg.rds")

```

`@sample_code`
```{r}
# Load compressed EEG data "eeg.rds"
eeg <- 

# Create time 
time <- 

# Plot data 

```

`@solution`
```{r}
# Load compressed EEG data "eeg.rds"
eeg <- readRDS("eeg.rds")

# Create time 
time <- seq(0,(length(eeg)-1)/(1024*60),1/(1024*60))

# Plot data 
plot(time,eeg)
```

`@sct`
```{r}
ex() %>% check_error()
ex() %>% check_object("eeg") %>% check_equal()
ex() %>% check_function("readRDS") %>% check_result() %>% check_equal()
ex() %>% check_object("time") %>% check_equal()
ex() %>% check_function("seq") %>% check_result() #%>% check_equal()
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

Now we will calculate the FFT of your EEG data and plot the frequency spectrum.

`@instructions`
Now we need the FFT and therefore the max power of 2 in the length of the signal.
1. Check out the highest power of 2 in the signal length and store it to ```n```. (You can use the R console as a calculator)
2. Calculate the FFT and store it to ```fft_eeg```. Use only a data length of power of 2.
3. Determine the frequencies (```freq```) that corresponded to the Fourier coefficients.

`@hint`
- Check the previous exercises if you need help!
- Be careful, you need to use brackets in calculations of the index like data[1:(2^18+1)]!

`@pre_exercise_code`
```{r}
download.file(url = "https://assets.datacamp.com/production/repositories/3401/datasets/636762184295f3f3370287b8a7a20cbc48aa5ae6/eeg_c4m1.rds", destfile = "eeg1.rds")
# Load compressed EEG data "eeg1.rds"
eeg <- readRDS("eeg1.rds")
```

`@sample_code`
```{r}
# Your EEG data is still stored in eeg1
# max power of 2 in the length of the signal
n <- 

# Calculate the FFT
fft_eeg <- 

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
fft_eeg <- fft(eeg)

# Calculate the frequencies
freq <- seq(0,512,512/(2^19))

# Plot the frequency spectrum
plot(freq,abs(fft_eeg[1:(2^19+1)]))
```

`@sct`
```{r}
ex() %>% check_error()
ex() %>% check_object("n") %>% check_equal()
ex() %>% check_object("fft_eeg") %>% check_equal()
ex() %>% check_function("fft") %>% check_result() %>% check_equal()
ex() %>% check_object("freq") %>% check_equal()
ex() %>% check_function("seq") %>% check_result() %>% check_equal()
ex() %>% check_function("plot") %>% check_result() %>% check_equal()
success_msg("You created successfull a frequency spectrum!")
```

---

## Bandpass filter Part I - frequency domain

```yaml
type: NormalExercise
key: a2a114075b
xp: 100
```

Now we come to the highlight of this session, the bandpass filter or how to select specific frequencies of a signal and suppress other components.

Lets start with the $\alpha$-band of an EEG. You remember it contains the frequency range from 8 to 13 Hz. The basic principle is now to use our frequency domain, and set all Fourier coefficients to 0, if frequencies are out of the range 8 to 13 Hz. But do not forget the mirror part -- is must be treated accordingly. 

The EEG data has a sampling rate of **1024 Hz** and is recorded for **17 minutes and 4 seconds**. The values are in µV.
Be aware that the calculations in this part can take some time.

`@instructions`
1. Define your minimum frequency and your maximum frequency of the $\alpha$-band.
2. Iterate over all frequencies ```freq``` and set the Fourier coefficients to 0, if they are not in the $\alpha$ band. Example ```for (i in 1:10) {i}``` will iterate i from 1 to 10. 

Do not forget the mirror part! Remember the first Fourier coefficient (index 1) and the (n/2+1)$^{th}$ Fourier coefficient (index n/2+1) are not mirrored and should always become 0. Otherwise the 2$^{nd}$ coefficient is mirrored to the n$^{th}$ coefficient, the 3$^{rd}$ is mirrored to the (n-1)$^{th}$ coefficient and so on, till the (n/2)$^{th}$ is mirrored to the (n/2+2)$^{th}$ coefficient.

`@hint`
- ```|``` and ```&``` are the logical OR and AND of R 
- As the ```fft_eeg``` is symmetric from the second to the last index, you have to set ```fft_eeg[i]``` and ```fft_eeg[n-i+2]``` to 0 for i = 2 to n/2+1.

`@pre_exercise_code`
```{r}
download.file(url = "https://assets.datacamp.com/production/repositories/3401/datasets/636762184295f3f3370287b8a7a20cbc48aa5ae6/eeg_c4m1.rds", destfile = "eeg.rds")
# Load compressed EEG data "eeg.rds"
eeg <- readRDS("eeg.rds")
# max power of 2 in the length of the signal
n <- 2^20
# Calculate the FFT
fft_eeg <- fft(eeg)
# Calculate the frequencies
freq <- seq(0,512,by = 512/(2^19))
```

`@sample_code`
```{r}
# eeg1, n = 2^20, fft_eeg1 and freq still available
# Set your minimum frequency and your maximum frequency (replace ___) 
freq_min <- ___
freq_max <- ___

# Set all Fourier coefficients 0, which not between freq_min and freq_max 
# The mirrored Fourier coefficients (replace ___) 
for (i in ___:___ {
  if ((freq[___] < ___) | (freq[___] > ___)) {
    # Set the fft_coefficients to zero, also in the mirrored part
    fft_eeg[___] <- 0
    fft_eeg[___] <- 0
    }
  }
fft_eeg[___] <- 0
fft_eeg[___] <- 0
     
# Check your result by plotting the fourier coefficients again (frequency spectrum)


```

`@solution`
```{r}
# eeg1, n = 2^20, fft_eeg1 and freq still available
# Set your minimum frequency and your maximum frequency (replace ___) 
freq_min <- 8
freq_max <- 13

# Set all Fourier coefficients 0, which not between freq_min and freq_max 
# The mirrored Fourier coefficients (replace ___) 
for (i in 2:(n/2+1)) {
  if ((freq[i] < freq_min) | (freq[i] > freq_max)) {
    # Set the fft_coefficients to zero, also in the mirrored part
    fft_eeg[i] <- 0
    fft_eeg[n-i+2] <- 0
    }
  }
fft_eeg[1] <- 0
fft_eeg[n/2+1] <- 0

# Check your result by plotting the fourier coefficients again (frequency spectrum)
plot(x=freq,y=abs(fft_eeg[1:(2^19+1)]))

```

`@sct`
```{r}
ex() %>% check_error()
ex() %>% check_object("eeg") %>% check_equal()
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
    check_code(., "2")
    check_code(., "(n/2)")
  }
  check_body(.) %>%check_if_else(1) %>%  {
  check_cond(.) %>% {
    check_code(., "freq_min")
    check_code(., "|")
    check_code(., "freq_max")
  	}
  check_if(.) %>%{
    ex() %>% check_code("fft_eeg[i]",fixed=TRUE) 
    ex() %>% check_code(c("fft_eeg[n-i+2]","fft_eeg[2+n-i]","fft_eeg[n+2-i]"),fixed=TRUE) 
  	}    
  }
}

ex() %>% check_object("fft_eeg") %>% check_equal()
ex() %>% check_function("plot") %>% check_result() %>% check_equal()
success_msg("Nice! As you can see in the plot, you have only left frequencies between 8 and 13 Hz.\n In the next task we will look at the effect of this")
```

---

## Bandpass filter Part II - Inverse FFT

```yaml
type: NormalExercise
key: d2eee606de
xp: 100
```

As we saw in the result of the last task, we eliminated the frequencies except for the range **8 to 13 Hz**. With the **inverse FFT (iFFT)** we can now reverse our bandpass filtered FFT and receive a signal with "only" frequencies between 8 to 13 Hz, also called the $\alpha$-band.  

The bandpass filtered Fourier coefficients are still available under ```fft_eeg1``` and the time series in minutes is still stored in ```time```. Let's plot the bandpass filtered signal!

`@instructions`
1. Use the inverse Fourier transform to obtain your bandpass filtered signal and save it to ```eeg1_alpha```. For iFFT you use ```fft()``` again, but with the argument ```inverse=TRUE``` (the default for this parameter is ```FALSE```). For the R implementation of FFT, we need to normalize the result by dividing by the length of data (see documentation of [fft](https://www.rdocumentation.org/packages/stats/versions/3.5.3/topics/fft). Finally we need only the real part ```Re()``` of the result. 
2. Plot the bandpass filtered signal.

`@hint`


`@pre_exercise_code`
```{r}
download.file(url = "https://assets.datacamp.com/production/repositories/3401/datasets/636762184295f3f3370287b8a7a20cbc48aa5ae6/eeg_c4m1.rds", destfile = "eeg1.rds")
# Load compressed EEG data "eeg1.rds"
eeg <- readRDS("eeg1.rds")
# max power of 2 in the length of the signal
n <- 2^20
# Calculate the FFT
fft_eeg <- fft(eeg)
# Calculate the frequencies
freq <- seq(0,512,by = 512/(2^19))
# Set all fourier coefficients 0, which not between freq_min and freq_max
freq_min <- 8
freq_max <- 13

# Set all Fourier coefficients 0, which not between freq_min and freq_max 
# The mirrored Fourier coefficients (replace ___) 
for (i in 2:(n/2)) {
  if ((freq[i] < freq_min) | (freq[i] > freq_max)) {
    # Set the fft_coefficients to zero, also in the mirrored part
    fft_eeg[i] <- 0
    fft_eeg[n-i+2] <- 0
    }
  }
fft_eeg[1] <- 0
fft_eeg[n/2+1] <- 0

# Create time 
time <- seq(0,(length(eeg)-1)/(1024*60),1/(1024*60))
```

`@sample_code`
```{r}
# Use fft(__, inverse=TRUE), length() and Re() as descriped
eeg_alpha <- 

# Plot bandpass filtered signal

```

`@solution`
```{r}
# Use fft(__, inverse=TRUE), length() and Re() as descriped
eeg_alpha <- Re(fft(fft_eeg,inverse=TRUE)/length(fft_eeg))

# Plot bandpass filtered signal
plot(x=time, y=eeg_alpha)
```

`@sct`
```{r}
ex() %>% check_error()
ex() %>% check_function("fft") %>% check_result() %>% check_equal()
ex() %>% check_function("length") %>% check_result() %>% check_equal()
ex() %>% check_function("Re") %>% check_result() %>% check_equal()
ex() %>% check_object("eeg_alpha") %>% check_equal()
ex() %>% check_function("plot") %>% check_result() %>% check_equal()
success_msg("You got your first bandpass filtered signal - go on!")
```

---

## EEG bandpass filter function

```yaml
type: NormalExercise
key: 5e928b2a5f
xp: 100
```

As we know how to apply a bandpass filter in R, we can now write a function, which allows us to use the filter for several EEG-bands!

To define a function in R you use the scheme:

> function_name ```<- function```(argument1, argument2, ...) {

>	#body of thefunction

>   ```return``` (values_you_want_to_return # without return, by default the last line is returned)

>}

`@instructions`
1. Define your function ```bandpass_filter```. Use the arguments ```signal```, ```freq_min```, ```freq_max```, and ```samp_rate```
2. Calculate the length of signal and store it to ```n``` (already implemented).
2. Calculate the FFT of ```signal``` in the function body (already implemented).
3. We used the filter from the previous exercises; you don't have to change anything!
4. Use fft(..., inverse=TRUE), len_eeg and Re() to get the bandpass filtered signal as in the previous task!
5. Return the ```bandpass_signal```

`@hint`


`@pre_exercise_code`
```{r}

```

`@sample_code`
```{r}
# Define your bandpass_filter function:
___ <- function(___,___,___) {
	# Calculate the length of signal (next suitable power of 2)
	n <- 2^(int(log(length(signal)-1)/log(2)+1))
  
    # Calculate frequency vector
    freq = seq(0,samp_rate/2,samp_rate/n)
  
  	# Calculate the FFT of signal
	fft_eeg <- fft(signal)
	
	# Here we have the filter (do not change!)
    for (i in 2:(n/2)) {
      if ((freq[i] < freq_min) | (freq[i] > freq_max)) {
        fft_eeg1[i] <- 0
        fft_eeg1[n-i+2] <- 0
        }
      }
    fft_eeg1[1] <- 0
    fft_eeg1[n/2+1] <- 0
	
	# Use fft(__, inverse=TRUE), n and Re() to get the bandpass filtered signal
	bandpass_signal <- Re(fft(___,inverse=TRUE)/___)

    # Use return to return bandpass_signal
	return(___)
 	}
```

`@solution`
```{r}
# Define your bandpass_filter function:
bandpass_filter <- function(signal,freq_min,freq_max,samp_rate) {
	# Calculate the length of signal (next suitable power of 2)
	n <- 2^(int(log(length(signal)-1)/log(2)+1))
  
    # Calculate frequency vector
    freq = seq(0,samp_rate/2,samp_rate/n)
  
  	# Calculate the FFT of signal
	fft_eeg <- fft(signal)
	
	# Here we have the filter (do not change!)
    for (i in 2:(n/2)) {
      if ((freq[i] < freq_min) | (freq[i] > freq_max)) {
        fft_eeg1[i] <- 0
        fft_eeg1[n-i+2] <- 0
        }
      }
    fft_eeg1[1] <- 0
    fft_eeg1[n/2+1] <- 0
	
	# Use fft(__, inverse=TRUE), n and Re() to get the bandpass filtered signal
	bandpass_signal <- Re(fft(fft_eeg,inverse=TRUE)/n)

    # Use return to return bandpass_signal
	return(bandpass_signal)
 	}
```

`@sct`
```{r}
ex() %>% check_fun_def('bandpass_filter') %>%{
    check_arguments(.)
    check_body(.) %>% {
       check_function(.,'fft', index = 1) %>% check_arg('z') %>% check_equal(eval = FALSE)
       check_function(., 'Re') %>% {
         check_function(., 'fft', index = 1) %>% check_arg(., 'z','inverse') %>% check_equal(eval = FALSE)
         }
       check_code(.,'Re(fft(fft_eeg,inverse=TRUE)/n)',fixed = TRUE)
       check_code(.,'return(bandpass_signal)',fixed = TRUE)
     
    }
  }
  
success_msg("Great, now we have a filter function!")

```

---

## EEG waves

```yaml
type: NormalExercise
key: 010ac11bc0
xp: 100
```

We are coming to our final task where we merge all together what we have learned! To see what happens in different sleep stages in the different EEG-bands, we will plot the α-, β-, δ-band of one EEG-channel and compare them with the corresponding sleep stages, measured in 30 second intervals. 

Short reminder of the EEG-bands:

$\alpha$-waves (8-13 Hz)

$\beta$-waves (13-30 Hz)

$\delta$-waves (0,5-3 Hz)

`@instructions`
The signal ```eeg```, ```freq```, ```time``` (in minutes) and the function ```bandpass_filter(signal, freq_min,freq_max,len_eeg)``` are still available. 

1. Use your function ```bandpass_filter()``` to calculate the α-, β-, δ-band of the ```eeg``` signal. Store the result in ```eeg_alpha```, ```eeg_beta``` and ```eeg_delta```.
2. Plot all 3 EEG-bands in 3 rows. Therefore, we already prepared some stuff. [par(mfrow=c(4,1))](https://www.rdocumentation.org/packages/graphics/versions/3.5.3/topics/par) will set 4 rows of plots. ```xlimit``` and ```ylimit```defines the range of the plot, here the first 4 minutes from -150 to 150 mV. Finally you just have fill the x-values for the plots (see the gaps!)

`@hint`


`@pre_exercise_code`
```{r}
# Define your bandpass_filter function:
bandpass_filter <- function(signal,freq,freq_min,freq_max) {
	# Calculate the length of signal
	n <- length(signal)
  
  	# Calculate the FFT of signal
	fft_eeg <- fft(signal)
	
	# Here we have the filter (do not change!)
  	# Mirrored part
    for (i in 2:(n/2)) {
      if ((freq[i] < freq_min) | (freq[i] > freq_max)) {
        fft_eeg[i] <- 0
        fft_eeg[n-i+2] <- 0
        }
#      else{
#        fft_eeg[i] <- (1-i)*fft_eeg[i]
#        fft_eeg[n-i+2] <- (1+i)*fft_eeg[n-i+2]        
#      }
      }
    # Check the first Fourier coefficient/frequency
    if ((freq[1] < freq_min) | (freq[1] > freq_max)) {
        fft_eeg[1] <- 0
        }
    # Check the hightest frequency
    if ((freq[n/2+1] < freq_min) | (freq[n/2+1] > freq_max)) {
        fft_eeg[n/2+1] <- 0
        }
  
	# Use fft(__, inverse=TRUE), n and Re() to get the bandpass filtered signal
	bandpass_signal <- Re(fft(fft_eeg,inverse=TRUE)/n)
  
    # Use return to return bandpass_signal
	return(bandpass_signal)
 	}

download.file(url = "https://assets.datacamp.com/production/repositories/3401/datasets/636762184295f3f3370287b8a7a20cbc48aa5ae6/eeg_c4m1.rds", destfile = "eeg1.rds")
# Load compressed EEG data "eeg1.rds"
eeg <- readRDS("eeg1.rds")[1:2^19]
freq <- seq(0,512,by = 512/(2^18))
# Create time 
time <- seq(0,length(eeg)/(1024*60)-1/(1024*60),by=1/(1024*60))

# sleep stages
sleep_stage <- c(2,2,2,2,2,5,1,5,5,5,5,5,5,5,1,2,2,2,2,2,2,2,2,2,2,2,2,1,4,4,4,4,4,4,4,4,4,4,4)
sleep_time <- seq(0,19,0.5)
```

`@sample_code`
```{r}
# Calculate the α-, β- and δ-band of the eeg signal (replace ___)
eeg_alpha <- ___ 
eeg_beta  <- ___ 
eeg_delta <- ___ 

# Plot preparation (Don't change!)
par(mfrow=c(4,1),mar=c(1,5,0,0),oma=c(2,2,0,0))
xlimit<-c(0,4)
ylimit<-c(-100,100)

# Plot α-, β- and δ-band in this order (replace ___)
plot(x=time,y=___,"l",xaxt='n',ylab="alpha",xlim=xlimit,ylim=ylimit)
plot(x=time,y=___,"l",xaxt='n',ylab="beta",xlim=xlimit,ylim=ylimit)
plot(x=time,y=___,"l",xaxt='n',ylab="delta",xlim=xlimit,ylim=ylimit)

# Plot sleep stages (Don't change!)
plot(sleep_time,sleep_stage,yaxt='n',xlab="time in minutes",ylab="sleep stage",xlim=xlimit)
axis(2, at=1:5, labels=c('N1','N2','N3','REM','W'))
```

`@solution`
```{r}
# Calculate the α-, β-, δ-band of the eeg signal
eeg_alpha <- bandpass_filter(eeg,freq,8,13) 
eeg_beta  <- bandpass_filter(eeg,freq,13,30) 
eeg_delta <- bandpass_filter(eeg,freq,0.5,3) 

# Plot preparation (Don't change!)
par(mfrow=c(4,1),mar=c(1,5,0,0),oma=c(2,2,0,0))
xlimit<-c(0,4)
ylimit<-c(-100,100)

# Plot α-, β- and δ-band in this order (replace ___)
plot(x=time,y=eeg_alpha,"l",xaxt='n',ylab="alpha",xlim=xlimit,ylim=ylimit)
plot(x=time,y=eeg_beta,"l",xaxt='n',ylab="beta",xlim=xlimit,ylim=ylimit)
plot(x=time,y=eeg_delta,"l",xaxt='n',ylab="delta",xlim=xlimit,ylim=ylimit)
plot(x=sleep_time,y=sleep_stage,yaxt='n',xlab="time in minutes",ylab="sleep stage",xlim=xlimit)

# Plot sleep stages (Don't change!)
axis(2, at=1:5, labels=c('N1','N2','N3','REM','W'))
```

`@sct`
```{r}
ex() %>% check_error()
#alpha
ex() %>% check_function("bandpass_filter") %>% {
  check_arg(., "signal") %>% check_equal()
  check_arg(., "freq") %>% check_equal()
  check_arg(., "freq_min") %>% check_equal(incorrect_msg="Check ```eeg_alpha```")
  check_arg(., "freq_max") %>% check_equal(incorrect_msg="Check ```eeg_alpha```")  
} 
ex() %>% check_function("bandpass_filter") %>% check_result() %>% check_equal()

#beta
ex() %>% check_function("bandpass_filter",index=2) %>% {
  check_arg(., "signal") %>% check_equal()
  check_arg(., "freq") %>% check_equal()
  check_arg(., "freq_min") %>% check_equal(incorrect_msg="Check ```eeg_beta```")
  check_arg(., "freq_max") %>% check_equal(incorrect_msg="Check ```eeg_beta```")  
} 
ex() %>% check_function("bandpass_filter",index=2) %>% check_result() %>% check_equal()

#delta
ex() %>% check_function("bandpass_filter",index=3) %>% {
  check_arg(., "signal") %>% check_equal()
  check_arg(., "freq") %>% check_equal()
  check_arg(., "freq_min") %>% check_equal(incorrect_msg="Check ```eeg_delta```")
  check_arg(., "freq_max") %>% check_equal(incorrect_msg="Check ```eeg_delta```")  
} 
ex() %>% check_function("bandpass_filter",index=3) %>% check_result() %>% check_equal()

ex() %>% check_object("eeg_alpha") %>% check_equal()
ex() %>% check_object("eeg_beta") %>% check_equal()
ex() %>% check_object("eeg_delta") %>% check_equal()

#plot
ex() %>% check_function("plot",index=1) %>% {
  check_arg(., "x") %>% check_equal()
  check_arg(., "y") %>% check_equal()
}
ex() %>% check_function("plot",index=2) %>% {
  check_arg(., "x") %>% check_equal()
  check_arg(., "y") %>% check_equal()
}
ex() %>% check_function("plot",index=3) %>% {
  check_arg(., "x") %>% check_equal()
  check_arg(., "y") %>% check_equal()
}

success_msg("Now you got a glue what we can do with time series analysis!")
```

---

## Let's have a last look of your day's work!

```yaml
type: PureMultipleChoiceExercise
key: 02a74d2a10
xp: 50
```

![](https://assets.datacamp.com/production/repositories/3401/datasets/5fe8bab9a6b6c49b7ceeea084bbfa7dc6c61fac1/eeg_band.png)
You can see the influence of the sleep stage to the EEG-bands. During awake (W) episodes the $\alpha$- and $\beta$-band show increasing amplitudes.
When does $\beta$-waves occur?

`@hint`


`@possible_answers`
1. Sleep
2. Eye movement
3. [Mental or physical activity or psychological burden]
4. Relaxed sitting

`@feedback`
1. Check ```Introduction to EEG``` again
2. Check ```Introduction to EEG``` again
3. Wonderful! You finished the todays curse.
4. Check ```Introduction to EEG``` again
