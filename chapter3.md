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
The EEG data has a sampling rate of 1024 Hz.

`@pre_exercise_code`
```{r}
download.file(url = "https://assets.datacamp.com/production/repositories/3401/datasets/95bca38defab344cd4660eae1a472d70224cfb89/eeg_c4m1.rds", destfile = "eeg1.rds")

# Load compressed file "eeg1.rds" with readRDS()
eeg1 <- 
```

***

```yaml
type: NormalExercise
key: 8a6c00c5b8
xp: 50
```

`@instructions`
Load & Plot
1. Load the compressed EEG file ```eeg1.rds``` with ```loadRDS(file)``` to ```eeg1```
2. Plot the EEG data.

`@hint`


`@sample_code`
```{r}
# Load EEG data from path_eeg1 and path_eeg2
eeg1 <-


# check length of data and set n data 
```

`@solution`
```{r}
# Load compressed EEG data "eeg1.rds"
#eeg1 <- readRDS("eeg1.rds")


# check length of data and set n data 
```

`@sct`
```{r}

```

***

```yaml
type: NormalExercise
key: c7e1e99971
xp: 50
```

`@instructions`
Now we will print the signals
3. 2. Check length of data and set ```n``` to the highest power of 2 in the length of data.
4. plot

`@hint`


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

## test

```yaml
type: NormalExercise
key: 81e2834c82
xp: 100
```



`@instructions`


`@hint`


`@pre_exercise_code`
```{r}
download.file(url = "https://assets.datacamp.com/production/repositories/3401/datasets/95bca38defab344cd4660eae1a472d70224cfb89/eeg_c4m1.rds", destfile = "eeg1.rds")
```

`@sample_code`
```{r}
# Load compressed file "eeg1.rds" with readRDS()
eeg1 <- 
```

`@solution`
```{r}

```

`@sct`
```{r}

```
