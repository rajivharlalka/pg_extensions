# Tinkering with Postgres Extensions

## How to create an extension

- create control file

```control
# <extension_name> extension
comment = 'Extension Description'
default_version = Version of the extension
reloacatable = true
```

The control file contains some metadata regarding the extension to be created.

- create a makefile

```make
PG_CONFIG = pg_config
PGXS := $(shell $(PG_CONFIG) --pgxs)
include $(PGXS)
```

These three lines are static over all make files. Need to specify more variables above these for personal extensions.

A detail list of all such variables can be found [here](https://www.postgresql.org/docs/current/extend-pgxs.html)

- With postgres running locally on the system, run `make install` which will install the extension in the `$SHAREDIR/postgres/extensions` directory. 

- Login into psql and install the extension directly using the `CREATE EXTENSION <extension_name>` SQL command.

This build process is through PGXS. 

There also exists another universal extension store the [PGXN](https://pgxn.org) which provides a more uniform build system for extensions in postgres.

## Testing Extensions

To Test an extension, firstly an test SQL file in the `/sql` folder is required. The sql file must be named as `<extension_name>_test.sql`. 

- Run `make installcheck` which will run the tests.

### INFO 
For the first test, the expected outputs are not known. So the tests fail. But along with the failed test, an output file with the same name as the test file is generated. 

- Copy that to the `/expected` directory and manually edit the output of each sql as it should be expected.

- From the next runs, the expected folder will be used to check against the tests.


## Cleaning the directory

Run `make clean` to clean the directory with unnecessary files created during tests.


## Sample Extensions

1. base64
  - base36--0.0.1.sql has the implementation in pure SQL
  - base36--0.0.2.sql has the implementation in C code.

2. get_sum
  - get_sum--0.0.1.sql : Get Sum of Two Numbers in C

# References

- A detailed guide on how to write postgres functions in C can be found here : https://www.postgresql.org/docs/current/xfunc-c.html

- Detailed list of all posiible content in a control file can be found here : https://www.postgresql.org/docs/current/extend-extensions.html