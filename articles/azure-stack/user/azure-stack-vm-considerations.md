---
title: Farkları ve sanal makineleri Azure yığınında dikkate alınacak noktalar | Microsoft Docs
description: Farklar ve konuları Azure yığınında sanal makinelerle çalışırken öğrenin.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 6613946D-114C-441A-9F74-38E35DF0A7D7
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2018
ms.author: brenduns
ms.openlocfilehash: 83a0b8ff040425ac30cff96936f2f639fd1b5643
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
---
# <a name="considerations-for-using-virtual-machines-in-azure-stack"></a>Sanal makineleri Azure yığınında kullanma konuları

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Azure yığın sanal makineler, isteğe bağlı, ölçeklenebilir bilgi işlem kaynakları sağlar. Sanal makineler (VM'ler) dağıtmadan önce sanal makine özellikleri Azure yığınında kullanılabilir ve Microsoft Azure arasındaki farkları anlamak gerekir. Bu makalede bu farklılıkları açıklar ve sanal makine dağıtımlarının planlaması için önemli noktalar tanımlar. Azure yığını ve Azure arasında üst düzey farklılıklar hakkında bilgi edinmek için [anahtar konuları](azure-stack-considerations.md) makalesi.

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
|Sanal makine kullanılabilirlik kümeleri|Birden çok hata etki alanlarını (2 veya 3 bölge başına)<br>Birden çok güncelleştirme etki alanı<br>Yönetilen disk desteği|Birden çok hata etki alanlarını (2 veya 3 bölge başına)<br>Birden çok güncelleştirme etki alanı (en fazla 20)<br>Yönetilen disk desteği yok|
|Sanal makine ölçek kümeleri|Otomatik ölçek desteklenen|Otomatik-ölçek desteklenmiyor.<br>Daha fazla örnek portal, Resource Manager şablonları veya PowerShell kullanılarak ayarlanan bir ölçek ekleyin.

## <a name="virtual-machine-sizes"></a>Sanal makine boyutları

Azure yığın kaynaklarının (sunucu yerel ve hizmet düzeyi.) kullanımını önlemek için kaynak sınırları uygular Bu sınırlar, diğer kiracılar tarafından kaynak tüketimini etkisini azaltarak Kiracı deneyimini geliştirmek.

- Sanal makineden ağ çıkışı için yerinde bant genişliği vardır. Azure yığınında Caps caps Azure ile aynıdır.
- Depolama kaynakları için Azure yığın depolama erişimi için kiracılar tarafından kaynakların temel operasyonda ekstra tüketimi önlemek için depolama IOPS sınırları uygular.
- Birden çok ekli veriler diske sahip VM'ler için her veri diski en büyük verimi HHDs için 500 IOPS, SSD için 2300 IOPS ise.

Aşağıdaki tabloda Azure yığında yapılandırmalarını yanı sıra desteklenen VM'ler listelenmektedir:

| Tür           | Boyut          | Desteklenen boyutlar aralığı |
| ---------------| ------------- | ------------------------ |
|Genel amaçlı |Temel A        |[A0 - A4](azure-stack-vm-sizes.md#basic-a)                   |
|Genel amaçlı |Standart bir     |[A0 - A7](azure-stack-vm-sizes.md#standard-a)              |
|Genel amaçlı |D Serisi       |[D1 - D4](azure-stack-vm-sizes.md#d-series)              |
|Genel amaçlı |Dv2 Serisi     |[D1_v2 - D5_v2](azure-stack-vm-sizes.md#ds-series)        |
|Genel amaçlı |DS serisi      |[DS1 - DS4](azure-stack-vm-sizes.md#dv2-series)            |
|Genel amaçlı |DSv2 serisi    |[DS1_v2 - DS5_v2](azure-stack-vm-sizes.md#dsv2-series)      |
|Bellek için iyileştirilmiş|D Serisi       |[D11 - D14](azure-stack-vm-sizes.md#mo-d)            |
|Bellek için iyileştirilmiş|DS serisi      |[DS11 - DS14](azure-stack-vm-sizes.md#mo-ds)|
|Bellek için iyileştirilmiş|Dv2 Serisi     |[D11_v2 - DS14_v2](azure-stack-vm-sizes.md#mo-dv2)     |
|Bellek için iyileştirilmiş|DSv2 serisi-  |[DS11_v2 - DS14_v2](azure-stack-vm-sizes.md#mo-dsv2)    |

Sanal makine boyutları ve bunların ilişkili kaynak miktarları Azure yığını ve Azure arasında tutarlı değil. Bu bellek, çekirdek sayısı ve oluşturulabilmesi için veri diski sayısı/boyutunu miktarını içerir. Ancak, VM'lerin performansını aynı boyutta ile belirli bir Azure yığın ortamda temel özelliklerine bağlıdır.

## <a name="virtual-machine-extensions"></a>Sanal makine uzantıları

 Azure yığın uzantıları, küçük bir kümesini içerir. Güncelleştirmeleri ve ek uzantıları Market dağıtımı kullanılabilir.

Azure yığın ortamınızda kullanılabilen sanal makine uzantıları listesini almak için aşağıdaki PowerShell betiğini kullanın:

```powershell
Get-AzureRmVmImagePublisher -Location local | `
  Get-AzureRmVMExtensionImageType | `
  Get-AzureRmVMExtensionImage | `
  Select Type, Version | `
  Format-Table -Property * -AutoSize
```

## <a name="api-versions"></a>API sürümleri

Sanal makine özellikleri Azure yığınında aşağıdaki API sürümlerini destekler:

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

## <a name="windows-activation"></a>Windows etkinleştirme

Windows ürünlerinin ürün kullanım hakları ve Microsoft Lisans Koşulları'nı uygun olarak kullanılmalıdır. Azure yığınını kullanan [otomatik VM etkinleştirme](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn303421(v%3dws.11)) Windows Server sanal makineleri (VM'ler) etkinleştirme (AVMA).

- Azure yığın ana bilgisayar Windows AVMA anahtarları ile Windows Server 2016 için etkinleştirir. Windows Server 2012 çalıştıran ya da daha sonra otomatik olarak etkinleştirilen tüm sanal makineleri.
- Çalışma Windows Server 2008 R2'in otomatik olarak etkinleştirilmez ve kullanarak etkinleştirilmelidir VM'ler [MAK etkinleştirmesi](https://technet.microsoft.com/library/ff793438.aspx).

Microsoft Azure Windows sanal makineleri etkinleştirme için KMS etkinleştirme kullanır. Bir VM Azure yığından Azure ve karşılaştığınız sorunları etkinleştirme taşırsanız, bkz: [sorun giderme Azure Windows sanal makine etkinleştirme sorunlarını](https://docs.microsoft.com/azure/virtual-machines/windows/troubleshoot-activation-problems). Ek bilgiler bulunabilir [sorun giderme Windows etkinleştirme hataları Azure vm'lerinde](https://blogs.msdn.microsoft.com/mast/2017/06/14/troubleshooting-windows-activation-failures-on-azure-vms/) Azure destek ekibi Blog Gönderisi.

## <a name="next-steps"></a>Sonraki adımlar

[Azure yığınında PowerShell ile Windows sanal makine oluşturma](azure-stack-quick-create-vm-windows-powershell.md)