---
title: "Python 用 Azure MySQL ライブラリ"
description: 
keywords: "Azure, Python, SDK, API, SQL, データベース, MySQL"
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/12/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: mysql
ms.openlocfilehash: f03134bfddfabc426cbcaf4d98ef86d14038861f
ms.sourcegitcommit: 41e90fe75de03d397079a276cdb388305290e27e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/23/2018
---
# <a name="azure-mysql-libraries-for-python"></a>Python 用 Azure MySQL ライブラリ 

## <a name="overview"></a>概要

MySQL Manager と pyodbc を使用して、Python から [Azure MySQL Database](/azure/mysql/overview) に格納されたリソースやデータを処理します。

## <a name="client-odbc-driver-and-pyodbc"></a>クライアント ODBC ドライバーと pyodbc

Azure Database for MySQL にアクセスするための推奨クライアント ライブラリは、Microsoft [ODBC ドライバー](/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries)です。 ODBC ドライバーを使用してデータベースに接続し、SQL ステートメントを直接実行します。

### <a name="example"></a>例

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

## <a name="management-api"></a>管理 API

Management API を使用して、ご利用のサブスクリプションの MySQL リソースの作成や管理を行うことができます。

### <a name="requirements"></a>必要条件
Python 用 MySQL 管理ライブラリをインストールする必要があります。
```bash
pip install azure-mgmt-rdbms
```

管理クライアントを使用して認証用の資格情報を取得する方法の詳細については、[Python SDK 認証](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate)に関するページを参照してください。

### <a name="example"></a>例

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
> [Management API を探す](/python/api/overview/azure/mysql/management)