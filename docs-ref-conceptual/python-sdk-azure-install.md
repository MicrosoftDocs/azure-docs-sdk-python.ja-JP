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
ms.openlocfilehash: 9fd11cbc7b987b970ceee85c7b11b22e3d6299ea
ms.sourcegitcommit: 31d7df367b15ec09a5a610eb333295bba0f6b351
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/26/2019
ms.locfileid: "67395449"
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

```bash
git clone git://github.com/Azure/azure-sdk-for-python.git
cd azure-sdk-for-python
python setup.py install
```

## <a name="install-an-older-version-with-pip"></a>pip を使用した古いバージョンのインストール
'azure==3.0.0' とバージョン詳細を指定することにより、古いバージョンの `azure` をインストールできます。
```bash
pip install azure==3.0.0 
```
## <a name="check-sdk-installation-details-with-pip"></a>pip を使用した SDK のインストール詳細の確認
`azure` SDK のインストール場所、バージョンの詳細などを確認できます。
```bash
pip show azure # Show installed version, location details etc.
pip freeze     # Output installed packages in requirements format.
pip list       # List installed packages, including editables.
```
## <a name="to-uninstall-with-pip"></a>pip でのアンインストール
`azure` メタ パッケージを使用して、すべての Azure ライブラリを 1 行でアンインストールできます。
```bash
pip uninstall azure 
```
> [!NOTE]
> `pip uninstall azure` により `azure` メタ パッケージが削除されますが、個別の `azure-*` パッケージ (および他の `adal` や `msrest` など) はそのまま残ります。 Python と pip の特徴として、依存関係のあるすべてのパッケージに関して、最初のパッケージをアンインストールしても依存関係はアンインストールされません。 `azure-` およびそのサポート パッケージを削除するには、コマンド `pip freeze | grep 'azure-' | xargs pip uninstall -y` を実行します (その後、adal、msrest、msrestazure の個別のアンインストールを実行します)。

