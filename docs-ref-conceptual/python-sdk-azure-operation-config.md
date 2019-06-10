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
ms.openlocfilehash: adeb6aa8a2c363c3119e97503df9536fb0633b4c
ms.sourcegitcommit: 434186988284e0a8268a9de11645912a81226d6b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/29/2019
ms.locfileid: "66376863"
---
# <a name="operation-config"></a>操作の構成 

操作のメソッドには、`kwargs` で指定できる追加のパラメーターがあります。 これは operation_config と呼ばれます。

操作の構成のオプションは次のとおりです。

|パラメーター名|Type|Role|
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
