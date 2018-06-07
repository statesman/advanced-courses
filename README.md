Advanced courses
================

Something of note: The calendar year 2016-17 shows Advanced Course rates (percent of those taking advance courses / taking any course) for seperate years 2015 and 2016. This is where a call to TEA might be in order. I don't know if that is a comparison to the previous year's cohort of students, or that some tests are administered in the first calendar year and others in the second calendar year of the same school year. Or if they are retakes. IF THEY ARE DIFFERENT COHORTS, then I would just choose the most recent year for a given category. I've looked through the [TAPR Glossary](https://tea.texas.gov/WorkArea/linkit.aspx?LinkIdentifier=id&ItemID=51539617810&libID=51539617810) and I didn't see an explantion why there are two distinct years in each data set.

## Getting the data (overexplained)

This starts with [TEA TAPR data](https://tea.texas.gov/perfreport/tapr/index.html).

- Choose the year of data you want.
- Go to **Data Download**.
- Choose **TAPR Data in Excel (Rates Only)**.
- If you are interested only in Central Texas, I recommend choosing **Campuses by Region**. By doing so, you'll end up with 800+ rows instead of 8000+.
- Choose **Region 13 - Austin**. Hit **Continue** to go to the next screen.
- I would **Select All** to get all the schools in the region. It's probably easier to filter by district later than to check all the boxes you want here.
- Click on **Advanced Courses (Grades 9-12 & 11-12)** and hit **Continue**. (Or whatever program you want to look at.)
- On the file format, I'm going to recommend **Comma delimited** because it is a smaller file and perhaps easier to programattically combine with other years.
    - You _might_ be tempted to use the **Excel** version, but those files are MUCH larger and can crash Excel, especially if you have the whole state. I _was_ able to open the Region 13 file.
- Here you want to select specific data points you want to download if there are particular grades or subjects you want to study. If you **Select All*, you'll get 280+ columns of data. That's a LOT of data.
- While on this page, you will also want to click on the link **Advanced Courses (Grades 9-12 & 11-12) Reference** and save that (I saved links below.) I noticed the description for Native American changed to American Indian in 2017. The column names are similar, except for the year.
- Once you've downloaded the file, rename it with the year so you know which one it is. Move it into a folder where you can work on it.

Repeat these steps above for each year you want data. Be sure to note/save the reference files, because they are different for each year (I think).

## School names

You'l find that this data does NOT have school names, only a campus number. You have to get school directory information to get the names of the schools. This requires a JOIN of two files, and careful consideration on the CAMPUS field, because they contain _leading zeros_. If you open this file in Excel [without telling it to treat these files as text](https://rptsvr1.tea.texas.gov/perfreport/tapr/2017/xplore/taprhelp.html), you will lose those zeros and your join will not work.

School name information is available on [AskTED](http://tea4avholly.tea.state.tx.us/tea.askted.web/Forms/Home.aspx).



## Reference files:
- [2016-17](https://rptsvr1.tea.texas.gov/perfreport/tapr/2017/xplore/cadv.html)
- [2015-16](https://rptsvr1.tea.texas.gov/perfreport/tapr/2016/xplore/cadv.html)
- [2014-15](https://rptsvr1.tea.texas.gov/perfreport/tapr/2015/xplore/cadv.html)
- [2013-14](https://rptsvr1.tea.texas.gov/perfreport/tapr/2014/xplore/cothr.html)
- [2012-13](https://rptsvr1.tea.texas.gov/perfreport/tapr/2013/xplore/cothr.html)

## Data masking
- [Masking rules for 2017](https://rptsvr1.tea.texas.gov/perfreport/tapr/2017/masking.html)

TEA masks numbers where the total number of students is less than five. The link to how they do it is above. My suggestion:
- If the field has an asterisk "*", Make that a blank field instead. (Perhaps through a search and replace?) That usually means there were no students in that category.
- If the field is "-1", that means there were fewer than five students. I would make these a "1" and then explain in any analysis that it means fewer than five.
- But ... you can only use these numbers at the individual campus level. If you try to add campuses together or do math, then those numbers will be off. A BETTER IDEA is to pull **District** data to get those answers.