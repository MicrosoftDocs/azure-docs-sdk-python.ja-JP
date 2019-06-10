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
# <a name="operation-config"></a><span data-ttu-id="d450e-104">操作の構成</span><span class="sxs-lookup"><span data-stu-id="d450e-104">Operation config</span></span> 

<span data-ttu-id="d450e-105">操作のメソッドには、`kwargs` で指定できる追加のパラメーターがあります。</span><span class="sxs-lookup"><span data-stu-id="d450e-105">Methods on operations have extra parameters which can be provided in the `kwargs`.</span></span> <span data-ttu-id="d450e-106">これは operation_config と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="d450e-106">This is called operation_config.</span></span>

<span data-ttu-id="d450e-107">操作の構成のオプションは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="d450e-107">The options for operation configuration are:</span></span>

|<span data-ttu-id="d450e-108">パラメーター名</span><span class="sxs-lookup"><span data-stu-id="d450e-108">Parameter name</span></span>|<span data-ttu-id="d450e-109">Type</span><span class="sxs-lookup"><span data-stu-id="d450e-109">Type</span></span>|<span data-ttu-id="d450e-110">Role</span><span class="sxs-lookup"><span data-stu-id="d450e-110">Role</span></span>|
|----------------------|------|---------------|
| <span data-ttu-id="d450e-111">確認</span><span class="sxs-lookup"><span data-stu-id="d450e-111">verify</span></span> |`bool`|<span data-ttu-id="d450e-112">SSL 証明書を確認するかどうか。</span><span class="sxs-lookup"><span data-stu-id="d450e-112">Whether to verify the SSL certificate.</span></span> <span data-ttu-id="d450e-113">既定値は True です。</span><span class="sxs-lookup"><span data-stu-id="d450e-113">Default is True.</span></span>|
|  <span data-ttu-id="d450e-114">cert</span><span class="sxs-lookup"><span data-stu-id="d450e-114">cert</span></span> |`str`| <span data-ttu-id="d450e-115">クライアント側で確認するためのローカル証明書へのパス。</span><span class="sxs-lookup"><span data-stu-id="d450e-115">Path to local certificate for client side verification.</span></span>|
|  <span data-ttu-id="d450e-116">timeout</span><span class="sxs-lookup"><span data-stu-id="d450e-116">timeout</span></span> |`int`| <span data-ttu-id="d450e-117">サーバー接続を確立するときのタイムアウト時間 (秒単位)。</span><span class="sxs-lookup"><span data-stu-id="d450e-117">Timeout for establishing a server connection in seconds.</span></span>|
|  <span data-ttu-id="d450e-118">allow_redirects</span><span class="sxs-lookup"><span data-stu-id="d450e-118">allow_redirects</span></span> |`bool` | <span data-ttu-id="d450e-119">リダイレクトを許可するかどうか。</span><span class="sxs-lookup"><span data-stu-id="d450e-119">Whether to allow redirects.</span></span>|
|  <span data-ttu-id="d450e-120">max_redirects</span><span class="sxs-lookup"><span data-stu-id="d450e-120">max_redirects</span></span>  |`int`| <span data-ttu-id="d450e-121">許可されるリダイレクトの最大回数。</span><span class="sxs-lookup"><span data-stu-id="d450e-121">Maimum number of allowed redirects.</span></span>|
|  <span data-ttu-id="d450e-122">proxies</span><span class="sxs-lookup"><span data-stu-id="d450e-122">proxies</span></span>  |`dict` |<span data-ttu-id="d450e-123">プロキシ サーバーの設定。</span><span class="sxs-lookup"><span data-stu-id="d450e-123">Proxy server settings.</span></span>|
|  <span data-ttu-id="d450e-124">use_env_proxies</span><span class="sxs-lookup"><span data-stu-id="d450e-124">use_env_proxies</span></span> |`bool` |<span data-ttu-id="d450e-125">ローカル環境変数からプロキシ設定を読み取るかどうか。</span><span class="sxs-lookup"><span data-stu-id="d450e-125">Whether to read proxy settings from local environment variables.</span></span>|
|  <span data-ttu-id="d450e-126">retries</span><span class="sxs-lookup"><span data-stu-id="d450e-126">retries</span></span>  |`int` | <span data-ttu-id="d450e-127">再試行回数の合計。</span><span class="sxs-lookup"><span data-stu-id="d450e-127">Total number of retry attempts.</span></span>|
|  <span data-ttu-id="d450e-128">enable_http_logger</span><span class="sxs-lookup"><span data-stu-id="d450e-128">enable_http_logger</span></span> | `bool`| <span data-ttu-id="d450e-129">デバッグ モードで HTTP のログを有効にする (既定では False)。</span><span class="sxs-lookup"><span data-stu-id="d450e-129">Enable logs of HTTP in debug mode (False by default).</span></span>|
