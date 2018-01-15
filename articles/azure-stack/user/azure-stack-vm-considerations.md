---
title: "Farkları ve sanal makineleri Azure yığınında dikkate alınacak noktalar | Microsoft Docs"
description: "Farklar ve konuları Azure yığınında sanal makinelerle çalışırken öğrenin."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 6613946D-114C-441A-9F74-38E35DF0A7D7
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2017
ms.author: mabrigg
ms.openlocfilehash: 8367f7897581ff9599b763c7a39232bbe6860b8f
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2018
---
# <a name="considerations-for-virtual-machines-in-azure-stack"></a>Sanal makineler Azure yığınında dikkate alınacak noktalar

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Sanal makine, bir isteğe bağlı, ölçeklenebilir bilgi işlem kaynakları Azure yığını tarafından sunulan yok. Sanal makineler kullandığınızda, Azure'da kullanılabilen özellikleri ve Azure yığın arasındaki farklar olduğunu anlamanız gerekir. Bu makalede benzersiz konuları sanal makineler ve Azure yığınında özellikleri için genel bir bakış sağlar. Azure yığını ve Azure arasında üst düzey farklılıklar hakkında bilgi edinmek için [anahtar konuları](azure-stack-considerations.md) makalesi.

## <a name="cheat-sheet-virtual-machine-differences"></a>Kopya sayfası: sanal makine farklar

| Özellik | Azure (Genel) | Azure Stack |
| --- | --- | --- |
| Sanal makine görüntüleri | Azure Marketi'nde bir sanal makine oluşturmak için kullanabileceğiniz görüntüleri içerir. Bkz: [Azure Marketi](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/compute?subcategories=virtual-machine-images&page=1) Azure Marketi'nde kullanılabilir görüntüleri listesini görüntülemek için sayfa. | Varsayılan olarak, yok tüm görüntüleri kullanılabilir Azure yığın marketi'ndeki. Azure yığın bulut yönetici yayımlamak veya kullanıcılar kullanabilmek için önce Azure yığın Market görüntülerini indirin. |
| Sanal makine boyutları | Azure sanal makineler için çok çeşitli boyutlarını destekler. Kullanılabilir boyutları ve seçenekleri hakkında bilgi edinmek için bkz [Windows sanal makine boyutları](../../virtual-machines/virtual-machines-windows-sizes.md) ve [Linux sanal makine boyutlarını](../../virtual-machines/linux/sizes.md) Konular. | Azure yığını mevcut olan sanal makine boyutlarını kümesini destekler. Desteklenen boyutlar listesini görüntülemek için başvurmak [sanal makine boyutlarını](#virtual-machine-sizes) bu makalenin. |
| Sanal makine kotaları | [Kota sınırları](../../azure-subscription-service-limits.md#service-specific-limits) Microsoft tarafından ayarlanır | Bunlar, kullanıcılar sanal makineleri sunar önce Azure yığın bulut yönetici kotaları atamanız gerekir. |
| Sanal makine uzantıları |Azure sanal makine uzantıları çok geniş bir yelpazedeki destekler. Kullanılabilir uzantılar hakkında bilgi edinmek için bkz [sanal makine uzantıları ve özellikleri](../../virtual-machines/windows/extensions-features.md) makalesi.| Azure yığın Azure'da kullanılabilen uzantıları kümesini destekler ve her uzantısına sahip belirli sürümleri. Azure yığın bulut yönetici kendi kullanıcılar için kullanılabilir duruma getirilmek üzere hangi uzantıların seçebilirsiniz. Desteklenen uzantılarının listesini görüntülemek için bkz [sanal makine uzantıları](#virtual-machine-extensions) bu makalenin. |
| Sanal makine ağı | Kiracı sanal makinesi için atanan ortak IP adresleri Internet üzerinden erişilebilir.<br><br><br>Azure sanal makinelerin olduğu sabit bir DNS adı | Bir kiracı sanal makineye atanan genel IP adresleri yalnızca Azure yığın Geliştirme Seti ortamında erişilebilir. Bir kullanıcı bu Azure yığın Geliştirme Seti erişiminiz olmalıdır [RDP](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop) veya [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) Azure yığınında oluşturulan bir sanal makineye bağlanmak için.<br><br>Belirli bir Azure yığın örneği içinde oluşturulan sanal makineler bulut yönetici tarafından yapılandırılan değere göre bir DNS adına sahip. |
| Sanal makine depolama | Destekler [yönetilen diskler.](../../virtual-machines/windows/managed-disks-overview.md) | Yönetilen diskleri Azure yığınında henüz desteklenmiyor. |
| API sürümleri | Azure her zaman en son API sürümü tüm sanal makine özellikleri vardır. | Azure yığını, bu hizmetler için belirli Azure hizmetlerinin ve belirli API sürümleri destekler. Desteklenen API sürümleri listesini görüntülemek için başvurmak [API sürümleri](#api-versions) bu makalenin. |
|Sanal makine kullanılabilirlik kümeleri|Birden çok hata etki alanlarını (2 veya 3 bölge başına)<br>Birden çok güncelleştirme etki alanı<br>Yönetilen disk desteği|Tek hata etki alanı<br>Tek güncelleştirme etki alanı<br>Yönetilen disk desteği yok|
|Sanal makine ölçek kümeleri|Otomatik ölçek desteklenen|Otomatik-ölçek desteklenmiyor.<br>Daha fazla örnek portal, Resource Manager şablonları veya PowerShell kullanılarak ayarlanan bir ölçek ekleyin.

## <a name="virtual-machine-sizes"></a>Sanal makine boyutları

Azure yığın aşağıdaki boyutları destekler:

| Tür | Boyut | Desteklenen boyutlar aralığı |
| --- | --- | --- |
|Genel amaçlı |Temel A|A0 - A4|
|Genel amaçlı |Standart bir|A0 - A7|
|Genel amaçlı |D Serisi|D1 - D4|
|Genel amaçlı |Dv2 Serisi|D1_v2 - D5_v2|
|Genel amaçlı |DS serisi|DS1 - DS4|
|Genel amaçlı |DSv2 serisi|DS1_v2 - DS5_v2|
|Bellek için iyileştirilmiş|DS serisi|DS11 - DS14|
|Bellek için iyileştirilmiş |DSv2 serisi|DS11_v2 - DS14_v2|

Sanal makine boyutları ve bunların ilişkili kaynak miktarları Azure yığını ve Azure arasında tutarlı değil. Örneğin, bu tutarlılık çekirdek sayısı ve sayı/oluşturulabilmesi için veri diski boyutunun bellek miktarını içerir. Ancak, aynı VM boyutu Azure yığınında performansını belirli bir Azure yığın ortamda temel özelliklerine bağlıdır.

## <a name="virtual-machine-extensions"></a>Sanal makine uzantıları

 Azure yığın Geliştirme Seti aşağıdaki sanal makine uzantısı sürümlerini destekler:

![VM Uzantıları](media/azure-stack-vm-considerations/vm-extensions.png)

Azure yığın ortamınızda kullanılabilen sanal makine uzantıları listesini almak için aşağıdaki PowerShell betiğini kullanın:

```powershell
Get-AzureRmVmImagePublisher -Location local | `
  Get-AzureRmVMExtensionImageType | `
  Get-AzureRmVMExtensionImage | `
  Select Type, Version | `
  Format-Table -Property * -AutoSize
```

## <a name="api-versions"></a>API sürümleri

Sanal makine özellikleri Azure yığın Development Kit'te aşağıdaki API sürümlerini destekler:

![VM kaynak türleri](media/azure-stack-vm-considerations/vm-resoource-types.png)

Azure yığın ortamınızda kullanılabilir sanal makine özellikleri için API sürümü almak için aşağıdaki PowerShell betiğini kullanabilirsiniz:

```powershell
Get-AzureRmResourceProvider | `
  Select ProviderNamespace -Expand ResourceTypes | `
  Select * -Expand ApiVersions | `
  Select ProviderNamespace, ResourceTypeName, @{Name="ApiVersion"; Expression={$_}} | `
  where-Object {$_.ProviderNamespace -like “Microsoft.compute”}
```
Bulut operatörü, Azure yığın ortamınızı daha yeni bir sürüme güncelleştirir, desteklenen kaynak türleri ve API sürümleri listesini farklılık gösterebilir.

## <a name="next-steps"></a>Sonraki adımlar

[Azure yığınında PowerShell ile Windows sanal makine oluşturma](azure-stack-quick-create-vm-windows-powershell.md)
