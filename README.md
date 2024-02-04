# ARAUS Soundscapes Affective Analysis

## Overview

This documentation presents an analysis of the [ARAUS dataset](https://arxiv.org/pdf/2207.01078.pdf), a large-scale dataset comprising 25,440 unique subjective perceptual responses from 749 participants to augmented urban soundscapes. Urban soundscapes are augmented through the addition of various "maskers" (like water or bird sounds). The dataset is publicly available and open to use under the Creative Commons Attribution-NonCommercial 4.0 International License (CC BY-NC 4.0). Full credit for the creation of the ARAUS dataset is attributed to Kenneth Ooi, Zhen-Ting Ong, Karn N. Watcharasupat, Bhan Lam, Joo Young Hong, and Woon-Seng Gan. The dataset and its associated materials can be accessed through the Digital Repository of Nanyang Technological University (DR-NTU) at this link: [ARAUS: A Large-Scale Dataset and Baseline Models of Affective Responses to Augmented Urban Soundscapes](https://doi.org/10.21979/N9/9OTEVX). Further, the GitHub documenting their research code can be accessed [here](https://github.com/ntudsp/araus-dataset-baseline-models).

The focus of this analysis is to explore how different psychoacoustic features of urban soundscapes influence affective states such as "pleasantness," "vibrance," and "eventfulness." Building upon the original dataset's comprehensive collection of perceptual responses, this analysis aims to identify key sound characteristics that contribute to restorative urban soundscapes, hypothesizing that certain psychoacoustic parameters as defined in [ISO/TS 12913-2:2018](https://www.iso.org/standard/75267.html) will evoke sensations of restoration (from Attention Restoration Theory) due to their high valence and arousal according to [Russell’s Circumplex Model of Emotions](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC2367156/#:~:text=The%20circumplex%20model%20of%20affect%20posits%20that%20the%20two%20underlying,salient%20situational%20and%20historical%20contexts).


## Methodology

1. **Data Gathering**
    - **Dataset Used:** Responses from the `responses.csv` file from the ARAUS dataset.
    - **Cleaning Process:** Removal of first and last (consistency stimuli) and attention stimulus soundscapes (as recommended by ARAUS creators for data analysis). Sorted by descending vibrant, pleasant, and eventful ratings using SQL.
    - **Analysis Tools:** Python with pandas for data manipulation and analysis.
    - **Key Parameters Analyzed:** Sharpness (S), Loudness (N), Fluctuation Strength (F), Roughness (R), and Tonality (T).
    - The lightly cleaned and sorted dataset is available in `datasets/ARAUS_precleaned.csv`
2. **Analysis Approach:**
    - Use Python `pandas` and `SciPy` for data filtering and analysis.
    - Isolate soundscapes based on specific affective ratings into remarkable and comparison groups.
    - Calculate mean and standard deviation for psychoacoustic parameters.
    - Perform independent t-tests to assess statistical significance.

#### Grouping
In the ARAUS dataset, soundscapes are rated on a scale of 1 to 5 on eight affective features: {pleasant, eventful, chaotic, vibrant, uneventful, calm, annoying, monotonous}. In this analysis, we focused on the states of vibrance (high valence, high arousal), pleasantness (high valence, moderate arousal), and eventfulness (moderate valence, high arousal). Low values of these affective ratings (through bottom percentiles) were used as comparison groups to evaluate what psychoacoustic parameters result in remarkable (top percentile ratings) reports of pleasantness, vibrance, or eventfulness. 

The data for these comparisons is accessible in this repository. However, the groups can be quickly adjusted to perform analysis and comparison between other groups, such as a comparison between groups of multiple affective features and other percentile ranges.

## Directory Structure
```
├── datasets/                                           # Location of all dataframes used in analysis.
│   ├── ARAUS_precleaned.csv                                  # Original (slightly cleaned) file with all columns.
│   ├── ARAUS_relevant.csv                                    # File with only columns used for statistical analysis.
│   ├── ARAUS_merged.csv                                      # File with duplicate soundscapes merged and averaged.
│   ├── remarkable_groups/                              # Storage for remarkable group data.
│   └── comparison_groups/                              # Storage for comparison group data.
│
├── t-test-outputs/                                     # Outputs from verbose_compare_groups function.
│   ├── corresponding_comparisons.md                    # Detailed comparison of group means.
│   └── total_comparisons.md                            # Summary of statistical analysis results.
│
├── ARAUS T-testing.ipynb                               # Main notebook for code, organized by purpose and creation.
└── requirements.txt                                     # List of libraries the project depends on for pip installation.

```

## Usage
The code is entirely documented and **modular**, so that groups, affective state filters, and psychoacoustic parameters can be easily adjusted in the `remarkable_groups` & `comparison_groups` lists, their filter lists specified when initializing the Group objects, and the `comparison_columns` dictionary respectively. 

#### Getting Started
1. Clone the repository from the [GitHub page](https://github.com/hectorastrom/ARAUS-T-Testing) using git
2. Create and activate a [virtual environment](https://code.visualstudio.com/docs/python/environments)
3. In the virtual environment, install the requirements from `requirements.txt` 

```pip freeze -r requirements.txt```

All functions and code live in `ARAUS T-testing.ipynb`
#### Using the Code
- All functions are pre-documented in `ARAUS T-testing.ipynb`. Make sure to run all prior cells before trying to run a later one.

- If you're trying to adjust which groups to compare, change the `remarkable_groups` and `comparison_groups` lists according by initializing Group objects with specific filters
    - Group objects accept three main parameters and a variable list of filters which is documented in the class defintion

- To compare specific groups and output to a new file, use the `verbose_compare_groups` function which will print a Markdown formatted comparison between two groups. You must open a file to enter as an argument for the function before using it
    - Use loops to create multiple comparisons in a single file, as demonstrated below

*Example format:*

```python
# Open file to write comparison output to
with open(file_path, "w") as file:
    # Compare all combinations of groups individually
    for r_group in remarkable_groups:
        for c_group in comparison_groups:
            verbose_compare_groups(
                r_group,
                c_group,
                file
            )
      # Optionally: separate comparisons
      file.write("______________________________________</br></br></br></br>\n\n")
```
Specific code in cell 7 used to generate `total_comparisons.md` and `corresponding_comparisons.md` serve as additional examples on adjusting the format of comparison, styling (including adding a table of contents), group inclusion, and file pathing.


## Results

A complete analysis of all remarkable groups vs. comparison groups outputted by `verbose_compare_groups` in cell 7 can be found in `/t-test-outputs/total_comparisons.md`. 

These are specific results for arbitrary, but insightful, filter parameters and groups. For more nuanced and specific results, we encourage performing independent adjustment of group selection and comparison generation: a process detailed [above](#using-the-code).


## Conclusions
*Through synthesizing and analyzing data from results comparing top 5% and bottom 5% of pleasant and vibrant soundscapes*

Our hypothesis that high valence and high arousal soundscapes will lead to restoration was not supported ubititiously. We found a strong descreptency between the directions of change to the psychoacoustic parameters whcih lead to highly rated pleasant soundscapes and the change that leads to highly rated vibrant soundscapes; in fact, they are completely incongruent.

To create **pleasant** soundscapes, the statistically significant results from `corresponding_comparisons.md` showed to: 

- **Raise** peak <u>sharpness</u> – *in particular high frequency content*
- Greatly **lower** average & peak <u>loudness</u> – *lowering both decibel levels and reducing high frequency content, which is perceieved as louder*
- **Lower** average & peak <u>fluctuation strength</u> – *reducing the speed of the slow up-and-down change in loudness*
- Greatly **lower** average & peak [roughness](https://hub.salford.ac.uk/sirc-acoustics/psychoacoustics/sound-quality-making-products-sound-better/an-introduction-to-sound-quality-testing/roughness-fluctuation-strength/) – *which decreases from a 70hz modulation frequency peak*
- Greatly **lower** average & peak <u>tonality</u> – *reducing distinct pitches that stand out against the background noise*

Conversely, to create **vibrant** soundscapes, the results showed to:

- **Raise** average & peak <u>loudness</u>
- **Raise** average & peak <u>fluctuation strength</u>
- **Raise** average & peak <u>rougness</u>
- **Raise** average & peak <u>tonality</u>

We can see that these results are in direct contrast. This clues us to reform our hypothesis, and to further distinguish the characteristics of pleasant soundscapes from vibrant soundscapes. Further testing must be conducted to determine if pleasant or vibrant soundscapes lead to greater restoration.



## Future Steps
- Explore tonality of the most remarkable soundscapes to determine a dominant frequency range in spectrum analysis.
- Perform experiment to analyze restorativeness of highly pleasant soundscapes and highly vibrant soundscapes (using features summarized in the [conclusions](#conclusions))
- Produce unconventional soundscapes emphasizing the psychoacoustic parameters found to be most restorative.

## Maintainers
- Hector Astrom - *hastrom@mit.edu*


---

*Note: This README is part of an ongoing research project at the MIT Media Lab analyzing the characteristics of soundscapes that lead to attention restoration. It integrates findings from initial and subsequent analyses to provide a comprehensive view of the role of psychoacoustic features in soundscapes in shaping affective states*
