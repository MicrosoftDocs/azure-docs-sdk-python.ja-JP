---
title: Python 用 Azure コマース ライブラリ
description: Python 用 Azure コマース ライブラリのリファレンス
keywords: Azure, Python, SDK, API, コマース
author: lisawong19
ms.author: liwong
manager: routlaw
ms.date: 02/21/2018
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 77008eb76c3a925d9c7e63fe9360ea5b25da49de
ms.sourcegitcommit: d7c26ac167cf6a6491358ac3153f268bc90e55e9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/24/2018
ms.locfileid: "29551605"
---
# <a name="azure-commerce-libraries-for-python"></a><span data-ttu-id="664af-104">Python 用 Azure コマース ライブラリ</span><span class="sxs-lookup"><span data-stu-id="664af-104">Azure Commerce libraries for python</span></span>

## <a name="management-apipythonapioverviewazurecommercemanagement"></a>[<span data-ttu-id="664af-105">Management API</span><span class="sxs-lookup"><span data-stu-id="664af-105">Management API</span></span>](/python/api/overview/azure/commerce/management)

```bash
pip install azure-mgmt-commerce
```
## <a name="create-the-commerce-client"></a><span data-ttu-id="664af-106">コマース クライアントの作成</span><span class="sxs-lookup"><span data-stu-id="664af-106">Create the commerce client</span></span>

<span data-ttu-id="664af-107">管理クライアントのインスタンスは、以下のコードで作成します。</span><span class="sxs-lookup"><span data-stu-id="664af-107">The following code creates an instance of the management client.</span></span>

<span data-ttu-id="664af-108">[サブスクリプション一覧](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping)から取得できる自分の ``subscription_id`` を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="664af-108">You will need to provide your ``subscription_id`` which can be retrieved from [your subscription list](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping).</span></span>

<span data-ttu-id="664af-109">Python SDK を使用した Azure Active Directory の認証処理と ``Credentials`` インスタンスの作成について詳しくは、「[Resource Management Authentication (リソース管理の認証)](/python/azure/python-sdk-azure-authenticate)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="664af-109">See [Resource Management Authentication](/python/azure/python-sdk-azure-authenticate) for details on handling Azure Active Directory authentication with the Python SDK, and creating a ``Credentials`` instance.</span></span>

```python
from azure.mgmt.commerce import UsageManagementClient
from azure.common.credentials import UserPassCredentials

# Replace this with your subscription id
subscription_id = '33333333-3333-3333-3333-333333333333'

# See above for details on creating different types of AAD credentials
credentials = UserPassCredentials(
    'user@domain.com',  # Your user
    'my_password',      # Your password
)

commerce_client = UsageManagementClient(
    credentials,
    subscription_id
)
``` 

## <a name="get-rate-card"></a><span data-ttu-id="664af-110">価格カードの取得</span><span class="sxs-lookup"><span data-stu-id="664af-110">Get rate card</span></span>

```python
# OfferDurableID: https://azure.microsoft.com/en-us/support/legal/offer-details/
rate = commerce_client.rate_card.get(
    "OfferDurableId eq 'MS-AZR-0062P' and Currency eq 'USD' and Locale eq 'en-US' and RegionInfo eq 'US'"
)
```

## <a name="get-usage"></a><span data-ttu-id="664af-111">使用状況の取得</span><span class="sxs-lookup"><span data-stu-id="664af-111">Get Usage</span></span>

```python
from datetime import date, timedelta

# Takes onky dates in full ISO8601 with 'T00:00:00Z'
# Return an iterator like object: https://docs.python.org/3/library/stdtypes.html#iterator-types
usage_iterator = commerce_client.usage_aggregates.list(
    str(date.today() - timedelta(days=1))+'T00:00:00Z',
    str(date.today())+'T00:00:00Z'
)
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="664af-112">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="664af-112">Explore the Management APIs</span></span>](/python/api/overview/azure/commerce/management)