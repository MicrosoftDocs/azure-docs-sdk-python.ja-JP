---
title: "Python 用 Azure Redis ライブラリ"
description: "Redis 用 Python クライアント ライブラリのリファレンス ドキュメント"
keywords: "Azure, Python, Redis, API, SDK, データベース, NoSQL"
author: sptramer
ms.author: sttramer
manager: douge
ms.date: 06/26/2017
ms.topic: article
ms.devlang: python
ms.service: redis
ms.openlocfilehash: c2f8ebcbcbd7b71c1fa96e46a8148a3c0b88877f
ms.sourcegitcommit: 41e90fe75de03d397079a276cdb388305290e27e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/23/2018
---
# <a name="azure-redis-cache-libraries-for-python"></a><span data-ttu-id="6bc25-104">Python 用 Azure Redis Cache ライブラリ</span><span class="sxs-lookup"><span data-stu-id="6bc25-104">Azure Redis Cache libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="6bc25-105">概要</span><span class="sxs-lookup"><span data-stu-id="6bc25-105">Overview</span></span>

<span data-ttu-id="6bc25-106">Azure Redis Cache は、広く普及しているオープン ソースの Redis プロジェクトを基盤にしています。</span><span class="sxs-lookup"><span data-stu-id="6bc25-106">Azure Redis Cache is based on the popular open source Redis project.</span></span> <span data-ttu-id="6bc25-107">これを使用すると、Microsoft によって管理されている、セキュリティで保護された専用 Redis インスタンスに、ご利用の Azure アプリでアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="6bc25-107">It gives you access to a secure, dedicated Redis instance, managed by Microsoft and accessible from your Azure apps.</span></span>

<span data-ttu-id="6bc25-108">Redis は高度なキーと値のストアです。キーには、文字列、ハッシュ、リスト、セット、ソート済みセットなどのデータ構造を格納できます。</span><span class="sxs-lookup"><span data-stu-id="6bc25-108">Redis is an advanced key-value store, where keys can contain data structures such as strings, hashes, lists, sets, and sorted sets.</span></span> <span data-ttu-id="6bc25-109">Redis では、このようなデータ型に対する一連のアトミック操作がサポートされています。</span><span class="sxs-lookup"><span data-stu-id="6bc25-109">Redis supports a set of atomic operations on these data types.</span></span>

<span data-ttu-id="6bc25-110">Azure Redis Cache の詳細については、[こちら](https://docs.microsoft.com/azure/redis-cache/)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="6bc25-110">Learn more about [Azure Redis Cache](https://docs.microsoft.com/azure/redis-cache/).</span></span>

## <a name="management-api"></a><span data-ttu-id="6bc25-111">管理 API</span><span class="sxs-lookup"><span data-stu-id="6bc25-111">Management API</span></span>

<span data-ttu-id="6bc25-112">Redis 管理 API を使用すると、ご利用のサブスクリプションの Redis リソースを作成したり管理したりすることができます。</span><span class="sxs-lookup"><span data-stu-id="6bc25-112">Create and manage your Redis resources in your subscription with the Redis management API.</span></span>

```bash
pip install redis
pip install azure-mgmt-redis
```

### <a name="example"></a><span data-ttu-id="6bc25-113">例</span><span class="sxs-lookup"><span data-stu-id="6bc25-113">Example</span></span>

<span data-ttu-id="6bc25-114">次の例では、新しい Redis キャッシュを作成します。</span><span class="sxs-lookup"><span data-stu-id="6bc25-114">The following example creates a new Redis cache:</span></span>

```python
from azure.mgmt.redis import RedisManagementClient
from azure.mgmt.redis.models import Sku, RedisCreateOrUpdateParameters

redis_client = RedisManagementClient(
    credentials,
    subscription_id
)
group_name = 'myresourcegroup'
cache_name = 'mycachename'
redis_cache = redis_client.redis.create_or_update(
    group_name,
    cache_name,
    RedisCreateOrUpdateParameters(
        sku = Sku(name = 'Basic', family = 'C', capacity = '1'),
        location = "East US"
    )
)
# redis_cache is a RedisResourceWithAccessKey instance
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="6bc25-115">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="6bc25-115">Explore the Management APIs</span></span>](/python/api/overview/azure/redis/management)

