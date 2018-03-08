---
title: "Python 用 Azure Notification Hubs ライブラリ"
description: "Python 用 Azure Notification Hubs ライブラリのリファレンス"
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
ms.openlocfilehash: 66b452a40fd524672f4dad92a9d1bd0ffb77a99d
ms.sourcegitcommit: d7c26ac167cf6a6491358ac3153f268bc90e55e9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/24/2018
---
# <a name="azure-notification-hubs-libraries-for-python"></a><span data-ttu-id="ad055-104">Python 用 Azure Notification Hubs ライブラリ</span><span class="sxs-lookup"><span data-stu-id="ad055-104">Azure Notification Hubs libraries for python</span></span>

## <a name="management-apipythonapioverviewazurenotificationhubsmanagement"></a>[<span data-ttu-id="ad055-105">Management API</span><span class="sxs-lookup"><span data-stu-id="ad055-105">Management API</span></span>](/python/api/overview/azure/notificationhubs/management)

```bash
pip install azure-mgmt-notificationhubs
```

## <a name="create-the-management-client"></a><span data-ttu-id="ad055-106">管理クライアントを作成する</span><span class="sxs-lookup"><span data-stu-id="ad055-106">Create the management client</span></span>

<span data-ttu-id="ad055-107">管理クライアントのインスタンスは、以下のコードで作成します。</span><span class="sxs-lookup"><span data-stu-id="ad055-107">The following code creates an instance of the management client.</span></span>

<span data-ttu-id="ad055-108">[サブスクリプション一覧](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping)から取得できる自分の ``subscription_id`` を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ad055-108">You will need to provide your ``subscription_id`` which can be retrieved from [your subscription list](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping).</span></span>

<span data-ttu-id="ad055-109">Python SDK を使用した Azure Active Directory の認証処理と ``Credentials`` インスタンスの作成について詳しくは、「[Resource Management Authentication (リソース管理の認証)](/python/azure/python-sdk-azure-authenticate)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="ad055-109">See [Resource Management Authentication](/python/azure/python-sdk-azure-authenticate) for details on handling Azure Active Directory authentication with the Python SDK, and creating a ``Credentials`` instance.</span></span>

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

## <a name="check-namespace-availability"></a><span data-ttu-id="ad055-110">名前空間を使用できるかどうかの確認</span><span class="sxs-lookup"><span data-stu-id="ad055-110">Check namespace availability</span></span>

<span data-ttu-id="ad055-111">通知ハブの名前空間を使用できるかどうかは、次のコードで確認します。</span><span class="sxs-lookup"><span data-stu-id="ad055-111">The following code check namespace availability of a notification hub.</span></span>
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
> [<span data-ttu-id="ad055-112">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="ad055-112">Explore the Management APIs</span></span>](/python/api/overview/azure/notificationhubs/management)
