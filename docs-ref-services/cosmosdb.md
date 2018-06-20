---
title: Azure Cosmos DB Libraries for Python
description: Python 用 Azure Cosmos DB クライアント ライブラリのリファレンス ドキュメント
keywords: Azure, Python, SDK, API, SQL, データベース, PostGres,Cosmos DB, NoSQL
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 03/20/2018
ms.topic: article
ms.devlang: python
ms.service: cosmosdb
ms.openlocfilehash: 391b556ece7d818406fa501763814eb7f0d50d22
ms.sourcegitcommit: 41e6e6b5469271f4ec497a322b460e2a2af2c73d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/28/2018
ms.locfileid: "30204139"
---
# <a name="azure-cosmos-db-libraries-for-python"></a><span data-ttu-id="affb0-104">Azure Cosmos DB Libraries for Python</span><span class="sxs-lookup"><span data-stu-id="affb0-104">Azure Cosmos DB libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="affb0-105">概要</span><span class="sxs-lookup"><span data-stu-id="affb0-105">Overview</span></span>

<span data-ttu-id="affb0-106">Python アプリケーションで Azure Cosmos DB を使用して、NoSQL データ ストアに JSON ドキュメントを格納し、クエリを実行します。</span><span class="sxs-lookup"><span data-stu-id="affb0-106">Use Azure Cosmos DB in your Python applications to store and query JSON documents in a NoSQL data store.</span></span>

<span data-ttu-id="affb0-107">[Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction) の詳細をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="affb0-107">Learn more about [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction).</span></span>

## <a name="client-library"></a><span data-ttu-id="affb0-108">クライアント ライブラリ</span><span class="sxs-lookup"><span data-stu-id="affb0-108">Client library</span></span>
 ```bash
pip install pydocumentdb
 ```

## <a name="management-library"></a><span data-ttu-id="affb0-109">管理ライブラリ</span><span class="sxs-lookup"><span data-stu-id="affb0-109">Management library</span></span>
```bash
pip install azure-mgmt-cosmosdb
```

### <a name="example"></a><span data-ttu-id="affb0-110">例</span><span class="sxs-lookup"><span data-stu-id="affb0-110">Example</span></span>

<span data-ttu-id="affb0-111">SQL に似たクエリ インターフェイスを使用して、Azure CosmosDB で一致するドキュメントを探します。</span><span class="sxs-lookup"><span data-stu-id="affb0-111">Find matching documents in Azure CosmosDB using a SQL-like query interface:</span></span>

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
> [<span data-ttu-id="affb0-112">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="affb0-112">Explore the Management APIs</span></span>](/python/api/overview/azure/cosmosdb/management)

## <a name="samples"></a><span data-ttu-id="affb0-113">サンプル</span><span class="sxs-lookup"><span data-stu-id="affb0-113">Samples</span></span>

[<span data-ttu-id="affb0-114">Azure Cosmos DB を使用して Python アプリを開発する</span><span class="sxs-lookup"><span data-stu-id="affb0-114">Develop a Python app using Azure Cosmos DB</span></span>](https://azure.microsoft.com/resources/samples/azure-cosmos-db-documentdb-python-getting-started/)


