# Headcount-Reconciliation <br><br/>

The aim of this project was to provide month on month comparison of Human Resources dataset called Headcount. We are interested in any changes that happened during a given month period.

The data for each month closed (snapshot of last day of month) is provided as an Excel document. 
<br><br/>

<p align="center">
<img width="850em" src="https://github.com/Power-BI-Solutions/Headcount-Reconciliation/blob/main/image/headcount.png" align = "center"/>
</p>
<br><br/>


### Set Up

To simplify data extraction, we set up a folder for Excel files that will be just simply damped in each month. We load and combine data from folder to Power BI. 
To start the process of combining files from the same folder follow these steps:
- from the menu ribbon select Get data 
- choose File > Folder
- select Connect

<p align="center">
<img width="850em" src="https://github.com/Power-BI-Solutions/Headcount-Reconciliation/blob/main/image/folder_load.png" align = "center"/>
</p>
<br><br/>


A new window called Folder will pop out:
- enter the folder path
- select OK
- choose Transform data to see the folder's files in Power Query Editor
<p align="center">
<img width="850em" src="https://github.com/Power-BI-Solutions/Headcount-Reconciliation/blob/main/image/path.png" align = "center"/>
</p>


<br><br/>

Once we are in Power Query Editor we can work with data, clean it and check data model.
<br><br/>


### Measures

To be able to compare previous record in one line within a table we need to set up couple of measures. I will demonstrate how we create measures related to name.

Firstly we need a measure for previous name, so we can have table columns with current and previous name next to each other. We assumed that ID within our people_data table is always linked to the same employee. Below is a DAX formula for our Prev Name calculation:

```dax
Prev Name = 
VAR __prevDate = 
CALCULATE( 
    MAX(people_data[Month]), 
    FILTER( 
        ALLEXCEPT( people_data, people_data[ID] ),  
        people_data[Month] < MAX(people_data[Month] )
    )
)
RETURN
    CALCULATE( MAX( people_data[Name]), ALLEXCEPT( people_data, people_data[ID] ), people_data[Month] = __prevDate ) 
```

Next measure related to name is a name match. We will use this measure for formatting or highlighting the change on row level. I used March 2023 release of Power BI and it can format on string measures. However this might be an issue with previous versions, where this is not possible. You can get around by using integer instead of strings. So for example "Match" will be 1 and "No Match" can be assighend 0.

```dax
Match Name (flag) = 
var _NAME = SELECTEDVALUE(people_data[Name])

return
IF(_NAME = people_data[Prev Name], "Match", "No Match")
```

We have to create Prev and Match measures for all columns we wish to compare. 
After this is completed I created a mesure called Row Flag that indicates if there was any change within all compared columns.

```dax
Row Flag = 
IF(
    ([Match CC (flag)] = "No Match" || 
     [Match Manager (flag)] = "No Match" || 
     [Match Name (flag)]= "No Match" || 
     [Match Role (flag)]="No Match"), "CHANGE", "MATCH")
```
<br><br/>


### Table Formatting

Suppose that we have created all the measures required for this project. We just need to create a table, Month filter and format changes for more user fiendly insight.

Suppose that we have created our reconciliation table and month filter. Let's format changes! 
- click on table visual so you can see column manes within Visualisations panel
<p align="center">
<img width="850em" src="https://github.com/Power-BI-Solutions/Headcount-Reconciliation/blob/main/image/columns.png" align = "center"/>
</p>
<br><br/>

- within Visualisations Columns select the one you wish to format (I have selected Name)
- click on dropdonw arrow > Conditional formatting > Background color
<p align="center">
<img width="850em" src="https://github.com/Power-BI-Solutions/Headcount-Reconciliation/blob/main/image/back_format.png" align = "center"/>
</p>
<br><br/>

- a new window called Background color will pop out 
- select Format style > Rules
- select Apply to > Values only
- seect What field should we base this on? > Match Name (flag)
- set your format rule and background on
<p align="center">
<img width="850em" src="https://github.com/Power-BI-Solutions/Headcount-Reconciliation/blob/main/image/back_color.png" align = "center"/>
</p>
<br><br/>

Now we just need to repeat formatting step above for all columns we wish to be highlighted if change occurs.

We usually have more data when it comes to employees, we can use the rest of the data to create insighthfull dashboards as an addtion to reconciliation demonstrated in this project. 
<br><br/>

Author: @MBohunickaCharles
