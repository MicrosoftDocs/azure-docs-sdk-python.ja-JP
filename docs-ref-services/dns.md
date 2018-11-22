---
title: Python 用 Azure DNS ライブラリ
description: Python 用 Azure DNS ライブラリのリファレンス
keywords: Azure, Python, SDK, API, DNS
author: sptramer
ms.author: sttramer
manager: douge
ms.date: 07/10/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: c70c3d44911b28b0a8c92b5f7efeca3bf1611f43
ms.sourcegitcommit: f439ba940d5940359c982015db7ccfb82f9dffd9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/21/2018
ms.locfileid: "52277254"
---
# <a name="azure-dns-libraries-for-python"></a><span data-ttu-id="1ea80-104">Python 用 Azure DNS ライブラリ</span><span class="sxs-lookup"><span data-stu-id="1ea80-104">Azure DNS libraries for python</span></span>

## <a name="overview"></a><span data-ttu-id="1ea80-105">概要</span><span class="sxs-lookup"><span data-stu-id="1ea80-105">Overview</span></span>

<span data-ttu-id="1ea80-106">[Azure DNS](/azure/dns/dns-overview) は、DNS ドメインのホスティング サービスであり、Azure インフラストラクチャを使用して DNS 解決を実行します。</span><span class="sxs-lookup"><span data-stu-id="1ea80-106">[Azure DNS](/azure/dns/dns-overview) is a hosting service for DNS domains that provides DNS resolution via the Azure infrastructure.</span></span>

<span data-ttu-id="1ea80-107">Azure DNS の概要については、「[Azure Portal で Azure DNS の使用を開始する](/azure/dns/dns-getstarted-portal)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="1ea80-107">To get started with Azure DNS, see [Get started with Azure DNS using the Azure portal](/azure/dns/dns-getstarted-portal).</span></span>

## <a name="management-apipythonapioverviewazurednsmanagement"></a>[<span data-ttu-id="1ea80-108">Management API</span><span class="sxs-lookup"><span data-stu-id="1ea80-108">Management API</span></span>](/python/api/overview/azure/dns/management)

```bash
pip install azure-mgmt-dns
```

## <a name="create-the-management-client"></a><span data-ttu-id="1ea80-109">管理クライアントを作成する</span><span class="sxs-lookup"><span data-stu-id="1ea80-109">Create the management client</span></span>

<span data-ttu-id="1ea80-110">管理クライアントのインスタンスは、以下のコードで作成します。</span><span class="sxs-lookup"><span data-stu-id="1ea80-110">The following code creates an instance of the management client.</span></span>

<span data-ttu-id="1ea80-111">[サブスクリプション一覧](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping)から取得できる自分の ``subscription_id`` を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1ea80-111">You will need to provide your ``subscription_id`` which can be retrieved from [your subscription list](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping).</span></span>

<span data-ttu-id="1ea80-112">Python SDK を使用した Azure Active Directory の認証処理と ``Credentials`` インスタンスの作成について詳しくは、「[Resource Management Authentication (リソース管理の認証)](/python/azure/python-sdk-azure-authenticate)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="1ea80-112">See [Resource Management Authentication](/python/azure/python-sdk-azure-authenticate) for details on handling Azure Active Directory authentication with the Python SDK, and creating a ``Credentials`` instance.</span></span>

```python 
from azure.mgmt.dns import DnsManagementClient
from azure.common.credentials import UserPassCredentials

# Replace this with your subscription id
subscription_id = '33333333-3333-3333-3333-333333333333'

# See above for details on creating different types of AAD credentials
credentials = UserPassCredentials(
    'user@domain.com',  # Your user
    'my_password',      # Your password
)

dns_client = DnsManagementClient(
    credentials,
    subscription_id
)
```

## <a name="create-dns-zone"></a><span data-ttu-id="1ea80-113">DNS ゾーンの作成</span><span class="sxs-lookup"><span data-stu-id="1ea80-113">Create DNS zone</span></span>
```python
# The only valid value is 'global', otherwise you will get a:
# The subscription is not registered for the resource type 'dnszones' in the location 'westus'.
zone = dns_client.zones.create_or_update(
    'MyResourceGroup',
    'pydns.com',
    {
            'zone_type': 'Public', # or Private
        'location': 'global'
    }
)
```
    
## <a name="create-a-record-set"></a><span data-ttu-id="1ea80-114">レコード セットの作成</span><span class="sxs-lookup"><span data-stu-id="1ea80-114">Create a Record Set</span></span>
```python
record_set = dns_client.record_sets.create_or_update(
    'MyResourceGroup',
    'pydns.com',
    'MyRecordSet',
    'A',
    {
            "ttl": 300,
            "arecords": [
                {
                "ipv4_address": "1.2.3.4"
                },
                {
                "ipv4_address": "1.2.3.5"
                }
            ]
    }
)
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="1ea80-115">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="1ea80-115">Explore the Management APIs</span></span>](/python/api/overview/azure/dns/management)
