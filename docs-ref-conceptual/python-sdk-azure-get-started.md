---
title: "Python 用 Azure ライブラリの概要"
description: "Python 用 Azure ライブラリの概要"
keywords: Azure, Python, SDK, API
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/05/2017
ms.topic: get-started
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.assetid: 
ms.openlocfilehash: 848ca9dc40000e68e5e3cea3af8b8a0d22747881
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/18/2017
---
# <a name="get-started-with-the-azure-libraries-for-python"></a>Python 用 Azure ライブラリの概要

このガイドでは、いくつかの Python 用 Azure ライブラリの使用方法について説明します。  認証の設定、Azure Storage アカウントの作成と使用、Azure SQL Database の作成と使用、仮想マシンのデプロイ、GitHub からの Azure App Service Web アプリのデプロイについて説明します。

## <a name="prerequisites"></a>前提条件

- Azure アカウント。 所有していない場合は、[無料試用版を入手](https://azure.microsoft.com/free/)してください。
- [Python](https://www.python.org/downloads/)
- [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart) または [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2)。

[!INCLUDE [azure-cloud-shell](../docs-ref-conceptual/includes/cloud-shell-try-it.md)]

## <a name="set-up-authentication"></a>認証の設定
> [!IMPORTANT]
> これは、すばやく開始できる開発者エクスペリエンスとして使用することをお勧めします。 運用環境では、[ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-python) か独自の資格情報システムを使用してください。
> CLI 構成に変更を加えると、SDK の実行に影響します。

SDK では、CLI のアクティブなサブスクリプションを使用してクライアントを作成できます。

アクティブな資格情報を定義するには、[az login](https://docs.microsoft.com/cli/azure/authenticate-azure-cli) を使用します。
既定のサブスクリプション ID は、1 つしかない場合はその値であり、そうでない場合は [az account](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli) を使用して定義できます。

```python
from azure.common.client_factory import get_client_from_cli_profile
from azure.mgmt.compute import ComputeManagementClient

client = get_client_from_cli_profile(ComputeManagementClient)
```
## <a name="create-a-virtual-environment"></a>仮想環境の作成

> [!IMPORTANT]
> 仮想環境の作成は省略可能ですが、作成することを強くお勧めします。

Bash (Linux または [Bash for Windows](https://msdn.microsoft.com/commandline/wsl/about)) で仮想環境を作成します。
```bash
pip install virtualenv
virtualenv mytestenv
cd mytestenv
source ./bin/activate
```

PowerShell での仮想環境の作成
```powershell
pip install virtualenv
virtualenv mytestenv
cd mytestenv
.\Scripts\activate
```

> [!IMPORTANT]
> [VSCode](https://code.visualstudio.com/) と [Python 拡張機能](https://marketplace.visualstudio.com/items?itemName=donjayamanne.python)を使用する場合、初めに `code .` を使用して環境を構成できることに注意してください。

## <a name="create-a-linux-virtual-machine"></a>Linux 仮想マシンの作成
このコードによって、米国東部 Azure リージョンで実行されるリソース グループ `sampleVmResourceGroup` に、`testLinuxVM` という名前の新しい Linux VM が作成されます。

認証を行います。
```azcli
az login
```
リソース グループを作成します。
```azurecli-interactive
az group create -l eastus --n sampleVmResourceGroup
```

仮想ネットワークとサブネットを作成します。
```azurecli-interactive
az network vnet create -g sampleVmResourceGroup -n azure-sample-vnet --address-prefix 10.0.0.0/16 --subnet-name azure-sample-subnet --subnet-prefix 10.0.0.0/24
```

パブリック IP アドレスを作成します。
```azurecli-interactive
az network public-ip create -g sampleVmResourceGroup -n azure-sample-ip --allocation-method Dynamic --version IPv6
```
ネットワーク インターフェイス クライアントを作成します。
```azurecli-interactive
az network nic create -g sampleVmResourceGroup --vnet-name azure-sample-vnet --subnet azure-sample-subnet -n azure-sample-nic --public-ip-address azure-sample-ip
```

```python
from azure.mgmt.network import NetworkManagementClient
from azure.mgmt.compute import ComputeManagementClient
from azure.common.client_factory import get_client_from_cli_profile

# Azure Datacenter
LOCATION = 'eastus'

# Resource Group
GROUP_NAME = 'sampleVmResourceGroup'

# Network
VNET_NAME = 'azure-sample-vnet'
SUBNET_NAME = 'azure-sample-subnet'

# VM
NIC_NAME = 'azure-sample-nic'
VM_NAME = 'testLinuxVM'

USERNAME = 'userlogin'
PASSWORD = 'Pa$$w0rd91'

IP_ADDRESS_NAME='azure-sample-ip'

VM_REFERENCE = {
    'linux': {
        'publisher': 'Canonical',
        'offer': 'UbuntuServer',
        'sku': '16.04.0-LTS',
        'version': 'latest'
    },
    'windows': {
        'publisher': 'MicrosoftWindowsServerEssentials',
        'offer': 'WindowsServerEssentials',
        'sku': 'WindowsServerEssentials',
        'version': 'latest'
    }
}


def run_example():

    compute_client = get_client_from_cli_profile(ComputeManagementClient)
    network_client = get_client_from_cli_profile(NetworkManagementClient)

    # get nic id
    nic = network_client.network_interfaces.get(GROUP_NAME, NIC_NAME)

    # Create Linux VM
    print('\nCreating Linux Virtual Machine')
    vm_parameters = create_vm_parameters(nic.id, VM_REFERENCE['linux'])
    print(vm_parameters)
    async_vm_creation = compute_client.virtual_machines.create_or_update(
        GROUP_NAME, VM_NAME, vm_parameters)
    async_vm_creation.wait()


def create_vm_parameters(nic_id, vm_reference):
    """Create the VM parameters structure.
    """
    return {
        'location': LOCATION,
        'os_profile': {
            'computer_name': VM_NAME,
            'admin_username': USERNAME,
            'admin_password': PASSWORD
        },
        'hardware_profile': {
            'vm_size': 'Standard_DS1_v2'
        },
        'storage_profile': {
            'image_reference': {
                'publisher': vm_reference['publisher'],
                'offer': vm_reference['offer'],
                'sku': vm_reference['sku'],
                'version': vm_reference['version']
            },
        },
        'network_profile': {
            'network_interfaces': [{
                'id': nic_id,
            }]
        },
    }


if __name__ == "__main__":
    run_example()
```

プログラムの実行が完了したら、サブスクリプション内の仮想マシンを Azure CLI 2.0 で確認します。

```azurecli-interactive
az vm list --resource-group sampleVmResourceGroup
```

コードが正しく動作したことを確認したら、CLI で VM とそのリソースを削除します。

```azurecli-interactive
az group delete --name sampleVmResourceGroup
```

## <a name="deploy-a-web-app-from-a-github-repo"></a>GitHub リポジトリからの Web アプリのデプロイ
このコードは、無料の価格レベルで稼働する新しい [Azure App Service Web アプリ](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview)に、GitHub リポジトリの `master` ブランチから Flask Web アプリケーションをデプロイするものです。 

開始する前に、https://github.com/Azure-Samples/python-docs-hello-world をフォークします。

認証を行います。
```azcli
az login
```
リソース グループを作成します。
```azurecli-interactive
az group create -l eastus -n sampleWebResourceGroup
```

無料の App Service プランを作成します。
```azurecli-interactive
az appservice plan create -g sampleWebResourceGroup -n sampleFreePlan  --sku Free
```

```python
from azure.mgmt.web import WebSiteManagementClient
from azure.mgmt.web.models import Site, SiteSourceControl, SiteConfig
from azure.common.client_factory import get_client_from_cli_profile

RESOURCE_GROUP_NAME = 'sampleWebResourceGroup'
SERVICE_PLAN_NAME = 'sampleFreePlanName'
WEB_APP_NAME = 'sampleflaskapp123'
REPO_URL = 'GitHub_Repository_URL'

#log in
web_client = get_client_from_cli_profile(WebSiteManagementClient)

# get service plan id
service_plan = web_client.app_service_plans.get(RESOURCE_GROUP_NAME, SERVICE_PLAN_NAME)

# create a web app
siteConfiguration = SiteConfig(
    python_version='3.4',
)
site_async_operation = web_client.web_apps.create_or_update(
    RESOURCE_GROUP_NAME,
    WEB_APP_NAME,
    Site(
        location='eastus',
        server_farm_id=service_plan.id,
        site_config=siteConfiguration
    ),
)

site = site_async_operation.result()
print('created webapp: ' + site.default_host_name)

# continuous deployment with GitHub
source_control_async_operation = web_client.web_apps.create_or_update_source_control(
    RESOURCE_GROUP_NAME,
    WEB_APP_NAME,
    SiteSourceControl(
        location='GitHub',
        repo_url= REPO_URL,
        branch='master'
    )
)

source_control = source_control_async_operation.result()
print("added source control to: " + source_control.name + "azurewebsites.net")
```

ブラウザーを開いてアプリケーションにアクセスします。CLI から次のコマンドを入力してください。
```azurecli-interactive
az appservice web browse --resource-group sampleWebResourceGroup --name YOUR_APP_NAME
```

デプロイを検証したら、Web アプリとプランをサブスクリプションから削除します。 
```azurecli-interactive
az group delete --name sampleWebResourceGroup
```


## <a name="connect-to-a-sql-database"></a>SQL Database への接続
このコードは、リモート アクセスを許可するファイアウォール規則を使用して新しい SQL データベースを作成し、Microsoft ODBC ドライバーを使用してそのデータベースに接続するものです。 pyodbc は、Linux 上で Microsoft ODBC Driver を使用して SQL データベースに接続します。 

認証を行います。
```azcli
az login
```
リソース グループを作成します。
```azurecli-interactive
az group create -l eastus -n azure-sample-group
```

SQL サーバーを作成します。
```azurecli-interactive
az sql server create --admin-password HusH_Sec4et --admin-user mysecretname -l eastus -n samplesqlserver123 -g azure-sample-group
```

ファイアウォール規則を追加します。
```azurecli-interactive
az sql server firewall-rule create --end-ip-address 167.220.0.235 --name firewall_rule_name_123.123.123.123 --resource-group azure-sample-group --server samplesqlserver123 --start-ip-address 123.123.123.123
```

SQL データベースを作成します。
```azurecli-interactive
az sql db create --name sample-db --resource-group azure-sample-group --server samplesqlserver123
```


```python
from azure.mgmt.sql import SqlManagementClient
from azure.common.client_factory import get_client_from_cli_profile
import pyodbc

LOCATION = 'eastus'
GROUP_NAME = 'azure-sample-group'
SERVER_NAME = 'samplesqlserver123'
DATABASE_NAME = 'sample-db'
USER_NAME ='mysecretname'
PASSWORD='HusH_Sec4et'

# authenticate
sql_client = get_client_from_cli_profile(SqlManagementClient)


def run_example():
    # Get SQL database
    print('Get SQL database')
    database = sql_client.databases.get(
        GROUP_NAME,
        SERVER_NAME,
        DATABASE_NAME
    )
    print(database)
    print("\n\n")


def create_and_insert():
    server = SERVER_NAME+'.database.windows.net'
    database = DATABASE_NAME
    username = USER_NAME
    password = PASSWORD
    driver = '{ODBC Driver 13 for SQL Server}'
    cnxn = pyodbc.connect(
        'DRIVER=' + driver + ';PORT=1433;SERVER=' + server + ';PORT=1443;DATABASE=' + database + ';UID=' + username + ';PWD=' + password)
    cursor = cnxn.cursor()
    tsql = "CREATE TABLE CLOUD (name varchar(255), code int);"
    tsqlInsert = "INSERT INTO CLOUD (name, code) VALUES ('Azure', 1); "
    selectValues = "SELECT * FROM CLOUD"
    with cursor.execute(tsql):
        print('Successfully created table!')
    with cursor.execute(tsqlInsert):
        print('Successfully inserted data into table')
    cursor.execute(selectValues)
    row = cursor.fetchone()
    while row:
        print(str(row[0]) + " " + str(row[1]))
        row = cursor.fetchone()
    cnxn.commit()

if __name__ == "__main__":
    run_example()
    create_and_insert()
```

コードが正しく動作することを確認したら、CLI を使用して SQL データベースとそのリソースを削除します。

```azurecli-interactive
az group delete --name azure-sample-group
```

## <a name="write-a-blob-into-a-new-storage-account"></a>新しいストレージ アカウントへの BLOB の書き込み

認証を行います。
```azcli
az login
```
リソース グループを作成します。
```azurecli-interactive
az group create -l eastus -n sampleStorageResourceGroup
```

ストレージ アカウントを作成します。
```azurecli-interactive
az storage account create -n samplestorageaccountname -g sampleStorageResourceGroup -l eastus --sku Standard_RAGRS
```

このコードは、[Azure ストレージ アカウント](https://docs.microsoft.com/azure/storage/storage-introduction)を作成し、Python 用 Azure Storage ライブラリを使用して新しい html ファイルをクラウドに作成するものです。 
```python
from azure.storage import CloudStorageAccount
from azure.storage.blob import PublicAccess
from azure.storage.blob.models import ContentSettings
from azure.common.client_factory import get_client_from_cli_profile
from azure.mgmt.storage import StorageManagementClient

RESOURCE_GROUP = 'sampleStorageResourceGroup'
STORAGE_ACCOUNT_NAME = 'samplestorageaccountname'
CONTAINER_NAME = 'samplecontainername'

# log in
storage_client = get_client_from_cli_profile(StorageManagementClient)

# create a public storage container to hold the file
storage_keys = storage_client.storage_accounts.list_keys(RESOURCE_GROUP, STORAGE_ACCOUNT_NAME)
storage_keys = {v.key_name: v.value for v in storage_keys.keys}

storage_client = CloudStorageAccount(STORAGE_ACCOUNT_NAME, storage_keys['key1'])
blob_service = storage_client.create_block_blob_service()

blob_service.create_container(CONTAINER_NAME, public_access=PublicAccess.Container)

blob_service.create_blob_from_bytes(
    CONTAINER_NAME,
    'helloworld.html',
    b'<center><h1>Hello World!</h1></center>',
    content_settings=ContentSettings('text/html')
)

print(blob_service.make_blob_url(CONTAINER_NAME, 'helloworld.html'))
```
コンテンツが正常にアップロードされたことを確認するには、出力された URL に移動します。 

CLI で次のコマンドを入力して、ストレージ アカウントをクリーンアップします。
```azurecli-interactive
az group delete --name sampleStorageResourceGroup
```

## <a name="explore-more-samples"></a>その他のサンプルを探す

Python 用 Azure 管理ライブラリを使用したリソースの管理とタスクの自動化の詳細については、[仮想マシン](python-sdk-azure-web-apps-samples.md)、[Web アプリ](python-sdk-azure-web-apps-samples.md)、[SQL データベース](python-sdk-azure-sql-database-samples.md)のサンプル コードを参照してください。


## <a name="reference"></a>リファレンス

すべてのパッケージには、[リファレンス](/python/api/overview/azure/?view=azure-python)が提供されています。
