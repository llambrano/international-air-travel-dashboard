# Data dictionary: US outbound by region 2011–2026 (monthly, tidy)

*File: `data/us_outbound_by_region_2011_2026.csv` · Prepared September 26, 2026*

## 1. What this file is

Monthly counts of trips abroad by U.S. citizens, by destination, from
January 2011 to June 2026. Every figure is public U.S. government data.

| **Item**    | **Detail**                                                                                                                                                                        |
|-------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Source      | U.S. Department of Commerce, International Trade Administration, National Travel and Tourism Office (NTTO): "US Outbound to World Regions" workbook, file dated September 3, 2026 |
| Source page | https://www.trade.gov/us-international-air-travel-statistics-i-92-data                                                                                                            |
| Coverage    | January 2011 to June 2026 (186 months); July-December 2026 not yet published                                                                                                      |
| Grain       | One row per month per series (15 series)                                                                                                                                          |
| Size        | 2,790 rows, 8 columns                                                                                                                                                             |
| Unit        | Number of departures (trips). One person who travels twice counts twice.                                                                                                          |

## 2. Columns

| **Column**   | **Type**          | **Description**                                                                                        | **Example**                                           |
|--------------|-------------------|--------------------------------------------------------------------------------------------------------|-------------------------------------------------------|
| year         | Whole number      | Calendar year of the departures.                                                                       | 2025                                                  |
| month        | Text              | Three-letter month name.                                                                               | Jul                                                   |
| month_start  | Date (YYYY-MM-DD) | First day of the month; use this for time-series charts and sorting.                                   | 2025-07-01                                            |
| region       | Text              | The series being counted: one of 8 world regions, Mexico or Canada, or a total (see the series table). | Europe                                                |
| category     | Text              | Type of series: region, country total, air subset or total. Use it to avoid double counting.           | region                                                |
| travelers    | Whole number      | Number of U.S. citizen departures to that destination in that month (trips, not unique people).        | 2787158                                               |
| source_table | Text              | Which NTTO table and release the figure comes from.                                                    | Final 2025 figures as published in NTTO's 2026 tab... |
| data_note    | Text              | Blank for most rows. Explains a known issue or correction for that row (50 rows have a note).          | Corrected: NTTO's final 2017 table repeats...         |

## 3. Series (values of the region column)

Each month has 15 rows, one per series. The category column tells you
which rows can be added together.

| **Series**      | **Category**  | **What it counts**                                                                                                         |
|-----------------|---------------|----------------------------------------------------------------------------------------------------------------------------|
| Europe          | region        | Overseas air departures to Europe                                                                                          |
| Caribbean       | region        | Overseas air departures to the Caribbean                                                                                   |
| Asia            | region        | Overseas air departures to Asia                                                                                            |
| South America   | region        | Overseas air departures to South America                                                                                   |
| Central America | region        | Overseas air departures to Central America (excludes Mexico)                                                               |
| Oceania         | region        | Overseas air departures to Australia, New Zealand and the Pacific                                                          |
| Middle East     | region        | Overseas air departures to the Middle East                                                                                 |
| Africa          | region        | Overseas air departures to Africa                                                                                          |
| Total Overseas  | total         | Sum of the 8 regions above                                                                                                 |
| Mexico (Total)  | country total | All trips to Mexico: air, land and border stays of 1+ nights (source: INEGI/Banco de Mexico); recent years are preliminary |
| Mexico (Air)    | air subset    | Air trips to Mexico only. Already included in Mexico (Total); never add the two together                                   |
| Canada (Total)  | country total | All trips to Canada: air, land and border stays of 1+ nights (source: Statistics Canada)                                   |
| Canada (Air)    | air subset    | Air trips to Canada only. Already included in Canada (Total); never add the two together                                   |
| North America   | total         | Published Mexico (Total) + Canada (Total). Use your own calculation instead (see issue 3)                                  |
| Grand Total     | total         | All U.S. citizen international departures: Total Overseas + Mexico (Total) + Canada (Total)                                |

### How the series fit together

- **Total Overseas** = Europe + Caribbean + Asia + South America +
  Central America + Oceania + Middle East + Africa

- **North America** = Mexico (Total) + Canada (Total) (calculate it
  yourself)

- **Grand Total** = Total Overseas + Mexico (Total) + Canada (Total)

- **Air subsets** (Mexico (Air), Canada (Air)) are already inside the
  country totals. Use them to compare air travel, never add them to
  totals.

- **Never sum the whole travelers column**: totals and subsets would be
  counted two or three times. Filter by category first.

## 4. Where each year comes from

NTTO revises past years after first publishing them. For every year the
file uses the latest published version.

| **Year**  | **Source table in the NTTO workbook**                                                  |
|-----------|----------------------------------------------------------------------------------------|
| 2011      | NTTO 2012 tab, prior-year comparison block                                             |
| 2012-2018 | Final block in the following year's tab (e.g. 2017 comes from the 2018 tab)            |
| 2019      | Revised block in the 2020 tab (Mexico and Canada were revised after the first release) |
| 2020      | Revised block in the 2021 tab                                                          |
| 2021      | Revised block in the 2022 tab                                                          |
| 2022-2024 | That year's own tab (no later revisions)                                               |
| 2025      | Full-year block in the 2026 tab                                                        |
| 2026      | 2026 tab, January-June (released September 2, 2026)                                    |

## 5. Inconsistencies found in the government data

A full check compared every month against every version NTTO published.
These are the issues found; issues 1-4 are flagged in the data_note
column.

| **\#** | **Where**                  | **What is wrong**                                                                                                                                                                                     | **How it is handled**                                                                                                |
|--------|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| 1      | April 2017, Mexico (Air)   | NTTO's final 2017 table (2018 tab, cell E72) repeats Canada's total of 880,234. The correct value, 855,209, appears in NTTO's original 2017 tab (E17) and the 2018 tab's comparison block (E49).      | Corrected to 855,209 and explained in the data_note column.                                     |
| 2      | 2011, Mexico (Total)       | The 12 monthly values add up to 20,706,608, but NTTO published an annual total of 20,084,000.                                                                                                         | Monthly values kept. Totals you calculate from this file will be 20.7 million for 2011.                              |
| 3      | 2022-2024, North America   | The published North America row doesn't equal Mexico (Total) + Canada (Total) in 22 months (6 in 2022, 4 in 2023, 12 in 2024), by up to 156,381 trips.                                                | Kept as published and noted. Calculate North America yourself as Mexico (Total) + Canada (Total).                    |
| 4      | April 2022, Grand Total    | The published Grand Total (5,989,988) is 10,642 lower than Total Overseas + Mexico (Total) + Canada (Total) (6,000,630).                                                                              | Kept as published. Calculating Grand Total yourself fixes it.                                                        |
| 5      | Revisions between releases | Mexico (Air) and Canada (Air) were revised as whole rows in several years (2011-2013, 2015, 2017-2019), and Mexico/Canada totals are revised regularly. The 8 regions never changed between versions. | The file always uses the latest published version; earlier NTTO releases may show the first-published numbers. |

**Recommended practice:** calculate Total Overseas, North America and
Grand Total from their parts instead of using the published total rows.
The 8 regions and the Mexico/Canada totals are consistent.

## 6. Things to keep in mind

- Methodology: from 2011 overseas and Mexico air counts come from DHS
  APIS passenger manifests, so 2011-2026 is comparable. The older 2000-2010
  NTTO files are air-only and in thousands, and should not be
  joined to this file without adjustment.

- Overseas regions count non-stop flights from a U.S. airport to a
  foreign airport, so a trip via a connecting country is counted where
  the flight lands.

- Mexico (Total) comes from Mexican government estimates and is labeled
  preliminary for recent years; values were rounded to whole trips.

- 2020-2021 were distorted by COVID-19 travel restrictions; treat them
  separately when forecasting.

- 2026 has only six months. Compare it with January-June of earlier
  years, not full years.

## 7. Reference: annual Grand Total

Totals calculated from the monthly Grand Total rows in this file.

| **Year** | **U.S. citizen departures** | **Note**                                          |
|----------|-----------------------------|---------------------------------------------------|
| 2011     | 59,326,560                  |                                                   |
| 2012     | 60,695,685                  |                                                   |
| 2013     | 61,343,634                  |                                                   |
| 2014     | 68,176,355                  |                                                   |
| 2015     | 74,191,474                  |                                                   |
| 2016     | 80,226,166                  |                                                   |
| 2017     | 87,554,794                  |                                                   |
| 2018     | 92,586,942                  |                                                   |
| 2019     | 99,270,496                  | Pre-pandemic peak                                 |
| 2020     | 33,160,265                  | COVID-19: April 2020 fell to 771,432              |
| 2021     | 48,711,033                  | Partial recovery                                  |
| 2022     | 80,846,987                  |                                                   |
| 2023     | 98,627,062                  | Back near 2019 levels                             |
| 2024     | 106,376,575                 | New record                                        |
| 2025     | 109,261,808                 | New record (+2.7%)                                |
| 2026     | 54,240,227                  | January-June only (+3.4% vs. same months of 2025) |
