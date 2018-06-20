---
title: Python 用 Azure リソース ライブラリ
description: ''
keywords: Azure, Python, SDK, API, リソース
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/19/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: resources
ms.openlocfilehash: 32e13bee27db091f0bca12c7d9ae4fc62165f4c0
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/18/2017
ms.locfileid: "20909395"
---
# <a name="azure-resources-libraries-for-python"></a>Python 用 Azure リソース ライブラリ 

## <a name="overview"></a>概要 
リソース グループ内の Azure リソースを管理します。

| パッケージ  |  Description |
|---|---|
|[azure.mgmt.resource.features][1]|Azure Feature Exposure Control (AFEC) は、リソース プロバイダーがユーザーへの機能の公開を制御するためのメカニズムを提供します。|
|[azure.mgmt.resource.links][2]|Azure リソースは、論理的な関係を形成するために互いをリンクすることができます。 異なるリソース グループに属しているリソースの間にリンクを確立することができます。|
|[azure.mgmt.resource.locks][3]|Azure リソースは、組織内の他のユーザーによるリソースの削除または変更を防ぐために、ロックすることができます。|
|[azure.mgmt.resource.managedapplications][4]|ARM マネージ アプリケーション (アプライアンス)。|
|[azure.mgmt.resource.policy][5]|リソースへのアクセスを管理および制御するには、カスタマイズしたポリシーを定義して一定のスコープで割り当てます。|
|[azure.mgmt.resource.resources][6]| リソースとリソース グループを扱うための操作を提供します。|
|[azure.mgmt.resource.subscriptions][7]|すべてのリソース グループとリソースは、サブスクリプション内に存在します。 これらの操作では、サブスクリプションとテナントに関する情報を取得することができます。|

[1]: /python/api/azure.mgmt.resource.features
[2]: /python/api/azure.mgmt.resource.links
[3]: /python/api/azure.mgmt.resource.locks
[4]: /python/api/azure.mgmt.resource.managedapplications
[5]: /python/api/azure.mgmt.resource.policy
[6]: /python/api/azure.mgmt.resource.resources
[7]: /python/api/azure.mgmt.resource.subscriptions

## <a name="install-the-libraries"></a>ライブラリをインストールする 
```bash
pip install azure-mgmt-resource
```

## <a name="example"></a>例
次の例は、リソース グループの作成方法を示しています。 

```python
from azure.mgmt.resource import ResourceManagementClient
client = ResourceManagementClient(credentials, subscription_id)
client.resource_groups.create(RESOURCE_GROUP_NAME, {'location':'eastus'})
```

アプリで使用できるその他の[サンプル Python コード](https://azure.microsoft.com/resources/samples/?platform=python)も参照してください。 