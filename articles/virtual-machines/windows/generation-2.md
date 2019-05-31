---
title: Azure üzerinde 2. nesil VM'ler (Önizleme) | Microsoft Docs
description: 2. nesil Azure Vm'lere genel bakış
services: virtual-machines-windows
documentationcenter: ''
author: laurenhughes
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/23/2019
ms.author: lahugh
ms.openlocfilehash: 1dcc0d3a652ccbf365a18ce734a54dc78515b1a7
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66388363"
---
# <a name="generation-2-vms-preview-on-azure"></a>Azure üzerinde 2. nesil VM'ler (Önizleme)

> [!IMPORTANT]
> 2. nesil sanal makineleri şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

2. nesil sanal makineler (VM) için destek Azure üzerinde genel önizlemeye sunuldu. Bir sanal makinenin oluşturulması oluşturduktan sonra değiştiremezsiniz. Bu nedenle, konuları gözden geçirmenizi öneririz [burada](https://docs.microsoft.com/windows-server/virtualization/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v) nesil seçmeden önce bu sayfadaki bilgilerinin yanı sıra.

1. kuşak Vm'lerde gibi desteklenmeyen 2. nesil Vm'leri destek anahtar özellikleri: daha fazla bellek, Intel® yazılım koruma Uzantıları (SGX) ve sanal kalıcı bellek (vPMEM). 2. kuşak Vm'leri Azure'da henüz desteklenmeyen bazı özellikler de var. Daha fazla bilgi için [özellikleri ve yetenekleri](#features-and-capabilities) bölümü.

2. nesil Vm'leri 1. kuşak sanal makineleri tarafından kullanılan BIOS tabanlı mimari yeni önyükleme UEFI tabanlı mimari vs kullanın. 1. kuşak sanal makinelere kıyasla, 2. kuşak Vm'lerde önyükleme ve yükleme sürelerini iyileştirir. 2. kuşak VM'ler için genel bir bakış ve 1. nesil ve 2. nesil arasındaki önemli farklılıkları bazıları için bkz: [Hyper-V'de 1 veya 2. kuşak sanal makine oluşturmalısınız?](https://docs.microsoft.com/windows-server/virtualization/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v).

## <a name="generation-2-vm-sizes"></a>2. nesil VM boyutları

1. kuşak sanal makineleri azure'da tüm VM boyutları tarafından desteklenir. Azure, artık genel önizlemede aşağıdaki seçili VM serisi 2. nesil desteği sunar:

* [Dsv2](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-general#dsv2-series) ve [Dsv3 serisi](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-general#dsv3-series-1)
* [Esv3 serisi](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-memory#esv3-series)
* [Fsv2 serisi](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-compute#fsv2-series-1)
* [GS serisi](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-memory#gs-series)
* [Ls serisi](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-previous-gen#ls-series) ve [Lsv2 serisi](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-storage#lsv2-series)
* [Mv2 serisi](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-memory#mv2-series)

## <a name="generation-2-vm-images-in-azure-marketplace"></a>2. nesil sanal makine görüntüleri Azure Marketi'nde

2. kuşak Vm'lerde aşağıdaki Azure Market görüntüleri destekler:

* Windows Server 2019 veri merkezi
* Windows Server 2016 Datacenter
* Windows Server 2012 R2 Datacenter
* Windows Server 2012 Datacenter

## <a name="on-premises-vs-azure-generation-2-vms"></a>Şirket içi vs 2. kuşak Azure Vm'leri

Hyper-V destekler 2. kuşak Vm'lerde şirket özelliklerinden bazıları Azure şu anda desteklemiyor.

| 2. nesil özelliği                | Şirket içi Hyper-V | Azure |
|-------------------------------------|---------------------|-------|
| Güvenli Önyükleme                         | :heavy_check_mark:  | : x:.   |
| Korumalı VM                         | :heavy_check_mark:  | : x:.   |
| vTPM                                | :heavy_check_mark:  | : x:.   |
| Sanallaştırma tabanlı güvenlik (VBS) | :heavy_check_mark:  | : x:.   |
| VHDX biçimi                         | :heavy_check_mark:  | : x:.   |

## <a name="features-and-capabilities"></a>Özellikler ve yetenekler

### <a name="generation-1-vs-generation-2-features"></a>1. kuşak ve 2. nesil özellikleri

| Özellik | 1. nesil | 2. nesil |
|---------|--------------|--------------|
| Önyükleme             | PCAT                      | UEFI                               |
| Disk denetleyicileri | IDE                       | SCSI                               |
| VM Boyutları         | Tüm VM boyutları kullanılabilir | Premium depolama Vm'leri yalnızca desteklenir |

### <a name="generation-1-vs-generation-2-capabilities"></a>1. kuşak ve 2. nesil özellikleri

| Özellik | 1. nesil | 2. nesil |
|------------|--------------|--------------|
| İşletim sistemi diski > 2 TB                    | : x:.                        | :heavy_check_mark: |
| Özel görüntü/Disk/takas işletim sistemi         | :heavy_check_mark:         | :heavy_check_mark: |
| Sanal makine ölçek kümesi desteği | :heavy_check_mark:         | :heavy_check_mark: |
| ASR/yedekleme                        | :heavy_check_mark:         | : x:.                |
| Paylaşılan Görüntü Galerisi              | :heavy_check_mark:         | : x:.                |
| Azure Disk Şifrelemesi             | :heavy_check_mark:         | : x:.                |

## <a name="creating-a-generation-2-vm"></a>Oluşturma 2. nesil VM

### <a name="marketplace-image"></a>Market görüntüsü

2. kuşak Vm'leri (UEFI önyükleme destekleyen) Market görüntüsünden Azure portal veya Azure CLI aracılığıyla oluşturulabilir.

`windowsserver-gen2preview` Teklifi yalnızca Windows 2. nesil görüntüleri içerir. Bu, 1. kuşak ve 2. nesil görüntüleri bakımından karışıklığı ortadan kaldırır. Nesil 2 VM'ler oluşturmak için seçin **görüntüleri** Bu teklif ve standart VM oluşturma işlemini izleyin.

Şu anda, aşağıdaki Windows nesil 2 görüntüleri Azure Market'te yayımlanan:

* 2019-datacenter-gen2
* 2016-datacenter-gen2
* 2012-r2-datacenter-gen2
* 2012-datacenter-gen2

2. nesil destekleyen ek görüntüleri ekleme devam edeceğiz gibi desteklenen Market görüntüleri listesi için özellikler bölümünü inceleyin.

### <a name="managed-image-or-managed-disk"></a>Yönetilen bir görüntü veya yönetilen disk

Nesil 2 VM'ler oluşturulabilir yönetilen bir görüntü veya yönetilen disk oluşturma 1. nesil aynı şekilde VM.

### <a name="virtual-machine-scale-sets"></a>Sanal makine ölçek kümeleri

Nesil 2 sanal makineleri aynı zamanda sanal makine ölçek kümeleri kullanılarak oluşturulabilir. Nesil 2 VM'ler Azure CLI aracılığıyla Azure sanal makine ölçek kümeleri kullanarak oluşturabilirsiniz.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

* **Nesil 2 VM'ler kullanılabilir tüm Azure bölgelerinde misiniz?**  
    Evet; Ancak, tüm [2. nesil VM boyutları](#generation-2-vm-sizes) her bölgede kullanılabilir. Kullanılabilirlik nesil 2 VM ve VM boyutunu kullanılabilirliğine bağlıdır.

* **2 VM nesil 1 ve nesil arasında bir fiyat fark var mı?**  
    1. kuşak ve 2. nesil VM'ler arasında fiyatlandırmasındaki fark yoktur.

* **İşletim sistemi disk boyutunu nasıl artırabilirim?**  
  İşletim sistemi diskleri 2 TB'tan büyük yeni nesil 2 VM'ler. Varsayılan olarak, çoğu işletim sistemi diskleri 2. kuşak VM'ler için 2 TB'den az olduğunu, ancak disk boyutunu önerilen maksimum 4 TB'a kadar artırılabilir. Azure CLI veya Azure Portalı aracılığıyla işletim sistemi disk boyutunu artırabilirsiniz. Programlı olarak genişletilen diskler hakkında daha fazla bilgi için bkz: [bir diski yeniden boyutlandırma](expand-os-disk.md).

  Azure portal aracılığıyla işletim sistemi disk boyutunu artırmak için:

  * Azure portalında sanal makine özellikleri sayfasına gidin.

  * Kapat ve kullanarak VM'yi **Durdur** düğme Azure portalında.

  * İçinde **diskleri** bölümünde, artırmak için istediğiniz işletim sistemi diskini seçin.

  * Seçin **yapılandırma** içinde **diskleri** bölümü ve güncelleştirme **boyutu** istediğiniz değer.

  * Sanal makine özellikleri sayfasına geri dönün ve **Başlat** VM.
  
  İşletim sistemi diskleri 2 TB'tan büyük için bir uyarı görebilirsiniz. Uyarı 2. kuşak VM'ler için geçerli değildir; Ancak, işletim sistemi disk boyutu 4 TB'den büyük olan **önerilmez.**

* **2. nesil Vm'leri, hızlandırılmış ağ destekliyor mu?**  
    Evet, 2. nesil VM'lerin desteklediği [hızlandırılmış ağ](../../virtual-network/create-vm-accelerated-networking-cli.md).

* **.Vhdx, nesil 2 destekleniyor mu?**  
    Hayır, yalnızca .vhd 2. kuşak Vm'lerde desteklenir.

* **2. nesil Vm'leri Ultra katı hal sürücüleri (SSD) destekliyor mu?**  
    Evet, 2. kuşak Vm'lerde Ultra SSD destekler.

* **2. kuşak VM'ler için nesil 1 ' geçirebilir miyim?**  
    Hayır, oluşturduktan sonra bir sanal makinenin nesli değiştiremezsiniz. VM Nesilleri arasında geçiş yapmak ihtiyacınız varsa, yeni bir sanal makine farklı bir nesil oluşturmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [Hyper-V 2. nesil sanal makinelerde](https://docs.microsoft.com/windows-server/virtualization/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v).

* Bilgi nasıl [VHD'yi hazırlama](prepare-for-upload-vhd-image.md) şirket içinden azure'a karşıya yükleme.
