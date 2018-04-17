---
title: PowerShell kullanarak bir Azure Analysis Services sunucusu oluşturma | Microsoft Docs
description: PowerShell kullanarak bir Azure Analysis Services sunucusu oluşturma hakkında bilgi edinin.
author: minewiskan
manager: kfile
ms.service: analysis-services
ms.topic: conceptual
ms.date: 04/12/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: cba5d5d6547421ed2d665fc78e35a654eda5fda6
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="create-an-azure-analysis-services-server-by-using-powershell"></a>PowerShell kullanarak bir Azure Analysis Services sunucusu oluşturma

Bu hızlı başlangıç, Azure aboneliğinizde bir [Azure kaynak grubunda](../azure-resource-manager/resource-group-overview.md) Azure Analysis Services sunucusu oluşturmak için komut satırından PowerShell kullanmayı anlatmaktadır.

Bu görev için Azure PowerShell modülünün 4.0 veya daha sonraki bir sürümü gerekir. Sürümü bulmak için ` Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yüklemek veya yükseltmek için bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-azurerm-ps). 

> [!NOTE]
> Sunucu oluşturulması ek hizmet ücretlerine neden olabilir. Daha fazla bilgi için bkz. [Analysis Services fiyatlandırması](https://azure.microsoft.com/pricing/details/analysis-services/).

## <a name="before-you-begin"></a>Başlamadan önce
Bu hızlı başlangıcı tamamlamak için şunlar gerekir:

* **Azure aboneliği**: Hesap oluşturmak için [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/offers/ms-azr-0044p/)’nü ziyaret edin.
* **Azure Active Directory**: Aboneliğinizin bir Azure Active Directory Kiracısı ile ilişkilendirilmiş olması ve ilgili dizinde bir hesabınızın olması gerekir. Daha fazla bilgi edinmek için bkz. [Kimlik doğrulaması ve kullanıcı izinleri](analysis-services-manage-users.md).

## <a name="import-azurermanalysisservices-module"></a>AzureRm.AnalysisServices modülünü içeri aktarın
Aboneliğinizde bir sunucu oluşturmak için [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices) bileşen modülünü kullanırsınız. AzureRm.AnalysisServices modülünü PowerShell oturumunuza yükleyin.

```powershell
Import-Module AzureRM.AnalysisServices
```

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Add-AzureRmAccount](/powershell/module/azurerm.profile/add-azurermaccount) komutunu kullanarak Azure aboneliğinizde oturum açın. Ekrandaki yönergeleri izleyin.

```powershell
Add-AzureRmAccount
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
 
[Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md), Azure kaynaklarının grup olarak dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Sunucunuzu oluşturduğunuzda, aboneliğinizde bir kaynak grubu belirtmeniz gerekir. Zaten bir kaynak grubunuz yoksa [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) komutunu çalıştırarak bir kaynak grubu oluşturabilirsiniz. Aşağıdaki örnekte Batı ABD bölgesinde `myResourceGroup` adında bir kaynak grubu oluşturulmaktadır.

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "West US"
```

## <a name="create-a-server"></a>Sunucu oluşturma

[New-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver) komutunu kullanarak yeni bir sunucu oluşturun. Aşağıdaki örnek, Batı ABD bölgesinde D1 katmanında myResourceGroup’ta myServer adlı bir sunucu oluşturur ve sunucu yöneticisi olarak philipc@adventureworks.com adresini belirtir.

```powershell
New-AzureRmAnalysisServicesServer -ResourceGroupName "myResourceGroup" -Name "myServer" -Location West US -Sku D1 -Administrator "philipc@adventure-works.com"
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

[Remove-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver) komutunu kullanarak sunucuyu aboneliğinizden kaldırabilirsiniz. Bu koleksiyondaki diğer hızlı başlangıçları ve öğreticileri kullanacaksanız sunucunuzu kaldırmayın. Aşağıdaki örnekte önceki adımda oluşturduğunuz sunucu kaldırılır.


```powershell
Remove-AzureRmAnalysisServicesServer -Name "myServer" -ResourceGroupName "myResourceGroup"
```

## <a name="next-steps"></a>Sonraki adımlar
[Azure Analysis Services’ı PowerShell ile yönetme](analysis-services-powershell.md)   
[SSDT’den model dağıtma](analysis-services-deploy.md)   
[Azure portalında bir model oluşturma](analysis-services-create-model-portal.md)