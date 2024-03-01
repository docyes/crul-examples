# Queries

The main way to use crul is through authoring and running queries. These queries are composed of one or more stages, "piped" together using the double pipe `||` syntax.

# Grammar

Queries can be broken up into 3 distinct parts:
1. Query itself
2. Individual stages
3. Command run in a stage (one of the commands in the crul query language)

Each stage runs sequentially, and operates on the results of the previous stage.

# Commands

A command has a `name`, optional `argument(s)` and optional `flag(s)`:

## Example

In this example the `seed` command generates a table of key value/pairs based on the first `argument` of a JSON string followed by a `flag` that only shows the key1 column.

```bash
seed '{ key1: "value1", key2: "value2" }' --table key1
```

### Name

A command name is a string with no spaces that denotes some form of action to perform.

```bash
seed
```

### Argument(s)

Arguments can be optional or required with multiple values seperated by a space. An argument can be a boolean, string or number. Strings with special chararacters can be escaped with enclosing quotes either single `'`, double `"` or <code>\`\`\`</code> triple quotes. 

```bash
`{ key1: "value1", key2: "value2" }`
```

### Flag(s)

A flag can be a boolean, string or number. Strings with special chararacters can be escaped with enclosing quotes either single `'`, double `"` or <code>\`\`\`</code> triple quotes.  Some commands support multiple flags with the same name and order is preserved.

```bash
--table key1
```

# Object Flattening (Tabulation)

Crul flattens hierarchical data structures into tabular datasets to simplify the process of filtering, shaping and transforming.

## Example

````bash
seed ```json
{
  key: "root key value",
  nested: {
    key: "nested key value" 
  }
}
```
````

## Result

| key    | nested.key |
| -------- | ------- |
| root key value  | nested key value |


# Nested Collections 

Data structures can have a collection of multiple items assigned to single key name. In the flattening process of hierarchical data structures with a key having multple items assigned to it (collection), the namespace will be prefixed with a zero-based numerical index. 

## Example 

````bash
seed ```json
{
  root: "this is the root",
  collection: [
    {key: "value 1"},
    {key: "value 2"}
  ]
}
```
````

## Result

| root    | collection.0.key | collection.1.key |
| -------- | ------- | ------- |
| this is the root | value 1 | value 2 |

# Nested Collection Row Conversion

Data structures that have a collection of multiple items assigned to single key name can be expanded into multiple rows using the `normalize` command. A prefix can be normalized into multiple roles including the shared keys at the root level.  

## Example

````bash
seed ```json
{
  root: "this is the root",
  collection: [
    {key: "value 1"},
    {key: "value 2"}
  ]
}
```
|| normalize collection
````

## Results

| root    | key | 
| -------- | ------- | 
| this is the root | value 1 | 
| this is the root | value 2 | 

# Retrieval Commands

Are used to retrieve data from a web source, such as REST, SOAP, GraphQL, Web Browser, etc.

# API Retrieval Command

The `api` command allows you to make REST requests to an endpoint, and returns the response in a tabular form. Currently this command supports XML, CSV, Parquet, ZIP, GZIP, Octet-Stream, Text and JSON response formats with unsupported formats returned as raw data.

## Example

Get all the cisco github repo members.

```bash
api get https://api.github.com/orgs/cisco/members
```

# GraphQL Retrieval Command

The `graphql` command allows you to make a query against a GraphQL API.

## Example

Get all the upcomming SpaceX launches.

```bash
graphql https://spacex-production.up.railway.app 'query SpaceXQuery {
  launchesUpcoming (limit: 100) {
    id
    mission_name
    launch_date_unix
    launch_success    
    rocket {
      rocket_name
      rocket {
        wikipedia
      }
    }    
  }
}'
|| normalize launchesUpcoming
```

# SOAP Retrieval Command

The `soap` command allows you to make a query against a SOAP service using a WSDL.

## Example

Get all the Workday signons. 

```bash
oauth --credential workday
|| soap '{
  "Request_Criteria": {"From_DateTime": "2023-07-17T12:15:00-07:00", "To_DateTime": "2023-07-17T13:15:00-07:00"},
  "Response_Filter": { "Page": 1, "Count": 100, "As_Of_Entry_DateTime": "2023-07-17T12:15:00-07:00" }
}'
--wsdl Identity_Management_edit
--method "Get_Workday_Account_Signons"
--security.bearer "$token.access_token$"
```
