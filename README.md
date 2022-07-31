# freebyMSSQL

The Microsoft SQL Server docker build for SQL Server Unit and System Test Scenarios.
Contains sqlpackage and the dotnet core 2.1 SDK for dacpac installs.

Here is a usage example for utilizing to create a dacpac install of a new database for system testing scenarios:

```
# Remove the last copy if it exists.
docker rm tiv-sql-localtest -f

# Set script variables.
pswd=SomePassword
db=dbName
dacpac=DbProject.dacpac

docker run -e 'ACCEPT_EULA=Y' -e "SA_PASSWORD=$pswd" \
   -p 1433:1433 --name tiv-sql-localtest \
   -d registry.freebytech.com/freebytech-pub/msssql:latest

docker cp ./$dacpac tiv-sql-localtest:/opt/downloads/$dacpac

docker exec -it tiv-sql-localtest bash -c "dotnet /opt/sqlpackage/sqlpackage.dll /Action:Publish /TargetConnectionString:\"Data Source=localhost;User ID=sa;Password=$pswd;Database=$db;Pooling=False\" /SourceFile:/opt/downloads/$dacpac /p:CreateNewDatabase=true"

```

More info about SQLPackage can be found here: https://docs.microsoft.com/en-us/sql/tools/sqlpackage/sqlpackage-download?view=sql-server-ver16

# Related Information

| Description                      | URL                                                    |
| -------------------------------- | ------------------------------------------------------ |
| SQL Package Latest Download      | https://docs.microsoft.com/en-us/sql/tools/sqlpackage/sqlpackage-download?view=sql-server-ver16 |
| SQL Package Release Notes        | https://docs.microsoft.com/en-us/sql/tools/sqlpackage/release-notes-sqlpackage?view=sql-server-ver16 |