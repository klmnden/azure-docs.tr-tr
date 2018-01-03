---
title: "Hyper-V çoğaltma Azure Site kurtarma ikincil site mimarisinin | Microsoft Docs"
description: "Bu makalede, Azure Site Recovery ile ikincil System Center VMM sitesine şirket içi Hyper-V VM’lerini çoğaltma mimarisine genel bir bakış sunulmaktadır."
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 12/19/2017
ms.author: raynew
ms.openlocfilehash: 3380d189518f811ca6cf628608a253e5d93b2730
ms.sourcegitcommit: c87e036fe898318487ea8df31b13b328985ce0e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2017
---
# <a name="hyper-v-replication-to-a-secondary-site"></a>İkincil bir siteye Hyper-V çoğaltma

Bu makalede, Azure portalından [Azure Site Recovery](site-recovery-overview.md) hizmeti kullanılarak, System Center Virtual Machine Manager (VMM) bulutlarındaki Hyper-V sanal makinelerini (VM) ikincil bir VMM sitesine çoğaltırken kullanılan bileşenler ve işlemler açıklanmaktadır.


## <a name="architectural-components"></a>Mimari bileşenler

Aşağıdaki tablo ve grafik ikincil bir siteye Hyper-V çoğaltma için kullanılan bileşenler üst düzey bir görünümünü sağlar.

**Bileşen** | **Gereksinim** | **Ayrıntılar**
--- | --- | ---
**Azure** | Azure aboneliği | VMM konumları arasında çoğaltmayı düzenleyip yönetmek için Azure aboneliğinde bir Kurtarma Hizmetleri kasası oluşturun.
**VMM sunucusu** | Birincil ve ikincil VMM konumu gerekir. | Bir tane birincil sitede, bir tane de ikincil sitede VMM sunucusu olması önerilir.
**Hyper-V sunucusu** |  Birincil ve ikincil VMM bulutlarında bir veya daha fazla Hyper-V konak sunucusu. | Verilerin, Kerberos veya sertifika kimlik doğrulaması kullanılarak, LAN ya da VPN üzerinden birincil ve ikincil Hyper-V ana bilgisayar sunucuları arasında çoğaltılması gerekir.  
**Hyper-V VM’leri** | Hyper-V ana bilgisayar sunucusunda. | Kaynak ana bilgisayar sunucusunda çoğaltmak istediğiniz en az bir VM olması gerekir.

**Şirket içi şirket içi mimarisi**

![Şirket içinden şirket içine](./media/concepts-hyper-v-to-secondary-architecture/arch-onprem-onprem.png)

## <a name="replication-process"></a>Çoğaltma işlemi

1. İlk çoğaltma tetiklendiğinde bir [Hyper-V VM anlık görüntü](https://technet.microsoft.com/library/dd560637.aspx) anlık görüntü alınmaz.
2. VM üzerindeki sanal sabit disklere çoğaltılmış tek tek, ikincil konum için ' dir.
3. İlk çoğaltma işlemi devam ederken disk değişimi meydana gelirse 
4. İlk çoğaltma tamamlandığında, değişim çoğaltması başlar. Hyper-V çoğaltma çoğaltma İzleyicisi değişiklikleri Hyper-V çoğaltma günlükleri (.hrl) izler. Bu günlük dosyaları disklerle aynı klasörde yer alır. Her diskin ikincil konuma gönderilen bir ilişkili .hrl dosyası vardır. İlk çoğaltma sırasında anlık görüntü ve günlük dosyaları disk kaynaklarını kullanır.
5. Günlükteki değişim disk değişiklikleri eşitlenir ve üst diske birleştirilir.
6. 

## <a name="failover-and-failback-process"></a>Yük devretme ve yeniden çalışma işlemi

- Üzerinde tek bir makine başarısız veya birden çok makinelerin yük devretme düzenlemek için kurtarma planları oluşturabilirsiniz.
- Şirket içi siteler arasında planlanmış veya planlanmamış bir yük devretme çalıştırabilirsiniz. Planlı bir yük devretme çalıştırırsanız, veri kaybı olmaması için kaynak VM’ler kapatılır.
    - Yük devretme makineler ikincil konumdaki korunmayan sonra ikincil bir siteye planlanmamış yük devretme gerçekleştirin
    - Planlı bir yük devretme gerçekleştirdiyseniz, işlemden sonra ikincil konumdaki makineler korunur.
- İlk yük devretme çalıştıktan sonra iş yükü çoğaltma VM erişme başlatmak için yürütün.
- Ayrıca birincil konumda yeniden kullanılabilir duruma geldiğinde, geri başarısız olabilir.
    - Birincil ikincil sitedeki çoğaltma işlemi başlatma için geriye doğru çoğaltma Başlat. Ters çoğaltma sanal makineleri korumalı bir duruma getirir, ancak ikincil veri merkezi hala etkin konumdur.
    - Birincil siteyi yeniden etkin konum durumuna getirmek için ikincil siteden birincil siteye planlı yük devretme başlatır ve arkasından başka bir ters çoğaltma gerçekleştirirsiniz.



## <a name="next-steps"></a>Sonraki adımlar


İzleyin [Bu öğretici](tutorial-vmm-to-vmm.md) VMM Bulutları arasında Hyper-V çoğaltmayı etkinleştirmek için.
