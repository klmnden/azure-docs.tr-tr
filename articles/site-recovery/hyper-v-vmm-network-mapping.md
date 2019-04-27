---
title: Site Recovery ile azure'a (VMM ile) Hyper-V VM'LERİNDE olağanüstü durum kurtarma için Ağ eşlemesi hakkında | Microsoft Docs
description: Azure Site Recovery ile azure'a olağanüstü durum kurtarma Hyper-V vm'lerini (VMM bulutlarında yönetilen) için ağ eşlemesini ayarlama işlemini açıklamaktadır.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 12/27/2018
ms.author: raynew
ms.openlocfilehash: cefde79cf8c544a6900b1efa5dbcefbc43638d40
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60679338"
---
# <a name="prepare-network-mapping-for-hyper-v-vm-disaster-recovery-to-azure"></a>Azure'a Hyper-V VM'LERİNDE olağanüstü durum kurtarma için ağ eşlemesini hazırlama


Bu makalede, anlamak ve System Center Virtual Machine Manager (VMM) bulutlarındaki Hyper-V sanal makinelerini azure'a veya ikincil bir siteye çoğaltma yaptığınızda ağ eşlemeye Hazırlama yardımcı olur. kullanarak [Azure Site Recovery](site-recovery-overview.md) hizmeti.


## <a name="prepare-network-mapping-for-replication-to-azure"></a>Azure'a çoğaltma için ağ eşlemesini hazırlama

Azure, bir kaynak VMM sunucusunda VM ağları arasında ağ eşleme eşlemeler çoğaltma yapıyorsanız ve hedef Azure sanal ağları. Eşleme sürecinde şu işlemler gerçekleştirilir:
-  **Ağ bağlantısı**— çoğaltılan Azure Vm'leri için eşlenen ağ bağlanmasını sağlar. Farklı bir kurtarma planında üzerinden başarısız olsa bile, aynı ağda yük devretme tüm makineler birbirine bağlanabilir.
- **Ağ geçidi**— bir ağ geçidi hedef Azure ağında ayarlanıp ayarlanmadığını VM'ler diğer şirket içi sanal makinelere bağlanabilir.

Ağ eşleme gibi çalışır:

- Bir Azure sanal ağı için bir kaynak VMM VM ağına eşleyin.
- Azure Vm'lerinde kaynak yük devretme sonrasında ağ eşlenmiş hedef sanal ağa bağlanır.
- Kaynak VM ağına eklenen yeni VM'ler, Çoğaltma gerçekleştiğinde eşlenen Azure ağına bağlanır.
- Hedef ağın birden çok alt ağı varsa ve bu alt ağlardan biri kaynak sanal makinenin bulunduğu alt ağ ile aynı adı taşıyorsa, çoğaltılan sanal makine yük devretme işleminin ardından hedef alt ağa bağlanır.
- Eşleşen ada sahip bir hedef alt ağ yoksa sanal makine ağdaki ilk alt ağa bağlanır.

## <a name="prepare-network-mapping-for-replication-to-a-secondary-site"></a>İkincil bir siteye çoğaltma için ağ eşlemesini hazırlama

İkincil bir siteye çoğaltma yapıyorsanız, bir kaynak VMM sunucusunda VM ağları ve bir hedef VMM sunucusunda VM ağları arasında ağ eşlemesi eşler. Eşleme sürecinde şu işlemler gerçekleştirilir:

- **Ağ bağlantısı**— Vm'lere yük devretme sonrasında uygun ağlara bağlanır. VM çoğaltma kaynak ağına eşlenmiş hedef ağa bağlanır.
- **En iyi VM yerleştirme**— çoğaltma Vm'leri Hyper-V ana bilgisayar sunucuları üzerinde en iyi şekilde yerleştirir. Çoğaltma sanal makineleri, eşlenen VM ağlarına erişebilen Konaklara yerleştirilir.
- **Hiçbir ağ eşlemesi**— ağ eşlemesini yapılandırmazsanız çoğalma VM'ler için herhangi bir VM ağını yük devretmeden sonra bağlanmaz.

Ağ eşleme gibi çalışır:

- İki VMM sunucularında veya iki site aynı sunucu tarafından yönetiliyorsa tek bir VMM sunucusunda VM ağları arasında ağ eşlemesi yapılandırılabilir.
- Ne zaman eşleştirme doğru yapılandırıldığından ve çoğaltma etkin birincil konumda VM bir ağa bağlı olması ve hedef konumda çoğaltması eşlenen alt ağına bağlanır.
- Site recovery'de ağ eşlemesi sırasında bir hedef VM ağı seçtiğinizde, kaynak VM ağı kullanan VMM kaynak bulutlarını, koruma için kullanılan hedef bulutlarda kullanılabilir hedef VM ağları ile birlikte görüntülenir.
- Hedef ağın birden çok alt ağı varsa ve bu alt ağlardan biri kaynak sanal makinenin bulunduğu alt ağ ile aynı ada sahip, ardından çoğaltma VM yük devretmenin ardından hedef alt ağa bağlanacaksınız. Eşleşen ada sahip bir hedef alt ağ varsa, sanal makine ağdaki ilk alt ağa bağlanır.

## <a name="example"></a>Örnek

Bu mekanizma göstermek için bir örnek aşağıda verilmiştir. Chicago, New York iki konum ile bir kuruluş ele alalım.

**Konum** | **VMM sunucusu** | **VM ağları** | **Eşlenen**
---|---|---|---
New York | VMM NewYork| VMNetwork1-NewYork | VMNetwork1 Şikago'eşlendi
 |  | VMNetwork2-NewYork | Eşlenmedi
Chicago | VMM Chicago| VMNetwork1 Chicago | VMNetwork1-NewYork eşlendi
 | | VMNetwork2 Chicago | Eşlenmedi

Bu örnekte:

- VM VMNetwork1-NewYork bağlı herhangi bir VM için oluşturulmuş bir çoğaltma, VMNetwork1-Şikago'ya bağlanır.
- VM VMNetwork2 NewYork veya VMNetwork2 Chicago için oluşturulmuş bir çoğaltma sırasında herhangi bir ağa bağlı olması gerekmez.

Bizim örnek kuruluş ve bulutla ilişkili mantıksal ağların VMM bulutlarında nasıl ayarlanıp aşağıda verilmiştir.

### <a name="cloud-protection-settings"></a>Bulut koruma ayarlarını

**Korumalı bulut** | **Bulut koruma** | **Mantıksal ağ (New York)**  
---|---|---
GoldCloud1 | GoldCloud2 |
SilverCloud1| SilverCloud2 |
GoldCloud2 | <p>NA</p><p></p> | <p>LogicalNetwork1 NewYork</p><p>LogicalNetwork1 Chicago</p>
SilverCloud2 | <p>NA</p><p></p> | <p>LogicalNetwork1 NewYork</p><p>LogicalNetwork1 Chicago</p>

### <a name="logical-and-vm-network-settings"></a>Mantıksal ve VM ağ ayarları

**Konum** | **Mantıksal ağ** | **İlişkili VM ağı**
---|---|---
New York | LogicalNetwork1 NewYork | VMNetwork1-NewYork
Chicago | LogicalNetwork1 Chicago | VMNetwork1 Chicago
 | LogicalNetwork2Chicago | VMNetwork2 Chicago

### <a name="target-network-settings"></a>Hedef ağ ayarları

Hedef VM ağ seçtiğinizde bu ayarları temel alarak aşağıdaki tabloda kullanılabilir seçenekler gösterilmektedir.

**Seç** | **Korumalı bulut** | **Bulut koruma** | **Hedef ağ yok**
---|---|---|---
VMNetwork1 Chicago | SilverCloud1 | SilverCloud2 | Kullanılabilir
 | GoldCloud1 | GoldCloud2 | Kullanılabilir
VMNetwork2 Chicago | SilverCloud1 | SilverCloud2 | Kullanılamaz
 | GoldCloud1 | GoldCloud2 | Kullanılabilir


Hedef ağın birden çok alt ağı varsa ve bu alt ağlardan biri kaynak sanal makinenin bulunduğu alt ağ ile aynı ada sahip, ardından çoğaltma sanal makinesi yük devretmenin ardından hedef alt ağa bağlanacaksınız. Eşleşen ada sahip bir hedef alt ağ yoksa sanal makine ağdaki ilk alt ağa bağlanır.


### <a name="failback-behavior"></a>Yeniden çalıştırma davranışı

Yeniden çalışma (ters çoğaltma) söz konusu olduğunda ne olacağını görmek için VMNetwork1 NewYork VMNetwork1-Chicago, aşağıdaki ayarlarla eşleştiğinden emin varsayalım.


**VM** | **VM ağına bağlı**
---|---
VM1 | VMNetwork1-Network
VM2 (VM1'in çoğaltması) | VMNetwork1 Chicago

Bu ayarlarla olası senaryolar birkaç içinde neler gözden geçirelim.

**Senaryo** | **Sonucu**
---|---
Yük devretme işleminden sonra VM-2 ağ özelliklerini değişiklik. | 1. VM kaynak ağına bağlı kalır.
VM-2 ağ özelliklerini, yük devretme işleminden sonra değiştirilir ve kesilir. | 1. VM bağlantısı kesildi.
VM-2 ağ özelliklerinin, yük devretme işleminden sonra değiştirilir ve VMNetwork2 Şikago'bağlanır. | 1. VM VMNetwork2 Chicago eşlenmediği ise kesilir.
Ağ eşlemesi VMNetwork1 Chicago değiştirilir. | VM-1'de artık VMNetwork1 Şikago'eşlenen ağ bağlanır.



## <a name="next-steps"></a>Sonraki adımlar

- [Hakkında bilgi edinin](hyper-v-vmm-networking.md) VMM ikincil siteye yük devretmenin ardından IP adresini.
- [Hakkında bilgi edinin](concepts-on-premises-to-azure-networking.md) azure'a yük devretmenin ardından IP adreslemesi.
