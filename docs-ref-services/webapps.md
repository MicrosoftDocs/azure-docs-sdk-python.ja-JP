---
title: Python 用 Azure Web Apps ライブラリ
description: ''
keywords: Azure, Python, SDK, API, Web Apps, App Service
author: lisawong19
ms.author: routlaw
manager: douge
ms.date: 06/12/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: appservice
ms.openlocfilehash: 26b578d9edc7023c06d4c9bfc8c8fb44a169c40d
ms.sourcegitcommit: 46bebbf5dd558750043ce5afadff2ec3714a54e6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2019
ms.locfileid: "67534176"
---
# <a name="azure-web-apps-libraries-for-python"></a>Python 用 Azure Web Apps ライブラリ

## <a name="overview"></a>概要

[Azure App Service](/azure/app-service) を使用して、Web サイト、Web アプリケーション、サービス、REST API のデプロイとスケーリングを行います。

Azure App Service の概要については、「[Azure に Python Web アプリを作成する](/azure/app-service-web/app-service-web-get-started-python)」を参照してください。

## <a name="management-api"></a>管理 API

Azure App Service でホストされている要素のデプロイ、管理、スケーリングを行うには、Management API を使用します。

pip を使ってライブラリをインストールします。

```bash
pip install azure-mgmt-web
```

### <a name="example"></a>例

Web アプリを GitHub リポジトリから Azure Web アプリにデプロイします。

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
> [Management API を探す](/python/api/overview/azure/webapps/management)

## <a name="samples"></a>サンプル

* [Python で Azure Web サイトを管理する][1]
* [ロジック アプリ ワークフローを作成する][2]

Web アプリケーションのサンプルの[完全な一覧](https://azure.microsoft.com/resources/samples/?platform=python&term=web-app)を表示します。

[1]: https://azure.microsoft.com/resources/samples/app-service-web-python-manage
[2]: ../docs-ref-conceptual/python-sdk-azure-samples-logic-app-workflow.md
