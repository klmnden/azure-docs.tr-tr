---
title: "Olağanüstü durum kurtarma gereksinimleri için Azure bölgeler arasında Azure Vm'lerini çoğaltma: Azure için Azure | Microsoft Docs"
description: "Olağanüstü durum kurtarma gereksinimleri için Azure Site Recovery hizmetiyle (Azure-Azure) Azure bölgeler arasında Azure sanal makineleri çoğaltmak için gereken adımları özetler."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmon
editor: 
ms.assetid: dab98aa5-9c41-4475-b7dc-2e07ab1cfd18
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: raynew
ms.openlocfilehash: 9ca33057f6030fdcc233f6053fdc392d62f8f9f4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-azure-vms-between-regions-with-azure-site-recovery"></a>Azure Site Recovery ile bölgeler arasında Azure Vm'lerini çoğaltma

>[!NOTE]
>
> Azure sanal makineleri (VM'ler) için Azure Site Recovery çoğaltma şu anda önizlemede değil.

Bu makalede kullanarak Azure bölgeler arasında Azure Vm'lerini çoğaltma açıklar [Site Recovery](site-recovery-overview.md) Azure portalında hizmet.

Bu makalenin veya üzerinde altındaki açıklamaları ve soruları sonrası [Azure kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="disaster-recovery-in-azure"></a>Azure olağanüstü durum kurtarma

Yerleşik Azure altyapı yetenekleri ve özellikleri Azure Vm'lerinde çalışan iş yüklerini için sağlam ve esnek kullanılabilirlik stratejisi katkıda bulunun. Ancak, Azure bölgeler arasında olağanüstü durum kurtarma için kendiniz planlama yapmanız neden pek çok nedeni vardır:

- Belirli uygulamalar ve iş devamlılığı ve olağanüstü durum kurtarma (BCDR) stratejinize gerektiren iş yükleri için özel uyumluluk yönergelerine karşılaması gerekir.
- Koruma ve yerleşik Azure işlevselliğine bağlı Azure VM'ler, iş kararları göre ve değil yalnızca kurtarma olanağı istiyor.
- Yük devretme ve kurtarma üretim üzerinde hiçbir etkisi olmadan, iş ve uyumluluk gereksinimlerinize uygun olarak test etmeniz gerekir.
- Üzerinden bir olağanüstü durumda kurtarma bölgesine başarısız ve özgün kaynak bölge sorunsuz bir şekilde başarısız gerekir.

Bu görevleri gerçekleştirmenize yardımcı olmak için Site Recovery Azure Azure VM çoğaltması için kullanın.


## <a name="why-use-site-recovery"></a>Neden Site Recovery kullanmalısınız?      

Site Recovery bölgeler arasında Azure Vm'lerini çoğaltma için basit bir yol sunar:

- **Otomatik dağıtım**. Etkin-etkin çoğaltma modeli, ikincil bölge pahalı ve karmaşık bir altyapıda için gerek yoktur. Çoğaltmayı etkinleştirmek, Site Recovery otomatik olarak gerekli kaynakları hedef bölgede Kaynak bölgesi ayarlarınızı temel alan oluşturur.
- **Denetim bölgeleri**. Site Recovery ile bir kıtada içinde herhangi bir bölgeyi herhangi bölgesinden çoğaltabilir. Bu standart arasında zaman uyumsuz olarak çoğaltır okuma erişimli coğrafi olarak yedekli depolama karşılaştırmak [eşleştirilmiş bölgeleri](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) yalnızca. Coğrafi olarak yedekli depolamaya okuma erişimi hedef bölgedeki veri salt okunur erişim sağlar.
- **Çoğaltma otomatik**. Site Recovery otomatik sürekli çoğaltma sağlar. Yük devretme ve yeniden çalışma tek bir tıklatmayla tetiklenebilir.
- **RTO ve RPO**. Site Recovery RTO ve RPO çok düşük tutmak için bölgeler bağlanan Azure ağ altyapısı yararlanır.
- **Sınama**. Olağanüstü durum kurtarma ayrıntısına gerektiğinde, üretim iş yükleri veya devam eden çoğaltmayı etkilemeden gereken isteğe bağlı yük devretme testlerini çalıştırabilirsiniz.
- **Kurtarma planları**. Kurtarma planları, yük devretme ve yeniden çalışma birden çok VM'ler üzerinde çalıştırılan tüm uygulamasının düzenlemek için kullanabilirsiniz. Kurtarma planı Özelliği Azure Otomasyonu runbook'ları ile zengin birinci sınıf tümleştirme vardır.


## <a name="deployment-summary"></a>Dağıtım özeti

Sanal makineleri çoğaltma Azure bölgeler arasında ayarlamak için yapmanız gerekenler, bir özeti aşağıda verilmiştir:

1. Kurtarma Hizmetleri kasası oluşturun. Kasa yapılandırma ayarlarını içeren çoğaltma ve yönetir.
2. Azure sanal makinelerini çoğaltmayı etkinleştirin.
3. Her şeyin beklendiği gibi çalıştığından emin olmak için yük devretme testi çalıştırın.

>[!IMPORTANT]
>
> Kontrol edebilirsiniz [Azure VM çoğaltması için destek matrisi](./site-recovery-support-matrix-azure-to-azure.md).

>[!IMPORTANT]
>
> Site Recovery çoğaltma için Azure VM'ler için gerekli ağ giden bağlantı yapılandırma hakkında daha fazla bilgi için bkz: [Ağ Kılavuzu belge](./site-recovery-azure-to-azure-networking-guidance.md).

### <a name="before-you-start"></a>Başlamadan önce

* Azure kullanıcı hesabınızın belirli sahip olması [izinleri](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) bir Azure sanal makinesi çoğaltmayı etkinleştirmek için.
* Olağanüstü durum kurtarma bölge kullanmak istediğiniz hedef konumda VM'ler oluşturmak için Azure aboneliğinizin etkinleştirilmesi gerekir. Gerekli Kotayı etkinleştirmek için desteğe başvurun.

## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

>[!NOTE]
>
> Kurtarma Hizmetleri kasası, sanal makineleri çoğaltmak istediğiniz konumda oluşturmanızı öneririz. Örneğin, hedef konumunuz Orta ise ABD, kasaya oluşturma **Orta ABD**.

## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

İçinde **kurtarma Hizmetleri kasaları**, kasa adını tıklatın. Kasaya tıklayın **+ Çoğalt** düğmesi.

### <a name="step-1-configure-the-source"></a>1. Adım Kaynak yapılandırma
1. İçinde **kaynak**seçin **Azure - Önizleme**.
2. İçinde **kaynak konumu**, çalışmakta her yere Vm'lerinizi Azure bölgesinde bir kaynak seçin.
3. Vm'leriniz dağıtım modelini seçin: **Resource Manager** veya **Klasik**.
4. Seçin **kaynak kaynak grubu** Resource Manager VM'ler için veya **bulut hizmeti** Klasik VM'ler için.

    ![Kaynak yapılandırma](./media/site-recovery-azure-to-azure/source.png)

### <a name="step-2-select-virtual-machines"></a>2. Adım Sanal makineleri seçin

* Çoğaltma ve ardından istediğiniz sanal makineleri seçin **Tamam**.

    ![Sanal makineleri seçin](./media/site-recovery-azure-to-azure/vms.png)

### <a name="step-3-configure-settings"></a>3. Adım Ayarları yapılandırma

1. Varsayılan hedef ayarları geçersiz kılar ve tercih ettiğiniz ayarları belirtmek için tıklatın **Özelleştir**. Daha fazla bilgi için bkz: [hedef kaynakları özelleştirme](site-recovery-replicate-azure-to-azure.md##customize-target-resources).

    ![Ayarları yapılandırma](./media/site-recovery-azure-to-azure/settings.png)


2. Varsayılan olarak, Site Recovery uygulamayla tutarlı anlık görüntüleri 4 saatte alır ve 24 saat için kurtarma noktalarını korur bir çoğaltma ilkesi oluşturur. Farklı ayarlarla bir ilke oluşturmak için tıklatın **Özelleştir** yanına **Çoğaltma İlkesi**.

    ![İlke özelleştirme](./media/site-recovery-azure-to-azure/customize-policy.png)

3. Hedef kaynakları hazırlamaya başlamak için tıklatın **hedef kaynakları oluşturmak**. Sağlama veya bunu bir dakika sürer. Sağlama işlemi sırasında dikey pencereyi kapatmayın veya baştan başlamanız gerekiyor.

4. Seçilen VM çoğaltmasını tetiklemek için tıklatın **çoğaltmasını etkinleştir**.

5. İlerleme durumunu izleyebilirsiniz **korumayı etkinleştir** iş **ayarları** > **işleri** > **Site Recovery işleri**.

6. İçinde **ayarları** > **çoğaltılan öğeler**, VM'ler ve ilk çoğaltma ilerleme durumunu görüntüleyebilirsiniz. VM ayarlarına detaya gitmek için tıklayın.

## <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma

Her şeyi ayarladıktan sonra her şeyin beklendiği gibi çalıştığından emin olmak için yük devretme testi çalıştırın:

1. İçinde tek bir makineye vermesine **ayarları** > **çoğaltılan öğeler**, VM tıklatın **+ yük devretme testi** simgesi.

2. Kurtarma planında yük devretme için **ayarları** > **kurtarma planları**, plana sağ **yük devretme testi**. Kurtarma planı oluşturmak için [aşağıdaki yönergeleri uygulayın](site-recovery-create-recovery-plans.md). 

3. İçinde **yük devretme testi**, hedef Azure sanal ağı seçin yük devretme gerçekleştikten sonra hangi Azure VM'ler bağlı.

4. Yük devretmeyi başlatmak için tıklatın **Tamam**. İlerleme durumunu izlemek için VM özelliklerini açmak için tıklatın. Veya tıklatabilirsiniz **yük devretme testi** kasa adını işinde > **ayarları** > **işleri** > **Site Recovery işleri**.

5. Yük devretme işlemi tamamlandıktan sonra Azure portalında Azure makine çoğaltma görünür > **sanal makineleri**. VM, uygun bir ağa bağlı olduğu uygun boyutta olduğundan ve çalıştığından emin olun.

6. Yük devretme testi sırasında oluşturulan sanal makineleri silmek için tıklatın **temizleme yük devretme testi** çoğaltılmış öğesi ya da kurtarma planı. İçinde **notları**, kaydetme ve yük devretme testiyle ilişkili gözlemlerinizi kaydetmek. 

[Daha fazla bilgi edinin](site-recovery-test-failover-to-azure.md) yük devretme sınaması işlemlerini hakkında.


## <a name="next-steps"></a>Sonraki adımlar

Sonra dağıtımı test etme:

- Farklı yük devretme türleri ve onları çalıştırma hakkında [daha fazla bilgi edinin](site-recovery-failover.md).
- Daha fazla bilgi edinmek [kurtarma planlarına kullanarak](site-recovery-create-recovery-plans.md) RTO azaltmak için.
