# Running MS SQL Server

## Prerequisites

Installing the driver on mac:
```bash
brew tap microsoft/mssql-release https://github.com/Microsoft/homebrew-mssql-release\nbrew update\nHOMEBREW_ACCEPT_EULA=Y brew install msodbcsql18 mssql-tools18

brew install mssql-tools18
```


## Starting the Database
Spin up the database:
```bash
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=Passw0rd" -p 1433:1433 --name sql1 --hostname sql1 --network host –rm   mcr.microsoft.com/mssql/server:2022-latest
```

## Connecting a client
```bash
sudo docker exec -it sql1 "bash"
/opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P "Passw0rd"
```

## Connecting Externally

Getting your IP address:
```bash
ifconfig | grep "inet " | grep -v 127.0.0.1 | head
```

Then
```bash
sqlcmd -S <your ip>,1433 -U SA -P "Passw0rd" -C 
```

## Starting Jupyter
```bash
jupyter lab
```
or, if jupyter is already running:
```bash
jupyter server list
```

# Resources
 * [Quickstart: Run SQL Server Linux container images with Docker](https://learn.microsoft.com/en-gb/sql/linux/quickstart-install-connect-docker?view=sql-server-ver16&pivots=cs1-bash)
 * [sqlcmd](https://learn.microsoft.com/en-us/sql/tools/sqlcmd/sqlcmd-use-utility?view=sql-server-ver16)
 * [Microsoft docs](https://learn.microsoft.com/en-us/sql/tools/sqlcmd/sqlcmd-use-utility?view=sql-server-ver16)


# Appendix

### Checking if remote access is enabled:
```
EXEC sp_configure 'remote access';
GO

SELECT name, value_in_use FROM sys.configurations WHERE name like '%access%';
GO
```

### SQL Cheatsheet / Experiments
```sql
CREATE DATABASE TestDB;
SELECT Name from sys.databases;
GO

USE TestDB;
CREATE TABLE Inventory (id INT, name NVARCHAR(50), quantity INT);
INSERT INTO Inventory VALUES (1, 'banana', 150); INSERT INTO Inventory VALUES (2, 'orange', 154);
SELECT * FROM Inventory WHERE quantity > 152;
GO
```