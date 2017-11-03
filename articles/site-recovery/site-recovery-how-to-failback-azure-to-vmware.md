---
title: "Azure'dan VMware başarısız nasıl | Microsoft Docs"
description: "Azure için sanal makinelerin yük devretme sonrasında geri şirket içi sanal makineleri getirmek için yeniden çalışma işlemi başlatabilirsiniz. Yeniden çalışmak adımlar öğrenin."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/05/2017
ms.author: ruturajd
ms.openlocfilehash: 1ca34b262a51b694cb9541750588bbea139eeae1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="fail-back-from-azure-to-an-on-premises-site"></a>Azure'dan şirket içi siteye başarısız

Bu makalede, şirket içi siteye geri sanal makinelere gelen Azure sanal makineleri kapatmaya açıklar. Başarısız için bu makaledeki yönergeleri geriye, VMware sanal makineleri veya Windows/Linux site bunlar şirket içi devredilir sonra fiziksel sunucuları Azure'a kullanarak izleyin [çoğaltmak VMware sanal makineleri ve fiziksel sunucular Azure Site Recovery ile Azure](site-recovery-vmware-to-azure-classic.md) Öğreticisi.

> [!WARNING]
> Ya da sonra geri dönme olamaz [tamamlandı geçiş](site-recovery-migrate-to-azure.md#what-do-we-mean-by-migration), bir sanal makine başka bir kaynak grubuna taşınmış veya Azure sanal makine silindi. Sanal makinenin korumasını devre dışı bırakırsanız, yeniden çalışma yapamazsınız.

> [!NOTE]
> VMware sanal makineleri üzerinde başarısız olursa yeniden çalışma için bir Hyper-v konağı yapılamıyor.

## <a name="overview-of-failback"></a>Yeniden çalışma genel bakış
Yeniden çalışma şeklini aşağıda verilmiştir. Azure üzerinde başarısız oldu sonra şirket içi sitenize birkaç aşamada başarısız:

1. [Koruyun](site-recovery-how-to-reprotect.md) Azure sanal makinelerde VMware sanal makineleri şirket içi sitenizdeki çoğaltmak başlatmaları böylece. Bu işlemin bir parçası olarak, ayrıca gerekir:
    1. Bir şirket içi ana hedef ayarlama: Windows ana hedef Windows sanal makineler için ve [Linux ana hedefinin](site-recovery-how-to-install-linux-master-target.md) Linux sanal makineleri için.
    2. Ayarlanmış bir [işlem sunucusu](site-recovery-vmware-setup-azure-ps-resource-manager.md).
    3. Başlatma [koruyun](site-recovery-how-to-reprotect.md). Bu, şirket içi sanal makineyi kapatın ve şirket içi disklerle Azure sanal makinenin verilerini eşitleyin.
5. Azure'da sanal makinelerinizi şirket içi sitenize çoğaltma sonra bir hata üzerinden Azure'dan şirket içi siteye başlatır.

Verilerinizi geri başarısız oldu sonra böylece azure'a başlatmaları geri için başarısız şirket içi sanal makineleri koruyun.

Hızlı bir genel bakış için Azure'dan şirket içi siteye yük devri konusunda aşağıdaki videoyu izleyin.
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video5-Failback-from-Azure-to-On-premises/player]

### <a name="fail-back-to-the-original-or-alternate-location"></a>Özgün veya alternatif konuma geri alabilirsiniz

Bir VMware sanal makinesi üzerinde başarısız olursa hala devam ediyorsa geri aynı kaynak şirket içi sanal makine için başarısız olabilir. Bu senaryoda, yalnızca değişiklikleri geri çoğaltılır. Bu senaryo, özgün konuma kurtarma bilinir. Şirket içi sanal makine mevcut değilse alternatif bir konum kurtarma senaryodur.

> [!NOTE]
> Özgün vCenter ve yapılandırma sunucusuna yalnızca geri dönme kullanabilirsiniz. Yeni yapılandırma sunucusu ve kullanmaya geri dönme dağıtamazsınız. Ayrıca, yeni bir vCenter mevcut yapılandırma sunucusu ve geri dönme yeni vCenter ekleyemezsiniz.

#### <a name="original-location-recovery"></a>Özgün konuma kurtarma

Özgün sanal makine başarısız olursa, aşağıdaki koşullar gereklidir:
* Sanal makine bir vCenter sunucusu tarafından yönetiliyorsa, ana hedefin ESX konak sanal makinenin veri deposuna erişimi olmalıdır.
* Sanal makine bir ESX konağında olmakla birlikte vCenter tarafından yönetilmiyor, sabit disk sanal makinenin ana hedefin konak erişmek için bir veri deposu olması gerekir.
* Sanal makinenize bir ESX konağında ise ve vCenter kullanmaz, koruyun önce ana hedefinin ESX konağının bulma tamamlamanız gerekir. Geri fiziksel sunucuları, çok çalıştırma işlemini gerçekleştiriyorsanız bu geçerlidir.
* Bir sanal depolama alanı ağı (vSAN) veya diskleri zaten mevcutsa ve şirket içi sanal makineye bağlı (RDM) eşleme ham aygıt temel bir disk dön başarısız olabilir.

#### <a name="alternate-location-recovery"></a>Alternatif konuma kurtarma
Senaryo, şirket içi sanal makine sanal makineyi yeniden korumayı önce mevcut değilse, alternatif bir konum kurtarma adı verilir. Yeniden koruma iş akışı, şirket içi sanal makine yeniden oluşturur. Bu ayrıca tam veri indirme neden olur.

* Farklı bir konuma başarısız olduğunda, sanal makine ana hedef sunucusu dağıtıldığı aynı ESX konak kurtarılacak. Disk oluşturmak için kullanılan veri deposu, sanal makineyi yeniden korumayı, seçilen veri deposu olacaktır.
* Yük devredebilir geriye yalnızca bir sanal makine dosya sistemi (VMFS) veya vSAN veri deposu. Bir RDM varsa, yeniden koruma ve geri dönme çalışmaz.
* Yeniden koruma değişikliklerden ve ardından bir büyük ilk veri aktarımını içerir. Sanal makine şirket içinde var olmadığından bu işlem bulunmaktadır. Tam veri geri çoğaltılması gerekir. Bu yeniden koruma ayrıca bir özgün konuma kurtarma daha uzun sürer.
* Geri RDM tabanlı disklere kapatamazsınız. Yalnızca yeni sanal makine disklerini (VMDKs) VMFS/vSAN veri deposu üzerinde oluşturulabilir.

Fiziksel makine üzerinden Azure için başarısız olduğunda başarısız geri yalnızca (P2A2V da bilinir) bir VMware sanal makinesi olarak. Bu akış altında alternatif konuma kurtarma döner.

* Bir Windows Server 2008 R2 SP1 fiziksel sunucuda korumalı ve Azure için yük devredildi geri başarısız olamaz.
* En az bir ana hedef sunucusu ve geri dönecek yapmanız gereken ESX/ESXi konaklarının Bul emin olun.

## <a name="have-you-completed-reprotection"></a>Yükü tamamladınız mı?
Devam etmeden önce böylece sanal makineler çoğaltılmış bir durumda ve bir yük devretme bir şirket içi siteye yeniden başlatabilir yeniden koruma adımları tamamlayın. Daha fazla bilgi için bkz: [Azure'dan şirket içi yeniden korumak nasıl](site-recovery-how-to-reprotect.md).

## <a name="prerequisites"></a>Ön koşullar

> [!IMPORTANT]
> Azure'a yük devretme sırasında şirket içi site erişilebilir olmayabilir ve yapılandırma sunucusu ya da bu nedenle olabilir beklemediğiniz kullanılabilir veya kapatın. Yeniden koruma ve yeniden çalışma sırasında şirket içi yapılandırma sunucusu çalışır ve bağlı OK durumda olması gerekir.

* Bir yeniden çalışma işlemi yaptığınızda, yapılandırma sunucusu şirket içinde gereklidir. Sunucu çalışır durumda olması gerekir ve sistem durumu Tamam olacak şekilde, hizmete bağlı. Yeniden çalışma sırasında sanal makine yapılandırma sunucusu veritabanında mevcut olmalıdır veya geri dönme başarılı olmaz. Bu nedenle, sunucunuzun düzenli olarak zamanlanmış yedeklemeler ele emin olun. Bir olağanüstü durum varsa, sunucu ile çalışmak yeniden çalışma için aynı IP adresi geri yüklemeniz gerekir.
* Ana hedef sunucusu, yeniden koruma/yeniden çalışma tetiklemeden önce tüm anlık görüntüleri olmamalıdır.

## <a name="steps-to-fail-back"></a>Yeniden çalışmak için adımları

> [!IMPORTANT]
> Yeniden başlatmadan önce sanal makine yükü tamamladığınızdan emin olun. Sanal makinelerin korumalı durumda olması ve durumlarını olmalıdır **Tamam**. Sanal makineleri yeniden korumak için okuma [koruyun nasıl](site-recovery-how-to-reprotect.md).

1. Çoğaltılan öğe sayfasında sanal makineyi seçin ve seçmek için sağ **planlanmamış yük devretme**.
2. İçinde **onaylayın yük devretme**, yük devretme yönünden (Azure) doğrulayın ve ardından Kurtarma noktası (son veya tutarlı en son uygulama) seçin, yük devretme için kullanmak istediğiniz. Uygulama tutarlı noktası en son noktası zamanında ve bazı veri kaybına neden olur.
3. Yük devretme sırasında azure'da sanal makineler Site Recovery kapatır. Bu geri dönme beklendiği gibi tamamlandı denetledikten sonra Azure sanal makinelerde kapatılmışsa olduğunu kontrol edebilirsiniz.

### <a name="to-what-recovery-point-can-i-fail-back-the-virtual-machines"></a>Hangi kurtarma noktasına ı geri sanal makineleri başarısız olabilir?

Yeniden çalışma sırasında sanal makine/kurtarma planı yeniden çalışmak için iki seçeneğiniz vardır.

Zamanında son işlenen noktası seçerseniz, tüm sanal makineler üzerinde kullanılabilir kendi son nokta zamanında devredilir. Durumunda kurtarma planı içinde bir çoğaltma grubu çoğaltma grubunun her bir sanal makine üzerinde kendi bağımsız son noktasına zamanında başarısız olur.

En az bir kurtarma noktası olana kadar bir sanal makine yeniden çalışamazsınız. Tüm sanal makinelerin en az bir kurtarma noktası elde edene kadar bir kurtarma planı geri kapatamazsınız.

> [!NOTE]
> En son kurtarma noktası bir kilitlenme tutarlı kurtarma noktasıdır.

Uygulama tutarlı bir kurtarma noktası seçerseniz, tek bir sanal makine yeniden çalışma en son kullanılabilir uygulamaları tutarlı kurtarma noktasına kurtarır. Bir çoğaltma grubu olan bir kurtarma planı söz konusu olduğunda, her çoğaltma grubu için kendi ortak kullanılabilir kurtarma noktası kurtarır.
Uygulamaları tutarlı kurtarma noktaları arkasında zamanında olabilir ve veri kaybı olabilir unutmayın.

### <a name="what-happens-to-vmware-tools-post-failback"></a>VMware araçları post geri dönmesi ne olur?

Azure'a yük devretme sırasında VMware araçları Azure sanal makine üzerinde çalışan olamaz. Yük devretme sırasında VMware araçları ASR Windows sanal makine durumunda, devre dışı bırakır. Linux sanal makine durumunda ASR yük devretme sırasında VMware araçları kaldırır.

Windows sanal makine yeniden çalışma sırasındaki VMware araçları yeniden çalışma sırasında yeniden büyük/küçük harf etkinleştiriliyor. Benzer şekilde, bir linux sanal makine için VMware araçları makinede yeniden çalışma sırasında yeniden yüklenir.

## <a name="next-steps"></a>Sonraki adımlar

Yeniden çalışma sona erdikten sonra kurtarılan sanal makinelerle silinir sağlamak için sanal makineyi yürütmek gerekir.

### <a name="commit"></a>İşleme
Yürütme başarısız sanal makine Azure'dan kaldırmak için gereklidir.
Korunan öğeye sağ tıklayın ve ardından **yürütme**. Bir işi başarısız azure'da sanal makineler üzerinde kaldırır.

### <a name="reprotect-from-on-premises-to-azure"></a>Şirket içinden Azure'a koruyun

Yürütme sona erdikten sonra sanal makineniz şirket içi sitesinde olduğu, ancak korunan olmaz. Yeniden azure'a başlamak için aşağıdakileri yapın:

1. İçinde **kasa** > **ayarı** > **öğeleri çoğaltılan**, geri başarısız olmuş ve ardından sanal makineleri seçin  **Yeniden koruma**.
2. Geri Azure'a veri göndermek için kullanılması gereken işlem sunucusu değerini verir.
3. Tıklatın **Tamam** yeniden koruma işini başlatın.

> [!NOTE]
> Bir şirket içi sanal makine önyükleme sonra yapılandırmayı sunucuya geri (en fazla 15 dakika) kaydetmek aracı için biraz zaman alabilir. Bu süre boyunca yeniden koruma başarısız olur ve aracının yüklü olmadığını bildiren bir hata iletisi döndürür. Birkaç dakika bekleyin ve sonra yeniden koruma yeniden deneyin.

Yeniden koruma işi tamamlandıktan sonra geri Azure için sanal makinenin çoğaltıldığını ve bir yük devretme yapabilirsiniz.

## <a name="common-issues"></a>Genel sorunlar
Bir yeniden çalışma yapmadan önce vCenter bağlı durumda olduğundan emin olun. Aksi takdirde, diskleri kesme ve sanal makineye ekleme başarısız olur.
