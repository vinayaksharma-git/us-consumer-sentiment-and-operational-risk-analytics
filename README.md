# US Financial Sector: Consumer Sentiment & Operational Risk Analytics 💵

## 1. Description ✒️
This data analytics project investigates operational efficiencies, regulatory compliance, and customer friction points across the US financial sector using the Consumer Financial Protection Bureau (CFPB) complaints dataset. By analyzing thousands of consumer records, the project uncovers critical operational risks, evaluates corporate response timelines against industry standard SLAs, and maps geographical and seasonal trends in consumer disputes.

## 2. Data Source 🌐
Kaggle: https://www.kaggle.com/datasets/kaggle/us-consumer-finance-complaints

## 3. Business Objective 🏢
1. What is the overall SLA (Service Level Agreement) failure rate for complaint responses ie complaints which receive an untimely response, and which corporate entities represent the highest risk of non-compliance?

2. Which specific financial product and sub-product combinations result in the highest rate of consumer disputes after a company provides a resolution?

3. Is there a correlation between the complaint submission channel and missing the response deadline?

4. What is the geographic distribution of complaint volumes across US states, how do these volumes fluctuate by month, and what are the dominant underlying issues driving regional spikes?

💡 ***NOTE:***

SLA almost always refers to a time deadline for resolving a task.

CFPB sets a strict rule for financial companies. When a consumer files a complaint, the company must respond within a specific timeframe (usually 15 days).

SLA Met: The company responds before the deadline (timely_response = Yes).

SLA Failure / Breach: The company takes too long and misses the deadline (timely_response = No).

## 4. Tools & Technologies 🛠️
**A. Data Cleaning:**
1. Excel: (Formulas, Power Query)
    - Removed useless required columns.
    - Make Table (from Table/Range) and open Power Query.
    - Replace null values (used unknown and 00000)
    - Corrected date format using:
        - month = IF(ISNUMBER(SEARCH("/", [@[date]])), LEFT([@[date]], 2))
        - day = IF(ISNUMBER(SEARCH("/", [@[date]])), MID([@[date]], 4, 2))
        - year = IF(ISNUMBER(SEARCH("/", [@[date]])), RIGHT([@[date]], 4))
        - date = IF(ISNUMBER(SEARCH("/", [@[date]])), DATE([@[year]], [@[month]], [@[day]]), [@[date]])

        💡 ***NOTE:***

        - SEARCH(), searches for index where "/" is located.
        - ISNUMBER(), tells if the provided value is number or not - (It used index number to validate).
        - DATE(), made date column out of extracted day, year & date values.
    - Corrected Zip Number using: 
        - =TEXT([@zipcode], "00000"), is used because zip code are of 5 digits.

        💡 ***NOTE:***
        - =TEXT(value, "format_text")
        - "0" (Zero): Forced placeholder. If the number has fewer digits than there are zeros in the format code, Excel forces leading or trailing zeros to display.
        - "#" (Hash): Optional placeholder. It displays the digit if it exists, but does not display extra zeros if the number is shorter.
        - "?" (Question Mark): Space alignment. It adds invisible spaces for missing digits so that decimal points line up neatly in a column.
2. Power BI: (Power Query, Measures, Visualization) 
    - Removed Columns
    - Column Type changed
    New Table to store Measures was created.

**B. Data Visualization:** 

1. Power BI (Measures, Visualization, DAX Formula)
    - To create dashboard, measures were used to provide DAX formula:
        - complaint_volume = COUNT(consumer_complaints[complaint_id])
        - disputed_count = COUNTX(FILTER(consumer_complaints, consumer_complaints[consumer_disputed?] = "Yes"), consumer_complaints[consumer_disputed?])
        - perc_dispute = (COUNTX(FILTER(consumer_complaints, consumer_complaints[consumer_disputed?] = "Yes"), consumer_complaints[consumer_disputed?])/COUNT(consumer_complaints[consumer_disputed?]))*100
        - perc_resolved = (COUNTX(FILTER(consumer_complaints, consumer_complaints[consumer_disputed?] = "No"), consumer_complaints[consumer_disputed?])/COUNT(consumer_complaints[consumer_disputed?]))*100
        - response_count = COUNT(consumer_complaints[timely_response])
        - SLA_Breach = COUNTX(filter(consumer_complaints, consumer_complaints[timely_response] = "No"), consumer_complaints[timely_response])
    - To visualize Table, Bar Graph, Line Chart, and Card were used, where conditional formating was used of few columns.

## 5. Key Insights & Solutions 📊
1. 97.47% of total response were timely response and 2.53% were untimely response. Out of which top 5 defaulters are Bank of America with 1,552 SLA Breach followed by Ocwen, Wells Fargo & Company, Citibank, Mobiloans.
2. Most dispute is related to Credit reporting with dispute count of 1,5078, followed by Mortgage, Credit card, Bank account or service (Checking Account), Mortgage (ARM).
3. 79.83% of consumer disputes were resolved and only 20.17% remained unresolved.
Out of 14,048 recoreded submittion mode, most SLA Breach happened when submitted via Web with count of 10,065, followed by Referral,  Phone, Postmail, Fax, and Email.
4. Most complaints were reported from the state of California, followed by Florida, Texas, New York, Georgia, with maximum complaint volume in March (9.65%) and January(9.17%).

## 6. Project Visuals 📀
[!image]()

🔗Demo Video: https://youtu.be/jPDt4gzr4kg?si=XcxFzkOS4BetxS4m