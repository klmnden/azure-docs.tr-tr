---
title: Azure şablonu SAS belirteci ve Azure CLI ile dağıtma | Microsoft Docs
description: SAS belirteci ile korumalı bir şablondan, kaynakları Azure'a dağıtmak için Azure Resource Manager ve Azure CLI kullanın.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: ''
ms.service: azure-resource-manager
ms.devlang: azurecli
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/31/2017
ms.author: tomfitz
ms.openlocfilehash: c869b76a0d1ba10bc27aefa60cbe4ed5b8d8201a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61061371"
---
# <a name="deploy-private-resource-manager-template-with-sas-token-and-azure-cli"></a>SAS belirteci ve Azure CLI ile özel Resource Manager şablonu dağıtma

Şablonunuzu bir depolama hesabında bulunduğunda, şablona erişimini kısıtlamak ve dağıtım sırasında bir paylaşılan erişim imzası (SAS) belirteci sağlayın. Bu konu, Azure PowerShell Resource Manager şablonları ile dağıtım sırasında bir SAS belirteci sağlamak için nasıl kullanılacağını açıklar. 

## <a name="add-private-template-to-storage-account"></a>Özel şablon için depolama hesabı ekleme

Bir depolama hesabı ve bir SAS belirteci ile dağıtımı sırasında bunları bağlantısını şablonlarınızı ekleyebilirsiniz.

> [!IMPORTANT]
> Aşağıdaki adımları izleyerek şablonu içeren blob yalnızca hesap sahibi erişimine açıktır. Ancak, blob için SAS belirteci oluşturduğunuzda, blob bu URI'ye sahip herkes tarafından erişilebilir olur. URI başka bir kullanıcı durdurur, kullanıcının şablon erişebilir. Bir SAS belirteci kullanarak şablonlarınızı erişimi sınırlayan, iyi bir yoludur, ancak parola gibi hassas verileri doğrudan şablonda içermemelidir.
> 
> 

Aşağıdaki örnek, bir özel depolama hesabı kapsayıcısı ayarlamak ayarlar ve bir şablonu yükler:
   
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
Bir depolama hesabındaki özel bir şablonu dağıtmak için bir SAS belirteci oluşturur ve URI şablonu için dahil edin. Dağıtımı tamamlamak için yeterli zamana izin vermek için sona erme tarihini ayarlayın.
   
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
* Şablon dağıtımı için bir giriş için bkz [kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](resource-group-template-deploy-cli.md).
* Bir şablon dağıtan bir tam örnek betik için bkz. [dağıtma Resource Manager şablon betiği](resource-manager-samples-cli-deploy.md)
* Şablonda parametreleri tanımlamak için bkz: [şablonları yazma](resource-group-authoring-templates.md#parameters).
