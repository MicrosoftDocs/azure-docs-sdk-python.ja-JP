---
title: Python 用 Azure Scheduler ライブラリ
description: Python 用 Azure Scheduler ライブラリのリファレンス
keywords: Azure, Python, SDK, API, Scheduler
author: lisawong19
ms.author: routlaw
manager: mbaldwin
ms.date: 02/21/2018
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 73c33ff808212fade192ca7c7c05a8e64d3fafda
ms.sourcegitcommit: 46bebbf5dd558750043ce5afadff2ec3714a54e6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2019
ms.locfileid: "67534212"
---
# <a name="azure-scheduler-libraries-for-python"></a>Python 用 Azure Scheduler ライブラリ

## <a name="install-the-libraries"></a>ライブラリをインストールする

## <a name="management"></a>管理

```bash
pip install azure-mgmt-scheduler
```
## <a name="example"></a>例

### <a name="create-the-management-client"></a>管理クライアントを作成する

管理クライアントのインスタンスは、以下のコードで作成します。

[サブスクリプション一覧](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping)から取得できる自分の ``subscription_id`` を指定する必要があります。

Python SDK を使用した Azure Active Directory の認証処理と ``Credentials`` インスタンスの作成について詳しくは、「[Resource Management Authentication (リソース管理の認証)](/python/azure/python-sdk-azure-authenticate)」を参照してください。

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

### <a name="create-a-job-collection"></a>ジョブ コレクションの作成

以下のコードは、既存のリソース グループにジョブ コレクションを作成するものです。
リソース グループの作成と管理については、[リソース管理](/python/api/overview/azure/azure.mgmt.resource)に関するページを参照してください。

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
> [Management API を探す](/python/api/overview/azure/scheduler/management)