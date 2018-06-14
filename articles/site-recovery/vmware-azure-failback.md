---
title: Azure Site Recovery ile VMware Azure'dan tutulamadı | Microsoft Docs
description: Azure için sanal makinelerin yük devretme sonrasında geri şirket içi sanal makineleri getirmek için yeniden çalışma başlatabilirsiniz. Yeniden çalışmak adımlar öğrenin.
services: site-recovery
author: nsoneji
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: nisoneji
ms.openlocfilehash: 8f580fa40bade2bf586dd116729881b249bbba88
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
ms.locfileid: "29944016"
---
# <a name="fail-back-from-azure-to-an-on-premises-site"></a>Azure'dan şirket içi siteye başarısız

Bu makalede, bir şirket içi VMware ortamı geri sanal makinelere gelen Azure sanal makineleri başarısız açıklar. Başarısız için bu makaledeki yönergeleri geriye, VMware sanal makineleri veya Windows/Linux site bunlar şirket içi devredilir sonra fiziksel sunucuları Azure'a kullanarak izleyin [yük devretme Azure Site kurtarma](site-recovery-failover.md) Öğreticisi.

## <a name="prerequisites"></a>Önkoşullar
- Hakkında ayrıntılar okuma sahip olduğundan emin olun [geri dönme farklı türde](concepts-types-of-failback.md) ve karşılık gelen uyarılar.

> [!WARNING]
> Ya da geri aldıktan sonra kapatamazsınız [tamamlandı geçiş](migrate-overview.md#what-do-we-mean-by-migration), bir sanal makine başka bir kaynak grubuna taşınmış veya Azure sanal makine silindi. Sanal makinenin korumasını devre dışı bırakırsanız, geri kapatamazsınız.

> [!WARNING]
> Bir Windows Server 2008 R2 SP1 fiziksel sunucuda korumalı ve Azure için yük devredildi geri başarısız olamaz.

> [!NOTE]
> VMware sanal makineleri üzerinde başarısız olduysa, bir Hyper-V konağına kapatamazsınız.


- Devam etmeden önce böylece sanal makineler çoğaltılmış bir durumda ve bir yük devretme bir şirket içi siteye yeniden başlatmak için yeniden koruma adımlarını tamamlayın. Daha fazla bilgi için bkz: [Azure'dan şirket içi yeniden korumak nasıl](vmware-azure-reprotect.md).

- Bir yeniden çalışma yapmadan önce vCenter bağlı durumda olduğundan emin olun. Aksi takdirde, diskleri kesme ve sanal makineye ekleme başarısız olur.

- Azure'a yük devretme sırasında şirket içi site erişilebilir olmayabilir ve yapılandırma sunucusu kullanılamıyor veya kapatılmış olabilir kapalı. Yeniden koruma ve yeniden çalışma sırasında şirket içi yapılandırma sunucusu çalışır ve bağlı OK durumda olması gerekir. 

- Yeniden çalışma sırasında sanal makine yapılandırma sunucusu veritabanında mevcut olmalıdır veya geri dönme başarılı olmaz. Sunucunuzun düzenli olarak zamanlanmış yedeklemeler ele emin olun. Bir olağanüstü durum oluşursa, sunucunun çalışması yeniden çalışma için özgün IP adresiyle geri yüklemeniz gerekir.

- Ana hedef sunucusu, yeniden koruma/yeniden çalışma tetiklemeden önce tüm anlık görüntüleri olmamalıdır.

## <a name="overview-of-failback"></a>Yeniden çalışma genel bakış
Azure üzerinde başarısız oldu sonra aşağıdaki adımları yürüterek, şirket içi sitenize başarısız olabilir:

1. [Koruyun](vmware-azure-reprotect.md) Azure sanal makinelerde VMware sanal makineleri şirket içi sitenizdeki çoğaltmak başlatmaları böylece. Bu işlemin bir parçası olarak, ayrıca gerekir:

    * Bir şirket içi ana hedefi olarak ayarlayın. Windows sanal makineler için bir Windows ana hedef kullanmanız ve [Linux ana hedefinin](vmware-azure-install-linux-master-target.md) Linux sanal makineleri için.
    * Ayarlanmış bir [işlem sunucusu](vmware-azure-set-up-process-server-azure.md).
    * Başlat [koruyun](vmware-azure-reprotect.md) şirket içi sanal makineyi kapatın ve şirket içi disklerle Azure sanal makinenin verilerini eşitlemek için.

2. Azure'da sanal makinelerinizi şirket içi sitenize çoğaltıldıktan sonra bir yük devretme şirket içi siteye Azure'dan başlatın.

3. Verilerinizi geri başarısız olduktan sonra böylece Azure'a çoğaltma başlatma, şirket içi sanal makineleri yeniden koruyun.

Hızlı bir genel bakış için bir şirket içi sitede yeniden çalıştırmak konusunda aşağıdaki videoyu izleyin:
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video5-Failback-from-Azure-to-On-premises/player]


## <a name="steps-to-fail-back"></a>Yeniden çalışmak için adımları

> [!IMPORTANT]
> Yeniden çalışma başlamadan önce sanal makine yükü tamamlandı emin olun. Sanal makinelerin korumalı durumda olması ve durumlarını olmalıdır **Tamam**. Sanal makineleri yeniden korumak için okuma [koruyun nasıl](vmware-azure-reprotect.md).

1. Çoğaltılan öğe sayfasında, sanal makineyi seçin. Seçmek için sağ **planlanmamış yük devretme**.
2. İçinde **onaylayın yük devretme**, yük devretme yönünden (Azure) doğrulayın. Kurtarma noktası (son veya tutarlı en son uygulama) seçin, yük devretme için kullanmak istediğiniz. Uygulama tutarlı noktası en son noktası zamanında ve bazı veri kaybına neden olur.
3. Yük devretme sırasında azure'da sanal makineler Site Recovery kapatır. Beklenen şekilde tamamlandı, yeniden çalışma denetledikten sonra Azure sanal makinelerde kapatıldığını kontrol edebilirsiniz.
4. **Commit** başarısız üzerinden sanal makine Azure'dan kaldırmak için gereklidir. Korunan öğeye sağ tıklayın ve ardından **yürütme**. Bir iş yükü sanal makineleri Azure'da kaldırır.


## <a name="to-what-recovery-point-can-i-fail-back-the-virtual-machines"></a>Hangi kurtarma noktasına ı geri sanal makineleri başarısız olabilir?

Yeniden çalışma sırasında sanal makine/kurtarma planı yeniden çalışmak için iki seçeneğiniz vardır.

- İşlenen son noktası zamanında seçerseniz, tüm sanal makinelerin kullanılabilir kendi son nokta zamanında yük devri. Kurtarma planı çoğaltma grubunda ise, her bir sanal makine çoğaltma grubunun üzerinden kendi bağımsız son noktasına zamanında başarısız olur.

  En az bir kurtarma noktası olana kadar bir sanal makine yeniden çalışamazsınız. Tüm sanal makinelerin en az bir kurtarma noktası elde edene kadar bir kurtarma planı geri kapatamazsınız.

  > [!NOTE]
  > En son kurtarma noktası bir kilitlenme tutarlı kurtarma noktasıdır.

- Uygulamaları tutarlı kurtarma noktası seçerseniz, tek bir sanal makine yeniden çalışma en son kullanılabilir uygulamaları tutarlı kurtarma noktasına kurtarır. Bir çoğaltma grubu olan bir kurtarma planı söz konusu olduğunda, her çoğaltma grubu için kendi ortak kullanılabilir kurtarma noktası kurtarır.
Uygulamaları tutarlı kurtarma noktalarına arkasında zamanında olabilir ve veri kaybı olabilir.

## <a name="what-happens-to-vmware-tools-post-failback"></a>VMware araçları post geri dönmesi ne olur?

Yük devretme sırasında VMware araçları Site Recovery Windows sanal makine söz konusu olduğunda, devre dışı bırakır. Windows sanal makine yeniden çalışma sırasında VMware araçları yeniden iler hale getirilir. 


## <a name="reprotect-from-on-premises-to-azure"></a>Şirket içinden Azure'a koruyun
Yeniden çalışma sona erdikten sonra yürütme başladıktan, Azure kurtarılan sanal makine silinir. Şimdi, sanal makine şirket içi sitesinde olmakla birlikte, korumalı olmaz. Yeniden azure'a başlamak için aşağıdakileri yapın:

1. Seçin **kasa** > **ayarı** > **öğeleri çoğaltılan**geri başarısız sanal makineleri seçin ve ardından  **Yeniden koruma**.
2. Geri Azure'a veri göndermek için kullanılması gereken işlem sunucusu değerini girin.
3. Seçin **Tamam** yeniden koruma işini başlatın.

> [!NOTE]
> Bir şirket içi sanal makine önyükleme sonra yapılandırmayı sunucuya geri (en fazla 15 dakika) kaydetmek aracı için biraz zaman alabilir. Bu süre boyunca yeniden koruma başarısız olur ve aracısı yüklü değil bildiren bir hata iletisi döndürür. Birkaç dakika bekleyin ve sonra yeniden koruma yeniden deneyin.

## <a name="next-steps"></a>Sonraki adımlar

Yeniden koruma işi tamamlandıktan sonra sanal makineyi çoğaltır geri Azure ve yapabilirsiniz bir [yük devretme](site-recovery-failover.md) sanal makinelerinizi yeniden Azure'a taşımak için.


