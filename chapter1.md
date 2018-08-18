---
  title: "Task 1"
  description: "First Task"
---

## Random Numbers

```yaml
type: NormalExercise 
lang: r
xp: 100 
skills: 1
key: 847996f3f9   
```


This is a first task to get familar with R and time series analysis.
We wanna start with some easy tasks. For time series analysis we need some data. So let's create some random numbers. Therefor we can use the command [runif()](https://www.rdocumentation.org/packages/compositions/versions/1.40-2/topics/runif). First argument should be the numbers of numbers you would like to generate. By default you will get shuffeld numbers between 0 and 1, but you can add as arguments also an intervall. `runif(10,1,10)` will give you 10 random numbers out of 1 to 10.


`@instructions`
(i) Generate 2^15 random numbers from 0 to 1 by using `runif()` and save it to `numbers`
(ii) Plot a histogram of your random numbers using `plot(hist(#))`

`@hint`
To raise the power in R use `n^p`
To give a value to a variable use `x<-10`

`@sample_code`

```{r}
# Have fun
```

`@solution`

```{r}
x <- runif(2^15)
plot(hist(x))
```

`@sct`

```{r}
# Update this to something more informative.
success_msg("Some praise! Then reinforce a learning objective from the exercise.")
```

---

## FFT spectrum

```yaml
type: NormalExercise 
xp: 100 
key: d9ac7746ab   
```


We asume we have a signal with 2^15 datapoints and a sampling rate of 64 Hz. So the FFT of our signal will return 2^15 complex numbers.


`@instructions`
Use your random numbers from the exercise bevore and calculate the FFT-spectrum of your "randome-time-series". fs=64Hz

`@hint`
no hint

`@pre_exercise_code`

```{r}

```


`@sample_code`

```{r}
# no test
```

`@solution`

```{r}
x<-runif(2^12)
y<-Re(fft(x))[2^11:2^12]
fs<-64 #Hz
plot(y,ylim=c(-50,50))
```

`@sct`

```{r}

```

