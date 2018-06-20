---
title: Python 用 Azure Redis ライブラリ
description: Redis 用 Python クライアント ライブラリのリファレンス ドキュメント
keywords: Azure, Python, Redis, API, SDK, データベース, NoSQL
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
ms.locfileid: "29479245"
---
# <a name="azure-redis-cache-libraries-for-python"></a>Python 用 Azure Redis Cache ライブラリ

## <a name="overview"></a>概要

Azure Redis Cache は、広く普及しているオープン ソースの Redis プロジェクトを基盤にしています。 これを使用すると、Microsoft によって管理されている、セキュリティで保護された専用 Redis インスタンスに、ご利用の Azure アプリでアクセスできます。

Redis は高度なキーと値のストアです。キーには、文字列、ハッシュ、リスト、セット、ソート済みセットなどのデータ構造を格納できます。 Redis では、このようなデータ型に対する一連のアトミック操作がサポートされています。

Azure Redis Cache の詳細については、[こちら](https://docs.microsoft.com/azure/redis-cache/)を参照してください。

## <a name="management-api"></a>管理 API

Redis 管理 API を使用すると、ご利用のサブスクリプションの Redis リソースを作成したり管理したりすることができます。

```bash
pip install redis
pip install azure-mgmt-redis
```

### <a name="example"></a>例

次の例では、新しい Redis キャッシュを作成します。

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
> [Management API を探す](/python/api/overview/azure/redis/management)

