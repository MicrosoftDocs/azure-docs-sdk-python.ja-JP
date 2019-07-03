---
title: Python 用 Graph RBAC ライブラリ
description: Python 用 Graph RBAC ライブラリのリファレンス
keywords: Azure, python, SDK, API, Graph RBAC
author: rloutlaw
ms.author: routlaw
manager: jfriend
ms.date: 05/10/2019
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: e9b0aba7998565284ae18e0036da96d033b2b05f
ms.sourcegitcommit: 46bebbf5dd558750043ce5afadff2ec3714a54e6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2019
ms.locfileid: "67534274"
---
# <a name="azure-active-directory-graph-libraries-for-python"></a><span data-ttu-id="d4063-104">Python 用 Azure Active Directory Graph ライブラリ</span><span class="sxs-lookup"><span data-stu-id="d4063-104">Azure Active Directory Graph libraries for Python</span></span>

> [!IMPORTANT]
>
> <span data-ttu-id="d4063-105">2019 年 2 月の時点で、Azure Active Directory Graph API のいくつかの初期バージョンを非推奨にするプロセスが開始され、Microsoft Graph API が代わりに使用されています。</span><span class="sxs-lookup"><span data-stu-id="d4063-105">As of February 2019, we started the process to deprecate some earlier versions of Azure Active Directory Graph API in favor of the Microsoft Graph API.</span></span> 
>
> <span data-ttu-id="d4063-106">詳細、更新、およびタイム フレームについては、Office デベロッパー センターの「[Microsoft Graph or the Azure AD Graph (Microsoft Graph か Azure AD Graph か)](https://dev.office.com/blogs/microsoft-graph-or-azure-ad-graph)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="d4063-106">For details, updates, and time frames, see [Microsoft Graph or the Azure AD Graph](https://dev.office.com/blogs/microsoft-graph-or-azure-ad-graph) in the Office Dev Center.</span></span>
>
> <span data-ttu-id="d4063-107">今後、アプリケーションでは Microsoft Graph API を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d4063-107">Moving forward, applications should use the Microsoft Graph API.</span></span> 

## <a name="overview"></a><span data-ttu-id="d4063-108">概要</span><span class="sxs-lookup"><span data-stu-id="d4063-108">Overview</span></span> 

<span data-ttu-id="d4063-109">[Active Directory Graph](/azure/active-directory/develop/active-directory-graph-api) でユーザーのサインオンを行い、アプリケーションと API へのアクセスを制御します。</span><span class="sxs-lookup"><span data-stu-id="d4063-109">Sign-on users and control access to applications and APIs with [Active Directory Graph](/azure/active-directory/develop/active-directory-graph-api).</span></span>    

## <a name="client-library"></a><span data-ttu-id="d4063-110">クライアント ライブラリ</span><span class="sxs-lookup"><span data-stu-id="d4063-110">Client library</span></span>   

 ```bash    
pip install azure-graphrbac 
``` 

### <a name="example"></a><span data-ttu-id="d4063-111">例</span><span class="sxs-lookup"><span data-stu-id="d4063-111">Example</span></span> 
> [!NOTE]   
> <span data-ttu-id="d4063-112">資格情報インスタンスの作成時に、リソース パラメーターを https://graph.windows.net に変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d4063-112">You need to change the resource parameter to https://graph.windows.net while creating the credentials instance</span></span>    
 ```python  
from azure.graphrbac import GraphRbacManagementClient   
from azure.common.credentials import UserPassCredentials    
 credentials = UserPassCredentials( 
            'user@domain.com',      # Your user 
            'my_password',          # Your password 
            resource="https://graph.windows.net"    
    )   
 tenant_id = "myad.onmicrosoft.com" 
 graphrbac_client = GraphRbacManagementClient(  
    credentials,    
    tenant_id   
)   
``` 
<span data-ttu-id="d4063-113">次のコードは、ユーザーを作成し、直接取得したり一覧のフィルター処理で取得したりしてから、それを削除します。</span><span class="sxs-lookup"><span data-stu-id="d4063-113">The following code creates a user, get it directly and by list filtering, and then delete it.</span></span>   
```python   
from azure.graphrbac.models import UserCreateParameters, PasswordProfile    
 user = graphrbac_client.users.create(  
    UserCreateParameters(   
        user_principal_name="testbuddy@{}".format(MY_AD_DOMAIN),    
        account_enabled=False,  
        display_name='Test Buddy',  
        mail_nickname='testbuddy',  
        password_profile=PasswordProfile(   
            password='MyStr0ngP4ssword',    
            force_change_password_next_login=True   
        )   
    )   
)   
# user is a User instance   
self.assertEqual(user.display_name, 'Test Buddy')   
 user = graphrbac_client.users.get(user.object_id)  
self.assertEqual(user.display_name, 'Test Buddy')   
 for user in graphrbac_client.users.list(filter="displayName eq 'Test Buddy'"): 
    self.assertEqual(user.display_name, 'Test Buddy')   
 graphrbac_client.users.delete(user.object_id)  
```