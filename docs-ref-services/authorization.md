---
title: Python 用 Azure 承認ライブラリ
description: Python 用 Azure 承認ライブラリのリファレンス
keywords: Azure, Python, SDK, API, 承認
author: lisawong19
ms.author: liwong
manager: routlaw
ms.date: 02/21/2018
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 8709bbd3cff448c7beb394621b163a4b3e3c3cd8
ms.sourcegitcommit: f439ba940d5940359c982015db7ccfb82f9dffd9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/21/2018
ms.locfileid: "52276766"
---
# <a name="azure-authorization-libraries-for-python"></a><span data-ttu-id="0c08e-104">Python 用 Azure 承認ライブラリ</span><span class="sxs-lookup"><span data-stu-id="0c08e-104">Azure Authorization libraries for python</span></span>

## <a name="management-apipythonapioverviewazureauthorizationmanagement"></a>[<span data-ttu-id="0c08e-105">Management API</span><span class="sxs-lookup"><span data-stu-id="0c08e-105">Management API</span></span>](/python/api/overview/azure/authorization/management)

```bash
pip install azure-mgmt-authorization
```

## <a name="create-the-management-client"></a><span data-ttu-id="0c08e-106">管理クライアントを作成する</span><span class="sxs-lookup"><span data-stu-id="0c08e-106">Create the management client</span></span>

<span data-ttu-id="0c08e-107">管理クライアントのインスタンスは、以下のコードで作成します。</span><span class="sxs-lookup"><span data-stu-id="0c08e-107">The following code creates an instance of the management client.</span></span>

<span data-ttu-id="0c08e-108">[サブスクリプション一覧](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping)から取得できる自分の ``subscription_id`` を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0c08e-108">You will need to provide your ``subscription_id`` which can be retrieved from [your subscription list](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping).</span></span>

<span data-ttu-id="0c08e-109">Python SDK を使用した Azure Active Directory の認証処理と ``Credentials`` インスタンスの作成について詳しくは、「[Resource Management Authentication (リソース管理の認証)](/python/azure/python-sdk-azure-authenticate)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="0c08e-109">See [Resource Management Authentication](/python/azure/python-sdk-azure-authenticate) for details on handling Azure Active Directory authentication with the Python SDK, and creating a ``Credentials`` instance.</span></span>

```python
from azure.mgmt.authorization import AuthorizationManagementClient
from azure.common.credentials import UserPassCredentials

# Replace this with your subscription id
subscription_id = '33333333-3333-3333-3333-333333333333'

# See above for details on creating different types of AAD credentials
credentials = UserPassCredentials(
    'user@domain.com',  # Your user
    'my_password'       # Your password
)

authorization_client = AuthorizationManagementClient(
    credentials,
    subscription_id
)
``` 

## <a name="check-permissions-for-a-resource-group"></a><span data-ttu-id="0c08e-110">リソース グループのアクセス許可チェック</span><span class="sxs-lookup"><span data-stu-id="0c08e-110">Check permissions for a resource group</span></span>

<span data-ttu-id="0c08e-111">以下のコードは、特定のリソース グループのアクセス許可をチェックするものです。</span><span class="sxs-lookup"><span data-stu-id="0c08e-111">The following code checks permissions in a given resource group.</span></span>
<span data-ttu-id="0c08e-112">リソース グループの作成と管理については、[リソース管理](/python/api/overview/azure/azure.mgmt.resource)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="0c08e-112">To create or manage resource groups, see [Resource Management](/python/api/overview/azure/azure.mgmt.resource).</span></span>

```python
from azure.mgmt.redis.models import Sku, RedisCreateOrUpdateParameters

group_name = 'myresourcegroup'
permissions = self.authorization_client.permissions.list_for_resource_group(
    group_name
)
# permissions is a iterable of Permissions instances
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="0c08e-113">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="0c08e-113">Explore the Management APIs</span></span>](/python/api/overview/azure/authorization/management)

