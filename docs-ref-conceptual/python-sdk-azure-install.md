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
ms.openlocfilehash: 5ce4ef27667d45697200eef67be92c62812b3809
ms.sourcegitcommit: c57305dad01cad925faf50a64953c408429d4ca9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/05/2017
---
# <a name="installation"></a><span data-ttu-id="fcdf5-104">インストール</span><span class="sxs-lookup"><span data-stu-id="fcdf5-104">Installation</span></span>

## <a name="installation-with-pip"></a><span data-ttu-id="fcdf5-105">pip によるインストール</span><span class="sxs-lookup"><span data-stu-id="fcdf5-105">Installation with pip</span></span>

<span data-ttu-id="fcdf5-106">各 Azure サービスのライブラリは、個別にインストールできます。</span><span class="sxs-lookup"><span data-stu-id="fcdf5-106">You can install each Azure service's library individually:</span></span>

```bash
pip install azure-batch          # Install the latest Batch runtime library
pip install azure-mgmt-scheduler # Install the latest Storage management library
```

<span data-ttu-id="fcdf5-107">プレビュー パッケージをインストールするには、`--pre` フラグを使用します。</span><span class="sxs-lookup"><span data-stu-id="fcdf5-107">Preview packages can be installed using the `--pre` flag:</span></span>

```bash
pip install --pre azure-mgmt-compute # will install only the latest Compute Management library
```

<span data-ttu-id="fcdf5-108">`azure` メタ パッケージを使用して、一連の Azure ライブラリを 1 行でインストールすることもできます。</span><span class="sxs-lookup"><span data-stu-id="fcdf5-108">You can also install a set of Azure libraries in a single line using the `azure` meta-package.</span></span>

```bash
pip install azure
```

<span data-ttu-id="fcdf5-109">このパッケージのプレビュー バージョンを公開します。このバージョンには、次のように、--pre フラグを使用してアクセスすることができます。</span><span class="sxs-lookup"><span data-stu-id="fcdf5-109">We publish a preview version of this package, which you can access using the --pre flag:</span></span>

```bash
pip install --pre azure
```

## <a name="install-from-github"></a><span data-ttu-id="fcdf5-110">GitHub からのインストール</span><span class="sxs-lookup"><span data-stu-id="fcdf5-110">Install from GitHub</span></span>

<span data-ttu-id="fcdf5-111">`azure` をソースからインストールする場合は、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="fcdf5-111">If you want to install `azure` from source:</span></span>

    git clone git://github.com/Azure/azure-sdk-for-python.git
    cd azure-sdk-for-python
    python setup.py install
