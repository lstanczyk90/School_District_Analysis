# School District Analysis

## **Overview**

The purpose of this analysis was to prepare tables for the School Board summarizing certain requested statistics for various schools. 

**NOTE**: During the analysis, it came to our attention that reading and math grades for 9th Graders at Thomas High School showed evidence of academic dishonesty. In order to provide as clear and accurate a picture as possible, the School Board asked us to modify our code and findings in order to disregard scores attributable to 9th Graders at Thomas High.

In order to accomplish this, we made the following changes:

- We first had to replace all grades for 9th Graders at Thomas High with Null Values. This was accomplished as follows:

```python
# Step 2. Use the loc method on the student_data_df to select all the reading scores from the 9th grade at Thomas High School and replace them with naN

student_data_df.loc[(student_data_df["grade"] == "9th") & (student_data_df["school_name"] == "Thomas High School"), ['reading_score']] = np.nan
   

```


```python
#  Step 3. Refactor the code in Step 2 to replace the math scores with NaN.

student_data_df.loc[(student_data_df["grade"] == "9th") & (student_data_df["school_name"] == "Thomas High School"), ['math_score']] = np.nan

```

- Next, we had to modify our student counts to exclude these students for future calculations. This was accomplished as follows:

```python
# Step 1. Get the number of students that are in ninth grade at Thomas High School.
# These students have no grades. 

thomas_ninth_count = school_data_complete_df.loc[(school_data_complete_df["school_name"] == "Thomas High School") & (school_data_complete_df["grade"] == "9th"), ["Student ID"]].count()


# Get the total student count 

student_count = school_data_complete_df["Student ID"].count()


# Step 2. Subtract the number of students that are in ninth grade at 
# Thomas High School from the total student count to get the new total student count.

new_student_count = student_count - thomas_ninth_count

```

- Lastly, we had to make modifications to the Per School Summary Table by replacing our existing calculations for Thomas High School with results that excluded 9th graders, but included all of the other grades. This was accomplished as follows:

```python
# Step 5.  Get the number of 10th-12th graders from Thomas High School (THS).

thomas_df = school_data_complete_df.loc[
    (school_data_complete_df["school_name"] == "Thomas High School") & 
    ((school_data_complete_df["grade"] == "10th") | 
    (school_data_complete_df["grade"] =="11th") | 
    (school_data_complete_df["grade"] == "12th"))]

count_thomas = thomas_df.count()["Student ID"]

count_thomas
```




    1174




```python
# Step 6. Get all the students passing math from THS

thomas_passing_math = thomas_df[(thomas_df["math_score"] >= 70)].count()["student_name"]

```


```python
# Step 7. Get all the students passing reading from THS

thomas_passing_reading = thomas_df[(thomas_df["reading_score"] >= 70)].count()["student_name"]

```


```python
# Step 8. Get all the students passing math and reading from THS

thomas_overall_passing = thomas_df[(thomas_df["math_score"] >= 70) & (thomas_df["reading_score"] >= 70)].count()["student_name"]


```


```python
# Step 9. Calculate the percentage of 10th-12th grade students passing math from Thomas High School. 

thomas_math_percentage = float(thomas_passing_math) / float(count_thomas) * 100
```


```python
# Step 10. Calculate the percentage of 10th-12th grade students passing reading from Thomas High School.

thomas_reading_percentage = float(thomas_passing_reading) / float(count_thomas) * 100
```


```python
# Step 11. Calculate the overall passing percentage of 10th-12th grade from Thomas High School. 

thomas_overall_percentage = float(thomas_overall_passing) / float(count_thomas) * 100
```


```python
# Step 12. Replace the passing math percent for Thomas High School in the per_school_summary_df.

per_school_summary_df.loc[["Thomas High School"], ["% Passing Math"]] = thomas_math_percentage
```


```python
# Step 13. Replace the passing reading percentage for Thomas High School in the per_school_summary_df.

per_school_summary_df.loc[["Thomas High School"], ["% Passing Reading"]] = thomas_reading_percentage
```


```python
# Step 14. Replace the overall passing percentage for Thomas High School in the per_school_summary_df.

per_school_summary_df.loc[["Thomas High School"], ["% Overall Passing"]] = thomas_overall_percentage
```



## Results

What follows is a before and after comparison of our results using the changes noted above:

- Overall Summary of Total Students, Total Budget, Average Scores, and Passing Rates for All Schools Within the District:

    **Old Table**

    ![alt text](https://github.com/lstanczyk90/School_District_Analysis/blob/1dbc5341310fb481ff3a6e63157637a235e7bf51/Resources/District%20Summary%20Old.PNG)

    **New Table**

    ![alt text](https://github.com/lstanczyk90/School_District_Analysis/blob/1dbc5341310fb481ff3a6e63157637a235e7bf51/Resources/District%20Summary%20New.PNG)

- Per-School Summary (Top 5 Schools) of Total Students, Total Budget, Per Student Budget, Average Scores and Passing Rates: 

    **Old Table**

    ![alt text](https://github.com/lstanczyk90/School_District_Analysis/blob/71ad5295ea136cd766f5a515c6390495ca753403/Resources/Top%20Schools%20Old.PNG)

    **New Table**

    ![alt text](https://github.com/lstanczyk90/School_District_Analysis/blob/1dbc5341310fb481ff3a6e63157637a235e7bf51/Resources/Top%20Schools%20New.PNG)

- Table of Math Scores By Grade:

    **Old Table**

    ![alt text](https://github.com/lstanczyk90/School_District_Analysis/blob/1dbc5341310fb481ff3a6e63157637a235e7bf51/Resources/Math%20Scores%20Old.PNG)

    **New Table**

    ![alt text](https://github.com/lstanczyk90/School_District_Analysis/blob/1dbc5341310fb481ff3a6e63157637a235e7bf51/Resources/Math%20Scores%20New.PNG)

- Table of Reading Scores By Grade:

    **Old Table**

    ![alt text](https://github.com/lstanczyk90/School_District_Analysis/blob/1dbc5341310fb481ff3a6e63157637a235e7bf51/Resources/Reading%20Scores%20Old.PNG)

    **New Table**

    ![alt text](https://github.com/lstanczyk90/School_District_Analysis/blob/1dbc5341310fb481ff3a6e63157637a235e7bf51/Resources/Reading%20Scores%20New.PNG)

- Table of Average Scores and Passing Rates Based on Per Capita Spending

    **Old Table**

    ![alt text](https://github.com/lstanczyk90/School_District_Analysis/blob/1dbc5341310fb481ff3a6e63157637a235e7bf51/Resources/Per%20Capita%20Old.PNG)

    **New Table**

    ![alt text](https://github.com/lstanczyk90/School_District_Analysis/blob/1dbc5341310fb481ff3a6e63157637a235e7bf51/Resources/Per%20Capita%20New.PNG)

- Table of Average Scores and Passing Rates Based on School Size


    **Old Table**

    ![alt text](https://github.com/lstanczyk90/School_District_Analysis/blob/1dbc5341310fb481ff3a6e63157637a235e7bf51/Resources/School%20Size%20Old.PNG)

    **New Table**

    ![alt text](https://github.com/lstanczyk90/School_District_Analysis/blob/1dbc5341310fb481ff3a6e63157637a235e7bf51/Resources/School%20Size%20New.PNG)

- Table of Average Scores and Passing Rates Based on School Type

    **Old Table**

    ![alt text](https://github.com/lstanczyk90/School_District_Analysis/blob/1dbc5341310fb481ff3a6e63157637a235e7bf51/Resources/School%20Type%20Old.PNG)

    **New Table**

    ![alt text](https://github.com/lstanczyk90/School_District_Analysis/blob/1dbc5341310fb481ff3a6e63157637a235e7bf51/Resources/School%20Type%20New.PNG)


## **Summary**

As you can tell from the before and after images above, there were no significant changes once we modified the scores between to account for the aforementioned academic dishonesty. 

Perhaps the biggest change was for both the Math Scores table and Reading Scores table above. As you can see in the images, 9th Grade Scores for Thomas High School are showing up as NAn, as these are being excluded from the analysis per the above. In the old version, average Math Scores for 9th Graders and Thomas High were 83.6, and average Reading Scores were 83.7.

Additionally, there were very minor changes in the District Summary report (the first one discussed in the bullets above). Specifically, under the original analysis, the average math and reading scores were 79.0 and 81.9, respectively. Under the new analysis, Math Scores changed slightly to 78.9. Again, due to rounding, the impact is negligible. 

Lastly, whereas Thomas High's position as the number two performing overall school did not change, some of the numbers (as evidenced in the image above) changed. As an example, whereas the previous Overall Passing Percentage was 90.48012%, the new percentage became 90.630324. There were similar changes in every category.