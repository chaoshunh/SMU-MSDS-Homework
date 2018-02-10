# Codebook for Unit 5 Live Session Assignment
## Files
* **yob2016.txt:** (raw data) Semi-colon delimited text file containing raw data for popular baby names in 2016.
* **yob2015.txt:** (raw data) Comma delimited text file containing raw data for popular baby names in 2015.
* **Top_10_Popular_Girl_Names2015-2016.csv:** (output csv) Comma separated values file containing the top 10 names and instances of each name for female children from 2015-2016.
* **DGeislinger_Livesession5.rmd:** (R Markdown file) Contains code and output for the creation of the output csv from the 2 raw data files.

## R Objects in *DGeislinger_Livesession5.rmd*
* **df:** (data.frame) Table imported from data in *yob2016.txt* with human readable variable (column) names.
* **bad_name:** (string) Misspelled name erroneously included in *yob2016.txt*.
* **y2016:** (data.frame) *df* but with *bad_name* row removed.
* **y2015:** (data.frame) Table imported from data in *yob2015.txt* with human readable variable (column) names.
* **final:** (data.frame) Merged data from *y2015* andd *y2016* merged by columns *Name* and *Gender*.
* **top_10:** (function) Returns the top 10 rows of a dataset (supplied as the sole argument) sorted by column *Total* in descending order.
* **girl_names_final:** (data.frame) The *final* data.frame but with boy names excluded.
* **top_10_girl_names:** (data.frame) Top 10 rows of *girl_names_final* as returned by *top_10*.
* **top_10_girl_names_for_csv:** (data.frame) *top_10_girl_names* data.frame formatted for writing to csv file by omitting all columns but *Name* and *Total*.
