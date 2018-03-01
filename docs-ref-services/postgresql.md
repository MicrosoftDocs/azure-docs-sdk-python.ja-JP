---
title: "Python 用 Azure PostgreSQL ライブラリ"
description: 
keywords: "Azure, Python, SDK, API, SQL, データベース, Postgres, PostgreSQL"
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
---
#<a name="azure-postgresql-libraries-for-python"></a>Python 用 Azure PostgreSQL ライブラリ

## <a name="overview"></a>概要
ODBC ドライバーと pyodbc を使用してデータベースに接続し、SQL ステートメントを直接実行します。

Azure Database for PostgreSQL の詳細については、[こちら](https://docs.microsoft.com/azure/postgresql/)を参照してください。

## <a name="client-odbc-driver-and-pyodbc"></a>クライアント ODBC ドライバーと pyodbc
Azure Database for PostgreSQL にアクセスするための推奨クライアント ライブラリは、Microsoft [ODBC ドライバーと pyodbc](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries) です。

### <a name="example"></a>例 

Azure Database for PostgreSQL に接続し、`SALES` テーブル内の全レコードを選択します。 データベースの ODBC 接続文字列は、Azure Portal から取得できます。

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

## <a name="management-api"></a>管理 API
### <a name="requirements"></a>必要条件
Python 用 PostgreSQL 管理ライブラリをインストールする必要があります。
```bash
pip install azure-mgmt-rdbms
```

管理クライアントを使用して認証用の資格情報を取得する方法の詳細については、[Python SDK 認証](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate)に関するページを参照してください。

### <a name="example"></a>例
この例では、既存の Postgres サーバー上に新しい Postgres データベースを作成します。
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
> [Management API を探す](/python/api/overview/azure/postgresql/management)

