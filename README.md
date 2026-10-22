# Netflix Content Analysis (EDA)

An exploratory data analysis of the **Netflix Titles** dataset: 8,807 movies and TV shows described by 12 columns. The project cleans messy, multi-valued fields and uses visualisations to understand what the catalogue contains, where it comes from, when it was added and who it is aimed at.

![Python](https://img.shields.io/badge/Python-3.x-blue) ![pandas](https://img.shields.io/badge/pandas-data%20analysis-150458) ![Seaborn](https://img.shields.io/badge/Seaborn-visualisation-4c72b0) ![Status](https://img.shields.io/badge/status-completed-brightgreen)

---

## Table of Contents

1. [Project Overview](#project-overview)
2. [Dataset](#dataset)
3. [Project Structure](#project-structure)
4. [Methodology](#methodology)
5. [Key Findings](#key-findings)
6. [Visualisations](#visualisations)
7. [Getting Started](#getting-started)
8. [Limitations](#limitations)
9. [Future Work](#future-work)
10. [Author](#author)

---

## Project Overview

**Goal:** understand the shape of the Netflix catalogue and turn the raw CSV into something that can be analysed reliably.

**Questions explored**

- How many titles are movies and how many are TV shows?
- Which countries contribute the most titles?
- Which genres dominate the catalogue?
- How has the number of titles added changed over the years and across months?
- Which age ratings are most common?
- Do movies and TV shows behave differently across countries, genres, months, years and ratings?

## Dataset

| Property | Value |
|---|---|
| File | `netflix_titles.csv` (public Netflix Movies and TV Shows dataset, available on Kaggle) |
| Rows | 8,807 |
| Columns | 12 |

| Column | Description |
|---|---|
| `show_id` | Unique ID of the title |
| `type` | Movie or TV Show |
| `title` | Title name |
| `director` | Director(s) |
| `cast` | Cast members |
| `country` | Production country or countries |
| `date_added` | Date the title was added to Netflix |
| `release_year` | Original release year |
| `rating` | Age or content rating |
| `duration` | Minutes (movies) or seasons (TV shows) |
| `listed_in` | Genre categories |
| `description` | Short synopsis |

**Missing values:** `director` 2,634 (about 30%), `country` 831 (about 9%), `cast` 825 (about 9%), `date_added` 10, `rating` 4, `duration` 3.

> The dataset file is not included in this repository. Download it from Kaggle and place it next to the notebook (see [Getting Started](#getting-started)).

## Project Structure

```
netflix-eda/
├── Netflix_p.ipynb      # Main analysis notebook
├── README.md            # This file
├── images/              # Charts exported from the notebook
└── netflix_titles.csv   # Dataset (download separately)
```

## Methodology

The notebook follows four stages.

### 1. Data understanding
`head()`, `tail()`, `shape`, `info()` and `isnull().sum()` to inspect structure, data types and missing values.

### 2. Data cleaning and transformation

| Column | Problem | Fix |
|---|---|---|
| `country` | Several countries in one cell (748 distinct values) | Split on commas, `explode()`, strip whitespace into `new_country` (123 individual countries); missing values set to `unknown` |
| `duration` | Minutes and seasons mixed in one column | Extract the number with regex, then create `movie_duration` and `tv_show_duration` |
| `rating` | Three rows contain runtimes such as `74 min` | Move the number to `runtime`, null the invalid rating, label remaining gaps `no_rating` in `new_rating` |
| `listed_in` | Several genres in one cell | Split and `explode()` into `listed_in_new` |
| `date_added` | Stored as text | Convert with `pd.to_datetime`, derive `year_added` and `months_added` |

### 3. Focused subsets
To keep charts readable, long tails were trimmed:

- `df_countries`: United States, India, United Kingdom, France, Canada, Japan
- `df` / `df_genres`: Comedies, TV Comedies, Dramas, TV Dramas, International Movies, International TV Shows, later merged into **Comedy**, **Drama** and **International**

### 4. Analysis
- **Univariate:** count plots (country, genre, type, month, year, rating) and a violin plot (genre).
- **Bivariate:** grouped count plots comparing Movies and TV Shows by country, genre group, month, year and rating.

## Key Findings

- **Movie-heavy catalogue.** Movies outnumber TV shows overall, and in every month and year examined.
- **United States leads.** In the six-country subset: United States 6,780, India 2,804, United Kingdom 1,783, France 916, Canada 877, Japan 733. The US alone exceeds the next three combined.
- **Japan is the exception.** The notebook's chart shows TV shows ahead of movies for Japan.
- **Top genres.** International Movies (3,513), Dramas (3,202) and Comedies (1,981) are the largest labels; International TV Shows (1,465), TV Dramas and TV Comedies lead on the TV side.
- **Growth from 2016.** Additions were small before 2015, rose to about 1,100 in 2016 and about 3,200 in 2017, and peaked around 2019 (about 5,400) and 2020 (about 5,150). 2021 looks lower because the data covers only part of that year.
- **Seasonality.** Movie additions are lowest in February and May and highest around January and July. TV-show additions are steadier through the year.
- **Mature-audience skew.** TV-MA and TV-14 are the two most common ratings for both movies and TV shows. Children's ratings are a small share.

## Visualisations

### Country

| All countries | Major six countries |
|---|---|
| ![All countries](images/01_titles_per_country_all.png) | ![Major countries](images/02_titles_major_countries.png) |

### Genre

![Genres](images/03_titles_per_genre_all.png)

![Major genres violin plot](images/04_major_genres_violin.png)

### Movies vs TV Shows

![Movies vs TV Shows](images/05_movies_vs_tv_shows.png)

### Time of addition

![Additions per month](images/06_additions_per_month.png)

![Additions per year](images/07_additions_per_year.png)

### Ratings

![Rating distribution](images/08_rating_distribution.png)

### Bivariate comparisons (Movies vs TV Shows)

![By country](images/09_type_by_country.png)

![By genre group](images/10_type_by_genre_group.png)

![By month](images/11_type_by_month.png)

![By year](images/12_type_by_year.png)

![By rating](images/13_type_by_rating.png)

## Getting Started

### Prerequisites
- Python 3.9 or newer
- Jupyter Notebook, JupyterLab or Google Colab

### Installation

```bash
# 1. Clone the repository
git clone <your-repository-url>
cd netflix-eda

# 2. (Optional) create a virtual environment
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate

# 3. Install dependencies
pip install pandas numpy matplotlib seaborn regex notebook
```

### Run

1. Download `netflix_titles.csv` from Kaggle and place it in the project folder.
2. Open `Netflix_p.ipynb`.
3. The notebook was written in Google Colab and loads the file from Google Drive. Change the path in the loading cell to your local file:

   ```python
   data = pd.read_csv("netflix_titles.csv")
   ```
4. Run all cells (`Kernel` > `Restart & Run All`).

## Limitations

- **Catalogue share is not audience demand.** The dataset has no viewing figures, so results describe what is available, not what is watched.
- **Counts are inflated after `explode()`.** A title with several countries or genres appears in several rows, so absolute totals (for example, the type counts printed after splitting) exceed the 8,807 real titles. Ratios remain useful, but for exact counts use unique `show_id` values.
- **Manual subsets.** Choosing six countries and six genre labels improves readability but drops the long tail.
- **Partial final year.** 2021 is incomplete.
- **Missing data.** Roughly 30% of directors and 9% each of cast and country are missing.
- **Descriptive only.** No statistical tests or models are used, so findings are patterns, not causes.

## Future Work

- Count unique titles with `show_id` when comparing categories.
- Plot `movie_duration` and `tv_show_duration` (already created in the notebook).
- Analyse the gap between `release_year` and `year_added`.
- Explore directors and cast networks.
- Apply text analysis (TF-IDF) to `description` and build a simple content-based recommender.
- Cluster titles by genre, country and rating, or add IMDb scores for popularity analysis.

## Author

**[Your Name]**
BCA (AI/ML), [Institution Name]
[LinkedIn / GitHub / Email]

## Acknowledgements

- Netflix Movies and TV Shows dataset (Kaggle)
- pandas, NumPy, Matplotlib and Seaborn documentation and communities
