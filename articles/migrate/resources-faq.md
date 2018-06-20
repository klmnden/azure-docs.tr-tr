---
title: Azure geçirmek - sık sorulan sorular (SSS) | Microsoft Docs
description: Adresleri Azure geçirme hakkında sık sorulan sorular
author: snehaamicrosoft
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 06/06/2018
ms.author: snehaa
ms.openlocfilehash: a18cab73a019039bf5e5829ad1faa4b8f1a70391
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36209954"
---
# <a name="azure-migrate---frequently-asked-questions-faq"></a>Azure geçirmek - sık sorulan sorular (SSS)

Bu makale Azure geçirme hakkında sık sorulan sorular içerir. Bu makaleyi okuduktan sonra herhangi bir sorgu varsa, yayınlayın [Azure geçişi Forumu](http://aka.ms/AzureMigrateForum).

## <a name="general"></a>Genel

### <a name="how-is-azure-migrate-different-from-azure-site-recovery"></a>Nasıl Azure geçirmek Azure Site Recovery'den farkı nedir?

Azure geçirme, şirket içi iş yüklerini bulabilir ve Azure, geçişi planlamanıza yardımcı olacak bir değerlendirme hizmetidir. [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/migrate-tutorial-on-premises-azure), olağanüstü durum kurtarma çözümü olmasının yanı sıra, şirket içi iş yüklerini Azure Iaas vm'lerine geçirmek yardımcı olur. 

### <a name="how-is-azure-migrate-different-from-azure-site-recovery-deployment-planner"></a>Nasıl Azure geçirmek Azure Site Recovery dağıtımı Planlayıcı'dan farklı mı?

Azure geçirme geçiş aracı planlaması ve Azure Site Recovery dağıtımı planlayıcısı aracı planlama bir olağanüstü durum kurtarma (DR).

**Azure VMware geçiş**: şirket içi iş yüklerini Azure'a geçirmek istiyorsanız, Azure geçirmek geçiş planlaması için kullanın. Azure geçirme şirket içi iş yüklerini değerlendirir ve rehberlik, Öngörüler ve Azure'a geçirme yardımcı olmak için mekanizma sağlar. Geçiş planınızla hazır olduğunuzda, Azure Site Recovery ve Azure veritabanı geçiş hizmeti gibi hizmetleri makineleri Azure'a geçirmek için kullanabilirsiniz.

**Azure Hyper-V geçiş**: Azure geçirmek şu anda yalnızca destekler değerlendirme VMware sanal makinelerinin Azure geçişi için. Hyper-V desteği Azure geçirmek için yol haritası açıktır. Bu arada, ASR dağıtım Planlayıcısı kullanabilirsiniz. Hyper-V desteği Azure geçirmek etkinleştirildikten sonra Azure geçirmek Hyper-V iş yükleri geçişini planlama için kullanabilirsiniz.

**VMware/Hyper-V olağanüstü durum kurtarma Azure**: olağanüstü durum kurtarma (DR) Azure Site Kurtarma (ASR) kullanarak Azure yapmak istiyorsanız, ASR dağıtım Planlayıcısı planlama DR için kullanın. ASR dağıtım Planlayıcısı şirket içi ortamınızın derin, ASR özgü bir değerlendirme yapar. ASR tarafından çoğaltma, sanal makinelerin yük devretme gibi başarılı DR işlemleri için gerekli olan ilgili öneriler sunulur.  

### <a name="does-azure-migrate-need-vcenter-server-to-discover-a-vmware-environment"></a>Azure geçirmek vCenter Server VMware ortamı bulmak için gerekiyor mu?

Evet, Azure geçirmek vCenter VMware ortamı bulmak için sunucu gerektirir. VCenter Server tarafından yönetilmeyen ESXi konakları bulunmasını desteklemez.

## <a name="discovery"></a>Bulma

### <a name="what-data-is-collected-by-azure-migrate"></a>Hangi verilerin Azure geçirmek tarafından toplanır?

Azure geçirme bulma, Gereci tabanlı bulma ve aracı tabanlı bulma iki tür destekler.
Gereci tabanlı bulma şirket içi sanal makineleri hakkındaki meta verileri toplar, tam listesi Gereci tarafından toplanan meta veriler, aşağıda listelenmiştir:

**VM yapılandırma verileri**
- VM görüntü adına (vCenter)
- VM envanteri yolu (konak/küme/klasöründe vCenter)
- IP adresi
- MAC adresi
- İşletim sistemi
- Çekirdek, diskleri, NIC sayısı
- Bellek boyutunu, Disk boyutları

**VM performans verileri**
- CPU kullanımı
- Bellek kullanımı
- VM'ye bağlı her disk için:
  - Disk üretilen işi okuma
  - Disk yazma üretimi
  - Disk okuma işlemleri sayısı / sn
  - Disk yazma işlemlerini sayısı / sn
- Her ağ bağdaştırıcısı için VM'ye bağlı:
  - Ağ içine
  - Ağ dışına

Aracı tabanlı bulma üstünde Gereci tabanlı bulma kullanılabilen bir seçenektir ve müşteriler yardım eden [bağımlılıkları görselleştirmek](how-to-create-group-machine-dependencies.md) şirket içi VM'lerin. Bağımlılık aracıları gibi FQDN, işletim sistemi, IP ayrıntıları toplamak adresi, MAC adresi, sanal makineden VM ve gelen ve giden TCP bağlantıları içinde çalışan işlemler. Aracı tabanlı bulma isteğe bağlıdır ve sanal makineleri bağımlılıkları görselleştirmek istemiyorsanız aracıları yüklememeyi seçebilirsiniz.

### <a name="where-is-the-collected-data-stored-and-for-how-long"></a>Toplanan verileri uzun depolanan ve hakkında bilgi için nerede?

Toplayıcı Gereci tarafından toplanan veriler geçiş projesi oluştururken belirttiğiniz Azure konumda depolanır. Verileri Microsoft abonelikte güvenli bir şekilde depolanır ve kullanıcı Azure geçirmek proje sildiğinde silinir.

Sanal makinelerin, aracıları yüklerseniz bağımlılık görselleştirme için kullanıcının aboneliği için oluşturulan bir OMS çalışma alanında ABD bağımlılık aracıları tarafından toplanan veriler depolanır. Kullanıcı kendi veya kendi Abonelikteki OMS çalışma siler sonra bu verileri silinir. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/migrate/concepts-dependency-visualization).

### <a name="how-does-the-collector-communicate-with-the-vcenter-server-and-the-azure-migrate-service"></a>Toplayıcı, vCenter Server ve Azure geçiş hizmeti ile nasıl iletişim?

VCenter sunucusuna (bağlantı noktası 443) Toplayıcı Gereci bağlayan Gereci kullanıcı tarafından sağlanan kimlik bilgilerini kullanarak. VCenter Server vCenter Server tarafından yönetilen sanal makineleri hakkındaki meta verileri toplamak için VMware Powerclı kullanarak sorgular. Bu her iki yapılandırma hakkında bilgi VM'ler (çekirdek, bellek, diskler, NIC vb.) yanı sıra her VM performans geçmişi son bir ay süreyle vCenter Server'dan toplar. Toplanan meta veriler daha sonra değerlendirmesi için Azure geçiş hizmetine (üzerinden internet https üzerinden) gönderilir. [Daha fazla bilgi](concepts-collector.md)

### <a name="is-the-data-encrypted-at-rest-and-while-in-transit"></a>Veriler, aktarım sırasında şifrelenir ve şifrelenir?

Evet, toplanan veriler REST hem aktarım sırasında şifrelenir. Gereci tarafından toplanan meta veriler güvenli bir şekilde Azure geçirmek hizmete Internet üzerinden https gönderilir. Toplanan meta depolanan [Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/database-encryption-at-rest) ve [Azure blob depolama](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) bir Microsoft abonelikte ve bekleme sırasında şifrelenir.

Bağımlılık aracıları tarafından toplanan veriler de içinde şifrelenmiş transit (güvenli https kanalı) ve kullanıcının abonelik günlük analizi çalışma depolanır. Ayrıca, bekleyen de şifrelenir.

### <a name="how-can-i-discover-a-multi-tenant-environment-in-azure-migrate"></a>Bir çok kiracılı ortamında Azure geçirmek nasıl bulabilmesi için?

Kiracılar arasında paylaşılan bir ortamda varsa ve başka bir kiracının Abonelikteki bir kiracı VM'ler bulmak istediğinizde değil, bulma kapsamı için toplayıcı Gereci kapsam alanında kullanabilirsiniz. Kiracılar konakları paylaşıyorsanız, yalnızca belirli kiracıya ait sanal makineleri salt okunur erişimi olan bir kimlik bilgisi oluşturun ve sonra Toplayıcı Gereci bu kimlik bilgilerini kullanın ve kapsam bulma yapmak için ana bilgisayar olarak belirtin. Alternatif olarak, vcenter Server'daki (tenant1 Klasör1 ve tenant2 klasör2 düşünelim), paylaşılan konak altında klasör oluşturabilirsiniz ve tenant2 Klasör1 içine tenant1 için sanal makineleri klasör2 taşıyın ve bulmaları Toplayıcı, buna göre kapsam uygun klasörü belirterek.

### <a name="how-many-virtual-machines-can-be-discovered-in-a-single-migration-project"></a>Kaç tane sanal makineyi bir tek geçiş projesinde bulunan?

1500 sanal makineleri tek geçiş projesinde bulabilir. Daha fazla makine şirket içi ortamınız varsa [daha fazla bilgi edinin](how-to-scale-assessment.md) hakkında Azure geçirmek büyük ortamında nasıl bulabilir.

## <a name="dependency-visualization"></a>Bağımlılık görselleştirme

### <a name="do-i-need-to-pay-to-use-the-dependency-visualization-feature"></a>Bağımlılık görselleştirme özelliği kullanmak ödeme gerekiyor mu?

Azure Geçişi ek ücret ödenmeden kullanılabilir. Azure Geçişi fiyatlandırması hakkında daha fazla bilgiyi [burada](https://azure.microsoft.com/pricing/details/azure-migrate/) bulabilirsiniz.

### <a name="can-i-use-an-existing-workspace-for-dependency-visualization"></a>Var olan bir çalışma bağımlılık görselleştirme için kullanabilir miyim?

Azure geçirme bağımlılık görselleştirme için varolan bir çalışma alanını kullanarak desteklemez, ancak, Microsoft İzleme Aracısı'nı (MMA) birden çok giriş destekler ve birden çok çalışma alanına veri göndermenizi sağlar. Bu nedenle dağıtılmış ve bir çalışma alanına yapılandırılmış aracıları zaten varsa, MMA aracısında birden çok giriş yararlanın ve bunu Azure geçirmek çalışma alanına (ek olarak mevcut çalışma) yapılandırabilir ve çalışması. [Burada](https://blogs.technet.microsoft.com/msoms/2016/05/26/oms-log-analytics-agent-multi-homing-support/) MMA aracısında birden çok giriş nasıl etkinleştirebilirsiniz üzerinde blogu.

## <a name="next-steps"></a>Sonraki adımlar

- Okuma [Azure geçirmek genel bakış](migrate-overview.md)
- Öğrenin [bulmak ve değerlendirmek](tutorial-assessment-vmware.md) VMware ortamı
