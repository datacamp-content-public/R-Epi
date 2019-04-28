---
title: 'More EEG Analysis'
description: ""
---

## Instantaneous amplitude by Hilbert transformation

```yaml
type: NormalExercise
key: 6743cda87e
xp: 100
```

Not ready jet.

`@instructions`


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

# define hilbert transformation from fft
hilbert <- function(signal){
  fft_signal <- fft(signal)
  n <- length(signal)
  
  # create h with zeros and length n
  h <- integer(n)
  # set first an n/2 value to 1
  h[1] <- 1
  h[(n/2+1)] <- 1
  h[2:(n/2)] <- 2
  
  # create hilbert trafo
  hilbert <- fft(fft_signal*h,inverse=TRUE)/n
  return(hilbert)
}

# LOAD DATA
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
# Calculate the α-, β-, δ-band of the eeg signal
eeg_alpha <- bandpass_filter(eeg,freq,8,13) 
eeg_beta  <- bandpass_filter(eeg,freq,13,30) 
eeg_delta <- bandpass_filter(eeg,freq,0.5,3) 

# Calculate hilberttrafo
eeg_alpha_hilbert <- abs(hilbert(eeg_alpha))
eeg_beta_hilbert <- abs(hilbert(eeg_beta))
eeg_delta_hilbert <- abs(hilbert(eeg_delta))

# Plot preparation (Don't change!)
par(mfrow=c(4,1),mar=c(1,5,0,0),oma=c(2,2,0,0))
xlimit<-c(0,4)
ylimit<-c(-100,100)

# Plot α-, β- and δ-band in this order (replace ___)
plot(x=time,y=eeg_alpha_hilbert,"l",xaxt='n',ylab="alpha",xlim=xlimit,ylim=ylimit)
plot(x=time,y=eeg_beta_hilbert,"l",xaxt='n',ylab="beta",xlim=xlimit,ylim=ylimit)
plot(x=time,y=eeg_delta_hilbert,"l",xaxt='n',ylab="delta",xlim=xlimit,ylim=ylimit)
plot(x=sleep_time,y=sleep_stage,yaxt='n',xlab="time in minutes",ylab="sleep stage",xlim=xlimit)

# Plot sleep stages (Don't change!)
axis(2, at=1:5, labels=c('N1','N2','N3','REM','W'))
```

`@solution`
```{r}

```

`@sct`
```{r}

```
