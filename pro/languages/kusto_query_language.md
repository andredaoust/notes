# Kusto Query Language

- [Kusto Query Language](#kusto-query-language)
  - [Common Operators](#common-operators)
    - [Search](#search)
    - [Where](#where)
    - [Limit](#limit)
    - [Count](#count)
    - [Summarize](#summarize)
    - [Extend](#extend)
    - [Project](#project)
    - [Distinct](#distinct)
    - [Top](#top)

## Common Operators

### Search
```
TableName
| search "query"
```

Looks in TableName for the text string "query" and yields the results.

```
TableName
| search kind=case_sensitive "Query"
```

Looks in TableName for the case sensitive text string "Query" and yields the results

```
search in (tableName1, tableName2, tableName3) "query"
```

Looks in all 3 tables listed for the text string "query"

```
TableName
| search ColumnName == "query"
```

Searches a column for the exact text

```
TableName
| search ColumnName:"query"
```

Search column for instances that contain "query"

```
TableName
| search * startswith "query"
```

Search across all columns (*) for columns that begin with "query" 

```
TableName
| search * endswith "query"
```

Search across all columns (*) for columns that end with "query"

```
TableName
| search "start * end" and ("A" or "B")
```

This will search all columns for text that begins with "start" and finishes with "end". And it will search all those columns for those that contains "A" or "B".

```
TableName
| search ColumnName matches regex "[A-Z]:"
```
Matches against a regex.

### Where  

```
TableName
| where TimeGenerated >=ago(1h)
```

Retrieves all entries in the table that were generated within the last hour.

Ago says "start right now, then go back in time N quantity" where N =
d - days
h - hours
m - minutes
s - seconds
ms - milliseconds

```
TableName
| where TimeGenereated >=ago(1h)
	And ColumnName == "this text"
```

```
TableName
| where TimeGenerated >=ago(1h)
	    And (ColumnA == "test text"
		or
		ColumnB == "this text")
```

Returns all columns that equal "test text" or "this text" from ColumnA or ColumnB created within the last hour.

```
TableName
| where TimeGenerated >=ago(1h)
| where (ColumnA == "test"
	    or 
	    ColumnB == "where")
| where columnC < 0
```

This pipes the first where clause result into the second which pipes to the third.

```
TableName
| where * has "text"
```

This simulates a Search query

```
TableName
| where * contains "text"
```

This works the same as 'has' and 'search'


### Limit  
```
TableName
| limit 10
```

Picks 10 rows at random

```
TableName
| where TimeGenerated >=ago(1h)
	and ColumnA == "text"
	and ColumnB > 0
| limit 10
```

This grabs 10 random rows from the result of the where clause. Limit is most useful during development/testing as you don't have to generate more than N results.

**note**: _Take_ is listed as an alternative to _limit_ but doesn't seem to work in the latest Azure Portal.


### Count  

```
TableName
| count
```

Returns the number of rows in the table

### Summarize  

```
TableName
| summarize count() by ColumnName
```

count() is a function here, not the operator

This will grab all unique entries in ColumnName and sum all the instances of that text.

```
TableName
| summarize count() by ColumnA, ColumnB
```

Sums the unique combinations of ColumnA and ColumnB

```
TableName
| summarize NewColumnNmae = count()
	        by ColumnA, columbB
```

This allows you to set the name of the sum column that will be generated.

```
TableName
| where ColumnA=="test"
| summarize NewCountName = count()
	        , otherAvgName = avg(ColumnB)
	        by ColumnA
```

This will return the number of ColumnA entries containing "test" and the average of the associated ColumnB

```
TableName
| summarize NumberOfEntries = count()
	        by bin(TimeGenerated, 7d)
```

bin allows you to summarize into logical groups. The example above groups the count() values by days.



### Extend  

```
TableName
| where ColumnA == "test"
| extend NewColumn = ColumnB / 1000
```

Extend adds another column to the returned data (NewColumn) with the specified result. 

Eg. If ColumnB was megabits, the NewColumn would should kilobits

```
TableName
| where TimeGenerated >=ago(10m)
| extend ColumnA = strcat(ColumnB, " - ", columnC)
```

This will create a new column called ColumnA with the text from ColumnB and columnC


### Project  

```
TableName
| project ColumnA
	    , ColumnB
	    , ColumnC
```

Shows only ColumnA, ColumnB and ColumnC in the results

```
TableName
| where ColumnA == "something"
| extend FreeGB = SpaceLeft / 1000
	    , FreeMB = SpaceLeft 
	    , FreeKB = SpaceLeft * 1000
| project ColumnB
	    , ColumnC
	    , FreeGB
	    , FreeMB
	    , FreeKB
```

This gest all the rows where ColumnA is "something" and calculates 3 new columns based off of the SpaceLeft column. It then only shows ColumnB, ColumnC, FreeGB, FreeMB and FreeKB.

```
TableName
| project-away ColumnA
```

This will omit the ColumnA from results.

```
TableName
| project-rename ColumnA = Test
```

This will rename the Test column to ColumnA

### Distinct 

```
TableName
| distinct ColumnA, ColumnB
```

Returns all unique combinations of ColumnA + ColumnB

```
TableName
| where ColumnA == "Error"
| distinct Source Column
```

### Top

```
TableName
| top 20 by TimeGenerated desc
```

Returns the most recent 20 entries

```
TableName
| where ColumnA == "test"  //get all test
	    And ColumnB >=ago(1h)  //within the last hour
| project ColumnC
	    , ColumnD
	    , ColumnF=ColumnE  //rename inline
| distinct ColumnC
	    , ColumnD
	    , ColumnE
| top 25 by Column E asc
```