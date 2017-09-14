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
# <a name="azure-dns-libraries-for-python"></a>Python 用 Azure DNS ライブラリ

## <a name="overview"></a>概要

[Azure DNS](/azure/dns/dns-overview) は、DNS ドメインのホスティング サービスであり、Azure インフラストラクチャを使用して DNS 解決を実行します。

Azure DNS の概要については、「[Azure Portal で Azure DNS の使用を開始する](/azure/dns/dns-getstarted-portal)」を参照してください。

## <a name="management-apis"></a>管理 API

Management API を使用して、DNS ゾーンと DNS レコードを作成、管理します。

pip を使用して管理パッケージをインストールします。

```bash
pip install azure-mgmt-dns
```

### <a name="example"></a>例

新しい DNS ゾーンを作成します。

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
> [Management API を探す](/python/api/overview/azure/dns/managementlibrary)
