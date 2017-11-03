---
title: "Şablonları Azure yığını için geliştirme | Microsoft Docs"
description: "Azure yığın şablon en iyi yöntemleri öğrenin"
services: azure-stack
documentationcenter: 
author: HeathL17
manager: byronr
editor: 
ms.assetid: 8a5bc713-6f51-49c8-aeed-6ced0145e07b
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: helaw
ms.openlocfilehash: ffad7bfd4ffcd9159dea23b70640f0ee761fbae0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-resource-manager-template-considerations"></a>Azure Resource Manager şablonu konuları

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Uygulamanızı geliştirirken şablon taşınabilirlik Azure ve Azure yığın arasında olmak önemlidir.  Bu konu, Azure Resource Manager geliştirme için Değerlendirmeler sağlar [şablonları](http://download.microsoft.com/download/E/A/4/EA4017B5-F2ED-449A-897E-BD92E42479CE/Getting_Started_With_Azure_Resource_Manager_white_paper_EN_US.pdf), uygulama ve test dağıtımınızda Azure Azure yığın ortam erişim olmadan prototip olabilir.

## <a name="public-namespaces"></a>Ortak ad alanları
Azure yığını, veri merkezinizde barındırıldığından, farklı hizmet uç noktası ad alanlarını Azure genel bulutunda vardır. Sonuç olarak, Azure yığınına dağıtmak çalıştığınızda sabit kodlanmış ortak uç noktaları Resource Manager şablonlarındaki başarısız. Bunun yerine, kullanabileceğiniz *başvuru* ve *birleştirme* işlevi dinamik olarak değerlerine göre hizmet uç noktası oluşturmak için dağıtım sırasında kaynak Sağlayıcısı'ndan alınan. Örneğin, belirtme yerine *blob.core.windows.net* şablonunuzda, almak [primaryEndpoints.blob](https://github.com/Azure/AzureStack-QuickStart-Templates/blob/master/101-simple-windows-vm/azuredeploy.json#L201) dinamik olarak ayarlamak için *osDisk.URI* uç noktası:

     "osDisk": {"name": "osdisk","vhd": {"uri": 
     "[concat(reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2015-06-15').primaryEndpoints.blob, variables('vmStorageAccountContainerName'),
      '/',variables('OSDiskName'),'.vhd')]"}}

## <a name="api-versioning"></a>API sürümü oluşturma
Azure hizmet sürümleri, Azure ve Azure yığın arasında farklılık gösterebilir. Her kaynak sunulan yetenekleri tanımlayan apiVersion özniteliği gerektirir. Azure yığınında API sürümü uyumluluğundan emin olmak için her kaynak sağlayıcısı için geçerli API sürümleri şunlardır:

| Kaynak sağlayıcısı | apiVersion |
| --- | --- |
| İşlem |`'2015-06-15'` |
| Ağ |`'2015-06-15'`, `'2015-05-01-preview'` |
| Depolama |`'2016-01-01'`, `'2015-06-15'`, `'2015-05-01-preview'` |
| KeyVault | `'2015-06-01'` |
| App Service |`'2015-08-01'` |
| MySQL |`'2015-09-01'` |
| SQL |`'2014-04-01-preview'` |

## <a name="template-functions"></a>Şablon işlevleri
Resource Manager [işlevleri](../../azure-resource-manager/resource-group-template-functions.md) dinamik şablonları oluşturmak için gerekli özellikleri sağlar. Örnek olarak, gibi görevler için işlevleri kullanabilirsiniz:

* Birleştirme veya dizeleri kırpma 
* Diğer kaynaklardan başvuru değeri
* Birden çok örneği dağıtma kaynakları yineleme 

Şablonlarınızı oluşturmak gibi bazı işlevleri Azure yığın geliştirme Seti'nde kullanılabilir değil ve kullanılmamalıdır. Bu işlevler şunlardır:

* Atla
* Al

## <a name="resource-location"></a>Kaynak konumu
Resource Manager şablonları konum özniteliği dağıtım sırasında kaynaklara yerleştirmek için kullanın. Azure'da, Batı ABD veya Güney Amerika gibi bir bölgesi Konumlar bakın. Azure yığınında konumları Azure yığın, veri merkezinizde olduğundan farklıdır.  Şablonları Azure ve Azure yığın arasında aktarılabilir olduğundan emin olmak için kaynakların dağıtırken kaynak grubu konumu başvuruda bulunmalıdır. Kullanarak bunu yapabilirsiniz `[resourceGroup().Location]` tüm kaynakları kaynak grubu konumu devral emin olmak için.  Aşağıdaki Resource Manager şablonu alıntı bir depolama hesabını dağıtırken bu işlevini kullanarak, bir örnek verilmiştir:

    "resources": [
    {
      "name": "[variables('storageAccountName')]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "[variables('apiVersionStorage')]",
      "location": "[resourceGroup().location]",
      "comments": "This storage account is used to store the VM disks",
      "properties": {
      "accountType": "Standard_GRS"
      }
    }
    ]


## <a name="next-steps"></a>Sonraki adımlar
* [Şablonları PowerShell ile dağıtma](azure-stack-deploy-template-powershell.md)
* [Azure CLI ile şablonlarını dağıtma](azure-stack-deploy-template-command-line.md)
* [Şablonları Visual Studio ile dağıtma](azure-stack-deploy-template-visual-studio.md)

