# Bank-Marketing-Data-Set-Lift-Analysis
Analysis of data from a term limit subscription marketing campaign by a Portugese bank, May 2008-Nov 2010

The aforementioned data set includes direct marketing campaigns (i.e. phone calls) of a Portuguese banking institution. The goal is to predict if the client will subscribe a term deposit (indicated in the y variable). Your task is to create a model that will help this banking institution determine, in advance, clients who will be receptive to such marketing campaigns. Clearly state the metric used for this problem.

Attribute Information (copied from data host site: https://archive.ics.uci.edu/ml/datasets/Bank+Marketing):
#  Data
## bank client data:

1 - age (numeric)

2 - job : type of job (categorical: 'admin.', 'blue-collar', 'entrepreneur', 'housemaid', 'management', 'retired', 'self-employed', 'services', 'student', 'technician', 'unemployed', 'unknown')

3 - marital : marital status (categorical: 'divorced', 'married', 'single', 'unknown'; note: 'divorced' means divorced or widowed)

4 - education (categorical: 'basic.4y', 'basic.6y', 'basic.9y', 'high.school', 'illiterate', 'professional.course', 'university.degree', 'unknown')

5 - default: has credit in default? (categorical: 'no', 'yes', 'unknown')

6 - housing: has housing loan? (categorical: 'no', 'yes', 'unknown')

7 - loan: has personal loan? (categorical: 'no', 'yes', 'unknown')
##  related with the last contact of the current campaign:

8 - contact: contact communication type (categorical: 'cellular', 'telephone')

9 - month: last contact month of year (categorical: 'jan', 'feb', 'mar', ..., 'nov', 'dec')

10 - day_of_week: last contact day of the week (categorical: 'mon', 'tue', 'wed', 'thu', 'fri')

11 - duration: last contact duration, in seconds (numeric).
##  other attributes:

12 - campaign: number of contacts performed during this campaign and for this client (numeric, includes last contact)

13 - pdays: number of days that passed by after the client was last contacted from a previous campaign (numeric; 999 means client was not previously contacted)

14 - previous: number of contacts performed before this campaign and for this client (numeric)

15 - poutcome: outcome of the previous marketing campaign (categorical: 'failure', 'nonexistent', 'success')
##  social and economic context attributes

16 - emp.var.rate: employment variation rate - quarterly indicator (numeric)

17 - cons.price.idx: consumer price index - monthly indicator (numeric)

18 - cons.conf.idx: consumer confidence index - monthly indicator (numeric)

19 - euribor3m: euribor 3 month rate - daily indicator (numeric)

20 - nr.employed: number of employees - quarterly indicator (numeric)
##  Output variable (desired target):

21 - y - has the client subscribed a term deposit? (binary: 'yes', 'no')

#  Notes/Glossary

Duration Important note: this attribute highly affects the output target (e.g., if duration=0 then y='no'). Yet, the duration is not known before a call is performed. Also, after the end of the call y is obviously known. Thus, this input should only be included for benchmark purposes and should be discarded if the intention is to have a realistic predictive model.

Employment Variation Rate: Quarterly percent change in employment rate for Portugal

Consumer Price Index (CPI): "CPIs aim to cover the whole set of goods and services consumed within the territory of a country by the population. To do this, a representative set is selected; the so-called “consumer basket”. Consumer goods and services include, for example, food and beverages, products for personal hygiene, newspapers and periodicals, expenditure on housing, water, electricity, gas and other fuels, health, transport, communications, education, restaurants and hotels." - From the "statistics" section of the EU Commission website https://ec.europa.eu/

It's a measure of inflation, how far consumer dollars go, and is used in monetary policy. This would be the Portugal CPI.

Consumer Confidence Index (CCI): "In the glossary on its website, The Conference Board defines the Consumer Confidence Survey as "a monthly report detailing consumer attitudes and buying intentions, with data available by age, income and region"." - From Wikipedia https://en.wikipedia.org/wiki/Consumer_confidence_index

Each country has its own metric, as it is too variable between countries. This would be Portugal's metric.

EURIBOR 3 Month: The EURIBOR, or the European Interbank Rate, is "a daily reference rate, published by the European Money Markets Institute,[1] based on the averaged interest rates at which Eurozone banks offer to lend unsecured funds to other banks in the euro wholesale money market (or interbank market)." - Wikipedia https://en.wikipedia.org/wiki/Euribor

"Euribor is the interest rate at which a large number of European banks do provide short term loans to one another. Banks which borrow money from other banks can use these funds to provide loans to other parties.[...]

Many banks do lend money by providing mortgages. In many European countries the interest rate which has to be paid for a short term loan or mortgage (short term fixed interest rate period) does follow the Euribor-rate. Once the Euribor-rate increases, the interest which has to be paid increases as well and vice versa." - Euribor-Rates.EU https://www.euribor-rates.eu/en/euribor-mortgage/

I added the above bolding to emphasize, higher Euribor rates mean higher interest payments on bank loans.

Number Employed: This is the quarterly averaged employment rate of Portugal in thousands.
#  Metric and Process

I will be performing a lift measurement to determine which subset of the population of clients has the likelihood of subscribing to term limit deposits. A bit about the lift metric from Wikipedia:

"[L]ift is a measure of the performance of a targeting model (association rule) at predicting or classifying cases as having an enhanced response (with respect to the population as a whole), measured against a random choice targeting model. A targeting model is doing a good job if the response within the target is much better than the average for the population as a whole. Lift is simply the ratio of these values: target response divided by average response."

    Wikipedia page https://en.wikipedia.org/wiki/Lift_(data_mining)

"Lift/cumulative gains charts [...] are instead a means of evaluating the results where your resources are finite. Either because there's a cost to action each result (in a marketing scenario) or you want to ignore a certain number of guaranteed voters, and only action those that are on the fence." - user morganics on Stack Overflow https://stackoverflow.com/a/45544488

The use of a Lift measure in this case will help the marketing team to decide where its money and time would best be invested in attempting to gain subscribers to this program.

I will start by narrowing down the 20 features using a random forest model and its feature importance metric. I will then calculate and plot lift and do a decile waterfall plot. From there, I will analyze the results and use statistical hypothesis testing to find which features are most useful in finding receptive clients.
