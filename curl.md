cURL
====

## Options

Flag        | Description
-----------:|-----------------------------------------------------------------
`-O`/`-o`   | Save result to file (provide filename with `-o`)
`-s`        | Mute
`-sS`       | Mute (but show errors)
`-L`        | Follow redirections
`-i`/`-I`   | Show response header (only the header with `-I`)
`-H`        | Send HTTP header
`-X`        | Request type (e.g.: GET, POST, PUT, DELETE, …)
`-d`        | Send data to the server. If the data starts with `@` the rest should be a file name to read the data from


## Examples

POST a JSON file 

```bash
curl -X POST -H "Content-Type: application/json" -d @file.json example.com
```
