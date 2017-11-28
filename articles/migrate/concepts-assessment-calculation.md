---
title: "Azure geçirmek değerlendirme hesaplamalarda | Microsoft Docs"
description: "Değerlendirme hesaplamalar Azure geçirmek hizmetindeki genel bir bakış sağlar."
services: migrate
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 39a63769-31eb-49f9-9089-4d3e4e88a412
ms.service: migrate
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/21/2017
ms.author: raynew
ms.openlocfilehash: f00825ff9a5018e67672ce452f01130f84919e52
ms.sourcegitcommit: 5bced5b36f6172a3c20dbfdf311b1ad38de6176a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2017
---
# <a name="assessment-calculations"></a>Değerlendirme hesaplamalar

[Azure geçirme](migrate-overview.md) geçiş Azure için şirket içi iş yüklerini değerlendirir. Bu makalede değerlendirmelerinin nasıl hesaplandığını hakkında bilgi sağlar.



## <a name="overview"></a>Genel Bakış

Bir Azure geçirmek değerlendirme üç aşamadan oluşur. Performans tabanlı boyutlandırma tahminler tarafından izlenen uygunluğu çözümleme, değerlendirme başlar ve son olarak, aylık tahmini maliyet. Öncekinin geçerse bir makine yalnızca bir sonraki aşamaya taşır. Bir makine Azure uygunluğu denetimi başarısız olursa, Azure ve boyutlandırma ve maliyetlendirme hesaplanan olmaz için örneğin, uygun değil olarak işaretlendiğinden. 


## <a name="azure-suitability-analysis"></a>Azure uygunluğu çözümleme

Azure'a geçirmek istediğiniz makinelere Azure gereksinimleri ve sınırlamaları karşılaması gerekir. Örneğin, bir şirket içi VM disk birden fazla 4 TB ise, Azure üzerinde barındırılamaz. Azure uyumluluk denetimleri aşağıdaki tabloda özetlenmiştir. 

**Onay** | **Ayrıntılar**
--- | ---
**Önyükleme türü** | Önyükleme konuk işletim sistemi diski BIOS ve UEFI değil türünde olmalıdır.
**Çekirdek** | Makinelerdeki çekirdek sayısına eşit olmalıdır (veya küçüktür) bir Azure VM için desteklenen en yüksek çekirdek sayısı (32).<br/><br/> Performans geçmişi kullanılabilir durumdaysa, karşılaştırma için kullanılan çekirdek Azure geçirmek göz önünde bulundurur. Rahatlık faktörü değerlendirme ayarlarında belirtilirse, kullanılan çekirdek sayısı rahatlık faktörüyle çarpılır.<br/><br/> Performans geçmişi yok ise, Azure geçirmek rahatlık faktörü uygulamadan ayrılmış çekirdek kullanır.
**Bellek** | Makine bellek boyutu eşit olması gerekir (veya küçüktür) bir Azure VM için izin verilen en fazla belleği (448 GB). <br/><br/> Performans geçmişi kullanılabilir durumdaysa, karşılaştırma için kullanılan bellek Azure geçirmek göz önünde bulundurur. Rahatlık faktörü belirtilirse, kullanılan bellek rahatlık faktörüyle çarpılır.<br/><br/> Geçmiş ise rahatlık faktörü uygulamadan ayrılmış bellek kullanılır.<br/><br/> 
**Windows Server 2003-2008 R2** | 32 bit ve 64-bit desteği.<br/><br/> Azure yalnızca en iyi çaba desteği sağlar.
**Windows Server 2008 R2 ile tüm Sp'ler** | 64-bit desteği.<br/><br/> Azure tam destek sağlar.
**Windows Server 2012 & tüm Sp'ler** | 64-bit desteği.<br/><br/> Azure tam destek sağlar.
**Windows Server 2012 R2 & tüm Sp'ler** | 64-bit desteği.<br/><br/> Azure tam destek sağlar.
**Windows Server 2016 & tüm Sp'ler** | 64-bit desteği.<br/><br/> Azure tam destek sağlar.
**Windows İstemcisi 7 ve üzeri** | 64-bit desteği.<br/><br/> Azure yalnızca Visual Studio abonelik desteği sağlar.
**Linux** | 64-bit desteği.<br/><br/> Azure, bunlar için tam destek sağlar [işletim sistemleri](../virtual-machines/linux/endorsed-distros.md).
**Depolama diskini** | Ayrılmış bir disk boyutunu 4 TB (4096 GB) olması gerekir veya daha az.<br/><br/> Makineye bağlı disk sayısı 65 veya daha az, da dahil olmak üzere işletim sistemi diski olması gerekir. 
**Ağ** | Daha az NIC'ler bağlı veya bir makine 32 olmalıdır.


## <a name="performance-based-sizing"></a>Performans tabanlı boyutlandırma

Bir makine Azure için uygun olarak işaretlendikten sonra Azure geçirmek, aşağıdaki ölçütleri kullanarak azure'da bir VM boyutu eşler:

- **Depolama onay**: Azure geçirmek çalışır bir diski azure'da makineye bağlı her disk eşlemek:-rahatlık faktörüyle çarpar (IOPS) saniye başına g/ç Azure geçirme. Bu ayrıca katları her disk rahatlık faktörüyle (MBps) verimini. Bu, etkin sağlar IOPS ve üretilen iş disk. Bunu temel alarak, Azure geçirmek için standart bir disk veya Azure premium disk eşler.
    - Hizmet bir disk gerekli IOPS ve üretilen iş ile bulamazsanız, Azure için makine uygun değil olarak işaretler.
    - Uygun bir disk kümesi bulursa, Azure geçirmek Depolama artıklık yöntemi ve değerlendirme ayarlarında belirtilen konuma desteği olanları seçer.
    - Birden çok uygun diskler varsa, en düşük maliyeti ile bir seçer.
- **Depolama disk verimliliğini**: [daha fazla bilgi edinin](../azure-subscription-service-limits.md#storage-limits) Azure hakkında disk ve VM sınırlar.
- **Disk türü**: Azure geçirmek, yalnızca yönetilen diskleri destekler.
- **Ağ denetimi**: şirket içi makinede NIC sayısını destekleyen bir Azure VM bulmak Azure geçirmek çalışır.
    - Bunu yapmak için onu / saniye (MBps) makinesi (ağ çıkışı) dışında tüm NIC'ler arasında aktarılan verileri toplar ve toplanan sayıya rahatlık faktörü uygular. Gerekli ağ performansını destekleyebilen bir Azure VM bulmak için kullanılan, bu sayı.
    - Azure geçirme sanal makineden ağ ayarlarını alır ve bir ağ veri merkezi dışında olmasını varsayar.
    - Ağ performans verileri kullanılabilir ise, yalnızca NIC sayısı için VM boyutlandırma olarak kabul edilir.
- **Onay işlem**: depolama ve ağ gereksinimleri hesaplandıktan sonra işlem gereksinimleri Azure geçirmek göz önünde bulundurur:
    - VM için performans verilerini varsa, kullanılan çekirdek ve bellek arar ve rahatlığı faktörü uygular. Bu sayıya göre uygun bir VM boyutu Azure'da bulmaya çalışır.
    - Uygun büyüklük bulunursa, makine uygun Azure için işaretlenir.
    - Uygun bir boyut bulunursa, Azure geçirmek depolama ve ağ hesaplamaları geçerlidir. Ardından, konum ve fiyatlandırma katmanı ayarlarını, son VM boyutu öneri de geçerlidir.


## <a name="monthly-cost-estimation"></a>Aylık maliyet tahmini

Boyutlandırma önerileri tamamlandıktan sonra Azure geçirmek geçiş sonrası işlem ve depolama maliyetleri hesaplar.

- **Maliyet işlem**: önerilen Azure VM boyutu kullanarak, Azure geçirmek faturalama VM için aylık maliyeti hesaplamak için API'sini kullanır. Hesaplama, işletim sistemi, Yazılım Güvencesi, konum ve para birimi ayarları dikkate alır. Bu maliyet aylık toplam işlem maliyetini hesaplamak için tüm makineler arasında toplar.
- **Depolama maliyeti**: bir makine tüm diskleri aylık maliyeti toplayarak hesaplanır maliyeti aylık depolama makineye bağlı. Azure geçirme toplam aylık depolama maliyetleri tüm makinelerin depolama maliyetleri toplayarak hesaplar. Şu anda, hesaplama göz önüne değerlendirme ayarlarında belirtilen teklifleri almaz.

Maliyetleri tahakkuk ayarlarında belirtilen para birimi görüntülenir. 


## <a name="next-steps"></a>Sonraki adımlar

[Şirket içi VMware Vm'leri için bir değerlendirmesi ayarlayın](tutorial-assessment-vmware.md)
