---
title: "Azure Site Recovery Azure çoğaltma mimarisinde VMware | Microsoft Docs"
description: "Bu makalede, bileşenleri ve Azure Site Recovery hizmeti ile şirket içi VMware Vm'lerini Azure'a çoğaltırken kullanılan mimariye genel bakış sağlar"
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 01/15/2018
ms.author: raynew
ms.openlocfilehash: 7999f23d167c6e8a7bcaf3a817e0cd2e80a1d649
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2018
---
# <a name="vmware-to-azure-replication-architecture"></a>VMware Azure çoğaltma mimarisi

Bu makalede mimari ve çoğaltma, yük devri ve VMware sanal makineleri (VM'ler) bir şirket içi VMware site ile Azure arasında Kurtarma sırasında kullanılan işlemleri kullanarak [Azure Site Recovery](site-recovery-overview.md) hizmet.


## <a name="architectural-components"></a>Mimari bileşenler

Aşağıdaki tablo ve grafik VMware çoğaltma Azure için kullanılan bileşenler üst düzey bir görünümünü sağlar.  

**Bileşen** | **Gereksinim** | **Ayrıntılar**
--- | --- | ---
**Azure** | Bir Azure aboneliği, Azure depolama hesabı ve Azure ağı. | Şirket içi Vm'lerden gelen çoğaltılan veriler depolama hesabında depolanır. Bir başarısız şirket içinden Azure'a çalıştırdığınızda azure VM'ler ile çoğaltılan veriler oluşturulur. Azure VM’leri oluşturulduğunda Azure sanal ağına bağlanır.
**Yapılandırma sunucusu makine** | Tek bir makine şirket içi. Bir VMware gerçekleştirebileceğiniz indirilen OVF şablondan dağıtılan VM farklı çalıştır öneririz.<br/><br/> Makine yapılandırma sunucusuna işlem sunucusu ve ana hedef sunucusu da dahil olmak üzere tüm şirket içi Site Recovery bileşenlerini çalışır. | **Yapılandırma sunucusu**: şirket içi ve Azure arasındaki iletişimi düzenler ve veri çoğaltma yönetir.<br/><br/> **İşlem sunucusu**: yapılandırma sunucusundaki varsayılan olarak yüklüdür. Çoğaltma verilerini alıp, önbelleğe alma, sıkıştırma ve şifreleme iyileştirir ve Azure depolama alanına gönderir. İşlem sunucusu çoğaltmak istediğiniz sanal makinelerin de Mobility hizmeti yükler ve şirket içi makinelerin otomatik bulma işlemini gerçekleştirir. Dağıtımınız büyüdükçe, daha büyük birimleri çoğaltması trafiğini işlemek için ek, ayrı bir işlem sunucuları ekleyebilirsiniz.<br/><br/>  **Ana hedef sunucusu**: yapılandırma sunucusundaki varsayılan olarak yüklüdür. Azure'dan yeniden çalışma sırasında çoğaltma verilerini işler. Büyük dağıtımlar için yeniden çalışma için bir ek, ayrı ana hedef sunucusu ekleyebilirsiniz.
**VMware sunucuları** | VMware sanal makineleri şirket içi vSphere ESXi sunucularda barındırılır. Ana bilgisayarları yönetmek için bir vCenter sunucusu öneririz. | Site Recovery dağıtımı sırasında VMware sunucularını kurtarma Hizmetleri Kasası'na ekleyin.
**Çoğaltılan makineler** | Mobility hizmeti, çoğaltılan her VMware VM yüklenir. | İşlem sunucusundan otomatik yüklemesine izin ver öneririz. Alternatif olarak hizmeti el ile yüklemek veya bir System Center Configuration Manager gibi otomatik dağıtım yöntemini kullanın.

**VMware-Azure arası mimari**

![Bileşenler](./media/concepts-vmware-to-azure-architecture/arch-enhanced.png)

## <a name="replication-process"></a>Çoğaltma işlemi

1.  Azure kaynakları ve şirket içi bileşenleri hazırlayın.
2.  Kurtarma Hizmetleri kasasına kaynak çoğaltma ayarlarını belirtin. Bu işlemin bir parçası olarak, şirket içi yapılandırma sunucusu ayarlayın. Bu sunucuyu bir VMware VM olarak dağıtmak için hazırlanmış bir OVF şablonunu indirebilir ve VMware VM oluşturmak için aktarın.
3. Hedef çoğaltma ayarları belirtin, bir çoğaltma ilkesi oluşturun ve VMware Vm'leri için çoğaltmayı etkinleştirme.
4.  Makinelerin çoğaltılacağı Çoğaltma İlkesi ve bir başlangıç kopyasını VM verisi uygun olarak Azure depolama alanına çoğaltılır.
5.  İlk çoğaltma sonlandırıldıktan sonra Azure delta değişikliklerini başlar. Bir makine için izlenen değişiklikler bir .hrl dosyasında saklanır.
    - HTTPS 443 numaralı bağlantı noktasını yapılandırma sunucusunda ile gelen çoğaltma yönetimi için iletişim kurar.
    - Makineleri çoğaltma veri gönderme bağlantı noktası HTTPS 9443 işlem sunucusu için gelen (değiştirilmiş).
    - Yapılandırma sunucusu, HTTPS 443 giden bağlantı noktası üzerinden Azure ile çoğaltma yönetimini düzenler.
    - İşlem sunucusu kaynak makinelerden gelen verileri alır, iyileştirip şifreler ve 443 giden bağlantı noktası üzerinden Azure depolamaya gönderir.
    - Çoklu VM tutarlılığını etkinleştirmek, çoğaltma grubundaki birbiriyle 20004 bağlantı noktası üzerinden iletişim kurar. Çoklu VM, birden çok makineyi yük devrettikleri zaman kilitlenmeyle tutarlı ve uygulamayla tutarlı kurtarma noktalarını paylaşan çoğaltma grupları halinde gruplandırdığınızda kullanılır. Bu özellik, makinelerin aynı iş yükünü çalıştırdığı ve tutarlı olmasının gerektiği durumlarda kullanışlıdır.
6.  Trafik, internet üzerinden ortak uç noktaları, Azure depolama alanına çoğaltır. Alternatif olarak, zure ExpressRoute kullanabilirsiniz [ortak eşleme](../expressroute/expressroute-circuit-peerings.md#azure-public-peering). Trafiğin siteden siteye bir VPN aracılığıyla şirket içi bir siteden Azure’a çoğaltılması desteklenmez.


**Azure çoğaltma işlemi VMware**

![Çoğaltma işlemi](./media/concepts-vmware-to-azure-architecture/v2a-architecture-henry.png)

## <a name="failover-and-failback-process"></a>Yük devretme ve yeniden çalışma işlemi

Gerektiği gibi çoğaltma ayarlama ve her şeyin beklendiği gibi çalıştığını denetlemek için bir olağanüstü durum kurtarma ayrıntısı seçeneğini (yük devretme testi) çalıştırdıktan sonra Yük devretme ve yeniden çalıştırabilirsiniz.

1. Üzerinde tek bir makine başarısız veya birden çok VM vermesine kurtarma planları oluşturabilirsiniz.
2. Bir yük devretme çalıştırdığınızda, Azure Vm'leri çoğaltılmış verileri Azure depolama alanında oluşturulur.
3. İlk yük devretme tetikleme sonra iş yükü Azure sanal makineden erişme başlatmak üzere yürütün.

Birincil şirket içi siteniz yeniden kullanılabilir olduğunda siteyi yeniden çalıştırabilirsiniz.
1. Yeniden çalışma altyapısını kurma gerek dahil olmak üzere:
    - **Azure'da geçici işlem sunucusu**: Azure'dan başarısız olmasına, bir Azure VM'yi yedekleme azure'dan çoğaltmayı düzenlemek için bir işlem sunucusu olarak davranacak şekilde ayarlayın. Yeniden çalışma sona erdikten sonra bu VM'yi silebilirsiniz.
    - **VPN bağlantısı**: yeniden çalışmak için bir VPN bağlantısı (veya Azure ExpressRoute) Azure ağından şirket içi siteye gerekir.
    - **Ayrı bir ana hedef sunucusu**: varsayılan olarak, yapılandırma sunucusunda şirket içi VMware VM ile yüklenen ana hedef sunucusu yeniden çalışma işler. Ancak, geri büyük miktarda trafik başarısız ihtiyacınız varsa, bu amaç için bir ayrı şirket içi ana hedef sunucusu ayarlamanız gerekir.
    - **Yeniden çalışma ilkesi**: Şirket içi sitenize geri çoğaltmak için bir yeniden çalışma ilkeniz olmalıdır. Azure için şirket içi çoğaltma ilkesi oluşturduğunuzda otomatik olarak oluşturuldu.
2. Bileşenleri yerinde olduktan sonra yeniden çalışma üç aşamada gerçekleşir:
    - 1. Aşama: Azure sanal makinelerini yeniden koruyun, böylece Azure'dan çoğaltmak şirket içi VMware sanal makinelerini dön.
    - 2. Aşama: şirket içi siteye bir yük devretme çalıştırın.
    - 3. Aşama: iş yükleri geri başarısız olmuş sonra şirket içi sanal makineleri çoğaltmak yeniden etkinleştirin.

**VMware azure'dan**

![Yeniden çalışma](./media/concepts-vmware-to-azure-architecture/enhanced-failback.png)


## <a name="next-steps"></a>Sonraki adımlar

İzleyin [Bu öğretici](tutorial-vmware-to-azure.md) VMware Azure çoğaltmayı etkinleştirmek için.
