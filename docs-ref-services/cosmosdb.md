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
ms.openlocfilehash: c2f3ea017a8864d4d2fb74a439c420f1f0313082
ms.sourcegitcommit: f439ba940d5940359c982015db7ccfb82f9dffd9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/21/2018
ms.locfileid: "52276796"
---
# <a name="azure-cosmos-db-libraries-for-python"></a><span data-ttu-id="6cdcc-104">Azure Cosmos DB Libraries for Python</span><span class="sxs-lookup"><span data-stu-id="6cdcc-104">Azure Cosmos DB libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="6cdcc-105">概要</span><span class="sxs-lookup"><span data-stu-id="6cdcc-105">Overview</span></span>

<span data-ttu-id="6cdcc-106">Python アプリケーションで Azure Cosmos DB を使用して、NoSQL データ ストアに JSON ドキュメントを格納し、クエリを実行します。</span><span class="sxs-lookup"><span data-stu-id="6cdcc-106">Use Azure Cosmos DB in your Python applications to store and query JSON documents in a NoSQL data store.</span></span>

<span data-ttu-id="6cdcc-107">[Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction) の詳細をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="6cdcc-107">Learn more about [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction).</span></span>

## <a name="client-library"></a><span data-ttu-id="6cdcc-108">クライアント ライブラリ</span><span class="sxs-lookup"><span data-stu-id="6cdcc-108">Client library</span></span>
 ```bash
pip install pydocumentdb
 ```

## <a name="management-library"></a><span data-ttu-id="6cdcc-109">管理ライブラリ</span><span class="sxs-lookup"><span data-stu-id="6cdcc-109">Management library</span></span>
```bash
pip install azure-mgmt-cosmosdb
```

### <a name="example"></a><span data-ttu-id="6cdcc-110">例</span><span class="sxs-lookup"><span data-stu-id="6cdcc-110">Example</span></span>

<span data-ttu-id="6cdcc-111">SQL に似たクエリ インターフェイスを使用して、Azure CosmosDB で一致するドキュメントを探します。</span><span class="sxs-lookup"><span data-stu-id="6cdcc-111">Find matching documents in Azure CosmosDB using a SQL-like query interface:</span></span>

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
> [<span data-ttu-id="6cdcc-112">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="6cdcc-112">Explore the Management APIs</span></span>](/python/api/overview/azure/cosmosdb/management)

## <a name="samples"></a><span data-ttu-id="6cdcc-113">サンプル</span><span class="sxs-lookup"><span data-stu-id="6cdcc-113">Samples</span></span>

* [<span data-ttu-id="6cdcc-114">Azure Cosmos DB SQL API アカウントに保存されたデータにアクセスして管理するための Python アプリを開発する</span><span class="sxs-lookup"><span data-stu-id="6cdcc-114">Develop a Python app to access and manage data stored in Azure Cosmos DB SQL API account</span></span>](https://github.com/Azure-Samples/azure-cosmos-db-python-getting-started.git)

* [<span data-ttu-id="6cdcc-115">Azure Cosmos DB MongoDB API アカウントに保存されたデータにアクセスして管理するための Python アプリを開発する</span><span class="sxs-lookup"><span data-stu-id="6cdcc-115">Develop a Python app to access and manage data stored in Azure Cosmos DB MongoDB API account</span></span>](https://github.com/Azure-Samples/CosmosDB-Flask-Mongo-Sample.git)

* [<span data-ttu-id="6cdcc-116">Azure Cosmos DB Gremlin API アカウントに保存されたデータにアクセスして管理するための Python アプリを開発する</span><span class="sxs-lookup"><span data-stu-id="6cdcc-116">Develop a Python app to access and manage data stored in Azure Cosmos DB Gremlin API account</span></span>](https://github.com/Azure-Samples/azure-cosmos-db-graph-python-getting-started.git)

* [<span data-ttu-id="6cdcc-117">Azure Cosmos DB Cassandra API アカウントに保存されたデータにアクセスして管理するための Python アプリを開発する</span><span class="sxs-lookup"><span data-stu-id="6cdcc-117">Develop a Python app to access and manage data stored in Azure Cosmos DB Cassandra API account</span></span>](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-python-getting-started.git)

* [<span data-ttu-id="6cdcc-118">Azure Cosmos DB Table API アカウントに保存されたデータにアクセスして管理するための Python アプリを開発する</span><span class="sxs-lookup"><span data-stu-id="6cdcc-118">Develop a Python app to access and manage data stored in Azure Cosmos DB Table API account</span></span>](https://github.com/Azure-Samples/storage-python-getting-started.git)


