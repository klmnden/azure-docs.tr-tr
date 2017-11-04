---
title: "Ağ eşlemesi Site Recovery ile Hyper-V VM çoğaltması için planlama | Microsoft Docs"
description: "Hyper-V sanal makine çoğaltmasını bir şirket içi veri merkezi Azure'a veya ikincil bir site için Ağ eşlemesi ayarlayın."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: tysonn
ms.assetid: fcaa2f52-489d-4c1c-865f-9e78e000b351
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 10/30/2017
ms.author: raynew
ms.openlocfilehash: 91d6d0466789daa662162c60bc3c97ba6115e7eb
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="plan-network-mapping-for-hyper-v-vm-replication-with-site-recovery"></a>Ağ eşlemesi Site Recovery ile Hyper-V VM çoğaltması için planlama



Bu makalede anlamak ve Hyper-V Vm'lerini azure'a veya ikincil bir siteye çoğaltma sırasında eşleme, kullanarak ağ için planlama yardımcı olacak [Azure Site Recovery hizmeti](site-recovery-overview.md).

Okuma sonra bu makalede bu makalenin sonundaki yorumları gönderin ya da teknik sorular [Azure kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="network-mapping-for-replication-to-azure"></a>Azure'a çoğaltma için Ağ eşlemesi

Ağ eşlemesi Hyper-V Vm'lerini (VMM yönetilen) çoğaltma Azure için kullanılır. Bir kaynak VMM sunucusunda VM ağları arasındaki eşlemeyi eşlemeleri ağ ve hedef Azure ağları. Eşleme şunları yapar:

- **Ağ bağlantısı**— çoğaltılan Azure Vm'lerinin eşlenen ağa bağlanmasını sağlar. Bunlar farklı kurtarma planları devredilir olsa bile aynı ağda yük devri tüm makineler birbirine bağlanabilir.
- **Ağ geçidi**— bir ağ geçidi hedef Azure ağında ayarlanıp ayarlanmadığını VM'ler diğer şirket içi sanal makinelere bağlanabilir.

Şunlara dikkat edin:

- Bir Azure sanal ağı için bir kaynak VMM VM ağ eşleyin.
- Azure VM'ler kaynak yük devretme sonrasında ağ eşlenmiş hedef sanal ağa bağlanır.
- Kaynak VM ağı'na eklenen yeni VM'ler, Çoğaltma gerçekleştiğinde eşlenen Azure ağına bağlanır.
- Hedef ağın birden çok alt ağı varsa ve bu alt ağlardan biri kaynak sanal makinenin bulunduğu alt ağ ile aynı adı taşıyorsa, çoğaltılan sanal makine yük devretme işleminin ardından hedef alt ağa bağlanır.
- Eşleşen ada sahip bir hedef alt ağ yoksa sanal makine ağdaki ilk alt ağa bağlanır.


## <a name="network-mapping-for-replication-to-a-secondary-datacenter"></a>Ağ eşlemesi için ikincil veri merkezine çoğaltma

Ağ eşleme (System Center Virtual Machine Manager (VMM) yönetilen) Hyper-V Vm'lerini ikincil bir veri merkezine çoğaltma yapılırken kullanılır. Bir kaynak VMM sunucusunda VM ağları ve bir hedef VMM sunucusunda VM ağları arasında ağ eşlemesi eşler. Eşleme şunları yapar:

- **Ağ bağlantısı**— VM'ler yük devretme sonrasında uygun ağlara bağlanır. Çoğaltma VM kaynak ağa eşlenmiş hedef ağa bağlanır.
- **En iyi yerleştirme**— çoğalma VM'ler Hyper-V ana bilgisayar sunucuları üzerinde en iyi şekilde yerleştirir. Çoğaltma sanal makineleri, eşlenen VM ağlarına erişebilen Konaklara yerleştirilir.
- **Ağ eşleme**— ağ eşlemesini yapılandırmazsanız, çoğaltma sanal makineleri herhangi bir VM ağına yük devretme sonrasında bağlanmayacaktır.

Şunlara dikkat edin:

- İki VMM sunucusu veya iki site aynı sunucu tarafından yönetiliyorsa tek bir VMM sunucusunda VM ağları arasında ağ eşlemesi yapılandırılabilir.
- Ne zaman eşleme doğru bir şekilde yapılandırıldığından ve çoğaltma etkinleştirilmiş VM birincil konumda bir ağa bağlı olması ve onun çoğaltması hedef konumda eşlenen ağa bağlanır.
-
- Bir hedef VM ağı sırasında ağ eşlemesini seçtiğinizde ağlar doğru VMM'de ayarlanan, kaynak VM ağı kullanan VMM kaynak Bulutları, koruma için kullanılan hedef bulut kullanılabilir hedef VM ağlarında birlikte görüntülenir .
- Hedef ağın birden çok alt ağı varsa ve bu alt ağlardan biri kaynak sanal makinenin bulunduğu alt ağ ile aynı ada sahip, ardından çoğaltma sanal makinesi yük devretme işleminden sonra hedef alt ağa bağlanır. Eşleşen ada sahip bir hedef alt ağ yoksa sanal makine ağdaki ilk alt ağa bağlanır.



### <a name="example"></a>Örnek

Burada, bu mekanizma göstermek için bir örnek verilmiştir. New York ve Şikago iki konumda bulunduğu bir kuruluşta atalım.

**Konum** | **VMM sunucusu** | **VM ağları** | **Eşlenen**
---|---|---|---
New York | VMM NewYork| VMNetwork1 NewYork | VMNetwork1 Şikago'eşlenmiş
 |  | VMNetwork2 NewYork | Eşlenmedi
Chicago | VMM Chicago| VMNetwork1 Chicago | VMNetwork1-NewYork eşlenmiş
 | | VMNetwork1 Chicago | Eşlenmedi

Bu örnekte:

- Bir çoğaltma sanal makinesi için VMNetwork1-NewYork bağlı herhangi bir sanal makine oluşturulduğunda, VMNetwork1 Şikago'bağlanır.
- Bir çoğaltma sanal makinesi VMNetwork2 NewYork veya VMNetwork2 Chicago oluşturulduğunda, herhangi bir ağa bağlı.

İşte nasıl VMM Bulutları bizim örnek kuruluş ve bulutlarıyla ilişkili mantıksal ağlar olarak ayarlanır.

#### <a name="cloud-protection-settings"></a>Bulut koruma ayarlarını

**Korumalı bulut** | **Bulut koruma** | **Mantıksal ağ (New York)**  
---|---|---
GoldCloud1 | GoldCloud2 |
SilverCloud1| SilverCloud2 |
GoldCloud2 | <p>NA</p><p></p> | <p>LogicalNetwork1 NewYork</p><p>LogicalNetwork1 Chicago</p>
SilverCloud2 | <p>NA</p><p></p> | <p>LogicalNetwork1 NewYork</p><p>LogicalNetwork1 Chicago</p>

#### <a name="logical-and-vm-network-settings"></a>Mantıksal ve VM ağ ayarları

**Konum** | **Mantıksal ağ** | **İlişkili bir VM ağı**
---|---|---
New York | LogicalNetwork1 NewYork | VMNetwork1 NewYork
Chicago | LogicalNetwork1 Chicago | VMNetwork1 Chicago
 | LogicalNetwork2Chicago | VMNetwork2 Chicago

#### <a name="target-network-settings"></a>Hedef ağ ayarları

Hedef VM ağ seçeneğini belirlediğinizde bu ayarları temel alarak, aşağıdaki tabloda kullanılabilir seçenekler gösterilmektedir.

**Seç** | **Korumalı bulut** | **Bulut koruma** | **Hedef ağ yok**
---|---|---|---
VMNetwork1 Chicago | SilverCloud1 | SilverCloud2 | Kullanılabilir
 | GoldCloud1 | GoldCloud2 | Kullanılabilir
VMNetwork2 Chicago | SilverCloud1 | SilverCloud2 | Kullanılamıyor
 | GoldCloud1 | GoldCloud2 | Kullanılabilir


Hedef ağın birden çok alt ağı varsa ve bu alt ağlardan biri kaynak sanal makinenin bulunduğu alt ağ ile aynı ada sahip, ardından çoğaltma sanal makinesi yük devretme işleminden sonra hedef alt ağa bağlanır. Eşleşen ada sahip bir hedef alt ağ yoksa sanal makine ağdaki ilk alt ağa bağlanır.


#### <a name="failback-behavior"></a>Yeniden çalışma davranışı

Yeniden çalışma (çoğaltmayı tersine çevirme) söz konusu olduğunda neler görmek için VMNetwork1 NewYork VMNetwork1-Chicago, aşağıdaki ayarlarla eşleştiğinden emin varsayalım.


**Sanal makine** | **VM ağına bağlı**
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

Hakkında bilgi edinin [ağ altyapısını planlama](site-recovery-network-design.md).
