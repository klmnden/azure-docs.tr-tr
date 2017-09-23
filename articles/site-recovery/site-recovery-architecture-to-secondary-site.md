---
title: "Azure Site Recovery'de ikincil bir şirket içi konuma yönelik şirket içi makine çoğaltma işlemi nasıl gerçekleştirilir? | Microsoft Docs"
description: "Bu makale, Azure Site Recovery hizmeti aracılığıyla şirket içi VM'leri ve fiziksel sunucuları ikincil bir konuma çoğaltırken kullanılan bileşenlere ve mimariye yönelik genel bir bakış sağlamaktadır."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/29/2017
ms.author: raynew
ms.translationtype: Human Translation
ms.sourcegitcommit: db18dd24a1d10a836d07c3ab1925a8e59371051f
ms.openlocfilehash: fca95c63964b955db7ddfbe53250702cc8af122e
ms.contentlocale: tr-tr
ms.lasthandoff: 06/15/2017

---
# <a name="how-does-on-premises-machine-replication-to-a-secondary-site-work-in-site-recovery"></a>Site Recovery'de ikincil bir konuma yönelik şirket içi makine çoğaltma işlemi nasıl gerçekleştirilir?

Bu makalede [Azure Site Recovery](site-recovery-overview.md) hizmeti aracılığıyla şirket içi sanal makineleri ve fiziksel sunucuları Azure'a çoğaltırken kullanılan bileşenler ve işlemler açıklanmıştır.

Aşağıdakileri ikincil bir şirket içi konuma çoğaltabilirsiniz:
- Şirket içi Hyper-V VM'leri, Hyper-V kümelerindeki Hyper-V VM'leri ve System Center Virtual Machine Manager (VMM) bulutlarında yönetilen tek başına konaklar.
- Şirket içi VMware VM'leri ve Windows/Linux fiziksel sunucuları. Bu senaryoda çoğaltma işlemi Scout tarafından yönetilir.

Tüm yorumlarınızı bu makalenin alt kısmında veya [Azure Kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)'nda paylaşabilirsiniz.

## <a name="replicate-hyper-v-vms-to-a-secondary-on-premises-site"></a>Hyper-V VM'lerini ikincil bir şirket içi konuma çoğaltma


### <a name="architectural-components"></a>Mimari bileşenler

Hyper-V VM’lerini ikincil bir siteye çoğaltmak için aşağıdakiler gerekir.

**Bileşen** | **Konum** | **Ayrıntılar**
--- | --- | ---
**Azure** | Bir Microsoft hesabınızın olması gerekir. |
**VMM sunucusu** | Bir tane birincil sitede, bir tane de ikincil sitede VMM sunucusu olması önerilir | Her VMM sunucusu İnternet’e bağlı olmalıdır.<br/><br/> Her sunucunun Hyper-V yetenek profili kümesi içeren en az bir VMM özel bulutu olmalıdır.<br/><br/> Azure Site Recovery Sağlayıcısını VMM sunucusuna yüklemeniz gerekir. Sağlayıcı, internet üzerinden Site Recovery hizmetiyle gerçekleştirilen çoğaltma işlemini düzenler ve yönetir. Sağlayıcı ile Azure arasındaki iletişimler güvenli ve şifrelidir.
**Hyper-V sunucusu** |  Birincil ve ikincil VMM bulutlarında bir veya daha fazla Hyper-V konak sunucusu.<br/><br/> Sunucular İnternet’e bağlı olmalıdır.<br/><br/> Verilerin, Kerberos veya sertifika kimlik doğrulaması kullanılarak, LAN ya da VPN üzerinden birincil ve ikincil Hyper-V ana bilgisayar sunucuları arasında çoğaltılması gerekir.  
**Hyper-V VM’leri** | Kaynak Hyper-V ana bilgisayar sunucusunda bulunur. | Kaynak ana bilgisayar sunucusunda çoğaltmak istediğiniz en az bir VM olması gerekir.

### <a name="replication-process"></a>Çoğaltma işlemi

1. Azure hesabını ayarlarsınız.
2. Site Recovery kurtarma için bir Kurtarma Hizmetleri kasası ayarlarsınız ve aşağıdakileri de içeren kasa ayarlarını yapılandırırsınız:

    - Çoğaltma kaynağı ve hedefi (birincil ve ikincil siteler).
    - Azure Site Recovery Sağlayıcısı ve Microsoft Azure Kurtarma Hizmetleri aracısının yüklenmesi. Sağlayıcı VMM sunucularına, aracı ise her bir Hyper-V konağına yüklenir.
    - Kaynak VMM bulutu için bir çoğaltma ilkesi oluşturursunuz. İlke, buluttaki tüm konaklarda yer alan tüm VM’lere uygulanır.
    - Hyper-V VM’leri için çoğaltmayı etkinleştirirsiniz. İlk çoğaltma, çoğaltma ilkesi ayarlarına uygun şekilde gerçekleştirilir.
4. Veri değişiklikleri izlenir ve delta değişikliklerinin çoğaltılması ilk çoğaltma işlemi tamamlandıktan sonra başlar. Bir öğe için izlenen değişiklikler bir .hrl dosyasında saklanır.
5. Her şeyin çalıştığından emin olmak için bir Yük devretme testi çalıştırırsınız.

**Şekil 1: VMM'den VMM'ye çoğaltma**

![Şirket içinden şirket içine](./media/site-recovery-components/arch-onprem-onprem.png)

### <a name="failover-and-failback-process"></a>Yük devretme ve yeniden çalışma işlemi

1. Şirket içi siteler arasında planlanmış veya planlanmamış bir [yük devretme](site-recovery-failover.md) gerçekleştirebilirsiniz. Planlı bir yük devretme çalıştırırsanız, veri kaybı olmaması için kaynak VM’ler kapatılır.
2. Tek bir makine üzerinden yük devredebilir veya [kurtarma planları](site-recovery-create-recovery-plans.md) oluşturarak birden çok makinenin devredilmesini düzenleyebilirsiniz.
4. İkincil bir siteye yönelik planlanmamış bir yük devretme gerçekleştirirseniz, işlem tamamlandıktan sonra ikincil konumdaki makineler koruma veya çoğaltma için etkinleştirilmez. Planlı bir yük devretme gerçekleştirdiyseniz, işlemden sonra ikincil konumdaki makineler korunur.
5. Daha sonra, kopya VM’deki iş yüküne erişmeye başlamak için yük devretmeyi yürütürsünüz.
6. Birincil sitenizi yeniden kullanılabilir duruma geldiğinde ters çoğaltma başlatarak ikincil siteden birincil siteye çoğaltma gerçekleştirirsiniz. Ters çoğaltma sanal makineleri korumalı bir duruma getirir, ancak ikincil veri merkezi hala etkin konumdur.
7. Birincil siteyi yeniden etkin konum durumuna getirmek için ikincil siteden birincil siteye planlı yük devretme başlatır ve arkasından başka bir ters çoğaltma gerçekleştirirsiniz.




## <a name="replicate-vmware-vmsphysical-servers-to-a-secondary-site"></a>VMware VM’lerini/Fiziksel sunucuları ikincil bir siteye çoğaltma

Aşağıdaki mimari bileşenler ile InMage Scout'u kullanarak VMware VM'lerini veya fiziksel sunucuları ikincil bir konuma çoğaltabilirsiniz:


### <a name="architectural-components"></a>Mimari bileşenler

**Bileşen** | **Konum** | **Ayrıntılar**
--- | --- | ---
**Azure** | InMage Scout. | InMage Scout elde etmek için bir Azure aboneliğine sahip olmanız gerekir.<br/><br/> Bir Kurtarma Hizmetleri kasası oluşturduktan sonra InMage Scout hizmetini indirip dağıtımı ayarlamak üzere en son güncelleştirmeleri yüklersiniz.
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

**Şekil 2: VMware'den VMware'e çoğaltma**

![VMware'den VMware'e](./media/site-recovery-components/vmware-to-vmware.png)


## <a name="next-steps"></a>Sonraki adımlar

[Destek matrisini](site-recovery-support-matrix-to-sec-site.md) inceleyin

