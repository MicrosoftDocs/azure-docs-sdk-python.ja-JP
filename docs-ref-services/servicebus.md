---
title: Python 用 Service Bus ライブラリ
description: Service Bus 用の Python クライアント ライブラリと管理ライブラリのリファレンス ドキュメント
keywords: Azure, Python, SDK, API, メッセージング, pubsub, pub-sub, メッセージ ブローカー
author: lisawong19
ms.author: liwong
manager: routlaw
ms.date: 02/21/2018
ms.topic: article
ms.devlang: python
ms.service: service-bus
ms.openlocfilehash: 02c172ff1a54d060c6af36a5a5daa5dcbff8795c
ms.sourcegitcommit: e35ec475d4b9d8061d0528a93aa8e1c4b7db6c0d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/02/2018
ms.locfileid: "39418957"
---
# <a name="service-bus-libraries-for-python"></a>Python 用 Service Bus ライブラリ

## <a name="overview"></a>概要

Microsoft Azure Service Bus は、信頼性の高いメッセージ キュー機能や永続的なパブリッシュ/サブスクライブ メッセージング機能など、クラウドベースのメッセージ指向ミドルウェアの一連のテクノロジをサポートしています。 

## <a name="install-the-libraries"></a>ライブラリをインストールする
```bash
pip install azure-servicebus
```

## <a name="servicebus-queues"></a>Service Bus キュー
プッシュ型配信 (ロング ポーリング) を使用した高度なメッセージング機能が要求されるシナリオでは、ストレージ キューよりも Service Bus キューの方が適している場合があります。より大きなメッセージ サイズへの対応、メッセージの順序付け、単一操作での破壊的な読み取り、スケジュール指定配信は、そのようなシナリオの例です。

このサービスでは、Shared Access Signature 認証または ACS 認証を使用することができます。

2014 年 8 月より後に Azure Portal を使用して作成された Service Bus 名前空間では、ACS 認証がサポートされません。 ACS に対応した名前空間は、Azure SDK で作成してください。

### <a name="shared-access-signature-authentication"></a>Shared Access Signature 認証

Shared Access Signature 認証を使用するには、次のコードで Service Bus サービスを作成します。

```python
from azure.servicebus import ServiceBusService

key_name = 'RootManageSharedAccessKey' # SharedAccessKeyName from Azure portal
key_value = '' # SharedAccessKey from Azure portal
sbs = ServiceBusService(service_namespace,
                        shared_access_key_name=key_name,
                        shared_access_key_value=key_value)
```

### <a name="acs-authentication"></a>ACS 認証

ACS 認証を使用するには、次のコードで Service Bus サービスを作成します。

```python
from azure.servicebus import ServiceBusService

account_key = '' # DEFAULT KEY from Azure portal
issuer = 'owner' # DEFAULT ISSUER from Azure portal
sbs = ServiceBusService(service_namespace,
                        account_key=account_key,
                        issuer=issuer)
```
### <a name="sending-and-receiving-messages"></a>メッセージの送受信

キューは、**create\_queue** メソッドを使用して作成できます。

```python
sbs.create_queue('taskqueue')
```
そのキューにメッセージを挿入するには、**send\_queue\_message** メソッドを呼び出します。

```python
from azure.servicebus import Message

msg = Message('Hello World!')
sbs.send_queue_message('taskqueue', msg)
```
**send\_queue\_message_batch** メソッドを使用すると、複数のメッセージを一度に送信することができます。

```python
from azure.servicebus import Message

msg1 = Message('Hello World!')
msg2 = Message('Hello World again!')
sbs.send_queue_message_batch('taskqueue', [msg1, msg2])
```
その後、**receive\_queue\_message** メソッドを呼び出すことで、メッセージをデキューすることができます。

```python
msg = sbs.receive_queue_message('taskqueue')
```

## <a name="servicebus-topics"></a>Service Bus トピック

Service Bus トピックは、Service Bus キュー上の発行/サブスクライブのシナリオを実装しやすくする抽象概念です。

サーバー側のトピックは、**create\_topic** メソッドを使用して作成できます。

```python
sbs.create_topic('taskdiscussion')
```
トピックには、**send\_topic\_message** メソッドを使用してメッセージを送信できます。

```python
from azure.servicebus import Message

msg = Message(b'Hello World!')
sbs.send_topic_message('taskdiscussion', msg)
```

**send\_topic\_message_batch** メソッドを使用すると、複数のメッセージを一度に送信することができます。

```python
from azure.servicebus import Message

msg1 = Message(b'Hello World!')
msg2 = Message(b'Hello World again!')
sbs.send_topic_message_batch('taskdiscussion', [msg1, msg2])
```

Python 3 では文字列型のメッセージが UTF-8 でエンコーディングされますが、Python 2 ではエンコーディングを独自に行う必要があることに注意してください。

その後クライアントは、**create\_subscription** メソッドを呼び出した後、**receive\_subscription\_message** メソッドを呼び出すことで、サブスクリプションを作成し、メッセージを取り出すことができます。 サブスクリプションが作成される前に送信されたメッセージは受信されないので注意してください。

```python
from azure.servicebus import Message

sbs.create_subscription('taskdiscussion', 'client1')
msg = Message('Hello World!')
sbs.send_topic_message('taskdiscussion', msg)
msg = sbs.receive_subscription_message('taskdiscussion', 'client1')
```

## <a name="event-hub"></a>イベント ハブ

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

## <a name="advanced-features"></a>高度な機能

### <a name="broker-properties-and-user-properties"></a>ブローカーのプロパティとユーザーのプロパティ

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

> [!div class="nextstepaction"]
> [Management API を探す](/python/api/overview/azure/servicebus/management)
