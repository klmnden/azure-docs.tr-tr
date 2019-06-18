---
title: Çok müşterili barındırma hakları ile azure'da Windows 10 dağıtma
description: Şirket içi lisanslarını Azure'a taşımalarına olanak, Windows Yazılım Güvencesi avantajları en üst düzeye öğrenin
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
ms.openlocfilehash: 7f43528c55cd22c2649ca0f1208da6f41695b98e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61485524"
---
# <a name="how-to-deploy-windows-10-on-azure-with-multitenant-hosting-rights"></a>Çok müşterili barındırma hakları ile azure'da Windows 10 dağıtma 
Müşteriler için Windows 10 Enterprise E3/E5 ile Windows sanal masaüstü erişimi ya da kullanıcı başına kullanıcı (kullanıcı Abonelik lisansı veya eklenti kullanıcı Abonelik Lisansı), çok Kiracılı barındırma hakları Windows 10 için Windows 10 lisanslarınızı buluta getirmenize olanak sağlar ve Windows 10 sanal makineleri başka bir lisans için ödeme yapmadan Azure üzerinde çalıştırın. Daha fazla bilgi için lütfen bkz [Windows 10 için çok Kiracılı barındırma](https://www.microsoft.com/en-us/CloudandHosting/licensing_sca.aspx).

> [!NOTE]
> Bu makale Azure Market'te Windows 10 Pro Masaüstü görüntüleri için lisanslama avantajı uygulayın.
> - Windows 7, 8.1, Azure Marketi'nde MSDN abonelikleri için 10 Enterprise (x64) görüntüleri için bkz [geliştirme/test senaryoları için azure'da Windows İstemcisi](client-images.md)
> - Windows Server lisanslama avantajları, başvurun [Windows Server görüntüleri için Azure hibrit kullanma avantajları](hybrid-use-benefit-licensing.md).
>

## <a name="deploying-windows-10-image-from-azure-marketplace"></a>Windows 10 Azure Market görüntüsünden dağıtma 
PowerShell, CLI ve Azure Resource Manager şablon dağıtımları için Windows 10 görüntüsü bulunabilir aşağıdaki publishername, teklif, sku.

| İşletim Sistemi  |      PublisherName      |  Sunduğu | Sku |
|:----------|:-------------:|:------|:------|
| Windows 10 Pro    | MicrosoftWindowsDesktop | Windows 10  | RS2-Pro   |
| Windows 10 Pro N  | MicrosoftWindowsDesktop | Windows 10  | RS2-ProN  |
| Windows 10 Pro    | MicrosoftWindowsDesktop | Windows 10  | RS3-Pro   |
| Windows 10 Pro N  | MicrosoftWindowsDesktop | Windows 10  | RS3-ProN  |

## <a name="uploading-windows-10-vhd-to-azure"></a>Windows 10 karşıya Azure VHD'nin
Windows 10 genelleştirilmiş VHD yüklüyorsanız, Windows 10, varsayılan olarak etkin yerleşik yönetici hesabı yok. Lütfen unutmayın. Yerleşik Yönetici hesabını etkinleştirmek için aşağıdaki komutu kullanarak özel betik uzantısı bir parçası olarak içerir.

```powershell
Net user <username> /active:yes
```

Aşağıdaki powershell kod parçacığı yerleşik yönetici de dahil olmak üzere tüm yönetici hesapları etkin olarak işaretlemektir. Bu örnekte, yerleşik yönetici kullanıcı adı bilinmiyor yararlıdır.
```powershell
$adminAccount = Get-WmiObject Win32_UserAccount -filter "LocalAccount=True" | ? {$_.SID -Like "S-1-5-21-*-500"}
if($adminAccount.Disabled)
{
    $adminAccount.Disabled = $false
    $adminAccount.Put()
}
```
Daha fazla bilgi için: 
* [Azure'a VHD'yi karşıya yükleme](upload-generalized-managed.md)
* [Yüklemek için bir Windows VHD hazırlama](prepare-for-upload-vhd-image.md)


## <a name="deploying-windows-10-with-multitenant-hosting-rights"></a>Çok Kiracılı barındırma hakları ile Windows 10 dağıtımı
Olduğundan emin olun [en son Azure PowerShell yüklenmiş ve yapılandırılmışsa](/powershell/azure/overview). VHD'nizi hazırlandıktan sonra VHD kullanarak Azure depolama hesabınız için karşıya `Add-AzVhd` cmdlet'i aşağıdaki gibi:

```powershell
Add-AzVhd -ResourceGroupName "myResourceGroup" -LocalFilePath "C:\Path\To\myvhd.vhd" `
    -Destination "https://mystorageaccount.blob.core.windows.net/vhds/myvhd.vhd"
```


**Azure Resource Manager şablon dağıtımı kullanarak dağıtın** Resource Manager şablonlarınızı için ek bir parametre içinde `licenseType` belirtilebilir. Daha fazla bilgi edinebilirsiniz [Azure Resource Manager şablonları yazma](../../resource-group-authoring-templates.md). Azure'a yüklenmiş VHD'nizi aldıktan sonra lisans türü işlem Sağlayıcısı'nın bir parçası olarak dahil etmek ve normal olarak, şablonunuzu dağıtmak için Resource Manager Şablonu Düzenle:
```json
"properties": {
    "licenseType": "Windows_Client",
    "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
    }
```

**PowerShell dağıtma** Windows Server sanal Makinenizi PowerShell aracılığıyla dağıtım yaparken, ek bir parametre için sahip `-LicenseType`. Azure'a yüklenmiş VHD'nizi oluşturduktan sonra kullanarak bir VM oluşturma `New-AzVM` ve lisans türünü aşağıdaki gibi belirtin:
```powershell
New-AzVM -ResourceGroupName "myResourceGroup" -Location "West US" -VM $vm -LicenseType "Windows_Client"
```

## <a name="verify-your-vm-is-utilizing-the-licensing-benefit"></a>Sanal makinenizin, lisans avantajından yararlanarak doğrulayın
Ya da PowerShell veya Resource Manager dağıtım yöntemi aracılığıyla, sanal Makinenizin dağıttıktan sonra lisans türü ile doğrulama `Get-AzVM` gibi:
```powershell
Get-AzVM -ResourceGroup "myResourceGroup" -Name "myVM"
```

Çıktı, Windows 10 için doğru lisans türü aşağıdaki örneğe benzer:

```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : Windows_Client
```

Bu çıktı karşılaştırır Azure hibrit kullanım teklifi, Azure Galerisi'nden düz dağıtılan VM gibi lisans olmadan dağıtılan aşağıdaki VM ile:

```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              :
```

## <a name="additional-information-about-joining-azure-ad"></a>Azure AD'ye katılımı hakkında ek bilgi
>[!NOTE]
>Azure, tüm Windows VM AAD'ye katılma için kullanılamaz yerleşik yönetici hesabıyla sağlar. Örneğin, *Ayarları > Hesap > Access iş veya Okul > + Connect* çalışmaz. Oluşturmalı ve el ile Azure AD'ye katılmak için ikinci bir yönetici hesabı oturum açın. Bir sağlama paketi kullanarak Azure AD'de de yapılandırabilirsiniz, bağlantısını kullanın *sonraki adımlar* bölümü daha fazla bilgi edinin.
>
>

## <a name="next-steps"></a>Sonraki Adımlar
- Daha fazla bilgi edinin [VDA Windows 10 yapılandırma](https://docs.microsoft.com/windows/deployment/vda-subscription-activation)
- Daha fazla bilgi edinin [Windows 10 için çok Kiracılı barındırma](https://www.microsoft.com/en-us/CloudandHosting/licensing_sca.aspx)


