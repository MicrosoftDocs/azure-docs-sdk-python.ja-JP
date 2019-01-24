---
title: Python 用 Azure MySQL/PostgreSQL ライブラリ
description: ''
keywords: Azure, Python, SDK, API, SQL, データベース, MySQL, PostgreSQL
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 07/19/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.openlocfilehash: 402e87ae81e6df64b040293992244902313e5b1b
ms.sourcegitcommit: fba77bdf8eb9f49621be94544d9fef88aff98c14
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/23/2019
ms.locfileid: "54747722"
---
# <a name="azure-mysqlpostgresql-libraries-for-python"></a>Python 用 Azure MySQL/PostgreSQL ライブラリ

## <a name="mysql"></a>MySQL

MySQL Manager と pyodbc を使用して、Python から [Azure MySQL Database](/azure/mysql/overview) に格納されたリソースやデータを処理します。

### <a name="client-odbc-driver-and-pyodbc"></a>クライアント ODBC ドライバーと pyodbc

Azure Database for MySQL にアクセスするための推奨クライアント ライブラリは、Microsoft [ODBC ドライバー](/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries)です。 ODBC ドライバーを使用してデータベースに接続し、SQL ステートメントを直接実行します。

#### <a name="example"></a>例

Azure Database for MySQL に接続し、sales テーブル内の全レコードを選択します。 データベースの ODBC 接続文字列は、Azure Portal から取得できます。

```python
SERVER = 'YOUR_SEVER_NAME' + '.mysql.database.azure.com'
DATABASE = 'YOUR_DATABASE_NAME'
USERNAME = 'YOUR_MYSQL_USERNAME'
PASSWORD = 'YOUR_MYSQL_PASSWORD'

DRIVER = '{MySQL ODBC 5.3 UNICODE Driver}'
cnxn = pyodbc.connect(
    'DRIVER=' + DRIVER + ';PORT=3306;SERVER=' + SERVER + ';PORT=3306;DATABASE=' + DATABASE + ';UID=' + USERNAME + ';PWD=' + PASSWORD)
cursor = cnxn.cursor()
selectsql = "SELECT * FROM SALES"  # SALES is an example table name
cursor.execute(selectsql)
```

### <a name="management-api"></a>管理 API

Management API を使用して、ご利用のサブスクリプションの MySQL リソースの作成や管理を行うことができます。

#### <a name="requirements"></a>必要条件
Python 用 MySQL 管理ライブラリをインストールする必要があります。
```bash
pip install azure-mgmt-rdbms
```

管理クライアントを使用して認証用の資格情報を取得する方法の詳細については、[Python SDK 認証](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate)に関するページを参照してください。

#### <a name="example"></a>例

MySQL 5.7 Database リソースを作成し、ファイアウォール規則を使って、特定の範囲の IP アドレスにアクセスを制限します。

```python

from azure.mgmt.rdbms.mysql import MySQLManagementClient
from azure.mgmt.rdbms.mysql.models import ServerForCreate, ServerPropertiesForDefaultCreate, ServerVersion

SUBSCRIPTION_ID = "YOUR_AZURE_SUBSCRIPTION_ID"
RESOURCE_GROUP = "YOUR_AZURE_RESOURCE-GROUP_WITH_POSTGRES"
MYSQL_SERVER = "YOUR_DESIRED_MYSQL_SERVER_NAME"
MYSQL_ADMIN_USER = "YOUR_MYSQL_ADMIN_USERNAME"
MYSQL_ADMIN_PASSWORD = "YOUR_MYSQL_ADMIN_PASSOWRD"
LOCATION = "westus"  # example Azure availability zone, should match resource group


client = MySQLManagementClient(credentials=creds,
    subscription_id=SUBSCRIPTION_ID)

server_creation_poller = client.servers.create_or_update(
    resource_group_name=RESOURCE_GROUP,
    server_name=MYSQL_SERVER,
    ServerForCreate(
        ServerPropertiesForDefaultCreate(
            administrator_login=MYSQL_ADMIN_USER,
            administrator_login_password=MYSQL_ADMIN_PASSWORD,
            version=ServerVersion.five_full_stop_seven
        ),
        location=LOCATION
    )
)

server = server_creation_poller.result()

# Open access to this server for IPs
rule_creation_poller = client.firewall_rules.create_or_update(
    RESOURCE_GROUP
    MYSQL_SERVER,
    "some_custom_ip_range_whitelist_rule_name",  # Custom firewall rule name
    "123.123.123.123",  # Start ip range
    "167.220.0.235"  # End ip range
)

firewall_rule = rule_creation_poller.result()
```

> [!div class="nextstepaction"]
> [Management API を探す](/python/api/overview/azure/postgresql/mysql/management)

## <a name="postgresql"></a>PostgreSQL
ODBC ドライバーと pyodbc を使用してデータベースに接続し、SQL ステートメントを直接実行します。

Azure Database for PostgreSQL の詳細については、[こちら](https://docs.microsoft.com/azure/postgresql/)を参照してください。

### <a name="client-odbc-driver-and-pyodbc"></a>クライアント ODBC ドライバーと pyodbc
Azure Database for PostgreSQL にアクセスするための推奨クライアント ライブラリは、Microsoft [ODBC ドライバーと pyodbc](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries) です。

#### <a name="example"></a>例 

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

### <a name="management-api"></a>管理 API
#### <a name="requirements"></a>必要条件
Python 用 PostgreSQL 管理ライブラリをインストールする必要があります。
```bash
pip install azure-mgmt-rdbms
```

管理クライアントを使用して認証用の資格情報を取得する方法の詳細については、[Python SDK 認証](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate)に関するページを参照してください。

#### <a name="example"></a>例
この例では、既存の Postgres サーバー上に新しい Postgres データベースを作成します。
```python
from azure.mgmt.rdbms.postgresql import PostgreSQLManagementClient

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
> [Management API を探す](/python/api/overview/azure/postgresql/mysql/management)
