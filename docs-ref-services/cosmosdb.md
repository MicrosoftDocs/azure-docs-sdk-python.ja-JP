---
title: Azure Cosmos DB Libraries for Python
description: Python 用 Azure Cosmos DB クライアント ライブラリのリファレンス ドキュメント
keywords: Azure, Python, SDK, API, SQL, データベース, PostGres,Cosmos DB, NoSQL
author: lisawong19
ms.author: routlaw
manager: douge
ms.date: 03/20/2018
ms.topic: article
ms.devlang: python
ms.service: cosmosdb
ms.openlocfilehash: bb5e2af6a28d9543cce0c1e80fab1575b78f8cfa
ms.sourcegitcommit: 46bebbf5dd558750043ce5afadff2ec3714a54e6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2019
ms.locfileid: "67534314"
---
# <a name="azure-cosmos-db-libraries-for-python"></a>Azure Cosmos DB Libraries for Python

## <a name="overview"></a>概要

Python アプリケーションで Azure Cosmos DB を使用して、NoSQL データ ストアに JSON ドキュメントを格納し、クエリを実行します。

[Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction) の詳細をご覧ください。

## <a name="client-library"></a>クライアント ライブラリ
 ```bash
pip install pydocumentdb
 ```

## <a name="management-library"></a>管理ライブラリ
```bash
pip install azure-mgmt-cosmosdb
```

### <a name="example"></a>例

SQL に似たクエリ インターフェイスを使用して、Azure CosmosDB で一致するドキュメントを探します。

```python
import pydocumentdb
import pydocumentdb.document_client as document_client

# Initialize the Python Azure Cosmos DB client
client = document_client.DocumentClient(config['ENDPOINT'], {'masterKey': config['MASTERKEY']})
# Create a database
db = client.CreateDatabase({ 'id': config['DOCUMENTDB_DATABASE'] })

# Create collection options
options = {
    'offerEnableRUPerMinuteThroughput': True,
    'offerVersion': "V2",
    'offerThroughput': 400
}

# Create a collection
collection = client.CreateCollection(db['_self'], { 'id': config['DOCUMENTDB_COLLECTION'] }, options)

# Create some documents
document1 = client.CreateDocument(collection['_self'],
    { 
        'id': 'server1',
        'Web Site': 0,
        'Cloud Service': 0,
        'Virtual Machine': 0,
        'name': 'some' 
    })

# Query them in SQL
query = { 'query': 'SELECT * FROM server s' }    

options = {} 
options['enableCrossPartitionQuery'] = True
options['maxItemCount'] = 2

result_iterable = client.QueryDocuments(collection['_self'], query, options)
results = list(result_iterable)

print(results)
```
> [!div class="nextstepaction"]
> [Management API を探す](/python/api/overview/azure/cosmosdb/management)

## <a name="samples"></a>サンプル

* [Azure Cosmos DB SQL API アカウントに保存されたデータにアクセスして管理するための Python アプリを開発する](https://github.com/Azure-Samples/azure-cosmos-db-python-getting-started.git)

* [Azure Cosmos DB MongoDB API アカウントに保存されたデータにアクセスして管理するための Python アプリを開発する](https://github.com/Azure-Samples/CosmosDB-Flask-Mongo-Sample.git)

* [Azure Cosmos DB Gremlin API アカウントに保存されたデータにアクセスして管理するための Python アプリを開発する](https://github.com/Azure-Samples/azure-cosmos-db-graph-python-getting-started.git)

* [Azure Cosmos DB Cassandra API アカウントに保存されたデータにアクセスして管理するための Python アプリを開発する](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-python-getting-started.git)

* [Azure Cosmos DB Table API アカウントに保存されたデータにアクセスして管理するための Python アプリを開発する](https://github.com/Azure-Samples/storage-python-getting-started.git)


