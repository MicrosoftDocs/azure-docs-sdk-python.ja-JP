---
title: Python 用 Service Bus ライブラリ
description: Service Bus 用の Python クライアント ライブラリと管理ライブラリのリファレンス ドキュメント
keywords: Azure, Python, SDK, API, メッセージング, pubsub, pub-sub, メッセージ ブローカー
author: annatisch
ms.author: antisch
manager: mayurid
ms.date: 01/15/2019
ms.topic: article
ms.devlang: python
ms.service: service-bus
ms.openlocfilehash: 014f34b6ba3d3a2a8ce15afcd826195d6051f98a
ms.sourcegitcommit: 434186988284e0a8268a9de11645912a81226d6b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/29/2019
ms.locfileid: "66376764"
---
# <a name="service-bus-libraries-for-python"></a>Python 用 Service Bus ライブラリ

Microsoft Azure Service Bus は、信頼性の高いメッセージ キュー機能や永続的なパブリッシュ/サブスクライブ メッセージング機能など、クラウドベースのメッセージ指向ミドルウェアの一連のテクノロジをサポートしています。

* [SDK のソース コード](https://github.com/Azure/azure-sdk-for-python/tree/master/azure-servicebus)
* [SDK のリファレンス ドキュメント](https://docs.microsoft.com/python/api/overview/azure/servicebus/client?view=azure-python)

## <a name="whats-new-in-v0500"></a>v0.50.0 の新機能

バージョン 0.50.0 では、メッセージの送受信に、新しい AMQP ベースの API を使用できるようになりました。 この更新には、**破壊的変更**が含まれています。

現時点でのアップグレードが適切であるかどうかを判断するには、「[v0.21.1 から v0.50.0 への移行](#migration-from-v0211-to-v0500)」をお読みください。

新しい AMQP ベースの API では、メッセージの受け渡しの信頼性やパフォーマンスが向上し、今後の機能サポートも強化されます。
また、新しい API では、メッセージの送受信や処理に対する非同期操作 (asyncio に基づく) もサポートされます。

従来の HTTP ベースの操作については、「[HTTP ベースの操作による従来の API の使用](#using-http-based-operations-of-the-legacy-api)」を参照してください。

## <a name="prerequisites"></a>前提条件

* Azure サブスクリプション - [無料アカウント](https://azure.microsoft.com/free/)を作成できます
* Azure [Service Bus 名前空間と管理資格情報](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-create-namespace-portal)
* [Python 2.7 から 3.7](https://www.python.org/downloads/)

## <a name="installation"></a>インストール

```bash
pip install azure-servicebus
```

## <a name="connect-to-azure-service-bus"></a>Azure Service Bus に接続する

### <a name="get-credentials"></a>資格情報の取得

次の Azure CLI スニペットを使用して、環境変数に Service Bus 接続文字列を設定します (この値は、Azure portal で見つけることもできます)。 このスニペットは Bash シェル用にフォーマットされています。

```Bash
RES_GROUP=<resource-group-name>
NAMESPACE=<servicebus-namespace>

export SB_CONN_STR=$(az servicebus namespace authorization-rule keys list \
 --resource-group $RES_GROUP \
 --namespace-name $NAMESPACE \
 --name RootManageSharedAccessKey \
 --query primaryConnectionString \
 --output tsv)
```

### <a name="create-client"></a>クライアントの作成

`SB_CONN_STR` 環境変数を設定したら、ServiceBusClient を作成できます。

```python
import os
from azure.servicebus import ServiceBusClient

connection_str = os.environ['SB_CONN_STR']

sb_client = ServiceBusClient.from_connection_string(connection_str)
```

非同期操作を使用する場合は、`azure.servicebus.aio` 名前空間を使用してください。

```python
import os
from azure.servicebus.aio import ServiceBusClient

connection_str = os.environ['SB_CONN_STR']

sb_client = ServiceBusClient.from_connection_string(connection_str)
```

## <a name="service-bus-queues"></a>Service Bus キュー

プッシュ型配信 (ロング ポーリング) を使用した高度なメッセージング機能が要求されるシナリオでは、Storage キューよりも Service Bus キューの方が適している場合があります。より大きなメッセージ サイズへの対応、メッセージの順序付け、単一操作での破壊的な読み取り、スケジュール指定配信は、そのような機能の例です。

### <a name="create-queue"></a>キューの作成

これにより、Service Bus 名前空間内に新しいキューが作成されます。 同じ名前のキューが既に名前空間内に存在する場合は、エラーが発生します。

```python
sb_client.create_queue("MyQueue")
```

省略可能なパラメーターを指定して、キューの動作を構成することもできます。

```python
sb_client.create_queue(
    "MySessionQueue",
    requires_session=True  # Create a sessionful queue
    max_delivery_count=5  # Max delivery attempts per message
)
```

### <a name="get-a-queue-client"></a>キュー クライアントの取得

キューとの間でのメッセージの送受信や、その他の操作を実行するには、QueueClient を使用できます。

```python
queue_client = sb_client.get_queue("MyQueue")
```

### <a name="sending-messages"></a>メッセージの送信

キュー クライアントでは、一度に 1 つ以上のメッセージを送信できます。

```python
from azure.servicebus import Message

message = Message("Hello World")
queue_client.send(message)

message_one = Message("First")
message_two = Message("Second")
queue_client.send([message_one, message_two])
```

QueueClient.send に対する呼び出しごとに、新しいサービス接続が作成されます。 複数の送信呼び出しに同じ接続を再利用するために、送信側を開くことができます。

```python
message_one = Message("First")
message_two = Message("Second")

with queue_client.get_sender() as sender:
    sender.send(message_one)
    sender.send(message_two)
```

非同期クライアントを使用している場合は、上述の操作で非同期の構文を使用します。

```python
from azure.servicebus.aio import Message

message = Message("Hello World")
await queue_client.send(message)

message_one = Message("First")
message_two = Message("Second")
async with queue_client.get_sender() as sender:
    await sender.send(message_one)
    await sender.send(message_two)
```

### <a name="receiving-messages"></a>メッセージの受信

メッセージは、継続的な反復子としてキューから受信できます。 メッセージ受信の既定のモードは [PeekLock](https://docs.microsoft.com/rest/api/servicebus/peek-lock-message-non-destructive-read) です。この場合、メッセージがキューから削除されるように、各メッセージを明示的に完了する必要があります。

```python
messages = queue_client.get_receiver()
for message in messages:
    print(message)
    message.complete()
```

サービス接続は、反復子全体にわたって開かれたままになります。 メッセージ ストリームが部分的に反復処理されている場合は、`with` ステートメントで受信側を実行して、接続を確実に閉じる必要があります。

```python
with queue_client.get_receiver() as messages:
    for message in messages:
        print(message)
        message.complete()
        break
```
非同期クライアントを使用している場合は、上述の操作で非同期の構文を使用します。

```python
async with queue_client.get_receiver() as messages:
    async for message in messages:
        print(message)
        await message.complete()
        break
```

## <a name="service-bus-topics-and-subscriptions"></a>Service Bus トピックとサブスクリプション

Service Bus のトピックとサブスクリプションは、発行とサブスクライブのパターンで一対多形式の通信を提供する Service Bus キュー上の抽象化です。 メッセージはトピックに送信された後、関連付けられている 1 つ以上のサブスクリプションに配信されますが、この方法は多数の受信者にスケーリングする場合に便利です。

### <a name="create-topic"></a>トピックの作成

これにより、Service Bus 名前空間内に新しいトピックが作成されます。 同じ名前のトピックが既に存在する場合は、エラーが発生します。

```python
sb_client.create_topic("MyTopic")
```

### <a name="get-a-topic-client"></a>トピック クライアントの取得

トピックへのメッセージの送信や、その他の操作を実行するには、TopicClient を使用できます。

```python
topic_client = sb_client.get_topic("MyTopic")
```

### <a name="create-subscription"></a>サブスクリプションの作成

これにより、Service Bus 名前空間内に、指定したトピックに対する新しいサブスクリプションが作成されます。

```python
sb_client.create_subscription("MyTopic", "MySubscription")
```

### <a name="get-a-subscription-client"></a>サブスクリプション クライアントの取得

トピックからのメッセージの受信や、その他の操作を実行するには、SubscriptionClient を使用できます。

```python
topic_client = sb_client.get_subscription("MyTopic", "MySubscription")
```

## <a name="migration-from-v0211-to-v0500"></a>v0.21.1 から v0.50.0 への移行

バージョン 0.50.0 には、重要な破壊的変更が導入されました。 元の HTTP ベースの API はv0.50.0 でも引き続き使用できますが、新しい名前空間 `azure.servicebus.control_client` に存在します。

### <a name="should-i-upgrade"></a>アップグレードの必要性について

新しいパッケージ (v0.50.0) を v0.21.1 と比較した場合、HTTP ベースの操作に関しては改善点はありません。 HTTP ベースの API は、新しい名前空間に存在するという点を除いては同じです。 このため、HTTP ベースの操作 (`create_queue`、`delete_queue` など) のみを使用する場合は、現時点でアップグレードを行うメリットはありません。

### <a name="how-do-i-migrate-my-code-to-the-new-version"></a>コードを新しいバージョンに移行する方法

v0.21.0 に対して作成したコードは、次のようにインポートの名前空間を変更するだけで、バージョン 0.50.0 に移植できます。

```python
# from azure.servicebus import ServiceBusService  <- This will now raise an ImportError
from azure.servicebus.control_client import ServiceBusService

key_name = 'RootManageSharedAccessKey' # SharedAccessKeyName from Azure portal
key_value = '' # SharedAccessKey from Azure portal
sbs = ServiceBusService(service_namespace,
                        shared_access_key_name=key_name,
                        shared_access_key_value=key_value)
```

## <a name="using-http-based-operations-of-the-legacy-api"></a>HTTP ベースの操作による従来の API の使用

以下の説明は従来の API についてのもので、追加変更を加えずに既存のコードを v0.50.0 に移植することを希望するユーザーを対象としています。 このリファレンスは、v0.21.1 を使用しているユーザーもガイダンスとしても使用できます。
新しいコードを作成するユーザーについては、上で説明した新しい API の使用をお勧めします。

### <a name="service-bus-queues"></a>Service Bus キュー

#### <a name="shared-access-signature-sas-authentication"></a>Shared Access Signature (SAS) 認証

Shared Access Signature 認証を使用するには、次のコードで Service Bus サービスを作成します。

```python
from azure.servicebus.control_client import ServiceBusService

key_name = 'RootManageSharedAccessKey' # SharedAccessKeyName from Azure portal
key_value = '' # SharedAccessKey from Azure portal
sbs = ServiceBusService(service_namespace,
                        shared_access_key_name=key_name,
                        shared_access_key_value=key_value)
```

#### <a name="access-control-service-acs-authentication"></a>Azure Access Control Service (ACS) 認証

新しい Service Bus 名前空間では、ACS はサポートされていません。 [アプリケーションを SAS 認証に移行する](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-migrate-acs-sas)ことをお勧めします。 以前の Service Bus 名前空間内で ACS 認証を使用するには、以下を使用して ServiceBusService を作成します。

```python
from azure.servicebus.control_client import ServiceBusService

account_key = '' # DEFAULT KEY from Azure portal
issuer = 'owner' # DEFAULT ISSUER from Azure portal
sbs = ServiceBusService(service_namespace,
                        account_key=account_key,
                        issuer=issuer)
```

#### <a name="sending-and-receiving-messages"></a>メッセージの送受信

キューは、**create\_queue** メソッドを使用して作成できます。

```python
sbs.create_queue('taskqueue')
```

そのキューにメッセージを挿入するには、**send\_queue\_message** メソッドを呼び出します。

```python
from azure.servicebus.control_client import Message

msg = Message('Hello World!')
sbs.send_queue_message('taskqueue', msg)
```

**send\_queue\_message_batch** メソッドを使用すると、複数のメッセージを一度に送信することができます。

```python
from azure.servicebus.control_client import Message

msg1 = Message('Hello World!')
msg2 = Message('Hello World again!')
sbs.send_queue_message_batch('taskqueue', [msg1, msg2])
```

その後、**receive\_queue\_message** メソッドを呼び出すことで、メッセージをデキューすることができます。

```python
msg = sbs.receive_queue_message('taskqueue')
```

### <a name="service-bus-topics"></a>Service Bus トピック

サーバー側のトピックは、**create\_topic** メソッドを使用して作成できます。

```python
sbs.create_topic('taskdiscussion')
```

トピックには、**send\_topic\_message** メソッドを使用してメッセージを送信できます。

```python
from azure.servicebus.control_client import Message

msg = Message(b'Hello World!')
sbs.send_topic_message('taskdiscussion', msg)
```

**send\_topic\_message_batch** メソッドを使用すると、複数のメッセージを一度に送信することができます。

```python
from azure.servicebus.control_client import Message

msg1 = Message(b'Hello World!')
msg2 = Message(b'Hello World again!')
sbs.send_topic_message_batch('taskdiscussion', [msg1, msg2])
```

Python 3 では文字列型のメッセージが UTF-8 でエンコーディングされますが、Python 2 ではエンコーディングを独自に行う必要があることに注意してください。

その後クライアントは、**create\_subscription** メソッドを呼び出した後、**receive\_subscription\_message** メソッドを呼び出すことで、サブスクリプションを作成し、メッセージを取り出すことができます。 サブスクリプションが作成される前に送信されたメッセージは受信されないので注意してください。

```python
from azure.servicebus.control_client import Message

sbs.create_subscription('taskdiscussion', 'client1')
msg = Message('Hello World!')
sbs.send_topic_message('taskdiscussion', msg)
msg = sbs.receive_subscription_message('taskdiscussion', 'client1')
```

### <a name="event-hub"></a>イベント ハブ

Event Hubs を使用すると、デバイスとサービスの多様なセットから高速でイベント ストリームを収集できます。

イベント ハブは、**create\_event\_hub** メソッドを使用して作成できます。

```python
sbs.create_event_hub('myhub')
```

イベントを送信するには、次のようにします。

```python
sbs.send_event('myhub', '{ "DeviceId":"dev-01", "Temperature":"37.0" }')
```

イベントの内容は、複数のメッセージを含んだ JSON エンコード文字列またはイベント メッセージです。

### <a name="advanced-features"></a>高度な機能

#### <a name="broker-properties-and-user-properties"></a>ブローカーのプロパティとユーザーのプロパティ

ここでは、[こちら](https://docs.microsoft.com/rest/api/servicebus/message-headers-and-properties)に定義されているブローカーのプロパティとユーザーのプロパティの使い方について説明します。

```python
sent_msg = Message(b'This is the third message',
                   broker_properties={'Label': 'M3'},
                   custom_properties={'Priority': 'Medium',
                                      'Customer': 'ABC'}
            )
```

datetime、int、float、boolean を使用することができます。

```python
props = {'hello': 'world',
         'number': 42,
         'active': True,
         'deceased': False,
         'large': 8555111000,
         'floating': 3.14,
         'dob': datetime(2011, 12, 14),
         'double_quote_message': 'This "should" work fine',
         'quote_message': "This 'should' work fine"}
sent_msg = Message(b'message with properties', custom_properties=props)
```

このライブラリの旧バージョンとの互換性を確保するために、`broker_properties` を JSON 文字列として定義してもかまいません。
その場合は、有効な JSON 文字列をご自身で確実に記述してください。RestAPI を送信する前に、Python によるチェックは実行されません。

```python
broker_properties = '{"ForcePersistence": false, "Label": "My label"}'
sent_msg = Message(b'receive message',
                   broker_properties = broker_properties
)
```

## <a name="next-steps"></a>次の手順

* [Service Bus のドキュメント](https://docs.microsoft.com/azure/service-bus-messaging)
* [SDK のソース コード](https://github.com/Azure/azure-sdk-for-python/tree/master/azure-servicebus)
* [SDK のリファレンス ドキュメント](https://docs.microsoft.com/python/api/overview/azure/servicebus/client?view=azure-python)
* [その他のサンプル](https://github.com/Azure/azure-sdk-for-python/tree/master/azure-servicebus/examples)
