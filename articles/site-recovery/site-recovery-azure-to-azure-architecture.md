---
title: "Azure sanal makine çoğaltmasını Azure bölgeler arasında Azure Site Recovery nasıl çalışır?  | Microsoft Belgeleri"
description: "Bu makalede, bileşenleri ve Azure Site Recovery hizmetini kullanarak Azure bölgeler arasında Azure sanal makineleri çoğaltırken kullanılan mimariye genel bakış sağlar."
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/31/2017
ms.author: sujayt
ms.openlocfilehash: 7a3e1eb129db530295cbf28bbce1dbe48575d50d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-does-azure-vm-replication-work-in-site-recovery"></a>Azure VM çoğaltma Site Recovery nasıl çalışır?


Bu makalede bileşenleri ve işlemler çoğaltma ve Azure sanal makinelerini (VM'ler) Kurtarma bir bölgesinden diğerine kullanarak söz konusu [Azure Site Recovery](site-recovery-overview.md) hizmet.

>[!NOTE]
>Site Recovery hizmeti ile Azure VM çoğaltma şu anda önizlemede değil.

Tüm yorumlarınızı bu makalenin alt kısmında paylaşabilir veya [Azure Kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)'nda soru sorabilirsiniz.

## <a name="architectural-components"></a>Mimari bileşenler

Aşağıdaki diyagramda (Bu örnekte, Doğu ABD konumunda) belirli bir bölgede bir Azure VM ortamına üst düzey bir görünümünü sağlar. Bir Azure VM ortamda:
- Uygulamalar, diskleri depolama hesaplarında yayılan Vm'lerinde çalışıyor olabilir.
- Sanal makineleri, sanal ağ içindeki bir veya daha fazla alt ağlarda eklenebilir.

![Müşteri ortamı](./media/site-recovery-azure-to-azure-architecture/source-environment.png)

Dağıtım önkoşulları ve gereksinimleri hakkında bilgi edinin [destek matrisi](site-recovery-support-matrix-azure-to-azure.md).

## <a name="replication-process"></a>Çoğaltma işlemi

### <a name="step-1"></a>1. Adım

Azure portalında Azure VM çoğaltma etkinleştirdiğinizde, aşağıdaki diyagramda ve tabloda gösterilen kaynakları otomatik olarak hedef bölgede oluşturulur. Varsayılan olarak, kaynakları kaynak bölge ayarları temel alınarak oluşturulur. Hedef ayarları gerektiği gibi özelleştirebilirsiniz. [Daha fazla bilgi edinin](site-recovery-replicate-azure-to-azure.md).

![Çoğaltma işlemi, 1. adım etkinleştir](./media/site-recovery-azure-to-azure-architecture/enable-replication-step-1.png)

**Kaynak** | **Ayrıntılar**
--- | ---
**Hedef kaynak grubu** | Yük devretme sonrasında çoğaltılmış sanal makineleri ait olduğu kaynak grubu.
**Hedef sanal ağ** | Sanal ağ içinde çoğaltılmış VM'ler yük devretme sonrasında bulunur. Ağ eşlemesi, kaynak ve hedef sanal ağlar arasında ve tersi yönde oluşturulur.
**Önbellek depolama hesapları** | Kaynak sanal makineleri üzerinde yapılan değişiklikler için hedef depolama hesabı çoğaltılır önce bunlar izlenen ve hedef konumu önbelleği depolama hesabında gönderilir. Bu VM'de çalıştırılan üretim uygulamalar üzerinde en az etki sağlar.
**Hedef depolama hesapları**  | Veri çoğaltılan hedef konumdaki depolama hesapları.
**Hedef kullanılabilirlik kümeleri**  | Kullanılabilirlik kümeleri, yük devretme sonrasında çoğaltılmış VM'ler bulunur.

### <a name="step-2"></a>2. Adım

Çoğaltma etkin olarak Site Recovery uzantısı Mobility hizmeti VM üzerinde otomatik olarak yüklenir. Aşağıdakiler gerçekleşir:

1. VM Site Recovery ile kaydedilir.

2. Sürekli çoğaltma VM için yapılandırılır. VM disklerde veri yazma sürekli olarak kaynak konumu önbelleği depolama hesabında aktarılır.

   ![Çoğaltma işlemi, 2. adım etkinleştir](./media/site-recovery-azure-to-azure-architecture/enable-replication-step-2.png)

   >[!IMPORTANT]
   > Site Recovery hiçbir zaman VM'ye gelen bağlantısı gerekir. VM Site Recovery hizmeti URL'leri/IP adresleri, Office 365 kimlik doğrulaması URL'lerini/IP adresleri ve önbellek depolama hesabı IP adresleri yalnızca giden bağlantı gerekir. Daha fazla bilgi için bkz: [Azure sanal makineleri çoğaltmak için Ağ Kılavuzu](site-recovery-azure-to-azure-networking-guidance.md) makalesi.

## <a name="continuous-replication-process"></a>Sürekli çoğaltma işlemi

Sürekli çoğaltma çalışmaya başladıktan sonra disk yazma işlemleri için önbellek depolama hesabı hemen aktarılır. Site Recovery verileri işler ve hedef depolama hesabı gönderir. Verilerin işlendikten sonra kurtarma noktaları birkaç dakikada bir hedef depolama hesabı oluşturulur.

## <a name="failover-process"></a>Yük devretme işlemi

Bir yük devretme başlatın, VM'ler hedef kaynak grubu içinde hedef sanal ağ, hedef alt oluşturulur ve hedef kullanılabilirlik kümesi. Bir yük devretme sırasında herhangi bir kurtarma noktası kullanabilirsiniz.

![Yük devretme işlemi](./media/site-recovery-azure-to-azure-architecture/failover.png)

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [ağ](site-recovery-azure-to-azure-networking-guidance.md) Azure VM çoğaltması için.
- Bir kılavuz izleyin [Azure Vm'lerini çoğaltma.](site-recovery-azure-to-azure.md)
