# pandas-challenge
Assignment 4: pandas-challenge
All Codes Used
-----------------------------------------------
# Dependencies and Setup
import pandas as pd
from pathlib import Path

# File to Load (Remember to Change These)
school_data_to_load = Path("Resources/schools_complete.csv")
student_data_to_load = Path("Resources/students_complete.csv")

# Read School and Student Data File and store into Pandas DataFrames
school_data = pd.read_csv(school_data_to_load)
student_data = pd.read_csv(student_data_to_load)

# Combine the data into a single dataset.  
school_data_complete = pd.merge(student_data, school_data, how="left", on=["school_name", "school_name"])
school_data_complete.head()
# Calculate the total number of unique schools
school_count = len(school_data_complete["school_name"].unique())
school_count
# Calculate the total number of students
student_count = len(school_data_complete["Student ID"])
student_count
# Calculate the total budget
total_budget = (school_data["budget"]).sum()
total_budget
# Calculate the average (mean) math score
average_math_score = (school_data_complete["math_score"]).mean()
average_math_score
# Calculate the average (mean) reading score
average_reading_score = (school_data_complete["reading_score"]).mean()
average_reading_score
# Use the following to calculate the percentage of students who passed math (math scores greather than or equal to 70)
passing_math_count = school_data_complete[(school_data_complete["math_score"] >= 70)].count()["student_name"]
passing_math_percentage = passing_math_count / float(student_count) * 100
passing_math_percentage
# Calculate the percentage of students who passeed reading (hint: look at how the math percentage was calculated)  
passing_reading_count = school_data_complete[(school_data_complete["reading_score"] >= 70)].count()["student_name"]
passing_reading_percentage = passing_reading_count / float(student_count) * 100
passing_reading_percentage
# Use the following to calculate the percentage of students that passed math and reading
passing_math_reading_count = school_data_complete[
    (school_data_complete["math_score"] >= 70) & (school_data_complete["reading_score"] >= 70)
].count()["student_name"]
overall_passing_rate = passing_math_reading_count /  float(student_count) * 100
overall_passing_rate
# Create a high-level snapshot of the district's key metrics in a DataFrame
district_summary = pd.DataFrame ({"Total number of unique schools" : [school_count],
                                  "Total students" : [student_count],
                                  "Total budget" : [total_budget],
                                  "Average math score" : [average_math_score],
                                  "Average reading score" : [average_reading_score],
                                  "% passing math" : [passing_math_percentage],
                                  "% passing reading" : [passing_reading_percentage],
                                  "% overall passing" : [overall_passing_rate]})
# BackUp & Copy
district_summary_copy = district_summary.copy()

# Formatting
district_summary_copy["Total students"] = district_summary_copy["Total students"].map("{:,}".format)
district_summary_copy["Total budget"] = district_summary_copy["Total budget"].map("${:,.2f}".format)
district_summary_copy["Average math score"] = district_summary_copy["Average math score"].map("{:,.6f}".format)
district_summary_copy["Average reading score"] = district_summary_copy["Average reading score"].map("{:,.6f}".format)
district_summary_copy["% passing math"] = district_summary_copy["% passing math"].map("{:,.3f}%".format)
district_summary_copy["% passing reading"] = district_summary_copy["% passing reading"].map("{:,.3f}%".format)
district_summary_copy["% overall passing"] = district_summary_copy["% overall passing"].map("{:,.3f}%".format)

# Display the DataFrame
district_summary_copy
# Use the code provided to select the school type
school_types = school_data.set_index(["school_name"])["type"]
school_types
# Calculate the total student count
per_school_counts = school_data_complete["school_name"].value_counts()
per_school_counts
# Calculate the total school budget and per capita spending
per_school_budget = school_data_complete.groupby(["school_name"])["budget"].mean()
per_school_capita = per_school_budget / per_school_counts
per_school_capita
# Calculate the average test scores
per_school_math = school_data_complete.groupby(["school_name"])["math_score"].mean()
per_school_reading = school_data_complete.groupby(["school_name"])["reading_score"].mean()
#Print to Ensure Accurate Reading (Math)
print(per_school_math)
#Print to Ensure Accurate Reading (Reading)
print(per_school_reading)
# Calculate the number of schools with math scores of 70 or higher
school_passing_math = school_data_complete[school_data_complete["math_score"]>=70]
#Print to Ensure Accurate Reading (Math)
print(school_passing_math)
# Calculate the number of schools with reading scores of 70 or higher
school_passing_reading = school_data_complete[school_data_complete['reading_score']>=70]
#Print to Ensure Accurate Reading (Reading)
print(school_passing_reading)
# Use the provided code to calculate the schools that passed both math and reading with scores of 70 or higher
passing_math_and_reading = school_data_complete[
    (school_data_complete["reading_score"] >= 70) & (school_data_complete["math_score"] >= 70)
]
#Print to Ensure Accurate Data
print(passing_math_and_reading)
# Use the provided code to calculate the passing rates
per_school_passing_math = school_passing_math.groupby(["school_name"]).count()["student_name"] / per_school_counts * 100
per_school_passing_reading = school_passing_reading.groupby(["school_name"]).count()["student_name"] / per_school_counts * 100
overall_passing_rate = passing_math_and_reading.groupby(["school_name"]).count()["student_name"] / per_school_counts * 100
#Print to Ensure Accurate Data
print(per_school_passing_math)
print(per_school_passing_reading)
print(overall_passing_rate)
# Create a DataFrame called `per_school_summary` with columns for the calculations above.
per_school_summary = pd.DataFrame ({"School Type" : school_types, 
                                  "Total Students" : per_school_counts,
                                  "Total School Budget" : per_school_budget,
                                  "Per Student Budget" : per_school_capita,
                                  "Average Math Score" : per_school_math,
                                  "Average Reading Score" : per_school_reading,
                                  "% Passing Math" : per_school_passing_math,
                                  "% Passing Reading" : per_school_passing_reading,
                                  "% Overall Passing" : overall_passing_rate})
# BackUp & Copy
per_school_summary_copy = per_school_summary.copy()

# Formatting
per_school_summary_copy["Total School Budget"] = per_school_summary_copy["Total School Budget"].map("${:,.2f}".format)
per_school_summary_copy["Per Student Budget"] = per_school_summary_copy["Per Student Budget"].map("${:,.2f}".format)
per_school_summary_copy["Average Math Score"] = per_school_summary_copy["Average Math Score"].map("{:,.6f}".format)
per_school_summary_copy["Average Reading Score"] = per_school_summary_copy["Average Reading Score"].map("{:,.6f}".format)
per_school_summary_copy["% Passing Math"] = per_school_summary_copy["% Passing Math"].map("{:,.3f}%".format)
per_school_summary_copy["% Passing Reading"] = per_school_summary_copy["% Passing Reading"].map("{:,.3f}%".format)
per_school_summary_copy["% Overall Passing"] = per_school_summary_copy["% Overall Passing"].map("{:,.3f}%".format)

# Display the DataFrame
per_school_summary_copy
# Sort the schools by `% Overall Passing` in descending order and display the top 5 rows.
top_schools = per_school_summary.sort_values("% Overall Passing", ascending = False)
top_schools.head(5)
# Sort the schools by `% Overall Passing` in ascending order and display the top 5 rows.
bottom_schools = per_school_summary.sort_values("% Overall Passing", ascending = True)
bottom_schools.head(5)
# Use the code provided to separate the data by grade
ninth_graders = school_data_complete[(school_data_complete["grade"] == "9th")]
tenth_graders = school_data_complete[(school_data_complete["grade"] == "10th")]
eleventh_graders = school_data_complete[(school_data_complete["grade"] == "11th")]
twelfth_graders = school_data_complete[(school_data_complete["grade"] == "12th")]
# Group by "school_name" and take the mean of each & Add only Math Score
ninth_graders_math_scores = ninth_graders.groupby(["school_name"])["math_score"].mean()
tenth_graders_math_scores = tenth_graders.groupby(["school_name"])["math_score"].mean()
eleventh_graders_math_scores = eleventh_graders.groupby(["school_name"])["math_score"].mean()
twelfth_graders_math_scores = twelfth_graders.groupby(["school_name"])["math_score"].mean()

# Combine each of the scores above into single DataFrame called `math_scores_by_grade`
math_scores_by_grade = pd.DataFrame ({"9th" : ninth_graders_math_scores, 
                                      "10th" : tenth_graders_math_scores,
                                      "11th" : eleventh_graders_math_scores,
                                      "12th" : twelfth_graders_math_scores})
# Minor data wrangling
math_scores_by_grade.index.name = None

# Display the DataFrame
math_scores_by_grade
# Use the code provided to separate the data by grade
ninth_graders = school_data_complete[(school_data_complete["grade"] == "9th")]
tenth_graders = school_data_complete[(school_data_complete["grade"] == "10th")]
eleventh_graders = school_data_complete[(school_data_complete["grade"] == "11th")]
twelfth_graders = school_data_complete[(school_data_complete["grade"] == "12th")]
# Group by "school_name" and take the mean of each & Add only Math Score
ninth_graders_reading_scores = ninth_graders.groupby(["school_name"])["reading_score"].mean()
tenth_graders_reading_scores = tenth_graders.groupby(["school_name"])["reading_score"].mean()
eleventh_graders_reading_scores = eleventh_graders.groupby(["school_name"])["reading_score"].mean()
twelfth_graders_reading_scores = twelfth_graders.groupby(["school_name"])["reading_score"].mean()

# Combine each of the scores above into single DataFrame called `math_scores_by_grade`
reading_scores_by_grade = pd.DataFrame ({"9th" : ninth_graders_reading_scores, 
                                      "10th" : tenth_graders_reading_scores,
                                      "11th" : eleventh_graders_reading_scores,
                                      "12th" : twelfth_graders_reading_scores})
# Minor data wrangling
reading_scores_by_grade = reading_scores_by_grade[["9th", "10th", "11th", "12th"]]
reading_scores_by_grade.index.name = None

# Display the DataFrame
reading_scores_by_grade
# Establish the bins 
spending_bins = [0, 585, 630, 645, 680]
spending_labels = ["<$585", "$585-630", "$630-645", "$645-680"]
spending_bins
# Create a copy of the school summary since it has the "Per Student Budget" 
school_spending_df = per_school_summary.copy()
school_spending_df
# Use `pd.cut` to categorize spending based on the bins.
school_spending_df["Spending Ranges (Per Student)"] = pd.cut(school_spending_df["Per Student Budget"],
                                                             spending_bins, labels=spending_labels)
school_spending_df
#  Calculate averages for the desired columns. 
spending_math_scores = school_spending_df.groupby(["Spending Ranges (Per Student)"])["Average Math Score"].mean()
spending_reading_scores = school_spending_df.groupby(["Spending Ranges (Per Student)"])["Average Reading Score"].mean()
spending_passing_math = school_spending_df.groupby(["Spending Ranges (Per Student)"])["% Passing Math"].mean()
spending_passing_reading = school_spending_df.groupby(["Spending Ranges (Per Student)"])["% Passing Reading"].mean()
overall_passing_spending = school_spending_df.groupby(["Spending Ranges (Per Student)"])["% Overall Passing"].mean()
# Display Data
spending_math_scores
# Display Data
spending_reading_scores
# Display Data
spending_passing_math
# Display Data
spending_passing_reading
# Display Data
overall_passing_spending
# Assemble into DataFrame
spending_summary = reading_scores_by_grade = pd.DataFrame ({"Average Math Score" : spending_math_scores, 
                                                            "Average Reading Score" : spending_reading_scores,
                                                            "% Passing Math" : spending_passing_math,
                                                            "% Passing Reading" : spending_passing_reading,
                                                            "% Overall Passing" : overall_passing_spending})
# Display results
spending_summary
# Establish the bins.
size_bins = [0, 1000, 2000, 5000]
size_labels = ["Small (<1000)", "Medium (1000-2000)", "Large (2000-5000)"]
# Categorize the spending based on the bins
# Use `pd.cut` on the "Total Students" column of the `per_school_summary` DataFrame.
school_size_df = per_school_summary.copy()
per_school_summary["School Size"] = pd.cut(school_size_df["Total Students"], size_bins, labels=size_labels)
per_school_summary
# Calculate averages for the desired columns. 
size_math_scores = per_school_summary.groupby(["School Size"])["Average Math Score"].mean()
size_reading_scores = per_school_summary.groupby(["School Size"])["Average Reading Score"].mean()
size_passing_math = per_school_summary.groupby(["School Size"])["% Passing Math"].mean()
size_passing_reading = per_school_summary.groupby(["School Size"])["% Passing Reading"].mean()
size_overall_passing = per_school_summary.groupby(["School Size"])["% Overall Passing"].mean()
# Display Data
size_math_scores
# Display Data
size_reading_scores
# Display Data
size_passing_math
# Display Data
size_passing_reading
# Display Data
size_overall_passing
# Create a DataFrame called `size_summary` that breaks down school performance based on school size (small, medium, or large).
# Use the scores above to create a new DataFrame called `size_summary`
size_summary = pd.DataFrame ({"Average Math Score" : size_math_scores,
                              "Average Reading Score" : size_reading_scores,
                              "% Passing Math" : size_passing_math,
                              "% Passing Reading" : size_passing_reading,
                              "% Overall Passing" : size_overall_passing})
# Display results
size_summary
# Group the per_school_summary DataFrame by "School Type" and average the results.
type_math_scores = per_school_summary.groupby(["School Type"])["Average Math Score"].mean()
type_reading_scores = per_school_summary.groupby(["School Type"])["Average Reading Score"].mean()
type_passing_math = per_school_summary.groupby(["School Type"])["% Passing Math"].mean()
type_passing_reading = per_school_summary.groupby(["School Type"])["% Passing Reading"].mean()
type_overall_passing = per_school_summary.groupby(["School Type"])["% Overall Passing"].mean()
# Display Data
type_math_scores
# Display Data
type_reading_scores
# Display Data
type_passing_math
# Display Data
type_passing_reading
# Display Data
type_overall_passing
# Assemble the new data by type into a DataFrame called `type_summary`
type_summary = pd.DataFrame ({"Average Math Score" : type_math_scores, 
                              "Average Reading Score" : type_reading_scores,
                              "% Passing Math" : type_passing_math,
                              "% Passing Reading" : type_passing_reading,
                              "% Overall Passing" : type_overall_passing})
# Display results
type_summary
