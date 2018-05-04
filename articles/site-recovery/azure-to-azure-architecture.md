---
title: Azure Site kurtarma için Azure Azure çoğaltma mimarisi | Microsoft Docs
description: Bu makalede, bileşenleri ve Azure VM'ler Azure Site Recovery hizmetini kullanarak Azure bölgeler arasında çoğaltırken kullanılan mimariye genel bakış sağlar.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 02/07/2018
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: ffa60e24b93caaaefcab70c99fa2c76065d97233
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="azure-to-azure-replication-architecture"></a>Azure için Azure çoğaltma mimarisi


Çoğaltma, yük devri ve Azure sanal makineleri (VM'ler) kullanarak Azure bölgeler arasında Kurtarma sırasında kullanılan mimarisi bu makalede [Azure Site Recovery](site-recovery-overview.md) hizmet.

>[!NOTE]
>Site Recovery hizmeti ile Azure VM çoğaltma şu anda önizlemede değil.



## <a name="architectural-components"></a>Mimari bileşenler

Aşağıdaki grafikte (Bu örnekte, Doğu ABD konumunda) belirli bir bölgede bir Azure VM ortamına üst düzey bir görünümünü sağlar. Bir Azure VM ortamda:
- Uygulamaları sanal makinelerin yönetilen disklerle çalıştırıyor olabilir veya yönetilmeyen diskleri depolama hesaplarında yayılan.
- Sanal makineleri, sanal ağ içindeki bir veya daha fazla alt ağlarda eklenebilir.


**Azure için Azure'a çoğaltma**

![Müşteri ortamı](./media/concepts-azure-to-azure-architecture/source-environment.png)

## <a name="replication-process"></a>Çoğaltma işlemi

### <a name="step-1"></a>1. Adım

Azure VM çoğaltma etkinleştirdiğinizde, aşağıdaki kaynaklar otomatik olarak hedef bölgede Kaynak bölgesi ayarlara göre oluşturulur. Hedef kaynakları ayarları gerektiği gibi özelleştirebilirsiniz.

![Çoğaltma işlemi, 1. adım etkinleştir](./media/concepts-azure-to-azure-architecture/enable-replication-step-1.png)

**Kaynak** | **Ayrıntılar**
--- | ---
**Hedef kaynak grubu** | Yük devretme sonrasında çoğaltılmış sanal makineleri ait olduğu kaynak grubu. Bu kaynak grubu konumu, kaynak sanal makine barındırılan Azure bölgesi dışında tüm Azure bölgesindeki olabilir.
**Hedef sanal ağ** | Sanal ağ içinde çoğaltılmış VM'ler yük devretme sonrasında bulunur. Ağ eşlemesi, kaynak ve hedef sanal ağlar arasında ve tersi yönde oluşturulur.
**Önbellek depolama hesapları** | Kaynak VM değişikliklerini bir hedef depolama hesabına çoğaltılır önce bunlar izlenen ve kaynak konumu önbelleği depolama hesabında gönderilir. Bu adım VM'de çalıştırılan üretim uygulamalar üzerinde en az etki sağlar.
**(VM kullanmayan kaynak diskleri yönetiliyorsa) depolama hesapları hedef**  | Veri çoğaltılan hedef konumdaki depolama hesapları.
** Çoğaltma yönetilen diskleri (VM olduğu açık kaynak diskleri yönetiliyorsa) **  | Diskleri veri çoğaltılan hedef konumda yönetilen.
**Hedef kullanılabilirlik kümeleri**  | Kullanılabilirlik kümeleri, yük devretme sonrasında çoğaltılmış VM'ler bulunur.

### <a name="step-2"></a>2. Adım

Çoğaltma etkin olarak Site Recovery uzantısı Mobility hizmeti VM üzerinde otomatik olarak yüklenir:

1. VM Site Recovery ile kaydedilir.

2. Sürekli çoğaltma VM için yapılandırılır. VM disklerde veri yazma önbelleği depolama hesabında kaynak konumu için sürekli olarak aktarılır.

   ![Çoğaltma işlemi, 2. adım etkinleştir](./media/concepts-azure-to-azure-architecture/enable-replication-step-2.png)


 Site Recovery hiçbir zaman VM'ye gelen bağlantısı gerekir. Yalnızca giden bağlantılar için aşağıdakiler gereklidir.

 - Site Recovery hizmeti URL'leri/IP adresleri
 - Office 365 kimlik doğrulaması URL'lerini/IP adresleri
 - Önbellek depolama hesabı IP adresleri

Çoklu VM tutarlılığını etkinleştirirseniz çoğaltma grubundaki makineler birbiriyle 20004 numaralı bağlantı noktası üzerinden iletişim kurar. VM’ler arasında 20004 numaralı bağlantı noktası üzerinden gerçekleştirilen iç iletişimi engelleyen bir güvenlik duvarı gereci olmadığından emin olun.

> [!IMPORTANT]
Linux VM’lerinin çoğaltma grubunun bir parçası olmasını istiyorsanız, 20004 numaralı bağlantı noktası üzerinden giden trafiğin, belirli Linux sürümünün kılavuzuna göre el ile açıldığından emin olun.

### <a name="step-3"></a>3. Adım

Sürekli çoğaltma sürüyor sonra disk yazma işlemleri için önbellek depolama hesabı hemen aktarılır. Site Recovery verileri işler ve hedefe gönderir depolama hesabı ya da çoğaltma yönetilen diskler. Verilerin işlendikten sonra kurtarma noktaları birkaç dakikada bir hedef depolama hesabı oluşturulur.

## <a name="failover-process"></a>Yük devretme işlemi

Bir yük devretme başlatın, VM'ler hedef kaynak grubu, hedef sanal ağ, hedef alt ağ oluşturulur ve hedef kullanılabilirlik kümesinde. Bir yük devretme sırasında herhangi bir kurtarma noktası kullanabilirsiniz.

![Yük devretme işlemi](./media/concepts-azure-to-azure-architecture/failover.png)

## <a name="next-steps"></a>Sonraki adımlar

[Hızlı bir şekilde çoğaltmak](azure-to-azure-quickstart.md) bir ikincil bölge için bir Azure VM.
