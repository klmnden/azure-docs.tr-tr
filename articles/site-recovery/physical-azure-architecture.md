---
title: "Azure Site Recovery Azure çoğaltma mimarisinde fiziksel sunucuya | Microsoft Docs"
description: "Bu makalede, bileşenleri ve Azure Site Recovery hizmeti ile şirket içi fiziksel sunucuların Azure'a çoğaltırken kullanılan mimariye genel bakış sağlar"
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 02/27/2017
ms.author: raynew
ms.openlocfilehash: e8a5f4fad75ea6211e96ba216c8b506306dcfa34
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="physical-server-to-azure-replication-architecture"></a>Fiziksel sunucuya Azure çoğaltma mimarisi

Bu makalede mimari ve çoğaltma, yük devri ve Windows ve Linux fiziksel sunucuları arasında bir şirket içi site ile Azure, Kurtarma sırasında kullanılan işlemleri kullanarak [Azure Site Recovery](site-recovery-overview.md) hizmet.


## <a name="architectural-components"></a>Mimari bileşenler

Aşağıdaki tablo ve grafik fiziksel sunucu çoğaltma Azure için kullanılan bileşenler üst düzey bir görünümünü sağlar.  

**Bileşen** | **Gereksinim** | **Ayrıntılar**
--- | --- | ---
**Azure** | Bir Azure aboneliği, Azure depolama hesabı ve Azure ağı. | Şirket içi Vm'lerden gelen çoğaltılan veriler depolama hesabında depolanır. Bir başarısız şirket içinden Azure'a çalıştırdığınızda azure VM'ler ile çoğaltılan veriler oluşturulur. Azure VM’leri oluşturulduğunda Azure sanal ağına bağlanır.
**Yapılandırma sunucusu** | Tek bir fiziksel makine şirket içi veya VMware VM tüm şirket içi Site Recovery bileşenlerini çalıştırmak için dağıtılır. VM yapılandırması sunucu, işlem sunucusu ve ana hedef sunucusunda çalışır. | Yapılandırma sunucusu yerinde bileşenler ile Azure arasındaki iletişimi düzenler ve veri çoğaltma işlemlerini yönetir.
 **İşlem sunucusu**:  | Yapılandırma sunucusu ile birlikte varsayılan olarak yüklüdür. | Çoğaltma ağ geçidi olarak davranır. Çoğaltma verilerini alıp bu verileri önbelleğe alma, sıkıştırma ve şifreleme işlemleriyle iyileştirir ve Azure depolama alanına gönderir.<br/><br/> İşlem sunucusu Mobility hizmeti çoğaltmak istediğiniz sunucularda de yükler.<br/><br/> Dağıtımınız büyüdükçe, daha büyük birimleri çoğaltması trafiğini işlemek için ek, ayrı bir işlem sunucuları ekleyebilirsiniz.
 **Ana hedef sunucu** | Yapılandırma sunucusu ile birlikte varsayılan olarak yüklüdür. | Azure’dan yeniden çalışma sırasında çoğaltma verilerini işler.<br/><br/> Büyük dağıtımlar için yeniden çalışma için bir ek, ayrı ana hedef sunucusu ekleyebilirsiniz.
**Çoğaltılmış sunucuları** | Mobility hizmetinin Çoğalttığınız her sunucuya yüklenir. | İşlem sunucusundan otomatik yüklemesine izin ver öneririz. Alternatif olarak hizmeti el ile yüklemek veya bir System Center Configuration Manager gibi otomatik dağıtım yöntemini kullanın. 

**Fiziksel Azure Mimarisi**

![Bileşenler](./media/physical-azure-architecture/arch-enhanced.png)

## <a name="replication-process"></a>Çoğaltma işlemi

1. Şirket içi ve Azure bileşenleri de dahil olmak üzere dağıtım, ayarlayın. Kurtarma Hizmetleri kasasına çoğaltma kaynak ve hedef yapılandırma sunucusunu ayarlamayı, bir çoğaltma ilkesi oluşturun ve çoğaltmayı etkinleştirme belirtin.
2. Makinelerin çoğaltılacağı Çoğaltma İlkesi ve bir başlangıç kopyasını sunucu verilerini uygun olarak Azure depolama alanına çoğaltılır.
3. İlk çoğaltma sonlandırıldıktan sonra Azure delta değişikliklerini başlar. Bir makine için izlenen değişiklikler bir .hrl dosyasında saklanır.
    - HTTPS 443 numaralı bağlantı noktasını yapılandırma sunucusunda ile gelen çoğaltma yönetimi için iletişim kurar.
    - Makineleri çoğaltma veri gönderme bağlantı noktası HTTPS 9443 işlem sunucusu için gelen (değiştirilmiş).
    - Yapılandırma sunucusu, HTTPS 443 giden bağlantı noktası üzerinden Azure ile çoğaltma yönetimini düzenler.
    - İşlem sunucusu kaynak makinelerden gelen verileri alır, iyileştirip şifreler ve 443 giden bağlantı noktası üzerinden Azure depolamaya gönderir.
    - Çoklu VM tutarlılığını etkinleştirirseniz çoğaltma grubundaki makineler birbiriyle 20004 numaralı bağlantı noktası üzerinden iletişim kurar. Çoklu VM, birden çok makineyi yük devrettikleri zaman kilitlenmeyle tutarlı ve uygulamayla tutarlı kurtarma noktalarını paylaşan çoğaltma grupları halinde gruplandırdığınızda kullanılır. Bu özellik, makinelerin aynı iş yükünü çalıştırdığı ve tutarlı olmasının gerektiği durumlarda kullanışlıdır.
4. Trafik İnternet üzerinden Azure depolama genel uç noktalarına çoğaltılır. Alternatif olarak, Azure ExpressRoute [genel eşliğini](../expressroute/expressroute-circuit-peerings.md#azure-public-peering) kullanabilirsiniz. Trafiğin siteden siteye bir VPN aracılığıyla şirket içi bir siteden Azure’a çoğaltılması desteklenmez.


**Azure çoğaltma işlemi için fiziksel**

![Çoğaltma işlemi](./media/physical-azure-architecture/v2a-architecture-henry.png)

## <a name="failover-and-failback-process"></a>Yük devretme ve yeniden çalışma işlemi

Gerektiği gibi çoğaltma ayarlama ve her şeyin beklendiği gibi çalıştığını denetlemek için bir olağanüstü durum kurtarma ayrıntısı seçeneğini (yük devretme testi) çalıştırdıktan sonra Yük devretme ve yeniden çalıştırabilirsiniz. Şunlara dikkat edin:

- Planlanan yük devretme desteklenmez.
- Geri bir şirket içi VMware VM başarısız olmalıdır. Başka bir deyişle, hatta şirket içi fiziksel sunucuların Azure'a çoğaltma yaptığımda bir şirket içi VMware altyapı gerekir.
- Üzerinde tek bir makine başarısız veya birlikte birden çok makine vermesine kurtarma planları oluşturabilirsiniz.
- Bir yük devretme çalıştırdığınızda, Azure Vm'leri çoğaltılmış verileri Azure depolama alanında oluşturulur.
- İlk yük devretme tetikleme sonra iş yükü Azure sanal makineden erişme başlatmak üzere yürütün.
- Birincil şirket içi siteniz yeniden kullanılabilir olduğunda siteyi yeniden çalıştırabilirsiniz.
- Yeniden çalışma altyapısını kurma gerek dahil olmak üzere:
    - **Azure'da geçici işlem sunucusu**: Azure'dan başarısız olmasına, bir Azure VM'yi yedekleme azure'dan çoğaltmayı düzenlemek için bir işlem sunucusu olarak davranacak şekilde ayarlayın. Yeniden çalışma sona erdikten sonra bu VM'yi silebilirsiniz.
    - **VPN bağlantısı**: yeniden çalışmak için bir VPN bağlantısı (veya Azure ExpressRoute) Azure ağından şirket içi siteye gerekir.
    - **Ayrı bir ana hedef sunucusu**: varsayılan olarak, yapılandırma sunucusunda şirket içi VMware VM ile yüklenen ana hedef sunucusu yeniden çalışma işler. Ancak, geri büyük miktarda trafik başarısız ihtiyacınız varsa, bu amaç için bir ayrı şirket içi ana hedef sunucusu ayarlamanız gerekir.
    - **Yeniden çalışma ilkesi**: Şirket içi sitenize geri çoğaltmak için bir yeniden çalışma ilkeniz olmalıdır. Azure için şirket içi çoğaltma ilkesi oluşturduğunuzda otomatik olarak oluşturuldu.
    - **VMware altyapı**: yeniden çalışma için bir VMware altyapı gerekir. Bir fiziksel sunucuda yeniden çalışamazsınız.
- Bileşenleri yerinde olduktan sonra yeniden çalışma üç aşamada gerçekleşir:
    - 1. Aşama: Azure sanal makinelerini yeniden koruyun, böylece Azure'dan çoğaltmak şirket içi VMware sanal makinelerini dön.
    - 2. Aşama: şirket içi siteye bir yük devretme çalıştırın.
    - 3. Aşama: iş yükleri geri başarısız olmuş sonra çoğaltmayı yeniden etkinleştirin.

**VMware azure'dan**

![Yeniden çalışma](./media/physical-azure-architecture/enhanced-failback.png)


## <a name="next-steps"></a>Sonraki adımlar

İzleyin [Bu öğretici](physical-azure-disaster-recovery.md) Azure çoğaltma fiziksel sunucuya etkinleştirmek için.
