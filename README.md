# Ballarat Data Exchange Data.ballarat.vic.gov.au Data Quality Guide for Tabular Data

## Introduction

**Ballarat Data Exchange Data.ballarat.vic.gov.au** was launched in 2020 as the City of Ballarat's [open data](http://opendatahandbook.org/guide/en/what-is-open-data/) portal.

From launch of the site we intended to make our data:

1. More relevant to the general public
2. More usable for researchers, developers and other power users. 

To meet these requirements, we adopted a new set of *data quality standards* to ensure *all* published data can be:

1. Visualised in charts and maps
2. Accessible via APIs

**Ballarat Data Exchange Data.ballarat.vic.gov.au** was launched in March 2020. All published datasets can be accessed via APIs, and almost all datasets are visualised so users can understand trends at a glance.

**All datasets must meet these data quality requirements before publication on Data.ballarat.vic.gov.au**.

---

## Key Principles

### 1. Machine Readability
Machine-readable data is data in a format that can be understood by a computer. This means that relevant fields can be extracted and parsed without human input. 

Just because a dataset is *digitally accessible* does not necessarily mean it is *machine readable* as well ([more on that here](https://www.data.gov/developers/blog/primer-machine-readability-online-documents-and-data)).

In the spirit of open data, datasets must be provided in non-proprietary file formats. Tabular data is provided as [CSV](https://en.wikipedia.org/wiki/Comma-separated_values) files or in [JSON](http://www.json.org/) format.

Metadata must be excluded from data files. They will be included in the Information tab provided on the Ballarat Data Exchange.

### 2. Tidy Data
The concept of *tidy data* is based on [this paper](http://vita.had.co.nz/papers/tidy-data.html) by Hadley Wickham, author of popular R packages ggplot2 and plyr.

Hadley proposes three principles:

1. **Each column is one variable.** This means each individual column must have a single unit of measurement
2. **Each row is one observation.** Note that this does not mean there is only one observed variable per row. There can be multiple observed variables per row: “An observation contains all values measured on the same unit (like a person, or a day, or a race) across attributes.” (Wickham Pg 3) 
3. **One type of observation unit per table** For example, observations on fertility rates of the whole female population and that females of different age groups should be in two tables: 
    - “Total Fertility Rate”, where observation unit is the entire female population
    - “Total Fertility Rate by Age Group”, where observation unit is female population of different age groups

### 3. Consistency
Each variable should be processed and documented in the same way across datasets. Examples include:

- Date formats
- Null and negligible values
- Unit of measurement

This makes it easier for users to mashup and use multiple datasets, even from different agencies


### 4. Granularity & Precision
As far as possible, departments should provide raw, granular data instead of aggregated and process data, such as percentages. 
Personal private information shall not be disclosed, as decribed in the City of Ballarat [Information Privacy Policy](https://www.ballarat.vic.gov.au/sites/default/files/2019-04/Information%20Privacy%20Policy.pdf)

Totals and sub-totals should be in separate tables if needed. For example, there are cases where aggregate numbers (e.g. totals, indices) cannot be derived from granular data points. 

### 5. Human Readability
Datasets should also be presented in a way that makes sense to human users. 

This is why we have created a new set of metadata specifications along with this Data Quality Guide, to ensure that datasets are documented clearly. For instance, each table must be accompanied by a schema, which states the variable type. 

Plain language should be used in naming datasets and the elements in them as far as possible. Column names should be descriptive and meaningful. All jargon must be explained clearly, and where necessary, reference links should be provided. 

### 6. Data Standards
Where there is an appropriate standard, the [Open Council Data Standards](http://standards.opencouncildata.org/) shall be followed.
---

## Structure for Tabular Data

### 1. Column Headers
- Only alphanumeric or these 3 special characters: .-_
    - Ampersand (&) should be replaced by “and” if needed
- Each must be unique
    - Can’t have two headers called "duration"
- All headers are in lower case
    - e.g. "rating"
- If header contains more than one word, use underscores to join
    - e.g. vehicle_type
- Units of measure should be omitted (to be included in metadata)
- Keep short (less than 25 characters)
    - The full name can be stored in schema section of the information tab

### 2. Column Order
- *Date and time variables* should always be in the first column for time series data
- *Fixed variables* should be ordered with the highest-level variable on the left and most granular variable on the right
- *Observed variables* should always be on the rightmost columns

### 3. Row Order
- Rows should be ordered from the leftmost column to the rightmost one
- For datetime variables, order chronologically
- For fixed variables, order alphabetically (or numerically)

### 4. Date and Time Variables
- Based on ISO8601, an international standard for representing date and time. We chose the "extended format" with the hyphens because it is humanly more readable.
    - Compare 2016-01-01 to 20160101

- All date and time variables must be in Australia/Melbourne timezone unless specified.

- Date variables:

    | Interval    | Column name | Format     | Range of values    | Example   |
    |-------------|-------------|------------|--------------------|-----------|
    | Annual      | `year`      | YYYY       | YYYY: 1900 onwards | 2015      |
    | Monthly     | `month`     | YYYY-MM    | MM: 01 to 12       | 2015-01   |
    | Daily       | `date`      | YYYY-MM-DD | DD: 01 to 31       | 2015-01-01|
    | Weekly      | `week`      | YYYY-[W]WW | [W]WW: W01 to W53  | 2015-W01  |
    | Quarterly   | `quarter`   | YYYY-[Q]Q  | [Q]Q: Q1 to Q4     | 2015-Q1   |
    | Half-yearly | `half_year` | YYYY-[H]H  | [H]H: H1 or H2     | 2015-H1   |
    
- For financial periods, prefix “financial_” to column name:

    | Interval               | Column name           | Format    | Example |
    |------------------------|-----------------------|-----------|---------|
    | Financial, annual      | `financial_year`      | YYYY      | 2015    |
    | Financial, quarterly   | `financial_quarter`   | YYYY-[Q]Q | 2015-Q1 |
    | Financial, half-yearly | `financial_half_year` | YYYY-[H]H | 2015-H1 |

    - Financial year start-date must be indicated in metadata

- For date-time variables:

    | Type        | Column name | Format                     | Example             |
    |-------------|-------------|----------------------------|---------------------|
    | Date + time | `date_time` | YYYY-MM-DD[T]hh:mm         | 2015-01-01T12:00    |
    |             |             | *or* YYYY-MM-DD[T]hh:mm:ss | 2015-01-01T12:00:00 |
    | Time only   | `time`      | hh:mm                      | 12:00               |
    |             |             | *or* hh:mm:ss              | 12:00:00            |

- Specify the timezone if it is not Australia/Melbourne:

    | Type        | Column name | Format                     | Example               |
    |-------------|-------------|----------------------------|-----------------------|
    | Date + time | `date_time` | YYYY-MM-DD[T]hh:mm+hh:mm   | 2015-01-01T12:00+00:00|
    |             |             | *or* YYYY-MM-DD[T]hh:mm:ss+hh:mm:ss | 2015-01-01T12:00:00+00:00:00 |

### 5. Textual Variables
- UTF-8 encoding should be used
    - This ensures that special characters such as Chinese characters can be decoded by users
- No line breaks within cells

### 6. Numeric Variables
- No commas
    - E.g. "1000" instead of "1,000"
- No units of measurement
    - Units should be in metadata only
- Express as full number where possible
    - If rounded, indicate in metadata
    - E.g. "1200000" instead of "1.2" (million)
- No rounding if possible
    - Give raw numbers as far as possible
    - If rounding is needed, try to provide at least 2 decimal places of precision
- Percentages can be expressed as either a proportion out of 1 or 100. 
    - E.g.  20% can be expressed as 20 or 0.2
    - The representation of percentages must be consistent throughout each CSV file
    - Agencies must indicate how percentages are expressed in the schema

### 7. Location variables
- Coordinates in EPSG 4326 or EPSG 3414:
- Should be represented in two columns
    - EPSG 4326: `latitude` and `longitude` or 
    - EPSG 3414: `x_coord` and `y_coord`
- In positive/negative floating point
    - e.g. `latitude`: 1.2896700; `longitude`: 103.8500700
- EPSG should be indicated in metadata
- Addresses should be represented in two columns if possible
    - `street_address`: e.g. 25 Armstrong Street South
    - `suburb`:Ballarat Central
    - `post_code`: e.g. 3350

### 8. Null/negligible values
- For any variable type:

    | Value | Meaning                                |
    |-------|----------------------------------------|
    | na    | Datum not available or not applicable  |
    | -     | Datum is negligible or not significant |
    | s     | Datum is suppressed                    |

- If possible, explain why there are such values in the metadata

---

## Reserved column names

The following column names should be used only if they adhere to the definitions in this guide:
- `year`
- `half_year`
- `quarter`
- `month`
- `week`
- `date`
- `datetime`
- `time`
- `financial_year`
- `financial_half_year`
- `financial_quarter`
- `latitude`
- `longitude`
- `x_coord`
- `y_coord`
- `street_address`
- `suburb`
- `post_code`

---

## Contact us 

We will continue to review this Data Quality Guide to ensure that we meet the needs of our users. 

We welcome all feedback to improve the quality of data published on **Data.ballarat.vic.gov.au**. Let us know on **Data.ballarat.vic.gov.au** if you have any comments or queries on the guide.
