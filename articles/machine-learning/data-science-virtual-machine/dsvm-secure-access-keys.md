---
title: Store erişim kimlik bilgilerini üzerinde veri bilimi sanal makinesi güvenli bir şekilde - Azure | Microsoft Docs
description: Erişim kimlik bilgileri veri bilimi sanal makinesi üzerinde güvenli bir şekilde depolamayı öğrenin. Yönetilen hizmet kimlikleri ve Azure anahtar kasası erişim kimlik bilgilerini depolamak için nasıl kullanılacağını öğreneceksiniz.
keywords: derin öğrenme yapay ZEKA, veri bilimi araçları, veri bilimi sanal makinesi, Jeo-uzamsal analiz, team data science Process'i
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.custom: seodec18
ms.assetid: ''
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2018
ms.author: gokuma
ms.openlocfilehash: 79dba586a5f7102d0012c381593551a951f1b38e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60502359"
---
# <a name="store-access-credentials-on-the-data-science-virtual-machine-securely"></a>Store erişimi veri bilimi sanal makinesi üzerinde güvenli bir şekilde kimlik bilgileri

Kodunuzda bulut Hizmetleri için kimlik doğrulaması için gereken kimlik bilgilerini yönetme bulut uygulamaları geliştirmek, yaygın bir sorundur. Bu kimlik bilgilerinin güvenlik altında tutulması önemli bir görevdir. İdeal olarak, bunlar hiç Geliştirici iş istasyonları üzerinde görünür veya kaynak denetimine iade. 

[Kimlikler Azure kaynakları için yönetilen](https://docs.microsoft.com/azure/active-directory/managed-service-identity/overview) yaptığı Azure vererek bu sorunu daha basit çözme hizmetleri, Azure Active Directory'de (Azure AD) otomatik olarak yönetilen bir kimlik. Bu kimlik, Azure AD kimlik doğrulaması, kodunuzdaki herhangi bir kimlik bilgisi olmadan destekleyen herhangi bir hizmeti kimlik doğrulaması için kullanabilirsiniz. 

MSI ile birlikte kullanmak üzere güvenli kimlik bilgileri yollarından biri açıklanmıştır [Azure anahtar kasası](https://docs.microsoft.com/azure/key-vault/), yönetilen gizli dizileri ve şifreleme anahtarları güvenli bir şekilde depolamak için Azure hizmeti. Yönetilen kimlik kullanarak bir anahtar kasasına erişmek ve şifreleme anahtarlarını ve yetkili gizli dizileri anahtar kasasından almak. 

Azure kaynaklarını ve Key Vault belgeleri için yönetilen kimlikleri bu hizmetler hakkında ayrıntılı bilgi için kapsamlı bir kaynak olur. Bu makalenin geri kalanında, veri bilimi sanal makinesi (Azure kaynaklarına erişmek için DSVM üzerinde) temel kullanımını MSI ve Key Vault size yol gösterir. 

## <a name="create-a-managed-identity-on-the-dsvm"></a>DSVM'nin bir yönetilen kimlik oluşturma 


```
# Prerequisite: You have already created a Data Science VM in the usual way.

# Create an identity principal for the VM.
az vm assign-identity -g <Resource Group Name> -n <Name of the VM>
# Get the principal ID of the DSVM.
az resource list -n <Name of the VM> --query [*].identity.principalId --out tsv
```


## <a name="assign-key-vault-access-permission-to-a-vm-principal"></a>Bir VM sorumlusu için Key Vault erişim izni atama
```
# Prerequisite: You have already created an empty Key Vault resource on Azure by using the Azure portal or Azure CLI. 

# Assign only get and set permission but not the capability to list the keys.
az keyvault set-policy --object-id <Principal ID of the DSVM from previous step> --name <Key Vault Name> -g <Resource Group of Key Vault>  --secret-permissions get set
```

## <a name="access-a-secret-in-the-key-vault-from-the-dsvm"></a>Anahtar kasasındaki gizli dizi DSVM erişim

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
## <a name="access-the-key-vault-from-python"></a>Python'dan anahtar kasasına erişim

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

## <a name="access-the-key-vault-from-azure-cli"></a>Azure CLI üzerinden anahtar kasasına erişim

```
# With managed identities for Azure resources set up on the DSVM, users on the DSVM can use Azure CLI to perform the authorized functions. Here are commands to access the key vault from Azure CLI without having to log in to an Azure account. 
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
