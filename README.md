<!-- markdownlint-disable MD001 MD033 MD034 MD041 -->
# Dockerfile of SQLite3

Alpine Docker image of SQLite3 built from the latest source code.

- Current SQLite3 version: [![SQLite Version](https://img.shields.io/endpoint?url=https%3A%2F%2Fraw.githubusercontent.com%2FKEINOS%2FDockerfile_of_SQLite3%2Fmaster%2FSQLite3-shields.io-badge.json)](https://github.com/KEINOS/Dockerfile_of_SQLite3/blob/master/VERSION_SQLite3.txt)
  - [View Available Tags (SQLite version)](https://hub.docker.com/r/keinos/sqlite3/tags) @ DockerHub
- Supported Architecture:
  - AMD64, ARM64, ARMv6, ARMv7
- [![Snyk Docker Scan](https://github.com/KEINOS/Dockerfile_of_SQLite3/actions/workflows/snyk_scan.yml/badge.svg)](https://github.com/KEINOS/Dockerfile_of_SQLite3/actions/workflows/snyk_scan.yml) [![Azure Container Scan](https://github.com/KEINOS/Dockerfile_of_SQLite3/actions/workflows/azure_scan.yml/badge.svg)](https://github.com/KEINOS/Dockerfile_of_SQLite3/actions/workflows/azure_scan.yml)

<details><summary>Image Information (Dockerfile, Security Scan, etc.)</summary>

- Repositories/Registries:
  - [Dockerfile](https://github.com/iomax/Dockerfile_of_SQLite3/blob/master/Dockerfile) @ GitHub
- Build Info:
  - Base Image: `alpine:latest`
  - SQLite3 Source: [https://www.sqlite.org/src/](https://www.sqlite.org/src/doc/trunk/README.md) @ SQLite.org

</details>

## Usage

```

Build the latest image locally as below:

```shellsession
$ docker build -t sqlite3:local https://github.com/iomax/Dockerfile_of_SQLite3.git
...
```

### Interactive

Running `sqlite3` command inside the container interactively.

```shellsession
$ docker run --rm -it -v "$(pwd):/workspace" -w /workspace sqlite3:local
SQLite version 3.28.0 2019-04-16 19:49:53
Enter ".help" for usage hints.
Connected to a transient in-memory database.
Use ".open FILENAME" to reopen on a persistent database.
sqlite> .open ./sample.db
sqlite> CREATE TABLE table_sample(timestamp TEXT, description TEXT);
sqlite> INSERT INTO table_sample VALUES(datetime("now"),"First sample data. Foo");
sqlite> INSERT INTO table_sample VALUES(datetime("now"),"Second sample data. Bar");
sqlite> .quit
$ ls
sample.db
```

- Note that you need to mount the working directory as a volume to the container.

### Command

- Running `sqlite3 --version` command:

```shellsession
$ docker run --rm sqlite3:local sqlite3 --version
3.38.2 2022-03-26 13:51:10 d33c709cc0af66bc5b6dc6216eba9f1f0b40960b9ae83694c986fbf4c1d6f08f
```

- Executing SQL query to the mounted database:

```shellsession
$ ls
sample.db
$ docker run --rm -it -v "$(pwd):/workspace" sqlite3:local sqlite3 /workspace/sample.db -header -column 'SELECT rowid, * FROM table_sample;'
rowid  timestamp            description
-----  -------------------  -----------------------
1      2022-04-16 14:09:52  First sample data. Foo
2      2022-04-16 14:09:58  Second sample data. Bar
```

- Note that you need to mount the working directory as a volume to the container.

### Run test

This container includes a [simple test script](https://github.com/iomax/Dockerfile_of_SQLite3/blob/master/run-test.sh).

You can run the script to see if the container and `sqlite3` binary is working.

```shellsession
$ docker run --rm sqlite3:local /run-test.sh
- Creating test DB ... created
rowid  timestamp            description
-----  -------------------  -----------------------
1      2022-04-16 14:18:34  First sample data. Hoo
2      2022-04-16 14:18:34  Second sample data. Bar
- Testing ...
  1st row value ... OK
  2nd row value ... OK

- Test result:
success
$ echo $?
0
```



## License

- [MIT License](https://github.com/iomax/Dockerfile_of_SQLite3/blob/master/LICENSE.md) by [The Dockerfile of SQLite3 Contributors](https://github.com/iomax/Dockerfile_of_SQLite3/graphs/contributors).
  - SQLite: [Public Domain](https://sqlite.org/copyright.html) by [D. Richard Hipp](https://en.wikipedia.org/wiki/D._Richard_Hipp) and [SQLite.org](https://sqlite.org/).
