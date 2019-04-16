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

In this task we will focus on one EEG channel and how to extract EEG bands!
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
plot(time,eeg1)
```

`@sct`
```{r}

```

---

## Frequency spectrum of EEG data

```yaml
type: NormalExercise
key: 3bfc4232e8
xp: 100
```



`@instructions`


`@hint`


`@pre_exercise_code`
```{r}
download.file(url = "https://assets.datacamp.com/production/repositories/3401/datasets/636762184295f3f3370287b8a7a20cbc48aa5ae6/eeg_c4m1.rds", destfile = "eeg1.rds")

```

`@sample_code`
```{r}

```

`@solution`
```{r}

```

`@sct`
```{r}

```

---

## Bandpass filter

```yaml
type: NormalExercise
key: a2a114075b
xp: 100
```



`@instructions`


`@hint`


`@pre_exercise_code`
```{r}
download.file(url = "https://assets.datacamp.com/production/repositories/3401/datasets/636762184295f3f3370287b8a7a20cbc48aa5ae6/eeg_c4m1.rds", destfile = "eeg1.rds")

```

`@sample_code`
```{r}

```

`@solution`
```{r}

```

`@sct`
```{r}

```
