---
title: Microsoft Azure'da barındırılan VM yapılandırmak için Azure Market | Microsoft Docs
description: Boyut, güncelleştirme ve Azure'da barındırılan bir VM'yi Genelleştirme açıklanmaktadır.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 10/19/2018
ms.author: pbutlerm
ms.openlocfilehash: 9cf363bc5f4230306c2fec99eb6287b23e598a4c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60744355"
---
# <a name="configure-the-azure-hosted-vm"></a>Azure'da barındırılan VM yapılandırma

Bu makalede, boyut, güncelleştirme ve bir Azure üzerinde barındırılan sanal makine (VM) generalize açıklanmaktadır.  Bu adımlar, Azure Market'ten dağıtılacak sanal makinenizin hazırlamak gereklidir.


## <a name="sizing-the-vhds"></a>VHD'ler boyutlandırma

<!--TD: Check if the following assertion is true. I didn't understand the original content. -->
Bir işletim sistemi (ve isteğe bağlı olarak ek hizmetler) ile önceden yapılandırılmış sanal makinelerin seçtiniz sonra standart bir Azure VM boyutu açıklandığı zaten seçtiniz [sanal makine SKU'ları sekmesini](./cpp-skus-tab.md).  Çözümünüzü önceden yapılandırılmış bir işletim sistemi ile başlayarak önerilen bir yaklaşımdır.  Bir işletim sistemi elle yüklüyorsanız, ancak daha sonra birincil VHD'nizi VM görüntünüzdeki boyut gerekir:

- Windows için işletim sistemi VHD'si 127-128 GB olarak oluşturulması gereken sabit biçimli VHD. 
- 30-50 GB olarak Linux için bu VHD oluşturulması gereken sabit biçimli VHD.

Fiziksel boyut 128 127 GB'den az ise, VHD seyrek olmalıdır. Sağlanan temel Windows ve SQL Server görüntüleri, zaten bu gereksinimleri karşılayan, bu nedenle biçimi veya alınan VHD'nin boyutunu değiştirmeyin. 

Veri diskleri en fazla 1 TB büyüklüğünde olabilir. Kendi boyutuna karar verirken, müşteriler dağıtım sırasında bir görüntü içindeki VHD'leri boyutlandıramayacağını unutmayın. Veri diski VHD'leri sabit biçimli VHD olarak oluşturulmalıdır. Bunlar aynı zamanda seyrek olmalıdır. Veri diskleri, başlangıçta boş olabilir veya veri içerebilir.


## <a name="install-the-most-current-updates"></a>En son güncelleştirmeleri yükleyin

İşletim sistemi makinelerinin temel görüntüler, yayımlanan en son güncelleştirmeleri içerir. İşletim sistemi VHD'si, oluşturduğunuz yayımlamadan önce tüm en son güvenlik ve Bakım yamalarla işletim sistemi ve yüklü tüm hizmetlerin güncelleştirdiğinizden emin olun.

Windows Server 2016 için çalıştırma **Güncelleştirmeleri denetle** komutu.  Aksi takdirde, daha eski Windows sürümleri için bkz. [güncelleştirme Windows Update aracılığıyla alma](https://support.microsoft.com/help/3067639/how-to-get-an-update-through-windows-update).  Windows update son kritik ve önemli güvenlik güncelleştirmelerini otomatik olarak yükler.

Linux dağıtımları için güncelleştirmeleri yaygın olarak indirilir ve komut satırı aracını veya grafik programı yüklenir.  Örneğin, Ubuntu Linux sağlar [apt-get](https://manpages.ubuntu.com/manpages/cosmic/man8/apt-get.8.html) komut ve [güncelleştirme Yöneticisi](https://manpages.ubuntu.com/manpages/cosmic/man8/update-manager.8.html) işletim sistemini güncelleştirmek için aracı.


## <a name="perform-additional-security-checks"></a>Ek güvenlik denetimleri gerçekleştirme

Azure marketi'ndeki çözüm görüntüleriniz için yüksek düzeyde güvenlik korumanız gerekir.  Aşağıdaki makaleye listesi güvenlik yapılandırmalarını ve bu hedef yardımcı olacak yordamlar sağlar: [Azure Market görüntüleri için güvenlik önerilerini](https://docs.microsoft.com/azure/security/security-recommendations-azure-marketplace-images).  Bu önerilerden bazıları, Linux tabanlı görüntüler için özeldir, ancak en herhangi bir VM görüntüsü için geçerlidir. 


## <a name="perform-custom-configuration-and-scheduled-tasks"></a>Özel yapılandırma ve zamanlanmış görevleri gerçekleştirme

Ek yapılandırma gerekirse, önerilen yaklaşım dağıtıldıktan sonra VM son değişiklikleri yapmak için başlangıçta çalışan bir zamanlanmış görev kullanmaktır.  Ayrıca, aşağıdaki önerileri göz önünde bulundurun:
- Bir kere çalıştırılacak görev etkinleştirilmişse, başarıyla tamamlandıktan sonra görev kendisini silmeniz önerilir.
- Yapılandırmaları değil Bel sürücüler dışında C ya da D, yalnızca bu iki sürücüleri olduğundan her zaman mevcut garanti edilir. C sürücüsünün işletim sistemi diskidir ve D sürücüsünü geçici yerel disktir.

Linux özelleştirmeleri hakkında daha fazla bilgi için bkz. [Linux yönelik sanal makine uzantılarına ve özelliklerine](https://docs.microsoft.com/azure/virtual-machines/extensions/features-linux).


## <a name="generalize-the-image"></a>Görüntüyü genelleştirme

Azure Marketi'ndaki tüm görüntüler genel bir şekilde yeniden kullanılabilir olmalıdır. Bu kullanılabilirlik elde etmek için işletim sistemi VHD'si olmalıdır *genelleştirilmiş*, bir VM'den tüm örneğe özel tanımlayıcıları ve yazılım sürücüleri kaldıran bir işlem.

### <a name="windows"></a>Windows

Windows işletim sistemi diskleri ile genelleştirilmiş [sysprep aracını](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview). Sysprep, sonradan güncelleştirmesi veya işletim sistemini yeniden çalıştırmanız gerekir. 

> [!WARNING]
>  Sysprep'i çalıştırın, sonra güncelleştirmeleri otomatik olarak çalıştırılabilir olduğundan VM'nin dağıtıldıktan kadar açmak gerekir.  Bu kapatma VHD işletim sistemine özgü örneği değişiklik yapmasını gerçekleştirilen sonraki güncelleştirmelerle kaçınır veya hizmetleri yüklendi.

Sysprep çalıştırma hakkında daha fazla bilgi için bkz. [VHD genelleştirmek için adımları](https://docs.microsoft.com/azure/virtual-machines/windows/prepare-for-upload-vhd-image#steps-to-generalize-a-vhd)

### <a name="linux"></a>Linux

İki aşamalı işlemi izleyerek bir Linux VM'yi Genelleştirme ve yeniden ayrı VM olarak dağıtma.  Daha fazla bilgi için bkz. [Sanal makine veya VHD görüntüsü oluşturma](../../../virtual-machines/linux/capture-image.md). 

#### <a name="remove-the-azure-linux-agent"></a>Azure Linux aracısı kaldırma
1.  Bir SSH istemcisi kullanarak Linux VM'nize bağlanın.
2.  SSH penceresinde aşağıdaki komutu yazın: <br/>
    `sudo waagent -deprovision+user`
3.  Tür `y` devam etmek için. (Ekleyebileceğiniz `-force` önceki komutuna parametre doğrulama adımını kaçının.)
4.  Komut tamamlandıktan sonra yazın `exit` SSH İstemcisi'ni kapatın.

<!-- TD: I need to add meat and/or references to the following steps -->
#### <a name="capture-the-image"></a>Yansıma Yakalama
1.  Azure portalına gidin, kaynak grubunuzu (RG) seçin ve VM ayırmayı.
2.  VHD'nizi genelleştirilmiş olduğundan ve bu VHD kullanarak yeni bir VM oluşturabilirsiniz.


## <a name="create-one-or-more-copies"></a>Bir veya birden çok kopya oluşturun

VM kopyaları oluşturmak genellikle bir çözüm farklı yapılandırmaları sunar ve benzeri için yedekleme, test, özelleştirilmiş yük devretme veya Yük Dengeleme için yararlıdır. Yinelenen ve yönetilmeyen bir kopya yapmak için birincil bir VHD indirme hakkında bilgi için bkz:

- Linux VM: [Azure'da Linux VHD'si indirin](../../../virtual-machines/linux/download-vhd.md)
- Windows VM: [Azure'dan bir Windows VHD indirme](../../../virtual-machines/windows/download-vhd.md)


## <a name="next-steps"></a>Sonraki adımlar

Sanal makinenizin yapılandırdıktan sonra hazır olduğunuz [bir sanal sabit diskten bir sanal makine dağıtma](./cpp-deploy-vm-vhd.md).
