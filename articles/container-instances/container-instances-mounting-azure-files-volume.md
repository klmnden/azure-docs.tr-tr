---
title: "Azure kapsayıcı örnekleri Azure dosyaları biriminde bağlama"
description: "Azure kapsayıcı örnekleri durumuyla kalıcı hale getirmek için bir Azure dosyaları birim öğrenin"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/31/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 41c3a449b39d6ef77e1dd0cf10699f8debcad475
ms.sourcegitcommit: 54fd091c82a71fbc663b2220b27bc0b691a39b5b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/12/2017
---
# <a name="mounting-an-azure-file-share-with-azure-container-instances"></a>Azure kapsayıcı örneği ile bir Azure dosya paylaşımı oluşturma

Varsayılan olarak, Azure kapsayıcı durum bilgisiz örnekleridir. Kapsayıcı kilitlenmesine veya durdurur, durumuna kaybolur. Kapsayıcı ömür ötesinde durumunu kalıcı hale getirmek için bir dış depolama alanından bir birim bağlama gerekir. Bu makalede, Azure kapsayıcı örnekleri ile kullanmak için bir Azure dosya paylaşımının gösterilmektedir.

## <a name="create-an-azure-file-share"></a>Bir Azure dosya paylaşımı oluşturma

Azure kapsayıcı örnekleri ile Azure dosya paylaşımının kullanmadan önce oluşturmanız gerekir. Dosya Paylaşımı ve Paylaşım barındırmak için bir depolama hesabı oluşturmak için aşağıdaki betiği çalıştırın. Betik rastgele bir değeri temel dizesi olarak ekler ve böylece depolama hesabı adı genel olarak benzersiz olması gerekir.

```azurecli-interactive
# Change these four parameters
ACI_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM
ACI_PERS_RESOURCE_GROUP=myResourceGroup
ACI_PERS_LOCATION=eastus
ACI_PERS_SHARE_NAME=acishare

# Create the storage account with the parameters
az storage account create -n $ACI_PERS_STORAGE_ACCOUNT_NAME -g $ACI_PERS_RESOURCE_GROUP -l $ACI_PERS_LOCATION --sku Standard_LRS

# Export the connection string as an environment variable, this is used when creating the Azure file share
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string -n $ACI_PERS_STORAGE_ACCOUNT_NAME -g $ACI_PERS_RESOURCE_GROUP -o tsv`

# Create the share
az storage share create -n $ACI_PERS_SHARE_NAME
```

## <a name="acquire-storage-account-access-details"></a>Depolama hesabı erişim ayrıntılarını alma

Azure kapsayıcı durumlarda bir birim olarak Azure dosya paylaşımının bağlamak için üç değerden gerekir: depolama hesabı adı, paylaşım adı ve depolama erişim tuşu.

Yukarıdaki betik kullandıysanız, depolama hesabı adı sonunda rastgele bir değeri ile oluşturuldu. (Rastgele bölümü dahil) son dizede sorgulamak için aşağıdaki komutları kullanın:

```azurecli-interactive
STORAGE_ACCOUNT=$(az storage account list --resource-group $ACI_PERS_RESOURCE_GROUP --query "[?contains(name,'$ACI_PERS_STORAGE_ACCOUNT_NAME')].[name]" -o tsv)
echo $STORAGE_ACCOUNT
```

Paylaşım adı zaten biliniyor (Bu *acishare* yukarıdaki komut), böylece tüm kalır olduğunu aşağıdaki komutu kullanarak bulunabilir depolama hesabı anahtarı:

```azurecli-interactive
STORAGE_KEY=$(az storage account keys list --resource-group $ACI_PERS_RESOURCE_GROUP --account-name $STORAGE_ACCOUNT --query "[0].value" -o tsv)
echo $STORAGE_KEY
```

## <a name="store-storage-account-access-details-with-azure-key-vault"></a>Azure anahtar kasası ile depolama hesabı erişim ayrıntılarını depola

Bir Azure anahtar kasası depolamanızı öneririz için depolama hesabı anahtarları, verilere erişimi koruyun.

Bir anahtar kasası ile Azure CLI oluşturun:

```azurecli-interactive
KEYVAULT_NAME=aci-keyvault
az keyvault create -n $KEYVAULT_NAME --enabled-for-template-deployment -g $ACI_PERS_RESOURCE_GROUP
```

`enabled-for-template-deployment` Anahtarı verir Azure Resource Manager çekme gizli anahtar kasanızı dağıtım sırasında.

Depolama hesabı anahtarı anahtar Kasası'nda yeni bir parola olarak depola:

```azurecli-interactive
KEYVAULT_SECRET_NAME=azurefilesstoragekey
az keyvault secret set --vault-name $KEYVAULT_NAME --name $KEYVAULT_SECRET_NAME --value $STORAGE_KEY
```

## <a name="mount-the-volume"></a>Birim

Bir kapsayıcıda bir birim olarak Azure dosya paylaşımının takma iki adımlı bir işlemdir. İlk olarak, paylaşımı kapsayıcı grubu tanımlayan bir parçası olarak ayrıntılarını sağlayın ve ardından bir veya daha fazla Grup kapsayıcılarında içinde takılı birim nasıl istediğinizi belirtin.

Bağlama için kullanılabilir hale getirmek istediğiniz birimleri tanımlamak için ekleyin bir `volumes` dizi Azure Resource Manager şablonu kapsayıcı Grup tanımında için sonra bunları tek tek kapsayıcıları tanımında başvuru.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageaccountname": {
      "type": "string"
    },
    "storageaccountkey": {
      "type": "securestring"
    }
  },
  "resources":[{
    "name": "hellofiles",
    "type": "Microsoft.ContainerInstance/containerGroups",
    "apiVersion": "2017-08-01-preview",
    "location": "[resourceGroup().location]",
    "properties": {
      "containers": [{
        "name": "hellofiles",
        "properties": {
          "image": "seanmckenna/aci-hellofiles",
          "resources": {
            "requests": {
              "cpu": 1,
              "memoryInGb": 1.5
            }
          },
          "ports": [{
            "port": 80
          }],
          "volumeMounts": [{
            "name": "myvolume",
            "mountPath": "/aci/logs/"
          }]
        }
      }],
      "osType": "Linux",
      "ipAddress": {
        "type": "Public",
        "ports": [{
          "protocol": "tcp",
          "port": "80"
        }]
      },
      "volumes": [{
        "name": "myvolume",
        "azureFile": {
          "shareName": "acishare",
          "storageAccountName": "[parameters('storageaccountname')]",
          "storageAccountKey": "[parameters('storageaccountkey')]"
        }
      }]
    }
  }]
}
```

Şablon ayrı Parametreler dosyasında sağlanan parametre olarak depolama hesabı adı ve anahtar içerir. Parametre dosyasını doldurmak için üç değerden gerekir: depolama hesabı adı, kaynak kimliği, Azure anahtar kasası ve depolama anahtarı depolamak için kullanılan anahtar kasasına gizli anahtar adı. Önceki adımları izlediyseniz, aşağıdaki komut dosyasıyla bu değerleri alabilirsiniz:

```azurecli-interactive
echo $STORAGE_ACCOUNT
echo $KEYVAULT_SECRET_NAME
az keyvault show --name $KEYVAULT_NAME --query [id] -o tsv
```

Değerleri parametreleri dosyaya ekleyin:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageaccountname": {
      "value": "<my_storage_account_name>"
    },
   "storageaccountkey": {
      "reference": {
        "keyVault": {
          "id": "<my_keyvault_id>"
        },
        "secretName": "<my_storage_account_key_secret_name>"
      }
    }
  }
}
```

## <a name="deploy-the-container-and-manage-files"></a>Kapsayıcıyı dağıtmak ve dosyalarını yönetme

Tanımlanan şablonla kapsayıcı oluşturun ve Azure CLI kullanarak, birim. Şablon dosyası adlı varsayılarak *azuredeploy.json* ve parametreler dosyası adlı *azuredeploy.parameters.json*, komut satırı sonra:

```azurecli-interactive
az group deployment create --name hellofilesdeployment --template-file azuredeploy.json --parameters @azuredeploy.parameters.json --resource-group myResourceGroup
```

Kapsayıcı başlatıldığında sonra aracılığıyla dağıtılan basit web uygulaması kullanarak **aci/seanmckenna-hellofiles** belirttiğiniz bağlama yolunda Azure dosya paylaşımında dosyaları yönetmek için resim. Aşağıdakiler aracılığıyla web uygulaması için IP adresi alın:

```azurecli-interactive
az container show --resource-group myResourceGroup --name hellofiles -o table
```

Gibi bir araç kullanabilirsiniz [Microsoft Azure Storage Gezgini](http://storageexplorer.com) almak ve dosya paylaşımı için yazılmış dosyasını inceleyin.

>[!NOTE]
> Azure Resource Manager şablonları kullanma hakkında daha fazla bilgi için bkz: parametre dosyaları ve Azure CLI ile dağıtma [Resource Manager şablonları ve Azure CLI kaynaklarla dağıtmak](../azure-resource-manager/resource-group-template-deploy-cli.md).

## <a name="next-steps"></a>Sonraki adımlar

- Azure kapsayıcı örneği kullanarak, ilk kapsayıcıyı dağıtmak [hızlı başlangıç](container-instances-quickstart.md)
- Hakkında bilgi edinin [Azure kapsayıcı örnekleri ile kapsayıcı orchestrators arasındaki ilişki](container-instances-orchestrator-relationship.md)
