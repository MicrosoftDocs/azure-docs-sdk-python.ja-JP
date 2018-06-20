---
title: Azure SDK for Python の操作の構成
description: Azure SDK for Python によってスローされる C
keywords: Azure, Python, SDK, API, 例外
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 03/07/2018
ms.topic: article
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: d7be7cd76d019c6741d93c04458376a9352e363b
ms.sourcegitcommit: 41e6e6b5469271f4ec497a322b460e2a2af2c73d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/28/2018
ms.locfileid: "30204261"
---
# <a name="operation-config"></a>操作の構成 

操作のメソッドには、kwargs で指定できる追加のパラメーターがあります。 これは operation_config と呼ばれます。

操作の構成のオプションは次のとおりです。

|パラメーター名|type|役割|
|----------------------|------|---------------|
| 確認 |`bool`|SSL 証明書を確認するかどうか。 既定値は True です。|
|  cert |`str`| クライアント側で確認するためのローカル証明書へのパス。|
|  timeout |`int`| サーバー接続を確立するときのタイムアウト時間 (秒単位)。|
|  allow_redirects |`bool` | リダイレクトを許可するかどうか。|
|  max_redirects  |`int`| 許可されるリダイレクトの最大回数。|
|  proxies  |`dict` |プロキシ サーバーの設定。|
|  use_env_proxies |`bool` |ローカル環境変数からプロキシ設定を読み取るかどうか。|
|  retries  |`int` | 再試行回数の合計。|
|  enable_http_logger | `bool`| デバッグ モードで HTTP のログを有効にする (既定では False)。|
