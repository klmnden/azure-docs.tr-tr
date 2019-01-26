---
title: Farklılıklar ve Azure stack'teki sanal makineler için dikkat edilmesi gerekenler | Microsoft Docs
description: Azure stack'teki sanal makinelerle çalışırken farklılıklar ve dikkat edilmesi gerekenler hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2018
ms.author: mabrigg
ms.reviewer: kivenkat
ms.openlocfilehash: bfe53ac99ae1719deeacc156b250fe5a7f87a99a
ms.sourcegitcommit: 97d0dfb25ac23d07179b804719a454f25d1f0d46
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/25/2019
ms.locfileid: "54913470"
---
# <a name="considerations-for-using-virtual-machines-in-azure-stack"></a>Azure Stack'te sanal makineleri kullanma konuları

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack sanal makineleri isteğe bağlı ve ölçeklenebilir işlem kaynağı sağlar. Sanal makineleri (VM'ler) dağıtmadan önce Microsoft Azure ile Azure Stack'te kullanılabilen sanal makine özellikleri arasındaki farkı anlamanız gerekir. Bu makalede, bu farklılıkları açıklar ve sanal makine dağıtımları planlaması için önemli noktalar yer tanımlar. Azure Stack ve Azure arasında üst düzey farklılıklar hakkında bilgi edinmek için bkz. [anahtar konuları](azure-stack-considerations.md) makalesi.

## <a name="cheat-sheet-virtual-machine-differences"></a>Kopya kağıdı: Sanal makine farkları

| Özellik | Azure (Genel) | Azure Stack |
| --- | --- | --- |
| Sanal makine görüntüleri | Azure marketi, sanal makine oluşturmak için kullanabileceğiniz görüntüleri içerir. Bkz: [Azure Marketi](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/compute?subcategories=virtual-machine-images&page=1) Azure Marketi'nde kullanılabilir görüntülerin listesini görüntülemek için sayfa. | Varsayılan olarak, yok herhangi bir görüntü kullanılabilir Azure Stack Market'te. Azure Stack bulut Yöneticisi, yayımlamak veya kullanıcılar kullanabilmeniz için Azure Stack marketini görüntüleri indirin. |
| Sanal makine boyutları | Azure sanal makineler için çok çeşitli boyutlarını destekler. Kullanılabilir boyutlar ve seçenekler hakkında bilgi edinmek için bkz [Windows sanal makine boyutları](../../virtual-machines/virtual-machines-windows-sizes.md) ve [Linux sanal makine boyutları](../../virtual-machines/linux/sizes.md) konuları. | Azure Stack, Azure'da kullanılabilen sanal makine boyutları kümesini destekler. Desteklenen boyut listesini görüntülemek için başvurmak [sanal makine boyutları](#virtual-machine-sizes) bu makalenin. |
| Sanal makine kotaları | [Kota sınırları](../../azure-subscription-service-limits.md#service-specific-limits) Microsoft tarafından ayarlanır | Sanal makineler kullanıcılarıyla sundukları önce Azure Stack bulut Yöneticisi kotaları atamanız gerekir. |
| Sanal makine uzantıları |Azure, çok çeşitli sanal makine uzantıları destekler. Kullanılabilir uzantılar hakkında bilgi için bkz [sanal makine uzantıları ve özellikleri](../../virtual-machines/windows/extensions-features.md) makalesi.| Azure Stack, Azure'da kullanılabilen uzantıları kümesini destekler ve her uzantının sahip belirli sürümler. Azure Stack bulut yönetici kullanıcıları için kullanılabilir yapılması hangi uzantıları seçebilirsiniz. Desteklenen uzantılar listesinde görüntülemek üzere başvurmak [sanal makine uzantıları](#virtual-machine-extensions) bu makalenin. |
| Sanal makine ağı | Kiracı sanal makinesi için atanan genel IP adresleri Internet üzerinden erişilebilir.<br><br><br>Azure sanal makineleri olan sabit bir DNS adı | Bir kiracı sanal makineye atanan genel IP adresleri yalnızca Azure Stack geliştirme Seti'ni ortamı içinden erişilebilir. Bir kullanıcının bu Azure Stack geliştirme Seti'ni erişiminiz olmalıdır [RDP](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop) veya [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) Azure Stack'te oluşturulan bir sanal makineye bağlanmak için.<br><br>Belirli bir Azure Stack örneğinde oluşturulan sanal makineler bulut Yöneticisi tarafından yapılandırılan değere göre bir DNS adına sahip. |
| Sanal makine depolama | Destekler [yönetilen diskler.](../../virtual-machines/windows/managed-disks-overview.md) | Yönetilen diskler, Azure Stack'te 1808 ve üzeri sürümü ile desteklenir. |
| Sanal makine disk performansı | Disk türünü ve boyutuna bağlıdır. | VM diskleri bağlı VM boyutuna bağlıdır başvurmak için [Azure Stack'te desteklenen sanal makine boyutları](azure-stack-vm-sizes.md) makalesi.
| API sürümleri | Azure, her zaman tüm sanal makine özellikleri için en son API sürümleri vardır. | Azure yığını, bu hizmetler için belirli Azure hizmetlerinin ve belirli API sürümlerini destekler. Desteklenen API sürümlerinin listesi görüntülemek için başvurmak [API sürümlerini](#api-versions) bu makalenin. |
| Azure örnek meta veri hizmeti | Azure örnek meta veri hizmeti yönetmek ve sanal makinelerinizi yapılandırmak için kullanılan sanal makine örneklerini çalıştırma hakkında bilgi sağlar.  | Örnek meta veri hizmeti, Azure Stack üzerinde desteklenmiyor. |
|Sanal makine kullanılabilirlik kümeleri|Birden çok hata etki alanları (2 veya 3 bölge başına)<br>Birden çok güncelleştirme etki alanları<br>Yönetilen disk desteği|Birden çok hata etki alanları (2 veya 3 bölge başına)<br>Birden çok güncelleştirme etki alanı (en fazla 20)<br>Yönetilen disk desteği yok|
|Sanal makine ölçek kümeleri|Desteklenen otomatik ölçeklendirme|Otomatik ölçeklendirme desteklenmiyor.<br>Portal, Resource Manager şablonları veya PowerShell kullanarak bir ölçek kümesine daha fazla örnek ekleyin.

## <a name="virtual-machine-sizes"></a>Sanal makine boyutları

Azure Stack uygular (server yerel hem de hizmet düzeyinde.) kaynakların tüketimini önlemek için kaynak sınırları Bu sınırlar, diğer kiracılar tarafından kaynak tüketimine etkisini azaltarak Kiracı deneyimini geliştirmek.

- Sanal makineden ağ çıkışı için yerinde bant genişliği sınırlaması vardır. Azure stack'teki Caps azure'da büyük harfler ile aynıdır.
- Depolama kaynakları için Azure Stack depolama erişimi için kiracılar tarafından kaynakların temel operasyonda ekstra tüketimi önlemek için depolama IOPS limitlerine uygular.
- Birden fazla bağlı veri diskleri olan VM'ler için her veri diskinin en yüksek aktarım 500 IOPS için HDD'ler ve SSD'ler için 2300 IOPS var.

Azure Stack'te kendi yapılandırması ile birlikte desteklenen VM'ler aşağıdaki tabloda listelenmektedir:

| Tür           | Boyut          | Desteklenen boyutlar aralığı |
| ---------------| ------------- | ------------------------ |
|Genel amaçlı |Temel A        |[A0 - A4](azure-stack-vm-sizes.md#basic-a)                   |
|Genel amaçlı |Standart A     |[A0 - A7](azure-stack-vm-sizes.md#standard-a)              |
|Genel amaçlı |D Serisi       |[D1 - D4](azure-stack-vm-sizes.md#d-series)              |
|Genel amaçlı |Dv2 Serisi     |[D1_v2 - D5_v2](azure-stack-vm-sizes.md#ds-series)        |
|Genel amaçlı |DS serisi      |[DS1 - DS4](azure-stack-vm-sizes.md#dv2-series)            |
|Genel amaçlı |DSv2 serisi    |[DS1_v2 - DS5_v2](azure-stack-vm-sizes.md#dsv2-series)      |
|Bellek için iyileştirilmiş|D Serisi       |[D11 - D14](azure-stack-vm-sizes.md#mo-d)            |
|Bellek için iyileştirilmiş|DS serisi      |[DS11 - DS14](azure-stack-vm-sizes.md#mo-ds)|
|Bellek için iyileştirilmiş|Dv2 Serisi     |[D11_v2 - DS14_v2](azure-stack-vm-sizes.md#mo-dv2)     |
|Bellek için iyileştirilmiş|DSv2 serisi-  |[DS11_v2 - DS14_v2](azure-stack-vm-sizes.md#mo-dsv2)    |

Sanal makine boyutları ve bunların ilişkili kaynak miktarları ise, Azure Stack ve Azure arasında tutarlıdır. Bu, bellek, çekirdek sayısı ve sayı/boyutu oluşturulabilmesi için veri disklerinin miktarını içerir. Ancak, aynı boyutta olan sanal makinelerin performans belirli bir Azure Stack ortamının temel özelliklerine bağlıdır.

## <a name="virtual-machine-extensions"></a>Sanal makine uzantıları

 Azure Stack, uzantılar, küçük bir kümesini içerir. Güncelleştirmeleri ve ek uzantı Market dağıtımı kullanılabilir.

Azure Stack ortamınıza kullanılabilir sanal makine uzantıları listesini almak için aşağıdaki PowerShell betiğini kullanın:

```powershell
Get-AzureRmVmImagePublisher -Location local | `
  Get-AzureRmVMExtensionImageType | `
  Get-AzureRmVMExtensionImage | `
  Select Type, Version | `
  Format-Table -Property * -AutoSize
```

## <a name="api-versions"></a>API sürümleri

Azure stack'teki sanal makine özellikleri, aşağıdaki API sürümlerini destekler:

![VM kaynak türleri](media/azure-stack-vm-considerations/vm-resoource-types.png)

Azure Stack ortamınıza kullanılabilen sanal makine özellikleri API sürümlerini almak için aşağıdaki PowerShell betiğini kullanabilirsiniz:

```powershell
Get-AzureRmResourceProvider | `
  Select ProviderNamespace -Expand ResourceTypes | `
  Select * -Expand ApiVersions | `
  Select ProviderNamespace, ResourceTypeName, @{Name="ApiVersion"; Expression={$_}} | `
  where-Object {$_.ProviderNamespace -like “Microsoft.compute”}
```

Desteklenen kaynak türlerini ve API sürümlerini listesi bulut operatörü, Azure Stack ortamınıza yeni bir sürüme güncelleştiriyorsa farklılık gösterebilir.

## <a name="windows-activation"></a>Windows etkinleştirme

Windows ürünlerinin ürün kullanım hakları ve Microsoft Lisans Koşulları'nı uygun olarak kullanılmalıdır. Azure Stack kullanan [otomatik sanal makine etkinleştirmesi](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn303421(v%3dws.11)) Windows Server sanal makineleri (VM'ler) etkinleştirme (AVMA).

- Azure Stack ana bilgisayar Windows AVMA anahtarları ile Windows Server 2016 için etkinleştirir. Windows Server 2012 R2 çalıştıran ya da daha sonra otomatik olarak etkinleştirilmiş tüm VM'ler.
- Windows Server 2012 çalıştıran veya önceki otomatik olarak etkinleştirilmez ve kullanarak etkinleştirilmesi için kullanılması gereken sanal makineleri [MAK etkinleştirmesi](https://technet.microsoft.com/library/ff793438.aspx). MAK etkinleştirmesini kullanmak için kendi ürün anahtarı sağlamanız gerekir.

Microsoft Azure, Windows sanal makineleri etkinleştirme için KMS etkinleştirme kullanır. Azure Stack bir VM'den Azure'a ve karşılaştığınız sorunlar etkinleştirme taşırsanız, bkz. [sorun giderme Azure Windows sanal makine etkinleştirme sorunlarını](https://docs.microsoft.com/azure/virtual-machines/windows/troubleshoot-activation-problems). Ek bilgiler bulunabilir [Azure Vm'leri üzerinde sorun giderme Windows etkinleştirme hatalarını](https://blogs.msdn.microsoft.com/mast/2017/06/14/troubleshooting-windows-activation-failures-on-azure-vms/) Azure destek ekibi Blog Gönderisi.

## <a name="next-steps"></a>Sonraki adımlar

[Azure stack'teki PowerShell ile Windows sanal makine oluşturma](azure-stack-quick-create-vm-windows-powershell.md)
