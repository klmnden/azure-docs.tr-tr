---
title: 'Hızlı başlangıç: PowerShell kullanarak bir Azure Analysis Services sunucusu oluşturma | Microsoft Docs'
description: PowerShell kullanarak bir Azure Analysis Services sunucusu oluşturma hakkında bilgi edinin.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: quickstart
ms.date: 10/18/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 8ce9606c6b59175e03da5382f76fb320080db86e
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49427035"
---
# <a name="quickstart-create-a-server---powershell"></a>Hızlı başlangıç: Sunucu oluşturma - PowerShell

Bu hızlı başlangıç, Azure aboneliğinizde bir Azure Analysis Services sunucusu oluşturmak için komut satırından PowerShell kullanmayı anlatmaktadır.

## <a name="prerequisites"></a>Ön koşullar

- **Azure aboneliği**: Hesap oluşturmak için [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/offers/ms-azr-0044p/)’nü ziyaret edin.
- **Azure Active Directory**: Aboneliğinizin bir Azure Active Directory Kiracısı ile ilişkilendirilmiş olması ve ilgili dizinde bir hesabınızın olması gerekir. Daha fazla bilgi edinmek için bkz. [Kimlik doğrulaması ve kullanıcı izinleri](analysis-services-manage-users.md).
- **Azure PowerShell modülünün 4.0 veya daha sonraki bir sürümü**. Sürümü bulmak için ` Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yüklemek veya yükseltmek için bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-azurerm-ps).

## <a name="import-azurermanalysisservices-module"></a>AzureRm.AnalysisServices modülünü içeri aktarın

Aboneliğinizde bir sunucu oluşturmak için [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices) bileşen modülünü kullanırsınız. AzureRm.AnalysisServices modülünü PowerShell oturumunuza yükleyin.

```powershell
Import-Module AzureRM.AnalysisServices
```

## <a name="log-in-to-azure"></a>Azure'da oturum açma

[Connect-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount) komutunu kullanarak Azure aboneliğinizde oturum açın. Ekrandaki yönergeleri izleyin.

```powershell
Connect-AzureRmAccount
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md), Azure kaynaklarının grup olarak dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Sunucunuzu oluşturduğunuzda, aboneliğinizde bir kaynak grubu belirtmeniz gerekir. Zaten bir kaynak grubunuz yoksa [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) komutunu çalıştırarak bir kaynak grubu oluşturabilirsiniz. Aşağıdaki örnekte Batı ABD bölgesinde `myResourceGroup` adında bir kaynak grubu oluşturulmaktadır.

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "WestUS"
```

## <a name="create-a-server"></a>Sunucu oluşturma

[New-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver) komutunu kullanarak yeni bir sunucu oluşturun. Aşağıdaki örnek, Batı ABD bölgesinde D1 (ücretsiz) katmanında myResourceGroup’ta myServer adlı bir sunucu oluşturur ve sunucu yöneticisi olarak philipc@adventureworks.com adresini belirtir.

```powershell
New-AzureRmAnalysisServicesServer -ResourceGroupName "myResourceGroup" -Name "myserver" -Location WestUS -Sku D1 -Administrator "philipc@adventure-works.com"
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

[Remove-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver) komutunu kullanarak sunucuyu aboneliğinizden kaldırabilirsiniz. Bu koleksiyondaki diğer hızlı başlangıçları ve öğreticileri kullanacaksanız sunucunuzu kaldırmayın. Aşağıdaki örnekte önceki adımda oluşturduğunuz sunucu kaldırılır.


```powershell
Remove-AzureRmAnalysisServicesServer -Name "myserver" -ResourceGroupName "myResourceGroup"
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta PowerShell kullanarak Azure aboneliğinizde sunucu oluşturmayı öğrendiniz. Sunucuyu oluşturduğunuza göre bir sunucu güvenlik duvarı yapılandırarak (isteğe bağlı) güvenliğinin sağlanmasına yardımcı olacaksınız. Sunucunuza doğrudan portaldan örnek bir veri modeli de ekleyebilirsiniz. Örnek modelin olması, model veritabanı rollerini yapılandırma ve istemci bağlantılarını test etme işlemlerini öğrenme konusunda yardımcı olur. Daha fazla bilgi edinmek için, örnek model ekleme öğreticisiyle devam edin.

> [!div class="nextstepaction"]
> [Hızlı başlangıç: Sunucu güvenlik duvarını yapılandırma - Portal](analysis-services-qs-firewall.md)      
> [!div class="nextstepaction"]
> [Öğretici: Sunucunuza örnek model ekleme](analysis-services-create-sample-model.md)