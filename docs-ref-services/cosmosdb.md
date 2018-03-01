---
title: "Python 用 Azure CosmosDB ライブラリ"
description: "Python 用 CosmosDB クライアント ライブラリのリファレンス ドキュメント"
keywords: "Azure, Python, SDK, API, SQL, データベース, PostGres,CosmosDB, NoSQL"
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 08/11/2017
ms.topic: article
ms.devlang: python
ms.service: cosmosdb
ms.openlocfilehash: d56dd69f4fc4513034046f9f721608ad94ff5cfe
ms.sourcegitcommit: 41e90fe75de03d397079a276cdb388305290e27e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/23/2018
---
# <a name="azure-cosmosdb-libraries-for-python"></a>Python 用 Azure CosmosDB ライブラリ

## <a name="overview"></a>概要

Python アプリケーションで CosmosDB を使用して、NoSQL データ ストアに JSON ドキュメントを格納し、クエリを実行します。

詳細については、[Azure CosmosDB](https://docs.microsoft.com/azure/cosmos-db/introduction) に関するページを参照してください。

## <a name="client-library"></a>クライアント ライブラリ
 ```bash
pip install pydocumentdb
 ```

## <a name="management-library"></a>管理ライブラリ
```bash
pip install azure-mgmt-cosmosdb
```

### <a name="example"></a>例

SQL に似たクエリ インターフェイスを使用して、CosmosDB で一致するドキュメントを探します。

```python
import pydocumentdb
import pydocumentdb.document_client as document_client

# Initialize the Python DocumentDB client
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

[Azure Cosmos DB の DocumentDB API を使用して Python アプリを開発する](https://azure.microsoft.com/resources/samples/azure-cosmos-db-documentdb-python-getting-started/)


