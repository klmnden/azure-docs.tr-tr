---
title: Depolama erişim kimlik bilgilerini üzerinde veri bilimi sanal makine güvenli bir şekilde - Azure | Microsoft Docs
description: Depolama erişim veri bilimi sanal makinede güvenli kimlik bilgileri.
keywords: derin öğrenme, AI, veri bilimi araçları, veri bilimi sanal makine, Jeo-uzamsal analytics, takım veri bilimi işlemi
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.component: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2018
ms.author: gokuma
ms.openlocfilehash: 30bf0de449596bb749e8f57c63ad056b85396a59
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36307783"
---
# <a name="store-access-credentials-on-the-data-science-virtual-machine-securely"></a>Depolama erişim veri bilimi sanal makinede güvenli kimlik bilgileri

Kodunuzda bulut hizmetlerine kimlik doğrulaması için gereken kimlik bilgilerini yönetme bulut uygulamalarını oluşturmanın yaygın bir sorundur. Bu kimlik bilgilerinin güvenlik altında tutulması önemli bir görevdir. İdeal olarak, bunlar hiç Geliştirici istasyonlarında görünür veya kaynak denetimine iade. 

[Hizmet kimliği (MSI) yönetilen](https://docs.microsoft.com/azure/active-directory/managed-service-identity/overview) Azure sağlayarak bu sorunu daha basit çözme yapar hizmetleri otomatik olarak yönetilen bir kimliği Azure Active Directory'de (Azure AD). Bu kimlik, Azure AD kimlik doğrulaması, kodunuzda herhangi bir kimlik bilgisi olmadan destekleyen herhangi bir hizmeti için kimlik doğrulaması kullanabilirsiniz. 

Kimlik bilgileri güvenli bir yoludur MSI ile birlikte kullanmak için [Azure anahtar kasası](https://docs.microsoft.com/azure/key-vault/), bir yönetilen gizli bilgiler ve şifreleme anahtarlarını güvenli bir şekilde depolamak için Azure hizmeti. Yönetilen hizmet kimliği kullanarak bir anahtar kasası erişim ve şifreleme anahtarlarını ve yetkili gizli anahtar Kasası'nı alın. 

MSI ve anahtar kasası belgeleri, bu hizmetler hakkında ayrıntılı bilgi için kapsamlı bir kaynaktır. Bu makalenin geri kalanında MSI ve anahtar kasası temel kullanımı veri bilimi sanal makine (Azure kaynaklarına erişmek için DSVM üzerinde) anlatılmaktadır. 

## <a name="create-a-managed-identity-on-the-dsvm"></a>Yönetilen bir kimliği üzerinde DSVM oluşturma 


```
# Prerequisite: You have already created a Data Science VM in the usual way.

# Create an identity principal for the VM.
az vm assign-identity -g <Resource Group Name> -n <Name of the VM>
# Get the principal ID of the DSVM.
az resource list -n <Name of the VM> --query [*].identity.principalId --out tsv
```


## <a name="assign-key-vault-access-permission-to-a-vm-principal"></a>Bir VM asıl anahtar kasası erişim iznini atayın
```
# Prerequisite: You have already created an empty Key Vault resource on Azure by using the Azure portal or Azure CLI. 

# Assign only get and set permission but not the capability to list the keys.
az keyvault set-policy --object-id <Principal ID of the DSVM from previous step> --name <Key Vault Name> -g <Resource Group of Key Vault>  --secret-permissions get set
```

## <a name="access-a-secret-in-the-key-vault-from-the-dsvm"></a>Bir gizli anahtar kasasında DSVM erişim

```
# Get the access token for the VM.
x=`curl http://localhost:50342/oauth2/token --data "resource=https://vault.azure.net" -H Metadata:true`
token=`echo $x | python -c "import sys, json; print(json.load(sys.stdin)['access_token'])"`

# Access the key vault by using the access token. 
curl https://<Vault Name>.vault.azure.net/secrets/SQLPasswd?api-version=2016-10-01 -H "Authorization: Bearer $token"
```

## <a name="access-storage-keys-from-the-dsvm"></a>DSVM erişim depolama anahtarları

```
# Prerequisite: You have granted your VM's MSI access to use storage account access keys based on instructions from the article at https://docs.microsoft.com/azure/active-directory/managed-service-identity/tutorial-linux-vm-access-storage. This article describes the process in more detail.

y=`curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true`
ytoken=`echo $y | python -c "import sys, json; print(json.load(sys.stdin)['access_token'])"`
curl https://management.azure.com/subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroup of Storage account>/providers/Microsoft.Storage/storageAccounts/<Storage Account Name>/listKeys?api-version=2016-12-01 --request POST -d "" -H "Authorization: Bearer $ytoken"

# Now you can access the data in the storage account from the retrieved storage account keys.
```
## <a name="access-the-key-vault-from-python"></a>Python'dan anahtar kasası erişim

```python
from azure.keyvault import KeyVaultClient
from msrestazure.azure_active_directory import MSIAuthentication

"""MSI Authentication example."""

# Get credentials.
credentials = MSIAuthentication(
    resource='https://vault.azure.net'
)

# Create a Key Vault client.
key_vault_client = KeyVaultClient(
credentials
)

key_vault_uri = "https://<key Vault Name>.vault.azure.net/"

secret = key_vault_client.get_secret(
key_vault_uri,  # Your key vault URL.
"SQLPasswd",       # The name of your secret that already exists in the key vault.
""              # The version of the secret; empty string for latest.
)
print("My secret value is {}".format(secret.value))
```

## <a name="access-the-key-vault-from-azure-cli"></a>Access anahtar Kasası'nı Azure CLI

```
# With a Managed Service Identity set up on the DSVM, users on the DSVM can use Azure CLI to perform the authorized functions. Here are commands to access the key vault from Azure CLI without having to log in to an Azure account. 
# Prerequisites: MSI is already set up on the DSVM as indicated earlier. Specific permission, like accessing storage account keys, reading specific secrets, and writing new secrets, is provided to the MSI. 

# Authenticate to Azure CLI without requiring an Azure account. 
az login --msi

# Retrieve a secret from the key vault. 
az keyvault secret show --vault-name <Vault Name> --name SQLPasswd

# Create a new secret in the key vault.
az keyvault secret set --name MySecret --vault-name <Vault Name> --value "Helloworld"

# List access keys for the storage account.
az storage account keys list -g <Storage Account Resource Group> -n <Storage Account Name>
```
