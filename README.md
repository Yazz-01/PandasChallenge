# Schools_Analysis_Pandas

1. About the Poject
Information from Schools and Students was merged to perform a global analysis of students performance at diferent sizes, types and budget of schools.
Analysis of key indicators helped to identify relevant behavior of students and trends within schools.

2. Prerequisites
Use python version 3, libraries required are numpy, pandas and io using jupyter notebooks.
import pandas as pd
import numpy as np
from io import StringIO

3. Built with
This project used two databases as csv files which were merged into one file analysed in python using jupyter notebook (ipnb).

school_data_to_load = "Resources/schools_complete.csv"
student_data_to_load = "Resources/students_complete.csv"

Read School and Student Data File and store into Pandas DataFrames
school_df = pd.read_csv(school_data_to_load)
student_df = pd.read_csv(student_data_to_load)

Combine the data into a single dataset.  
school_student_df = pd.merge(student_df, school_df, how="left", on=["school_name", "school_name"])

4. Data Analysis

Calculating parameters for the summary

#Calculate the total number of schools
total_schools=school_df['school_name'].count()

#Calculate the total number of students
tot_students= student_df['student_name'].count()

#Calculate the total budget per school
total_budget= school_df["budget"].sum()

#Calculate the average math score 
average_math_score=student_df["math_score"].mean()

#Calculate the average reading score
average_reading_score=student_df["reading_score"].mean()

#Calculate the number of students with a passing math score
students_pass_math_score= school_student_df.loc[school_student_df["math_score"]>=70] 
#Calculate the percentage of students with a passing math score
perc_stu_pass_math= len(students_pass_math_score)/tot_students *100

#Calculate the number of students with a passing reading score
students_pass_reading_score=school_student_df.loc[school_student_df["reading_score"]>=70]
#Calculate the percentage of students with a passing reading score
perc_stu_pass_reading= len(students_pass_reading_score)/tot_students *100

#Calculate the percentage of students who passed math and reading (% Overall Passing)
overall_pass_math_reading= school_student_df.loc[(school_student_df["math_score"]>=70) &(school_student_df["reading_score"]>=70)].count()["student_name"]
overall_pass_grade=overall_pass_math_reading/tot_students*100


Create a dataframe to hold the above results
dist_summary=pd.DataFrame({
    "Total Schools":[total_schools],"Total Students":[tot_students],
    "Total Budget":[total_budget],"Average Math Score": [average_math_score],"Average Reading Score": [average_reading_score],
    "Passing Math %": [perc_stu_pass_math],"Passing Reading %": [perc_stu_pass_reading], "Overall Passing %":[overall_pass_grade]})
dist_summary

Improve formatting before outputting
dist_summary["Total Budget"] = dist_summary["Total Budget"].map("${0:,.2f}".format)
dist_summary["Total Students"] = dist_summary["Total Students"].map("{0:,.0f}".format)
dist_summary

4. 2
----- School Summary

school_index= school_df.set_index('school_name')

Groupby function on schools
school_stus_index= school_stus_df.set_index('school_name')
school_group= school_stus_index.groupby(['school_name'])
school_group.head(10)

#Calculating parameters for the summary
School Type (District per school)
school_type=school_index["type"] 

Total_Students per school
total_students= school_index["size"]

Total Budget per School
school_budget=school_index["budget"] 

Per student Budget
student_budget= school_budget/total_students

Averages using Groupby on the reading score per school
average_reading_score = school_group["reading_score"].mean()

Averages using Groupby on the math score per school
average_math_score = school_group["math_score"].mean()

% Passing Math
passing_math= students_pass_math_score.groupby("school_name") 
perc_passing_math= passing_math["Student ID"].count()/total_students*100

% Passing Reading
passing_reading= students_pass_reading_score.groupby("school_name") 
perc_passing_reading= passing_reading["Student ID"].count()/total_students*100

% Overall Passing (The percentage of students that passed math and reading.)
Calculate the percentage of students who passed math and reading (% Overall Passing)
overall_pass_m_r= school_student_df.loc[(school_student_df["math_score"]>=70) &(school_student_df["reading_score"]>=70)]
overall_passing=overall_pass_m_r.groupby("school_name").count()["student_name"] /total_students*100

Formatting the numbers
schools_summary["Total School Budget"]= schools_summary["Total School Budget"].map("${:,.2f}".format)
schools_summary["Per Student Budget"]= schools_summary["Per Student Budget"].map("${:,.2f}".format)

School Summary DataFrame
School Summary DataFrame
schools_summary=pd.DataFrame({
    "School Type":school_type,
    "Total Students":total_students,
    "Total School Budget":school_budget,
    "Per Student Budget":student_budget,
    "Average Math Score":average_math_score,
    "Average Reading Score":average_reading_score,
    "% Passing Math":perc_passing_math,
    "% Passing Reading":perc_passing_reading,
    "Overall Passing Rate":overall_passing})

4.3-------Top Performing Schools (Sorted and displayed the top five performing schools by % overall passing.)

top_schools= schools_summary.loc[schools_summary["Overall Passing Rate"]>90]
top_schools.sort_values(["Overall Passing Rate"], ascending=False).head()

4.4----------- Bottom Performing Schools (By Passing Rate)
bottom_perf_schools= schools_summary.loc[schools_summary["Overall Passing Rate"]<75]
bottom_perf_schools.sort_values(["Overall Passing Rate"], ascending=True).head()




4.5------- Math Scores by Grade
First using loc to filter per grade, then groupby per school
Second getting column of math score 

9th grade
nineth= student_df.loc[student_df["grade"]=="9th"].groupby(["school_name"])
nineth_math_score=nineth["math_score"].mean()

10th grade
tenth= student_df.loc[student_df["grade"]=="10th"].groupby(["school_name"])
tenth_math_score=tenth["math_score"].mean()

11th grade
eleventh= student_df.loc[student_df["grade"]=="11th"].groupby(["school_name"])
eleventh_math_score=eleventh["math_score"].mean()

12th grade
twelveth= student_df.loc[student_df["grade"]=="12th"].groupby(["school_name"])
twelveth_math_score=twelveth["math_score"].mean()

Creating summary of Math scores by Grade for students of each grade level (9th, 10th, 11th, 12th) at each school.
math_score_grade= pd.DataFrame({"9th":nineth_math_score, "10th":tenth_math_score,
                   "11th":eleventh_math_score, "12th":twelveth_math_score
                    }, columns=["9th","10th","11th","12th"])
math_score_grade.head(15)



4.6------ Reading Scores by Grade
First using loc to filter per grade, then groupby per school
Second getting column of reading score

9th grade
nineth= student_df.loc[student_df["grade"]=="9th"].groupby(["school_name"])
nineth_read_score=nineth["reading_score"].mean()

10th grade
tenth= student_df.loc[student_df["grade"]=="10th"].groupby(["school_name"])
tenth_read_score=tenth["reading_score"].mean()

11th grade
eleventh= student_df.loc[student_df["grade"]=="11th"].groupby(["school_name"])
eleventh_read_score=eleventh["reading_score"].mean()

12th grade
twelveth= student_df.loc[student_df["grade"]=="12th"].groupby(["school_name"])
twelveth_read_score=twelveth["reading_score"].mean()




4.7 Creating a table that lists the average Reading Score for students of each grade level (9th, 10th, 11th, 12th) at each school.
read_score_grade= pd.DataFrame({"9th":nineth_read_score, "10th":tenth_read_score,
                   "11th":eleventh_read_score, "12th":twelveth_read_score
                    }, columns=["9th","10th","11th","12th"])
read_score_grade.head(15)

4.8 Scores by School Spending

Binning 
spen_ran_bins= [0,585,630,645,680 ]
spen_ran_labels= ["<$585","$585-630","$630-645","$645-680"]
spend_ranges_df=schools_summary

Creting binns for rthe DataFrame
spend_ranges_df["Spending Ranges(Per Student)"]= pd.cut(student_budget,spen_ran_bins,labels=spen_ran_labels)


Spending Ranges(Per Student) Groupping by our bins

spent_math= spend_ranges_df.groupby("Spending Ranges(Per Student)").mean()["Average Math Score"]

per_math= spend_ranges_df.groupby("Spending Ranges(Per Student)").mean()["% Passing Math"]

per_reading= spend_ranges_df.groupby("Spending Ranges(Per Student)").mean()["% Passing Reading"]

spent_reading= spend_ranges_df.groupby("Spending Ranges(Per Student)").mean()["Average Reading Score"]

spent_overall= spend_ranges_df.groupby("Spending Ranges(Per Student)").mean()["Overall Passing Rate"]


Create a table that breaks down school performances based on average Spending Ranges (Per Student). Four reasonable bins were created to group school spending.
school_spending_summary=pd.DataFrame({"Average Math Score":spent_math,
                                      "Average Reading Score":spent_reading,
                                      "% Passing Math":per_math,
                                      "% Passing Reading":per_reading,
                                      "Overall Passing Rate":spent_overall},
                                    columns=["Average Math Score","Average Reading Score",
                                    "% Passing Math","% Passing Reading","Overall Passing Rate"])

Improve formatting before outputting
school_spending_summary["Average Math Score"]=school_spending_summary["Average Math Score"].map("{0:,.2f}".format)
school_spending_summary["Average Reading Score"]=school_spending_summary["Average Reading Score"].map("{0:,.2f}".format)
school_spending_summary["% Passing Math"]=school_spending_summary["% Passing Math"].map("{0:,.2f}".format)
school_spending_summary["% Passing Reading"]=school_spending_summary["% Passing Reading"].map("{0:,.2f}".format)
school_spending_summary["Overall Passing Rate"]=school_spending_summary["Overall Passing Rate"].map("{0:,.2f}".format)


4.9 Scores by School Size
Binning 
bins= [0,1000,2000,5000]
labels= ["Small (<1000)","Medium (1000-2000)","Large (2000-5000)"]

DataFrame
size_ranges= schools_summary

Creting binns for rthe DataFrame
size_ranges["School Size"]= pd.cut(total_students,bins,labels=labels)

Scores by School Size, Groupping by our bins

ave_math= size_ranges.groupby("School Size").mean()["Average Math Score"]

ave_reading= size_ranges.groupby("School Size").mean()["Average Reading Score"]

size_per_math= size_ranges.groupby("School Size").mean()["% Passing Math"]

size_per_reading= size_ranges.groupby("School Size").mean()["% Passing Reading"]

size_per_overall= size_ranges.groupby("School Size").mean()["Overall Passing Rate"]

score_school_size_summary=pd.DataFrame({"Average Math Score":ave_math,
                                      "Average Reading Score":ave_reading,
                                      "% Passing Math":size_per_math,
                                      "% Passing Reading":size_per_reading,
                                      "Overall Passing Rate":size_per_overall},
                                    columns=["Average Math Score","Average Reading Score",
                                    "% Passing Math","% Passing Reading","Overall Passing Rate"])


4.10. Scores by School Type

DataFrame with information to use
type_school= schools_summary

Creting the parameters for the Summary of Scores by School Type

ave_math_typ= type_school.groupby("School Type").mean()["Average Math Score"]

ave_reading_type= type_school.groupby("School Type").mean()["Average Reading Score"]

per_math= type_school.groupby("School Type").mean()["% Passing Math"]

per_reading= type_school.groupby("School Type").mean()["% Passing Reading"]

per_overall= type_school.groupby("School Type").mean()["Overall Passing Rate"]

score_school_type_summary=pd.DataFrame({"Average Math Score":ave_math_typ,
                                      "Average Reading Score":ave_reading_type,
                                      "% Passing Math":per_math,
                                      "% Passing Reading":per_reading,
                                      "Overall Passing Rate":per_overall},
                                    columns=["Average Math Score","Average Reading Score",
                                    "% Passing Math","% Passing Reading","Overall Passing Rate"])

Printing the summary
score_school_type_summary


5. Conclusions and trends identified:

First, students performed better at reading than at math subject considering an overall passing rate higher than 70. The average reading score is higher than math as from the top performing scores reading score goes from 84.04 to 83.97 as the lowest whereas the best math score goes from 83.83 to 83.06. This information ilustrates strudents generally performed better at reading than at math subject. 

Also, the top performing schools were charter schools were the district schools where at the bottom performing schools. The average Math score goes from 77.07 to 76.84 and the Reading score from 80.96 to 80.74. 


However, deeper financial analysis undercover that lower the schools spending per students performed better at Math and Reading. Overall passing rate conveying Math and Reading scores was 90.37 % in schools with spending ranges < $585. However, overall passing rate in shools with spending range per student form  $645 to $680 was 53.53. 

Finally, it can be concluded that students enrolled in charter schools perfomed better at Math and Reading than students enrolled at District schools. Schools with more spending per student performed worse at these subjects. 


