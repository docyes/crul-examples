# Commands

A command has a `name`, optional `argument(s)` and optional `flag(s)`:

## Example

In this example the `seed` command generates a table of key value/pairs based on the first `argument` of a JSON string followed by a `flag` that only shows the key1 column.

```bash
seed '{ key1: "value1", key2: "value2" }' --table key1
```

### Name

A command name is a string with no spaces that denotes some form of action to perform.

ex.,
```bash
seed
```

### Argument(s)

An argument can be a boolean, string or number. Strings with special chararacters can be escaped with enclosing quotes either single `'`, double `"` or <code>\`\`\`</code> triple quotes. Arguments can be optional or required with multiple values seperated by a space.

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

Data structures can have a collection of multiple items assigned to single key name. In the flattening process of hierarchical data structures if a key has multple items assigned to it (collection), the namespace will be prefixed with a zero-based numerical index. 


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

