---
title: Python 用 Azure サーバー マネージャー ライブラリ
description: Python 用 Azure サーバー マネージャー ライブラリのリファレンス
keywords: Azure, Python, SDK, API, サーバー マネージャー
author: lisawong19
ms.author: liwong
manager: routlaw
ms.date: 02/22/2018
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 480513a76683c97fdac8d2a65b38fde0fffb6dcd
ms.sourcegitcommit: 41e90fe75de03d397079a276cdb388305290e27e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/23/2018
ms.locfileid: "29479235"
---
# <a name="azure-server-manager-libraries-for-python"></a><span data-ttu-id="362ad-104">Python 用 Azure サーバー マネージャー ライブラリ</span><span class="sxs-lookup"><span data-stu-id="362ad-104">Azure Server Manager libraries for python</span></span>

## <a name="management-apipythonapioverviewazureservermanagermanagement"></a>[<span data-ttu-id="362ad-105">Management API</span><span class="sxs-lookup"><span data-stu-id="362ad-105">Management API</span></span>](/python/api/overview/azure/servermanager/management)

```bash
pip install azure-mgmt-servermanager
```

## <a name="create-the-management-client"></a><span data-ttu-id="362ad-106">管理クライアントを作成する</span><span class="sxs-lookup"><span data-stu-id="362ad-106">Create the management client</span></span>

<span data-ttu-id="362ad-107">管理クライアントのインスタンスは、以下のコードで作成します。</span><span class="sxs-lookup"><span data-stu-id="362ad-107">The following code creates an instance of the management client.</span></span>

<span data-ttu-id="362ad-108">[サブスクリプション一覧](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping)から取得できる自分の ``subscription_id`` を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="362ad-108">You will need to provide your ``subscription_id`` which can be retrieved from your [subscription list](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping).</span></span>

<span data-ttu-id="362ad-109">Python SDK を使用した Azure Active Directory の認証処理と ``Credentials`` インスタンスの作成について詳しくは、「[Resource Management Authentication (リソース管理の認証)](/python/azure/python-sdk-azure-authenticate)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="362ad-109">See [Resource Management Authentication](/python/azure/python-sdk-azure-authenticate) for details on handling Azure Active Directory authentication with the Python SDK, and creating a ``Credentials`` instance.</span></span>

```python
from azure.mgmt.servermanager import ServerManagement
from azure.common.credentials import UserPassCredentials

# Replace this with your subscription id
subscription_id = '33333333-3333-3333-3333-333333333333'

# See above for details on creating different types of AAD credentials
credentials = UserPassCredentials(
    'user@domain.com',  # Your user
    'my_password',      # Your password
)

servermanager_client = ServerManagement(
    credentials,
    subscription_id
)
``` 

## <a name="create-gateway"></a><span data-ttu-id="362ad-110">ゲートウェイの作成</span><span class="sxs-lookup"><span data-stu-id="362ad-110">Create gateway</span></span>
```python
gateway_async = servermanager_client.gateway.create(
    'MyResourceGroup',
    'MyGateway',
    'centralus'
)
gateway = gateway_async.result() # Blocking wait
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="362ad-111">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="362ad-111">Explore the Management APIs</span></span>](/python/api/overview/azure/servermanager/management)