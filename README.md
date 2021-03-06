Advanced courses
================

This is written mainly to guide Melissa Taboada in working with TAPR data about advanced placement course attendance.

The good news: I have a finished data set in `data-work/ac_any_subject.csv` for you that combines five years together, adjusts the masked data values and adds the school and district names. You can walk into your workshop with data ready to analyze. All the combining and data munging was done through programming, and you can [view on Github](https://github.com/statesman/advanced-courses/blob/master/notebooks/01_data_process.ipynb), FWIW.

However, if you want to go through all the steps, or understand the concepts of what I've done, I've written out each step with explanations, etc.

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
    - You _might_ be tempted to use the **Excel** version, but those files are MUCH larger and can crash Excel, especially if you have the whole state or all the test subjects. I _was_ able to open the Region 13 file for the "any subject-only" files.
- Here you want to select specific data points you want to download if there are particular grades or subjects you want to study. If you **Select All*, you'll get 280+ columns of data. That's a LOT of data. I might suggest using just the "any subject" versions instead of individual subjects. (You'll see below I downloaded a bunch of differen formats for you.)
- While on this page, you will also want to click on the link **Advanced Courses (Grades 9-12 & 11-12) Reference** and save that (I saved links below.) I noticed the description for Native American changed to American Indian in 2017. The column names are similar, except for the year.
- Once you've downloaded the file, rename it with the year so you know which one it is. Move it into a folder where you can work on it.

Repeat these steps above for each year you want data. Be sure to note/save the reference files, because they are different for each year (I think).

### Downloaded data
I've already downloaded all these files in a variety of formats.
- `data-raw/csv-any` has downloaded files for "Advanced Courses (Grades 9-12) - Any Subject, Percent Taking" (or "Advanced Course/Dual Enrollment, Percent Taking" for older years). This is a percentage of students taking any advanced course compared to the student body. These are the `.dat` files but are really just comma delimited.
- `data-raw/excel-any` are the same as above, but in Excel. You'll get an error prompt when opening ... just say Yes.
- `data-raw/csv-all` has the `.dat` downloaded files that includes not only the "Any subject" columns, but also the individual subjects. Many more columns of data.
- `data-raw/excel-all` same as above, but in Excel.

### Notes about the data
- There was a data format change in 2013, so 2012 is in a different format.
- 2013 is also when they started counting individual subjects, so that data does not exist for 2012.

### Column Reference files for Advance Course data:
- [2016-17](https://rptsvr1.tea.texas.gov/perfreport/tapr/2017/xplore/cadv.html)
- [2015-16](https://rptsvr1.tea.texas.gov/perfreport/tapr/2016/xplore/cadv.html)
- [2014-15](https://rptsvr1.tea.texas.gov/perfreport/tapr/2015/xplore/cadv.html)
- [2013-14](https://rptsvr1.tea.texas.gov/perfreport/tapr/2014/xplore/cothr.html)
- [2012-13](https://rptsvr1.tea.texas.gov/perfreport/tapr/2013/xplore/cothr.html)

### School directory data
You'l find that the Advanced Course data does NOT have school names, only a campus number. You have to get school directory information to get the names of the schools. This requires a JOIN of two files, and careful consideration on the `CAMPUS` and `School Number` fields, because they may contain _leading zeros_, depending on the file format you use. If you open the `.dat` or `.csv` files in Excel [without telling it to treat these files as text](https://rptsvr1.tea.texas.gov/perfreport/tapr/2017/xplore/taprhelp.html), you will lose those zeros and your join will not work.

School name information is available on [AskTED](http://tea4avholly.tea.state.tx.us/tea.askted.web/Forms/Home.aspx). Download the file called 	**Download School and District File**.

### Data masking
TEA masks numbers where the total number of students is less than five. The link to how they do it is above. My suggestion:
- If the field has an asterisk "*", Make that a blank field instead. (Perhaps through a search and replace?) That usually means there were no students in that category.
- If the field is "-1", that means there were fewer than five students. I would make these a "1" and then explain in any analysis that it means fewer than five.
- But ... you can only use these numbers at the individual campus level. If you try to add campuses together or do math, then those numbers will be off. A BETTER IDEA is to pull **District** data to get those answers.
- [TEA masking rules for 2017](https://rptsvr1.tea.texas.gov/perfreport/tapr/2017/masking.html)

### Questions and caveats
The calendar year 2016-17 shows Advanced Course rates (percent of those taking advance courses / taking any course) for seperate years 2015 and 2016. This is where a call to TEA might be in order. I don't know if that is a comparison to the previous year's cohort of students, or that some tests are administered in the first calendar year and others in the second calendar year of the same school year. Or if they are retakes. IF THEY ARE DIFFERENT COHORTS, then I would just choose the most recent year for a given category. I've looked through the [TAPR Glossary](https://tea.texas.gov/WorkArea/linkit.aspx?LinkIdentifier=id&ItemID=51539617810&libID=51539617810) and I didn't see an explantion why there are two distinct years in each data set.
- Melissa says these are two years to compare against. As such, I pulled the most recent release available for any given year.

## Steps to process the data.
In this repo, I've have a Jupyter Notebook that processes the data for Any Subject, Grades 9-12, and it can be found in `notebooks/01_data_process.ipynp`. I use programming to do this because I'm more likely to catch mistakes, and it is easier to fix those that come early in a long process. That said, these steps can be done manually.

- Make a copy of the data folder, so you keep pristine origials. If doing this manually, I would use the files in `data-raw/excel-any` so you work with these in Excel.
    - If you do use the csv verisons, make sure to import them as text, and [make sure the CAMPUS column is treated as text](https://support.office.com/en-us/article/text-import-wizard-c5b02af6-fda1-4440-899f-f78bafe41857), and not a number.

### Combine the year files
- You are going to create a new file that is a combination of all the other ones.
    - Check each of the individual files to make sure the columns are the same and in the same order, using the **Column Reference files** listed above. 2012 is the odd one because the columns names are different.
    - Add a new column and call it "Year", and then add the year value to all the cells in that column. That way, once you've combined them, you know which data set each row came from.
- Create your new file, and then copy and paste each of the others into the same sheet, one below the other. You can either exclude the header or delete it afterward. I usually move them over and double check them to the header row to make sure the columns are the same. [Freezing the top row can help with that.](https://support.office.com/en-us/article/freeze-panes-to-lock-rows-and-columns-dab2ffc9-020d-4026-8121-67dd25f2508f)
- Rename the header row to something you can understand.

### Deal with masked data
Now you have to de-mask the data (see notes above), which takes some editorial decisions.
- The data uses a period in cells where there is no data. If you are going to use in visualization or charting software at all, then I would remove those. You can use Replace, but use "Find entire cells only" to replace a period with nothing.
- If there are fewer than five students in a percentage, then TEA used "-1" in the data. This is the more difficult decision. You can't really add these percentage values of the data anyway, so my choice was to change them to a "1" to is till show at least something and then explain that in chatter anytime I use them. Some folks who are doing math on some data will take the median of what it _could_ be, so "3", in this case. Make your choice, and use Replace with "Find entire cells only".

### Join with the directory
This part I'm not sure how you'll handle and I'm running out of time to research another way. Ask your folks at the workshop how you can join two different files on a Campus number. I thought I had used [merge-csv](http://merge-csv.com/) before, but I'm not getting it right. Perhaps folks at the workshop can walk you through [VLOOKUP](https://support.office.com/en-us/article/vlookup-function-0bbc8083-26fe-4963-8ab8-93a18ad188a1). Here is [a video tutorial](https://support.office.com/en-us/article/video-vlookup-when-and-how-to-use-it-9a86157a-5542-4148-a536-724823014785), though I have not previewed it. That all said, here are the steps:
- There is already a copy of this at `data-raw/Directory.csv`, but to get it fresh go to [from TEA](http://tea4avholly.tea.state.tx.us/tea.askted.web/Forms/Home.aspx) and download the **Download School and District** file. Or you can get the one with Site address if you will want that.
- I would _make a copy_ of that file (so you have the original still), then open the copy and remove the columns you don't need. Do be sure to keep School Number, School Name and District Name, as you need all of those, for sure.
- Next you need a tool that will join the two. On your course data, the columns is "CAMPUS". On the directory file, it is "School Number". 
- In the end, save a copy of the file that has the school and district names

### Filter to districts you care about
Now that you have district names, you can filter the data set using the "District Name" to just the districts you care about.

That's it! :-)