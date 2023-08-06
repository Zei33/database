# Node Database

This is an implementation of the MySQL2 library for NodeJS.

This package offers a shared database object when imported into another project.
For large and complex projects, there will be many files that need to have access to the database connection.
Instead of passing a reference to a connection through to each file through functions or initialisers, simply import this package at the top of the file instead.
The connection will only actually be instantiated once, no matter how many times the package is imported.

Install the package by cloning it into a directory outside of your project.
Then within your project, run `npm install file:../database`.

Finally, within your project root directory, create a file named `database.json`.
```json
{
	"databases": [
		{
		"host": "localhost",
		"user": "username",
		"password": "password",
		"database": "main",
		"flags": "-FOUND_ROWS",
		"charset": "utf8mb4_0900_ai_ci",
		"multipleStatements": true,
		"connectionLimit": 10,
		"queueLimit": 0,
		"timezone": "+00:00",
		"type": "source",
		"label": "dev"
		}
	]
}
```

You may define as many connections as you want in the `databases` array. 
The connection parameters are the same as mysql2 with two additions.
The `type` key can be either `source` or `target`. 
There must be only one source but can be any number of targets.
All `query()` will be run on all connections, but only the source connection will return results.
The `label` parameter is required to distinguish which connection caused an error.

To use it in your project, simply call `const connection = require("database");` in any file within your project, anywhere you need to access the database.
There is no disadvantage to calling it as many times as you like.
