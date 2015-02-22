# GettingAndCleaningWeekk3Assignment

This is a readme describing the process I used for the completion of the getting and cleaning data course project .

The first section of the code is where the data is sourced. It was downloaded from the commented out url in line 1 then manually unzipped and placed in a folder at the same level as the RStudio project.

The next section (lines 2- 34) was the process for importing the test data, relabelling the column headers then combining into a single dataframe.
The features data frame (lables taken from the features.txt file) has the extra punctuation "-", "(" & "(" are removed using the gsub command. this vector is then used as the names for the data set.

The import process was repeated in the next section (lines 36-60) to import and process the training data.

Using factor replacement the numeric class identifiers are replaced with the descriptive name from the activity_lables data.frame. This column is then relabeled to "activity".

The columns selected are any columns with either "mean" or "std" in the variable name. the variable name is listed in the features.txt file that came in the source data. This data set will mean any spacial information for each of the sensor types can be analysed.

All extra data and varibles are then removed leaving only the tidydata frame left for summary analysis.

the tidydata frame is then summarised by each activity factor for each column using the data.table package. This summary table is then written to a .txt file for uploading for the final submission.

To read the file back into R use the code:

```{r}
read.table("tidysummary.txt", sep = " ", header =T, row.names=F)
```
