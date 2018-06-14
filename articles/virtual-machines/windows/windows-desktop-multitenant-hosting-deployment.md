---
title: Çok kullanıcılı barındırma hakları ile azure'da Windows 10 dağıtma
description: Azure için şirket içi lisansları getirmek için Windows Yazılım Güvencesi Avantajlarınızı en üst düzeye öğrenin
services: virtual-machines-windows
documentationcenter: ''
author: xujing
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 1/24/2018
ms.author: xujing
ms.openlocfilehash: 5952c602a90568a9ce9e71dfa2c0dd383aed4e16
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30915380"
---
# <a name="how-to-deploy-windows-10-on-azure-with-multitenant-hosting-rights"></a>Çok kullanıcılı barındırma hakları ile azure'da Windows 10 dağıtma 
Müşteriler için Windows 10 Kurumsal E3/E5 ile kullanıcı başına ya da Windows sanal masaüstü erişimi her kullanıcı (kullanıcı Abonelik lisansı veya eklenti kullanıcı Abonelik Lisansı), çok kullanıcılı barındırma hakları Windows 10 için Windows 10 lisanslarınızı buluta getirmelerine olanak tanır ve Windows 10 sanal makineleri Azure üzerinde başka bir lisans için ödeme olmadan çalıştırın. Daha fazla bilgi için lütfen bkz [Windows 10 için çok kullanıcılı barındırma](https://www.microsoft.com/en-us/CloudandHosting/licensing_sca.aspx).

> [!NOTE]
> Bu makalede, Windows 10 Pro Masaüstü görüntüleri için lisans avantajı Azure Marketi uygulamak için gösterir.
> - Windows 7, 8.1, MSDN abonelikleri için Azure Marketi 10 Enterprise (x64) görüntülerinde için lütfen [geliştirme ve test senaryoları için azure'da Windows İstemcisi](client-images.md)
> - Avantajları lisans Windows Server için lütfen [Azure karma kullanma avantajları için Windows Server görüntülerini](hybrid-use-benefit-licensing.md).
>

## <a name="deploying-windows-10-image-from-azure-marketplace"></a>Windows 10 Azure Market görüntüsünden dağıtma 
PowerShell'i, CLI ve Azure Resource Manager şablonu dağıtımları için Windows 10 görüntüsü bulunabilir aşağıdaki publishername, teklif, sku.

| İşletim Sistemi  |      PublisherName      |  Sunduğu | Sku |
|:----------|:-------------:|:------|:------|
| Windows 10 Pro    | MicrosoftWindowsDesktop | Windows-10  | RS2-Pro   |
| Windows 10 Pro N  | MicrosoftWindowsDesktop | Windows-10  | RS2-ProN  |
| Windows 10 Pro    | MicrosoftWindowsDesktop | Windows-10  | RS3-Pro   |
| Windows 10 Pro N  | MicrosoftWindowsDesktop | Windows-10  | RS3-ProN  |

## <a name="uploading-windows-10-vhd-to-azure"></a>Windows 10 karşıya azure'a VHD
genelleştirilmiş bir Windows 10 VHD yüklüyorsanız, Windows 10 varsayılan olarak etkin yerleşik yönetici hesabı yok Lütfen unutmayın. Yerleşik Yönetici hesabını etkinleştirmek için aşağıdaki komutu özel betik uzantısı'nın bir parçası olarak ekleyin.

```powershell
Net user <username> /active:yes
```

Yerleşik yönetici de dahil olmak üzere tüm yönetici hesapları etkin olarak işaretlemek için aşağıdaki powershell parçacığını değil. Bu örnek, yerleşik yönetici kullanıcı adı bilinmiyor yararlıdır.
```powershell
$adminAccount = Get-WmiObject Win32_UserAccount -filter "LocalAccount=True" | ? {$_.SID -Like "S-1-5-21-*-500"}
if($adminAccount.Disabled)
{
    $adminAccount.Disabled = $false
    $adminAccount.Put()
}
```
Daha fazla bilgi için: 
* [Azure'a VHD yüklemek nasıl](upload-generalized-managed.md)
* [Windows Azure'a yüklemeniz VHD hazırlama](prepare-for-upload-vhd-image.md)


## <a name="deploying-windows-10-with-multitenant-hosting-rights"></a>Windows 10 çok müşterili barındırma haklarıyla dağıtma
Olduğundan emin olun [yüklenmiş ve yapılandırılmış en son Azure PowerShell](/powershell/azure/overview). VHD hazır olduktan sonra Azure depolama hesabı kullanmaya VHD'nin yüklenmesi `Add-AzureRmVhd` şekilde cmdlet:

```powershell
Add-AzureRmVhd -ResourceGroupName "myResourceGroup" -LocalFilePath "C:\Path\To\myvhd.vhd" `
    -Destination "https://mystorageaccount.blob.core.windows.net/vhds/myvhd.vhd"
```


**Azure Resource Manager şablonu dağıtımı dağıtmanızı** için ek bir parametre, Resource Manager şablonları içinde `licenseType` belirtilebilir. Daha fazla bilgi edinebilirsiniz [Azure Resource Manager şablonları yazma](../../resource-group-authoring-templates.md). Azure'a karşıya, VHD olduktan sonra lisans türü işlem sağlayıcısı bir parçası olarak dahil edin ve normal olarak, şablonunuzu dağıtmak için Resource Manager şablonunu düzenleyin:
```json
"properties": {  
   "licenseType": "Windows_Client",
   "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
   }
```

**PowerShell dağıtma** , Windows Server VM PowerShell aracılığıyla dağıtırken için ek bir parametre olan `-LicenseType`. Azure için karşıya, VHD'yi oluşturduktan sonra kullanarak bir VM oluşturun `New-AzureRmVM` ve lisans türü aşağıdaki gibi belirtin:
```powershell
New-AzureRmVM -ResourceGroupName "myResourceGroup" -Location "West US" -VM $vm -LicenseType "Windows_Client"
```

## <a name="verify-your-vm-is-utilizing-the-licensing-benefit"></a>VM lisans avantajı kullanılarak doğrulayın
Her iki PowerShell veya Resource Manager dağıtım yöntemi, VM dağıttıktan sonra lisans türü ile doğrulayın `Get-AzureRmVM` gibi:
```powershell
Get-AzureRmVM -ResourceGroup "myResourceGroup" -Name "myVM"
```

Çıkış, doğru lisans türüne sahip Windows 10 için aşağıdaki örneğe benzer:

```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : Windows_Client
```

Bu çelişir çıktı Azure karma kullanımı avantajı, doğrudan Azure galerisinden dağıtılan VM gibi lisans olmadan dağıtılan aşağıdaki VM ile:

```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              :
```

## <a name="additional-information-about-joining-azure-ad"></a>Azure AD birleştirme hakkında ek bilgi
>[!NOTE]
>Azure AAD katılmaya kullanılamaz yerleşik yönetici hesabı olan tüm Windows VM'ler sağlar. Örneğin, *ayarlar > hesabı > erişim iş veya Okul > + Bağlan* çalışmaz. Oluşturun ve el ile Azure AD katılım için ikinci bir yönetici hesabı ile oturum açın. Azure AD sağlama paketini kullanarak da yapılandırabilirsiniz, bağlantısını kullanın *sonraki adımlar* daha fazla bilgi için bölüm.
>
>

## <a name="next-steps"></a>Sonraki Adımlar
- Daha fazla bilgi edinmek [VDA Windows 10 yapılandırma](https://docs.microsoft.com/windows/deployment/vda-subscription-activation)
- Daha fazla bilgi edinmek [Windows 10 için çok kullanıcılı barındırma](https://www.microsoft.com/en-us/CloudandHosting/licensing_sca.aspx)


