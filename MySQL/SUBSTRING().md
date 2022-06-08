> reference link: [w3schools](https://www.w3schools.com/mysql/func_mysql_substring.asp), [StackOverflow](https://stackoverflow.com/questions/12771311/how-to-substring-a-mysql-table-column)

# MySQL SUBSTRING()
* **Note:** The SUBSTR() and MID() functions equals to the SUBSTRING() function.

## Syntax
```SQL
SUBSTRING(string, start, length)
OR
SUBSTIRNG(string FROM start FOR length)
```

### Parameter Values

|Parameter|Description|
|---|---|
|string|Required. The string to extract from|
|start|Required. The start position. Can be both a positive or negative number. If it is a positive number, this function extracts from the beginning of the string. If it is a negative number, this function extracts from the end of the string|
|length|Optional. The number of characters to extract. If omitted, the whole string will be returned (from the start position)|

* **Precautions:** start의 값이 0이 되면 오류가 발생한다. 처음부터 시작하려면 start 값에 1을 대입해야 한다(보통 프로그래밍 언어에서, 배열은 0부터 시작하므로, 헷갈리기 쉽다)

## Example

### Example 1: String
```SQL
SELECT SUBSTRING("Welcome to SQL", -5, 5) AS ExtractString
```

### Example 2: Column
```SQL
SELECT commit_id, project_name, SUBSTRING(old_commit, 1, 4) 
FROM commits;
```
