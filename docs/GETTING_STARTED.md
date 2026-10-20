# Run your own investigation

## Prerequisites

Use a disposable SQL Server database, a disposable MongoDB database, Apache JMeter, and a Java runtime compatible with your chosen JMeter and driver versions. The historical versions are recorded in [the study](STUDY.md).

Install a Microsoft SQL Server JDBC driver and a compatible MongoDB Java driver set in JMeter's `lib` directory, then restart JMeter. The plan uses legacy `com.mongodb.MongoClient` APIs and also imports `com.mongodb.client.MongoClients`; verify both are available in a coherent dependency set. Javadoc archives are not runtime drivers.

## Prepare the data

1. Obtain the Stack Overflow data used for your experiment. No backup is bundled here.
2. Restore the SQL Server database and import or migrate corresponding data to MongoDB.
3. Verify Posts, Users, and PostTypes row/document counts and join-key mappings.
4. Inspect indexes and record their definitions before measurement.
5. Reconcile the standalone MongoDB scripts (`posts`, `users`, `postTypes`, often `_id`) with the JMeter collection (`Posts`) and numeric `Id` fields. Collection names and key types must match your import.

The original report points to a Stack Overflow SQL Server distribution. Its exact download, licensing conditions, and subset reconstruction must be checked separately. This repository does not automate provisioning.

## Configure JMeter

Open `Jmeter/Jmeter_SQLvsMongo.jmx` in JMeter. Connection settings use JMeter properties:

| Property | Default |
| --- | --- |
| `sql.url` | `jdbc:sqlserver://localhost:1433;databaseName=StackOverflow2010;encrypt=false;` |
| `sql.username` | Empty; supply your own |
| `sql.password` | Empty; supply your own |
| `mongo.host` | `localhost` |
| `mongo.port` | `27017` |
| `mongo.database` | `StackOverflow2010` |
| `mongo.username` | Empty; supply your own |
| `mongo.password` | Empty; supply your own |

The MongoDB authentication database is the selected database. Adjust the connection code if your user authenticates elsewhere. The default SQL URL reflects the original local experiment; configure transport settings for your environment.

Place connection values in a local `benchmark.local.properties` file, using `property=value` lines. This filename is ignored by Git. Do not commit credentials.

## Validate before measurement

The full plan inserts 500,000 synthetic posts and updates/deletes matching records with negative IDs. Use restored test copies and check predicates first. Changing the user count also changes how often Once Only Controllers run: they execute once per thread.

Start with a small, individually selected workload in the GUI. Resolve driver, authentication, schema, and query-equivalence issues before executing the full plan. Disable GUI-heavy listeners for measurement and ensure failures are included in your result export.

## Export a run

After validation, run from the repository root using an installed JMeter command:

```sh
jmeter -n -t Jmeter/Jmeter_SQLvsMongo.jmx -q benchmark.local.properties -l results/run-001.jtl -e -o results/run-001-report
```

Use a fresh report directory for each run. Keep a run manifest with engine versions, dataset counts, indexes, host resources, thread settings, and warm-up policy. The checked-in plan uses one thread, a one-second ramp-up, and two loops. Configure and validate additional concurrency separately.

No databases or load tests were started when preparing this English edition.
