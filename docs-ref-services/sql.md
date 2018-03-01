---
title: "Python 用 Azure SQL Database ライブラリ"
description: "ODBC ドライバーと pyodbc を使用して Azure SQL Database に接続したり、Management API を使用して Azure SQL インスタンスを管理したりします。"
author: lisawong19
ms.author: liwong
manager: routlaw
ms.date: 01/09/2018
ms.topic: reference
ms.devlang: python
ms.service: sql-database
ms.openlocfilehash: 6c442a7a1e639938c993e8c1e6f74bc5e0a730b7
ms.sourcegitcommit: 41e90fe75de03d397079a276cdb388305290e27e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/23/2018
---
# <a name="azure-sql-database-libraries-for-python"></a><span data-ttu-id="afb6b-103">Python 用 Azure SQL Database ライブラリ</span><span class="sxs-lookup"><span data-stu-id="afb6b-103">Azure SQL Database libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="afb6b-104">概要</span><span class="sxs-lookup"><span data-stu-id="afb6b-104">Overview</span></span>

<span data-ttu-id="afb6b-105">pyodbc [ODBC データベース ドライバー](https://github.com/mkleehammer/pyodbc/wiki/Drivers-and-Driver-Managers)を使用して、[Azure SQL Database](/azure/sql-database/sql-database-technical-overview) に格納されたデータを Python で処理します。</span><span class="sxs-lookup"><span data-stu-id="afb6b-105">Work with data stored in [Azure SQL Database](/azure/sql-database/sql-database-technical-overview) from Python with the pyodbc [ODBC database driver](https://github.com/mkleehammer/pyodbc/wiki/Drivers-and-Driver-Managers).</span></span> <span data-ttu-id="afb6b-106">Azure SQL Database への接続、Transact-SQL ステートメントを使用したデータの照会、pyodbc での[サンプル](https://github.com/mkleehammer/pyodbc/wiki/Getting-started)の使用に関する[クイック スタート](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="afb6b-106">View our [quickstart](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python) on connecting to an Azure SQL database and using Transact-SQL statements to query data and getting started [sample](https://github.com/mkleehammer/pyodbc/wiki/Getting-started) with pyodbc.</span></span>

## <a name="install-odbc-driver-and-pyodbc"></a><span data-ttu-id="afb6b-107">ODBC ドライバーと pyodbc のインストール</span><span class="sxs-lookup"><span data-stu-id="afb6b-107">Install ODBC driver and pyodbc</span></span>

```bash
pip install pyodbc
```
<span data-ttu-id="afb6b-108">Python とデータベースの通信ライブラリのインストールの詳細については、[こちら](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="afb6b-108">More [details](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries) about installing the python and database communication libraries.</span></span>

## <a name="connect-and-execute-a-sql-query"></a><span data-ttu-id="afb6b-109">接続と SQL クエリの実行</span><span class="sxs-lookup"><span data-stu-id="afb6b-109">Connect and execute a SQL query</span></span>

### <a name="connect-to-a-sql-database"></a><span data-ttu-id="afb6b-110">SQL Database への接続</span><span class="sxs-lookup"><span data-stu-id="afb6b-110">Connect to a SQL database</span></span>

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

### <a name="execute-a-sql-query"></a><span data-ttu-id="afb6b-111">SQL クエリの実行</span><span class="sxs-lookup"><span data-stu-id="afb6b-111">Execute a SQL query</span></span>

```python
cursor.execute("SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid")
row = cursor.fetchone()
while row:
    print (str(row[0]) + " " + str(row[1]))
    row = cursor.fetchone()
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="afb6b-112">pyodbc のサンプル</span><span class="sxs-lookup"><span data-stu-id="afb6b-112">pyodbc sample</span></span>](https://github.com/mkleehammer/pyodbc/wiki/Getting-started)

## <a name="connecting-to-orms"></a><span data-ttu-id="afb6b-113">ORM への接続</span><span class="sxs-lookup"><span data-stu-id="afb6b-113">Connecting to ORMs</span></span>

<span data-ttu-id="afb6b-114">pyodbc は、[SQLAlchemy](http://docs.sqlalchemy.org/en/latest/dialects/mssql.html?highlight=pyodbc#module-sqlalchemy.dialects.mssql.pyodbc) や [Django](https://github.com/lionheart/django-pyodbc/) などの他の ORM で動作します。</span><span class="sxs-lookup"><span data-stu-id="afb6b-114">pyodbc works with other ORMs such as [SQLAlchemy](http://docs.sqlalchemy.org/en/latest/dialects/mssql.html?highlight=pyodbc#module-sqlalchemy.dialects.mssql.pyodbc) and [Django](https://github.com/lionheart/django-pyodbc/).</span></span> 

## <a name="management-apipythonapioverviewazuresqlmanagement"></a>[<span data-ttu-id="afb6b-115">Management API</span><span class="sxs-lookup"><span data-stu-id="afb6b-115">Management API</span></span>](/python/api/overview/azure/sql/management)

<span data-ttu-id="afb6b-116">Management API を使用すると、ご利用のサブスクリプションの Azure SQL Database リソースを作成したり管理したりすることができます。</span><span class="sxs-lookup"><span data-stu-id="afb6b-116">Create and manage Azure SQL Database resources in your subscription with the management API.</span></span> 

```bash
pip install azure-common
pip install azure-mgmt-sql
pip install azure-mgmt-resource
```

## <a name="example"></a><span data-ttu-id="afb6b-117">例</span><span class="sxs-lookup"><span data-stu-id="afb6b-117">Example</span></span>

<span data-ttu-id="afb6b-118">SQL Database リソースを作成し、ファイアウォール規則を使って、特定の範囲の IP アドレスにアクセスを制限します。</span><span class="sxs-lookup"><span data-stu-id="afb6b-118">Create a SQL Database resource and restrict access to a range of IP addresses using a firewall rule.</span></span>

```python
RESOURCE_GROUP = 'YOUR_RESOURCE_GROUP_NAME'
LOCATION = 'eastus'  # example Azure availability zone, should match resource group
SQL_DB = 'YOUR_SQLDB_NAME'

# create resource client
resource_client = get_client_from_cli_profile(ResourceManagementClient)
# create resource group
resource_client.resource_groups.create_or_update(RESOURCE_GROUP, {'location': LOCATION})

sql_client = get_client_from_cli_profile(SqlManagementClient)

# Create a SQL server
server = sql_client.servers.create_or_update(
    RESOURCE_GROUP,
    SQL_DB,
    {
        'location': LOCATION,
        'version': '12.0', # Required for create
        'administrator_login': USERNAME, # Required for create
        'administrator_login_password': PASSWORD # Required for create
    }
)

# Open access to this server for IPs
firewall_rule = sql_client.firewall_rules.create_or_update(
    RESOURCE_GROUP
    SQL_DB,
    "firewall_rule_name_123.123.123.123",
    "123.123.123.123", # Start ip range
    "167.220.0.235"  # End ip range
)
```
> [!div class="nextstepaction"]
> [<span data-ttu-id="afb6b-119">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="afb6b-119">Explore the Management APIs</span></span>](/python/api/overview/azure/sql/management)

