---
title: "Azure sanal makinelerini çoğaltma Azure bölgeler arasında mimarisi gözden | Microsoft Docs"
description: "Bu makalede, bileşenleri ve Azure VM'ler Azure Site Recovery hizmetini kullanarak Azure bölgeler arasında çoğaltırken kullanılan mimariye genel bakış sağlar."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 09/10/2017
ms.author: raynew
ms.openlocfilehash: f9cd57e47a8463440148b2bcc6c21beacd54382a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-to-azure-replication-architecture"></a>Azure için Azure çoğaltma mimarisi


Bu makalede mimari ve çoğaltma, yük devri ve Azure sanal makineleri (VM'ler) kullanarak Azure bölgeler arasında Kurtarma sırasında kullanılan işlemleri [Azure Site Recovery](site-recovery-overview.md) hizmet.

>[!NOTE]
>Site Recovery hizmeti ile Azure VM çoğaltma şu anda önizlemede değil.



## <a name="architectural-components"></a>Mimari bileşenler

Aşağıdaki grafikte (Bu örnekte, Doğu ABD konumunda) belirli bir bölgede bir Azure VM ortamına üst düzey bir görünümünü sağlar. Bir Azure VM ortamda:
- Uygulamalar, diskleri depolama hesaplarında yayılan Vm'lerinde çalışıyor olabilir.
- Sanal makineleri, sanal ağ içindeki bir veya daha fazla alt ağlarda eklenebilir.


**Azure için Azure'a çoğaltma**

![Müşteri ortamı](./media/concepts-azure-to-azure-architecture/source-environment.png)

## <a name="replication-process"></a>Çoğaltma işlemi

### <a name="step-1"></a>1. Adım

Azure VM çoğaltma etkinleştirdiğinizde, aşağıda gösterilen kaynakları otomatik olarak hedef bölgede Kaynak bölgesi ayarlarına dayanarak oluşturulur. Hedef kaynakları ayarları gerektiği gibi özelleştirebilirsiniz. 

![Çoğaltma işlemi, 1. adım etkinleştir](./media/concepts-azure-to-azure-architecture/enable-replication-step-1.png)

**Kaynak** | **Ayrıntılar**
--- | ---
**Hedef kaynak grubu** | Yük devretme sonrasında çoğaltılmış sanal makineleri ait olduğu kaynak grubu.
**Hedef sanal ağ** | Sanal ağ içinde çoğaltılmış VM'ler yük devretme sonrasında bulunur. Ağ eşlemesi, kaynak ve hedef sanal ağlar arasında ve tersi yönde oluşturulur.
**Önbellek depolama hesapları** | Kaynak VM'ler değişiklikler bir hedef depolama hesabına çoğaltılır önce bunlar izlenen ve hedef konumu önbelleği depolama hesabında gönderilir. Bu VM'de çalıştırılan üretim uygulamalar üzerinde en az etki sağlar.
**Hedef depolama hesapları**  | Veri çoğaltılan hedef konumdaki depolama hesapları.
**Hedef kullanılabilirlik kümeleri**  | Kullanılabilirlik kümeleri, yük devretme sonrasında çoğaltılmış VM'ler bulunur.

### <a name="step-2"></a>2. Adım

Çoğaltma etkin olarak Site Recovery uzantısı Mobility hizmeti VM üzerinde otomatik olarak yüklenir:

1. VM Site Recovery ile kaydedilir.

2. Sürekli çoğaltma VM için yapılandırılır. VM disklerde veri yazma önbelleği depolama hesabında kaynak konumu için sürekli olarak aktarılır.

   ![Çoğaltma işlemi, 2. adım etkinleştir](./media/concepts-azure-to-azure-architecture/enable-replication-step-2.png)

  
 Site Recovery hiçbir zaman VM'ye gelen bağlantısı gerekir. Site Recovery hizmeti URL'leri/IP adresleri, Office 365 kimlik doğrulaması URL'lerini/IP adresleri ve önbellek depolama hesabı IP adresleri yalnızca giden bağlantılar gereklidir.

### <a name="step-3"></a>3. Adım

Sürekli çoğaltma sürüyor sonra disk yazma işlemleri için önbellek depolama hesabı hemen aktarılır. Site Recovery verileri işler ve hedef depolama hesabı gönderir. Verilerin işlendikten sonra kurtarma noktaları birkaç dakikada bir hedef depolama hesabı oluşturulur.

## <a name="failover-process"></a>Yük devretme işlemi

Bir yük devretme başlatın, VM'ler hedef kaynak grubu, hedef sanal ağ, hedef alt ağ oluşturulur ve hedef kullanılabilirlik kümesinde. Bir yük devretme sırasında herhangi bir kurtarma noktası kullanabilirsiniz.

![Yük devretme işlemi](./media/concepts-azure-to-azure-architecture/failover.png)

## <a name="next-steps"></a>Sonraki adımlar

İkincil bir bölgeye Azure VM çoğaltmayı etkinleştirmek için öğreticiyi izleyin destek matrisini inceleyin.
Bir yük devretme ve yeniden çalıştırın.