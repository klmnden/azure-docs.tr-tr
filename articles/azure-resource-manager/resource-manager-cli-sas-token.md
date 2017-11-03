---
title: "SAS belirteci ve Azure CLI ile Azure şablonu dağıtma | Microsoft Docs"
description: "SAS belirteci ile korunan bir şablondan kaynakları Azure'a dağıtmak için Azure Resource Manager ve Azure CLI kullanın."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/31/2017
ms.author: tomfitz
ms.openlocfilehash: 22387aadd8f53a65efb76a29a9403c46a2c25954
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-private-resource-manager-template-with-sas-token-and-azure-cli"></a>SAS belirteci ve Azure CLI ile özel Resource Manager şablonu dağıtma

Şablonunuzu bir depolama hesabında bulunduğunda, şablona erişimi kısıtlayabilir ve dağıtım sırasında bir paylaşılan erişim imzası (SAS) belirteci sağlayın. Bu konu Azure PowerShell'i Resource Manager şablonları ile dağıtımı sırasında bir SAS belirteci sağlamak için nasıl kullanılacağını açıklar. 

## <a name="add-private-template-to-storage-account"></a>Özel şablon için depolama hesabı ekleme

Bir depolama hesabı ve bunları bir SAS belirteci ile dağıtım sırasında bağlantı şablonlarınızı ekleyebilirsiniz.

> [!IMPORTANT]
> Aşağıdaki adımları izleyerek, şablonu içeren blob yalnızca hesap sahibi erişilebilir. Blob için bir SAS belirteci oluşturduğunuzda, ancak blob bu URI ile bir kişiye erişilebilir. Başka bir kullanıcı URI kesişirse kullanıcı şablonu erişebilir. Bir SAS belirteci kullanarak şablonlarınızı erişimi sınırlayan, iyi bir yoludur ancak parolalar gibi hassas verileri doğrudan şablonunda içermemelidir.
> 
> 

Aşağıdaki örnek, bir özel depolama hesabı kapsayıcısının ayarlar ve bir şablon yükler:
   
```azurecli
az group create --name "ManageGroup" --location "South Central US"
az storage account create \
    --resource-group ManageGroup \
    --location "South Central US" \
    --sku Standard_LRS \
    --kind Storage \
    --name {your-unique-name}
connection=$(az storage account show-connection-string \
    --resource-group ManageGroup \
    --name {your-unique-name} \
    --query connectionString)
az storage container create \
    --name templates \
    --public-access Off \
    --connection-string $connection
az storage blob upload \
    --container-name templates \
    --file vmlinux.json \
    --name vmlinux.json \
    --connection-string $connection
```

### <a name="provide-sas-token-during-deployment"></a>Dağıtım sırasında SAS belirteci sağlayın
Özel bir şablon bir depolama hesabında dağıtmak için bir SAS belirteci oluştur ve şablon için URI dahil edin. Dağıtım tamamlamak için yeterli zamana olanak sona erme saati ayarlayın.
   
```azurecli
expiretime=$(date -u -d '30 minutes' +%Y-%m-%dT%H:%MZ)
connection=$(az storage account show-connection-string \
    --resource-group ManageGroup \
    --name {your-unique-name} \
    --query connectionString)
token=$(az storage blob generate-sas \
    --container-name templates \
    --name vmlinux.json \
    --expiry $expiretime \
    --permissions r \
    --output tsv \
    --connection-string $connection)
url=$(az storage blob url \
    --container-name templates \
    --name vmlinux.json \
    --output tsv \
    --connection-string $connection)
az group deployment create --resource-group ExampleGroup --template-uri $url?$token
```

Bir SAS belirteci ile bağlı şablonları kullanma örneği için bkz: [Azure Resource Manager ile bağlı şablonları kullanma](resource-group-linked-templates.md).

## <a name="next-steps"></a>Sonraki adımlar
* Şablon dağıtımı için giriş için bkz [Resource Manager şablonları ve Azure PowerShell ile kaynakları dağıtmak](resource-group-template-deploy-cli.md).
* Bir şablon dağıtan bir tam örnek betik için bkz: [dağıtmak Resource Manager şablonu komut dosyası](resource-manager-samples-cli-deploy.md)
* Şablonda parametreleri tanımlamak için bkz: [şablonları yazma](resource-group-authoring-templates.md#parameters).
* Kuruluşların abonelikleri etkili bir şekilde yönetmek için Resource Manager'ı nasıl kullanabileceği hakkında yönergeler için bkz. [Azure kurumsal iskelesi: öngörücü abonelik idaresi](resource-manager-subscription-governance.md).
