---
title: インストール
description: Azure Python SDK のインストール方法
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
ms.openlocfilehash: 792feac12f8328e2467017530065350e347c59b7
ms.sourcegitcommit: 757bf84535fd9d8299c4b51ec92a5ab1926cb671
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
ms.locfileid: "29565821"
---
# <a name="installation"></a>インストール

## <a name="which-python-and-which-version-to-use"></a>使用する Python とそのバージョン
いくつかの Python インタープリターが提供されています。以下に例を示します。

* CPython - 最も一般的に使用されている標準的な Python インタープリター
* PyPy - CPython に準拠している高速な代替実装
* IronPython - .Net/CLR 上で実行する Python インタープリター
* Jython - Java 仮想マシン上で実行する Python インタープリター

**CPython** v2.7 または v3.4 以降および PyPy 5.4.0 が、Python Azure SDK に対してテストされ、サポートされています。

## <a name="where-to-get-python"></a>Python を入手するには
CPython はいくつかの方法で入手できます。

* [Python](https://www.python.org/) から直接入手
* 信頼できるディストリビューターから入手 ([Anaconda](https://www.anaconda.com/)、[Enthought](https://www.enthought.com/)、[ActiveState](https://www.activestate.com/) など)
* ソースからビルド

特定のニーズがない限り、最初の 2 つの入手方法をお勧めします。

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