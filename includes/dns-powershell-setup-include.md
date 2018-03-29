---
title: dosya için PowerShell Azure DNS'de ekleyin.
description: dosya için PowerShell Azure DNS'de ekleyin.
services: dns
author: subsarma
ms.service: dns
ms.topic: include file for PowerShell for Azure DNS
ms.date: 03/21/2018
ms.author: subsarma
ms.custom: include file for PowerShell for Azure DNS
ms.openlocfilehash: 1ddfd1ae8dffbc5d381773ea9679713e93a44a32
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
## <a name="set-up-azure-powershell-for-azure-dns"></a>Azure DNS için Azure PowerShell ayarlayın

### <a name="before-you-begin"></a>Başlamadan önce

Yapılandırmanıza başlamadan önce aşağıdaki öğelerin bulunduğunu doğrulayın.

* Azure aboneliği. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
* Azure Resource Manager PowerShell cmdlet'lerinin en yeni sürümünü yüklemeniz gerekir. Daha fazla bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azureps-cmdlets-docs).

Ayrıca, özel bölgeler (genel Önizleme) kullanmak için aşağıdaki PowerShell modülleri ve sürümleri sahip emin olmak gerekir. 
* AzureRM.Dns - [sürüm 4.1.0'da](https://www.powershellgallery.com/packages/AzureRM.Dns/4.1.0) veya üstü
* AzureRM.Network - [sürüm 5.4.0](https://www.powershellgallery.com/packages/AzureRM.Network/5.4.0) veya üstü

```powershell 
Find-Module -Name AzureRM.Dns 
``` 
 
```powershell 
Find-Module -Name AzureRM.Network 
``` 
 
Yukarıdaki komutların çıktısı AzureRM.Dns sürümü 4.1.0'da veya daha yüksek bir sürüm ve AzureRM.Network için 5.4.0 ya da daha yüksek bir sürümü olduğunu göstermek gerekir.  

Sisteminizin önceki sürümlere sahip olmaması durumunda, ya da Azure PowerShell'in en son sürümünü yükleyin veya yükleyip yukarıdaki modüllerini PowerShell Galerisi'nden modül sürümleri yanındaki yukarıda bağlantıları kullanarak. Daha sonra bunları yükleyebilirsiniz kullanarak aşağıdaki komutları. Her iki modülleri gereklidir ve tam olarak geriye dönük olarak uyumludur. 

```powershell
Install-Module -Name AzureRM.Dns -Force
```

```powershell
Install-Module -Name AzureRM.Network -Force
```

### <a name="sign-in-to-your-azure-account"></a>Azure hesabınızda oturum açma

PowerShell konsolunuzu açın ve hesabınıza bağlanın. Daha fazla bilgi için bkz: [PowerShell kullanarak Resource Manager ile](../articles/azure-resource-manager/powershell-azure-resource-manager.md).

```powershell
Login-AzureRmAccount
```

### <a name="select-the-subscription"></a>Aboneliği seçme
 
Hesapla ilişkili abonelikleri kontrol edin.

```powershell
Get-AzureRmSubscription
```

Hangi Azure aboneliğinizin kullanılacağını seçin.

```powershell
Select-AzureRmSubscription -SubscriptionName "your_subscription_name"
```

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure Resource Manager, tüm kaynak gruplarının bir konum belirtmesini gerektirir. Bu konum, kaynak grubundaki kaynaklar için varsayılan konum olarak kullanılır. Ancak tüm DNS kaynakları bölgesel değil de global olduğundan, kaynak grubu konumu seçiminin Azure DNS üzerinde hiçbir etkisi yoktur.

Var olan bir kaynak grubunu kullanıyorsanız bu adımı atlayabilirsiniz.

```powershell
New-AzureRmResourceGroup -Name MyAzureResourceGroup -location "West US"
```

### <a name="register-resource-provider"></a>Kaynak sağlayıcısını kaydetme

Azure DNS hizmeti, Microsoft.Network kaynak sağlayıcısı tarafından yönetilir. Azure DNS'yi kullanabilmeniz için, Azure aboneliğinizin bu kaynak sağlayıcısını kullanmak üzere kayıtlı olması gerekir. Bu, her bir abonelik için tek seferlik bir işlemdir.

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
```
