---
title: "インストール"
description: "Azure Python SDK のインストール方法"
keywords: Azure, Python, SDK, API
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/05/2017
ms.topic: install
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.assetid: 
ms.openlocfilehash: 1ee0c110673f908832c1c9e8fd6dac4c90c16e8e
ms.sourcegitcommit: ce2921d9a6f21d58fdf77cbc843c9b4af0ea796d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/31/2017
---
# <a name="installation"></a>インストール

## <a name="installation-with-pip"></a>pip によるインストール

各 Azure サービスのライブラリは、個別にインストールできます。

```bash
pip install azure-batch          # Install the latest Batch runtime library
pip install azure-mgmt-scheduler # Install the latest Storage management library
```

プレビュー パッケージをインストールするには、`--pre` フラグを使用します。

```bash
pip install --pre azure-mgmt-compute # will install only the latest Compute Management library
```

`azure` メタ パッケージを使用して、一連の Azure ライブラリを 1 行でインストールすることもできます。

```bash
pip install azure
```

このパッケージのプレビュー バージョンを公開します。このバージョンには、次のように、--pre フラグを使用してアクセスすることができます。

```bash
pip install --pre azure
```

## <a name="install-from-github"></a>GitHub からのインストール

`azure` をソースからインストールする場合は、次のようにします。

    git clone git://github.com/Azure/azure-sdk-for-python.git
    cd azure-sdk-for-python
    python setup.py install
