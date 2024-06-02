# GettingAndCleaningDataFinal

Final Project for Getting and Cleaning Data Course
==================================================

In this project, we used the HAR Dataset (Human Acitivty Recognition Using Smartphones) available on UCI's Machine Learning Repository. For this project, I collated the available data, cleaned it, and ran numerical analysis on it to derive the necessary tidy data. The following criteria for the project was as follows:

* The purpose of this project is to demonstrate your ability to collect, work with, and clean a data set.
  1. The submitted data set is tidy. 
  2. The Github repo contains the required scripts.
  3. GitHub contains a code book that modifies and updates the available codebooks with the data to indicate all the variables and summaries calculated, along with units,   and any other relevant information.
  4. The README that explains the analysis files is clear and understandable.
  5. The work submitted for this project is the work of the student who submitted it.
  
==================================================

Script Explanation
==================================================

In this repository, there are 2 scripts that operate on the HAR Dataset: "run_analysis.R" and "create_table.R". 

## run_analysis.R:

This is the main script of this project. It performs several tasks:

1. Loads in the libraries **dplyr, readr, tidyr** to conduct data analysis/cleaning.
2. Sources the locations of the test/train data, the corresponding participant and activity data, and feature vectors labels and sets the corresponding working directory. It also saves a copy of old working directory for repeated executions of the script.
3. With the data sources, runs **create_table** with the test/train data, the corresponding participant and activity data. The first execution is for obtaining the cleaned and labeled testing data-frame; the second execution is for the training data-frame. *(See the explanation of "create_table.R")*
4. Compiles the testing and training data-frames into a single table called **"full_data_tbl"**. 
5. Subsets **full_data_tbl** to columns where measurements in the experiment are computed using **std()** (standard deviation) or **mean()** (mean) or the participants and activity columns. Such columns are extracted using *regex*.
6. Using **sapply**, relabels the activity column of **full_data_tbl** using the **activity_labels_tbl** data-frame with a lambda function on its' first column.
7. Cleans up old, unused data and resets the working directory.
8. Converts **full_data_tbl** into a tibble-frame called **tibble_data_tbl**. Groups by **(Participant, Activity)**.
9. Using **dplyr::summarize, across, everything**, creates a tidy tibble-frame called **tidy_tbl** which contains the mean value of all measurements in **full_data_tbl** along each grouping. This was done by implementing **summarize** on **across(everything(),~mean(.x),...)** and using **across**'s *.names* parameter to appropriately label the columns of the computed data.
10. Removes **tibble_data_tbl** and views **full_data_tbl** and **tidy_data_tbl**.


## create_table.R:

This script contains a function called **"create_table"** which takes the test/train data, the corresponding activity and participant data, and feature vector labels (*in that order*) as input and outputs a properly-formatted test/train data-frame. The default feature vector labels' source is **"features.txt"**.

First, **create_table** receives the previously-named datas' sources and reads them into corresponding data-frames. 
Next, it combines the participant, activity, and test/train data into a single data-frame.
Finally, it labels the data-frame appropriately, removes previously created data-frames, and returns the collated data-frame back to the main script.