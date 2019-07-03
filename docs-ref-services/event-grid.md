---
title: Python 用 Azure Event Grid ライブラリ
description: ''
keywords: Azure, Python, SDK, API, Event Grid
author: lisawong19
ms.author: routlaw
manager: routlaw
ms.date: 08/21/2017
ms.topic: article
ms.devlang: python
ms.service: event-grid
ms.openlocfilehash: e5df1078116f13f959923eac3e0c7b5789545278
ms.sourcegitcommit: 46bebbf5dd558750043ce5afadff2ec3714a54e6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2019
ms.locfileid: "67534298"
---
# <a name="event-grid-libraries-for-python"></a>Python 用 Event Grid ライブラリ


Azure Event Grid は、パブリッシュ/サブスクライブ モデルを使用した画一的なイベントの使用を可能にする、完全に管理されたインテリジェントなイベント ルーティング サービスです。

Azure Event Grid の[詳細を確認](/azure/event-grid/overview)し、[Azure Blob Storage イベントのチュートリアル](/azure/storage/blobs/storage-blob-event-quickstart)を開始してください。 

## <a name="publish-sdk"></a>発行 SDK

Azure Event Grid 発行 SDK を使用して、イベントの認証、作成、処理、トピックへの発行を行います。

### <a name="installation"></a>インストール 

[pip](https://pip.pypa.io/en/stable/quickstart/) を使用してパッケージをインストールします。

```bash
pip install azure-eventgrid
```

### <a name="example"></a>例 

次のコードでは、イベントをトピックに発行します。 トピック キーとエンドポイントは、Azure Portal または Azure CLI を使用して取得できます。

```azurecli-interactive
endpoint=$(az eventgrid topic show --name <topic_name> -g gridResourceGroup --query "endpoint" --output tsv)
key=$(az eventgrid topic key list --name <topic_name> -g gridResourceGroup --query "key1" --output tsv)
```

```python
from datetime import datetime
from azure.eventgrid import EventGridClient
from msrest.authentication import TopicCredentials

def publish_event(self):

        credentials = TopicCredentials(
            self.settings.EVENT_GRID_KEY
        )
        event_grid_client = EventGridClient(credentials)
        event_grid_client.publish_events(
            "your-endpoint-here",
            events=[{
                'id' : "dbf93d79-3859-4cac-8055-51e3b6b54bea",
                'subject' : "Sample subject",
                'data': {
                    'key': 'Sample Data'
                },
                'event_type': 'SampleEventType',
                'event_time': datetime(2018, 5, 2),
                'data_version': 1
            }]
        )
```

> [!div class="nextstepaction"]
> [クライアント API を探す](/python/api/overview/azure/eventgrid/client)

## <a name="management-sdk"></a>管理 SDK

管理 SDK を使用して、Event Grid のインスタンス、トピック、サブスクリプションを作成、更新、削除します。

### <a name="installation"></a>インストール 

[pip](https://pip.pypa.io/en/stable/quickstart/) を使用してパッケージをインストールします。

```bash
pip install azure-mgmt-eventgrid
```

### <a name="example"></a>例

次のコードでは、カスタム トピックを作成し、エンドポイントをそのトピックにサブスクライブします。 その後、HTTPS 経由でイベントをトピックに送信します。
RequestBin は、エンドポイントを作成してそこに送信された要求を確認することができるオープン ソースのサードパーティ ツールです。 [RequestBin](https://requestbin.com) にアクセスし、 **[Create a RequestBin]\(RequestBin の作成\)** をクリックします。 Bin URL をコピーしてください。トピックをサブスクライブするときにこの URL が必要になります。

```python
from azure.mgmt.resource import ResourceManagementClient
from azure.mgmt.eventgrid import EventGridManagementClient
import requests

RESOURCE_GROUP_NAME = 'gridResourceGroup'
TOPIC_NAME = 'gridTopic1234'
LOCATION = 'westus2'
SUBSCRIPTION_ID = 'YOUR_SUBSCRIPTION_ID'
SUBSCRIPTION_NAME = 'gridSubscription'
REQUEST_BIN_URL = 'YOUR_REQUEST_BIN_URL'

# create resource group
resource_client = ResourceManagementClient(credentials, SUBSCRIPTION_ID)
resource_client.resource_groups.create_or_update(
    RESOURCE_GROUP_NAME,
    {
        'location': LOCATION
    }
)

event_client = EventGridManagementClient(credentials, SUBSCRIPTION_ID)

# create a custom topic
event_client.topics.create_or_update(RESOURCE_GROUP_NAME, TOPIC_NAME, LOCATION)

# subscribe to a topic
scope = '/subscriptions/'+SUBSCRIPTION_ID+'/resourceGroups/'+RESOURCE_GROUP_NAME+'/providers/Microsoft.EventGrid/topics/'+TOPIC_NAME
event_client.event_subscriptions.create(scope, SUBSCRIPTION_NAME,
    {
        'destination': {
            'endpoint_url': REQUEST_BIN_URL
        }
    }
)

# send an event to topic
# get endpoint url
url = event_client.event_subscriptions.get_full_url(scope, SUBSCRIPTION_NAME).endpoint_url
# get key
key = event_client.topics.list_shared_access_keys(RESOURCE_GROUP_NAME,TOPIC_NAME).key1
headers = {'aeg-sas-key': key}
s = requests.get('https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/customevent.json')
r = requests.post(url, data=s, headers=headers)
print(r.status_code)
print(r.content)
```
送信されたばかりのイベントを表示するために、前に作成した RequestBin URL の場所に移動します。

リソースのクリーンアップ
```azurecli-interactive
az group delete --name gridResourceGroup
```

> [!div class="nextstepaction"]
> [Management API を探す](/python/api/overview/azure/eventgrid/management)

## <a name="learn-more"></a>詳細情報

[Event Grid SDK を使用してイベントを受信する](/azure/event-grid/receive-events)
