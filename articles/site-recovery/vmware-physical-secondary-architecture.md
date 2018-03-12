---
title: "Azure Site Recovery VMware/fiziksel sunucu çoğaltma mimarisi | Microsoft Docs"
description: "Bu makalede bileşenleri ve çoğaltma VMware Vm'lerini veya fiziksel Windows/Linux sunucuları Azure Site Recovery ile ikincil VMware siteye şirket içi kullanılan mimarisi genel bakış sağlar."
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 03/06/2018
ms.author: raynew
ms.openlocfilehash: 97a990aa3ed9043280888900d8fc7b604b6c22b5
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="vmware-vmphysical-server-to-vmware-replication-architecture"></a>VMware VM/fiziksel sunucuya VMware çoğaltma mimarisi

Bu makalede mimari ve çoğaltma, yük devri ve şirket içi VMware sanal makineleri (VM'ler) veya fiziksel Windows/Linux sunucuları için ikincil VMware sitesi kullanarak bir kurtarma sırasında kullanılan işlemleri [Azure Site Recovery](site-recovery-overview.md).


## <a name="architectural-components"></a>Mimari bileşenler

**Alan** | **Bileşen** | **Ayrıntılar**
--- | --- | ---
**Azure** | Bu senaryoyu, InMage Scout kullanarak dağıtırsınız. | InMage Scout elde etmek için bir Azure aboneliğine sahip olmanız gerekir.<br/><br/> Bir Kurtarma Hizmetleri kasası oluşturduktan sonra InMage Scout hizmetini indirip dağıtımı ayarlamak üzere en son güncelleştirmeleri yüklersiniz.
**İşlem sunucusu** | Birincil sitede bulunur | Önbelleğe alma, sıkıştırma ve veri iyileştirme işlemlerini yürütmek için işlem sunucusunu dağıtırsınız.<br/><br/> Ayrıca bu sunucu, Birleşik Aracı'nın korumak istediğiniz makinelere göndermeli yükleme işlemini yürütür.
**Yapılandırma sunucusu** | İkincil sitede bulunur | Yapılandırma sunucusu, yönetim Web sitesini veya vContinuum konsolunu kullanarak dağıtımınızı yönetir, yapılandırır ve izler.
**vContinuum sunucusu** | İsteğe bağlı. Yapılandırma sunucusuyla aynı konuma yüklenir. | Korunan ortamınızı yönetmeye ve izlemeye yönelik bir konsol sağlar.
**Ana hedef sunucu** | İkincil sitede bulunur | Ana hedef sunucu çoğaltılan verileri tutar. İşlem sunucusundan verileri alır, ikincil sitede çoğaltılan bir makine oluşturur ve veri bekletme noktalarını tutar.<br/><br/> İhtiyacınız olan ana hedef sunucusu sayısı koruduğunuz makine sayısına bağlıdır.<br/><br/> Birincil sitede yeniden çalıştırmak isterseniz burada da bir ana hedef sunucusuna sahip olmanız gerekir. Birleşik Aracı bu sunucuya yüklenir.
**VMware ESX/ESXi ve vCenter sunucusu** |  VM’ler ESX/ESXi ana bilgisayarlarında barındırılır. Ana bilgisayarlar bir vCenter sunucusu ile yönetilir | VMware VM’lerini çoğaltmak için bir VMware altyapınız olmalıdır.
**VM’ler/fiziksel sunucular** |  Çoğaltmak istediğiniz VMware VM’leri veya fiziksel sunucularda yüklü Birleşik Aracı. | Aracı,tüm bileşenler arasındaki bir iletişim sağlayıcısı gibi davranır.

### <a name="replication-process"></a>Çoğaltma işlemi

1. Her sitede bileşen sunucularını (yapılandırma, işlem, ana hedef) ayarlayıp çoğaltmak istediğiniz makinelere Birleşik Aracı'yı yükleyin.
2. İlk çoğaltmanın ardından makinelerdeki aracılar çoğaltma değişimleri işlem sunucusuna gönderir.
3. İşlem sunucusu verileri iyileştirir ve ikincil sitedeki ana hedef sunucusuna aktarır. Yapılandırma sunucusu çoğaltma sürecini yönetir.

**Şekil 6: VMware’den VMware’e çoğaltma**

![VMware'den VMware'e](./media/site-recovery-components/vmware-to-vmware.png)



## <a name="next-steps"></a>Sonraki adımlar

[Ayarlanan](vmware-physical-secondary-disaster-recovery.md) VMware Vm'lerini ve fiziksel sunucuları ikincil bir siteye olağanüstü durum kurtarma.
