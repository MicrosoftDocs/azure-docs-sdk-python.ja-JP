---
title: Python 用 Azure SQL Database ライブラリ
description: ODBC ドライバーと pyodbc を使用して Azure SQL データベースに接続したり、Management API を使用して Azure SQL インスタンスを管理したりします。
author: lisawong19
ms.author: liwong
manager: routlaw
ms.date: 01/09/2018
ms.topic: reference
ms.devlang: python
ms.service: sql-database
ms.openlocfilehash: 5b73977fb58ed3cb17d675784da921b0e199d165
ms.sourcegitcommit: 560362db0f65307c8b02b7b7ad8642b5c4aa6294
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/08/2018
ms.locfileid: "33901355"
---
# <a name="azure-sql-database-libraries-for-python"></a>Python 用 Azure SQL Database ライブラリ

## <a name="overview"></a>概要

pyodbc [ODBC データベース ドライバー](https://github.com/mkleehammer/pyodbc/wiki/Drivers-and-Driver-Managers)を使用して、[Azure SQL Database](/azure/sql-database/sql-database-technical-overview) に格納されたデータを Python で処理します。 Azure SQL データベースへの接続、Transact-SQL ステートメントを使用したデータの照会、pyodbc での[サンプル](https://github.com/mkleehammer/pyodbc/wiki/Getting-started)の使用に関する[クイック スタート](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python)をご覧ください。

## <a name="install-odbc-driver-and-pyodbc"></a>ODBC ドライバーと pyodbc のインストール

```bash
pip install pyodbc
```
Python とデータベースの通信ライブラリのインストールの詳細については、[こちら](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries)をご覧ください。

## <a name="connect-and-execute-a-sql-query"></a>接続と SQL クエリの実行

### <a name="connect-to-a-sql-database"></a>SQL Database への接続

```python
import pyodbc

server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
driver= '{ODBC Driver 13 for SQL Server}'

cnxn = pyodbc.connect('DRIVER='+driver+';PORT=1433;SERVER='+server+';PORT=1443;DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
```

### <a name="execute-a-sql-query"></a>SQL クエリの実行

```python
cursor.execute("SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid")
row = cursor.fetchone()
while row:
    print (str(row[0]) + " " + str(row[1]))
    row = cursor.fetchone()
```

> [!div class="nextstepaction"]
> [pyodbc のサンプル](https://github.com/mkleehammer/pyodbc/wiki/Getting-started)

## <a name="connecting-to-orms"></a>ORM への接続

pyodbc は、[SQLAlchemy](http://docs.sqlalchemy.org/en/latest/dialects/mssql.html?highlight=pyodbc#module-sqlalchemy.dialects.mssql.pyodbc) や [Django](https://github.com/lionheart/django-pyodbc/) などの他の ORM で動作します。 

## <a name="management-apipythonapioverviewazuresqlmanagement"></a>[Management API](/python/api/overview/azure/sql/management)

Management API を使用すると、ご利用のサブスクリプションの Azure SQL Database リソースを作成したり管理したりすることができます。 

```bash
pip install azure-common
pip install azure-mgmt-sql
pip install azure-mgmt-resource
```

## <a name="example"></a>例

SQL Database リソースを作成し、ファイアウォール規則を使って、特定の範囲の IP アドレスにアクセスを制限します。

```python
from azure.common.client_factory import get_client_from_cli_profile
from azure.mgmt.resource import ResourceManagementClient
from azure.mgmt.sql import SqlManagementClient

RESOURCE_GROUP = 'YOUR_RESOURCE_GROUP_NAME'
LOCATION = 'eastus'  # example Azure availability zone, should match resource group
SQL_SERVER = 'yourvirtualsqlserver'
SQL_DB = 'YOUR_SQLDB_NAME'
USERNAME = 'YOUR_USERNAME'
PASSWORD = 'YOUR_PASSWORD'

# create resource client
resource_client = get_client_from_cli_profile(ResourceManagementClient)
# create resource group
resource_client.resource_groups.create_or_update(RESOURCE_GROUP, {'location': LOCATION})

sql_client = get_client_from_cli_profile(SqlManagementClient)

# Create a SQL server
server = sql_client.servers.create_or_update(
    RESOURCE_GROUP,
    SQL_SERVER,
    {
        'location': LOCATION,
        'version': '12.0', # Required for create
        'administrator_login': USERNAME, # Required for create
        'administrator_login_password': PASSWORD # Required for create
    }
)

# Create a SQL database in the Basic tier
database = sql_client.databases.create_or_update(
    RESOURCE_GROUP,
    SQL_SERVER,
    SQL_DB,
    {
        'location': LOCATION,
        'collation': 'SQL_Latin1_General_CP1_CI_AS',
        'create_mode': 'default',
        'requested_service_objective_name': 'Basic'
    }
)

# Open access to this server for IPs
firewall_rule = sql_client.firewall_rules.create_or_update(
    RESOURCE_GROUP,
    SQL_DB,
    "firewall_rule_name_123.123.123.123",
    "123.123.123.123", # Start ip range
    "167.220.0.235"  # End ip range
)
```
> [!div class="nextstepaction"]
> [Management API を探す](/python/api/overview/azure/sql/management)

