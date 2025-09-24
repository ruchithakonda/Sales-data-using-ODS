# Sales-data-using-ODS
The sales data was extracted from Kaggle .In this dataset I used the SAS ODS Templates,title,footnotes,labels,annotations and showing charts.Here i used sales by region for bar chart and  slaes over time for line chart.I used template as define style outtemp.

/ Import  the data from csv*/
proc import datafile="C:\yourpath\sales_data.csv" 
    out=salesdata
    dbms=csv
    replace;
    getnames=yes;
run;

/*Using contents procedure */
proc contents data=salesdata; run;
proc print data=salesdata(obs=10); run;

/* Sales Trend over Time (Line Chart) */
ods pdf file="C:\yourpath\sales_summary.pptx" style=outtemp; 

/* Line chart for sales trend */
title "Sales Trend Over Time";
proc sgplot data=salesdata;
    series x=Date y=Sales / lineattrs=(thickness=2 color=blue);
    xaxis label="Date" grid;
    yaxis label="Sales Amount" grid;
    keylegend / position=bottom;
run;

/* Regional Sales Comparison (Bar Chart) */
title "Total Sales by Region";
proc sgplot data=salesdata;
    vbar Region / response=Sales stat=sum datalabel
        fillattrs=(color=cx1f77b4); /* Clean single color */
    xaxis label="Region";
    yaxis label="Total Sales";
run;

/*Highlight Key Takeaways using Annotations */
title "High Sales Regions Identified";
proc sgplot data=salesdata;
    vbar Region / response=Sales stat=sum datalabel;
    refline 100000 / axis=y lineattrs=(pattern=shortdash color=red thickness=2)
        label="Target Sales";
run;

/*  Add context using footnotes */
footnote "Note: Target line represents business goal of 100,000";

/*Summary Insight Slide */
title "Business Insights Summary";
proc odstext;
    p "1. Sales show consistent growth over months.";
    p "2. Region A & C are top performers, exceeding targets.";
    p "3. Region D requires strategic focus to improve.";
run;

ods pdf  close;
