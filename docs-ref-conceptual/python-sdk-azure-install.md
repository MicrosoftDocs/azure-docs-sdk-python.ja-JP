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
# <a name="installation"></a><span data-ttu-id="41fb9-104">インストール</span><span class="sxs-lookup"><span data-stu-id="41fb9-104">Installation</span></span>

## <a name="which-python-and-which-version-to-use"></a><span data-ttu-id="41fb9-105">使用する Python とそのバージョン</span><span class="sxs-lookup"><span data-stu-id="41fb9-105">Which Python and which version to use</span></span>
<span data-ttu-id="41fb9-106">いくつかの Python インタープリターが提供されています。以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="41fb9-106">There are several Python interpreters available - examples include:</span></span>

* <span data-ttu-id="41fb9-107">CPython - 最も一般的に使用されている標準的な Python インタープリター</span><span class="sxs-lookup"><span data-stu-id="41fb9-107">CPython - the standard and most commonly used Python interpreter</span></span>
* <span data-ttu-id="41fb9-108">PyPy - CPython に準拠している高速な代替実装</span><span class="sxs-lookup"><span data-stu-id="41fb9-108">PyPy - fast, compliant alternative implementation to CPython</span></span>
* <span data-ttu-id="41fb9-109">IronPython - .Net/CLR 上で実行する Python インタープリター</span><span class="sxs-lookup"><span data-stu-id="41fb9-109">IronPython - Python interpreter that runs on .Net/CLR</span></span>
* <span data-ttu-id="41fb9-110">Jython - Java 仮想マシン上で実行する Python インタープリター</span><span class="sxs-lookup"><span data-stu-id="41fb9-110">Jython - Python interpreter that runs on the Java Virtual Machine</span></span>

<span data-ttu-id="41fb9-111">**CPython** v2.7 または v3.4 以降および PyPy 5.4.0 が、Python Azure SDK に対してテストされ、サポートされています。</span><span class="sxs-lookup"><span data-stu-id="41fb9-111">**CPython** v2.7 or v3.4+ and PyPy 5.4.0 are tested and supported for the Python Azure SDK.</span></span>

## <a name="where-to-get-python"></a><span data-ttu-id="41fb9-112">Python を入手するには</span><span class="sxs-lookup"><span data-stu-id="41fb9-112">Where to get Python?</span></span>
<span data-ttu-id="41fb9-113">CPython はいくつかの方法で入手できます。</span><span class="sxs-lookup"><span data-stu-id="41fb9-113">There are several ways to get CPython:</span></span>

* <span data-ttu-id="41fb9-114">[Python](https://www.python.org/) から直接入手</span><span class="sxs-lookup"><span data-stu-id="41fb9-114">Directly from [Python](https://www.python.org/)</span></span>
* <span data-ttu-id="41fb9-115">信頼できるディストリビューターから入手 ([Anaconda](https://www.anaconda.com/)、[Enthought](https://www.enthought.com/)、[ActiveState](https://www.activestate.com/) など)</span><span class="sxs-lookup"><span data-stu-id="41fb9-115">From a reputable distro such as [Anaconda](https://www.anaconda.com/), [Enthought](https://www.enthought.com/) or [ActiveState](https://www.activestate.com/)</span></span>
* <span data-ttu-id="41fb9-116">ソースからビルド</span><span class="sxs-lookup"><span data-stu-id="41fb9-116">Build from source!</span></span>

<span data-ttu-id="41fb9-117">特定のニーズがない限り、最初の 2 つの入手方法をお勧めします。</span><span class="sxs-lookup"><span data-stu-id="41fb9-117">Unless you have a specific need, we recommend the first two options.</span></span>

## <a name="installation-with-pip"></a><span data-ttu-id="41fb9-118">pip によるインストール</span><span class="sxs-lookup"><span data-stu-id="41fb9-118">Installation with pip</span></span>

<span data-ttu-id="41fb9-119">各 Azure サービスのライブラリは、個別にインストールできます。</span><span class="sxs-lookup"><span data-stu-id="41fb9-119">You can install each Azure service's library individually:</span></span>

```bash
pip install azure-batch          # Install the latest Batch runtime library
pip install azure-mgmt-scheduler # Install the latest Storage management library
```

<span data-ttu-id="41fb9-120">プレビュー パッケージをインストールするには、`--pre` フラグを使用します。</span><span class="sxs-lookup"><span data-stu-id="41fb9-120">Preview packages can be installed using the `--pre` flag:</span></span>

```bash
pip install --pre azure-mgmt-compute # will install only the latest Compute Management library
```

<span data-ttu-id="41fb9-121">`azure` メタ パッケージを使用して、一連の Azure ライブラリを 1 行でインストールすることもできます。</span><span class="sxs-lookup"><span data-stu-id="41fb9-121">You can also install a set of Azure libraries in a single line using the `azure` meta-package.</span></span>

```bash
pip install azure
```

<span data-ttu-id="41fb9-122">このパッケージのプレビュー バージョンを公開します。このバージョンには、次のように、--pre フラグを使用してアクセスすることができます。</span><span class="sxs-lookup"><span data-stu-id="41fb9-122">We publish a preview version of this package, which you can access using the --pre flag:</span></span>

```bash
pip install --pre azure
```

## <a name="install-from-github"></a><span data-ttu-id="41fb9-123">GitHub からのインストール</span><span class="sxs-lookup"><span data-stu-id="41fb9-123">Install from GitHub</span></span>

<span data-ttu-id="41fb9-124">`azure` をソースからインストールする場合は、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="41fb9-124">If you want to install `azure` from source:</span></span>

    git clone git://github.com/Azure/azure-sdk-for-python.git
    cd azure-sdk-for-python
    python setup.py install