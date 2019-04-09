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

In the last example we could actually see anything happens. Thats why we investigate in the next step real data. To load data we use here [scan()](https://www.rdocumentation.org/packages/base/versions/3.5.3/topics/scan).

`@instructions`
Let's start with loading your data. 
1. Load the data from ```path_data``` to ```data``` by using ```scan()```. The first argument is the path. 
2. The data has a sampling rate of 4 Hz. Create a time series of the time and store it to ```time```. Now plot the data to see how the data looks like. If you like, you can set the x-label and y-label with 
3. The plot from task 2 isn't quit well. Let's plot again, but only the second houre of data and with a **line plot**, by adding ```"l"``` to the arguments of ```plot()```

`@hint`
- Do you remeber the ```seq(start,end,step)``` command to create the time series of the time?!

- You can plot by ```plot(x=time-values,y-data-values)```.

`@pre_exercise_code`
```{r}
path_data = "https://assets.datacamp.com/production/repositories/3401/datasets/d191ac1f6ae2fda3392c4d41b892ba8bd2822bf3/atmung.dat"
```

`@sample_code`
```{r}
# The path to "atmung.dat" is stored in path_data
# Load the data
data <- scan(path_data)

# Create time
time <- seq(0,375,0.25)

# Plot the data
plot(x=time,y=data)

# define start and end
start <- 240
end <- 480

# Plot only the second houre as line-plot
plot(x=time[start:end],y=data[start:end],"l")
```

`@solution`
```{r}
# Load the data
data <- scan(path_data)

# Create time
time <- seq(0,375,0.25)

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
ex() %>% check_function("plot") %>% check_result() %>% check_equal()
ex() %>% check_function("plot") %>% check_result() %>% check_equal()
success_msg("Well done!")
```

---

## Frequency spectrum of our respiration data

```yaml
type: NormalExercise
key: 4617f36b93
xp: 100
```

In the last example, we 

`@instructions`


`@hint`


`@pre_exercise_code`
```{r}



```

`@sample_code`
```{r}
# Plot the data
start <- 240
end <- 480
# Plot only the second houre as line-plot
plot(x=time[start:end],y=data[start:end],"l")
```

`@solution`
```{r}

```

`@sct`
```{r}

```
