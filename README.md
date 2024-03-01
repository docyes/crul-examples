# Commands

A command has a `name`, optional `argument(s)` and optional `flag(s)`:

## Example

In this example the `seed` command generates a table of key value/pairs based on the first argument of a JSON string followed by a flag that only shows the key1 column.

```bash
seed '{ key1: "value1", key2: "value2" }' --table key1
```

### Name

A command name is a string with no spaces that denotes some form of action to perform.

ex.,
```bash
seed
```

### Argument

An argument can be a boolean, string or number. Strings with special chararacters can be escaped with enclosing quotes either single `'`, double `"` or `\`\`\`` triple quotes. 

```bash
`{ key1: "value1", key2: "value2" }`
```

### Flag
```bash
--table key1
```


