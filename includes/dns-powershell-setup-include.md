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
ms.openlocfilehash: e96b312f03069256396261bd6efe2f2586cdadea
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
## <a name="set-up-azure-powershell-for-azure-dns"></a>Azure DNS için Azure PowerShell ayarlayın

### <a name="before-you-begin"></a>Başlamadan önce

Yapılandırmanıza başlamadan önce aşağıdaki öğelerin bulunduğunu doğrulayın.

* Azure aboneliği. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
* Azure Resource Manager PowerShell cmdlet'lerinin en yeni sürümünü yüklemeniz gerekir. Daha fazla bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azureps-cmdlets-docs).

Ayrıca, özel bölgeler (genel Önizleme) kullanmak için aşağıdaki PowerShell modülleri ve sürümleri sahip emin olmak gerekir. 
* AzureRM.Dns - [sürüm 4.1.0'da](https://www.powershellgallery.com/packages/AzureRM.Dns/4.1.0) veya üstü
* AzureRM.Network - [sürüm 5.4.0](https://www.powershellgallery.com/packages/AzureRM.Network/5.4.0) veya üstü

PowerShell Galerisi'nden yukarıdaki modülleri yukarıda modül sürümleri yanındaki bağlantıları kullanarak yükleyebilirsiniz. Daha sonra bunları yükleyebilirsiniz kullanarak aşağıdaki komutları. Her iki modülleri gereklidir ve tam olarak geriye dönük olarak uyumludur. 

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