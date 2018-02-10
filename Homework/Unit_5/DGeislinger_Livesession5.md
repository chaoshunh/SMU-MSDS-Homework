---
title: "6306 - Homework 5"
author: "Dana Geislinger"
date: "February 10, 2018"
output:
  html_document:
    keep_md: true
---
## 1. **Data Munging:** Process data from the popular children's name for 2016 dataset *yob2016.txt* in R.
#### A. Import *yob2016.txt* into variable *df* with human-readable column titles.

```r
# Import data into df variable
#   header: whether or not data file includes header row
#   sep: delimiter character to split variables in rows
df = read.table("yob2016.txt", header=FALSE, sep=";")
# Give meaningful variable names to columns in df
names(df) = c("Name", "Gender", "2016 Count")
```

#### B. Print summary and structure of *df*.

```r
# Print summary of 2016 child name datafram
summary(df)
```

```
##       Name       Gender      2016 Count     
##  Aalijah:    2   F:18758   Min.   :    5.0  
##  Aaliyan:    2   M:14111   1st Qu.:    7.0  
##  Aamari :    2             Median :   12.0  
##  Aarian :    2             Mean   :  110.7  
##  Aarin  :    2             3rd Qu.:   30.0  
##  Aaris  :    2             Max.   :19414.0  
##  (Other):32857
```

```r
# Print structure of 2016child name dataframe
str(df)
```

```
## 'data.frame':	32869 obs. of  3 variables:
##  $ Name      : Factor w/ 30295 levels "Aaban","Aabha",..: 9317 22546 3770 26409 12019 20596 6185 339 9298 11222 ...
##  $ Gender    : Factor w/ 2 levels "F","M": 1 1 1 1 1 1 1 1 1 1 ...
##  $ 2016 Count: int  19414 19246 16237 16070 14722 14366 13030 11699 10926 10733 ...
```

#### C. Find and print the name that was erroneously entered into *yob2016.txt* twice and misspelled with 3 y's at the end.

```r
# Print names in df ending with 'yyy'
#   value: if FALSE, print line number of match. if TRUE, print value of match.
bad_name = grep('y{3}$', df$Name, value=TRUE)
print(bad_name)
```

```
## [1] "Fionayyy"
```

#### D. Remove the misspelled name from *df* and save resultant dataset in new variable *y2016*.

```r
# Create new dataset 'y2016' equal to 'df' but with misspelled name removed
y2016 = subset(df, Name != bad_name)
# Make sure name has been removed
#   Check length of 'Name' columns to make sure 'y2016' has 1 name less than 'df'
#   Check that 'bad_name' does not appear in y2016
if (length(y2016$Name) == length(df$Name) - 1 &  !bad_name %in% y2016$Name) {sprintf("'%s' removed from dataset.", bad_name)}
```

```
## [1] "'Fionayyy' removed from dataset."
```

## 2. Merge data from *yob2015.txt* with previously imported dataset *y2016*.
#### A. Import *yob2015.txt* with human-readable column titles and save in variable *y2015*.

```r
# Import data into y2015 variable
#   header: whether or not data file includes header row
#   sep: delimiter character to split variables in rows
y2015 = read.table("yob2015.txt", header=FALSE, sep=",")
# Give meaningful variable names to columns in df
names(y2015) = c("Name", "Gender", "2015 Count")
```

#### B. Print the last 10 rows of the dataset and note interesting observations.

```r
# Print last 10 rows of 'y2015'
tail(y2015, 10)
```

```
##         Name Gender 2015 Count
## 33054   Ziyu      M          5
## 33055   Zoel      M          5
## 33056  Zohar      M          5
## 33057 Zolton      M          5
## 33058   Zyah      M          5
## 33059 Zykell      M          5
## 33060 Zyking      M          5
## 33061  Zykir      M          5
## 33062  Zyrus      M          5
## 33063   Zyus      M          5
```
It is interesting that the last 10 names (least popular in the list) all end in 'Z'. This is because the least popular names in the dataset all have counts of 5, and tied-counts appear to be sorted alphebetically by name in the dataset. 
Also interesting to note is that these name are all boy's names. If we look at the full dataset, this is because female names are all listed first in order of popularity (with alphabetical order used in cases of ties), followed by all male names in order of popularity.

#### C. Merge *y2015* and *y2016* by the *Name* column and assign to variable *final*. Rows without values for **both** years should be ignored.

```r
# Combine y2015 and y2016 dataframes by the 'Name' column
#   by: columns to merge by
#   all: if TRUE, includes rows with data for either dataframe merged. If FALSE, only includes rows present in both dataframes.
final = merge(y2015, y2016, by=c("Name", "Gender"), all=FALSE)
```

## 3. Perform data summary on the merged dataframe *final*.
#### A. Create a 'Total' column in final that lists the sum of children named in both years. Print the total number of children given popular names over the course of 2015 and 2016.

```r
# Add 'Total' column with total count for each name for 2015 and 2016
final$Total = final$`2015 Count` + final$`2016 Count`
# Print the total number of people given popular names in 2015 and 2016 according to this data set
sum(final$Total)
```

```
## [1] 7239231
```

#### B. Sort *final* dataframe by the total column and print the overall top 10 most popular names.

```r
# Define function to output the top 10 highest rows of a dataframe based on the value of the 'Total' column
#   Use arrange function from dplyr package to sort 'final' by 'Total'
#     head: Output of arrange provided as argument to head to print only the 10 most popular names
#     desc(Total): tells arrange to sort by 'Total' column, but in decreasing order
top_10 = function(data) {head(dplyr::arrange(data, desc(Total)), 10)}
# Print the top 10 most popular names
for (name in top_10(final)$Name) {
  print(name)
}
```

```
## [1] "Emma"
## [1] "Olivia"
## [1] "Noah"
## [1] "Liam"
## [1] "Sophia"
## [1] "Ava"
## [1] "Mason"
## [1] "William"
## [1] "Jacob"
## [1] "Isabella"
```

#### C. Omit boys names and list only the top 10 most popular girl's names for 2015-2016.

```r
# Create a dataframe of final but only 
girl_names_final = subset(final, Gender == 'F')
# Use the 'top_10' function to print the top 10 girl names
top_10_girl_names = top_10(girl_names_final)
for (name in top_10_girl_names$Name) {
  print(name)
}
```

```
## [1] "Emma"
## [1] "Olivia"
## [1] "Sophia"
## [1] "Ava"
## [1] "Isabella"
## [1] "Mia"
## [1] "Charlotte"
## [1] "Abigail"
## [1] "Emily"
## [1] "Harper"
```

#### D. Write the top 10 most popular girls names to a csv file.

```r
# Create dataframe of top 10 girl names, but only the 'Name' and 'Total' columns
top_10_girl_names_for_csv = top_10_girl_names[, c("Name", "Total")]
# Save top 10 most popular girl names to 'Top10PopularGirlNames.csv'
#   file: name of file to save to
#   row.names: Whether or not to include row names (row numbers if no name assigned) in output file
write.csv(top_10_girl_names_for_csv, file="Top_10_Popular_Girl_Names_2015-2016.csv", row.names=FALSE)
```

## 4. Link to GitHub repo for Homework 5: https://github.com/danageis/SMU-MSDS-Homework/tree/master/Homework/Unit_5
