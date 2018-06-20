---
title: Python 用 Azure Scheduler ライブラリ
description: Python 用 Azure Scheduler ライブラリのリファレンス
keywords: Azure, Python, SDK, API, Scheduler
author: lisawong19
ms.author: liwong
manager: mbaldwin
ms.date: 02/21/2018
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 3d2691ae1ba84c41f25de2b099aacefaa92152ed
ms.sourcegitcommit: d7c26ac167cf6a6491358ac3153f268bc90e55e9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/24/2018
ms.locfileid: "29551615"
---
# <a name="azure-scheduler-libraries-for-python"></a><span data-ttu-id="75419-104">Python 用 Azure Scheduler ライブラリ</span><span class="sxs-lookup"><span data-stu-id="75419-104">Azure Scheduler libraries for python</span></span>

## <a name="install-the-libraries"></a><span data-ttu-id="75419-105">ライブラリをインストールする</span><span class="sxs-lookup"><span data-stu-id="75419-105">Install the libraries</span></span>

## <a name="management"></a><span data-ttu-id="75419-106">管理</span><span class="sxs-lookup"><span data-stu-id="75419-106">Management</span></span>

```bash
pip install azure-mgmt-scheduler
```
## <a name="example"></a><span data-ttu-id="75419-107">例</span><span class="sxs-lookup"><span data-stu-id="75419-107">Example</span></span>

### <a name="create-the-management-client"></a><span data-ttu-id="75419-108">管理クライアントを作成する</span><span class="sxs-lookup"><span data-stu-id="75419-108">Create the management client</span></span>

<span data-ttu-id="75419-109">管理クライアントのインスタンスは、以下のコードで作成します。</span><span class="sxs-lookup"><span data-stu-id="75419-109">The following code creates an instance of the management client.</span></span>

<span data-ttu-id="75419-110">[サブスクリプション一覧](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping)から取得できる自分の ``subscription_id`` を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="75419-110">You will need to provide your ``subscription_id`` which can be retrieved from [your subscription list](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping).</span></span>

<span data-ttu-id="75419-111">Python SDK を使用した Azure Active Directory の認証処理と ``Credentials`` インスタンスの作成について詳しくは、「[Resource Management Authentication (リソース管理の認証)](/python/azure/python-sdk-azure-authenticate)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="75419-111">See [Resource Management Authentication](/python/azure/python-sdk-azure-authenticate) for details on handling Azure Active Directory authentication with the Python SDK, and creating a ``Credentials`` instance.</span></span>

```python
from azure.mgmt.scheduler import SchedulerManagementClient
from azure.common.credentials import UserPassCredentials

# Replace this with your subscription id
subscription_id = '33333333-3333-3333-3333-333333333333'

# See above for details on creating different types of AAD credentials
credentials = UserPassCredentials(
    'user@domain.com',  # Your user
    'my_password',      # Your password
)

scheduler_client = SchedulerManagementClient(
    credentials,
    subscription_id
)
```

### <a name="create-a-job-collection"></a><span data-ttu-id="75419-112">ジョブ コレクションの作成</span><span class="sxs-lookup"><span data-stu-id="75419-112">Create a job collection</span></span>

<span data-ttu-id="75419-113">以下のコードは、既存のリソース グループにジョブ コレクションを作成するものです。</span><span class="sxs-lookup"><span data-stu-id="75419-113">The following code creates a job collection under an existing resource group.</span></span>
<span data-ttu-id="75419-114">リソース グループの作成と管理については、[リソース管理](/python/api/overview/azure/azure.mgmt.resource)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="75419-114">To create or manage resource groups, see [Resource Management](/python/api/overview/azure/azure.mgmt.resource).</span></span>

```python
from azure.mgmt.scheduler.models import JobCollectionDefinition, JobCollectionProperties, Sku

group_name = 'myresourcegroup'
job_collection_name = "myjobcollection"
scheduler_client.job_collections.create_or_update(
    group_name,
    job_collection_name,
    JobCollectionDefinition(
        location = "West US",
        properties = JobCollectionProperties(
            sku = Sku(
                name="Free"
            )
        )
    )
)
# scheduler_client is a JobCollectionDefinition instance
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="75419-115">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="75419-115">Explore the Management APIs</span></span>](/python/api/overview/azure/scheduler/management)