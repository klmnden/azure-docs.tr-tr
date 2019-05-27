---
title: Azure şablonu SAS belirteci ve PowerShell ile dağıtma | Microsoft Docs
description: SAS belirteci ile korumalı bir şablondan, kaynakları Azure'a dağıtmak için Azure Resource Manager ve Azure PowerShell kullanın.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: ''
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/19/2017
ms.author: tomfitz
ms.openlocfilehash: 818aa63ced56d7cf382536f10bb6150199ab74e9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66128425"
---
# <a name="deploy-private-resource-manager-template-with-sas-token-and-azure-powershell"></a>SAS belirteci ve Azure PowerShell ile özel Resource Manager şablonu dağıtma

Şablonunuzu bir depolama hesabında bulunduğunda, şablona erişimini kısıtlamak ve dağıtım sırasında bir paylaşılan erişim imzası (SAS) belirteci sağlayın. Bu konu, Azure PowerShell Resource Manager şablonları ile dağıtım sırasında bir SAS belirteci sağlamak için nasıl kullanılacağını açıklar. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="add-private-template-to-storage-account"></a>Özel şablon için depolama hesabı ekleme

Bir depolama hesabı ve bir SAS belirteci ile dağıtımı sırasında bunları bağlantısını şablonlarınızı ekleyebilirsiniz.

> [!IMPORTANT]
> Aşağıdaki adımları izleyerek şablonu içeren blob yalnızca hesap sahibi erişimine açıktır. Ancak, blob için SAS belirteci oluşturduğunuzda, blob bu URI'ye sahip herkes tarafından erişilebilir olur. URI başka bir kullanıcı durdurur, kullanıcının şablon erişebilir. Bir SAS belirteci kullanarak şablonlarınızı erişimi sınırlayan, iyi bir yoludur, ancak parola gibi hassas verileri doğrudan şablonda içermemelidir.
> 
> 

Aşağıdaki örnek, bir özel depolama hesabı kapsayıcısı ayarlamak ayarlar ve bir şablonu yükler:
   
```powershell
# create a storage account for templates
New-AzResourceGroup -Name ManageGroup -Location "South Central US"
New-AzStorageAccount -ResourceGroupName ManageGroup -Name {your-unique-name} -Type Standard_LRS -Location "West US"
Set-AzCurrentStorageAccount -ResourceGroupName ManageGroup -Name {your-unique-name}

# create a container and upload template
New-AzStorageContainer -Name templates -Permission Off
Set-AzStorageBlobContent -Container templates -File c:\MyTemplates\storage.json
```

## <a name="provide-sas-token-during-deployment"></a>Dağıtım sırasında SAS belirteci sağlayın
Bir depolama hesabındaki özel bir şablonu dağıtmak için bir SAS belirteci oluşturur ve URI şablonu için dahil edin. Dağıtımı tamamlamak için yeterli zamana izin vermek için sona erme tarihini ayarlayın.
   
```powershell
Set-AzCurrentStorageAccount -ResourceGroupName ManageGroup -Name {your-unique-name}

# get the URI with the SAS token
$templateuri = New-AzStorageBlobSASToken -Container templates -Blob storage.json -Permission r `
  -ExpiryTime (Get-Date).AddHours(2.0) -FullUri

# provide URI with SAS token during deployment
New-AzResourceGroup -Name ExampleGroup -Location "South Central US"
New-AzResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateUri $templateuri
```

Bir SAS belirteci ile bağlı şablonları kullanma örneği için bkz: [Azure Resource Manager ile bağlı şablonları kullanma](resource-group-linked-templates.md).


## <a name="next-steps"></a>Sonraki adımlar
* Şablon dağıtımı için bir giriş için bkz [kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](resource-group-template-deploy.md).
* Bir şablon dağıtan bir tam örnek betik için bkz. [dağıtma Resource Manager şablon betiği](resource-manager-samples-powershell-deploy.md)
* Şablonda parametreleri tanımlamak için bkz: [şablonları yazma](resource-group-authoring-templates.md#parameters).
