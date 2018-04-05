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
---
# <a name="operation-config"></a><span data-ttu-id="da411-104">操作の構成</span><span class="sxs-lookup"><span data-stu-id="da411-104">Operation config</span></span> 

<span data-ttu-id="da411-105">操作のメソッドには、kwargs で指定できる追加のパラメーターがあります。</span><span class="sxs-lookup"><span data-stu-id="da411-105">Methods on operations have extra parameters which can be provided in the kwargs.</span></span> <span data-ttu-id="da411-106">これは operation_config と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="da411-106">This is called operation_config.</span></span>

<span data-ttu-id="da411-107">操作の構成のオプションは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="da411-107">The options for operation configuration are:</span></span>

|<span data-ttu-id="da411-108">パラメーター名</span><span class="sxs-lookup"><span data-stu-id="da411-108">Parameter name</span></span>|<span data-ttu-id="da411-109">type</span><span class="sxs-lookup"><span data-stu-id="da411-109">Type</span></span>|<span data-ttu-id="da411-110">役割</span><span class="sxs-lookup"><span data-stu-id="da411-110">Role</span></span>|
|----------------------|------|---------------|
| <span data-ttu-id="da411-111">確認</span><span class="sxs-lookup"><span data-stu-id="da411-111">verify</span></span> |`bool`|<span data-ttu-id="da411-112">SSL 証明書を確認するかどうか。</span><span class="sxs-lookup"><span data-stu-id="da411-112">Whether to verify the SSL certificate.</span></span> <span data-ttu-id="da411-113">既定値は True です。</span><span class="sxs-lookup"><span data-stu-id="da411-113">Default is True.</span></span>|
|  <span data-ttu-id="da411-114">cert</span><span class="sxs-lookup"><span data-stu-id="da411-114">cert</span></span> |`str`| <span data-ttu-id="da411-115">クライアント側で確認するためのローカル証明書へのパス。</span><span class="sxs-lookup"><span data-stu-id="da411-115">Path to local certificate for client side verification.</span></span>|
|  <span data-ttu-id="da411-116">timeout</span><span class="sxs-lookup"><span data-stu-id="da411-116">timeout</span></span> |`int`| <span data-ttu-id="da411-117">サーバー接続を確立するときのタイムアウト時間 (秒単位)。</span><span class="sxs-lookup"><span data-stu-id="da411-117">Timeout for establishing a server connection in seconds.</span></span>|
|  <span data-ttu-id="da411-118">allow_redirects</span><span class="sxs-lookup"><span data-stu-id="da411-118">allow_redirects</span></span> |`bool` | <span data-ttu-id="da411-119">リダイレクトを許可するかどうか。</span><span class="sxs-lookup"><span data-stu-id="da411-119">Whether to allow redirects.</span></span>|
|  <span data-ttu-id="da411-120">max_redirects</span><span class="sxs-lookup"><span data-stu-id="da411-120">max_redirects</span></span>  |`int`| <span data-ttu-id="da411-121">許可されるリダイレクトの最大回数。</span><span class="sxs-lookup"><span data-stu-id="da411-121">Maimum number of allowed redirects.</span></span>|
|  <span data-ttu-id="da411-122">proxies</span><span class="sxs-lookup"><span data-stu-id="da411-122">proxies</span></span>  |`dict` |<span data-ttu-id="da411-123">プロキシ サーバーの設定。</span><span class="sxs-lookup"><span data-stu-id="da411-123">Proxy server settings.</span></span>|
|  <span data-ttu-id="da411-124">use_env_proxies</span><span class="sxs-lookup"><span data-stu-id="da411-124">use_env_proxies</span></span> |`bool` |<span data-ttu-id="da411-125">ローカル環境変数からプロキシ設定を読み取るかどうか。</span><span class="sxs-lookup"><span data-stu-id="da411-125">Whether to read proxy settings from local environment variables.</span></span>|
|  <span data-ttu-id="da411-126">retries</span><span class="sxs-lookup"><span data-stu-id="da411-126">retries</span></span>  |`int` | <span data-ttu-id="da411-127">再試行回数の合計。</span><span class="sxs-lookup"><span data-stu-id="da411-127">Total number of retry attempts.</span></span>|
|  <span data-ttu-id="da411-128">enable_http_logger</span><span class="sxs-lookup"><span data-stu-id="da411-128">enable_http_logger</span></span> | `bool`| <span data-ttu-id="da411-129">デバッグ モードで HTTP のログを有効にする (既定では False)。</span><span class="sxs-lookup"><span data-stu-id="da411-129">Enable logs of HTTP in debug mode (False by default).</span></span>|
