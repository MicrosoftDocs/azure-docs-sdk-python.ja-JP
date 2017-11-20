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
ms.openlocfilehash: 5382779d659f7c85e5ecbd304920e00b78a08a49
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/18/2017
---
# <a name="azure-cosmosdb-libraries-for-python"></a><span data-ttu-id="fa762-104">Python 用 Azure CosmosDB ライブラリ</span><span class="sxs-lookup"><span data-stu-id="fa762-104">Azure CosmosDB libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="fa762-105">概要</span><span class="sxs-lookup"><span data-stu-id="fa762-105">Overview</span></span>

<span data-ttu-id="fa762-106">Python アプリケーションで CosmosDB を使用して、NoSQL データ ストアに JSON ドキュメントを格納し、クエリを実行します。</span><span class="sxs-lookup"><span data-stu-id="fa762-106">Use CosmosDB in your Python applications to store and query JSON documents in a NoSQL data store.</span></span>

<span data-ttu-id="fa762-107">詳細については、[Azure CosmosDB](https://docs.microsoft.com/azure/cosmos-db/introduction) に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="fa762-107">Learn more about [Azure CosmosDB](https://docs.microsoft.com/azure/cosmos-db/introduction).</span></span>

## <a name="client-library"></a><span data-ttu-id="fa762-108">クライアント ライブラリ</span><span class="sxs-lookup"><span data-stu-id="fa762-108">Client library</span></span>
 ```bash
pip install pydocumentdb
 ```

## <a name="management-library"></a><span data-ttu-id="fa762-109">管理ライブラリ</span><span class="sxs-lookup"><span data-stu-id="fa762-109">Management library</span></span>
```bash
pip install azure-mgmt-cosmosdb
```

### <a name="example"></a><span data-ttu-id="fa762-110">例</span><span class="sxs-lookup"><span data-stu-id="fa762-110">Example</span></span>

<span data-ttu-id="fa762-111">SQL に似たクエリ インターフェイスを使用して、CosmosDB で一致するドキュメントを探します。</span><span class="sxs-lookup"><span data-stu-id="fa762-111">Find matching documents in CosmosDB using a SQL-like query interface:</span></span>

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
> [<span data-ttu-id="fa762-112">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="fa762-112">Explore the Management APIs</span></span>](/python/api/overview/azure/cosmosdb/managementlibrary)

## <a name="samples"></a><span data-ttu-id="fa762-113">サンプル</span><span class="sxs-lookup"><span data-stu-id="fa762-113">Samples</span></span>

[<span data-ttu-id="fa762-114">Azure Cosmos DB の DocumentDB API を使用して Python アプリを開発する</span><span class="sxs-lookup"><span data-stu-id="fa762-114">Develop a Python app using Azure Cosmos DB's DocumentDB API</span></span>](https://azure.microsoft.com/resources/samples/azure-cosmos-db-documentdb-python-getting-started/)


