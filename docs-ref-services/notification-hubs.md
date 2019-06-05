---
title: Python 用 Azure Notification Hubs ライブラリ
description: Python 用 Azure Notification Hubs ライブラリのリファレンス
keywords: Azure, Python, SDK, API, Notification Hubs
author: lisawong19
ms.author: liwong
manager: routlaw
ms.date: 02/22/2018
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: ea5ef3722635484d23459d1d39ec6216d4574b85
ms.sourcegitcommit: 434186988284e0a8268a9de11645912a81226d6b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/29/2019
ms.locfileid: "66376903"
---
# <a name="azure-notification-hubs-libraries-for-python"></a><span data-ttu-id="dfa10-104">Python 用 Azure Notification Hubs ライブラリ</span><span class="sxs-lookup"><span data-stu-id="dfa10-104">Azure Notification Hubs libraries for python</span></span>

## <a name="management-apipythonapioverviewazurenotificationhubsmanagement"></a>[<span data-ttu-id="dfa10-105">Management API</span><span class="sxs-lookup"><span data-stu-id="dfa10-105">Management API</span></span>](/python/api/overview/azure/notificationhubs/management)

```bash
pip install azure-mgmt-notificationhubs
```

## <a name="create-the-management-client"></a><span data-ttu-id="dfa10-106">管理クライアントを作成する</span><span class="sxs-lookup"><span data-stu-id="dfa10-106">Create the management client</span></span>

<span data-ttu-id="dfa10-107">管理クライアントのインスタンスは、以下のコードで作成します。</span><span class="sxs-lookup"><span data-stu-id="dfa10-107">The following code creates an instance of the management client.</span></span>

<span data-ttu-id="dfa10-108">[サブスクリプション一覧](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping)から取得できる自分の ``subscription_id`` を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="dfa10-108">You will need to provide your ``subscription_id`` which can be retrieved from [your subscription list](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping).</span></span>

<span data-ttu-id="dfa10-109">Python SDK を使用した Azure Active Directory の認証処理と ``Credentials`` インスタンスの作成について詳しくは、「[Resource Management Authentication (リソース管理の認証)](/python/azure/python-sdk-azure-authenticate)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="dfa10-109">See [Resource Management Authentication](/python/azure/python-sdk-azure-authenticate) for details on handling Azure Active Directory authentication with the Python SDK, and creating a ``Credentials`` instance.</span></span>

```python
from azure.mgmt.notificationhubs import NotificationHubsManagementClient
from azure.common.credentials import UserPassCredentials

# Replace this with your subscription id
subscription_id = '33333333-3333-3333-3333-333333333333'

# See above for details on creating different types of AAD credentials
credentials = UserPassCredentials(
    'user@domain.com',  # Your user
    'my_password',      # Your password
)

redis_client = NotificationHubsManagementClient(
    credentials,
    subscription_id
)
```

## <a name="check-namespace-availability"></a><span data-ttu-id="dfa10-110">名前空間を使用できるかどうかの確認</span><span class="sxs-lookup"><span data-stu-id="dfa10-110">Check namespace availability</span></span>

<span data-ttu-id="dfa10-111">通知ハブの名前空間を使用できるかどうかは、次のコードで確認します。</span><span class="sxs-lookup"><span data-stu-id="dfa10-111">The following code check namespace availability of a notification hub.</span></span>

```python
from azure.mgmt.notificationhubs.models import CheckAvailabilityParameters

account_name = 'mynotificationhub'
output = notificationhubs_client.namespaces.check_availability(
    azure.mgmt.notificationhubs.models.CheckAvailabilityParameters(
        name = account_name
    )
)
# output is a CheckAvailibilityResource instance
print(output.is_availiable) # Yes, it's 'availiable', it's a typo in the REST API
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="dfa10-112">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="dfa10-112">Explore the Management APIs</span></span>](/python/api/overview/azure/notificationhubs/management)
