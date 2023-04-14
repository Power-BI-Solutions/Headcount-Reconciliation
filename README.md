# Headcount-Reconciliation <br><br/>

The aim of this project was to provide month on month comparison of Human Resources dataset called Headcount. We are interested in any changes that happened during a given month period.

The data for each month closed (snapshot of last day of month) is provided as an Excel document. 
<br><br/>

<p align="center">
<img width="650em" src="https://github.com/Power-BI-Solutions/Headcount-Reconciliation/blob/main/image/headcount.png" align = "center"/>
</p>
<br><br/>


### Set Up

To simplify data extraction, we set up a folder for Excel files that will be just simply damped in each month. We load and combine data from folder to Power BI. 
To start the process of combining files from the same folder follow these steps:
- from the menu ribbon select Get data 
- choose File > Folder
- select Connect

<p align="center">
<img width="650em" src="https://github.com/Power-BI-Solutions/Headcount-Reconciliation/blob/main/image/folder_load.png" align = "center"/>
</p>
<br><br/>


A new window called Folder will pop out:
- enter the folder path
- select OK
- choose Transform data to see the folder's files in Power Query Editor
<p align="center">
<img width="650em" src="https://github.com/Power-BI-Solutions/Headcount-Reconciliation/blob/main/image/path.png" align = "center"/>
</p>
<br><br/>

Once we are in Power Query Editor we can work with data, clean it and quality check.



