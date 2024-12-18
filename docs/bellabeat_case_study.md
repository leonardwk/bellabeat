Bellabeat Case Study
================

# Case Study 2: Bellabeat Data Analysis

## Introduction

Welcome to the second case study of the Google Data Analytics
Professional Certificate! In this scenario, you are a junior data
analyst working on the marketing analyst team at Bellabeat, a high-tech
manufacturer of health-focused products for women.

**In their journey to grow in the global smart device market, Bellabeat
has created several products:**

- **Bellabeat app**: The Bellabeat app provides users with health data
  related to their activity, sleep, stress, menstrual cycle, and
  mindfulness habits. This data can help users better understand their
  current habits and make healthy decisions. The Bellabeat app connects
  to their line of smart wellness products.
- **Leaf**: Bellabeat’s classic wellness tracker can be worn as a
  bracelet, necklace, or clip. The Leaf tracker connects to the
  Bellabeat app to track activity, sleep, and stress.
- **Time**: This wellness watch combines the timeless look of a classic
  timepiece with smart technology to track user activity, sleep, and
  stress. The Time watch connects to the Bellabeat app to provide you
  with insights into your daily wellness.
- **Spring**: This is a water bottle that tracks daily water intake
  using smart technology to ensure that you are appropriately hydrated
  throughout the day. The Spring bottle connects to the Bellabeat app to
  track your hydration levels.
- **Bellabeat membership**: Bellabeat also offers a subscription-based
  membership program for users. Membership gives users 24/7 access to
  fully personalized guidance on nutrition, activity, sleep, health and
  beauty, and mindfulness based on their lifestyle and goals.

Bellabeat is a successful small company, but they have the potential to
become a larger player in the global smart device market. **Urška
Sršen**, cofounder and Chief Creative Officer of Bellabeat, believes
that analyzing smart device fitness data could help unlock new growth
opportunities for the company. You have been asked to focus on one of
Bellabeat’s products and analyze smart device data to gain insight into
how consumers are using their smart devices. The insights you discover
will then help guide marketing strategy for the company.

In order to answer the key business questions, you will follow the six
steps of the data analysis process: **ask, prepare, process, analyze,
share, and act**.

## Ask

What are the goals of this case study?

Sršen asks you to analyze smart device usage data in order to gain
insight into how consumers use non-Bellabeat smart devices. She then
wants you to select one Bellabeat product to apply these insights to in
your presentation.

In order to address the business task, you must use the following three
questions to guide your analysis:

1.  What are some trends in smart device usage?
2.  How could these trends apply to Bellabeat customers?
3.  How could these trends help influence Bellabeat marketing strategy?

I can discover trends in smart device usage by looking into a [FitBit
Fitness Tracker Data
Set](https://www.kaggle.com/datasets/arashnic/fitbit) (CC0: Public
Domain, dataset made available through
[Mobius](https://www.kaggle.com/arashnic)): This Kaggle data set
contains personal fitness tracker from thirty fitbit users. Thirty
eligible Fitbit users consented to the submission of personal tracker
data, including minute-level output for physical activity, heart rate,
and sleep monitoring. It includes information about daily activity,
steps, and heart rate that can be used to explore users’ habits.

### Factors to Consider

Although this data set has a large list of variables that can be used
for analysis, there are some limitations, such as:

- There are only 30 participants in the study.
- The participate do not necessarily represent the larger FitBit
  userbase, as certain users may be less likely to take part in a survey
  due to lower activity. This could lead to selection bias.
- A lack of specificity when it comes to gender, which could be relevant
  as Bellabeat’s target audience are women.

## Prepare

In order to prepare the dataset for analysis, the following steps must
be taken:

1.  Download data and store it appropriately.
2.  Identify how it’s organized.
3.  Sort and filter the data.
4.  Determine the credibility of the data.

I will do so using the R, a powerful tool that can process datasets,
create clear visualizations, and present key points via R Markdown, a
notebook interface that can produce high quality, interactive documents
in a multitude of formats, such as this PDF file.

### Installing and Loading Packages

While R is incredibly useful, it requires certain packages to streamline
operations, which we can install using the `install.packages()`
function.

``` r
# Installing packages with install.packages()
if(!require(dplyr)) install.packages("dplyr")
```

    ## Loading required package: dplyr

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
if(!require(janitor)) install.packages("janitor")
```

    ## Loading required package: janitor

    ## 
    ## Attaching package: 'janitor'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     chisq.test, fisher.test

``` r
if(!require(tidyverse)) install.packages("tidyverse")
```

    ## Loading required package: tidyverse

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ forcats   1.0.0     ✔ readr     2.1.5
    ## ✔ ggplot2   3.5.1     ✔ stringr   1.5.1
    ## ✔ lubridate 1.9.3     ✔ tibble    3.2.1
    ## ✔ purrr     1.0.2     ✔ tidyr     1.3.1
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
if(!require(lubridate)) install.packages("lubridate")
if(!require(ggplot2)) install.packages("ggplot2")
if(!require(skimr)) install.packages("skimr")
```

    ## Loading required package: skimr

In addition, we must load these packages using the `library()` function.

``` r
# Loading packages with library()
library("dplyr")
library("janitor")
library("tidyverse")
library("lubridate")
library("ggplot2")
library("skimr")
```

We can now import and prepare the data for cleaning. The given data
files are separated into two one-month logs of data, split into eleven
files each. The file sizes vary widely, from 4 KB to over 87,000 KB. The
larger data sets are comprised of mostly minute-based data, with the
smallest being 3,200 KB. Although the level of specificity can be
useful, it will likely not have a significant effect on the results of
this case study. Therefore, it would be wise to avoid importing these
data files.

With the minute-based files out of the picture, we can assign each CSV
file to its own data frame as shown here:

``` r
# Assigning each CSV file to its own data frame
hourly_intensities <- read_csv("hourlyIntensities_merged.csv")
```

    ## Rows: 22099 Columns: 4
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): ActivityHour
    ## dbl (3): Id, TotalIntensity, AverageIntensity
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
hourly_calories <- read_csv("hourlyCalories_merged.csv")
```

    ## Rows: 22099 Columns: 3
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): ActivityHour
    ## dbl (2): Id, Calories
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
hourly_steps <- read_csv("hourlySteps_merged.csv")
```

    ## Rows: 22099 Columns: 3
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): ActivityHour
    ## dbl (2): Id, StepTotal
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
daily_activity <- read_csv("dailyActivity_merged.csv")
```

    ## Rows: 940 Columns: 15
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (1): ActivityDate
    ## dbl (14): Id, TotalSteps, TotalDistance, TrackerDistance, LoggedActivitiesDi...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
daily_intensities <- read_csv("dailyIntensities_merged.csv")
```

    ## Rows: 940 Columns: 10
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): ActivityDay
    ## dbl (9): Id, SedentaryMinutes, LightlyActiveMinutes, FairlyActiveMinutes, Ve...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
daily_steps <- read_csv("dailySteps_merged.csv")
```

    ## Rows: 940 Columns: 3
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): ActivityDay
    ## dbl (2): Id, StepTotal
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
daily_calories <- read_csv("dailyCalories_merged.csv")
```

    ## Rows: 940 Columns: 3
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): ActivityDay
    ## dbl (2): Id, Calories
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
daily_sleep <- read_csv("sleepDay_merged.csv")
```

    ## Rows: 413 Columns: 5
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): SleepDay
    ## dbl (4): Id, TotalSleepRecords, TotalMinutesAsleep, TotalTimeInBed
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
weight_log <- read_csv("weightLogInfo_merged.csv")
```

    ## Rows: 67 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): Date
    ## dbl (6): Id, WeightKg, WeightPounds, Fat, BMI, LogId
    ## lgl (1): IsManualReport
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

## Process

### Inspecting our Data

Now that we have our set of data frames, let’s create some tibbles and
inspect the first few rows using `head()`:

``` r
# Inspecting each data frame
head(hourly_intensities)
```

    ## # A tibble: 6 × 4
    ##           Id ActivityHour          TotalIntensity AverageIntensity
    ##        <dbl> <chr>                          <dbl>            <dbl>
    ## 1 1503960366 4/12/2016 12:00:00 AM             20            0.333
    ## 2 1503960366 4/12/2016 1:00:00 AM               8            0.133
    ## 3 1503960366 4/12/2016 2:00:00 AM               7            0.117
    ## 4 1503960366 4/12/2016 3:00:00 AM               0            0    
    ## 5 1503960366 4/12/2016 4:00:00 AM               0            0    
    ## 6 1503960366 4/12/2016 5:00:00 AM               0            0

``` r
head(hourly_calories)
```

    ## # A tibble: 6 × 3
    ##           Id ActivityHour          Calories
    ##        <dbl> <chr>                    <dbl>
    ## 1 1503960366 4/12/2016 12:00:00 AM       81
    ## 2 1503960366 4/12/2016 1:00:00 AM        61
    ## 3 1503960366 4/12/2016 2:00:00 AM        59
    ## 4 1503960366 4/12/2016 3:00:00 AM        47
    ## 5 1503960366 4/12/2016 4:00:00 AM        48
    ## 6 1503960366 4/12/2016 5:00:00 AM        48

``` r
head(hourly_steps)
```

    ## # A tibble: 6 × 3
    ##           Id ActivityHour          StepTotal
    ##        <dbl> <chr>                     <dbl>
    ## 1 1503960366 4/12/2016 12:00:00 AM       373
    ## 2 1503960366 4/12/2016 1:00:00 AM        160
    ## 3 1503960366 4/12/2016 2:00:00 AM        151
    ## 4 1503960366 4/12/2016 3:00:00 AM          0
    ## 5 1503960366 4/12/2016 4:00:00 AM          0
    ## 6 1503960366 4/12/2016 5:00:00 AM          0

``` r
head(daily_activity)
```

    ## # A tibble: 6 × 15
    ##           Id ActivityDate TotalSteps TotalDistance TrackerDistance
    ##        <dbl> <chr>             <dbl>         <dbl>           <dbl>
    ## 1 1503960366 4/12/2016         13162          8.5             8.5 
    ## 2 1503960366 4/13/2016         10735          6.97            6.97
    ## 3 1503960366 4/14/2016         10460          6.74            6.74
    ## 4 1503960366 4/15/2016          9762          6.28            6.28
    ## 5 1503960366 4/16/2016         12669          8.16            8.16
    ## 6 1503960366 4/17/2016          9705          6.48            6.48
    ## # ℹ 10 more variables: LoggedActivitiesDistance <dbl>,
    ## #   VeryActiveDistance <dbl>, ModeratelyActiveDistance <dbl>,
    ## #   LightActiveDistance <dbl>, SedentaryActiveDistance <dbl>,
    ## #   VeryActiveMinutes <dbl>, FairlyActiveMinutes <dbl>,
    ## #   LightlyActiveMinutes <dbl>, SedentaryMinutes <dbl>, Calories <dbl>

``` r
head(daily_intensities)
```

    ## # A tibble: 6 × 10
    ##         Id ActivityDay SedentaryMinutes LightlyActiveMinutes FairlyActiveMinutes
    ##      <dbl> <chr>                  <dbl>                <dbl>               <dbl>
    ## 1   1.50e9 4/12/2016                728                  328                  13
    ## 2   1.50e9 4/13/2016                776                  217                  19
    ## 3   1.50e9 4/14/2016               1218                  181                  11
    ## 4   1.50e9 4/15/2016                726                  209                  34
    ## 5   1.50e9 4/16/2016                773                  221                  10
    ## 6   1.50e9 4/17/2016                539                  164                  20
    ## # ℹ 5 more variables: VeryActiveMinutes <dbl>, SedentaryActiveDistance <dbl>,
    ## #   LightActiveDistance <dbl>, ModeratelyActiveDistance <dbl>,
    ## #   VeryActiveDistance <dbl>

``` r
head(daily_steps)
```

    ## # A tibble: 6 × 3
    ##           Id ActivityDay StepTotal
    ##        <dbl> <chr>           <dbl>
    ## 1 1503960366 4/12/2016       13162
    ## 2 1503960366 4/13/2016       10735
    ## 3 1503960366 4/14/2016       10460
    ## 4 1503960366 4/15/2016        9762
    ## 5 1503960366 4/16/2016       12669
    ## 6 1503960366 4/17/2016        9705

``` r
head(daily_calories)
```

    ## # A tibble: 6 × 3
    ##           Id ActivityDay Calories
    ##        <dbl> <chr>          <dbl>
    ## 1 1503960366 4/12/2016       1985
    ## 2 1503960366 4/13/2016       1797
    ## 3 1503960366 4/14/2016       1776
    ## 4 1503960366 4/15/2016       1745
    ## 5 1503960366 4/16/2016       1863
    ## 6 1503960366 4/17/2016       1728

``` r
head(daily_sleep)
```

    ## # A tibble: 6 × 5
    ##           Id SleepDay        TotalSleepRecords TotalMinutesAsleep TotalTimeInBed
    ##        <dbl> <chr>                       <dbl>              <dbl>          <dbl>
    ## 1 1503960366 4/12/2016 12:0…                 1                327            346
    ## 2 1503960366 4/13/2016 12:0…                 2                384            407
    ## 3 1503960366 4/15/2016 12:0…                 1                412            442
    ## 4 1503960366 4/16/2016 12:0…                 2                340            367
    ## 5 1503960366 4/17/2016 12:0…                 1                700            712
    ## 6 1503960366 4/19/2016 12:0…                 1                304            320

``` r
head(weight_log)
```

    ## # A tibble: 6 × 8
    ##           Id Date       WeightKg WeightPounds   Fat   BMI IsManualReport   LogId
    ##        <dbl> <chr>         <dbl>        <dbl> <dbl> <dbl> <lgl>            <dbl>
    ## 1 1503960366 5/2/2016 …     52.6         116.    22  22.6 TRUE           1.46e12
    ## 2 1503960366 5/3/2016 …     52.6         116.    NA  22.6 TRUE           1.46e12
    ## 3 1927972279 4/13/2016…    134.          294.    NA  47.5 FALSE          1.46e12
    ## 4 2873212765 4/21/2016…     56.7         125.    NA  21.5 TRUE           1.46e12
    ## 5 2873212765 5/12/2016…     57.3         126.    NA  21.7 TRUE           1.46e12
    ## 6 4319703577 4/17/2016…     72.4         160.    25  27.5 TRUE           1.46e12

### Column Names

Next, let’s see how the columns are organized using `col()`:

``` r
# Seeing how each data frame is organized
colnames(hourly_intensities)
```

    ## [1] "Id"               "ActivityHour"     "TotalIntensity"   "AverageIntensity"

``` r
colnames(hourly_calories)
```

    ## [1] "Id"           "ActivityHour" "Calories"

``` r
colnames(hourly_steps)
```

    ## [1] "Id"           "ActivityHour" "StepTotal"

``` r
colnames(daily_activity)
```

    ##  [1] "Id"                       "ActivityDate"            
    ##  [3] "TotalSteps"               "TotalDistance"           
    ##  [5] "TrackerDistance"          "LoggedActivitiesDistance"
    ##  [7] "VeryActiveDistance"       "ModeratelyActiveDistance"
    ##  [9] "LightActiveDistance"      "SedentaryActiveDistance" 
    ## [11] "VeryActiveMinutes"        "FairlyActiveMinutes"     
    ## [13] "LightlyActiveMinutes"     "SedentaryMinutes"        
    ## [15] "Calories"

``` r
colnames(daily_intensities)
```

    ##  [1] "Id"                       "ActivityDay"             
    ##  [3] "SedentaryMinutes"         "LightlyActiveMinutes"    
    ##  [5] "FairlyActiveMinutes"      "VeryActiveMinutes"       
    ##  [7] "SedentaryActiveDistance"  "LightActiveDistance"     
    ##  [9] "ModeratelyActiveDistance" "VeryActiveDistance"

``` r
colnames(daily_steps)
```

    ## [1] "Id"          "ActivityDay" "StepTotal"

``` r
colnames(daily_calories)
```

    ## [1] "Id"          "ActivityDay" "Calories"

``` r
colnames(daily_sleep)
```

    ## [1] "Id"                 "SleepDay"           "TotalSleepRecords" 
    ## [4] "TotalMinutesAsleep" "TotalTimeInBed"

``` r
colnames(weight_log)
```

    ## [1] "Id"             "Date"           "WeightKg"       "WeightPounds"  
    ## [5] "Fat"            "BMI"            "IsManualReport" "LogId"

### Observations

From the first few rows of each data set as well as the column names, we
can make a few observations:

1.  The variables are named using pascal case, which capitalizes the
    first letter of each word.
2.  The name of the date column in each file can vary, as most of the
    files refer to the date as `ActivityDay`, while others may use
    `SleepDay` or `Date`.
3.  The date format differs from file to file, likely due to the method
    of recording data, as some are based on the hour and others are
    dependent on the day.

### Cleaning the Data

Before analyzing the data, we must clean it and ensure standardization
between data sets by completing these steps:

1.  As previously mentioned, the column names utilize pascal case. We
    can alter the names to use snake case, as per [tidyverse’s style
    guide](https://style.tidyverse.org/syntax.html), which uses all
    lowercase characters and underscores to separate words.
2.  We can standardize the name of each date column to `date`.
3.  We can separate the day of the activity from the hour into two
    different columns if the data file records both.
4.  We can standardize each ‘date’ format to one format. Let’s do the
    “month-day-year” format

``` r
# Changing column names from pascal case to snake case and standardizing date name
hourly_intensities <- hourly_intensities %>% 
  rename(
    id = Id,
    date = ActivityHour,
    total_intensity = TotalIntensity,
    average_intensity = AverageIntensity
  )

hourly_calories <- hourly_calories %>% 
  rename(
    id = Id,
    date = ActivityHour,
    calories = Calories,
  )

hourly_steps <- hourly_steps %>% 
  rename(
    id = Id,
    date = ActivityHour,
    total_steps = StepTotal
  )

daily_activity <- daily_activity %>% 
  rename(
    id = Id,
    date = ActivityDate,
    total_steps = TotalSteps,
    total_distance = TotalDistance,
    tracked_distance = TrackerDistance,
    logged_distance = LoggedActivitiesDistance,
    very_active_distance = VeryActiveDistance,
    moderately_active_distance = ModeratelyActiveDistance,
    lightly_active_distance = LightActiveDistance,
    sedentary_active_distance = SedentaryActiveDistance,
    very_active_minutes = VeryActiveMinutes,
    fairly_active_minutes = FairlyActiveMinutes,
    lightly_active_minutes = LightlyActiveMinutes,
    sedentary_minutes = SedentaryMinutes,
    calories = Calories
  )

daily_intensities <- daily_intensities %>% 
  rename(
    id = Id,
    date = ActivityDay,
    sedentary_minutes = SedentaryMinutes,
    lightly_active_minutes = LightlyActiveMinutes,
    fairly_active_minutes = FairlyActiveMinutes,
    very_active_minutes = VeryActiveMinutes,
    sedentary_active_distance = SedentaryActiveDistance,
    lightly_active_distance = LightActiveDistance,
    moderately_active_distance = ModeratelyActiveDistance,
    very_active_distance = VeryActiveDistance
  )

daily_steps <- daily_steps %>% 
  rename(
    id = Id,
    date = ActivityDay,
    total_steps = StepTotal
  )

daily_calories <- daily_calories %>% 
  rename(
    id = Id,
    date = ActivityDay,
    calories = Calories
  )

daily_sleep <- daily_sleep %>% 
  rename(
    id = Id,
    date = SleepDay,
    total_sleep_records = TotalSleepRecords,
    total_minutes_asleep = TotalMinutesAsleep,
    total_time_in_bed = TotalTimeInBed
  )

weight_log <- weight_log %>% 
  rename(
    id = Id,
    date = Date,
    weight_kg = WeightKg,
    weight_lb = WeightPounds,
    fat = Fat,
    bmi = BMI,
    is_manual_report = IsManualReport,
    log_id = LogId
  )
```

Now that the column and date names have all been standardized, we need
to ensure consistency between all of the different date variables. As
previously mentioned, this requires separating the `date` of each row
from the `time` into two distinct columns:

``` r
# Separating date and time, then moving time to be adjacent to date in files.
hourly_intensities <- hourly_intensities %>% 
  mutate(
    time = format(as.POSIXct(mdy_hms(hourly_intensities$date)), format = "%H:%M:%S"),
    date = as.Date(hourly_intensities$date, "%m/%d/%Y")
  ) %>% 
  relocate(
    time, .after = date
  )
hourly_calories <- hourly_calories %>% 
  mutate(
    time = format(as.POSIXct(mdy_hms(hourly_calories$date)), format = "%H:%M:%S"),
    date = as.Date(hourly_calories$date, "%m/%d/%Y")
  ) %>% 
  relocate(
    time, .after = date
  )
hourly_steps <- hourly_steps %>% 
  mutate(
    time = format(as.POSIXct(mdy_hms(hourly_steps$date)), format = "%H:%M:%S"),
    date = as.Date(hourly_steps$date, "%m/%d/%Y")
  ) %>% 
  relocate(
    time, .after = date
  )
daily_sleep <- daily_sleep %>% 
  mutate(
    time = format(as.POSIXct(mdy_hms(daily_sleep$date)), format = "%H:%M:%S"),
    date = as.Date(daily_sleep$date, "%m/%d/%Y")
  ) %>% 
  relocate(
    time, .after = date
  )
weight_log <- weight_log %>% 
  mutate(
    time = format(as.POSIXct(mdy_hms(weight_log$date)), format = "%H:%M:%S"),
    date = as.Date(weight_log$date, "%m/%d/%Y")
  ) %>% 
  relocate(
    time, .after = date
  )
```

Time to standardize the ‘date’ format:

``` r
daily_activity <- daily_activity  %>%
  mutate(
    date = as.Date(date, format = "%m/%d/%Y")
  )
daily_intensities <- daily_intensities %>% 
  mutate(
    date = as.Date(date, format = "%m/%d/%Y")
  )
daily_steps <- daily_steps %>% 
  mutate(
    date = as.Date(date, format = "%m/%d/%Y")
  )
daily_calories <- daily_calories %>% 
  mutate(
    date = as.Date(date, format = "%m/%d/%Y")
  )
```

Next, let’s get rid of duplicates:

``` r
# Dropping duplicates
hourly_intensities <- hourly_intensities %>% 
  distinct()
hourly_calories <- hourly_calories %>% 
  distinct()
hourly_steps <- hourly_steps %>% 
  distinct()
daily_activity <- daily_activity %>% 
  distinct()
daily_intensities <- daily_intensities %>% 
  distinct()
daily_steps <- daily_steps %>% 
  distinct()
daily_calories <- daily_calories %>% 
  distinct()
daily_sleep <- daily_sleep %>% 
  distinct()
weight_log <- weight_log %>% 
  distinct()
```

We can validate that there are no duplicates:

``` r
# Checking for duplicates and missing values
cat("hourly_intensities:", sum(duplicated(hourly_intensities)), "duplicates\n")
```

    ## hourly_intensities: 0 duplicates

``` r
cat("hourly_calories:", sum(duplicated(hourly_calories)), "duplicates\n")
```

    ## hourly_calories: 0 duplicates

``` r
cat("hourly_steps:", sum(duplicated(hourly_steps)), "duplicates\n")
```

    ## hourly_steps: 0 duplicates

``` r
cat("daily_activity:", sum(duplicated(daily_activity)), "duplicates\n")
```

    ## daily_activity: 0 duplicates

``` r
cat("daily_intensities:", sum(duplicated(daily_intensities)), "duplicates\n")
```

    ## daily_intensities: 0 duplicates

``` r
cat("daily_steps:", sum(duplicated(daily_steps)), "duplicates\n")
```

    ## daily_steps: 0 duplicates

``` r
cat("daily_calories:", sum(duplicated(daily_calories)), "duplicates\n")
```

    ## daily_calories: 0 duplicates

``` r
cat("daily_sleep:", sum(duplicated(daily_sleep)), "duplicates\n")
```

    ## daily_sleep: 0 duplicates

``` r
cat("weight_log:", sum(duplicated(weight_log)), "duplicates\n")
```

    ## weight_log: 0 duplicates

## Analyze

### Unique Participants

As we look into each data set, it is important to understand whether the
number of participants of each set is sufficient for analysis.

``` r
# Finding the number of unique participants in each data set
cat("hourly_intensities:", n_distinct(hourly_intensities$id), "unique participants\n")
```

    ## hourly_intensities: 33 unique participants

``` r
cat("hourly_calories:", n_distinct(hourly_calories$id), "unique participants\n")
```

    ## hourly_calories: 33 unique participants

``` r
cat("hourly_steps:", n_distinct(hourly_steps$id), "unique participants\n")
```

    ## hourly_steps: 33 unique participants

``` r
cat("daily_activity:", n_distinct(daily_activity$id), "unique participants\n")
```

    ## daily_activity: 33 unique participants

``` r
cat("daily_intensities:", n_distinct(daily_intensities$id), "unique participants\n")
```

    ## daily_intensities: 33 unique participants

``` r
cat("daily_steps:", n_distinct(daily_steps$id), "unique participants\n")
```

    ## daily_steps: 33 unique participants

``` r
cat("daily_calories:", n_distinct(daily_calories$id), "unique participants\n")
```

    ## daily_calories: 33 unique participants

``` r
cat("daily_sleep:", n_distinct(daily_sleep$id), "unique participants\n")
```

    ## daily_sleep: 24 unique participants

``` r
cat("weight_log:", n_distinct(weight_log$id), "unique participants\n")
```

    ## weight_log: 8 unique participants

Almost all of the data sets have the same amount of distinct
participants, however, `daily_sleep` and `weight_log` have significantly
less. In particular, `weight_log` only features 8 participants. As a
result, we should avoid analyzing this data set as the sample size is
too small and even one outlier could skew analysis heavily.

### Summary Statistics

Before we dive into comparing specific columns of each data set, let’s
get an overview of the summary statistics using `summary()`:

``` r
# Summary Statistics
hourly_intensities %>% 
  select(
    total_intensity,
    average_intensity
  ) %>% 
  summary()
```

    ##  total_intensity  average_intensity
    ##  Min.   :  0.00   Min.   :0.0000   
    ##  1st Qu.:  0.00   1st Qu.:0.0000   
    ##  Median :  3.00   Median :0.0500   
    ##  Mean   : 12.04   Mean   :0.2006   
    ##  3rd Qu.: 16.00   3rd Qu.:0.2667   
    ##  Max.   :180.00   Max.   :3.0000

``` r
hourly_calories %>% 
  select(
    calories
  ) %>% 
  summary()
```

    ##     calories     
    ##  Min.   : 42.00  
    ##  1st Qu.: 63.00  
    ##  Median : 83.00  
    ##  Mean   : 97.39  
    ##  3rd Qu.:108.00  
    ##  Max.   :948.00

``` r
hourly_steps %>% 
  select(
    total_steps
  ) %>% 
  summary()
```

    ##   total_steps     
    ##  Min.   :    0.0  
    ##  1st Qu.:    0.0  
    ##  Median :   40.0  
    ##  Mean   :  320.2  
    ##  3rd Qu.:  357.0  
    ##  Max.   :10554.0

``` r
daily_activity %>% 
  select(
    total_distance,
    tracked_distance,
    logged_distance,
  ) %>% 
  summary()
```

    ##  total_distance   tracked_distance logged_distance 
    ##  Min.   : 0.000   Min.   : 0.000   Min.   :0.0000  
    ##  1st Qu.: 2.620   1st Qu.: 2.620   1st Qu.:0.0000  
    ##  Median : 5.245   Median : 5.245   Median :0.0000  
    ##  Mean   : 5.490   Mean   : 5.475   Mean   :0.1082  
    ##  3rd Qu.: 7.713   3rd Qu.: 7.710   3rd Qu.:0.0000  
    ##  Max.   :28.030   Max.   :28.030   Max.   :4.9421

``` r
daily_intensities %>% 
  select(
    sedentary_minutes,
    lightly_active_minutes,
    fairly_active_minutes,
    very_active_minutes,
    sedentary_active_distance,
    lightly_active_distance,
    moderately_active_distance,
    very_active_distance
  ) %>% 
  summary()
```

    ##  sedentary_minutes lightly_active_minutes fairly_active_minutes
    ##  Min.   :   0.0    Min.   :  0.0          Min.   :  0.00       
    ##  1st Qu.: 729.8    1st Qu.:127.0          1st Qu.:  0.00       
    ##  Median :1057.5    Median :199.0          Median :  6.00       
    ##  Mean   : 991.2    Mean   :192.8          Mean   : 13.56       
    ##  3rd Qu.:1229.5    3rd Qu.:264.0          3rd Qu.: 19.00       
    ##  Max.   :1440.0    Max.   :518.0          Max.   :143.00       
    ##  very_active_minutes sedentary_active_distance lightly_active_distance
    ##  Min.   :  0.00      Min.   :0.000000          Min.   : 0.000         
    ##  1st Qu.:  0.00      1st Qu.:0.000000          1st Qu.: 1.945         
    ##  Median :  4.00      Median :0.000000          Median : 3.365         
    ##  Mean   : 21.16      Mean   :0.001606          Mean   : 3.341         
    ##  3rd Qu.: 32.00      3rd Qu.:0.000000          3rd Qu.: 4.782         
    ##  Max.   :210.00      Max.   :0.110000          Max.   :10.710         
    ##  moderately_active_distance very_active_distance
    ##  Min.   :0.0000             Min.   : 0.000      
    ##  1st Qu.:0.0000             1st Qu.: 0.000      
    ##  Median :0.2400             Median : 0.210      
    ##  Mean   :0.5675             Mean   : 1.503      
    ##  3rd Qu.:0.8000             3rd Qu.: 2.053      
    ##  Max.   :6.4800             Max.   :21.920

``` r
daily_steps %>% 
  select(
    total_steps
  ) %>% 
  summary()
```

    ##   total_steps   
    ##  Min.   :    0  
    ##  1st Qu.: 3790  
    ##  Median : 7406  
    ##  Mean   : 7638  
    ##  3rd Qu.:10727  
    ##  Max.   :36019

``` r
daily_calories %>% 
  select(
    calories
  ) %>% 
  summary()
```

    ##     calories   
    ##  Min.   :   0  
    ##  1st Qu.:1828  
    ##  Median :2134  
    ##  Mean   :2304  
    ##  3rd Qu.:2793  
    ##  Max.   :4900

``` r
daily_sleep %>% 
  select(
    total_sleep_records,
    total_minutes_asleep,
    total_time_in_bed
  ) %>% 
  summary()
```

    ##  total_sleep_records total_minutes_asleep total_time_in_bed
    ##  Min.   :1.00        Min.   : 58.0        Min.   : 61.0    
    ##  1st Qu.:1.00        1st Qu.:361.0        1st Qu.:403.8    
    ##  Median :1.00        Median :432.5        Median :463.0    
    ##  Mean   :1.12        Mean   :419.2        Mean   :458.5    
    ##  3rd Qu.:1.00        3rd Qu.:490.0        3rd Qu.:526.0    
    ##  Max.   :3.00        Max.   :796.0        Max.   :961.0

``` r
weight_log %>% 
  select(
    weight_lb,
    fat,
    bmi
  ) %>% 
  summary()
```

    ##    weight_lb          fat             bmi       
    ##  Min.   :116.0   Min.   :22.00   Min.   :21.45  
    ##  1st Qu.:135.4   1st Qu.:22.75   1st Qu.:23.96  
    ##  Median :137.8   Median :23.50   Median :24.39  
    ##  Mean   :158.8   Mean   :23.50   Mean   :25.19  
    ##  3rd Qu.:187.5   3rd Qu.:24.25   3rd Qu.:25.56  
    ##  Max.   :294.3   Max.   :25.00   Max.   :47.54  
    ##                  NA's   :65

We can make a few observations based on these summary statistics about
this sample group:

- The median sleep time is **7.21 hours**, which is more than the [CDC’s
  recommended amount of sleep for
  adults](https://www.cdc.gov/sleep/data-research/facts-stats/adults-sleep-facts-and-stats.html#:~:text=The%20recommended%20amount%20of%20sleep,Getting%20insufficient%20sleep.).
- **7,406** is the median amount of steps, which is well below the
  [10,000](https://www.cdc.gov/pcd/issues/2016/pdf/16_0111.pdf) steps
  commonly cited as a daily goal.
- The median sedentary time is **17.63 hours**. If sleep is removed from
  this number, the amount is still incredibly high, at **10.42 hours**.
  [High sedentary activity can increase the risk of type-2 diabetes,
  cancer, and heart
  disease](https://www.who.int/europe/publications/i/item/9789240014886).
- The combined total of median fairly active minutes and very active
  minutes is only **70 minutes** a week, a number well below the
  [recommended](https://www.who.int/publications/i/item/9789241599979)
  **150** minutes of moderately active activity or **75** minutes of
  very active activity.

### Visualizing Basic Trends

While some general statistics can be derived from the given FitBit
dataset, we should dive deeper into the data and find more trends from
comparing different variables.

One key data point we can use to guide our takeaways from this dataset
is the calories column. Using the `calories` column, we can track the
effectiveness of the different measurements listed in the dataset. One
such measurement is the amount of daily steps. Let’s graph the
relationship between `total_steps` and `calories`.

``` r
# Visualizing total_steps vs. calories
daily_activity %>% 
  ggplot(aes(x = total_steps, y = calories)) +
  geom_point() +
  geom_smooth() +
  theme_minimal() +
  labs(title = "Total Steps vs. Calories Burned") + 
  xlab("Total Steps") +
  ylab("Calories Burned")
```

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

![](bellabeat_case_study_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->
As expected, the graph illustrates that there is a clear correllation
between calories burned and total steps on a day-to-day basis.

However, is this the same with different levels of activity? Let’s
compare the time spent on each level of activity with the daily
calories.

First, we can start from `sedentary_minutes` and make our way towards
`very_active_minutes`.

``` r
# Visualizing sedentary_minutes vs. calories
daily_activity %>% 
  ggplot(aes(x = sedentary_minutes, y = calories)) +
  geom_point() +
  geom_smooth() + 
  theme_minimal() +
  labs(title = "Sedentary Minutes vs. Calories Burned") +
  xlab("Sedentary Minutes") +
  ylab("Calories Burned")
```

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

![](bellabeat_case_study_files/figure-gfm/unnamed-chunk-14-1.png)<!-- -->
Unsurprisingly, `sedentary_minutes` does not seem to significantly
increase the total calories burned, as it includes time spent sleeping.
However, the downwards trend near the end of the graph demonstrates that
as it takes up more of a person’s day, `sedentary_minutes` can take away
from time spent on strenous activity, reducing the amount of calories
burned.

Next, let’s inspect `lightly_active_minutes` vs. `calories`:

``` r
# Visualizing lightly_active_minutes vs. calories
daily_activity %>% 
  ggplot(aes(x = lightly_active_minutes, y = calories)) +
  geom_point() +
  geom_smooth() + 
  theme_minimal() +
  labs(title = "Lightly Active Minutes vs. Calories Burned") +
  xlab("Lightly Active Minutes") +
  ylab("Calories Burned")
```

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

![](bellabeat_case_study_files/figure-gfm/unnamed-chunk-15-1.png)<!-- -->
Similar to the `sedentary_minutes` vs `calories` graph,
`lightly_active_minutes` does not seem to have a significant positive
increase on daily calories burned. However, unlike sedentary activity,
light physical activity does seem to not hamper total calories burned.

Next, we can visualize the effect `fairly_active_minutes` has on
`calories`:

``` r
# Visualizing fairly_active_minutes vs. calories
daily_activity %>% 
  ggplot(aes(x = fairly_active_minutes, y = calories)) +
  geom_point() +
  geom_smooth() + 
  theme_minimal() +
  labs(title = "Fairly Active Minutes vs. Calories Burned") +
  xlab("Fairly Active Minutes") +
  ylab("Calories Burned")
```

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

![](bellabeat_case_study_files/figure-gfm/unnamed-chunk-16-1.png)<!-- -->
The trend line at the beginning section of the graph, where the majority
of data points are located, indicates that fairly active physical
activity can have some impact on total daily calories, however, it seems
more limited in its effect. Nonetheless, it appears to have a marginally
higher impact than lightly active physical activity.

Finally, let’s visualize how `very_active_minutes` effects `calories`:

``` r
# Visualizing very_active_minutes vs. calories
daily_activity %>% 
  ggplot(aes(x = very_active_minutes, y = calories)) +
  geom_point() +
  geom_smooth() + 
  theme_minimal() +
  labs(title = "Very Active Minutes vs. Calories Burned") +
  xlab("Very Active Minutes") +
  ylab("Calories Burned")
```

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

![](bellabeat_case_study_files/figure-gfm/unnamed-chunk-17-1.png)<!-- -->
From this graph, we can see that `very_active_minutes` has a significant
impact on `calories`. The consequences of very active activity is
substantially higher than all other forms of physical activity,
reflected in the trend line.

### Trends on each Day of the Week

Now that we can be fairly certain that the activity level has a high
impact on calories burned, let’s dive deeper into the different
variables that can affect each user.

One variable that could have a potential impact is the day of the week.
Each day of the week could each play a significant role in each user’s
physical activity level for that day. For example, Mondays, which are
typically followed by a workday on Tuesday, might leave less free time
than a Friday, where users usually do not have to worry about working
the next day.

To inspect this aspect of the data, we should create a new variable
categorizing activity into a certain day:

``` r
# Adding a new column for the day of the week to each dataset
daily_activity <- daily_activity %>% 
  mutate(
    day_of_week = weekdays(daily_activity$date)
  ) %>% 
  relocate(
    day_of_week, .after = date
  )
hourly_intensities <- hourly_intensities %>% 
  mutate(
    day_of_week = weekdays(hourly_intensities$date)
  ) %>% 
  relocate(
    day_of_week, .after = date
  )
hourly_calories <- hourly_calories %>% 
  mutate(
    day_of_week = weekdays(hourly_calories$date)
  ) %>% 
  relocate(
    day_of_week, .after = date
  )
hourly_steps <- hourly_steps %>% 
  mutate(
    day_of_week = weekdays(hourly_steps$date)
  ) %>% 
  relocate(
    day_of_week, .after = date
  )
daily_intensities <- daily_intensities %>% 
  mutate(
    day_of_week = weekdays(daily_intensities$date)
  ) %>% 
  relocate(
    day_of_week, .after = date
  )
daily_steps <- daily_steps %>% 
  mutate(
    day_of_week = weekdays(daily_steps$date)
  ) %>% 
  relocate(
    day_of_week, .after = date
  )
daily_calories <- daily_calories %>% 
  mutate(
    day_of_week = weekdays(daily_calories$date)
  ) %>% 
  relocate(
    day_of_week, .after = date
  )
daily_sleep  <- daily_sleep %>% 
  mutate(
    day_of_week = weekdays(daily_sleep$date)
  ) %>% 
  relocate(
    day_of_week, .after = date
  )
weight_log <- weight_log %>% 
  mutate(
    day_of_week = weekdays(weight_log$date)
  ) %>% 
  relocate(
    day_of_week, .after = date
  )
```

Then we can visualize activity levels vs. `day_of_week`:

``` r
# Visualizing activity levels vs. `day_of_week`
day_of_week_ordered <- factor(
  daily_activity$day_of_week, 
  levels = c("Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday")
  )
daily_activity %>% 
  ggplot(aes(x = day_of_week_ordered, y = very_active_minutes)) +
  geom_point(position = "jitter", alpha = 0.25) +
  geom_smooth() + 
  theme_minimal() +
  labs(title = "Day of Week vs. Very Active Minutes") +
  xlab("Day of Week") +
  ylab("Very Active Minutes") +
  stat_summary(
    geom = "point",
    fun = median,
    color = "red",
    size = 2,
    alpha = 0.7
  ) +
  theme(axis.text.x = element_text(angle=45, vjust=1, hjust=1))
```

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

![](bellabeat_case_study_files/figure-gfm/unnamed-chunk-19-1.png)<!-- -->
We can see that `very_active_minutes` actually drops on Friday,
Saturday, and Sunday, where people usually have more free time. This
could be due to a priority on spending time outside of high physical
activity, focusing on social activities instead. With that said, the
actual difference in time spent on very active activity is not
significant enough to create a noticeable trend.

Next, let’s look at calories burned on each day of the week:

``` r
# Visualizing `calories` vs. `day_of_week`
daily_activity %>% 
  ggplot(aes(x = day_of_week_ordered, y = calories)) +
  geom_point(position = "jitter", alpha = 0.25) +
  geom_smooth() + 
  theme_minimal() +
  labs(title = "Day of Week vs. Calories") +
  xlab("Day of Week") +
  ylab("Calories") +
  stat_summary(
    geom = "point",
    fun = median,
    color = "red",
    size = 2,
    alpha = 0.7
  ) +
  theme(axis.text.x = element_text(angle=45, vjust=1, hjust=1))
```

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

![](bellabeat_case_study_files/figure-gfm/unnamed-chunk-20-1.png)<!-- -->
Similar to `very_active_minutes`, there doesn’t seem to be a significant
difference in the amount of calories burned relative to the
`day_of_week`.

### Displaying Sleep Activity

Next, let’s see how sleep affects daily calories. First, by merging
`daily_activity` and `daily_sleep`, and then visualizing whether there
is any correlation between `sleep` and `calories`.

``` r
# Merging daily_activity and daily_sleep
daily_activity_sleep_merged <- full_join(daily_activity, daily_sleep, by = c("id", "date", "day_of_week"))
daily_activity_sleep_merged <- daily_activity_sleep_merged[complete.cases(daily_activity_sleep_merged), ]

# Visualizing total minutes asleep vs. calories burned daily
daily_activity_sleep_merged %>% 
  ggplot(aes(x = total_minutes_asleep, y = calories)) +
  geom_point() +
  geom_smooth() + 
  theme_minimal() +
  labs(title = "Total Minutes Asleep vs. Calories Burned") +
  xlab("Total Minutes Asleep") +
  ylab("Calories Burned")
```

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

![](bellabeat_case_study_files/figure-gfm/unnamed-chunk-21-1.png)<!-- -->
As we can see, there is not a clear correlation on total minutes asleep
and calories burned on a daily basis. This does not indicate that sleep
is not important to a person’s health though, merely that calories
burned are not directly affected.

Let’s check how sleep varies on weekdays in comparison to weekends:

``` r
# Visualizing `total_minutes_asleep` vs. `day_of_week`
day_of_week_ordered <- factor(
  daily_activity_sleep_merged$day_of_week, 
  levels = c("Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday")
  )
daily_activity_sleep_merged %>% 
  ggplot(aes(x = day_of_week_ordered, y = total_minutes_asleep)) +
  geom_point(position = "jitter", alpha = 0.25) +
  geom_smooth() + 
  theme_minimal() +
  labs(title = "Day of Week vs. Total Minutes Asleep") +
  xlab("Day of Week") +
  ylab("Total Minutes Asleep") +
  stat_summary(
    geom = "point",
    fun = median,
    color = "red",
    size = 2,
    alpha = 0.7
  ) +
  theme(axis.text.x = element_text(angle=45, vjust=1, hjust=1))
```

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

![](bellabeat_case_study_files/figure-gfm/unnamed-chunk-22-1.png)<!-- -->
Although there is not a significant amount of variation between each day
of the week, weekends, and Sunday in particular, seems to have a higher
amount of minutes asleep. So it may be better for users to keep an eye
out on how much they sleep on weekdays, especially Friday.

## Share

### Hourly Comparisons

We understand that generally, very active physical activity is
incredibly beneficial for burning daily calories, however, how does that
vary during the day? Are users burning more calories during the morning
or evening? The `hourly_calories`, `hourly_intensities`, and
`hourly_steps` data frames can provide important insights into these
questions.

First, let’s combine these data frames:

``` r
# Merging daily_activity and daily_sleep
hourly_calories_intensities <- full_join(hourly_calories, hourly_intensities, by = c("id", "date", "day_of_week", "time"))
hourly_activity <- full_join(hourly_calories_intensities, hourly_steps, by = c("id", "date", "day_of_week", "time"))
head(hourly_activity)
```

    ## # A tibble: 6 × 8
    ##       id date       day_of_week time  calories total_intensity average_intensity
    ##    <dbl> <date>     <chr>       <chr>    <dbl>           <dbl>             <dbl>
    ## 1 1.50e9 2016-04-12 Tuesday     00:0…       81              20             0.333
    ## 2 1.50e9 2016-04-12 Tuesday     01:0…       61               8             0.133
    ## 3 1.50e9 2016-04-12 Tuesday     02:0…       59               7             0.117
    ## 4 1.50e9 2016-04-12 Tuesday     03:0…       47               0             0    
    ## 5 1.50e9 2016-04-12 Tuesday     04:0…       48               0             0    
    ## 6 1.50e9 2016-04-12 Tuesday     05:0…       48               0             0    
    ## # ℹ 1 more variable: total_steps <dbl>

Then, we can see what time users are being very active:

``` r
hourly_activity %>% 
  ggplot(aes(x = time)) +
  geom_bar(aes(weight = total_intensity), stat = "count") +
  theme_minimal() +
  labs(title = "Hour vs. Intensity Count ") +
  xlab("Hour") +
  ylab("Intensity Count") +
  theme(axis.text.x = element_text(angle=50, vjust=1, hjust=1))
```

![](bellabeat_case_study_files/figure-gfm/unnamed-chunk-24-1.png)<!-- -->
From the chart, it seems that around 8:00 AM, users start to be more
active, peaking from 5:00 PM to 7:00 PM. It is likely that around this
time, users are going to the gym, enduring more active physical activity
in comparison to the work day.

Additionally, we can display when users are burning calories throughout
the day:

``` r
hourly_activity %>% 
  ggplot(aes(x = time)) +
  geom_bar(aes(weight = calories), stat = "count") +
  theme_minimal() +
  labs(title = "Hour vs. Calorie Count") +
  xlab("Hour") +
  ylab("Calorie Count") +
  theme(axis.text.x = element_text(angle=50, vjust=1, hjust=1))
```

![](bellabeat_case_study_files/figure-gfm/unnamed-chunk-25-1.png)<!-- -->
Similar to the previous chart depicting user intensity by hour, the
calories burned seem to peak around 5:00 PM to 7:00 PM. However, there
is significant calorie usage early in the day, especially starting from
8:00 AM.

## Act

After completing the key steps of establishing the goals of this case
study, preparing the given data for analysis, processing the data,
inspecting and cleaning each data set, and analyzing the various trends
that emerge from the data, we can provide our recommendations to the
Bellabeat Marketing Team as well as how it can be applied to current
Bellabeat products.

### Recommendations

**In order to continue Bellabeat’s journey in its growth amidst the
global smart device market, we suggest:**

- Highlighting **Time’s** capability to track sleep and user activity.
  In particular, inform consumers of the significance of **very active
  physical activity** when burning daily calories.
- Emphasizing ease of use when **Time** is connected to the **Bellabeat
  app**.
- Reminding users, via the **Bellabeat app** or **Time**, to exercise
  specifically around **5:00 PM** to **7:00 PM**, as those are the peak
  hours of recorded physical intensity.
- Calling attention to important metrics for each user, such as their
  daily step count and amount of time spent on fairly active to very
  active physical activity, using the **Bellabeat app**. Daily step
  count can be compared to the recommended goal of
  **[10,000](https://www.cdc.gov/pcd/issues/2016/pdf/16_0111.pdf)
  steps**, while goals of weekly minutes of heightened physical activity
  can reference the
  [recommended](https://www.who.int/publications/i/item/9789241599979)
  **150** minutes of moderately active activity or **75** minutes of
  very active activity. To further encourage users to increase physical
  activity, a digital point system can be used to compare daily or
  weekly step goals and physical activity levels to past activity.

### **Thank You!**
