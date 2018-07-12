---
title: Python 用 Azure Key Vault ライブラリ
description: Python 用 Azure Key Vault クライアント ライブラリのリファレンス ドキュメント
author: lisawong19
keywords: Azure, Python, SDK, API, キー, Key Vault, 認証, シークレット, キー, セキュリティ
manager: douge
ms.author: liwong
ms.date: 07/18/2017
ms.topic: article
ms.devlang: python
ms.service: keyvault
ms.openlocfilehash: 555f55dcf7355a1a82dc3ca5826e0d0bd3fad414
ms.sourcegitcommit: 8476146ae9bcd1533db47adbe2524b27b93aaba0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/09/2018
ms.locfileid: "37925943"
---
# <a name="azure-key-vault-libraries-for-python"></a>Python 用 Azure Key Vault ライブラリ

## <a name="overview"></a>概要

Azure Key Vault に格納されるキーとシークレットの作成、更新、削除は、クライアント ライブラリを使って行います。

キー コンテナーの作成、アプリケーションの承認、アクセス許可の管理は、Azure Key Vault 管理ライブラリを使って行います。 

[Azure Key Vault](/azure/key-vault/key-vault-whatis) の詳細はこちらです。

## <a name="install-the-libraries"></a>ライブラリをインストールする

### <a name="client-library"></a>クライアント ライブラリ

```bash
pip install azure-keyvault
```

## <a name="examples"></a>例

キー コンテナーから [JSON Web キー](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-18)を取得します。

```python
from azure.keyvault import KeyVaultClient, KeyVaultAuthentication
from azure.common.credentials import ServicePrincipalCredentials

credentials = None

def auth_callback(server, resource, scope):
    credentials = ServicePrincipalCredentials(
        client_id = '', #client id
        secret = '',
        tenant = '',
        resource = "https://vault.azure.net"
    )
    token = credentials.token
    return token['token_type'], token['access_token']

client = KeyVaultClient(KeyVaultAuthentication(auth_callback))

key_bundle = client.get_key(vault_url, key_name, key_version)
json_key = key_bundle.key
```

同様に、次のスニペットを使用して、コンテナーからシークレットを取得できます。

```
from azure.keyvault import KeyVaultClient, KeyVaultAuthentication
from azure.common.credentials import ServicePrincipalCredentials

credentials = None

def auth_callback(server, resource, scope):
    credentials = ServicePrincipalCredentials(
        client_id = '',
        secret = '',
        tenant = '',
        resource = "https://vault.azure.net"
    )
    token = credentials.token
    return token['token_type'], token['access_token']

client = KeyVaultClient(KeyVaultAuthentication(auth_callback))

secret_bundle = client.get_secret("https://VAULT_ID.vault.azure.net/", "SECRET_ID", "SECRET_VERSION")

print(secret_bundle.value)
```

> [!div class="nextstepaction"]
> [クライアント API を探す](/python/api/overview/azure/keyvault/client)

### <a name="management-api"></a>管理 API

```bash
pip install azure-mgmt-keyvault
```

### <a name="example"></a>例
次の例は、Azure Key Vault を作成する方法を示します。 

```python
from azure.mgmt.keyvault import KeyVaultManagementClient

GROUP_NAME = 'your_resource_group_name'
KV_NAME = 'your_key_vault_name'
#The object ID of the User or Application for access policies. Find this number in the portal
OBJECT_ID = '00000000-0000-0000-0000-000000000000'

kv_client = KeyVaultManagementClient(credentials, subscription_id)

vault = kv_client.vaults.create_or_update(
    GROUP_NAME,
    KV_NAME,
    {
        'location': 'eastus',
        'properties': {
            'sku': {
                'name': 'standard'
            },
            'tenant_id': os.environ['AZURE_TENANT_ID'],
            'access_policies': [{
                'tenant_id': os.environ['AZURE_TENANT_ID'],
                'object_id': OBJECT_ID,
                'permissions': {
                    'keys': ['all'],
                    'secrets': ['all']
                }
            }]
        }
    }
)
```
> [!div class="nextstepaction"]
> [クライアント API を探す](/python/api/overview/azure/keyvault/client)

> [!div class="nextstepaction"]
> [Management API を探す](/python/api/overview/azure/keyvault/management)

## <a name="samples"></a>サンプル
* [キー コンテナーの管理][1] 
* [キー コンテナーの回復][2]

[1]: https://azure.microsoft.com/resources/samples/key-vault-python-manage/
[2]: https://azure.microsoft.com/resources/samples/key-vault-recovery-python/

Azure Key Vault のサンプルの[完全な一覧](https://azure.microsoft.com/resources/samples/?platform=python&term=key+vault)を表示します。 
