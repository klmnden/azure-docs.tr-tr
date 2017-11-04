---
title: "Azure'da VMware çoğaltma işlemini mimarisi gözden | Microsoft Docs"
description: "Bu makalede, bileşenleri ve Azure Site Recovery hizmeti ile şirket içi VMware Vm'lerini Azure'a çoğaltırken kullanılan mimariye genel bakış sağlar"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d03d2dd3-2455-4ca8-a942-a342030ee6ce
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/10/2017
ms.author: raynew
ms.openlocfilehash: ac1151d15a88650f5845cb879cd210e9f7cba0fd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="vmware-to-azure-replication-architecture"></a>VMware Azure çoğaltma mimarisi

Bu makalede mimari ve çoğaltma, yük devri ve VMware sanal makineleri (VM'ler) bir şirket içi VMware site ile Azure arasında Kurtarma sırasında kullanılan işlemleri kullanarak [Azure Site Recovery](site-recovery-overview.md) hizmet.


## <a name="architectural-components"></a>Mimari bileşenler

Aşağıdaki tablo ve grafik VMware çoğaltma Azure için kullanılan bileşenler üst düzey bir görünümünü sağlar.  

**Bileşen** | **Gereksinim** | **Ayrıntılar**
--- | --- | ---
**Azure** | Bir Azure aboneliği, Azure depolama hesabı ve Azure ağı. | Şirket içi Vm'lerden gelen çoğaltılan veriler depolama hesabında depolanır. Bir başarısız şirket içinden Azure'a çalıştırdığınızda azure VM'ler ile çoğaltılan veriler oluşturulur. Azure VM’leri oluşturulduğunda Azure sanal ağına bağlanır.
**Yapılandırma sunucusu** | Tek bir VMware VM tüm şirket içi Site Recovery bileşenlerini çalıştırmak için dağıtılan şirket içi. VM yapılandırması sunucu, işlem sunucusu ve ana hedef sunucusunda çalışır. | Yapılandırma sunucusu yerinde bileşenler ile Azure arasındaki iletişimi düzenler ve veri çoğaltma işlemlerini yönetir.
 **İşlem sunucusu**:  | Yapılandırma sunucusu ile birlikte varsayılan olarak yüklüdür. | Çoğaltma ağ geçidi olarak davranır. Çoğaltma verilerini alıp bu verileri önbelleğe alma, sıkıştırma ve şifreleme işlemleriyle iyileştirir ve Azure depolama alanına gönderir.<br/><br/> İşlem sunucusu çoğaltmak istediğiniz sanal makinelerin de Mobility hizmeti yükler ve sanal makineleri otomatik olarak bulmayı şirket içi VMware sunucularda gerçekleştirir.<br/><br/> Dağıtımınız büyüdükçe, daha büyük birimleri çoğaltması trafiğini işlemek için ek, ayrı bir işlem sunucuları ekleyebilirsiniz.
 **Ana hedef sunucu** | Yapılandırma sunucusu ile birlikte varsayılan olarak yüklüdür. | Azure’dan yeniden çalışma sırasında çoğaltma verilerini işler.<br/><br/> Büyük dağıtımlar için yeniden çalışma için bir ek, ayrı ana hedef sunucusu ekleyebilirsiniz.
**VMware sunucuları** | VMware sanal makineleri şirket içi vSphere ESXi sunucularda barındırılır. Ana bilgisayarları yönetmek için bir vCenter sunucusu öneririz. | Site Recovery dağıtımı sırasında VMware sunucularını kurtarma Hizmetleri Kasası'na ekleyin.
**Çoğaltılan makineler** | Mobility hizmeti, çoğaltılan her VMware VM yüklenir. | İşlem sunucusundan otomatik yüklemesine izin ver öneririz. Alternatif olarak hizmeti el ile yüklemek veya bir System Center Configuration Manager gibi otomatik dağıtım yöntemini kullanın. 

**VMware-Azure arası mimari**

![Bileşenler](./media/concepts-vmware-to-azure-architecture/arch-enhanced.png)

## <a name="replication-process"></a>Çoğaltma işlemi

1. Şirket içi ve Azure bileşenleri de dahil olmak üzere dağıtım, ayarlayın. Kurtarma Hizmetleri kasasına çoğaltma kaynak ve hedef yapılandırma sunucusunu ayarlamayı, bir çoğaltma ilkesi oluşturun ve çoğaltmayı etkinleştirme belirtin.
2. Makinelerin çoğaltılacağı Çoğaltma İlkesi ve bir başlangıç kopyasını VM verisi uygun olarak Azure depolama alanına çoğaltılır.
3. İlk çoğaltma sonlandırıldıktan sonra Azure delta değişikliklerini başlar. Bir makine için izlenen değişiklikler bir .hrl dosyasında saklanır.
    - HTTPS 443 numaralı bağlantı noktasını yapılandırma sunucusunda ile gelen çoğaltma yönetimi için iletişim kurar.
    - Makineleri çoğaltma veri gönderme bağlantı noktası HTTPS 9443 işlem sunucusu için gelen (değiştirilmiş).
    - Yapılandırma sunucusu, HTTPS 443 giden bağlantı noktası üzerinden Azure ile çoğaltma yönetimini düzenler.
    - İşlem sunucusu kaynak makinelerden gelen verileri alır, iyileştirip şifreler ve 443 giden bağlantı noktası üzerinden Azure depolamaya gönderir.
    - Çoklu VM tutarlılığını etkinleştirmek, çoğaltma grubundaki birbiriyle 20004 bağlantı noktası üzerinden iletişim kurar. Çoklu VM, birden çok makineyi yük devrettikleri zaman kilitlenmeyle tutarlı ve uygulamayla tutarlı kurtarma noktalarını paylaşan çoğaltma grupları halinde gruplandırdığınızda kullanılır. Bu özellik, makinelerin aynı iş yükünü çalıştırdığı ve tutarlı olmasının gerektiği durumlarda kullanışlıdır.
4. Trafik İnternet üzerinden Azure depolama genel uç noktalarına çoğaltılır. Alternatif olarak, Azure ExpressRoute [genel eşliğini](../expressroute/expressroute-circuit-peerings.md#azure-public-peering) kullanabilirsiniz. Trafiğin siteden siteye bir VPN aracılığıyla şirket içi bir siteden Azure’a çoğaltılması desteklenmez.


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

Gözden geçirme için destek matrisi VMware Azure çoğaltmayı etkinleştirmek için öğreticiyi izleyin.
Bir yük devretme ve yeniden çalıştırın.