---
title: "Python 用 Service Bus ライブラリ"
description: "Service Bus 用の Python クライアント ライブラリと管理ライブラリのリファレンス ドキュメント"
keywords: "Azure, Python, SDK, API, メッセージング, pubsub, pub-sub, メッセージ ブローカー"
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/26/2017
ms.topic: article
ms.devlang: python
ms.service: service-bus
ms.openlocfilehash: bf7be945f2c7a3daea93ff4e5b770459c00632c8
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/18/2017
---
# <a name="service-bus-libraries-for-python"></a>Python 用 Service Bus ライブラリ

## <a name="overview"></a>概要

Microsoft Azure Service Bus は、信頼性の高いメッセージ キュー機能や永続的なパブリッシュ/サブスクライブ メッセージング機能など、クラウドベースのメッセージ指向ミドルウェアの一連のテクノロジをサポートしています。 

## <a name="install-the-libraries"></a>ライブラリをインストールする
```bash
pip install azure-mgmt-servicebus
```

## <a name="example"></a>例
メッセージをキューに送信する

```python
from azure.servicebus import Message

msg1 = Message('Hello World!')
msg2 = Message('Hello World again!')
sbs.send_queue_message_batch('taskqueue', [msg1, msg2])
# dequeue the message
msg = sbs.receive_queue_message('taskqueue')
```
> [!div class="nextstepaction"]
> [Management API を探す](/python/api/overview/azure/servicebus/managementlibrary)

