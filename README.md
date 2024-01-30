# ARAUS Soundscapes Affective Analysis

## Overview

This documentation presents an analysis of the ARAUS dataset, a large-scale dataset comprising 25,440 unique subjective perceptual responses to augmented urban soundscapes. Urban soundscapes are augmented through the addition of various "maskers" (like water or bird sounds). The dataset is publicly available and open to use under the Creative Commons Attribution-NonCommercial 4.0 International License (CC BY-NC 4.0). The ARAUS dataset is the work of Ooi et al., and full credit for the creation of this dataset goes to Kenneth Ooi, Zhen-Ting Ong, Karn N. Watcharasupat, Bhan Lam, Joo Young Hong, and Woon-Seng Gan. The dataset and its associated materials can be accessed through the Digital Repository of Nanyang Technological University (DR-NTU) at this link: [ARAUS: A Large-Scale Dataset and Baseline Models of Affective Responses to Augmented Urban Soundscapes](https://doi.org/10.21979/N9/9OTEVX).

The focus of this analysis is to explore how different psychoacoustic features of urban soundscapes influence affective states such as "pleasantness," "vibrance," and "eventfulness." Building upon the original dataset's comprehensive collection of perceptual responses, this analysis aims to identify key sound characteristics that contribute to restorative urban soundscapes, hypothesizing that certain psychoacoustic parameters as defined in [ISO/TS 12913-2:2018](https://www.iso.org/standard/75267.html) will evoke sensations of restoration (from Attention Restoration Theory) due to their high valence and arousal according to [Russell’s Circumplex Model of Emotions](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC2367156/#:~:text=The%20circumplex%20model%20of%20affect%20posits%20that%20the%20two%20underlying,salient%20situational%20and%20historical%20contexts).


## Methodology

1. **Data Gathering**
    - **Dataset Used:** Responses from the `responses.csv` file from the ARAUS dataset.
    - **Cleaning Process:** Removal of first, last, and attention stimulus soundscapes. Sorted by descending vibrant, pleasant, and eventful ratings using SQL.
    - **Analysis Tools:** Python with pandas for data manipulation and analysis.
    - **Key Parameters Analyzed:** Sharpness (S), Loudness (N), Fluctuation Strength (F), Roughness (R), and Tonality (T).
    - The cleaned and sorted dataset is available in `sorted_affective.csv`
2. **Analysis Approach:**
    - Use Python `pandas` and `SciPy` for data filtering and analysis.
    - Isolate soundscapes based on specific affective ratings into remarkable and comparison groups.
    - Calculate mean and standard deviation for psychoacoustic parameters.
    - Perform independent t-tests to assess statistical significance.

#### Grouping
In the ARAUS dataset, soundscapes are rated on a scale of 1 to 5 on eight affective features: {pleasant, eventful, chaotic, vibrant, uneventful, calm, annoying, monotonous}. In this analysis, we focused on the states of vibrance (high valence, high arousal), pleasantness (high valence, moderate arousal), and eventfulness (moderate valence, high arousal). A pleasant value of 1 was used as a comparison group to evaluate what psychoacoustic parameters result in "remarkable" (rating equal to 5) reports of pleasantness, vibrance, or eventfulness.

## Usage
The code is entirely documented and **modular**, so that groups, affective state filters, and psychoacoustic parameters can be easily adjusted in cell 3 dictionaries. 

#### Getting Started
1. Clone the repository from the [GitHub page](https://github.com/hectorastrom/ARAUS-T-Testing) using git
2. In a virtual environment, install the requirements from `requirements.txt` 

```pip freeze -r requirements.txt```

All functions and code live in `ARAUS T-testing.ipynb`
#### Using the Code
All functions are pre-documented in `ARAUS T-testing.ipynb`. Make sure to run all prior cells before trying to run a later one.

If you're trying to adjust which groups to compare, change the `remarkable_groups` and `comparison_groups` dictionaries in cell 3.

To compare specific groups and output to a new file, use the `verbose_compare_groups` function combined with the `select_group` function to specify individual groups. Below is an example of how to use these functions:

```python
# Open file to write comparison output to
with open(file_path, "w") as file:
    # Compare all combinations of groups individually
    for r_group in remarkable_group_data:
        for c_group in comparison_group_data:
            verbose_compare_groups(
                select_group(remarkable_group_data, r_group),
                select_group(comparison_group_data, c_group),
                columns_of_interest : dict,
                file,
                p_value
            )
```


## Results Summary

*Note that a complete analysis outputted by `verbose_compare_groups` in cell 6 can be found in `/t-test-outputs/total_comparisons.md`*

#### Comparison of High vs. Low Pleasantness Ratings

- **High Pleasantness (Rating 5) vs. Low Pleasantness (Rating 1)**
  - **Statistically Significant Differences:**
    - Lower average sharpness, loudness, fluctuation strength, roughness, and tonality in the high pleasantness group, suggesting a quieter and less harsh soundscape.
  - **Not Statistically Significant:**
    - Similar peak sharpness levels between high and low pleasantness ratings.

#### Comparison of High Eventfulness vs. Low Pleasantness Ratings

- **High Eventfulness (Rating 5) vs. Low Pleasantness (Rating 1)**
  - **Statistically Significant Differences:**
    - Higher average fluctuation strength and tonality in the high eventfulness group, indicating a more dynamic and tonally rich environment.
  - **Not Statistically Significant:**
    - Comparable levels of loudness and peak sharpness between high eventfulness and low pleasantness ratings.

#### Comparison of High Vibrance vs. Low Pleasantness Ratings

- **High Vibrance (Rating 5) vs. Low Pleasantness (Rating 1)**
  - **Statistically Significant Differences:**
    - Higher average tonality and lower average loudness in the high vibrance group, suggesting a tonally richer but quieter soundscape.
  - **Not Statistically Significant:**
    - Similar peak sharpness and roughness levels between high vibrance and low pleasantness ratings.

## Conclusions
To create restorative soundscapes:

1. **Enhance Tonality and Fluctuation Strength:** 
    - Key to increasing vibrancy and eventfulness.
2. **Moderate Loudness:** 
    - Lower average loudness correlates with more pleasant soundscapes.
3. **Balance Sharpness and Roughness:** 
    - Important for maintaining a pleasant acoustic environment.


## Future Steps
- Explore tonality of many remarkable soundscapes for dominant frequency range in spectrum analysis.
- Perform experiment to analyze restorativeness of remarkable soundscapes (with features summarized in previous section).
- Produce unconventional soundscapes emphasizing the psychoacoustic parameters found to be most restorative.
---

*Note: This README is part of an ongoing research project at the MIT Media Lab analyzing the characteristics of soundscapes that lead to attention restoration. It integrates findings from initial and subsequent analyses to provide a comprehensive view of the role of psychoacoustic features in shaping affective states in urban environments.*
