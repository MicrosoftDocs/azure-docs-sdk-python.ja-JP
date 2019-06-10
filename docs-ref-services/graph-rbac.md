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
ms.openlocfilehash: 27238e00463ae30ec0e47e8c18497ffb9edac62c
ms.sourcegitcommit: 253c8d4b3dbc2bb76d1a273a757ab96ba37617a1
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/16/2019
ms.locfileid: "65731538"
---
# <a name="azure-active-directory-graph-libraries-for-python"></a>Python 用 Azure Active Directory Graph ライブラリ

> [!IMPORTANT]
>
> 2019 年 2 月の時点で、Azure Active Directory Graph API のいくつかの初期バージョンを非推奨にするプロセスが開始され、Microsoft Graph API が代わりに使用されています。 
>
> 詳細、更新、およびタイム フレームについては、Office デベロッパー センターの「[Microsoft Graph or the Azure AD Graph (Microsoft Graph か Azure AD Graph か)](https://dev.office.com/blogs/microsoft-graph-or-azure-ad-graph)」を参照してください。
>
> 今後、アプリケーションでは Microsoft Graph API を使用する必要があります。 

## <a name="overview"></a>概要 

[Active Directory Graph](/azure/active-directory/develop/active-directory-graph-apis) でユーザーのサインオンを行い、アプリケーションと API へのアクセスを制御します。   

## <a name="client-library"></a>クライアント ライブラリ   

 ```bash    
pip install azure-graphrbac 
``` 

### <a name="example"></a>例 
> [!NOTE]   
> 資格情報インスタンスの作成時に、リソース パラメーターを https://graph.windows.net に変更する必要があります。    
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
次のコードは、ユーザーを作成し、直接取得したり一覧のフィルター処理で取得したりしてから、それを削除します。   
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