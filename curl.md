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
`-d`        | Send data to the server. If the data starts with `@` the rest should be a filename to read the data from
`-c`/`-b`   | Respectively writes/reads cookies to/from specified file


## Examples

POST a JSON file 

```bash
curl -X POST -H "Content-Type: application/json" -d @file.json https://example.com
```

Subimit a login form, store the cookie and use it for next request

```bash
curl -X POST -d "login=user&pass=secret" -c cookie.txt https://example.com/login
curl -b cookie.txt https://example.com/profile
```
