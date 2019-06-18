---
title: Azure DNS için PowerShell dosyasını dahil etme
description: Azure DNS için PowerShell dosyasını dahil etme
services: dns
author: subsarma
ms.service: dns
ms.topic: include file for PowerShell for Azure DNS
ms.date: 03/21/2018
ms.author: subsarma
ms.custom: include file for PowerShell for Azure DNS
ms.openlocfilehash: 32c516ccee3a9f4f7604a3e330285703a776b47d
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67133620"
---
## <a name="set-up-azure-powershell-for-azure-dns"></a>Azure DNS için Azure PowerShell ayarlama

### <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [requires-azurerm](requires-azurerm.md)]

Yapılandırmanıza başlamadan önce aşağıdaki öğelerin bulunduğunu doğrulayın.

* Azure aboneliği. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
* Azure Resource Manager PowerShell cmdlet’lerinin en yeni sürümünü yüklemeniz gerekir. Daha fazla bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azureps-cmdlets-docs).

Ayrıca, Özel Bölgeler (Genel Önizleme) kullanmak için aşağıdaki PowerShell modüllerine ve sürümlerine sahip olduğunuzdan emin olmanız gerekir. 
* AzureRM.Dns - [sürüm 4.1.0](https://www.powershellgallery.com/packages/AzureRM.Dns/4.1.0) veya üzeri
* AzureRM.Network - [sürüm 5.4.0](https://www.powershellgallery.com/packages/AzureRM.Network/5.4.0) veya üzeri

```powershell 
Find-Module -Name AzureRM.Dns 
``` 
 
```powershell 
Find-Module -Name AzureRM.Network 
``` 
 
Yukarıdaki komutların çıktısı, AzureRM.Dns sürümünün 4.1.0 veya üzeri bir sürüm olduğunu ve AzureRM.Network için 5.4.0 ya da üzeri bir sürüm olduğunu göstermek gerekir.  

Sisteminizin önceki sürümlere sahip olması durumunda, Modül sürümlerinin yanındaki bağlantıları kullanarak en son Azure PowerShell sürümünü yükleyebilir veya PowerShell Galerisinden yukarıdaki modülleri yükleyebilirsiniz. Daha sonra aşağıdaki komutları kullanarak bunları yükleyebilirsiniz. Her iki modül de zorunludur ve tamamen geri dönük olarak uyumludur. 

```powershell
Install-Module -Name AzureRM.Dns -Force
```

```powershell
Install-Module -Name AzureRM.Network -Force
```

### <a name="sign-in-to-your-azure-account"></a>Azure hesabınızda oturum açma

PowerShell konsolunuzu açın ve hesabınıza bağlanın. Daha fazla bilgi için [oturum oturum AzureRM](/powershell/azure/azurerm/authenticate-azureps).

```powershell
Connect-AzureRmAccount
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
