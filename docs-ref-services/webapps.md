---
title: Python 用 Azure Web Apps ライブラリ
description: ''
keywords: Azure, Python, SDK, API, Web Apps, App Service
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/12/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: appservice
ms.openlocfilehash: 4870394c6ee39cde546d090fa1a0d136609851b3
ms.sourcegitcommit: 434186988284e0a8268a9de11645912a81226d6b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/29/2019
ms.locfileid: "66376691"
---
# <a name="azure-web-apps-libraries-for-python"></a><span data-ttu-id="6f431-103">Python 用 Azure Web Apps ライブラリ</span><span class="sxs-lookup"><span data-stu-id="6f431-103">Azure Web Apps libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="6f431-104">概要</span><span class="sxs-lookup"><span data-stu-id="6f431-104">Overview</span></span>

<span data-ttu-id="6f431-105">[Azure App Service](/azure/app-service) を使用して、Web サイト、Web アプリケーション、サービス、REST API のデプロイとスケーリングを行います。</span><span class="sxs-lookup"><span data-stu-id="6f431-105">Deploy and scale websites, web applications, services, and REST APIs with [Azure App Service](/azure/app-service).</span></span>

<span data-ttu-id="6f431-106">Azure App Service の概要については、「[Azure に Python Web アプリを作成する](/azure/app-service-web/app-service-web-get-started-python)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="6f431-106">To get started with Azure App Service, see [Create a Python web app in Azure](/azure/app-service-web/app-service-web-get-started-python).</span></span>

## <a name="management-api"></a><span data-ttu-id="6f431-107">管理 API</span><span class="sxs-lookup"><span data-stu-id="6f431-107">Management API</span></span>

<span data-ttu-id="6f431-108">Azure App Service でホストされている要素のデプロイ、管理、スケーリングを行うには、Management API を使用します。</span><span class="sxs-lookup"><span data-stu-id="6f431-108">Deploy, manage, and scale elements hosted in the Azure App Service with the management API.</span></span>

<span data-ttu-id="6f431-109">pip を使ってライブラリをインストールします。</span><span class="sxs-lookup"><span data-stu-id="6f431-109">Install the library via pip.</span></span>

```bash
pip install azure-mgmt-web
```

### <a name="example"></a><span data-ttu-id="6f431-110">例</span><span class="sxs-lookup"><span data-stu-id="6f431-110">Example</span></span>

<span data-ttu-id="6f431-111">Web アプリを GitHub リポジトリから Azure Web アプリにデプロイします。</span><span class="sxs-lookup"><span data-stu-id="6f431-111">Deploy a webapp from a GitHub repository into Azure Web App.</span></span>

```python
siteConfiguration = SiteConfig(
    python_version='3.4'
)

# create a web app
web_client.web_apps.create_or_update(
    RESOURCE_GROUP_NAME,
    WEB_APP_NAME,
    Site(
        location='eastus',
        server_farm_id=SERVICE_PLAN_ID,
        site_config=siteConfiguration
    )
)

# continuous deployment with GitHub
source_control_async_operation = web_client.web_apps.create_or_update_source_control(
    RESOURCE_GROUP_NAME,
    WEB_APP_NAME,
    SiteSourceControl(
        location='GitHub',
        repo_url='https://github.com/lisawong19/python-docs-hello-world',
        branch='master'
    )
)
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="6f431-112">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="6f431-112">Explore the Management APIs</span></span>](/python/api/overview/azure/webapps/management)

## <a name="samples"></a><span data-ttu-id="6f431-113">サンプル</span><span class="sxs-lookup"><span data-stu-id="6f431-113">Samples</span></span>

* <span data-ttu-id="6f431-114">[Python で Azure Websites を管理する][1]</span><span class="sxs-lookup"><span data-stu-id="6f431-114">[Manage Azure websites with python][1]</span></span>
* <span data-ttu-id="6f431-115">[ロジック アプリ ワークフローを作成する][2]</span><span class="sxs-lookup"><span data-stu-id="6f431-115">[Create a Logic App workflow][2]</span></span>

<span data-ttu-id="6f431-116">Web アプリケーションのサンプルの[完全な一覧](https://azure.microsoft.com/resources/samples/?platform=python&term=web-app)を表示します。</span><span class="sxs-lookup"><span data-stu-id="6f431-116">View the [complete list](https://azure.microsoft.com/resources/samples/?platform=python&term=web-app) of web application samples.</span></span>

[1]: https://azure.microsoft.com/resources/samples/app-service-web-python-manage
[2]: ../docs-ref-conceptual/python-sdk-azure-samples-logic-app-workflow.md
