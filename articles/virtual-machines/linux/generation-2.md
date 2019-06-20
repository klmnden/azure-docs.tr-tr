---
title: 2\. nesil VM'ler (Önizleme) için Azure desteği | Microsoft Docs
description: 2\. kuşak VM'ler için Azure desteği'ne genel bakış
services: virtual-machines-linux
documentationcenter: ''
author: laurenhughes
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/23/2019
ms.author: lahugh
ms.openlocfilehash: 352df275742c38307065252d2f65bb4253d78e5d
ms.sourcegitcommit: 6e6813f8e5fa1f6f4661a640a49dc4c864f8a6cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67151270"
---
# <a name="support-for-generation-2-vms-preview-on-azure"></a>Oluşturma için 2 VM'ler (Önizleme) Azure üzerinde destekler.

> [!IMPORTANT]
> 2\. kuşak VM'ler için Azure desteği, şu anda Önizleme aşamasındadır. Bu önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanmaktadır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için [Microsoft Azure önizlemeleri için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). 

2\. nesil sanal makineler (VM) için destek Azure önizlemede kullanıma sunulmuştur. Oluşturduktan sonra bir sanal makinenin oluşturulması değiştirmek olamaz, bu nedenle bir nesil seçmeden önce bu sayfadaki konuları gözden geçirin. 

2\. nesil Vm'leri 1. kuşak Vm'lerde desteklenmeyen anahtar özelliklerini destekler. Bu özellikler, artan bellek, Intel yazılım koruma Uzantıları (Intel SGX) içerir ve sanallaştırılmış kalıcı bellek (vPMEM). 2\. kuşak Vm'leri Azure'da henüz desteklenmeyen bazı özellikler de var. Daha fazla bilgi için [özellikleri ve yetenekleri](#features-and-capabilities) bölümü.

BIOS tabanlı mimari nesil 1 VM'ler kullanılan yerine 2. kuşak Vm'lerde yeni önyükleme UEFI tabanlı mimari kullanın. 1\. kuşak sanal makinelere kıyasla, 2. kuşak Vm'lerde önyükleme ve yükleme sürelerini iyileştirir. 2\. kuşak VM'ler için genel bir bakış ve 1. nesil ve 2. nesil farklılıklardan bazıları için bkz: [Hyper-V'de 1 veya 2. kuşak sanal makine oluşturmalısınız?](https://docs.microsoft.com/windows-server/virtualization/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v).

## <a name="generation-2-vm-sizes"></a>2\. nesil VM boyutları

1\. kuşak sanal makineleri azure'da tüm VM boyutları tarafından desteklenir. Azure, artık aşağıdaki seçili VM serisi için Önizleme 2. nesil destek sunar:

* [Dsv2 serisi](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-general#dsv2-series) ve [Dsv3 serisi](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-general#dsv3-series-1)
* [Esv3 serisi](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory#esv3-series)
* [Fsv2 serisi](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-compute#fsv2-series-1)
* [GS serisi](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory#gs-series)
* [Ls serisi](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-previous-gen#ls-series) ve [Lsv2 serisi](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-storage#lsv2-series)
* [Mv2 serisi](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory#mv2-series)

## <a name="generation-2-vm-images-in-azure-marketplace"></a>2\. nesil sanal makine görüntüleri Azure Marketi'nde

2\. kuşak Vm'lerde aşağıdaki Market görüntüleri destekler:

* Windows Server 2019 veri merkezi
* Windows Server 2016 Datacenter
* Windows Server 2012 R2 Datacenter
* Windows Server 2012 Datacenter

## <a name="on-premises-vs-azure-generation-2-vms"></a>Şirket içi vs. 2. kuşak Azure Vm'leri

Azure, Hyper-V desteklediği için 2. nesil VM'ler şirket içinde özelliklerinden bazıları şu anda desteklemiyor.

| 2\. nesil özelliği                | Şirket içi Hyper-V | Azure |
|-------------------------------------|---------------------|-------|
| Güvenli Önyükleme                         | :heavy_check_mark:  | : x:.   |
| Korumalı VM                         | :heavy_check_mark:  | : x:.   |
| vTPM                                | :heavy_check_mark:  | : x:.   |
| Sanallaştırma tabanlı güvenlik (VBS) | :heavy_check_mark:  | : x:.   |
| VHDX biçimi                         | :heavy_check_mark:  | : x:.   |

## <a name="features-and-capabilities"></a>Özellikler ve yetenekler

### <a name="generation-1-vs-generation-2-features"></a>1\. kuşak ve 2. nesil özellikleri

| Özellik | 1\. nesil | 2\. nesil |
|---------|--------------|--------------|
| Önyükleme             | PCAT                      | UEFI                               |
| Disk denetleyicileri | IDE                       | SCSI                               |
| VM boyutları         | Tüm VM boyutları | Premium depolamayı destekleyen VM'ler |

### <a name="generation-1-vs-generation-2-capabilities"></a>1\. kuşak ve 2. nesil özellikleri

| Özellik | 1\. nesil | 2\. nesil |
|------------|--------------|--------------|
| İşletim sistemi diski > 2 TB                    | : x:.                        | :heavy_check_mark: |
| Özel disk/görüntü/takas işletim sistemi         | :heavy_check_mark:         | :heavy_check_mark: |
| Sanal makine ölçek kümesi desteği | :heavy_check_mark:         | :heavy_check_mark: |
| ASR/yedekleme                        | :heavy_check_mark:         | : x:.                |
| Paylaşılan görüntü Galerisi              | :heavy_check_mark:         | : x:.                |
| Azure disk şifrelemesi             | :heavy_check_mark:         | : x:.                |

## <a name="creating-a-generation-2-vm"></a>Oluşturma 2. nesil VM

### <a name="marketplace-image"></a>Market görüntüsü

Azure portal veya Azure CLI, nesil 2 VM'ler UEFI önyükleme destekleyen bir Market görüntüsünden oluşturabilirsiniz.

`windowsserver-gen2preview` Teklifi yalnızca Windows 2. nesil görüntüleri içerir. Bu paket, 1. kuşak ve 2. nesil görüntüleri arasındaki karışıklıkları önler. 2\. nesil oluşturmak için VM seçin **görüntüleri** Bu teklif ve VM'yi oluşturmak için standart işlemini izleyin.

Şu anda Market, aşağıdaki Windows nesil 2 görüntüleri sunar:

* 2019-datacenter-gen2
* 2016-datacenter-gen2
* 2012-r2-datacenter-gen2
* 2012-datacenter-gen2

Bkz: [özellikleri ve yetenekleri](#features-and-capabilities) bölümünü desteklenen Market görüntüleri için geçerli bir listesi.

### <a name="managed-image-or-managed-disk"></a>Yönetilen bir görüntü veya yönetilen disk

2\. nesil oluşturabileceğiniz bir yönetilen bir görüntü veya yönetilen disk oluşturma 1. nesil aynı şekilde VM'den VM.

### <a name="virtual-machine-scale-sets"></a>Sanal makine ölçek kümeleri

Ayrıca, sanal makine ölçek kümeleri'ni kullanarak nesil 2 VM'ler oluşturabilirsiniz. Azure CLI, nesil 2 VM'ler oluşturmak için ayarlar Azure ölçek kullanın.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

* **Nesil 2 VM'ler kullanılabilir tüm Azure bölgelerinde misiniz?**  
    Evet. Ancak tüm [2. nesil VM boyutları](#generation-2-vm-sizes) her bölgede kullanılabilir. Kullanılabilirlik nesil 2 VM ve VM boyutunu kullanılabilirliğine bağlıdır.

* **2 VM nesil 1 ve nesil arasında bir fiyat fark var mı?**  
    Hayır.

* **İşletim sistemi disk boyutunu nasıl artırabilirim?**  
  İşletim sistemi diskleri 2 TB'tan büyük yeni nesil 2 VM'ler. Varsayılan olarak, işletim sistemi diskleri 2. kuşak VM'ler için 2 TB değerinden küçük. Disk boyutunu önerilen en çok 4 TB'a kadar artırabilirsiniz. İşletim sistemi disk boyutunu artırmak için Azure CLI veya Azure Portalı'nı kullanın. Diskleri program aracılığıyla genişletme hakkında daha fazla bilgi için bkz: [bir diski yeniden boyutlandırma](expand-disks.md).

  Azure portalından işletim sistemi disk boyutunu artırmak için:

  1. Azure portalında sanal makine özellikleri sayfasına gidin.
  1. VM'yi serbest bırakın ve kapatmak için **Durdur** düğmesi.
  1. İçinde **diskleri** bölümünde, istediğiniz artırmak için işletim sistemi diskini seçin.
  1. İçinde **diskleri** bölümünden **yapılandırma**ve güncelleştirme **boyutu** istediğiniz değer.
  1. Sanal makine özellikleri sayfasına geri gidin ve **Başlat** VM.

  İşletim sistemi disk 2 TB'tan büyük bir uyarı görebilirsiniz. Uyarı 2. kuşak VM'ler için geçerli değildir. Ancak, işletim sistemi disk boyutu 4 TB'den büyük olan *önerilmez.*

* **Nesil 2 sanal makineleri destek hızlandırılmış musunuz?**  
    Evet. Daha fazla bilgi için [hızlandırılmış ağ ile VM oluşturma](../../virtual-network/create-vm-accelerated-networking-cli.md).

* **VHDX, nesil 2 destekleniyor mu?**  
    Hayır, yalnızca VHD 2. kuşak Vm'leri destekler.

* **2. nesil Vm'leri Azure Ultra Yüksek Disk Depolama destekliyor mu?**  
    Evet.

* **Nesil 2 sanal makine nesil 1 ' geçirebilirim?**  
    Hayır, bir sanal makine nesli, oluşturduktan sonra değiştiremezsiniz. VM Nesilleri arasında geçiş yapmak istiyorsanız farklı nesil yeni bir VM oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

* Hakkında bilgi edinin [Hyper-V 2. nesil sanal makinelerde](https://docs.microsoft.com/windows-server/virtualization/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v).
