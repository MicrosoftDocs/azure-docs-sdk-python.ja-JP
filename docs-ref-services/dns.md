---
title: "Python 用 Azure DNS ライブラリ"
description: "Python 用 Azure DNS ライブラリのリファレンス"
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
ms.openlocfilehash: 59ae3628b4a810db8fee21aacf46c13054dc8cd3
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/18/2017
---
# <a name="azure-dns-libraries-for-python"></a><span data-ttu-id="72ab5-104">Python 用 Azure DNS ライブラリ</span><span class="sxs-lookup"><span data-stu-id="72ab5-104">Azure DNS libraries for python</span></span>

## <a name="overview"></a><span data-ttu-id="72ab5-105">概要</span><span class="sxs-lookup"><span data-stu-id="72ab5-105">Overview</span></span>

<span data-ttu-id="72ab5-106">[Azure DNS](/azure/dns/dns-overview) は、DNS ドメインのホスティング サービスであり、Azure インフラストラクチャを使用して DNS 解決を実行します。</span><span class="sxs-lookup"><span data-stu-id="72ab5-106">[Azure DNS](/azure/dns/dns-overview) is a hosting service for DNS domains that provides DNS resolution via the Azure infrastructure.</span></span>

<span data-ttu-id="72ab5-107">Azure DNS の概要については、「[Azure Portal で Azure DNS の使用を開始する](/azure/dns/dns-getstarted-portal)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="72ab5-107">To get started with Azure DNS, see [Get started with Azure DNS using the Azure portal](/azure/dns/dns-getstarted-portal).</span></span>

## <a name="management-apis"></a><span data-ttu-id="72ab5-108">管理 API</span><span class="sxs-lookup"><span data-stu-id="72ab5-108">Management APIs</span></span>

<span data-ttu-id="72ab5-109">Management API を使用して、DNS ゾーンと DNS レコードを作成、管理します。</span><span class="sxs-lookup"><span data-stu-id="72ab5-109">Create and manage DNS zones and records with the management API.</span></span>

<span data-ttu-id="72ab5-110">pip を使用して管理パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="72ab5-110">Install the management package with pip.</span></span>

```bash
pip install azure-mgmt-dns
```

### <a name="example"></a><span data-ttu-id="72ab5-111">例</span><span class="sxs-lookup"><span data-stu-id="72ab5-111">Example</span></span>

<span data-ttu-id="72ab5-112">新しい DNS ゾーンを作成します。</span><span class="sxs-lookup"><span data-stu-id="72ab5-112">Create a new DNS zone.</span></span>

```python
from azure.mgmt.dns import DnsManagementClient

dns_client = DnsManagementClient(credentials, 'your-subscription-id')
zone = dns_client.zones.create_or_update('resource-group',
                                         'zone_name_no_dot',
                                         {
                                            "location": "global"
                                         })

```

> [!div class="nextstepaction"]
> [<span data-ttu-id="72ab5-113">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="72ab5-113">Explore the Management APIs</span></span>](/python/api/overview/azure/dns/managementlibrary)
