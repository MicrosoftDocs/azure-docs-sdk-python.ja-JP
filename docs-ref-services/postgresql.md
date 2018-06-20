---
title: Python 用 Azure PostgreSQL ライブラリ
description: ''
keywords: Azure, Python, SDK, API, SQL, データベース, Postgres, PostgreSQL
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 07/11/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: postgresql
ms.openlocfilehash: cad5995072d5040764986765d9a900f46f5141ec
ms.sourcegitcommit: 41e90fe75de03d397079a276cdb388305290e27e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/23/2018
ms.locfileid: "29478935"
---
#<a name="azure-postgresql-libraries-for-python"></a><span data-ttu-id="9c912-103">Python 用 Azure PostgreSQL ライブラリ</span><span class="sxs-lookup"><span data-stu-id="9c912-103">Azure PostgreSQL libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="9c912-104">概要</span><span class="sxs-lookup"><span data-stu-id="9c912-104">Overview</span></span>
<span data-ttu-id="9c912-105">ODBC ドライバーと pyodbc を使用してデータベースに接続し、SQL ステートメントを直接実行します。</span><span class="sxs-lookup"><span data-stu-id="9c912-105">Use the ODBC driver and pyodbc to connect to the database and execute SQL statements directly.</span></span>

<span data-ttu-id="9c912-106">Azure Database for PostgreSQL の詳細については、[こちら](https://docs.microsoft.com/azure/postgresql/)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="9c912-106">Learn more about [Azure Database for PostgreSQL](https://docs.microsoft.com/azure/postgresql/).</span></span>

## <a name="client-odbc-driver-and-pyodbc"></a><span data-ttu-id="9c912-107">クライアント ODBC ドライバーと pyodbc</span><span class="sxs-lookup"><span data-stu-id="9c912-107">Client ODBC driver and pyodbc</span></span>
<span data-ttu-id="9c912-108">Azure Database for PostgreSQL にアクセスするための推奨クライアント ライブラリは、Microsoft [ODBC ドライバーと pyodbc](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries) です。</span><span class="sxs-lookup"><span data-stu-id="9c912-108">The recommended client library for accessing Azure Database for PostgreSQL is the Microsoft [ODBC driver and pyodbc](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries)</span></span>

### <a name="example"></a><span data-ttu-id="9c912-109">例</span><span class="sxs-lookup"><span data-stu-id="9c912-109">Example</span></span> 

<span data-ttu-id="9c912-110">Azure Database for PostgreSQL に接続し、`SALES` テーブル内の全レコードを選択します。</span><span class="sxs-lookup"><span data-stu-id="9c912-110">Connect to a Azure Database for PostgreSQL and select all records in the `SALES` table.</span></span> <span data-ttu-id="9c912-111">データベースの ODBC 接続文字列は、Azure Portal から取得できます。</span><span class="sxs-lookup"><span data-stu-id="9c912-111">You can get the ODBC connection string for the database from the Azure Portal.</span></span>

```python
import pyodbc

SERVER = 'YOUR_SERVER_NAME.postgres.database.azure.com'
DATABASE = 'YOUR_DB_NAME'
USERNAME = 'YOUR_USERNAME'
PASSWORD = 'YOUR_PASSWORD'

DRIVER = '{PostgreSQL ODBC Driver}'
cnxn = pyodbc.connect(
    'DRIVER=' + DRIVER + ';PORT=5432;SERVER=' + SERVER +
    ';PORT=5432;DATABASE=' + DATABASE + ';UID=' + USERNAME +
    ';PWD=' + PASSWORD)
cursor = cnxn.cursor()
selectsql = "SELECT * FROM SALES" # SALES is an example table name
cursor.execute(selectsql)
```

## <a name="management-api"></a><span data-ttu-id="9c912-112">管理 API</span><span class="sxs-lookup"><span data-stu-id="9c912-112">Management API</span></span>
### <a name="requirements"></a><span data-ttu-id="9c912-113">必要条件</span><span class="sxs-lookup"><span data-stu-id="9c912-113">Requirements</span></span>
<span data-ttu-id="9c912-114">Python 用 PostgreSQL 管理ライブラリをインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="9c912-114">You must install the PostgreSQL management libraries for Python.</span></span>
```bash
pip install azure-mgmt-rdbms
```

<span data-ttu-id="9c912-115">管理クライアントを使用して認証用の資格情報を取得する方法の詳細については、[Python SDK 認証](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="9c912-115">Please see the [Python SDK authentication](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate) page for details on obtaining credentials to authenticate with the management client.</span></span>

### <a name="example"></a><span data-ttu-id="9c912-116">例</span><span class="sxs-lookup"><span data-stu-id="9c912-116">Example</span></span>
<span data-ttu-id="9c912-117">この例では、既存の Postgres サーバー上に新しい Postgres データベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="9c912-117">In this example we will create a new Postgres database on our existing Postgres server.</span></span>
```python
from azure.mgtm.rdbms.postgresql import PostgreSQLManagementClient

SUBSCRIPTION_ID = "YOUR_AZURE_SUBSCRIPTION_ID"
RESOURCE_GROUP = "YOUR_AZURE_RESOURCE_GROUP_WITH_POSTGRES"
POSTGRES_SERVER = "YOUR_POSTGRES_SERVER_NAME"
DB_NAME = "YOUR_DESIRED_DATABASE_NAME"

client = PostgreSQLManagementClient(credentials, SUBSCRIPTION_ID)

db_creation_poller = client.databases.create_or_update(
    resource_group_name=RESOURCE_GROUP,
    server_name=POSTGRES_SERVER, database_name=DB_NAME)
db = db_creation_poller.result()
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="9c912-118">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="9c912-118">Explore the Management APIs</span></span>](/python/api/overview/azure/postgresql/management)

