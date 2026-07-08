# Raw Data Directory

Place all downloaded data files here. Do NOT modify these files.

## Required Files (18 total)

### Census ACS Tables (6 files)
Download from: data.census.gov
Geography: All States | Year: 2023

1. `census_acs_s1701.csv` - Poverty Status (Table S1701)
2. `census_acs_s1901.csv` - Income (Table S1901)
3. `census_acs_b25064.csv` - Median Gross Rent (Table B25064)
4. `census_acs_b11001.csv` - Household Type (Table B11001)
5. `census_acs_b02001.csv` - Race (Table B02001)
6. `census_acs_b08301.csv` - Commuting (Table B08301)

### Zillow (1 file)
Download from: zillow.com/research/data

7. `zillow_zhvi_state.csv` - Home Value Index (ZHVI All Homes, State level)

### MERIC Cost of Living (1 file)
Download from: meric.mo.gov/data/cost-living-data-series

8. `meric_coli.xlsx` - Cost of Living Index by State

### Tax Foundation (3 files)
Download from: taxfoundation.org

9. `taxfoundation_sales.csv` - State Sales Tax Rates
10. `taxfoundation_income.csv` - State Income Tax Rates
11. `taxfoundation_property.csv` - Property Tax Rates by State

### USDA ERS (2 files)
Download from: ers.usda.gov/data-products/county-level-data-sets

12. `usda_ers_education.xlsx` - Education levels (use "Education" tab)
13. `usda_ers_rucc.xlsx` - Rural-Urban Continuum Codes 2023

### FBI UCR (1 file)
Download from: cde.ucr.cjis.gov

14. `fbi_estimated_crimes_2023.csv` - Filter to 2023, state-level only

### SAMHSA (1 file)
Download from: samhsa.gov/data/data-we-collect/nsduh-national-survey-drug-use-and-health

15. `samhsa_nsduh_state_prevalence.csv` - State drug use prevalence (unzip first)

### CDC NCHS (1 file)
Download from: cdc.gov/nchs/nvss/marriage-divorce.htm

16. `cdc_divorce_rates.xlsx` - State divorce rates 2000-2023

### HUD (1 file)
Download from: hudexchange.info/programs/coc/coc-homeless-populations

17. `hud_pit_count_2023.xlsx` - Point-in-Time Count 2023

---

**Important Notes:**
- All files must be 2023 data (except where noted)
- Census files: remove "Note" rows if present
- SAMHSA file: downloads as ZIP, extract CSV first
- CDC & HUD files: remove header/footer rows before use
- FBI: AVOID 2021 data (only 65% coverage that year)
