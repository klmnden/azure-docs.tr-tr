---
title: Azure'dan VMware başarısız nasıl | Microsoft Docs
description: Azure için sanal makinelerin yük devretme sonrasında geri şirket içi sanal makineleri getirmek için yeniden çalışma işlemi başlatabilirsiniz. Yeniden çalışmak adımlar öğrenin.
services: site-recovery
documentationcenter: ''
author: nsoneji
manager: gauravd
editor: ''
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 02/22/2018
ms.author: nisoneji
ms.openlocfilehash: 50dccf6196b7affd3d21087f851b929d0e850f6d
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2018
ms.locfileid: "29768476"
---
# <a name="fail-back-from-azure-to-an-on-premises-site"></a>Azure'dan şirket içi siteye başarısız

Bu makalede, şirket içi VMware ortamı geri sanal makinelere gelen Azure sanal makineleri kapatmaya açıklar. Başarısız için bu makaledeki yönergeleri geriye, VMware sanal makineleri veya Windows/Linux site bunlar şirket içi devredilir sonra fiziksel sunucuları Azure'a kullanarak izleyin [Site Recovery yük](site-recovery-failover.md) Öğreticisi.

## <a name="prerequisites"></a>Önkoşullar
- Hakkında ayrıntılar okuduğunuzu olun [geri dönme farklı türde](concepts-types-of-failback.md) ve karşılık gelen uyarılar.

> [!WARNING]
> Ya da sonra geri dönme olamaz [tamamlandı geçiş](site-recovery-migrate-to-azure.md#what-do-we-mean-by-migration), bir sanal makine başka bir kaynak grubuna taşınmış veya Azure sanal makine silindi. Sanal makinenin korumasını devre dışı bırakırsanız, yeniden çalışma yapamazsınız.

> [!WARNING]
> Bir Windows Server 2008 R2 SP1 fiziksel sunucuda korumalı ve Azure için yük devredildi geri başarısız olamaz.

> [!NOTE]
> VMware sanal makineleri üzerinde başarısız olursa yeniden çalışma için bir Hyper-v konağı yapılamıyor.


- Devam etmeden önce böylece sanal makineler çoğaltılmış bir durumda ve bir yük devretme bir şirket içi siteye yeniden başlatabilir yeniden koruma adımları tamamlayın. Daha fazla bilgi için bkz: [Azure'dan şirket içi yeniden korumak nasıl](site-recovery-how-to-reprotect.md).

- Bir yeniden çalışma yapmadan önce vCenter bağlı durumda olduğundan emin olun. Aksi takdirde, diskleri kesme ve sanal makineye ekleme başarısız olur.

- Azure'a yük devretme sırasında şirket içi site erişilebilir olmayabilir ve yapılandırma sunucusu ya da bu nedenle olabilir kullanılamıyor veya kapatın. Yeniden koruma ve yeniden çalışma sırasında şirket içi yapılandırma sunucusu çalışır ve bağlı OK durumda olması gerekir. 

- Yeniden çalışma sırasında sanal makine yapılandırma sunucusu veritabanında mevcut olmalıdır veya geri dönme başarılı olmaz. Bu nedenle, sunucunuzun düzenli olarak zamanlanmış yedeklemeler ele emin olun. Bir olağanüstü durum varsa, sunucunun özgün IP adresiyle çalışmak yeniden çalışma için geri yüklemeniz gerekir.

- Ana hedef sunucusu, yeniden koruma/yeniden çalışma tetiklemeden önce tüm anlık görüntüleri olmamalıdır.

## <a name="overview-of-failback"></a>Yeniden çalışma genel bakış
Azure üzerinde başarısız oldu sonra aşağıdaki adımları yürüterek, şirket içi sitenize başarısız olabilir:

1. [Koruyun](site-recovery-how-to-reprotect.md) Azure sanal makinelerde VMware sanal makineleri şirket içi sitenizdeki çoğaltmak başlatmaları böylece. Bu işlemin bir parçası olarak, ayrıca gerekir:
    1. Bir şirket içi ana hedef ayarlama: Windows ana hedef Windows sanal makineler için ve [Linux ana hedefinin](site-recovery-how-to-install-linux-master-target.md) Linux sanal makineleri için.
    2. Ayarlanmış bir [işlem sunucusu](site-recovery-vmware-setup-azure-ps-resource-manager.md).
    3. Başlatma [koruyun](site-recovery-how-to-reprotect.md). Bu, şirket içi sanal makineyi kapatın ve şirket içi disklerle Azure sanal makinenin verilerini eşitleyin.

1. Azure'da sanal makinelerinizi şirket içi sitenize çoğaltma sonra bir hata üzerinden Azure'dan şirket içi siteye başlatır.

1. Verilerinizi geri başarısız oldu sonra böylece Azure'a çoğaltma başlatma, şirket içi sanal makineleri yeniden, koruyun.

Hızlı bir genel bakış için bir şirket içi sitede yeniden çalıştırmak konusunda aşağıdaki videoyu izleyin.
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video5-Failback-from-Azure-to-On-premises/player]


## <a name="steps-to-fail-back"></a>Yeniden çalışmak için adımları

> [!IMPORTANT]
> Yeniden başlatmadan önce sanal makine yükü tamamladığınızdan emin olun. Sanal makinelerin korumalı durumda olması ve durumlarını olmalıdır **Tamam**. Sanal makineleri yeniden korumak için okuma [koruyun nasıl](site-recovery-how-to-reprotect.md).

1. Çoğaltılan öğe sayfasında sanal makineyi seçin ve seçmek için sağ **planlanmamış yük devretme**.
2. İçinde **onaylayın yük devretme**, yük devretme yönünden (Azure) doğrulayın ve ardından Kurtarma noktası (son veya tutarlı en son uygulama) seçin, yük devretme için kullanmak istediğiniz. Uygulama tutarlı noktası en son noktası zamanında ve bazı veri kaybına neden olur.
3. Yük devretme sırasında azure'da sanal makineler Site Recovery kapatır. Bu geri dönme beklendiği gibi tamamlandı denetledikten sonra Azure sanal makinelerde kapatılmışsa olduğunu kontrol edebilirsiniz.
4. **Commit** başarısız sanal makineyi Azure.Right-tıklayın korumalı öğe kaldırın ve ardından gerekli **yürütme**. Bir işi başarısız azure'da sanal makineler üzerinde kaldırır.


## <a name="to-what-recovery-point-can-i-fail-back-the-virtual-machines"></a>Hangi kurtarma noktasına ı geri sanal makineleri başarısız olabilir?

Yeniden çalışma sırasında sanal makine/kurtarma planı yeniden çalışmak için iki seçeneğiniz vardır.

- Zamanında son işlenen noktası seçerseniz, tüm sanal makineler üzerinde kullanılabilir kendi son nokta zamanında devredilir. Durumunda, kurtarma planı içinde bir çoğaltma grubu, ardından her bir sanal makine çoğaltma grubunun üzerinden kendi bağımsız son noktasına zamanında başarısız olur.

    En az bir kurtarma noktası olana kadar bir sanal makine yeniden çalışamazsınız. Tüm sanal makinelerin en az bir kurtarma noktası elde edene kadar bir kurtarma planı geri kapatamazsınız.

> [!NOTE]
> En son kurtarma noktası bir kilitlenme tutarlı kurtarma noktasıdır.

- Uygulama tutarlı bir kurtarma noktası seçerseniz, tek bir sanal makine yeniden çalışma en son kullanılabilir uygulamaları tutarlı kurtarma noktasına kurtarır. Bir çoğaltma grubu olan bir kurtarma planı söz konusu olduğunda, her çoğaltma grubu için kendi ortak kullanılabilir kurtarma noktası kurtarır.
Uygulamaları tutarlı kurtarma noktalarına arkasında zamanında olabilir ve veri kaybı olabilir.

## <a name="what-happens-to-vmware-tools-post-failback"></a>VMware araçları post geri dönmesi ne olur?

Yük devretme sırasında VMware araçları Azure Site Recovery Windows sanal makine durumunda, devre dışı bırakır. Windows sanal makine yeniden çalışma sırasındaki VMware araçları yeniden etkinleştiriliyor. 


## <a name="reprotect-from-on-premises-to-azure"></a>Şirket içinden Azure'a koruyun
Yeniden çalışma sona erdikten sonra tamamlama başlattınız Azure kurtarılan sanal makine silinir. Şimdi, sanal makine şirket içi sitesinde olmakla birlikte, korumalı olmaz. Yeniden azure'a başlamak için aşağıdakileri yapın:

1. İçinde **kasa** > **ayarı** > **öğeleri çoğaltılan**, geri başarısız olmuş ve ardından sanal makineleri seçin  **Yeniden koruma**.
2. Geri Azure'a veri göndermek için kullanılması gereken işlem sunucusu değerini verir.
3. Tıklatın **Tamam** yeniden koruma işini başlatın.

> [!NOTE]
> Bir şirket içi sanal makine önyükleme sonra yapılandırmayı sunucuya geri (en fazla 15 dakika) kaydetmek aracı için biraz zaman alabilir. Bu süre boyunca yeniden koruma başarısız olur ve aracının yüklü olmadığını bildiren bir hata iletisi döndürür. Birkaç dakika bekleyin ve sonra yeniden koruma yeniden deneyin.

## <a name="next-steps"></a>Sonraki adımlar

Sonra yeniden koruma işi tamamlanana, geri Azure için sanal makinenin çoğaltıldığını ve yapabileceğiniz bir [yük devretme](site-recovery-failover.md) sanal makinelerinizi yeniden Azure'a taşımak için.


