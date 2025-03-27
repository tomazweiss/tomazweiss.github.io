---
title: "The Decline of Stack Overflow"
excerpt_separator: "<!--more-->"
categories:
  - blog
tags:
  - R
  - ggplot2
  - visualization
  - salaries
  - stackoverflow
classes: wide
---


For many years, Stack Overflow has been the go-to platform for developers seeking solutions to coding problems. 
However, with the rise of AI-driven coding assistants, the platform's relevance has taken a hit. In this post, 
we'll explore the data behind Stack Overflow’s decline, comparing trends in question volume and shifts in popular tags 
after the introduction of ChatGPT and other LLMs.

Other articles have explored this trend as well, such as [this](https://devclass.com/2025/01/08/coding-help-on-stackoverflow-dives-as-ai-assistants-rise/){:target="_blank"}
analysis by DevClass () 
and [this](https://blog.pragmaticengineer.com/are-llms-making-stackoverflow-irrelevant/){:target="_blank"} post by Pragmatic Engineer. 
However, these discussions primarily focus on overall activity decline rather than specific changes in question tags. 
In this post, we will take a closer look at which topics have been most affected by the shift to AI-driven assistance.
Additionally, it is interesting to examine which topics have been the least affected by AI, highlighting areas where human assistance remains valuable.

The data for this analysis was obtained from the [StackExchange Data Explorer](https://data.stackexchange.com/stackoverflow/query/new){:target="_blank"}
which allows SQL queries against their databases.



## A Sharp Decline in Question Volume
The time series chart below illustrates a significant drop in question volume, particularly after November 2022, when 
ChatGPT was publicly released. Prior to this, Stack Overflow had already been experiencing a slow decline, but after 
ChatGPT’s introduction, the rate of decline became much more pronounced as developers increasingly turned to AI for quick, 
context-aware answers.

[![so_timeseries]({{site.url}}/assets/images/so_timeseries.png)]({{site.url}}/assets/images/so_timeseries.png){:target="_blank"}


## Which Topics Are Affected the Most?

To better understand the impact, we analyzed how the number of questions for the most popular tags in November 2022 
changed by November 2024. While some topics have declined more than others, which is to be expected, there is an interesting
pattern in these declines.

[![so_decline]({{site.url}}/assets/images/so_decline.png)]({{site.url}}/assets/images/so_decline.png){:target="_blank"}

What stands out is that questions related to fundamental programming concepts (such as lists, dictionaries, loops, strings, and functions) 
and data analysis (including pandas, dataframes, arrays, SQL, and NumPy) have experienced the most significant declines.

It is possible that AI-generated answers for the most impacted topics are of particularly high quality, making human assistance 
less necessary. Alternatively, developers working with these technologies may be more inclined to incorporate AI into their workflow, 
reducing their reliance on traditional Q&A platforms. 

Conversely, topics related to operating systems (Windows, Android, and iOS) and certain development frameworks, IDEs and cloud platforms 
(including Next.js, .NET, SwiftUI, React Native, Angular, Android Studio, Visual Studio Code, and Azure) have experienced comparatively smaller decreases in activity.
This could be due to the fact that questions in these areas frequently involve frontend and UI-related challenges or require visual 
explanations with screenshots, elements where AI-generated responses still struggle to provide adequate support.

A comprehensive interpretation of these results is left to readers with a deeper understanding of the listed technologies. 



**Found this content interesting or useful? Fuel future articles with a coffee!**
<a href="https://www.buymeacoffee.com/tomazweiss" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" alt="Buy Me A Coffee" style="height: 60px !important;width: 217px !important;" ></a>


## The Code

### Time series

Query:
``` sql
-- number of questions over time
select
  DATETRUNC(day, p.CreationDate) as day,
  count(*) as n_questions
from 
  Posts p
where 
  PostTypeId = 1 -- question
group by
  DATETRUNC(day, p.CreationDate)
order by 1;
```

Plot:
``` r
# renv::init()

library(tidyverse)
library(janitor)
library(ggplot2)
library(slider)


# data import -------------------------------------------------------------

data <- read_csv('data/counts_by_day.csv') |> clean_names()


# data preparation --------------------------------------------------------

data <- 
  data |> 
  mutate(day = as.Date(day)) |> 
  arrange(day) |> 
  mutate(rolling_avg = slider::slide_period_dbl(
    .x = n_questions,
    .i = day,
    .period = "day",
    .f = mean,
    .before = 27,
    .complete = FALSE
  ))


# plot --------------------------------------------------------------------

# arrow location
arrow_x <- as.Date("2022-11-30")
arrow_y <- data$rolling_avg[data$day == arrow_x]  # corresponding y value


fig <- 
  data |> 
  ggplot(aes(x = day, y = rolling_avg)) +
  geom_line(size = 1, color = 'steelblue') + 
  scale_x_date(date_breaks = "1 year", 
               date_labels = "%Y", 
               minor_breaks = NULL, 
               expand = expansion(add = c(30, 30))) +
  scale_y_continuous(breaks = seq(0, 7000, by = 1000), expand = expansion(add = c(5, 80))) +
  labs(title = "Number of Stack Overflow Questions over Time", 
       subtitle = 'Showing 28 day rolling average.',
       x = "Date", 
       y = "Number of Questions") +
  annotate("text", x = max(data$day), y = 6600, 
           label = "tomazweiss.github.io \nData Source: StackExchange Data Explorer", 
           hjust = 1, vjust = 0, size = 4) +
  theme_light() +
  annotate("segment", x = arrow_x + 100, y = arrow_y + 1000, xend = arrow_x + 0, yend = arrow_y + 150,
           arrow = arrow(type = "closed", length = unit(0.15, "inches")), color = "black") +
  annotate("text", x = arrow_x + 350, y = arrow_y + 1100, label = "ChatGPT launches",
           hjust = 1, size = 4, color = "black")

fig

ggsave('plots/so_timeseries.png', plot = fig, width = 400, height = 200, units = 'mm', dpi = 'retina')
```

### Percentage Decrease by Tag

Query:
``` sql
-- monthly number of questions by tag for most popular tags
with target_tags as
  (select
    TagName as tag_name,
    count(*) as cnt
  from 
    Posts p
    inner join
    PostTags pt on p.Id = pt.PostId
    inner join
    Tags t on pt.TagId = t.Id
  where PostTypeId = 1 -- question
  and DATETRUNC(mm, p.CreationDate) between '2022-01-01' and '2022-12-01'
  group by
    TagName
    having count(*) >= 7000
   )
select
  tt.tag_name,
  DATETRUNC(mm, p.CreationDate) as month,
  count(*) as n_questions
from 
  Posts p
  inner join
  PostTags pt on p.Id = pt.PostId
  inner join
  Tags t on pt.TagId = t.Id
  inner join 
  target_tags tt on t.TagName = tt.tag_name
where 
  PostTypeId = 1 -- question
  and DATETRUNC(mm, p.CreationDate) between '2022-11-01' and '2025-02-01'
group by
  tt.tag_name,
  DATETRUNC(mm, p.CreationDate)
order by 1, 2;
```

Plot:
``` r
library(tidyverse)
library(janitor)


# data import -------------------------------------------------------------

data <- read_csv('data/QueryResults_month.csv') |> clean_names()


# data preparation --------------------------------------------------------

data <- data |> mutate(month = ymd(month))

starting_values <- 
  data |> 
  filter(month == as.Date("2022-11-01")) 

ending_values <- 
  data |> 
  filter(month == as.Date("2024-11-01")) 

data_plot <- 
  starting_values |> 
  left_join(ending_values, by = 'tag_name') |> 
  mutate(n_questions_end = replace_na(n_questions_end, 0)) |> 
  mutate(ratio = n_questions_end / n_questions_start) |> 
  mutate(decline_pct = -100*(1 - ratio)) |> 
  arrange(decline_pct)

data_plot <- data_plot |> 
  mutate(tag_name = paste0(tag_name, ' (', n_questions_start, ')')) 

data_plot$tag_name <- factor(data_plot$tag_name, levels = data_plot$tag_name)


# plot --------------------------------------------------------------------

fig <- 
  data_plot |> 
  ggplot(aes(x = decline_pct, y = tag_name)) +
  geom_col(fill = "steelblue") +
  theme_light() +
  scale_x_continuous(
    labels = function(x) paste0(x, "%"), # Add % symbol to labels
    breaks = seq(-100, 0, by = 10),
    expand = expansion(add = c(0.5, 0.5)),
    sec.axis = sec_axis(~ ., 
                        breaks = seq(-100, 0, by = 10), 
                        labels =  function(x) paste0(x, "%")
    )    
  ) +
  labs(
    title = "Percentage Drop in Number of Stack Overflow Questions by their Tag",
    subtitle = paste0("Comparing November 2022 to November 2024.\n",
              "Showing 75 most popular tags of 2022. Numbers in brackets next to tag names represent number of questions in November 2022."),
    x = "Percentage Decrease",
    y = "Tag Name"
  ) +
  annotate("label", x = 0, y = 0, 
         label = "tomazweiss.github.io \nData Source: StackExchange Data Explorer", hjust = 1.03, vjust = -0.3, 
         size = 3.5, fill = "white", label.size = 0)

fig

ggsave('plots/so_decline.png', plot = fig, width = 300, height = 300, units = 'mm', dpi = 'retina')
```


## GitHub
[https://github.com/tomazweiss/stackoverflow_decline](https://github.com/tomazweiss/stackoverflow_decline){:target="_blank"}

