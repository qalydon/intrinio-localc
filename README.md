# LibreOffice Calc Extension for Intrinio
Copyright © 2017, 2018 by Dave Hocker as Qalydon

## Notice
On 2018-11-12 I received email notification that Intrinio was going to
shut down the free service as of 2018-12-12. If you have a free account,
you probably received the same notification. Therefore, you must acquire
a paid Intrinio account to continue using this extension after 2018-12-12.

As an alternative you might consider using the IEX LOCalc extension for
the [IEX Trading](https://iextrading.com) service. This extension is avilable
on [GitHub](https://github.com/qalydon/iex-localc). The IEX service
provides a different set of functions, but you might find it useful.
Currently, the IEX service is free and requires no sign up.

## Overview
This project implements a LibreOffice Calc (LOCalc) addin extension that can
retrieve data from the Intrinio Marketplace service. It provides a
subset of the functions implemented by the
[Intrinio Excel AddIn](https://github.com/intrinio/intrinio-excel) plus
an additional set of functions not found in the Intrinio Excel AddIn.
Currently, only functions that will work with a basic, free Intrinio
account have been implemented. In the future, the function set may be
expanded to provide parity with the Intrinio Excel AddIn.

The LOCalc addin works on the Windows, macOS and Ubuntu versions of
[LibreOffice (version >= 5.0)](https://www.libreoffice.org/).

## License
GNU GENERAL PUBLIC LICENSE Version 3, 29 June 2007. Refer to the
[LICENSE.md](https://github.com/qalydon/intrinio-localc/blob/master/README.md)
file for complete details.

## Download
Download the latest **intrinio.oxt** (the add-in file) from
[here](https://github.com/qalydon/intrinio-localc/releases).

## Installation
1. Download the latest **intrinio.oxt** (the add-in file) from
[here](https://github.com/qalydon/intrinio-localc/releases).
1. Start LibreOffice or LibreOffice Calc.
1. From the Tools menu, open the Extension Manager.
1. Look through the list of installed add-ins for Intrinio Fintech
Marketplace. If you find it, click the Remove button to remove it.
For best results, **remove an existing Intrinio Fintech Markeplace
add-in first**.
1. Click the Add button.
1. Navigate to the location where you downloaded **intrinio.oxt**.
Select it.
1. Choose if you want the add-in installed for you or everyone.
1. Click the Close button.
1. If LibreOffice asks to restart, do so.
1. The first time that you use an Intrinio function, the addin will
prompt you for your user name and password.

It is recommended that you always remove an existing version of the
add-in before installing an update. Othwerwise, your results may be
unpredictable.

## Configuration File
The addin keeps information such as your user name and password in
a configuration file. You should be aware of this configuration file
and make sure that its permissions allow only you to access it.

Under Windows, only your user account should have access to the file.
Under macOS or Ubuntu, the permissions on the file should be something
like 600 (only your account has read/write access).

The content of the configuration file is JSON and looks something like this.
```
{
    "password": "0123456789abcdef0123456789abcdef",
    "user": "0123456789abcdef0123456789abcdef",
    "loglevel": "debug",
    "cachelife": 180
}
```

| Key | Value |
|:-----|:-------|
| password | As supplied by Intrinio |
| certifi | The location of the cacert.pem file |
| user | As supplied by Intrinio |
| loglevel | error, warning, info, debug (default) |
| cachelife | The life time of cached IntrinioDataPoint data<br/>-1 means cache lives until LibreOffice closes.<br/>0 means no caching.<br/>&gt;0 sets a specific cache life value in seconds.|

Under normal circumstances, you should only need to change the loglevel and/or cachelife
settings.

The location of the configuration file depends on your operating system.

| OS | Location |
|----|----------|
| Windows | C:\Users\username\AppData\Local\libreoffice\intrinio\intrinio.conf |
| macOS | /Users/username/libreoffice/intrinio/intrinio.conf |
| Ubuntu | /home/username/libreoffice/intrinio/intrinio.conf |

## Example Files
You can find a number of example files in the [examples folder](https://github.com/qalydon/intrinio-localc/tree/master/examples).
Some of
the examples are LO conversions/derivatives of template files from the
[Intrinio Excel AddIn.](https://github.com/intrinio/intrinio-excel)
These files show you how most of the LOCalc Extension functions
can be used.

## LOCalc Functions
The addin provides a number of functions for retrieving data from
the Intrinio Marketplace service. The total set of functions is
divided into two groups.

1. Functions that are common to the Intrinio Excel AddIn.
1. Functions that are **unique** to the LOCalc AddIn.

## Data Caching
Most of the Intrinio functions cache (store for future reuse) the data they retrieve.
This minimizes the number of calls to Intrinio APIs, thus increasing the utlity 
of a free Intrinio account. All historical and static data
is cached until LibreOffice is closed (sort of "forever"). 

However, IntrinioDataPoint data is cached subject to a "cache life."
The data is considered valid until its life expires at which point
it will be re-retrieved as needed.
The cache life defaults to 180 seconds or 3 minutes. The cache life setting
can be customized through the [configuration file](#configuration-file).

## Functions Common to the Excel AddIn
To the degree possible, these functions work like the similarly named
[Intrinio Excel Addin functions](http://docs.intrinio.com/excel-addin#intrinio-excel-functions).

### IntrinoUsage
Use IntrinioUsage to retrieve information about how much of the Intrinio
service you have used.
```
=IntrinioUsage(access_code, item)
```
[access_code list](http://docs.intrinio.com/?javascript--api#usage): The com_fin_data
access_code covers most of the basic, free Intrinio service.

item: current | percent | limit | status_code

### IntrinioDataPoint
This function works like the equivalent
[IntrinioDataPoint](http://docs.intrinio.com/excel-addin#intriniodatapoint)
Excel AddIn function.
```
=IntrinioDataPoint(identifier, item)
```
**Remember, data retrieved by IntrinioDataPoint is subject to the cache life setting.**

### IntrinioHistoricalPrices
This function works like the equivalent
[IntrinioHistoricalPrices](http://docs.intrinio.com/excel-addin#intriniohistoricalprices)
Excel AddIn function.
```
=IntrinioHistoricalPrices(ticker, item, sequence, start_date, end_date, frequency)
```

### IntrinioHistoricalData
This function works like the equivalent
[IntrinioHistoricalDate](http://docs.intrinio.com/excel-addin#intriniohistoricaldata)
Excel AddIn function.
```
=IntrinioHistoricalData(ticker, item, sequence, start_date, end_date, frequency, data_type)
```

### IntrinioNews
This function works like the equivalent
[IntrinioNews](http://docs.intrinio.com/excel-addin#intrinionews)
Excel AddIn function.
```
=IntrinioNews(identifier, item, sequence)
```

### IntrinioFundamentals
This function works like the equivalent
[IntrinioFundamentals](http://docs.intrinio.com/excel-addin#intriniofundamentals)
Excel AddIn function.
```
=IntrinioFundamentals(identifier, statement, type, sequence, item)
```

### IntrinioTags
This function works like the equivalent
[IntrinioTags](http://docs.intrinio.com/excel-addin#intriniotags)
Excel AddIn function.
```
=IntrinioTags(identifier, statement, sequence, item)
```

### IntrinioFinancials
This function works like the equivalent
[IntrinioFinancials](http://docs.intrinio.com/?javascript--api#standardized-financials)
Excel AddIn function.
```
=IntrinioFinancials(identifier, statement, fiscal_year, fiscal_period, tag, rounding)
OR
=IntrinioFinancials(identifier, statement, sequence, type, tag, rounding)
```
Some explanation is in order as the Intrinio documentation does not really clarify
exactly how the parameters work.

The IntrinioFinancials function starts by assuming that the first form
is being used. However, if the fiscal_year parameter is a value
less than 1900, the value is treated as a sequence number. This
implies that the second form is being used.

In this case, the sequence and type parameters are translated to
a fiscal_year and fiscal_period value by using the IntrinioFundamentals
function.
```
fiscal_year = IntrinioFundamentals(ticker, statement, type, sequence, "fiscal_year")
fiscal_period = IntrinioFundamentals(ticker, statement, type, sequence, "fiscal_period")
```
Essentially, this maps the sequence parameter to a year in an inverse order.
Sequence value 0 will correspond to the most recent fundamental while
value 1 will be back one increment. For example, if the type value is "FY",
then the sequence number will be the relative fiscal year.

### IntrinioReportedFundamentals
This function works like the equivalent
[IntrinioReportedFundamentals](http://docs.intrinio.com/excel-addin#intrinioreportedfundamentals)
Excel AddIn function.
```
=IntrinioReportedFundamentals(identifier, statement, type, sequence, item)
```

### IntrinioReportedTags
This function works like the equivalent
[IntrinioReportedTags](http://docs.intrinio.com/excel-addin#intrinioreportedtags)
Excel AddIn function.
```
=IntrinioReportedTags(identifier, statement, sequence, item)
```

### IntrinioReportedFinancials
This function works like the equivalent
[IntrinioReportedFinancials](http://docs.intrinio.com/excel-addin#intrinioreportedfinancials)
Excel AddIn function.
```
=IntrinioReportedFinancials(identifier, statement, fiscal_year, fiscal_period, xbrl_tag, domain_tag)
OR
=IntrinioFinancials(identifier, statement, sequence, type, xbrl_tag, domain_tag)
```
Some explanation is in order as the Intrinio documentation does not really clarify
exactly how the parameters work.

The IntrinioReportedFinancials function starts by assuming that the first form
is being used. However, if the fiscal_year parameter is a value
less than 1900, the value is treated as a sequence number. This
implies that the second form is being used.

In this case, the sequence and type parameters are translated to
a fiscal_year and fiscal_period value by using the IntrinioReportedFundamentals
function.
```
fiscal_year = IntrinioReportedFundamentals(identifier, statement, type, sequence, "fiscal_year")
fiscal_period = IntrinioReportedFundamentals(identifier, statement, type, sequence, "fiscal_period")
```
Essentially, this maps the sequence parameter to a year in an inverse order.
Sequence value 0 will correspond to the most recent fundamental while
value 1 will be back one increment. For example, if the type value is "FY",
then the sequence number will be the relative fiscal year.

## Functions Unique to the LOCalc AddIn
These functions provide access to additonal data - data that cannot be
accessed from the Excel AddIn.

There is a general usage model at play for these functions. There is
a query set and an identifier set. The query set contains query,
query-count, tag-count and tag functions.
The identifier set contains identifier, tag-count and tag functions.

The query-count function can be used to determine how many indices
are in the result set of a given query. Using this value, you can
construct a spreadsheet with cells for each index in the result set.
Similarly, you can use the tag-count function to determine how many
data items are available for an index. Use the tag function to
retrieve the tag/item name for each available tag.

The **IntrinioIndices.ods** spreadsheet provides an example of how this
is done.

### Indices
This set of functions returns an indices list and information for all
indices covered by [Intrinio](http://docs.intrinio.com/?javascript--api#indices37).
#### Indices by Query
```
=IntrinioIndicesQuery(query, indextype, sequence, item)
```
Returns a single data item for a selected index.
* query - a string that is used to filter the list of indices by
index name or symbol.
* indextype - stock_market, economic or sector.
* sequence - refers to the nth (0 < n < query-count) index in the
result list.
* item - the tag/item value to be returned.

```
=IntrinioIndicesQueryCount(query, indextype)
```
Returns the count of indices in the resultant list.
* query - a string that is used to filter the list of indices by
index name or symbol.
* indextype - stock_market, economic or sector.

```
=IntrinioIndicesQueryTagCount(query, indextype)
```
Returns the number of tags/items that are available for an index.
* query - a string that is used to filter the list of indices by
index name or symbol.
* indextype - stock_market, economic or sector.

```
=IntrinioIndicesQueryTag(query, indextype, sequence)
```
Returns a tag/item name for a selected index.
* query - a string that is used to filter the list of indices by
index name or symbol.
* indextype - stock_market, economic or sector.
* sequence - refers to the nth (0 < n < tag-count) tag/item in the list
of available tags/items.

#### Indices by Identifier
```
=IntrinioIndex(identifier, item)
```
Returns a single data item for the given index.
* identifier - the Intrinio symbol associated with the index. See
[Stock Market Indices](http://docs.intrinio.com/master/stock-indices).
* item - the item/tag value to be returned.

```
=IntrinioIndexTagCount(identifier)
```
Returns the number of tags/items that are available for an index.
* identifier - the Intrinio symbol associated with the index.

```
=IntrinioIndexTag(identifier, sequence):
```
Returns a tag/item name for an index.
* identifier - the Intrinio symbol associated with the index.
* sequence - refers to the nth (0 < n < tag-count) tag/item in the list
of available tags/items.

### Companies
For addtional information on avaliable data see
[Intrinio](http://docs.intrinio.com/?javascript--api#companies).
#### Companies by Query
```
=IntrinioCompaniesQuery(query, lastfilingdate, sequence, item)
```
Returns a single data item for a selected company.
* query - a string that is used to filter the list of companies by
company name or symbol.
* lastfilingdate - a date value that returns the list of companies whose
latest SEC filing was filed on or after this date.
* sequence - refers to the nth (0 < n < query-count) company in the
result list.
* item - the tag/item value to be returned.

```
=IntrinioCompaniesQueryCount(query, lastfilingdate)
```
Returns the count of companies in the resultant list.
* query - a string that is used to filter the list of companies by
company name or symbol.
* lastfilingdate - a date value that returns the list of companies whose
latest SEC filing was filed on or after this date.

```
=IntrinioCompaniesQueryTagCount(query, lastfilingdate)
```
Returns the number of tags/items that are available for a company.
* query - a string that is used to filter the list of companies by
company name or symbol.
* lastfilingdate - a date value that returns the list of companies whose
latest SEC filing was filed on or after this date.

```
=IntrinioCompaniesQueryTag(query, lastfilingdate, sequence)
```
Returns a tag/item name for a selected company.
* query - a string that is used to filter the list of companies by
company name or symbol.
* lastfilingdate - a date value that returns the list of companies whose
latest SEC filing was filed on or after this date.
* sequence - refers to the nth (0 < n < tag-count) tag/item in the list
of available tags/items.

#### Companies by Identifier
```
=IntrinioCompany(identifier, item)
```
Returns a single data item for the given company.
* identifier - the [Intrinio](http://docs.intrinio.com/master/us-securities#home)
symbol associated with the company.
* item - the/tag value to be returned.

```
=IntrinioCompanyTagCount(identifier)
```
Returns the number of tags/items that are available for a company.
* identifier - the [Intrinio](http://docs.intrinio.com/master/us-securities#home)
symbol associated with the company.

```
=IntrinioCompanyTag(identifier, sequence):
```
Returns a tag/item name for an company.
* identifier - the [Intrinio](http://docs.intrinio.com/master/us-securities#home)
symbol associated with the company.
* sequence - refers to the nth (0 < n < tag-count) tag/item in the list
of available tags/items.

### Securities
For additinal information on avaliable security data see
[Intrinio](http://docs.intrinio.com/?javascript--api#securities).

#### Securities by Query
```
=IntrinioSecuritiesQuery(query, exchangesymbol, lastcrspadjdate, sequence, item)
```
Returns a single data item for a selected security.
* query - a string that is used to filter the list of securities by
security name or symbol.
* exhangesymbol - the [Intrinio Stock Market Symbol](http://docs.intrinio.com/master/stock-exchanges#home)
for the list of securities.
* lastcrspadjdate - filters the result to securities that have had
adjusted stock prices due to a corporate event after this date.
* sequence - refers to the nth (0 < n < query-count) security in the
result list.
* item - the tag/item value to be returned.

```
=IntrinioSecuritiesQueryCount(query, exchangesymbol, lastcrspadjdate)
```
Returns the count of securities in the resultant list.
* query - a string that is used to filter the list of securities by
security name or symbol.
* exhangesymbol - the [Intrinio Stock Market Symbol](http://docs.intrinio.com/master/stock-exchanges#home)
for the list of securities.
* lastcrspadjdate - filters the result to securities that have had
adjusted stock prices due to a corporate event after this date.

```
=IntrinioSecuritiesQueryTagCount(query, exchangesymbol, lastcrspadjdate)
```
Returns the number of tags/items that are available for a security.
* query - a string that is used to filter the list of securities by
security name or symbol.
* exhangesymbol - the [Intrinio Stock Market Symbol](http://docs.intrinio.com/master/stock-exchanges#home)
for the list of securities.
* lastcrspadjdate - filters the result to securities that have had
adjusted stock prices due to a corporate event after this date.

```
=IntrinioSecuritiesQueryTag(query, exchangesymbol, lastcrspadjdate, sequence)
```
Returns a tag/item name for a selected security.
* query - a string that is used to filter the list of securities by
security name or symbol.
* exhangesymbol - the [Intrinio Stock Market Symbol](http://docs.intrinio.com/master/stock-exchanges#home)
for the list of securities.
* lastcrspadjdate - filters the result to securities that have had
adjusted stock prices due to a corporate event after this date.
* sequence - refers to the nth (0 < n < tag-count) tag/item in the list
of available tags/items.

#### Securities by Identifier
```
=IntrinioSecurity(identifier, item)
```
Returns a single data item for the given security.
* identifier - the [Intrinio](http://docs.intrinio.com/master/us-securities#home)
symbol associated with the security.
* item - the/tag value to be returned.

```
=IntrinioSecurityTagCount(identifier)
```
Returns the number of tags/items that are available for a security.
* identifier - the Intrinio symbol associated with the security.

```
=IntrinioSecurityTag(identifier, sequence):
```
Returns a tag/item name for an security.
* identifier - the Intrinio symbol associated with the security.
* sequence - refers to the nth (0 < n < tag-count) tag/item in the list
of available tags/items.

### Company SEC Filings
For additional information on avaliable SEC filing data see
[Intrinio](http://docs.intrinio.com/?javascript--api#company-sec-filings).

```
=IntrinioCompanySECFilings(identifier, reporttype, startdate, enddate, sequence, item)
```
Returns a single data item for a selected company filing.
* identifier - the Intrinio symbol associated with the security.
* reporttype - 10-K, 10-Q, 8-K, 4, etc. See [report types](https://en.wikipedia.org/wiki/SEC_filing#All_filing_types).
* startdate - the earliest filing date for which to return filings.
* enddate - the last filing date for which to return filings.
* sequence - refers to the nth (0 < n < count) filing in the list
of available filings.
* item - the/tag value to be returned.

```
=IntrinioCompanySECFilingsCount(identifier, reporttype, startdate, enddate)
```
Returns the count of available company filings.
* identifier - the Intrinio symbol associated with the security.
* reporttype - 10-K, 10-Q, 8-K, 4, etc. See [report types](https://en.wikipedia.org/wiki/SEC_filing#All_filing_types).
* startdate - the earliest filing date for which to return filings.
* enddate - the last filing date for which to return filings.

```
=IntrinioCompanySECFilingsTagCount()
```
Returns the count of data items/tags available for each filing.

```
=IntrinioCompanySECFilingsTag(sequence)
```
Returns the name of a data item/tag available for each filing.
* sequence - refers to the nth (0 < n < tag-count) tag/item in the list
of available tags/items.

## References
* [Intrinio Web Site](https://intrinio.com)
* [Intrinio Excel AddIn](http://docs.intrinio.com/excel-addin#intrinionews)
* [LibreOffice Web Site](https://www.libreoffice.org/)