---
title: Python 用 Azure Cognitive Services モジュール
description: Python 用 Azure Cognitive Services モジュールのリファレンス
keywords: Azure, python, SDK, API, Cognitive Services
author: rloutlaw
ms.author: routlaw
manager: angerobe
ms.date: 04/04/2018
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 5a23a52414e70facd6feae3af3956a5131f6b5c4
ms.sourcegitcommit: 46bebbf5dd558750043ce5afadff2ec3714a54e6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2019
ms.locfileid: "67534341"
---
# <a name="azure-cognitive-services-modules-for-python"></a><span data-ttu-id="04650-104">Python 用 Azure Cognitive Services モジュール</span><span class="sxs-lookup"><span data-stu-id="04650-104">Azure Cognitive Services modules for Python</span></span>

<span data-ttu-id="04650-105">Python 用 Azure Cognitive Services モジュールを使用して、画像および顔認識、言語分析、検索を Python アプリ、Web サイト、ツールに追加します。</span><span class="sxs-lookup"><span data-stu-id="04650-105">Add image and face recognition, language analysis, and search to your Python apps, websites, and tools using the Azure Cognitive Services modules for Python.</span></span>

## <a name="vision-modules"></a><span data-ttu-id="04650-106">Vision モジュール</span><span class="sxs-lookup"><span data-stu-id="04650-106">Vision modules</span></span>

### <a name="computer-vision"></a><span data-ttu-id="04650-107">Computer Vision</span><span class="sxs-lookup"><span data-stu-id="04650-107">Computer Vision</span></span> 

<span data-ttu-id="04650-108">画像内にあるビジュアル コンテンツに関する情報を返します。</span><span class="sxs-lookup"><span data-stu-id="04650-108">Returns information about visual content found in an image:</span></span>

- <span data-ttu-id="04650-109">タグ付け、説明、ドメイン固有モデルを使用してコンテンツを特定し、確実にラベル付けします。</span><span class="sxs-lookup"><span data-stu-id="04650-109">Use tagging, descriptions, and domain-specific models to identify content and label it with confidence.</span></span>
- <span data-ttu-id="04650-110">成人向け/わいせつな描写に対する設定を適用すれば、アダルト コンテンツの自動制限を有効にできます。</span><span class="sxs-lookup"><span data-stu-id="04650-110">Apply adult/racy settings to enable automated restriction of adult content.</span></span>
- <span data-ttu-id="04650-111">画像の種類や写真内の配色を特定します。</span><span class="sxs-lookup"><span data-stu-id="04650-111">Identify image types and color schemes in pictures.</span></span>

<span data-ttu-id="04650-112">お使いのブラウザーで無料の [Computer Vision を試す](https://azure.microsoft.com/en-us/services/cognitive-services/computer-vision/)。</span><span class="sxs-lookup"><span data-stu-id="04650-112">[Try Computer Vision](https://azure.microsoft.com/en-us/services/cognitive-services/computer-vision/) for free in your browser.</span></span>

<span data-ttu-id="04650-113">[pip](https://pip.pypa.io/en/stable/quickstart/) で Python モジュールを入手する:</span><span class="sxs-lookup"><span data-stu-id="04650-113">Get the Python module with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```python
pip install azure-cognitiveservices-vision-computervision
```

<span data-ttu-id="04650-114">Computer Vision API の[詳細](/azure/cognitive-services/computer-vision/home)を確認し、[Computer Vision API Python クイックスタート](/azure/cognitive-services/computer-vision/quickstarts/python)を開始する。</span><span class="sxs-lookup"><span data-stu-id="04650-114">[Learn more](/azure/cognitive-services/computer-vision/home) about the Computer Vision API and get started with the [Computer Vision API Python quickstart](/azure/cognitive-services/computer-vision/quickstarts/python).</span></span>

### <a name="content-moderator"></a><span data-ttu-id="04650-115">Content Moderator</span><span class="sxs-lookup"><span data-stu-id="04650-115">Content Moderator</span></span>

<span data-ttu-id="04650-116">マシンによるテキスト、ビデオ、および画像のモデレート機能を、人によるレビュー ツールで強化します。</span><span class="sxs-lookup"><span data-stu-id="04650-116">Machine-assisted moderation of text, video and images, augmented with human review tools.</span></span>

<span data-ttu-id="04650-117">[pip](https://pip.pypa.io/en/stable/quickstart/) で Python モジュールを入手する:</span><span class="sxs-lookup"><span data-stu-id="04650-117">Get the Python module with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```python
pip install azure-cognitiveservices-vision-contentmoderator
```

<span data-ttu-id="04650-118">Content Moderator サービスの[詳細](/azure/cognitive-services/content-moderator/overview)を確認する。</span><span class="sxs-lookup"><span data-stu-id="04650-118">[Learn more](/azure/cognitive-services/content-moderator/overview) about the Content Moderator service.</span></span>

### <a name="custom-vision-service"></a><span data-ttu-id="04650-119">Custom Vision Service</span><span class="sxs-lookup"><span data-stu-id="04650-119">Custom Vision Service</span></span>

<span data-ttu-id="04650-120">画像をアップロードして、特定のユース ケースに合わせて、Computer Vision モデルをトレーニングおよびカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="04650-120">Upload images to train and customize a computer vision model for your specific use case.</span></span> <span data-ttu-id="04650-121">モデルをトレーニングすることで、API でモデルを使って画像にタグ付けし、結果を評価して、分類器を向上させることができます。</span><span class="sxs-lookup"><span data-stu-id="04650-121">Once the model is trained, you can use the API to tag images using the model and evaluate the results to improve your classifier.</span></span>

<span data-ttu-id="04650-122">[pip](https://pip.pypa.io/en/stable/quickstart/) で Python モジュールを入手する:</span><span class="sxs-lookup"><span data-stu-id="04650-122">Get the Python module with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```python
pip install azure-cognitiveservices-vision-customvision
```

<span data-ttu-id="04650-123">Custom Vision Service の[詳細](/azure/cognitive-services/Custom-Vision-Service/home)を確認し、[Custom Vision Python チュートリアル](/azure/cognitive-services/Custom-Vision-Service/python-tutorial)を開始する</span><span class="sxs-lookup"><span data-stu-id="04650-123">[Learn more](/azure/cognitive-services/Custom-Vision-Service/home) about the Custom Vision service and get started with the [Custom Vision Python tutorial](/azure/cognitive-services/Custom-Vision-Service/python-tutorial)</span></span>

### <a name="face-api"></a><span data-ttu-id="04650-124">Face API</span><span class="sxs-lookup"><span data-stu-id="04650-124">Face API</span></span>

<span data-ttu-id="04650-125">写真に含まれる顔を検出、識別、分析、グループ化、タグ付けします。</span><span class="sxs-lookup"><span data-stu-id="04650-125">Detect, identify, analyze, organize, and tag faces in photos.</span></span> 

<span data-ttu-id="04650-126">お使いのブラウザーで [Face API を試す](https://azure.microsoft.com/en-us/services/cognitive-services/face/)。</span><span class="sxs-lookup"><span data-stu-id="04650-126">[Try the Face API](https://azure.microsoft.com/en-us/services/cognitive-services/face/) in your browser.</span></span>

<span data-ttu-id="04650-127">[pip](https://pip.pypa.io/en/stable/quickstart/) で Python モジュールを入手する:</span><span class="sxs-lookup"><span data-stu-id="04650-127">Get the Python module with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```python
pip install cognitive-face
```

<span data-ttu-id="04650-128">Face API の[詳細](/azure/cognitive-services/face/overview)を確認し、[Face API Python クイックスタート](/azure/cognitive-services/Face/Tutorials/FaceAPIinPythonTutorial)を開始する。</span><span class="sxs-lookup"><span data-stu-id="04650-128">[Learn more](/azure/cognitive-services/face/overview) about the Face API and get started with the [Face API Python quickstart](/azure/cognitive-services/Face/Tutorials/FaceAPIinPythonTutorial).</span></span>

## <a name="search-modules"></a><span data-ttu-id="04650-129">Search モジュール</span><span class="sxs-lookup"><span data-stu-id="04650-129">Search modules</span></span>

### <a name="web-search"></a><span data-ttu-id="04650-130">Web 検索</span><span class="sxs-lookup"><span data-stu-id="04650-130">Web search</span></span>

<span data-ttu-id="04650-131">Bing Web Search API でインデックス設定された Web ドキュメントを取得し、結果の種類、新しさなどで結果を絞り込みます。</span><span class="sxs-lookup"><span data-stu-id="04650-131">Retrieve web documents indexed by the Bing Web Search API and narrow down the results by result type, freshness and more.</span></span> 

<span data-ttu-id="04650-132">お使いのブラウザーで [Web Search API を試す](https://azure.microsoft.com/en-us/services/cognitive-services/bing-web-search-api/)。</span><span class="sxs-lookup"><span data-stu-id="04650-132">[Try the Web Search API](https://azure.microsoft.com/en-us/services/cognitive-services/bing-web-search-api/) in your browser.</span></span>

<span data-ttu-id="04650-133">[pip](https://pip.pypa.io/en/stable/quickstart/) で Python モジュールを入手する:</span><span class="sxs-lookup"><span data-stu-id="04650-133">Get the Python module with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```python
pip install azure-cognitiveservices-search-websearch
```

<span data-ttu-id="04650-134">Bing Web Search API の[詳細](/azure/cognitive-services/bing-web-search/overview)を確認し、[Web Search API Python クイックスタート](/azure/cognitive-services/bing-web-search/quickstarts/python)を開始する。</span><span class="sxs-lookup"><span data-stu-id="04650-134">[Learn more](/azure/cognitive-services/bing-web-search/overview) about the Bing Web Search API and get started with the [Web Search API Python quickstart](/azure/cognitive-services/bing-web-search/quickstarts/python).</span></span>

### <a name="image-search"></a><span data-ttu-id="04650-135">イメージの検索</span><span class="sxs-lookup"><span data-stu-id="04650-135">Image search</span></span>

<span data-ttu-id="04650-136">画像を検索し、検索結果として、サムネイル、完全な画像 URL、画像のメタデータなどを取得します。</span><span class="sxs-lookup"><span data-stu-id="04650-136">Search for images and get thumbnails, full image URLs, image metadata and more in your results.</span></span>

<span data-ttu-id="04650-137">お使いのブラウザーで [Image Search API を試す](https://azure.microsoft.com/en-us/services/cognitive-services/bing-image-search-api/)。</span><span class="sxs-lookup"><span data-stu-id="04650-137">[Try the Image Search API](https://azure.microsoft.com/en-us/services/cognitive-services/bing-image-search-api/) in your browser.</span></span>

<span data-ttu-id="04650-138">[pip](https://pip.pypa.io/en/stable/quickstart/) で Python モジュールを入手する:</span><span class="sxs-lookup"><span data-stu-id="04650-138">Get the Python module with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```python
pip install azure-cognitiveservices-search-imagesearch
```

<span data-ttu-id="04650-139">Bing Image Search API の[詳細](/azure/cognitive-services/bing-image-search/overview)を確認し、[Image Search API Python クイックスタート](/azure/cognitive-services/bing-image-search/quickstarts/python)を開始する。</span><span class="sxs-lookup"><span data-stu-id="04650-139">[Learn more](/azure/cognitive-services/bing-image-search/overview) about the Bing Image Search API and get started with the [Image Search API Python quickstart](/azure/cognitive-services/bing-image-search/quickstarts/python).</span></span>


### <a name="entity-search"></a><span data-ttu-id="04650-140">エンティティ検索</span><span class="sxs-lookup"><span data-stu-id="04650-140">Entity search</span></span>

<span data-ttu-id="04650-141">指定した検索用語または場所に最も関連性のあるエンティティ (場所、人、または物事) を検索します。</span><span class="sxs-lookup"><span data-stu-id="04650-141">Search for the most relevant entity (place, person, or thing) for a given search term or location.</span></span>

<span data-ttu-id="04650-142">お使いのブラウザーで [Entity Search API を試す](https://azure.microsoft.com/services/cognitive-services/bing-entity-search-api/)。</span><span class="sxs-lookup"><span data-stu-id="04650-142">[Try the Entity Search API](https://azure.microsoft.com/services/cognitive-services/bing-entity-search-api/) in your browser.</span></span>

<span data-ttu-id="04650-143">[pip](https://pip.pypa.io/en/stable/quickstart/) で Python モジュールを入手する:</span><span class="sxs-lookup"><span data-stu-id="04650-143">Get the Python module with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```python
pip install azure-cognitiveservices-search-entitysearch
```

<span data-ttu-id="04650-144">Bing Entity Search API の[詳細](/azure/cognitive-services/bing-entities-search/search-the-web)を確認し、[Entity Search API Python クイックスタート](/azure/cognitive-services/bing-entities-search/quickstarts/python)を開始する。</span><span class="sxs-lookup"><span data-stu-id="04650-144">[Learn more](/azure/cognitive-services/bing-entities-search/search-the-web) about the Bing Entity Search API and get started with the [Entity Search API Python quickstart](/azure/cognitive-services/bing-entities-search/quickstarts/python).</span></span>

### <a name="custom-search"></a><span data-ttu-id="04650-145">カスタム検索</span><span class="sxs-lookup"><span data-stu-id="04650-145">Custom search</span></span>

<span data-ttu-id="04650-146">ご自身の特定の検索ドメインに対応するカスタム Web 検索を作成します。</span><span class="sxs-lookup"><span data-stu-id="04650-146">Build and a custom web search that meets your specific search domain.</span></span>

<span data-ttu-id="04650-147">[pip](https://pip.pypa.io/en/stable/quickstart/) で Python モジュールを入手する:</span><span class="sxs-lookup"><span data-stu-id="04650-147">Get the Python module with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```python
pip install azure-cognitiveservices-search-customsearch
```

<span data-ttu-id="04650-148">Bing Custom Search サービスの[詳細](/azure/cognitive-services/bing-custom-search/)を確認し、[Custom Search API Python クイックスタート](/azure/cognitive-services/bing-custom-search/call-endpoint-python)で Python からご自身のカスタム検索のクエリを開始する。</span><span class="sxs-lookup"><span data-stu-id="04650-148">[Learn more](/azure/cognitive-services/bing-custom-search/) about the Bing Custom Search service and get started with querying your custom search from Python with the [Custom Search API Python quickstart](/azure/cognitive-services/bing-custom-search/call-endpoint-python).</span></span>

### <a name="video-search"></a><span data-ttu-id="04650-149">ビデオ検索</span><span class="sxs-lookup"><span data-stu-id="04650-149">Video search</span></span>

<span data-ttu-id="04650-150">Web 上のビデオを検索し、作成者、エンコード、長さ、および閲覧数のメタデータを含む結果を取得します。</span><span class="sxs-lookup"><span data-stu-id="04650-150">Find videos across the web and get results with creator, encoding, length, and view count metadata.</span></span>

<span data-ttu-id="04650-151">お使いのブラウザーで [Video Search API を試す](https://azure.microsoft.com/services/cognitive-services/bing-video-search-api/)。</span><span class="sxs-lookup"><span data-stu-id="04650-151">[Try the Video Search API](https://azure.microsoft.com/services/cognitive-services/bing-video-search-api/) in your browser.</span></span>

<span data-ttu-id="04650-152">[pip](https://pip.pypa.io/en/stable/quickstart/) で Python モジュールを入手する:</span><span class="sxs-lookup"><span data-stu-id="04650-152">Get the Python module with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```python
pip install azure-cognitiveservices-search-videosearch
```

<span data-ttu-id="04650-153">Bing Video Search サービスの[詳細](/azure/cognitive-services/bing-video-search/search-the-web)を確認し、[Video Search API Python クイックスタート](/azure/cognitive-services/bing-video-search/python)を開始する。</span><span class="sxs-lookup"><span data-stu-id="04650-153">[Learn more](/azure/cognitive-services/bing-video-search/search-the-web) about the Bing Video Search service and get started with the [Video Search API Python quickstart](/azure/cognitive-services/bing-video-search/python).</span></span>


### <a name="news-search"></a><span data-ttu-id="04650-154">ニュース検索</span><span class="sxs-lookup"><span data-stu-id="04650-154">News search</span></span>

<span data-ttu-id="04650-155">Web 上のニュース記事を検索し、記事、関連するニュース、画像、およびプロバイダー情報のメタデータを操作します。</span><span class="sxs-lookup"><span data-stu-id="04650-155">Search the web for news articles and work with article, related news, images, and provider info metadata.</span></span>

<span data-ttu-id="04650-156">お使いのブラウザーで [News Search API を試す](https://azure.microsoft.com/services/cognitive-services/bing-news-search-api/)。</span><span class="sxs-lookup"><span data-stu-id="04650-156">[Try the News Search API](https://azure.microsoft.com/services/cognitive-services/bing-news-search-api/) in your browser.</span></span>

<span data-ttu-id="04650-157">[pip](https://pip.pypa.io/en/stable/quickstart/) で Python モジュールを入手する:</span><span class="sxs-lookup"><span data-stu-id="04650-157">Get the Python module with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```python
pip install azure-cognitiveservices-search-newssearch
```

<span data-ttu-id="04650-158">Bing News Search サービスの[詳細](/azure/cognitive-services/bing-news-search/search-the-web)を確認し、[News Search API Python クイックスタート](/azure/cognitive-services/bing-news-search/python)を開始する。</span><span class="sxs-lookup"><span data-stu-id="04650-158">[Learn more](/azure/cognitive-services/bing-news-search/search-the-web) about the Bing News Search service and get started with the [News Search API Python quickstart](/azure/cognitive-services/bing-news-search/python).</span></span>


## <a name="language-modules"></a><span data-ttu-id="04650-159">言語モジュール</span><span class="sxs-lookup"><span data-stu-id="04650-159">Language modules</span></span>

### <a name="text-analytics"></a><span data-ttu-id="04650-160">Text Analytics</span><span class="sxs-lookup"><span data-stu-id="04650-160">Text Analytics</span></span> 

<span data-ttu-id="04650-161">Text Analytics API は、未加工のテキストに対する自然言語処理を提供するクラウドベースのサービスです。</span><span class="sxs-lookup"><span data-stu-id="04650-161">The Text Analytics API is a cloud-based service that provides  natural language processing over raw text.</span></span> <span data-ttu-id="04650-162">この API の主要機能は次の 3 つです。</span><span class="sxs-lookup"><span data-stu-id="04650-162">The API includes three main functions:</span></span>

- <span data-ttu-id="04650-163">センチメント分析</span><span class="sxs-lookup"><span data-stu-id="04650-163">Sentiment analysis</span></span>
- <span data-ttu-id="04650-164">キー フレーズの抽出</span><span class="sxs-lookup"><span data-stu-id="04650-164">Key phrase extraction</span></span>
- <span data-ttu-id="04650-165">言語検出</span><span class="sxs-lookup"><span data-stu-id="04650-165">Language detection</span></span>

<span data-ttu-id="04650-166">お使いのブラウザーで [Text Analytics API を試す](https://azure.microsoft.com/en-us/services/cognitive-services/text-analytics/)。</span><span class="sxs-lookup"><span data-stu-id="04650-166">[Try the Text Analytics API](https://azure.microsoft.com/en-us/services/cognitive-services/text-analytics/) in your browser.</span></span>

<span data-ttu-id="04650-167">[pip](https://pip.pypa.io/en/stable/quickstart/) で Python モジュールを入手する:</span><span class="sxs-lookup"><span data-stu-id="04650-167">Get the Python module with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```python
pip install azure-cognitiveservices-language-textanalytics
```

<span data-ttu-id="04650-168">Text Analytics API の[詳細](/azure/cognitive-services/text-analytics/overview)を確認し、[Text Analytics API Python クイックスタート](/azure/cognitive-services/text-analytics/quickstarts/python)を開始する。</span><span class="sxs-lookup"><span data-stu-id="04650-168">[Learn more](/azure/cognitive-services/text-analytics/overview) about the Text Analytics API and get started with the [Text Analytics API Python quickstart](/azure/cognitive-services/text-analytics/quickstarts/python).</span></span>


### <a name="spell-check"></a><span data-ttu-id="04650-169">スペル チェック</span><span class="sxs-lookup"><span data-stu-id="04650-169">Spell Check</span></span>

<span data-ttu-id="04650-170">Bing Spell Check API を使用して、コンテキストに応じた文法およびスペル チェックを実行します。</span><span class="sxs-lookup"><span data-stu-id="04650-170">Perform contextual grammar and spell checking with the Bing Spell Check API.</span></span>

<span data-ttu-id="04650-171">お使いのブラウザーで [Spell Check API を試す](https://azure.microsoft.com/en-us/services/cognitive-services/spell-check/)。</span><span class="sxs-lookup"><span data-stu-id="04650-171">[Try the Spell Check API](https://azure.microsoft.com/en-us/services/cognitive-services/spell-check/) in your browser.</span></span>

<span data-ttu-id="04650-172">[pip](https://pip.pypa.io/en/stable/quickstart/) で Python モジュールを入手する:</span><span class="sxs-lookup"><span data-stu-id="04650-172">Get the Python module with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```python
pip install azure-cognitiveservices-language-spellcheck
```

<span data-ttu-id="04650-173">Spell Check API の[詳細](/azure/cognitive-services/bing-spell-check/proof-text)を確認し、[Spell Check API Python クイックスタート](/azure/cognitive-services/bing-spell-check/quickstarts/python)を開始する。</span><span class="sxs-lookup"><span data-stu-id="04650-173">[Learn more](/azure/cognitive-services/bing-spell-check/proof-text) about the Spell Check API and get started with the [Spell Check API Python quickstart](/azure/cognitive-services/bing-spell-check/quickstarts/python).</span></span>
