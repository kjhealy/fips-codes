# County and State FIPS codes
 
Kieran Healy (`@kjhealy`)

## Summary

- Three CSV files with some basic FIPS identifying information for US States (`state_fips_master.csv`), Counties (`county_fips_master.csv`), and both together (`state_and_county_fips_master.csv`). I got sick of constantly having to write code to match on one or other of these identifiers in order to merge data files (e.g. for maps). So this can serve as a basis for harmonizing files that use one, some, or some variant of these identifiers. For example, sometimes leading zeros are omitted in the FIPS, sometimes not; sometimes the FIPS is coded in data as one number, sometimes as a character-vector of digits, sometimes as two separate state and county numbers, and so on. The Census also has its own supra-state units (regions and divisions). These files make it easier to merge and match to data indexed in one or other of these ways.




## Files

The `county_fips_master.csv` file looks like this:


| fips|county_name    |state_abbr |state_name |long_name         | sumlev| region| division| state| county|crosswalk |region_name |division_name      |
|----:|:--------------|:----------|:----------|:-----------------|------:|------:|--------:|-----:|------:|:---------|:-----------|:------------------|
| 1001|Autauga County |AL         |Alabama    |Autauga County AL |     50|      3|        6|     1|      1|3-6-1-1   |South       |East South Central |
| 1003|Baldwin County |AL         |Alabama    |Baldwin County AL |     50|      3|        6|     1|      3|3-6-1-3   |South       |East South Central |
| 1005|Barbour County |AL         |Alabama    |Barbour County AL |     50|      3|        6|     1|      5|3-6-1-5   |South       |East South Central |
| 1007|Bibb County    |AL         |Alabama    |Bibb County AL    |     50|      3|        6|     1|      7|3-6-1-7   |South       |East South Central |
| 1009|Blount County  |AL         |Alabama    |Blount County AL  |     50|      3|        6|     1|      9|3-6-1-9   |South       |East South Central |
| 1011|Bullock County |AL         |Alabama    |Bullock County AL |     50|      3|        6|     1|     11|3-6-1-11  |South       |East South Central |

- `crosswalk` is just a concatenation of region, division, state, and county identifiers, sometimes useful for merging with files that do not include a full numeric FIPS code, but instead split it up by state and county.

The `state_fips_master.csv` file looks like this:


|state_name |state_abbr |long_name     | fips| sumlev| region| division| state|region_name |division_name      |
|:----------|:----------|:-------------|----:|------:|------:|--------:|-----:|:-----------|:------------------|
|Alabama    |AL         |Alabama AL    |    1|     40|      3|        6|     1|South       |East South Central |
|Alaska     |AK         |Alaska AK     |    2|     40|      4|        9|     2|West        |Pacific            |
|Arizona    |AZ         |Arizona AZ    |    4|     40|      4|        8|     4|West        |Mountain           |
|Arkansas   |AR         |Arkansas AR   |    5|     40|      3|        7|     5|South       |West South Central |
|California |CA         |California CA |    6|     40|      4|        9|     6|West        |Pacific            |
|Colorado   |CO         |Colorado CO   |    8|     40|      4|        8|     8|West        |Mountain           |

And the `state_and_county_fips_master.csv` file looks like this:


| fips|name           |state |
|----:|:--------------|:-----|
|    0|UNITED STATES  |NA    |
| 1000|ALABAMA        |NA    |
| 1001|Autauga County |AL    |
| 1003|Baldwin County |AL    |
| 1005|Barbour County |AL    |
| 1007|Bibb County    |AL    |

- This file includes the US overall FIPS code (00000) and each of the state codes as five digit numbers, in order. The country's name and the names of the states are in all-caps.

## Some Gotchas 

When merging and filtering county-level data, there are a few things that might bite you, as they have bitten me. 

- Beware of confusion that may arise when trying to merge FIPS encoded as a character (e.g. State code "04") and FIPS encoded as an integer (4). 
- Counties or Parishes with "Saint" or "St" in their names are sometimes inconsistently spelled or abbreviated.
- Some places have "La" in their name. It's LaSalle Parish (LA), but La Salle County (TX), but LaSalle County (IL). These are sometimes written incorrectly.
- Doña Ana County, NM, has the only "ñ" in America's county names. It can get mangled in the character encoding of text files (especially old ones), or converted to a mere "n".
- It's "District of Columbia", not "Washington DC".
- The District of Columbia has a State-like FIPS code (11) *and* a County-like one (11001).

In any case, you are never trying to match on character strings when merging data that should have unique numerical identifiers. Right? 
