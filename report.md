## Introduction

### Bacground Information

The following project was used to analyze the grooming patterns of
Nothern America’s River Otters. Before doing any analysis on the
dataset, it was a important to have some background information on the
river residing animals. River otters can be diurnal or nocturnal, with
most being more active at night. Giant otters are only active during the
day. Clawless otters are primarily nocturnal, though some individuals
may be active during the day in remote areas where humans are not
present. Sea otters are mostly active during the day. Feeding and
grooming are the primary daily activities, which are interspersed with
rest periods. Note that the claws aspects and sleeping patterns of
otters are not explored for this project. Grooming To keep their fur
insulating, all otters must groom it constantly. River otters spend a
significant amount of time grooming, and many species have designated
areas on land for drying and grooming their fur. The majority of them
dry themselves vigorously by rolling on the ground or rubbing against
logs or vegetation. Researchers discovered that sea otters spend at
least 11% to 48% of their day grooming. They comb their fur and remove
debris with their paws and claws. They can also aerate their fur by
blowing air into it and pounding the water with their feet to create
foam. Because of its flexible body and loose-fitting skin, an otter can
reach every part of its fur.

### The Dataset

The dataset had the following features:

**Group-This was a group of otters that was watched at one time. For
instance group A had one female and 3 males** **Season -There were only
two categories; whether it was the breeding season or not** **Time - The
time in minutes the group was watched** **Groomer - The otter that did
the grooming. They were named by gender and a number was asssigned**
**Groomee - The otter that was groomed. The same naming convention was
as the groomers** **Frequency – The number of instances of grooming that
occurred.** \## objectives The project had the following objectives: 1.
Determine whether animals in each group groomed equally 2. Identify any
preferences in terms of grooming in each group. 3. Determine if males
groomed more than females 4. Mark any differences in the grooming rates
in each season \## Tools Used The analysis of the otter dataset was done
in R and relied heavily on the Tidyverse package which included the
ggplot plotting libarary that was used in the visualization that are
featured in this report.

## Data Engineering

Before the dataset could be summarized into plots, it was first mutated
into a tibble to allow for better handling and summarizing of the data.
The data was provided in the format of an R datafile. The first action
that was done was to find the genders of the groomers and groomees.
`Case_when` method was used to determine the genders of each otter since
the first letter indicated the otter’s gender.

    load("classdata.RData")
    otter = as.data.frame(otter)
    head(otter)

    ##   group season time groomer groomee frequency
    ## 1     A      N  127      F1      M2         6
    ## 2     A      N  127      F1      M3         1
    ## 3     A      N  127      F1      M4         2
    ## 4     A      N  127      M2      F1         0
    ## 5     A      N  127      M2      M3         0
    ## 6     A      N  127      M2      M4         0

    otter = as_tibble(otter)
    head(otter)

    ## # A tibble: 6 × 6
    ##   group season  time groomer groomee frequency
    ##   <chr> <chr>  <dbl> <chr>   <chr>       <dbl>
    ## 1 A     N        127 F1      M2              6
    ## 2 A     N        127 F1      M3              1
    ## 3 A     N        127 F1      M4              2
    ## 4 A     N        127 M2      F1              0
    ## 5 A     N        127 M2      M3              0
    ## 6 A     N        127 M2      M4              0

    # Adding column based on other column:
    otter_df = otter %>%
      mutate(groomer_gender = case_when(
        startsWith(otter$groomer, "F") ~ "FEMALE",
        startsWith(otter$groomer, "M") ~ "MALE"
      ))%>%
      mutate(groomee_gender = case_when(
        startsWith(otter$groomee, "F") ~ "FEMALE",
        startsWith(otter$groomee, "M") ~ "MALE"
      ))
    otter_df %>% 
      head() %>%
      kable()

<table>
<colgroup>
<col style="width: 8%" />
<col style="width: 9%" />
<col style="width: 6%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 13%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<thead>
<tr class="header">
<th style="text-align: left;">group</th>
<th style="text-align: left;">season</th>
<th style="text-align: right;">time</th>
<th style="text-align: left;">groomer</th>
<th style="text-align: left;">groomee</th>
<th style="text-align: right;">frequency</th>
<th style="text-align: left;">groomer_gender</th>
<th style="text-align: left;">groomee_gender</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: left;">A</td>
<td style="text-align: left;">N</td>
<td style="text-align: right;">127</td>
<td style="text-align: left;">F1</td>
<td style="text-align: left;">M2</td>
<td style="text-align: right;">6</td>
<td style="text-align: left;">FEMALE</td>
<td style="text-align: left;">MALE</td>
</tr>
<tr class="even">
<td style="text-align: left;">A</td>
<td style="text-align: left;">N</td>
<td style="text-align: right;">127</td>
<td style="text-align: left;">F1</td>
<td style="text-align: left;">M3</td>
<td style="text-align: right;">1</td>
<td style="text-align: left;">FEMALE</td>
<td style="text-align: left;">MALE</td>
</tr>
<tr class="odd">
<td style="text-align: left;">A</td>
<td style="text-align: left;">N</td>
<td style="text-align: right;">127</td>
<td style="text-align: left;">F1</td>
<td style="text-align: left;">M4</td>
<td style="text-align: right;">2</td>
<td style="text-align: left;">FEMALE</td>
<td style="text-align: left;">MALE</td>
</tr>
<tr class="even">
<td style="text-align: left;">A</td>
<td style="text-align: left;">N</td>
<td style="text-align: right;">127</td>
<td style="text-align: left;">M2</td>
<td style="text-align: left;">F1</td>
<td style="text-align: right;">0</td>
<td style="text-align: left;">MALE</td>
<td style="text-align: left;">FEMALE</td>
</tr>
<tr class="odd">
<td style="text-align: left;">A</td>
<td style="text-align: left;">N</td>
<td style="text-align: right;">127</td>
<td style="text-align: left;">M2</td>
<td style="text-align: left;">M3</td>
<td style="text-align: right;">0</td>
<td style="text-align: left;">MALE</td>
<td style="text-align: left;">MALE</td>
</tr>
<tr class="even">
<td style="text-align: left;">A</td>
<td style="text-align: left;">N</td>
<td style="text-align: right;">127</td>
<td style="text-align: left;">M2</td>
<td style="text-align: left;">M4</td>
<td style="text-align: right;">0</td>
<td style="text-align: left;">MALE</td>
<td style="text-align: left;">MALE</td>
</tr>
</tbody>
</table>

Before proceeding the Groomer and Gromee otters were paired together to
form mates column:

    otter_df = otter_df  %>% 
      mutate(otter_df, mates = paste(otter_df$groomer, otter_df$groomee))

    otter_df %>% 
      head() %>%
      kable()

<table style="width:100%;">
<colgroup>
<col style="width: 7%" />
<col style="width: 8%" />
<col style="width: 6%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 12%" />
<col style="width: 18%" />
<col style="width: 18%" />
<col style="width: 7%" />
</colgroup>
<thead>
<tr class="header">
<th style="text-align: left;">group</th>
<th style="text-align: left;">season</th>
<th style="text-align: right;">time</th>
<th style="text-align: left;">groomer</th>
<th style="text-align: left;">groomee</th>
<th style="text-align: right;">frequency</th>
<th style="text-align: left;">groomer_gender</th>
<th style="text-align: left;">groomee_gender</th>
<th style="text-align: left;">mates</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: left;">A</td>
<td style="text-align: left;">N</td>
<td style="text-align: right;">127</td>
<td style="text-align: left;">F1</td>
<td style="text-align: left;">M2</td>
<td style="text-align: right;">6</td>
<td style="text-align: left;">FEMALE</td>
<td style="text-align: left;">MALE</td>
<td style="text-align: left;">F1 M2</td>
</tr>
<tr class="even">
<td style="text-align: left;">A</td>
<td style="text-align: left;">N</td>
<td style="text-align: right;">127</td>
<td style="text-align: left;">F1</td>
<td style="text-align: left;">M3</td>
<td style="text-align: right;">1</td>
<td style="text-align: left;">FEMALE</td>
<td style="text-align: left;">MALE</td>
<td style="text-align: left;">F1 M3</td>
</tr>
<tr class="odd">
<td style="text-align: left;">A</td>
<td style="text-align: left;">N</td>
<td style="text-align: right;">127</td>
<td style="text-align: left;">F1</td>
<td style="text-align: left;">M4</td>
<td style="text-align: right;">2</td>
<td style="text-align: left;">FEMALE</td>
<td style="text-align: left;">MALE</td>
<td style="text-align: left;">F1 M4</td>
</tr>
<tr class="even">
<td style="text-align: left;">A</td>
<td style="text-align: left;">N</td>
<td style="text-align: right;">127</td>
<td style="text-align: left;">M2</td>
<td style="text-align: left;">F1</td>
<td style="text-align: right;">0</td>
<td style="text-align: left;">MALE</td>
<td style="text-align: left;">FEMALE</td>
<td style="text-align: left;">M2 F1</td>
</tr>
<tr class="odd">
<td style="text-align: left;">A</td>
<td style="text-align: left;">N</td>
<td style="text-align: right;">127</td>
<td style="text-align: left;">M2</td>
<td style="text-align: left;">M3</td>
<td style="text-align: right;">0</td>
<td style="text-align: left;">MALE</td>
<td style="text-align: left;">MALE</td>
<td style="text-align: left;">M2 M3</td>
</tr>
<tr class="even">
<td style="text-align: left;">A</td>
<td style="text-align: left;">N</td>
<td style="text-align: right;">127</td>
<td style="text-align: left;">M2</td>
<td style="text-align: left;">M4</td>
<td style="text-align: right;">0</td>
<td style="text-align: left;">MALE</td>
<td style="text-align: left;">MALE</td>
<td style="text-align: left;">M2 M4</td>
</tr>
</tbody>
</table>

    otter_df = otter_df %>%
      mutate(mating_type = case_when(
        startsWith(otter_df$groomer_gender, "F")& startsWith(otter_df$groomee_gender, "M") ~ "HETEROSEXUAL",
        startsWith(otter_df$groomer_gender, "M")& startsWith(otter_df$groomee_gender, "F") ~ "HETEROSEXUAL",
        startsWith(otter_df$groomer_gender, "F")& startsWith(otter_df$groomee_gender, "F") ~ "HOMOSEXUAL",
        startsWith(otter_df$groomer_gender, "M")& startsWith(otter_df$groomee_gender, "M") ~ "HOMOSEXUAL",
      ))
    otter_df %>% 
      head() %>%
      kable()

<table style="width:100%;">
<colgroup>
<col style="width: 6%" />
<col style="width: 7%" />
<col style="width: 5%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 10%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 6%" />
<col style="width: 13%" />
</colgroup>
<thead>
<tr class="header">
<th style="text-align: left;">group</th>
<th style="text-align: left;">season</th>
<th style="text-align: right;">time</th>
<th style="text-align: left;">groomer</th>
<th style="text-align: left;">groomee</th>
<th style="text-align: right;">frequency</th>
<th style="text-align: left;">groomer_gender</th>
<th style="text-align: left;">groomee_gender</th>
<th style="text-align: left;">mates</th>
<th style="text-align: left;">mating_type</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: left;">A</td>
<td style="text-align: left;">N</td>
<td style="text-align: right;">127</td>
<td style="text-align: left;">F1</td>
<td style="text-align: left;">M2</td>
<td style="text-align: right;">6</td>
<td style="text-align: left;">FEMALE</td>
<td style="text-align: left;">MALE</td>
<td style="text-align: left;">F1 M2</td>
<td style="text-align: left;">HETEROSEXUAL</td>
</tr>
<tr class="even">
<td style="text-align: left;">A</td>
<td style="text-align: left;">N</td>
<td style="text-align: right;">127</td>
<td style="text-align: left;">F1</td>
<td style="text-align: left;">M3</td>
<td style="text-align: right;">1</td>
<td style="text-align: left;">FEMALE</td>
<td style="text-align: left;">MALE</td>
<td style="text-align: left;">F1 M3</td>
<td style="text-align: left;">HETEROSEXUAL</td>
</tr>
<tr class="odd">
<td style="text-align: left;">A</td>
<td style="text-align: left;">N</td>
<td style="text-align: right;">127</td>
<td style="text-align: left;">F1</td>
<td style="text-align: left;">M4</td>
<td style="text-align: right;">2</td>
<td style="text-align: left;">FEMALE</td>
<td style="text-align: left;">MALE</td>
<td style="text-align: left;">F1 M4</td>
<td style="text-align: left;">HETEROSEXUAL</td>
</tr>
<tr class="even">
<td style="text-align: left;">A</td>
<td style="text-align: left;">N</td>
<td style="text-align: right;">127</td>
<td style="text-align: left;">M2</td>
<td style="text-align: left;">F1</td>
<td style="text-align: right;">0</td>
<td style="text-align: left;">MALE</td>
<td style="text-align: left;">FEMALE</td>
<td style="text-align: left;">M2 F1</td>
<td style="text-align: left;">HETEROSEXUAL</td>
</tr>
<tr class="odd">
<td style="text-align: left;">A</td>
<td style="text-align: left;">N</td>
<td style="text-align: right;">127</td>
<td style="text-align: left;">M2</td>
<td style="text-align: left;">M3</td>
<td style="text-align: right;">0</td>
<td style="text-align: left;">MALE</td>
<td style="text-align: left;">MALE</td>
<td style="text-align: left;">M2 M3</td>
<td style="text-align: left;">HOMOSEXUAL</td>
</tr>
<tr class="even">
<td style="text-align: left;">A</td>
<td style="text-align: left;">N</td>
<td style="text-align: right;">127</td>
<td style="text-align: left;">M2</td>
<td style="text-align: left;">M4</td>
<td style="text-align: right;">0</td>
<td style="text-align: left;">MALE</td>
<td style="text-align: left;">MALE</td>
<td style="text-align: left;">M2 M4</td>
<td style="text-align: left;">HOMOSEXUAL</td>
</tr>
</tbody>
</table>

## Analysis:

### Grooming Patterns of Each Group

In the following two plots the first two objectives are met, that is: •
Determine whether animals in each group groomed equally • Identify any
preferences in terms of grooming in each group. The first objective wa
to determine if the animals in each group groomed equally. For instance,
in Group A there was one female and three males. Did the female groom
equally with all the females? It is important to note that for each
group the female partner had multiple partners. The female otter groomed
with all the males in the group, however, the highest number of grouping
was done with M2. The least number of grouping was done between the
males. The otters thefore do not groom equally between each other.

![](report_files/figure-markdown_strict/unnamed-chunk-4-1.png)

We also note that the frequency of grooming has reduces significantly
when it is not the breeding season. Although the pattern of which otter
grooms one with the other remains the same. From the plot below we note
that the otters maintain their preference in the partners they groom
with.

    ## No summary function supplied, defaulting to `mean_se()`
    ## No summary function supplied, defaulting to `mean_se()`

![](report_files/figure-markdown_strict/unnamed-chunk-5-1.png)

### Grooming Patterns For Each Gender:

<table>
<thead>
<tr class="header">
<th style="text-align: left;">groomee_gender</th>
<th style="text-align: right;">n</th>
<th style="text-align: right;">prop</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: left;">FEMALE</td>
<td style="text-align: right;">152</td>
<td style="text-align: right;">0.3857868</td>
</tr>
<tr class="even">
<td style="text-align: left;">MALE</td>
<td style="text-align: right;">242</td>
<td style="text-align: right;">0.6142132</td>
</tr>
</tbody>
</table>

Females were observed to groomers 38.6% percent of the times.However, it
is important to note that the number of males in this dataset is higher
than that of females. The proportion does not change whether its the
breeding season or not. The bar plot below for each group gives a more
objective look at groomers for each group in each season

    otter_df %>% # data # manipulation
      ggplot()+
      geom_bar(aes(x=season, y = frequency, 
                   fill=groomer_gender), # separate by category
               stat = "summary", 
               position="dodge")+ # make it side-by-side
             theme_light()+facet_wrap(~group)+ggtitle("Grooming Patterns Group, Gender, and Season Wise Gender")

    ## No summary function supplied, defaulting to `mean_se()`
    ## No summary function supplied, defaulting to `mean_se()`
    ## No summary function supplied, defaulting to `mean_se()`
    ## No summary function supplied, defaulting to `mean_se()`
    ## No summary function supplied, defaulting to `mean_se()`

![](report_files/figure-markdown_strict/unnamed-chunk-7-1.png)

### Grooming Rates Per Season

    otter_df %>% # data # manipulation
      ggplot()+
      geom_bar(aes(x=season, y = frequency, 
                   fill=groomee_gender), # separate by category
               stat = "summary", 
               position="dodge")+ # make it side-by-side
      theme_light()+ggtitle("Grooming Frequency For Each Season")

    ## No summary function supplied, defaulting to `mean_se()`

![](report_files/figure-markdown_strict/unnamed-chunk-8-1.png)

We note that the grooming frequency in the breeding season is higher,
than when they are not breeding.

### Same Sex Grooming

Same Sex Grooming was also explored for each of the groups:

    # create a dataset
    mating_type <- otter_df$mating_type
    group <- otter_df$group
    time <- otter_df$time
    season = otter_df$season
    data <- data.frame(mating_type,group,time, season)

    # Grouped
    ggplot(data, aes(fill=group, y=time, x=mating_type)) + 
      geom_bar(position="dodge", stat="identity") + facet_wrap(~season)+theme_light()+ggtitle("Observation Time for Each Group in each Season")

![](report_files/figure-markdown_strict/unnamed-chunk-9-1.png)

There were only two groups that displayed same sex grooming that is
Group A and H In terms of time, the time of observation of the groups
was shorter in the breeding season except for group B.

    # create a dataset
    mating_type <- otter_df$mating_type
    group <- otter_df$group
    frequency <- otter_df$frequency
    season = otter_df$season
    data <- data.frame(mating_type,group,frequency, season)

    # Grouped
    ggplot(data, aes(fill=group, y=frequency, x=mating_type)) + 
      geom_bar(position="dodge", stat="identity") + facet_wrap(~season)+theme_light()+ggtitle("Grooming Frequency For Each Season")

![](report_files/figure-markdown_strict/unnamed-chunk-10-1.png)

Same sex grooming was a bit higher in the Not breeding season while the
opposite trend was observed in the heterosexual grooming(referring to
grooming between different sexes), it was higher in the breeding season
but dropped when it was not the breeding season. \## Conclusions

**1. Note that for each group the female partner had multiple partners,
therefore mated with the available otters.** **2. The least number of
grouping was done between the males. The otters thefore do not groom
equally between each other.** **3. We note that the grooming frequency
in the breeding season is higher, than when they are not breeding.**
**4. Same sex grooming was slightly higher during the non-breeding
season, whereas heterosexual grooming (referring to grooming between
different sexes) was higher during the breeding season but decreased
during the non-breeding season.**
