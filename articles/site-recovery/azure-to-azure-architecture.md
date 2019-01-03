---
title: Azure Site recovery'de Azure'a çoğaltma mimarisi | Microsoft Docs
description: Bu makalede, Azure Site Recovery hizmetini kullanarak Azure Vm'leri için Azure bölgeleri arasında olağanüstü durum kurtarma ayarlama çoğaltırken kullanılan bileşenler ve genel bir bakış sağlar.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 12/27/2018
ms.author: raynew
ms.openlocfilehash: bb67051ae969497b9e191c0ceb6c0271375e094e
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/27/2018
ms.locfileid: "53787687"
---
# <a name="azure-to-azure-disaster-recovery-architecture"></a>Azure'dan Azure'a olağanüstü durum kurtarma mimarisi


Bu makalede, çoğaltma, yük devretme ve kurtarma Azure sanal makinelerini (VM) kullanarak Azure bölgeleri arasında olağanüstü durum kurtarma dağıtırken kullanılan mimarisi [Azure Site Recovery](site-recovery-overview.md) hizmeti.




## <a name="architectural-components"></a>Mimari bileşenler

Aşağıdaki grafikte, belirli bir bölgede (Bu örnekte, Doğu ABD konumunda) bir Azure VM ortamında üst düzey bir görünümünü sağlar. Bir Azure VM ortamında:
- Uygulamaları yönetilen diskleri olan Vm'lerde çalışabilir veya yönetilmeyen diskler depolama hesabı arasında birçok şekilde yayılabilir.
- Vm'leri, sanal ağ içindeki bir veya daha fazla alt ağlarda eklenebilir.


**Azure'dan Azure'a çoğaltma**

![Müşteri ortamı](./media/concepts-azure-to-azure-architecture/source-environment.png)

## <a name="replication-process"></a>Çoğaltma işlemi

### <a name="step-1"></a>1. Adım

Azure VM çoğaltma etkinleştirdiğinizde aşağıdaki kaynaklar otomatik olarak hedef bölgede kaynak bölge ayarlara göre oluşturulur. Hedef kaynakları ayarları gerektiği gibi özelleştirebilirsiniz.

![Çoğaltma işlemi, 1. Adım'ı etkinleştir](./media/concepts-azure-to-azure-architecture/enable-replication-step-1.png)

**Kaynak** | **Ayrıntılar**
--- | ---
**Hedef kaynak grubu** | Yük devretme sonrasında çoğaltılmış VM'lerin ait olduğu kaynak grubu. Bu kaynak grubunun konumunu, kaynak sanal makineleri barındırılan Azure bölgesi dışında herhangi bir Azure bölgesinde olabilir.
**Hedef sanal ağ** | Sanal ağ, çoğaltılan VM'ler yük devretmeden sonra bulunur. Ağ eşleme, kaynak ve hedef sanal ağlar arasında ve bunun tersi de oluşturulur.
**Önbellek depolama hesapları** | Kaynak VM değişiklikler bir hedef depolama hesabına çoğaltılmadan önce sayfalar izlenir ve kaynak konumundaki önbellek depolama hesabına gönderilir. Bu adım, üretim uygulamaları VM'de çalıştırılan üzerinde en az etki sağlar.
**Depolama hesapları (kaynak sanal makine tarafından kullanılmayan yönetilen diskleri kullanmıyorsa) hedef**  | Depolama hesabı hedef konumda olduğu veriler çoğaltılır.
** Yönetilen çoğaltma diskleri (kaynak VM açıktır yönetilen diskleri kullanmıyorsa) **  | Yönetilen diskler çoğaltılan verileri için hedef konumunda.
**Hedef kullanılabilirlik kümeleri**  | Kullanılabilirlik kümeleri yük devretme sonrasında çoğaltılmış VM'lerin bulunduğu olan.

### <a name="step-2"></a>2. Adım

Çoğaltma etkin olarak, Site Recovery Mobility hizmeti uzantısı VM'de otomatik olarak yüklenir:

1. VM'nin Site Recovery ile kaydedilir.

2. Sürekli çoğaltma VM için yapılandırılır. VM disk üzerindeki veri yazma, sürekli olarak kaynak konumundaki önbellek depolama hesabına aktarılır.

   ![Çoğaltma işlemi, 2. Adım'ı etkinleştir](./media/concepts-azure-to-azure-architecture/enable-replication-step-2.png)


 Site Recovery, sanal makineye gelen bağlantılara hiçbir zaman işlemesi gerekmez. Yalnızca giden bağlantı aşağıdakiler için gereklidir.

 - Site Recovery hizmet URL'leri/IP adresleri
 - Office 365 kimlik doğrulaması URL'lerini/IP adresleri
 - Önbellek depolama hesabı IP adresleri

Çoklu VM tutarlılığını etkinleştirirseniz çoğaltma grubundaki makineler birbiriyle 20004 numaralı bağlantı noktası üzerinden iletişim kurar. VM’ler arasında 20004 numaralı bağlantı noktası üzerinden gerçekleştirilen iç iletişimi engelleyen bir güvenlik duvarı gereci olmadığından emin olun.

> [!IMPORTANT]
Linux VM’lerinin çoğaltma grubunun bir parçası olmasını istiyorsanız, 20004 numaralı bağlantı noktası üzerinden giden trafiğin, belirli Linux sürümünün kılavuzuna göre el ile açıldığından emin olun.

### <a name="step-3"></a>3. Adım

Sürekli çoğaltma sürüyor sonra disk yazmalarının hemen önbellek depolama hesabına aktarılır. Site Recovery verileri işler ve hedefe gönderir depolama hesabı veya çoğaltma yönetilen diskler. Kurtarma noktaları hedef depolama hesabında, veriler işlendikten sonra birkaç dakikada oluşturulur.

## <a name="failover-process"></a>Yük devretme işlemi

Bir yük devretme başlatın, VM'ler hedef kaynak grubu, hedef sanal ağ, hedef alt ağ oluşturulur ve hedef kullanılabilirlik kümesi. Yük devretme sırasında herhangi bir kurtarma noktasını kullanabilirsiniz.

![Yük devretme işlemi](./media/concepts-azure-to-azure-architecture/failover.png)

## <a name="next-steps"></a>Sonraki adımlar

[Hızlı bir şekilde çoğaltmak](azure-to-azure-quickstart.md) Azure VM'LERİNİ ikincil bir bölgeye.
