---
title: "SAS belirteci ve PowerShell ile Azure şablonu dağıtma | Microsoft Docs"
description: "SAS belirteci ile korunan bir şablondan kaynakları Azure'a dağıtmak için Azure Resource Manager ve Azure PowerShell kullanın."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/19/2017
ms.author: tomfitz
ms.openlocfilehash: 1e3cea027b599e2b1af1ced0fdf14e2cc8a0db82
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-private-resource-manager-template-with-sas-token-and-azure-powershell"></a>SAS belirteci ve Azure PowerShell ile özel Resource Manager şablonu dağıtma

Şablonunuzu bir depolama hesabında bulunduğunda, şablona erişimi kısıtlayabilir ve dağıtım sırasında bir paylaşılan erişim imzası (SAS) belirteci sağlayın. Bu konu Azure PowerShell'i Resource Manager şablonları ile dağıtımı sırasında bir SAS belirteci sağlamak için nasıl kullanılacağını açıklar. 

## <a name="add-private-template-to-storage-account"></a>Özel şablon için depolama hesabı ekleme

Bir depolama hesabı ve bunları bir SAS belirteci ile dağıtım sırasında bağlantı şablonlarınızı ekleyebilirsiniz.

> [!IMPORTANT]
> Aşağıdaki adımları izleyerek, şablonu içeren blob yalnızca hesap sahibi erişilebilir. Blob için bir SAS belirteci oluşturduğunuzda, ancak blob bu URI ile bir kişiye erişilebilir. Başka bir kullanıcı URI kesişirse kullanıcı şablonu erişebilir. Bir SAS belirteci kullanarak şablonlarınızı erişimi sınırlayan, iyi bir yoludur ancak parolalar gibi hassas verileri doğrudan şablonunda içermemelidir.
> 
> 

Aşağıdaki örnek, bir özel depolama hesabı kapsayıcısının ayarlar ve bir şablon yükler:
   
```powershell
# create a storage account for templates
New-AzureRmResourceGroup -Name ManageGroup -Location "South Central US"
New-AzureRmStorageAccount -ResourceGroupName ManageGroup -Name {your-unique-name} -Type Standard_LRS -Location "West US"
Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name {your-unique-name}

# create a container and upload template
New-AzureStorageContainer -Name templates -Permission Off
Set-AzureStorageBlobContent -Container templates -File c:\MyTemplates\storage.json
```

## <a name="provide-sas-token-during-deployment"></a>Dağıtım sırasında SAS belirteci sağlayın
Özel bir şablon bir depolama hesabında dağıtmak için bir SAS belirteci oluştur ve şablon için URI dahil edin. Dağıtım tamamlamak için yeterli zamana olanak sona erme saati ayarlayın.
   
```powershell
Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name {your-unique-name}

# get the URI with the SAS token
$templateuri = New-AzureStorageBlobSASToken -Container templates -Blob storage.json -Permission r `
  -ExpiryTime (Get-Date).AddHours(2.0) -FullUri

# provide URI with SAS token during deployment
New-AzureRmResourceGroup -Name ExampleGroup -Location "South Central US"
New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateUri $templateuri
```

Bir SAS belirteci ile bağlı şablonları kullanma örneği için bkz: [Azure Resource Manager ile bağlı şablonları kullanma](resource-group-linked-templates.md).


## <a name="next-steps"></a>Sonraki adımlar
* Şablon dağıtımı için giriş için bkz [Resource Manager şablonları ve Azure PowerShell ile kaynakları dağıtmak](resource-group-template-deploy.md).
* Bir şablon dağıtan bir tam örnek betik için bkz: [dağıtmak Resource Manager şablonu komut dosyası](resource-manager-samples-powershell-deploy.md)
* Şablonda parametreleri tanımlamak için bkz: [şablonları yazma](resource-group-authoring-templates.md#parameters).
* Kuruluşların abonelikleri etkili bir şekilde yönetmek için Resource Manager'ı nasıl kullanabileceği hakkında yönergeler için bkz. [Azure kurumsal iskelesi: öngörücü abonelik idaresi](resource-manager-subscription-governance.md).

