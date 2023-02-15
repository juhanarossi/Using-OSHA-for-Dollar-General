# Using-OSHA-for-Dollar-General
# Gathering Data

An Excel file containing the analyzed data is here.

To track the violations of the retailing corporations issued by the Occupational Safety and Health Administration (OSHA), we went onto the administration's [establishment search website](https://www.osha.gov/ords/imis/establishment.html).

The website combines information as an HTML list as shown, so it is scrapable. 

Though the violation details are contained in a different website, we noticed the URL of these pages only differ by their activity code, hence we decided to use Python to scrape the websites, get their activity code, and proceed to the detailed page by changing the activity code.


We have written a code through Python that scrapes all the information returned via specific keywords and requirements we have typed in.


For the scrape, we searched “Dollar General”. When logging in to the items, OSHA addressed Dollar General under multiple names. We have included “Dollar General” and its subsidiaries dolgen’, ‘Dolgencorp’, and ‘DG Retail.’

We have to create separate query lines with different time periods, “ 2012-12-19 - 2022-12-19” and “2010-01-01 - 2012-12-18” because the website only allows searching for a decade at a time. Our latest search date is Dec.19, 2022, because the establishment website only contains the data until this date as we conducted our scrape on Jan. 16, 2023.

The scrape returned us with an error saying “No table found” for ‘DG Retail’ and ‘dolgen’ before 2012. After manually searching the website, we realized there are no violations cited under both names before 2012, which caused the error. So we deleted the “2010-01-01 - 2012-12-20” queries for both of the keywords.

The queries returned 260 inspections with violations in total.

The scraping returned to us with six different CSV files: activities, summary, items, related activity, investigated inspections, and details. 

Furthermore, as a control group. We have searched the violations of “Dollar Tree” and its subsidiary “Family Dollar,” “Walmart,” and “Kroger,” two other retailers via the same process. 

We made a decision to only scrape Family Tree since 2016-01-01, as Dollar Tree acquired Family Dollar on July 6, 2015. The Dollar Tree & Family Dollar query returned us with 527 inspections with violations.
# Cleaning Data
After viewing the data we have gathered, we decided to join the activities_df, details_df, and violationsummary_df together under the key of the activity code.

For the penalty path, we located all the “current penalty” rows and exported them to a new csv.

For the violations path, we located all the “current violation” rows and exported them to a new csv.
# Dropping the Duplicates
When doing final checks on the data, we discovered duplicated items in the Dollar General and Dollar Tree query. We couldn’t pin down the precise cause of the issue, but it appears that having the same search terms (in this case “Dollar General” and “Dolgencorp”) featured both in the query terms and also in results (search results featured both terms) confused the program and it returned multiple results or rows of data for the same inspection number.

We dropped the duplicates via ‘df.drop_duplicates’, only leaving one match for each inspection number. The duplicates were only found in our Dollar General and Dollar Tree queries.

We also checked our results against the data published at [Violation Tracker](https://violationtracker.goodjobsfirst.org/?parent=dollar-general&order=pen_year&sort=). Through inspection, we concluded that no similar issue occurred with Dollar Tree, Walmart, or the Kroger database. We have kept the drop duplicates step for each of the retailers nevertheless.
# Getting the information
For each company’s data, we have conducted mostly identical analysis procedures:

We have specially flagged the data for the year 2022 as most of its cases are not closed. From our previous interview, we learned that retailers can argue their violations before the case is closed, and they can be charged less or even have the citation completely dismissed. We have introduced how we have addressed this issue in each of our charts separately.

We have seen a discrepancy between the violation listed and the actual violation item numbers. For example, the Violation in 2010 in PA has 4 violations in total, but only shows 2 in “Vio‘. Hence, we decided to use the number of violation items actually listed.

We separated all the current penalties, current violations, initial penalties, and initial violations rows into separate CSV files. 

For each of the files, we grouped the rows by year to create four separate data frames of  current penalties, current violations, initial penalties, and initial violations for each of the four retailers.

# Getting Types of Violations (Dollar General only)
For the Dollar General data, we also wanted to know what were the most cited violations. In the data frame, OSHA listed the exact regulation a company has violated. We simply grouped the Dollar General Data by the regulation number.

Worth to be noted, when a violation is dropped, OSHA still keeps the original violation in this part of the chart. As a result, our result reflects the initial type of violations.

Then, we searched the top five most violated regulations on the [OSHA’s regulation page](https://www.osha.gov/laws-regs/regulations/standardnumber/1910#1910.22) manually. Which relates to item storage, fire extinguishers, and blocked exits. These three were also named as the major types of violations according to the press release issued by OSHA.
