---
title: Şablonları Azure Stack için geliştirme | Microsoft Docs
description: Azure Stack şablon en iyi uygulamaları öğrenin
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 8a5bc713-6f51-49c8-aeed-6ced0145e07b
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2019
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: 34804dae53fcf06d1a18bf503cdabea61f272585
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/07/2019
ms.locfileid: "54065401"
---
# <a name="azure-resource-manager-template-considerations"></a>Azure Resource Manager şablonu konuları

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Uygulamanızı geliştirirken, Azure ve Azure Stack arasında şablon taşınabilirliği sağlamak önemlidir. Bu makalede Azure Resource Manager geliştirme dikkat edilmesi gereken noktalar sunulmaktadır [şablonları](https://download.microsoft.com/download/E/A/4/EA4017B5-F2ED-449A-897E-BD92E42479CE/Getting_Started_With_Azure_Resource_Manager_white_paper_EN_US.pdf), uygulama ve test dağıtımınızı azure'da bir Azure Stack ortamınıza erişmeden prototipi olabilir.

## <a name="resource-provider-availability"></a>Kaynak sağlayıcısı kullanılabilirliği

Dağıtmayı planladığınız şablon zaten mevcut veya Azure Stack'te Önizleme aşamasında olan Microsoft Azure Hizmetleri yalnızca kullanmanız gerekir.

## <a name="public-namespaces"></a>Ortak ad alanları

Azure Stack, veri merkezinizde barındırıldığı için Azure genel bulutunda farklı hizmet uç nokta ad alanları vardır. Azure Stack dağıtmayı denediğinizde sonuç olarak, sabit kodlanmış ortak uç noktalar Azure Resource Manager şablonlarında başarısız. Hizmet uç noktaları kullanarak dinamik olarak oluşturabileceğiniz `reference` ve `concatenate` değerleri, dağıtım sırasında kaynak Sağlayıcısı'ndan almak için işlevleri. Örneğin, sabit kodlama yerine *blob.core.windows.net* almak, şablonunuzda [primaryEndpoints.blob](https://github.com/Azure/AzureStack-QuickStart-Templates/blob/master/101-vm-windows-create/azuredeploy.json#L175) dinamik olarak ayarlamak için *osDisk.URI* uç noktası:

```json
"osDisk": {"name": "osdisk","vhd": {"uri":
"[concat(reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2015-06-15').primaryEndpoints.blob, variables('vmStorageAccountContainerName'),
 '/',variables('OSDiskName'),'.vhd')]"}}
```

## <a name="api-versioning"></a>API sürümü oluşturma

Azure hizmet sürümleri, Azure ve Azure Stack arasında farklılık gösterebilir. Her kaynak gerektiriyor **apiVersion** sunulan tanımlayan özniteliği. Azure stack'teki API sürüm uyumluluğu sağlamak için her kaynak sağlayıcısı için aşağıdaki API sürümlerini geçerlidir:

| Kaynak Sağlayıcısı | apiVersion |
| --- | --- |
| İşlem |`'2015-06-15'` |
| Ağ |`'2015-06-15'`, `'2015-05-01-preview'` |
| Depolama |`'2016-01-01'`, `'2015-06-15'`, `'2015-05-01-preview'` |
| KeyVault | `'2015-06-01'` |
| App Service |`'2015-08-01'` |

## <a name="template-functions"></a>Şablon işlevleri

Azure Resource Manager [işlevleri](../../azure-resource-manager/resource-group-template-functions.md) dinamik şablonları oluşturmak için gereken özellikleri sağlar. Örneğin, İşlevler için görevleri aşağıdaki gibi kullanabilirsiniz:

* Birleştirme veya kırpma dizeleri.
* Değerler diğer kaynaklardan başvuruyor.
* Birden çok örneği dağıtılacak kaynaklar üzerinde yineleme.

Bu işlevler, Azure Stack'te kullanılamaz:

* Atla
* sınav zamanı

## <a name="resource-location"></a>Kaynak konumu

Azure Resource Manager şablonlarını kullanma bir `location` dağıtım sırasında kaynaklara yerleştirmek için özniteliği. Azure'da, Batı ABD veya Güney Amerika gibi bir bölgesi Konumlar bakın. Azure Stack'te Azure Stack, veri merkezinizde olduğundan konumları farklıdır. Şablonları Azure ve Azure Stack arasında aktarılamaz emin olmak için tek tek kaynakları dağıtma gibi bir kaynak grubu konumu başvurmalıdır. Bunu kullanarak yapabilirsiniz `[resourceGroup().Location]` tüm kaynakları kaynak grubu konumu devral emin olmak için. Aşağıdaki kod, bir depolama hesabını dağıtırken bu işlevi kullanarak, bir örnek verilmiştir:

```json
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
```

## <a name="next-steps"></a>Sonraki adımlar

* [Şablonları PowerShell ile dağıtma](azure-stack-deploy-template-powershell.md)
* [Şablonları Azure CLI ile dağıtma](azure-stack-deploy-template-command-line.md)
* [Şablonları Visual Studio ile dağıtma](azure-stack-deploy-template-visual-studio.md)
