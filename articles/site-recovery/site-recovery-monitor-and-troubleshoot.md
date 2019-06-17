---
title: Azure Site RECOVERY'yi izleme | Microsoft Docs
description: İzleme ve Azure Site Recovery çoğaltma sorunlarını ve işlemleri portalı kullanarak sorun giderme
author: raynew
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 03/18/2019
ms.author: raynew
ms.openlocfilehash: 5a659da4bcc86544c31d7a789779253a0f571f34
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66497557"
---
# <a name="monitor-site-recovery"></a>Site kurtarma İzleyicisi

Bu makalede, Azure Site Recovery'nin yerleşik İzleme özelliklerini izleme ve sorun giderme için kullanmayı öğrenin. 

## <a name="use-the-dashboard"></a>Panoyu kullanma

1. Kasaya tıklayın **genel bakış** Site Recovery panosunu açın. Panolar sayfaları için hem Site Recovery ve Backup vardır ve bunlar arasında geçiş yapabilirsiniz.

    ![Site Recovery Panosu](./media/site-recovery-monitor-and-troubleshoot/dashboard.png)

2.  Pano, tek bir konumda kasa için tüm izleme bilgilerini birleştirir. Panodan farklı alanlara detaya gidebilirsiniz. 

    ![Site Recovery Panosu](./media/site-recovery-monitor-and-troubleshoot/site-recovery-overview-page.png).

3. Üzerinde **çoğaltılan öğeler**, tıklayın **görünümü tüm** tüm sunucuları kasaya görmek için.
4. Her bölümde durumu ayrıntıları tıklayarak detayına gidin. İçinde **altyapı görünümü**, izleme bilgilerini çoğaltılan makineler türüne göre sıralayabilirsiniz.

## <a name="monitor-replicated-items"></a>Çoğaltılan öğeler izleyin

Çoğaltılan öğeler bölüm çoğaltma kasaya etkin olan tüm makineler durumunu gösterir.

**State** | **Ayrıntılar**
--- | ---
Sorunsuz | Çoğaltma normal şekilde devam eder. Herhangi bir hata veya uyarı belirtileri algılanır.
Uyarı | Çoğaltma etkileyebilecek bir veya daha fazla uyarı belirtileri algılanır.
Kritik | Bir veya daha fazla kritik çoğaltma hatası belirtileri tespit ettik.<br/><br/> Bu hata Belirtiler genellikle göstergeleri olduğundan, çoğaltma takılı kalıyor veya veri değişim oranı gibi hızlı ilerleyen değil.
Geçerli değil | Şu anda çoğaltma olması beklenen olmayan sunucuları. Bu yük devreden makineler içerebilir.

## <a name="monitor-test-failovers"></a>Yük devretme testi İzleyicisi

Kasada makineler için test yük devretme durumunu görüntüleyebilirsiniz.

- Her altı aylık en az bir kez çoğaltılan makinelerde yük devretme testi çalıştırmanızı öneririz. Bu, yük devretme, üretim ortamınızın kesintiye uğratmadan beklendiği gibi çalıştığını denetlemek için bir yoludur. 
- Yalnızca Yük devretme ve yük devretme sonrası Temizleme işlemini tamamladıktan sonra başarıyla yük devretme testi başarılı olarak kabul edilir.

**State** | **Ayrıntılar**
--- | ---
Sınama önerilir | Koruma etkinleştirildikten sonra Yük devretme testi sahip olmayan makineler.
Başarıyla gerçekleştirildi | Makine ile ya da daha başarılı test yük devretmeleri.
Geçerli değil | Yük devretme testi için şu anda uygun olmayan makineler. Örneğin, yük devretmesi makineler var. ilk çoğaltma ve test yük devretme/yük devretme sürüyor

## <a name="monitor-configuration-issues"></a>İzleyici yapılandırma sorunları

**Yapılandırma sorunlarını** bölümü başarıyla yük devretme yeteneğinizi etkileyebilir sorunların bir listesini gösterir.

- Yapılandırma sorunları (dışında yazılım güncelleştirme kullanılabilirlik), varsayılan olarak her 12 saatte bir çalışan düzenli Doğrulayıcı işlemi tarafından algılanır. Hemen yanında Yenile simgesine tıklayarak çalıştırmak için Doğrulayıcı işlemi zorlayabilirsiniz **yapılandırma sorunlarını** bölüm başlığı.
- Daha fazla bilgi almak için bağlantıları tıklatın. Belirli makinelere etkileyen sorunlar için tıklatın **dikkat etmeniz gereken** içinde **hedef yapılandırmaları** sütun. Ayrıntılar için düzeltme önerileri içerir.

**State** | **Ayrıntılar**
--- | ---
Eksik yapılandırmaları | Gerekli bir ayar eksik bir kurtarma ağı veya bir kaynak grubu gibi.
Eksik kaynakları | Belirtilen kaynak bulunamıyor veya bu abonelikte mevcut değil. Örneğin, kaynak geçişi veya silinmiş. İzlenen kaynakları dahil hedef kaynak grubu, hedef VNet/alt ağ, günlük/hedef depolama hesabı, hedef kullanılabilirlik kümesi, hedef IP adresi.
Abonelik kotası |  Kullanılabilir abonelik kaynak kotası Bakiye tüm kasadaki makineler yük devretme için gerekli dengeyi karşılaştırılır.<br/><br/> Yetersiz kota Bakiye yeterli kaynak yoksa bildirilir.<br/><br/> Kotalar VM çekirdek sayısı için VM ailesi çekirdek sayısı, ağ arabirim kartı (NIC) sayısı izlemekte olduğunuz.
Yazılım güncelleştirmeleri | Yeni yazılım güncelleştirmeleri ve süresi dolan yazılım sürümleri hakkında bilgi kullanılabilirliği.


## <a name="monitoring-errors"></a>Hataları izleme 
**Hata özeti** bölüm çoğaltma sunucuları kasaya ve etkilenen makine sayısı etkileyebilecek etkin hata Belirtiler gösterir.

- Bölümün başında, şirket içi altyapı bileşenlerini etkileyen hatalar gösterilir. Örneğin, olmayan-giriş şirket içi yapılandırma sunucusu, VMM sunucusunun veya Hyper-V konağında çalışan Azure Site Recovery Sağlayıcısı'ndan bir sinyal.
- Sonra çoğaltma hatası belirtileri etkileyen çoğaltılmış sunucuları gösterilir.
- Tablo girişleri hata önem derecesi azalan olarak ve ardından etkilenen makine sayısı azalan göre sıralanır.
- Etkilenen sunucusu sayısı, tek bir arka plandaki sorunu birden çok makineyi etkileyen olup olmadığını anlamak için kullanışlı bir yoldur. Örneğin, bir sorun büyük olasılıkla tüm makineleri Azure'a çoğaltma etkileyebilir. 
- Tek bir sunucu üzerinde birden çok çoğaltma hataları oluşabilir. Bu durumda, bu sunucu, etkilenen sunucuları listesinde her hata belirti sayar. Sorun çözüldükten sonra çoğaltma parametrelerini iyileştirmek ve hata makineden kaldırılır.

## <a name="monitor-the-infrastructure"></a>Altyapı izleyin.

**Altyapı görünümü** altyapı bileşenleri dahil olan çoğaltma ve sunucular ve Azure Hizmetleri arasındaki bağlantı durumunu gösterir.

- Yeşil bir çizgi bağlantı sağlıklı olduğunu gösterir.
- Kaplanmış hata simgesi kırmızı bir çizgi etkisi bağlantısının bir veya daha fazla hata belirtilerin varlığını gösterir.
-  Fare işaretçisi hata ve etkilenen varlıkların sayısına gösterilecek hata simgenin üzerine getirin. Etkilenen varlıkların filtrelenmiş bir liste simgesine tıklayın.

    ![Site Recovery altyapı görünümü (kasa)](./media/site-recovery-monitor-and-troubleshoot/site-recovery-vault-infra-view.png)

## <a name="tips-for-monitoring-the-infrastructure"></a>Altyapı izleme için ipuçları

- Şirket içi altyapı bileşenleri (yapılandırma sunucusu, işlem sunucuları, VMM sunucusu, Hyper-V konakları, VMware makinelerini) Site Recovery sağlayıcısı ve/veya aracılarının en son sürümlerine çalışır durumda olduğundan emin olun.
- Altyapı Görünümü'nde tüm özelliklerini kullanmak için çalıştırılması gerektiği [güncelleştirme paketi 22](https://support.microsoft.com/help/4072852) bu bileşenler için.
- Altyapı görünümü kullanmak için ortamınızda uygun çoğaltma senaryosu seçin. Daha fazla bilgi için görünümde detaya gidebilirsiniz. Aşağıdaki tablo, hangi senaryoları temsil gösterir.

    **Senaryo** | **State**  | **Kullanılabilir görüntülemek ister misiniz?**
    --- |--- | ---
    **Şirket içi siteler arasında çoğaltma** | Tüm durumlar | Hayır 
    **Azure bölgeleri arasında Azure VM çoğaltma**  | Çoğaltma etkin ilk çoğaltma sürüyor | Evet
    **Azure bölgeleri arasında Azure VM çoğaltma** | Yük devretme / yeniden çalışma | Hayır   
    **Azure'a VMware çoğaltması için** | Çoğaltma etkin ilk çoğaltma sürüyor | Evet     
    **Azure'a VMware çoğaltması için** | Yeniden üzerinden/başarısız oldu | Hayır      
    **Hyper-V'den azure'a çoğaltma** | Yeniden üzerinden/başarısız oldu | Hayır

- Kasa menüsünden, tek bir çoğaltma makinesi için altyapı görünümü görmek için tıklayın **çoğaltılan öğeler**ve bir sunucu seçin.  

### <a name="common-questions"></a>Sık sorulan sorular


**Kasa görünümünde altyapı toplam sayısından farklı sanal makinelerin sayısı neden çoğaltılan öğeler gösteriliyor?**

Kasa altyapı görünümü çoğaltma senaryoları tarafından belirlenir. Yalnızca şu anda seçili çoğaltma senaryosu makinelerinizde sayısı görünüm için dahil edilmiştir. Ayrıca, biz yalnızca Azure'a çoğaltma için yapılandırılmış makinelerin sayısı. Makine üzerinde başarısız oldu veya bir şirket içi siteye çoğaltılan makineler Görünümü'nde sayılmaz.

**Toplam sayısını Panoda çoğaltılan öğeler, farklı Essentials çekmecede gösterilen çoğaltılmış öğe sayısı neden oluyor?**

Yalnızca makineleri hangi ilk çoğaltma tamamlanana için Essentials çekmecede gösterilen sayısı eklenir. Çoğaltılan öğeler üzerinde toplam kasaya dahil olmak üzere, tüm makineler içerir hangi ilk çoğaltma için şu anda sürüyor.


## <a name="monitor-recovery-plans"></a>İzleyici kurtarma planları

İçinde **kurtarma planları bölüm** planlarının sayısını inceleyin, yeni planları oluşturun ve mevcut olanları düzenleyin.  

## <a name="monitor-jobs"></a>İşleri izleme

**İşleri** bölümü, Site Recovery işlemlerini durumunu yansıtır.

- Azure Site recovery'de işlemlerinin çoğu, oluşturulan ve işlemin ilerlemesini izlemek için kullanılan bir izleme işi ile zaman uyumsuz olarak yürütülür. 
- İş nesnesi durum ve işlemin ilerlemesini izlemek gereken tüm bilgileri var. 

İşleri gibi izleyin:

1. Panodaki > **işleri** bölümü tamamladınız, ilerlemeleri ya da giriş bekleniyor son 24 saat içinde bulunan işleri özetini görebilirsiniz. İlgili işleri hakkında daha fazla bilgi almak için herhangi bir durumu tıklayabilirsiniz.
2. Tıklayın **tümünü görüntüle** son 24 saat içindeki tüm işleri görmek için.

    > [!NOTE]
    > İş bilgileri kasa menüsünden erişebilirsiniz > **Site Recovery işleri**. 

2. İçinde **Site Recovery işleri** listesinde, işlerinin listesi görüntülenir. Üst menüden alabilirsiniz belirli işler için hata ayrıntılarına filtre belirli ölçütlere göre işler listesi ve seçili iş ayrıntılarını Excel'e dışarı aktar.
3. Tıklayarak bir işi detaya gidebilirsiniz. 

## <a name="monitor-virtual-machines"></a>Sanal makineleri izleme

Ayrıca Panoda sanal makineler sayfasındaki makineler izleyebilirsiniz. 

1. Kasaya tıklayın **çoğaltılan öğeler** çoğaltılan makinelerin bir listesini almak için.  Alternatif olarak, Pano sayfasında kapsamlı kısayolları tıklayarak korumalı öğelerin filtrelenmiş bir listesini alabilirsiniz.

    ![Site Recovery çoğaltılan öğeleri listesi görünümü](./media/site-recovery-monitor-and-troubleshoot/site-recovery-virtual-machine-list-view.png)

2. Üzerinde **çoğaltılan öğeler** sayfasında görüntüleyebilir ve filtre bilgileri. Üst Eylem menüsünde yük devretme testi çalıştırma veya belirli hataları görüntüleme dahil olmak üzere belirli bir makine için eylemler gerçekleştirebilirsiniz.
3. Tıklayın **sütunları** RPO, gösterilecek örneğin ek sütunları gösterecek şekilde yapılandırma sorunlarını ve çoğaltma hataları hedefleyin.
4. Tıklayın **filtre** çoğaltma durumu veya belirli bir çoğaltma ilkesi gibi belirli parametreleri temel alan bilgiler görüntülemek için.
5. Bir makine için yük devretme testi gibi işlemleri başlatmak için ya da onunla ilişkili özel hata ayrıntılarını görüntülemek için sağ tıklayın.
6. Daha fazla için detaylarına gitmek için bir makineye tıklayın. Ayrıntıları içerir:
   - **Çoğaltma bilgileri**: Geçerli durumu ve makinenin sistem durumu.
   - **RPO** (kurtarma noktası hedefi): Geçerli RPO için sanal makine ve RPO son Hesaplandı zaman.
   - **Kurtarma noktaları**: Makine için en son kullanılabilir kurtarma noktaları.
   - **Yük devretmeye hazırlık**: Yük devretme testi (makineler) Mobility hizmetini çalıştıran makine ve yapılandırma sorunlarını çalışan Aracısı sürümü makinenin çalıştırılıp çalıştırılmadığını gösterir.
   - **Hataları**: Şu anda makine ve olası nedenleri/Eylemler gözlemlenen çoğaltma hatası belirtileri listesi.
   - **Olayları**: Makineyi etkileyen en son olayların kronolojik bir listesi. Hata ayrıntılarını şu anda gözlemlenebilir hatası belirtileri olayları bir geçmiş kaydını makine etkilemiş sorunlarını üzerindeyken gösterir.
   - **Altyapı görünümü**: Makineleri Azure'a çoğaltırken senaryosu için altyapı durumunu gösterir.

     ![Site Recovery çoğaltılan öğe ayrıntıları/genel bakış](./media/site-recovery-monitor-and-troubleshoot/site-recovery-virtual-machine-details.png)


### <a name="common-questions"></a>Sık sorulan sorular

**RPO en son kullanılabilir kurtarma noktasından farklı mı?**


- Site Recovery, makineleri Azure'a çoğaltmak için çok adımlı zaman uyumsuz bir işlem kullanır.
- Çoğaltma sondan adımda birlikte meta veri, makine üzerinde son değişiklikleri bir günlük/önbellek depolama hesabına kopyalanır.
- Kurtarılabilir bir noktasını tanımlamak üzere etiket yanı sıra bu değişiklikler, hedef bölgede depolama hesabına yazılır.
-  Site Recovery şimdi sanal makine kurtarılabilir bir noktası oluşturabilirsiniz.
- Bu noktada, şimdiye kadarki depolama hesabına yüklediniz değişikliklerin RPO karşılanıp karşılanmadığını. Diğer bir deyişle, makinenin RPO bu noktada eşittir geçen süreyi gelen kurtarılabilir noktasına karşılık gelen zaman damgası.
- Artık, Site Recovery karşıya yüklenen veriler depolama hesabından seçer ve bu makine için oluşturulan çoğaltma diskleri uygular.
- Site Recovery kurtarma noktası oluşturur ve bu noktaya yük devretme sırasında kurtarma için kullanılabilir hale getirir. Bu nedenle en son kullanılabilir kurtarma noktası, zaten işlendikten ve çoğaltma diskleri için uygulanan en son kurtarma noktasına karşılık gelen zaman damgasını gösterir.

> [!NOTE]
> Bir hatalı sistem saatini çoğaltma kaynak makinede ya da şirket içi altyapı sunucularına hesaplanan RPO değeri eğme. RPO doğru raporlama için sistem saati tüm sunucuları ve makineler üzerinde doğru olduğundan emin olun. 

## <a name="subscribe-to-email-notifications"></a>E-posta bildirimlerine abone olma

Bu kritik olaylar için e-posta bildirimleri almak için abone olabilirsiniz:
 
- Kritik durum çoğaltılan makinelerin için.
- Şirket içi altyapı bileşenlerini ve Site Recovery hizmeti arasında bağlantı yok. Bir sinyal mekanizması kullanılarak bir kasada kayıtlı Site Recovery ile şirket içi sunucuları arasında bir bağlantı algılandı.
- Yük devretme hataları.

Aşağıdaki şekilde abone olabilirsiniz:

Kasadaki > **izleme** bölümünde **Site Recovery etkinlikleri**.
1. Tıklayın **e-posta bildirimleri**.
1. İçinde **e-posta bildirimi**bildirimlerini açmak ve kimin göndermek belirtin. Tüm abonelik yöneticileri, bildirimler ve isteğe bağlı olarak belirli bir e-posta adreslerini gönderilmesini gönderebilirsiniz.

    ![E-posta bildirimleri](./media/site-recovery-monitor-and-troubleshoot/email.png)
