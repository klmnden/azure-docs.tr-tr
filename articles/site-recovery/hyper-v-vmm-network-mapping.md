---
title: Azure Site Recovery ile (VMM ile) Hyper-V VM çoğaltması için Ağ eşlemesi hakkında | Microsoft Docs
description: Azure Site Recovery ile VMM bulutlarında yönetilen Hyper-V Vm'lerini çoğaltma için Ağ eşlemesi ayarlamak açıklar.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 05/02/2018
ms.author: raynew
ms.openlocfilehash: fa596bf4941ac791fa1bc697399a4591d97ba68f
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
ms.locfileid: "34076375"
---
# <a name="prepare-network-mapping-for-hyper-v-vm-replication-to-azure"></a>Hyper-V sanal makinelerini Azure’a çoğaltma işlemi için ağ eşlemesini hazırlama


Bu makalede anlamak ve System Center Virtual Machine Manager (VMM) bulutlarında Hyper-V Vm'lerini azure'a veya ikincil bir siteye çoğaltma olduğunda ağ eşlemesi için hazırlanma yardımcı olacak kullanarak [Azure Site Recovery](site-recovery-overview.md) hizmet.


## <a name="prepare-network-mapping-for-replication-to-azure"></a>Ağ eşlemesi Azure'a çoğaltma için hazırlanma

Ne zaman Azure, bir kaynak VMM sunucusunda VM ağları arasında ağ eşlemesi eşlemeleri çoğaltma yapıyorsanız ve hedef Azure sanal ağları. Eşleme şunları yapar:
    -  **Ağ bağlantısı**— çoğaltılan Azure Vm'lerinin eşlenen ağa bağlanmasını sağlar. Bunlar farklı kurtarma planları devredilir olsa bile aynı ağda yük devri tüm makineler birbirine bağlanabilir.
    - **Ağ geçidi**— bir ağ geçidi hedef Azure ağında ayarlanıp ayarlanmadığını VM'ler diğer şirket içi sanal makinelere bağlanabilir.

Ağ eşlemesi gibi çalışır:

- Bir Azure sanal ağı için bir kaynak VMM VM ağ eşleyin.
- Azure VM'ler kaynak yük devretme sonrasında ağ eşlenmiş hedef sanal ağa bağlanır.
- Kaynak VM ağı'na eklenen yeni VM'ler, Çoğaltma gerçekleştiğinde eşlenen Azure ağına bağlanır.
- Hedef ağın birden çok alt ağı varsa ve bu alt ağlardan biri kaynak sanal makinenin bulunduğu alt ağ ile aynı adı taşıyorsa, çoğaltılan sanal makine yük devretme işleminin ardından hedef alt ağa bağlanır.
- Eşleşen ada sahip bir hedef alt ağ yoksa sanal makine ağdaki ilk alt ağa bağlanır.

## <a name="prepare-network-mapping-for-replication-to-a-secondary-site"></a>Ağ eşlemesi ikincil bir siteye çoğaltma için hazırlanma

İkincil bir siteye çoğaltma yapıyorsanız, bir kaynak VMM sunucusunda VM ağları ve bir hedef VMM sunucusunda VM ağları arasında ağ eşlemesi eşler. Eşleme şunları yapar:

- **Ağ bağlantısı**— VM'ler yük devretme sonrasında uygun ağlara bağlanır. Çoğaltma VM kaynak ağa eşlenmiş hedef ağa bağlanır.
- **En iyi VM yerleştirme**— çoğalma VM'ler Hyper-V ana bilgisayar sunucuları üzerinde en iyi şekilde yerleştirir. Çoğaltma sanal makineleri, eşlenen VM ağlarına erişebilen Konaklara yerleştirilir.
- **Ağ eşleme**— ağ eşlemesini yapılandırmazsanız, çoğaltma sanal makineleri herhangi bir VM ağına yük devretme sonrasında bağlanmayacaktır.

Ağ eşlemesi gibi çalışır:

- İki VMM sunucusu veya iki site aynı sunucu tarafından yönetiliyorsa tek bir VMM sunucusunda VM ağları arasında ağ eşlemesi yapılandırılabilir.
- Ne zaman eşleme doğru bir şekilde yapılandırıldığından ve çoğaltma etkinleştirilmiş VM birincil konumda bir ağa bağlı olması ve onun çoğaltması hedef konumda eşlenen ağa bağlanır.
- Ağ eşlemesi Site Kurtarma sırasında hedef VM ağ seçtiğinizde, kaynak VM ağı kullanan VMM kaynak Bulutları, koruma için kullanılan hedef bulut kullanılabilir hedef VM ağlarında birlikte görüntülenir.
- Hedef ağın birden çok alt ağı varsa ve bu alt ağlardan biri kaynak sanal makinenin bulunduğu alt ağ ile aynı ada sahip, ardından çoğaltma VM'de yük devretme işleminden sonra hedef alt ağa bağlanır. Eşleşen ada sahip bir hedef alt ağ varsa, VM ağındaki ilk alt ağa bağlanır.

## <a name="example"></a>Örnek

Burada, bu mekanizma göstermek için bir örnek verilmiştir. New York ve Şikago iki konumda bulunduğu bir kuruluşta atalım.

**Konum** | **VMM sunucusu** | **VM ağları** | **Eşlenen**
---|---|---|---
New York | VMM NewYork| VMNetwork1-NewYork | VMNetwork1 Şikago'eşlenmiş
 |  | VMNetwork2-NewYork | Eşlenmedi
Chicago | VMM Chicago| VMNetwork1 Chicago | VMNetwork1-NewYork eşlenmiş
 | | VMNetwork2 Chicago | Eşlenmedi

Bu örnekte:

- VM VMNetwork1-NewYork bağlı herhangi bir VM için oluşturulan bir çoğaltma, VMNetwork1 Şikago'bağlanır.
- VM VMNetwork2 NewYork veya VMNetwork2 Chicago için oluşturulmuş bir çoğaltma, herhangi bir ağa bağlanmayacaktır.

İşte nasıl VMM Bulutları bizim örnek kuruluş ve bulutlarıyla ilişkili mantıksal ağlar olarak ayarlanır.

### <a name="cloud-protection-settings"></a>Bulut koruma ayarlarını

**Korumalı bulut** | **Bulut koruma** | **Mantıksal ağ (New York)**  
---|---|---
GoldCloud1 | GoldCloud2 |
SilverCloud1| SilverCloud2 |
GoldCloud2 | <p>NA</p><p></p> | <p>LogicalNetwork1 NewYork</p><p>LogicalNetwork1 Chicago</p>
SilverCloud2 | <p>NA</p><p></p> | <p>LogicalNetwork1 NewYork</p><p>LogicalNetwork1 Chicago</p>

### <a name="logical-and-vm-network-settings"></a>Mantıksal ve VM ağ ayarları

**Konum** | **Mantıksal ağ** | **İlişkili bir VM ağı**
---|---|---
New York | LogicalNetwork1 NewYork | VMNetwork1-NewYork
Chicago | LogicalNetwork1 Chicago | VMNetwork1 Chicago
 | LogicalNetwork2Chicago | VMNetwork2 Chicago

### <a name="target-network-settings"></a>Hedef ağ ayarları

Hedef VM ağ seçeneğini belirlediğinizde bu ayarları temel alarak, aşağıdaki tabloda kullanılabilir seçenekler gösterilmektedir.

**Seç** | **Korumalı bulut** | **Bulut koruma** | **Hedef ağ yok**
---|---|---|---
VMNetwork1 Chicago | SilverCloud1 | SilverCloud2 | Kullanılabilir
 | GoldCloud1 | GoldCloud2 | Kullanılabilir
VMNetwork2 Chicago | SilverCloud1 | SilverCloud2 | Kullanılamıyor
 | GoldCloud1 | GoldCloud2 | Kullanılabilir


Hedef ağın birden çok alt ağı varsa ve bu alt ağlardan biri kaynak sanal makinenin bulunduğu alt ağ ile aynı ada sahip, ardından çoğaltma sanal makinesi yük devretme işleminden sonra hedef alt ağa bağlanır. Eşleşen ada sahip bir hedef alt ağ yoksa sanal makine ağdaki ilk alt ağa bağlanır.


### <a name="failback-behavior"></a>Yeniden çalışma davranışı

Yeniden çalışma (çoğaltmayı tersine çevirme) söz konusu olduğunda neler görmek için VMNetwork1 NewYork VMNetwork1-Chicago, aşağıdaki ayarlarla eşleştiğinden emin varsayalım.


**VM** | **VM ağına bağlı**
---|---
VM1 | VMNetwork1 ağ
VM2 (VM1 çoğaltma) | VMNetwork1 Chicago

Şimdi bu ayarlarla olası senaryolar birkaç içinde neler gözden geçirin.

**Senaryo** | **Sonucu**
---|---
Yük devretme işleminden sonra VM-2 Ağ özelliklerinde değişiklik. | VM 1 kaynak ağına bağlı kalır.
VM-2 ağ özellikleri yük devretme işleminden sonra değiştirilir ve bağlantısı kesilir. | VM 1 kesilir.
VM-2 ağ özellikleri yük devretme işleminden sonra değiştirilir ve VMNetwork2 Şikago'bağlanır. | VMNetwork2 Chicago eşlenmediği olduysa, VM-1 kesilecektir.
Ağ eşlemesi VMNetwork1 Chicago değiştirilir. | VM-1, şimdi VMNetwork1 Şikago'eşlenen ağa bağlanır.



## <a name="next-steps"></a>Sonraki adımlar

- [Hakkında bilgi edinin](hyper-v-vmm-networking.md) ikincil VMM sitesi için yük devretme sonrasında IP adresleme.
- [Hakkında bilgi edinin](concepts-on-premises-to-azure-networking.md) azure'a yük devretme sonrasında IP adresleme.
