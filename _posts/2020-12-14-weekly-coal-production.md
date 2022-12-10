---
layout: post
title:  "U.S. coal production this year fell to its lowest level in more than 3 decades"
subtitle: "Comparing 2020 weekly U.S. coal production to historic levels"
# date:   2020-12-14 06:00:00
# thumbnail: "https://raw.githubusercontent.com/measrainsey/weekly-coal/main/figures/fig_weekly_coal_production.png"
# <!--permalink: kincade-aod-->
# categories: [energy, coal, R]
---

The scripts needed to create this figure are provided in this [Github repository](https://github.com/measrainsey/weekly-coal/).  

<img src="https://raw.githubusercontent.com/measrainsey/weekly-coal/main/figures/fig_weekly_coal_production.png" width="800" class="img-center">

According to the U.S. Energy Information Administration’s (EIA) [Weekly Coal Production estimates](https://www.eia.gov/coal/production/weekly/), coal production in nearly every week has been lower than they've been since the 1980s. The figure above shows 2020 weekly coal production (yellow line) up to December 5, 2020 (week 49). Given the anomoly of 2020, I also wanted to compare 2020 coal production to 2019 levels (red line). In general, weekly coal production in 2019 was lower than the historic range from 1984-2018 -- unsurprising considering [national coal production in 2019 was the lowest since 1978](https://www.eia.gov/todayinenergy/detail.php?id=44536). 

## Notes on the data

All of the data needed for this analysis can be downloaded on the EIA's [Weekly Coal Production estimates](https://www.eia.gov/coal/production/weekly/) website. To view previous weeks' data, click on the "Archive" link.

I decided to use the "Original estimates*" version for 2020 since the data goes up to a more recent week than the "Revised estimates" version. For 1968-2019, I used the "Revised estimates" version. A simple bash script can be used to automate the data downloading process for all the years:

```bash
#!/bin/bash
# for 2020, get original estimates:
    wget -nc 'https://www.eia.gov/coal/production/weekly/current_year/weekprodforecast2020tot.xls'

# for all historic years, get revised estimates:
    for i in $(seq -w 2015 2019)
        do wget -nc 'https://www.eia.gov/coal/production/weekly/current_year/weekprod'$i'tot.xls'
        done

    for i in $(seq -w 1984 2014)
        do wget -nc 'https://www.eia.gov/coal/production/weekly/archive/weekprod'$i'tot.xls'
        done
```

### Data cleaning

The main cleaning involved for this analysis was renaming columns and binding datasets across the years, since the formatting and number of weeks (52 or 53) changed between years. Additionally, I noticed that for some years, the week 1 and/or week 53 production values were significantly lower than the other weeks', leading me to assume that one year's 53rd week can be shared with the following year's 1st week. In such instances of low week 1 and/or week 53 production values, I aggregated the production to be the later year's week 1 production. 
