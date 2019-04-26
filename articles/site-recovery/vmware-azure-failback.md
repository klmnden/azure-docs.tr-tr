---
title: Azure'dan VMware vm'lerinin olağanüstü durum kurtarma sırasında Azure Site Recovery ile azure'a yeniden çalışma | Microsoft Docs
description: VMware Vm'lerini ve fiziksel sunucuları azure'a olağanüstü durum kurtarma sırasında azure'a yük devredildikten sonra şirket içi sitede yeniden çalıştırmak öğrenin.
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.date: 01/15/2019
ms.topic: conceptual
ms.author: mayg
ms.openlocfilehash: 7773a2f43eb076075be484d92fde31094a2b584b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60318130"
---
# <a name="fail-back-vmware-vms-and-physical-servers-from-azure-to-an-on-premises-site"></a>Başarısız bir şirket içi siteye VMware Vm'lerini ve fiziksel sunucuları Azure'dan geri

Bu makalede, bir şirket içi VMware ortamını geri sanal makinelere gelen Azure sanal makineler başarısız açıklar. Başarısız için bu makaledeki yönergeleri yedekleme, VMware sanal makinelerini veya Windows/Linux site bunlar üzerinden şirket içi ad'nizden devrettikten sonra fiziksel sunucuları Azure'a kullanarak izleyin [Azure Site recovery'de yük devretme](site-recovery-failover.md) öğretici.

## <a name="prerequisites"></a>Önkoşullar
- Hakkında ayrıntılar okuduğunuzu emin [geri dönme farklı türde](concepts-types-of-failback.md) ve karşılık gelen uyarılar.

> [!WARNING]
> Ya da geri aldıktan sonra çalıştıramazsınız [tamamlandı geçiş](migrate-overview.md#what-do-we-mean-by-migration), bir sanal makine başka bir kaynak grubuna taşındı veya Azure sanal makine silindi. Sanal makinenin korumasını devre dışı bırakırsanız, yeniden çalışma gerçekleştiremezsiniz.

> [!WARNING]
> Windows Server 2008 R2 SP1'i bir fiziksel sunucu olarak korumalı ve Azure'a devredilen geri devredilemez.

> [!NOTE]
> VMware sanal makinelerini vermezse, bir Hyper-V ana bilgisayara çalışma özelliğini kullanamazsınız.


- Devam etmeden önce böylece çoğaltılan durumundaki sanal makinelere ve bir yük devretme bir şirket içi siteye yeniden başlatabilirsiniz yeniden koruma adımlarını tamamlayın. Daha fazla bilgi için [Azure'dan şirket içine yeniden korumak nasıl](vmware-azure-reprotect.md).

- Bir yeniden çalışma gerçekleştirmeden önce vCenter bağlı durumda olduğundan emin olun. Aksi takdirde diskler bağlantısının kesilmesini ve sanal makineye eklemeden başarısız olur.

- Azure'a yük devretme sırasında şirket içi site erişilebilir olmayabilir ve yapılandırma sunucusu kullanılamıyor veya kapatılmış olabilir aşağı. Yeniden koruma ve yeniden çalışma sırasında şirket içi yapılandırma sunucusu çalıştığını ve bağlı bir OK durumunda olmalıdır. 

- Yeniden çalışma sırasında sanal makine yapılandırma sunucusu veritabanında mevcut olmalıdır veya yeniden çalışma başarısız olmaz. Düzenli olarak zamanlanmış yedeklemeler sunucunuzun dikkate aldığınızdan emin olun. Bir olağanüstü durum oluşursa, iş yeniden çalışma özgün IP adresiyle bir sunucuyu geri yüklemek gerekir.

- Ana hedef sunucusunda yeniden koruma/yeniden çalışma tetiklemeden önce tüm anlık görüntüleri sahip olmamalıdır.

## <a name="overview-of-failback"></a>Yeniden çalışma genel bakış
Azure'a üzerinden başarısız olduktan sonra aşağıdaki adımları çalıştırarak şirket içi sitenize geri başarısız olabilir:

1. [Yeniden koruma](vmware-azure-reprotect.md) Azure sanal makinelerinde, VMware sanal makinelerini şirket içi sitenizde çoğaltmak başlatılması. Bu işlemin bir parçası olarak da yapmanız gerekir:

    * Şirket içi ana hedef ayarlayın. Windows sanal makinelerde Windows ana hedef kullanmak ve [Linux ana hedef](vmware-azure-install-linux-master-target.md) Linux sanal makineleri için.
    * Ayarlanmış bir [işlem sunucusu](vmware-azure-set-up-process-server-azure.md).
    * Başlangıç [yeniden koruma](vmware-azure-reprotect.md) şirket içi sanal makineyi kapatın ve şirket içi diskler ile Azure sanal makinenin verilerini eşitlemek için.

2. Sanal makinelerinizi azure'da, şirket içi sitenize çoğaltıldıktan sonra bir yük devretme şirket içi siteye Azure'dan başlatın.

3. Verilerinizi geri başarısız olduktan sonra böylece bunlar Azure'a çoğaltmaya başlamak için şirket içi sanal makineleri yeniden koruyun.

Hızlı bir genel bakış için nasıl bir şirket içi sitede yeniden çalıştırmak aşağıdaki videoyu izleyin:
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video5-Failback-from-Azure-to-On-premises/player]


## <a name="steps-to-fail-back"></a>Yeniden çalışma için adımları

> [!IMPORTANT]
> Yeniden çalışma başlamadan önce sanal makineleri yeniden koruma tamamlandı emin olun. Sanal makinelerin korumalı durumda olması gerekir ve bunların sistem durumu olmalıdır **Tamam**. Sanal makineleri yeniden korumak için okuma [yeniden korumak nasıl](vmware-azure-reprotect.md).

1. Çoğaltılan öğeler sayfasında, sanal makineyi seçin. Seçmek için sağ **planlanmamış yük devretme**.
2. İçinde **yük devretmeyi Onayla**, yük devretme yönü (azure'dan) doğrulayın. Ardından (son veya en son uygulama ile tutarlı) kurtarma noktasını seçin, yük devretme için kullanmak istediğiniz. Uygulama tutarlı noktası en son noktası saattir ve bazı verilerin kaybolmasına neden olur.
3. Yük devretme sırasında azure'da sanal makineler Site Recovery kapatır. Beklendiği gibi tamamlandı, yeniden çalışma denetledikten sonra Azure sanal makineleri kapatmak kontrol edebilirsiniz.
4. **İşleme** devredilen sanal makine Azure'dan kaldırması gerekir. Korunan öğeye sağ tıklayın ve ardından **işleme**. Bir işi devredilen sanal makinelerin Azure'da kaldırır.


## <a name="to-what-recovery-point-can-i-fail-back-the-virtual-machines"></a>Hangi kurtarma noktasına miyim sanal makineleri yeniden çalıştırabilirsiniz?

Yeniden çalışma sırasında sanal makinesi/kurtarma planı yeniden çalışma için iki seçeneğiniz vardır.

- İşlenen son zaman noktasına zamanında seçerseniz, tüm sanal makineler, en son kullanılabilir noktaya zamanında yük devri. Kurtarma planı içinde bir çoğaltma grubu varsa, her bir sanal makine çoğaltma grubunun kendi bağımsız son noktasına zamanında devreder.

  En az bir kurtarma noktası olana kadar bir sanal makine yeniden çalışma özelliğini kullanamazsınız. Tüm sanal makineler en az bir kurtarma noktası kadar bir kurtarma planı yeniden çalışma özelliğini kullanamazsınız.

  > [!NOTE]
  > En son kurtarma noktası bir kilitlenme ile tutarlı kurtarma noktasıdır.

- Uygulamayla tutarlı kurtarma noktasını seçerseniz, tek sanal makine yeniden çalışma için en son kullanılabilir uygulamayla tutarlı kurtarma noktası kurtarır. Çoğaltma grubu bir kurtarma planı söz konusu olduğunda, her çoğaltma grubu, ortak bir kullanılabilir kurtarma noktası kurtarır.
Uygulamayla tutarlı kurtarma noktalarını arkasındaki olabilir ve veri kaybı olabilir.

## <a name="what-happens-to-vmware-tools-post-failback"></a>VMware araçları post azure'dan ne olacak?

Yük devretme sırasında VMware araçları Site Recovery bir Windows sanal makine söz konusu olduğunda, devre dışı bırakır. Windows sanal makinesinin yeniden çalışma sırasında VMware araçlarını yeniden iler hale getirilir. 


## <a name="reprotect-from-on-premises-to-azure"></a>Şirket içinden Azure'a yeniden koruma
Kurtarılan sanal makineleri azure'da yeniden çalışma tamamlandıktan ve siz işleme başladıktan sonra silinir. Artık, şirket içi sitede sanal makinedir, ancak korunan olmaz. Tekrar Azure'a çoğaltmak başlatmak için aşağıdakileri yapın:

1. Seçin **kasası** > **ayarı** > **çoğaltılan öğeler**geri başarısız olan sanal makineleri seçin ve ardından  **Yeniden koruma**.
2. İşlem sunucusu verileri Azure'a geri göndermek için kullanılması gereken değeri girin.
3. Seçin **Tamam** yeniden koruma işini başlatmak için.

> [!NOTE]
> Bir şirket içi sanal makine önyükleme sonra aracının yapılandırma sunucusunu yeniden (en fazla 15 dakika) kaydetmek biraz zaman alabilir. Bu süre boyunca yeniden koruma başarısız olur ve aracı yüklü olmadığını bildiren bir hata iletisi döndürür. Birkaç dakika bekleyin ve sonra yeniden koruma yeniden deneyin.

## <a name="next-steps"></a>Sonraki adımlar

Yeniden koruma işi tamamlandıktan sonra sanal makineyi çoğaltır ve Azure için geri yapabilirsiniz bir [yük devretme](site-recovery-failover.md) sanal makinelerinizi Azure'a yeniden taşıyabilirsiniz.


