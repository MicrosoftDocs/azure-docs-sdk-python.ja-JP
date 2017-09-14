---
title: "Python 用 Azure SQL Database ライブラリ"
description: 
keywords: "Azure, Python, SDK, API, SQL, データベース, pyodbc"
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 07/11/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: sql-database
ms.openlocfilehash: b580c5011412bc77fd8fd55b709a305be07e2316
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/18/2017
---
# <a name="azure-sql-database-libraries-for-python"></a><span data-ttu-id="79889-103">Python 用 Azure SQL Database ライブラリ</span><span class="sxs-lookup"><span data-stu-id="79889-103">Azure SQL Database libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="79889-104">概要</span><span class="sxs-lookup"><span data-stu-id="79889-104">Overview</span></span>

<span data-ttu-id="79889-105">Microsoft ODBC ドライバーと pyodbc を使用して、[Azure SQL Database](/azure/sql-database/sql-database-technical-overview) に格納されたデータを Python で処理します。</span><span class="sxs-lookup"><span data-stu-id="79889-105">Work with data stored in [Azure SQL Database](/azure/sql-database/sql-database-technical-overview) from Python with the Microsoft ODBC driver and pyodbc.</span></span> 

## <a name="client-odbc-driver-and-pyodbc"></a><span data-ttu-id="79889-106">クライアント ODBC ドライバーと pyodbc</span><span class="sxs-lookup"><span data-stu-id="79889-106">Client ODBC driver and pyodbc</span></span>

```bash
pip install pyodbc
```
<span data-ttu-id="79889-107">Python とデータベースの通信ライブラリのインストールの詳細については、[こちら](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="79889-107">More details about installing the python and database communication libraries can be found [here](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries).</span></span>

### <a name="example"></a><span data-ttu-id="79889-108">例</span><span class="sxs-lookup"><span data-stu-id="79889-108">Example</span></span>

<span data-ttu-id="79889-109">SQL データベースに接続して、テーブル内のすべてのレコードを選択します。</span><span class="sxs-lookup"><span data-stu-id="79889-109">Connect to a SQL database and select all records in a table.</span></span>

```python
import pyodbc 

SERVER = 'YOUR_SERVER_NAME.database.windows.net'
DATABASE = 'YOUR_DATABASE_NAME'
USERNAME = 'YOUR_DB_USERNAME'
PASSWORD = 'YOUR_DB_PASSWORD'

DRIVER= '{ODBC Driver 13 for SQL Server}'
cnxn = pyodbc.connect('DRIVER=' + DRIVER + ';PORT=1433;SERVER=' + SERVER +
    ';PORT=1443;DATABASE=' + DATABASE + ';UID=' + USERNAME + ';PWD=' + PASSWORD)
cursor = cnxn.cursor()
selectsql = "SELECT * FROM SALES"  # SALES is an example table name
cursor.execute(selectsql)
```

## <a name="management-api"></a><span data-ttu-id="79889-110">Management API</span><span class="sxs-lookup"><span data-stu-id="79889-110">Management API</span></span>

<span data-ttu-id="79889-111">Management API を使用すると、ご利用のサブスクリプションの Azure SQL Database リソースを作成したり管理したりすることができます。</span><span class="sxs-lookup"><span data-stu-id="79889-111">Create and manage Azure SQL Database resources in your subscription with the management API.</span></span> 

```bash
pip install azure-mgmt-sql
```

### <a name="example"></a><span data-ttu-id="79889-112">例</span><span class="sxs-lookup"><span data-stu-id="79889-112">Example</span></span>

<span data-ttu-id="79889-113">SQL Database リソースを作成し、ファイアウォール規則を使って、特定の範囲の IP アドレスにアクセスを制限します。</span><span class="sxs-lookup"><span data-stu-id="79889-113">Create a SQL Database resource and restrict access to a range of IP addresses using a firewall rule.</span></span>

```python
RESOURCE_GROUP = 'YOUR_RESOURCE_GROUP_NAME'
LOCATION = 'eastus'  # example Azure availability zone, should match resource group
SQL_DB = 'YOUR_SQLDB_NAME'

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
> [<span data-ttu-id="79889-114">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="79889-114">Explore the Management APIs</span></span>](/python/api/overview/azure/sql/managementlibrary)

## <a name="samples"></a><span data-ttu-id="79889-115">サンプル</span><span class="sxs-lookup"><span data-stu-id="79889-115">Samples</span></span>

* <span data-ttu-id="79889-116">[SQL データベースの作成と管理][1]</span><span class="sxs-lookup"><span data-stu-id="79889-116">[Create and manage SQL databases][1]</span></span>    
* <span data-ttu-id="79889-117">[Python を使って接続し、データを照会する][2]</span><span class="sxs-lookup"><span data-stu-id="79889-117">[Use Python to connect and query data][2]</span></span>   

[1]: https://github.com/Azure-Samples/sql-database-python-manage
[2]: https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python

<span data-ttu-id="79889-118">Azure SQL Database サンプルの[完全な一覧](https://azure.microsoft.com/resources/samples/?platform=python&term=SQL)を表示します。</span><span class="sxs-lookup"><span data-stu-id="79889-118">View the [complete list](https://azure.microsoft.com/resources/samples/?platform=python&term=SQL) of Azure SQL database samples.</span></span> 