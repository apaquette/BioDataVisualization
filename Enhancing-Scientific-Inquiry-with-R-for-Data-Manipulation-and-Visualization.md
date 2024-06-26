Enhancing Scientific Inquiry with R for Data Manipulation and
Visualization
================
Alexandre Paquette
17th April, 2024

- [Introduction](#introduction)
  - [Understanding Scientific
    Research](#understanding-scientific-research)
  - [A Real-world Case Study](#a-real-world-case-study)
- [Research Question](#research-question)
  - [Rationale Behind the Study](#rationale-behind-the-study)
  - [Methodology](#methodology)
- [Data Manipulation](#data-manipulation)
  - [Data Cleanup](#data-cleanup)
  - [Derive New Data](#derive-new-data)
    - [Calculate average and median, and standard
      deviation](#calculate-average-and-median-and-standard-deviation)
- [Data Visualization with R](#data-visualization-with-r)
  - [Old Visualizations (using excel)](#old-visualizations-using-excel)
  - [Using Functions in R](#using-functions-in-r)
  - [Bar charts with R](#bar-charts-with-r)
  - [Scatter plots with R](#scatter-plots-with-r)
- [Conclusion](#conclusion)
- [Glossary of Terms](#glossary-of-terms)

# Introduction

Data visualization is a fundamental tool across a multitude of
disciplines, serving as a conduit for conveying complex information in a
visually accessible and palatable manner. In scientific research, where
the exploration and interpretation of data are paramount, effective
visualization techniques play a pivotal role in interpreting patterns,
trends, and relationships within datasets. By transforming raw data into
intuitive visual representations, researchers can gain deeper insights
into their findings and communicate them with clarity and precision. In
this report, we walk through the process of producing data
visualizations for scientific research, exploring the methodologies and
strategies that underline the creation of impactful visual narratives.

## Understanding Scientific Research

Scientific research constitutes a meticulous and systematic pursuit of
knowledge, aiming to present phenomena, develop theories, and solve
practical problems through rigorous methodologies via experimentation,
observation, analysis, and interpretation. Central to this endeavor is
the collection and processing of data, which serves as the cornerstone
for empirical investigation. However, the mere accumulation of data is
insufficient without an effective way to interpret and communicate. Data
visualization emerges as a vital tool in this process, enabling
researchers to distill complex datasets into intuitive visual
representations that facilitate hypothesis confirmation or refutation.
By harnessing the power of visualization, researchers can discern
patterns, trends, and relationships within data, thereby advancing our
understanding of the natural world and informing evidence-based
decision-making.

## A Real-world Case Study

The real-world case study at the heart of our visualization endeavor
revolves around an investigation into the potential benefits of
utilizing a bio-priming agent comprising *Bacillus cereus* and
*Pseudomonas alcaligenes* during the germination stage of plant
development. The overarching objective of this study is to assess
whether the application of this bio-priming agent contributes to the
overall health and vigor of plants upon completion of the germination
process.

The study aims to demonstrate the efficacy of the bio-priming agent in
enhancing plant growth, resilience, and productivity by leveraging the
natural symbiotic relationship between *Bacillus cereus* and
*Pseudomonas alcaligenes*. Through a series of controlled experiments
and rigorous data collection protocols, researchers seek to evaluate
various physiological parameters, including seed germination rates,
seedling vigor, root development, and resistance to environmental
stressors.

By employing a multidisciplinary approach that integrates microbiology,
plant biology, and agronomy, the study endeavors to shed light on the
potential applications of bio-priming techniques in sustainable
agriculture practices. The findings from this research hold promise for
informing agricultural strategies aimed at optimizing plant health and
productivity while minimizing reliance on synthetic inputs and
agrochemicals.

This real-world case study serves as the foundation for our exploration
of data visualization techniques, providing a concrete context for the
application of visual analytics in scientific research and innovation
within the agricultural domain.

# Research Question

The focal point of our investigation revolves around a central research
question: How do we effectively transform real-world data from
scientific studies into meaningful visual representations that
facilitate insight and understanding? This inquiry stems from the
imperative to distill scientific data into actionable insights,
particularly within the context of scientific research.

## Rationale Behind the Study

Our study aims to shed light on the process of taking real-world data
from a scientific study, manipulating it to extract meaningful insights,
and visualizing these insights for enhanced comprehension. Specifically,
our interest lies in exploring the relationship between the application
of a PGPB (Plant Growth Promoting Bacteria) consortium as a bio-priming
agent on the seeds of various plant species and the resulting health and
vigor of plants during the germination growth stage of the plant life.

Data visualization serves as a powerful tool in this endeavor, enabling
us to visually compare growth parameters and trends between experimental
bio-primed seeds and control non-exposed seeds. Through visual
representations, we seek to uncover patterns, trends, and correlations
within the growth data, providing valuable insights into the efficacy of
bio-priming with PGPB as a plant growth promoter.

## Methodology

Our methodology revolves around leveraging programming tools,
specifically R and relevant libraries, to manipulate and visualize the
data obtained from the scientific study. By harnessing the capabilities
of R programming and associated libraries, we aim to streamline the data
processing and visualization workflows, facilitating efficient analysis
and interpretation of the dataset.

The specific libraries and techniques employed will be determined based
on the nature of the data and the visualization requirements. However,
our methodology will prioritize clarity, accuracy, and reproducibility,
ensuring that the visualizations produced effectively communicate the
insights derived from the data.

This approach allows us to not only explore the intricacies of data
manipulation and visualization within the context of scientific research
but also lays the foundation for future research endeavors in
data-driven inquiry.

# Data Manipulation

In the pursuit of our research regarding the effective of real-world
data into insightful visual representation, the ‘Data Manipulation’
stage serves as a precursor to the visualization process. We will be
leveraging the capabilities of R programming and relevant libraries such
as dplyr, tidyr, and readr, in order to prepare our data for
visualization. We will load our raw data from a CSV file into a
variable, denoted as ‘bioData’, which will be our foundation for
subsequent analysis and visualizations. This step facilitates the
organization and structuring of our dataset, and enables the exploration
of key insights and trends within the data.

``` r
# for data manipulation:
library(dplyr)
library(tidyr)
library(readr)

# for visualization
library(ggplot2)
```

The following code loads our raw data into the bioData variable.

``` r
bioData <- read.csv("rawData.csv")
```

## Data Cleanup

In this section, we will refine and restructure our raw data to prepare
it for visualization and analysis.

Our first step involves address missing values by replacing all NA
values with 0, ensuring consistency and completeness in our dataset.

``` r
bioData[is.na(bioData)] <- 0
```

Subsequently, we will standardize the formatting of key columns by
converting values to lowercase and categorical data types (factor). This
facilitates uniformity and ease of analysis.

``` r
bioData$Plant.Type <- tolower(bioData$Plant.Type)
bioData$Control.Experimental <- tolower(bioData$Control.Experimental)
bioData$Common.Name <- tolower(bioData$Common.Name)
bioData$Scientific.Name <- tolower(bioData$Scientific.Name)
bioData$Group <- factor(bioData$Group)
bioData$Plant.Type <- factor(bioData$Plant.Type)
bioData$Control.Experimental <- factor(bioData$Control.Experimental)
```

Additionally, we will rename our columns to correct spelling mistakes
and maintain a consistent format across our dataset.

``` r
bioData <- bioData %>%
  rename(Common_Name = Common.Name,
         Scientific_Name = Scientific.Name,
         Plant_Type = Plant.Type,
         Control_Experimental = Control.Experimental,
         Root_Count = number.of.roots.over.all,
         Sprout_Count = number.of.sprouts.over.all,
         Sprout_Week_1_Change_cm = Change.in.week.1..sprout.,
         Sprout_Week_2_Change_cm = change.in.week.2..sprout.,
         Sprout_Week_3_Change_cm = change.in.week.3..sprout.,
         Sprout_Week_4_Change_cm = change.in.week.4..sprout.,
         Seed_Amount = seed.amount,
         Seed_Amount_Variation = seed.amount.variation..,
         Root_Week_1_Change_cm = Change.in.week.1..root.,
         Root_Week_2_Change_cm = change.in.week.2..root.,
         Root_Week_3_Change_cm = change.in.week.3..root.,
         Root_Week_4_Change_cm = change.in.week.4..root.,
         Root_Lengths_cm = Indavidual.lengrths..roots.,
         Sprout_Lengths_cm = Indavidual.langth..sprouts.,
         Healthy_Seed_Count = healthy.seed.count
         )
```

To further streamline the dataset, we remove entries that did not
exhibit growth for both sprouts and roots, enhancing the relevance and
accuracy of our analysis.

``` r
bioData <- bioData %>% filter(Healthy_Seed_Count != 0)
```

We consolidate matching plant entries entries by control/experimental
groups, simplifying the dataset while retaining essential information.

``` r
# remove leading and trailing commas
remove_commas <- function(x) {
  gsub("^,|,$", "", x)
}

bioData <- bioData %>%
  group_by(Scientific_Name, Control_Experimental) %>%
  summarize(
    Common_Name = first(Common_Name),
    Root_Lengths_cm = remove_commas(paste(Root_Lengths_cm, collapse = ", ")),
    Plant_Type = first(Plant_Type),
    Root_Count = sum(Root_Count),
    Sprout_Count = sum(Sprout_Count),
    Sprout_Week_1_Change_cm = mean(Sprout_Week_1_Change_cm),
    Sprout_Week_2_Change_cm = mean(Sprout_Week_2_Change_cm),
    Sprout_Week_3_Change_cm = mean(Sprout_Week_3_Change_cm),
    Sprout_Week_4_Change_cm = mean(Sprout_Week_4_Change_cm),
    Seed_Amount = sum(Seed_Amount),
    Seed_Amount_Variation = first(Seed_Amount_Variation),
    Root_Week_1_Change_cm = mean(Root_Week_1_Change_cm),
    Root_Week_2_Change_cm = mean(Root_Week_2_Change_cm),
    Root_Week_3_Change_cm = mean(Root_Week_3_Change_cm),
    Root_Week_4_Change_cm = mean(Root_Week_4_Change_cm),
    Sprout_Lengths_cm = remove_commas(paste(Sprout_Lengths_cm, collapse = ", ")),
    Healthy_Seed_Count = sum(Healthy_Seed_Count)
  ) %>%
  ungroup()
```

We generate unique identifiers for each entry in the dataset,
facilitating tracking of individual data points.

``` r
bioData <- bioData %>% mutate(id = row_number())
bioData <- bioData %>% select(id, everything())
```

Furthermore, we extract the multiple values from the ‘Root Lengths’ and
‘Sprout Lengths’ columns, allowing them to be treated as individual
numeric values for future visualization. By converting these values to
numeric format, we ensure consistency and accuracy in our dataset.

``` r
rootLengthsData <- bioData %>% 
  separate_rows(Root_Lengths_cm, sep = ",") %>% 
  select(id, Root_Length_cm = Root_Lengths_cm)

rootLengthsData <- rootLengthsData %>% 
  mutate(Root_Length_cm = as.numeric(Root_Length_cm))
```

``` r
sproutLengthsData <- bioData %>%
  separate_rows(Sprout_Lengths_cm, sep = ",") %>%
  select(id, Sprout_Length_cm = Sprout_Lengths_cm) %>%
  mutate(Sprout_Length_cm = as.numeric(Sprout_Length_cm))

sproutLengthsData <- sproutLengthsData[complete.cases(sproutLengthsData),]

bioData <- bioData %>% select(-Root_Lengths_cm, -Sprout_Lengths_cm)
```

The extracted root and sprout lengths for each plant are retained in
their raw form to preserve the integrity of the data for further
investigation.

Additionally, we extract the weekly change readings for both root and
sprout growth, consolidating them into separate dataset for analysis.

``` r
weeklyRootGrowths <- gather(
                      select(bioData, 
                            id, Scientific_Name, Control_Experimental, Common_Name, Plant_Type,
                            "1" = Root_Week_1_Change_cm, 
                            "2" = Root_Week_2_Change_cm, 
                            "3" = Root_Week_3_Change_cm, 
                            "4" = Root_Week_4_Change_cm), 
                      key = "week", 
                      value = "measurement_cm",
                      -id, -Scientific_Name, -Control_Experimental, -Common_Name, -Plant_Type)
weeklyRootGrowths <- weeklyRootGrowths %>% mutate(week = as.numeric(week))
```

``` r
weeklySproutGrowths <- gather(
                      select(bioData, 
                            id, Scientific_Name, Control_Experimental, Common_Name, Plant_Type,
                            "1" = Sprout_Week_1_Change_cm, 
                            "2" = Sprout_Week_2_Change_cm, 
                            "3" = Sprout_Week_3_Change_cm, 
                            "4" = Sprout_Week_4_Change_cm), 
                      key = "week", 
                      value = "measurement_cm",
                      -id, -Scientific_Name, -Control_Experimental, -Common_Name, -Plant_Type)
weeklySproutGrowths <- weeklySproutGrowths %>% 
  mutate(week = as.numeric(week))
```

These data cleanup steps culminate in the removal of redundant columns
from our main dataset.

``` r
bioData <- bioData %>% select(-Root_Week_1_Change_cm, -Root_Week_2_Change_cm, -Root_Week_3_Change_cm, -Root_Week_4_Change_cm, -Sprout_Week_1_Change_cm, -Sprout_Week_2_Change_cm, -Sprout_Week_3_Change_cm, -Sprout_Week_4_Change_cm)
```

## Derive New Data

In this stage, we will go through the process of extracting new
information from our dataset by deriving metrics and indicators. To
enhance our understanding of the effect of bio-priming on plant growth,
we can augment our original dataset with additional columns to calculate
key measures such as average and median root and stem lengths.
Furthermore, we compute the average growth over time for both root and
sprout dimensions. By integrating these derived metrics into our
dataset, we enable deeper exploration and interpretation of the data.

### Calculate average and median, and standard deviation

In this section, we introduce a versatile method designed to compute
summary statistics on a given dataset and append the results to the main
dataset. The function `calculate_summar` takes as input the main dataset
(`bioData`) and a summary dataset (`rootLengthsData` or
`sproutLengthsData`) depending on the specific calculation required.

``` r
calculate_summary <- function(main_data, summary_data, id_col, summary_col, summary_name, summary_method){
  summary_values <- summary_data %>%
    group_by({{id_col}}) %>%
    summarize(
      {{summary_name}} := summary_method({{summary_col}})
    )
  
  result <- left_join(main_data, summary_values, by = join_by({{id_col}}))
  return(result)
}
```

With this method, we can compute key metrics such as average, median,
and standard deviation for both root and sprout lengths. The function
`calculate_summary` facilitates efficient aggregation and integration of
summary statistics into our main dataset.

``` r
bioData <- calculate_summary(bioData, rootLengthsData, id, Root_Length_cm, avg_root_length_cm, mean) # average root length
bioData <- mutate(bioData, avg_root_length_cm = avg_root_length_cm / Healthy_Seed_Count)
bioData <- calculate_summary(bioData, sproutLengthsData, id, Sprout_Length_cm, avg_sprout_length_cm, mean) # average sprout length
bioData <- mutate(bioData, avg_sprout_length_cm = avg_sprout_length_cm / Healthy_Seed_Count)
bioData <- calculate_summary(bioData, rootLengthsData, id, Root_Length_cm, median_root_length_cm, median) # Median sprout length
bioData <- calculate_summary(bioData, sproutLengthsData, id, Sprout_Length_cm, median_sprout_length_cm, median) # Median sprout length
bioData <- calculate_summary(bioData, rootLengthsData, id, Root_Length_cm, root_length_sd, sd) # root length standard deviation
bioData <- mutate(bioData, root_length_sd = root_length_sd / Healthy_Seed_Count)
bioData <- calculate_summary(bioData, sproutLengthsData, id, Sprout_Length_cm, sprout_length_sd, sd) # sprout length standard deviation
bioData <- mutate(bioData, sprout_length_sd = sprout_length_sd / Healthy_Seed_Count)
```

# Data Visualization with R

Using R programming, researchers can unlock new insights, uncover hidden
patterns, and communicate their findings with clarity and precision. In
this section, we explore how to perform data visualization with R, using
the `ggplot2` library. We explore various techniques and methodologies
for visualizing our data by leveraging the rich functionality and
flexibility offered by R. We will examine visualizations created in
Excel and identify limitations that can be overcome by using R and
`ggplot2`.

## Old Visualizations (using excel)

Excel has long been a popular choice for its accessibility and
user-friendly interface. However, it falls short when confronted with
the complexity and nuances inherent to scientific data. To illustrate
this, we present a visualization produced using Excel below:

![](Excel_vis/image003.png)

The data displayed in this visualization is difficult to decipher, and
little information is gleamed from the data. The improvements `ggplot2`
in R to our visualizations will become apparent in our later sections.

## Using Functions in R

Encapsulating common visualization tasks into reusable functions enables
researchers to efficiently generate a wide range of visualizations
tailed to their specific needs. The power and versatility of utilizing
custom functions in R enable us to streamline the process of
visualization.

We introduce three distinct functions designed to facilitate the
production of visualizations: `create_bar_plot`, `create_scatter_plot`,
and `get_r_squared`. These functions serve as useful tools for
automating the generation of bar charts, scatter plots, and calculating
the R-square value of a line of best fit. These functions enable
researchers to expedite their data analysis pipeline, enhance
reproducibility, and gain deeper insights into their datasets.

``` r
create_bar_plot <- function(data, x_val, y_val, fill_val, sd = NULL, titleName, subtitleName = "", x_title, y_title, legend = TRUE, hideXAxisText = FALSE,...){
  colors <- c("control" = "#7cb5ec", "experimental" = "#f7a35c")
  
  chart <- ggplot(data, aes(x = {{x_val}}, y = {{y_val}}, fill = {{fill_val}})) +
          geom_bar(stat = "identity", position = position_dodge(width = 0.8), width = 0.6) +
            scale_fill_manual(values = colors, name = "") +
            labs(title = titleName, subtitle = subtitleName, x = x_title, y = y_title) +
            theme(axis.text.x = element_text(angle = 45, hjust = 1),
                  plot.title = element_text(size = 20),
                  plot.margin = margin(30, 30, 30, 30, "pt"),
                  plot.background = element_rect(fill = "white"),      # Set plot background color
                  panel.background = element_rect(fill = "white"))  # Rotate x-axis labels for better readability
  
  if(!missing(sd)){
    sd_filter <- !is.null(data[[deparse(substitute(sd))]]) & data[[deparse(substitute(sd))]] > 0
    chart <- chart + geom_errorbar(aes(ymin = pmax({{y_val}} - {{sd}}, 0), 
                                        ymax = {{y_val}} + {{sd}}), 
                                        width = 0.4, 
                                        position = position_dodge(width = 0.8), na.rm = TRUE)
  }
  
  if(!legend){
    chart <- chart + guides(fill = FALSE)
  }
  
  if(hideXAxisText){
    chart <- chart + theme(axis.text.x = element_blank()) + labs(x = "")
  }
  
  fileName <- paste("charts/",titleName,subtitleName,".png")
  suppressMessages(ggsave(fileName, chart))
  return(chart)
}

create_scatter_plot <- function(data, x_val, y_val, fill_val, line_of_best_fit = FALSE, growth_curve = FALSE, titleName, subtitleName = "", x_title, y_title, legend = TRUE, hideXAxisText = FALSE,...){
  colors <- c("experimental" = "#7cb5ec", "control" = "#f7a35c")
  
  fileName <- paste("charts/",titleName,subtitleName)
  
  chart <- ggplot(data, aes(x = {{x_val}}, y = {{y_val}}, color = {{fill_val}})) + 
    geom_point() +
    scale_color_manual(values = colors, name = "") +
    labs(title = titleName, subtitle = subtitleName, x = x_title, y = y_title) +
    theme(plot.title = element_text(size = 20),
          plot.margin = margin(30, 30, 30, 30, "pt"),
          plot.background = element_rect(fill = "white"),      # Set plot background color
          panel.background = element_rect(fill = "white"))  # Rotate x-axis labels for better

  if(line_of_best_fit){
    chart <- chart + 
      geom_smooth(method = "lm", 
                  formula = y ~ x, 
                  se = TRUE,
                  aes(group = Control_Experimental), 
                  stat = "smooth", 
                  fullrange = TRUE)
    fileName <- paste(fileName, "_lobf")
  }

  if(growth_curve){
    chart <- chart + 
        geom_smooth(method = "loess", 
                    formula = y ~ x, 
                    se = FALSE,
                    aes(group = Control_Experimental), 
                    stat = "smooth", 
                    fullrange = TRUE)
    fileName <- paste(fileName, "_gc")
  }
  fileName <- paste(fileName, ".png")
  suppressMessages(ggsave(fileName, chart))

  return(chart)
}

get_r_squared <- function(data, grouping, value, category){
  r_squared <- data %>% 
    group_by(!!sym(grouping)) %>% 
    summarise(rsquared = summary(lm(as.formula(paste0(value, " ~ ", category))))$r.squared)
  return(r_squared)
}
```

## Bar charts with R

Using bar charts with our dataset enables us to gain a comprehensive
understanding of the relationship between bio-priming treatment and
plant growth statistics.

We begin by extracting data specifically for root and sprout lengths
from our main dataset, `bioData`, creating separate datasets named
`bioDataRoot` and `bioDataSprout`.

``` r
bioDataRoot <- bioData %>%
              group_by(Scientific_Name) %>%
              filter(n() == 2) %>%
              filter(all(avg_root_length_cm != 0))

bioDataSprout <- bioData %>%
              group_by(Scientific_Name) %>%
              filter(n() == 2) %>%
              filter(all(avg_sprout_length_cm != 0))
```

Using our create_bar_plot function from earlier, we compare the average
root and sprout lengths between control and experimental samples.

``` r
create_bar_plot(data = bioDataRoot,
                x_val = Scientific_Name,
                y_val = avg_root_length_cm,
                fill_val = Control_Experimental,
                sd = root_length_sd,
                titleName = "Average Root Length",
                x_title = "Plant Species",
                y_title = "Avg Length (cm)")
```

![](Enhancing-Scientific-Inquiry-with-R-for-Data-Manipulation-and-Visualization_files/figure-gfm/unnamed-chunk-19-1.png)<!-- -->

We also visualize these comparisons separately for monocot and dicot
plants.

``` r
create_bar_plot(data = bioDataSprout,
                x_val = Scientific_Name,
                y_val = avg_sprout_length_cm,
                fill_val = Control_Experimental,
                sd = sprout_length_sd,
                titleName = "Average Sprout Length",
                x_title = "Plant Species",
                y_title = "Avg Length (cm)")
```

![](Enhancing-Scientific-Inquiry-with-R-for-Data-Manipulation-and-Visualization_files/figure-gfm/unnamed-chunk-20-1.png)<!-- -->

``` r
create_bar_plot(data = subset(bioDataRoot, Plant_Type == "monocot"),
                x_val = Scientific_Name,
                y_val = avg_root_length_cm,
                fill_val = Control_Experimental,
                sd = root_length_sd,
                titleName = "Average Root Length",
                subtitleName = "for monocot plants",
                x_title = "Monocot Plant Species",
                y_title = "Avg Length (cm)")
```

![](Enhancing-Scientific-Inquiry-with-R-for-Data-Manipulation-and-Visualization_files/figure-gfm/unnamed-chunk-21-1.png)<!-- -->

``` r
create_bar_plot(data = subset(bioDataRoot, Plant_Type == "dicot"),
                x_val = Scientific_Name,
                y_val = avg_root_length_cm,
                fill_val = Control_Experimental,
                sd = root_length_sd,
                titleName = "Average Root Length",
                subtitleName = "for dicot plants",
                x_title = "Dicot Plant Species",
                y_title = "Avg Length (cm)")
```

![](Enhancing-Scientific-Inquiry-with-R-for-Data-Manipulation-and-Visualization_files/figure-gfm/unnamed-chunk-22-1.png)<!-- -->

``` r
create_bar_plot(data = subset(bioDataSprout, Plant_Type == "monocot"),
                x_val = Scientific_Name,
                y_val = avg_sprout_length_cm,
                fill_val = Control_Experimental,
                sd = sprout_length_sd,
                titleName = "Average Sprout Length ",
                subtitleName = "for monocot plants",
                x_title = "Monocot Plant Species",
                y_title = "Avg Length (cm)")
```

![](Enhancing-Scientific-Inquiry-with-R-for-Data-Manipulation-and-Visualization_files/figure-gfm/unnamed-chunk-23-1.png)<!-- -->

``` r
create_bar_plot(data = subset(bioDataSprout, Plant_Type == "dicot"),
                x_val = Scientific_Name,
                y_val = avg_sprout_length_cm,
                fill_val = Control_Experimental,
                sd = sprout_length_sd,
                titleName = "Average Sprout Length",
                subtitleName = "for dicot plants",
                x_title = "Dicot Plant Species",
                y_title = "Avg Length (cm)")
```

![](Enhancing-Scientific-Inquiry-with-R-for-Data-Manipulation-and-Visualization_files/figure-gfm/unnamed-chunk-24-1.png)<!-- -->

We can further demonstrate the efficiency of R by automating the
visualization process for each plant in our dataset, producing nearly 30
visualizations with minimal code.

``` r
unique_plant_names <- unique(bioDataRoot$Scientific_Name)
# Iterate through each unique plant name
for(plant_name in unique_plant_names){
  # Generate a chart for the current plant
  create_bar_plot(data = filter(bioDataRoot, Scientific_Name == plant_name),
                  x_val = Scientific_Name,
                  y_val = avg_root_length_cm,
                  fill_val = Control_Experimental,
                  sd = root_length_sd,
                  titleName = "Average Root Length",
                  subtitleName = paste("for ", plant_name),
                  x_title = "Plant Species",
                  y_title = "Avg Length (cm)",
                  hideXAxisText = TRUE)
}
```

``` r
unique_plant_names <- unique(bioDataSprout$Scientific_Name)
# Iterate through each unique plant name
for(plant_name in unique_plant_names){
  # Generate a chart for the current plant
  create_bar_plot(data = filter(bioDataSprout, Scientific_Name == plant_name),
                  x_val = Scientific_Name,
                  y_val = avg_sprout_length_cm,
                  fill_val = Control_Experimental,
                  sd = sprout_length_sd,
                  titleName = "Average Sprout Length",
                  subtitleName = paste("for ", plant_name),
                  x_title = "Plant Species",
                  y_title = "Avg Length (cm)",
                  hideXAxisText = TRUE)
}
```

Finally, we illustrate the distribution of plants with higher average
root and sprout lengths between control and experimental conditions,
providing insights into the effectiveness of the experimental treatment.

``` r
higher_length <- bioDataRoot %>%
  group_by(Scientific_Name) %>% 
  summarise(
    higher_length_control = sum(avg_root_length_cm[Control_Experimental == "control"] > avg_root_length_cm[Control_Experimental == "experimental"]),
    higher_length_experimental = sum(avg_root_length_cm[Control_Experimental == "experimental"] > avg_root_length_cm[Control_Experimental == "control"])
  ) %>%
  summarise(
    higher_length_control = sum(higher_length_control),
    higher_length_experimental = sum(higher_length_experimental)
  ) %>%
  pivot_longer(cols = starts_with("higher_length"), names_to = "Category", values_to = "total_count") %>%
  mutate(Category = gsub("higher_length_", "",Category))

create_bar_plot(data = higher_length,
                x_val = Category,
                y_val = total_count,
                fill_val = Category,
                titleName = "Higher Average Root Length",
                subtitleName = "per Condition",
                x_title = "Plant Species",
                y_title = "Number of plants",
                hideXAxisText = TRUE)
```

![](Enhancing-Scientific-Inquiry-with-R-for-Data-Manipulation-and-Visualization_files/figure-gfm/unnamed-chunk-27-1.png)<!-- -->

``` r
higher_length <- bioDataSprout %>%
  group_by(Scientific_Name) %>% 
  summarise(
    higher_length_control = sum(avg_sprout_length_cm[Control_Experimental == "experimental"] > avg_sprout_length_cm[Control_Experimental == "control"]),
    higher_length_experimental = sum(avg_sprout_length_cm[Control_Experimental == "control"] > avg_sprout_length_cm[Control_Experimental == "experimental"])
  ) %>%
  summarise(
    higher_length_control = sum(higher_length_control),
    higher_length_experimental = sum(higher_length_experimental)
  ) %>%
  pivot_longer(cols = starts_with("higher_length"), names_to = "Category", values_to = "total_count") %>%
  mutate(Category = gsub("higher_length_", "",Category)) %>%
  mutate (sd = 0)

create_bar_plot(data = higher_length,
                x_val = Category,
                y_val = total_count,
                fill_val = Category,
                titleName = "Higher Average Sprout Length",
                subtitleName = "per Condition",
                x_title = "Plant Species",
                y_title = "Number of plants",
                hideXAxisText = TRUE)
```

![](Enhancing-Scientific-Inquiry-with-R-for-Data-Manipulation-and-Visualization_files/figure-gfm/unnamed-chunk-28-1.png)<!-- -->

## Scatter plots with R

Using scatter plots for visualizations gives us insight into how the
bio-priming treatment affects the growth patterns of different plant
species. It allows us to visualize the growth of roots and sprouts over
a period of 4 weeks.

First, we ensure the integrity of our data by remove any entries that
lack corresponding control data. In this case, we know that the plant
*antirrhinum majus* lacks such data, so it will be removed from our
dataset.

``` r
weeklyRootGrowths <- weeklyRootGrowths %>% filter(Scientific_Name != "antirrhinum majus")
weeklySproutGrowths <- weeklySproutGrowths %>% filter(Scientific_Name != "antirrhinum majus")
```

Next, we employ the `create_scatter_plot` function to generate the
visualization to compare the average growth of roots and sprouts between
control and experimental over four weeks. Additionally, we computer the
R-squared value to assess how good our line of best fit matches the
data. Two versions of each visualization are produced: one with a line
of best fit, and one with a growth curve.

``` r
create_scatter_plot(data = weeklyRootGrowths, 
                    x_val = week, 
                    y_val = measurement_cm, 
                    fill_val = Control_Experimental,
                    line_of_best_fit = TRUE,
                    titleName = "Weekly Root Growth",
                    x_title = "Week", 
                    y_title = "Measurments (cm)")
```

![](Enhancing-Scientific-Inquiry-with-R-for-Data-Manipulation-and-Visualization_files/figure-gfm/unnamed-chunk-30-1.png)<!-- -->

``` r
create_scatter_plot(data = weeklyRootGrowths, 
                    x_val = week, 
                    y_val = measurement_cm, 
                    fill_val = Control_Experimental,
                    growth_curve = TRUE,
                    titleName = "Weekly Root Growth",
                    x_title = "Week", 
                    y_title = "Measurments (cm)")
```

![](Enhancing-Scientific-Inquiry-with-R-for-Data-Manipulation-and-Visualization_files/figure-gfm/unnamed-chunk-30-2.png)<!-- -->

``` r
rsquared_data <- get_r_squared(data = weeklyRootGrowths, grouping = "Control_Experimental", value = "measurement_cm", category = "week") %>% 
                  mutate(Plant = "All Plants", Plant_Section = "root")
```

``` r
create_scatter_plot(data = weeklySproutGrowths, 
                    x_val = week, 
                    y_val = measurement_cm, 
                    fill_val = Control_Experimental,
                    line_of_best_fit = TRUE,
                    titleName = "Weekly Sprout Growth", 
                    x_title = "Week", 
                    y_title = "Measurments (cm)")
```

![](Enhancing-Scientific-Inquiry-with-R-for-Data-Manipulation-and-Visualization_files/figure-gfm/unnamed-chunk-31-1.png)<!-- -->

``` r
create_scatter_plot(data = weeklySproutGrowths, 
                    x_val = week, 
                    y_val = measurement_cm, 
                    fill_val = Control_Experimental,
                    growth_curve = TRUE,
                    titleName = "Weekly Sprout Growth", 
                    x_title = "Week", 
                    y_title = "Measurments (cm)")
```

![](Enhancing-Scientific-Inquiry-with-R-for-Data-Manipulation-and-Visualization_files/figure-gfm/unnamed-chunk-31-2.png)<!-- -->

``` r
rsquared_data <- bind_rows(rsquared_data, 
                           get_r_squared(data = weeklySproutGrowths, grouping = "Control_Experimental", value = "measurement_cm", category = "week") %>%
                             mutate(Plant = "All Plants", Plant_Section = "sprout")
                           )
```

We also extend our analysis to individual plants by iterating through
the dataset and creating scatter plots for each plant’s weekly growth.
This iterative approach enables us to tailor visualizations to the
characteristics of each plant.

``` r
unique_plant_names <- unique(weeklyRootGrowths$Scientific_Name)
# Iterate through each unique plant name
for(plant_name in unique_plant_names){
  # Generate a chart for the current plant
  create_scatter_plot(data = filter(weeklyRootGrowths, Scientific_Name == plant_name),
                      x_val = week,
                      y_val = measurement_cm,
                      fill_val = Control_Experimental,
                      line_of_best_fit = TRUE,
                      titleName = "Weekly Root Growth",
                      subtitleName = paste("for ", plant_name),
                      x_title = "Week",
                      y_title = "Measurments (cm)")
  
  create_scatter_plot(data = filter(weeklyRootGrowths, Scientific_Name == plant_name),
                      x_val = week,
                      y_val = measurement_cm,
                      fill_val = Control_Experimental,
                      growth_curve = TRUE,
                      titleName = "Weekly Root Growth",
                      subtitleName = paste("for ", plant_name),
                      x_title = "Week",
                      y_title = "Measurments (cm)")
  rsquared_data <- bind_rows(rsquared_data, 
                           get_r_squared(data = filter(weeklyRootGrowths, Scientific_Name == plant_name), 
                                         grouping = "Control_Experimental", 
                                         value = "measurement_cm", category = "week") %>%
                             mutate(Plant = plant_name, Plant_Section = "root")
                           )
}
```

``` r
unique_plant_names <- unique(weeklySproutGrowths$Scientific_Name)
# Iterate through each unique plant name
for(plant_name in unique_plant_names){
  # Generate a chart for the current plant
  create_scatter_plot(data = filter(weeklySproutGrowths, Scientific_Name == plant_name),
                      x_val = week,
                      y_val = measurement_cm,
                      fill_val = Control_Experimental,
                      line_of_best_fit = TRUE,
                      titleName = "Weekly Sprout Growth",
                      subtitleName = paste("for ", plant_name),
                      x_title = "Week",
                      y_title = "Measurments (cm)")
  
  create_scatter_plot(data = filter(weeklySproutGrowths, Scientific_Name == plant_name),
                      x_val = week,
                      y_val = measurement_cm,
                      fill_val = Control_Experimental,
                      growth_curve = TRUE,
                      titleName = "Weekly Sprout Growth",
                      subtitleName = paste("for ", plant_name),
                      x_title = "Week",
                      y_title = "Measurments (cm)")
    rsquared_data <- bind_rows(rsquared_data, 
                           get_r_squared(data = filter(weeklySproutGrowths, Scientific_Name == plant_name), 
                                         grouping = "Control_Experimental", 
                                         value = "measurement_cm", category = "week") %>%
                             mutate(Plant = plant_name, Plant_Section = "sprout")
                           )
}
```

Finally, we output the computed R-squared values and export them to a
CSV file for future reference and analysis.

``` r
write_csv(rsquared_data, "rsquared_data.csv") # save our rsquared values into a .csv for later use
```

# Conclusion

Our report underscores the pivotal role of R in facilitating the
creation of clean, reproducible, and high-quality visualizations. These
visualizations are essential for gaining insights from experimental
data. Leveraging R’s robust programming capabilities enables us to
generate a multitude of visualizations, shedding light on the effects of
bio-priming treatment on plant growth across various species.
Additionally, by crafting reusable code, we were able to expedite the
analysis process, and establish a framework that can be applied to
future research endeavors.

Despite the strengths, our analysis revealed a notable drawback: the
significant amount of time invested in data cleaning. The issue stemmed
from the data not being initially formatted for our specific analytical
needs, outlining the importance of data preparation in experimental
design. To address this challenge in future studies, it is imperative to
collaborate closely with data producers to ensure the datasets are
structured in a manner conducive to analysis.

Looking ahead, several suggestions can be made to enhance the efficiency
and effectiveness of our analytical approach. Firstly, implementing data
validation protocols during data collection can mitigate the need for
extensive cleaning and pre-processing. Additionally, investing in
training programs to enhance researchers’ proficiency in R programming
and statistical analysis can foster a more seamless integration of
computational tools into research workflows. Furthermore, exploring the
use of machine learning algorithms for automated data cleaning and
feature extraction may offer opportunities to streamline the analysis
process further.

# Glossary of Terms

Bio-priming  
The process of coating the seed with a plant-growth promoting bacteria
consortium comprised of Basillus ceres and pusdomonas

Monocot plant  
The seeds of these plants typically contain a single embryonic leaf

Dicot plant  
A plant whose germinating seed contain two embryonic leaves

Embryonic leaf  
The plant embryo, also known as cotyledon
