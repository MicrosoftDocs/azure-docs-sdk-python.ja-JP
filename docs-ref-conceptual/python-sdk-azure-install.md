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
# <a name="installation"></a><span data-ttu-id="1fd48-104">インストール</span><span class="sxs-lookup"><span data-stu-id="1fd48-104">Installation</span></span>

## <a name="which-python-and-which-version-to-use"></a><span data-ttu-id="1fd48-105">使用する Python とそのバージョン</span><span class="sxs-lookup"><span data-stu-id="1fd48-105">Which Python and which version to use</span></span>

<span data-ttu-id="1fd48-106">いくつかの Python インタープリターが提供されています。以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="1fd48-106">There are several Python interpreters available - examples include:</span></span>

* <span data-ttu-id="1fd48-107">CPython - 最も一般的に使用されている標準的な Python インタープリター</span><span class="sxs-lookup"><span data-stu-id="1fd48-107">CPython - the standard and most commonly used Python interpreter</span></span>
* <span data-ttu-id="1fd48-108">PyPy - CPython に準拠している高速な代替実装</span><span class="sxs-lookup"><span data-stu-id="1fd48-108">PyPy - fast, compliant alternative implementation to CPython</span></span>
* <span data-ttu-id="1fd48-109">IronPython - .Net/CLR 上で実行する Python インタープリター</span><span class="sxs-lookup"><span data-stu-id="1fd48-109">IronPython - Python interpreter that runs on .Net/CLR</span></span>
* <span data-ttu-id="1fd48-110">Jython - Java 仮想マシン上で実行する Python インタープリター</span><span class="sxs-lookup"><span data-stu-id="1fd48-110">Jython - Python interpreter that runs on the Java Virtual Machine</span></span>

<span data-ttu-id="1fd48-111">**CPython** v2.7 または v3.4 以降および PyPy 5.4.0 が、Python Azure SDK に対してテストされ、サポートされています。</span><span class="sxs-lookup"><span data-stu-id="1fd48-111">**CPython** v2.7 or v3.4+ and PyPy 5.4.0 are tested and supported for the Python Azure SDK.</span></span>

## <a name="where-to-get-python"></a><span data-ttu-id="1fd48-112">Python を入手するには</span><span class="sxs-lookup"><span data-stu-id="1fd48-112">Where to get Python?</span></span>

<span data-ttu-id="1fd48-113">CPython はいくつかの方法で入手できます。</span><span class="sxs-lookup"><span data-stu-id="1fd48-113">There are several ways to get CPython:</span></span>

* <span data-ttu-id="1fd48-114">[Python](https://www.python.org/) から直接入手</span><span class="sxs-lookup"><span data-stu-id="1fd48-114">Directly from [Python](https://www.python.org/)</span></span>
* <span data-ttu-id="1fd48-115">信頼できるディストリビューターから入手 ([Anaconda](https://www.anaconda.com/)、[Enthought](https://www.enthought.com/)、[ActiveState](https://www.activestate.com/) など)</span><span class="sxs-lookup"><span data-stu-id="1fd48-115">From a reputable distro such as [Anaconda](https://www.anaconda.com/), [Enthought](https://www.enthought.com/) or [ActiveState](https://www.activestate.com/)</span></span>
* <span data-ttu-id="1fd48-116">ソースからビルド</span><span class="sxs-lookup"><span data-stu-id="1fd48-116">Build from source!</span></span>

<span data-ttu-id="1fd48-117">特定のニーズがない限り、最初の 2 つの入手方法をお勧めします。</span><span class="sxs-lookup"><span data-stu-id="1fd48-117">Unless you have a specific need, we recommend the first two options.</span></span>

## <a name="installation-with-pip"></a><span data-ttu-id="1fd48-118">pip によるインストール</span><span class="sxs-lookup"><span data-stu-id="1fd48-118">Installation with pip</span></span>

<span data-ttu-id="1fd48-119">各 Azure サービスのライブラリは、個別にインストールできます。</span><span class="sxs-lookup"><span data-stu-id="1fd48-119">You can install each Azure service's library individually:</span></span>

```bash
pip install azure-batch          # Install the latest Batch runtime library
pip install azure-mgmt-scheduler # Install the latest Storage management library
```

<span data-ttu-id="1fd48-120">プレビュー パッケージをインストールするには、`--pre` フラグを使用します。</span><span class="sxs-lookup"><span data-stu-id="1fd48-120">Preview packages can be installed using the `--pre` flag:</span></span>

```bash
pip install --pre azure-mgmt-compute # will install only the latest Compute Management library
```

<span data-ttu-id="1fd48-121">`azure` メタ パッケージを使用して、一連の Azure ライブラリを 1 行でインストールすることもできます。</span><span class="sxs-lookup"><span data-stu-id="1fd48-121">You can also install a set of Azure libraries in a single line using the `azure` meta-package.</span></span>

```bash
pip install azure
```

<span data-ttu-id="1fd48-122">このパッケージのプレビュー バージョンを公開します。このバージョンには、次のように、--pre フラグを使用してアクセスすることができます。</span><span class="sxs-lookup"><span data-stu-id="1fd48-122">We publish a preview version of this package, which you can access using the --pre flag:</span></span>

```bash
pip install --pre azure
```

## <a name="install-from-github"></a><span data-ttu-id="1fd48-123">GitHub からのインストール</span><span class="sxs-lookup"><span data-stu-id="1fd48-123">Install from GitHub</span></span>

<span data-ttu-id="1fd48-124">`azure` をソースからインストールする場合は、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="1fd48-124">If you want to install `azure` from source:</span></span>

```bash
git clone git://github.com/Azure/azure-sdk-for-python.git
cd azure-sdk-for-python
python setup.py install
```

## <a name="install-an-older-version-with-pip"></a><span data-ttu-id="1fd48-125">pip を使用した古いバージョンのインストール</span><span class="sxs-lookup"><span data-stu-id="1fd48-125">Install an older version with pip</span></span>
<span data-ttu-id="1fd48-126">'azure==3.0.0' とバージョン詳細を指定することにより、古いバージョンの `azure` をインストールできます。</span><span class="sxs-lookup"><span data-stu-id="1fd48-126">You can install an older version of `azure` by specifying 'azure==3.0.0' version details.</span></span>
```bash
pip install azure==3.0.0 
```
## <a name="check-sdk-installation-details-with-pip"></a><span data-ttu-id="1fd48-127">pip を使用した SDK のインストール詳細の確認</span><span class="sxs-lookup"><span data-stu-id="1fd48-127">Check SDK installation details with pip</span></span>
<span data-ttu-id="1fd48-128">`azure` SDK のインストール場所、バージョンの詳細などを確認できます。</span><span class="sxs-lookup"><span data-stu-id="1fd48-128">You can check `azure` SDK installation location, version details etc.</span></span>
```bash
pip show azure # Show installed version, location details etc.
pip freeze     # Output installed packages in requirements format.
pip list       # List installed packages, including editables.
```
## <a name="to-uninstall-with-pip"></a><span data-ttu-id="1fd48-129">pip でのアンインストール</span><span class="sxs-lookup"><span data-stu-id="1fd48-129">To uninstall with pip</span></span>
<span data-ttu-id="1fd48-130">`azure` メタ パッケージを使用して、すべての Azure ライブラリを 1 行でアンインストールできます。</span><span class="sxs-lookup"><span data-stu-id="1fd48-130">You can uninstall all Azure libraries in a single line using the `azure` meta-package.</span></span>
```bash
pip uninstall azure 
```
> [!NOTE]
> <span data-ttu-id="1fd48-131">`pip uninstall azure` により `azure` メタ パッケージが削除されますが、個別の `azure-*` パッケージ (および他の `adal` や `msrest` など) はそのまま残ります。</span><span class="sxs-lookup"><span data-stu-id="1fd48-131">`pip uninstall azure`removes the `azure` meta-package but leaves the individual `azure-*` packages behind (and others, like `adal` and `msrest` ).</span></span> <span data-ttu-id="1fd48-132">Python と pip の特徴として、依存関係のあるすべてのパッケージに関して、最初のパッケージをアンインストールしても依存関係はアンインストールされません。</span><span class="sxs-lookup"><span data-stu-id="1fd48-132">An aspect of Python and pip is that for all packages that have dependencies, uninstalling the initial package does not uninstall the dependencies.</span></span> <span data-ttu-id="1fd48-133">`azure-` およびそのサポート パッケージを削除するには、コマンド `pip freeze | grep 'azure-' | xargs pip uninstall -y` を実行します (その後、adal、msrest、msrestazure の個別のアンインストールを実行します)。</span><span class="sxs-lookup"><span data-stu-id="1fd48-133">To remove `azure-` and its supporting packages, run the command `pip freeze | grep 'azure-' | xargs pip uninstall -y` (and then perform individual uninstalls for adal, msrest, and msrestazure).</span></span>

