---
layout: post
Title: Data Processing for NC School Support Services Report
category: coursework
---

# Data Dictionary

Statistical Profiles are provided by the NC State Board of Education and NC Department of Public Instruction.

LEA Personnel Summary data was retrieved from the [Statistical Profile site](http://apps.schools.nc.gov/ords/f?p=145:110:::NO) on April 18, 2018. I filtered for only records from 2018 and downloaded the data to a file named LEA_2018_PersonnelSummary.csv. 

Field Name | Data Type | Description
:---: | :---: | :---:
Year | text | Year that listed personnel are funded
LEA | text | 3-character code for the Local Education Agency (school district)
LEA Name | text | Full name for the Local Education Agency
Position | text | Type of personnel, i.e. Teachers, Professionals, Administrators, Others
Assignment Classification | text | Description of personnel, i.e. Guidance, Psychological
State Fund | integer | Count of personnel funded by North Carolina
Federal Fund | integer | Count of personnel funded by federal 
Local Fund | integer | Count of personnel funded by local
Total Fund | integer | Total count of personnel funded at LEA for assignment classification
Gender Male | integer | Count of male personnel
Gender Female | integer | Count of female personnel
Gender White | integer | Count of white personnel
Gender Black | integer | Count of black personnel
Gender Other | integer | Count of personnel of race other than white or black


LEA Expenditures data was retrieved from the [Statistical Profile site](http://apps.schools.nc.gov/ords/f?p=145:114:::NO) on April 18, 2018. I filtered for only records from 2017 and downloaded the data to a CSV file namedLEA_2017_Expenditures. The 2017 records show expenditures for the 2017 - 2018 school year. 

Field Name | Data Type | Description
:---: | :---: | :---:
Year | text | Year that listed personnel are funded
LEA | text | 3-character code for the Local Education Agency (school district)
LEA Name | text | Full name for the Local Education Agency
Type | text | Expenditure type, i.e. Employee Benefits, Salaries, Total
Source State | integer | Funding amount from North Carolina
Source Federal | integer | Funding amount from federal 
Source Local | integer | Funding amount from local
Source Total | integer | Total funding amount at LEA for expenditure type
Pupil State | integer | Funding per student from North Carolina
Pupil Federal | integer | Funding per student from federal
Pupil Local | integer | Funding per student from local
Per Pupil Total | integer | Total funding per student
Per Pupil Percent | double | Percent of funding 


# Data Diary

For each LEA in the 2017-2018 academic year, I wanted to identify the number of students and number of student support specialists. From there, I compared the student to support specialist ratio with the nationally recommended ratios. Support specialists are broken down into different categories, and I am analyzing two categories of support: school guidance counselors and school psychologists.

### (1) Identified number of students by LEA

* Downloaded the 2 data sets described above as CSV files
* Imported 2 data sets as tables into new SQLite database named LEA_SupportServices.db
* Ran query to select only the records for Total Expenditures and inserted into a new table named LEA_2017_ExpenditureTotal. Exported this table as a CSV file.

```sql
CREATE TABLE LEA_2017_ExpenditureTotal AS
SELECT * FROM LEA_2017_Expenditures
WHERE LEA_2017_Expenditures.Type = "TOTAL";
```

* Opened the ExpenditureTotal file in Excel and created new integer field called PupilCount for the number of student at each LEA. Filled with calculated value: PupilCount = SourceTotal / PerPupilTotal and rounded to whole number. 
* Exported this as a new CSV called LEA_2017_PupilCount and imported the table into SQLite

### (2) Identified number of support specialists by LEA

* Join tables PupilCount and ExpenditureTotal and select only records for 2 types of support workers: psychological, guidance. Create new tables using this query, one for guidance counselor counts and one for school pscyhologist counts.

```sql
CREATE TABLE LEA_2018_Guidance AS
SELECT LEA_2018_PersonnelSummary.LEAName, LEA_2018_PersonnelSummary.AssignmentClassification, LEA_2018_PersonnelSummary.TotalFund,
LEA_2017_PupilCount.PupilCount
FROM LEA_2017_PupilCount
INNER JOIN LEA_2018_PersonnelSummary
ON LEA_2017_PupilCount.LEA = LEA_2018_PersonnelSummary.LEA
WHERE LEA_2018_PersonnelSummary.AssignmentClassification = ‘Guidance’;
```

* Exported new tables and opened in Excel. Created new calculated field for the ratio of students to 1 support professional. Ratio = IF( TotalFund=0 ,  “” , PupilCount/TotalFund).
* Added a record for North Carolina totals and summed count of students and support specialists.
* Saved these csv files and imported into Tableau to create visualizations

Check out my final article on this data: [Teacher protests about more than a raise](http://www.reagancline.com/coursework/2018/05/06/Data-Driven-Journalism-Teacher-Protests-About-More-Than-A-Raise.html). Special thanks to my data sherpa Bryan Noreen, data specialist at EdNC & Reach NC Voices.
