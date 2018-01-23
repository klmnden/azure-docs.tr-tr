---
title: Azure Site Recovery izleyebilir ve | Microsoft Docs
description: "İzleme ve Azure Site Recovery çoğaltma sorunlarını ve Portalı'nı kullanarak işlemleri sorun giderme"
services: site-recovery
documentationcenter: 
author: bsiva
manager: abhemraj
editor: raynew
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 01/06/2017
ms.author: bsiva
ms.openlocfilehash: 6dca032c8611ac4f7e66eb6f7e22e53f49209143
ms.sourcegitcommit: 5ac112c0950d406251551d5fd66806dc22a63b01
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2018
---
# <a name="monitoring-and-troubleshooting-azure-site-recovery"></a>İzleme ve Azure Site Recovery sorun giderme

Bu makalede, Azure Site Recovery'nin yerleşik izleme özelliklerinin izleme ve sorun giderme için nasıl kullanılacağını öğrenin. Şunları nasıl yapacağınızı öğrenin:
> [!div class="checklist"]
> - Azure Site Recovery Panosu (kasası genel bakış sayfası) kullanın
> - İzleme ve çoğaltma sorunlarını giderme
> - İzleyici Azure Site kurtarma işleri/işlemleri
> - E-posta bildirimlerine abone olma

## <a name="using-the-azure-site-recovery-dashboard"></a>Azure Site Recovery panosunu kullanma

Azure Site Recovery Pano kasası genel bakış sayfasında tek bir konumda kasa için tüm izleme bilgileri birleştirir. Kasa panosunu ve Pano bölümleri giderek daha fazla bilgi almak için inebilirsiniz başlangıç. Azure Site Recovery Pano başlıca parçaları aşağıdaki gibidir:

### <a name="1-switch-between-azure-backup-and-azure-site-recovery-dashboards"></a>1. Panolar Azure Backup ve Azure Site Recovery arasında geçiş yapma

Sayfanın üst kısmındaki genel bakış geçiş anahtarı yedekleme ve Site kurtarma için Pano sayfaları arasında geçiş olanak sağlar. Bir kez yapıldıktan sonra bu seçim hatırlanan ve kasa için genel bakış sayfasında bir sonraki açışınızda varsayılan. Site Recovery panoyu görmek için Site kurtarma seçeneğini belirleyin. 

En son kullanılabilir bilgiler Pano yansıtması adına, Azure Site Recovery Pano sayfası çeşitli kısımlarını her 10 dakikada otomatik olarak yenileyin.

![Azure Site Recovery genel bakış sayfasında izleme özellikleri](media/site-recovery-monitor-and-troubleshoot/site-recovery-overview-page.png)

### <a name="2-replicated-items"></a>2. Çoğaltılan öğe

Çoğaltılan öğe bölümü kasaya korunan sunucuların çoğaltma durumunu genel bir bakış sunar. 

<table>
<tr>
    <td>Sorunsuz</td>
    <td>Çoğaltma, normal olarak bu sunucular ve hiçbir hata işlendiğini veya uyarı Belirtiler algılandı.</td>
</tr>
<tr>
    <td>Uyarı </td>
    <td>Bir veya daha fazla uyarı Belirtiler çoğaltma etkileyebilir veya çoğaltma normal şekilde devam değil belirtmek için bu sunucular algılandı.</td>
</tr>
<tr>
    <td>Kritik</td>
    <td>Bu sunucular için bir veya daha fazla kritik çoğaltma hatası belirtileri algılanmadı. Bu hata Belirtiler genellikle göstergesidir çoğaltmanın ya da takıldı veya oranı bu sunucular için verileri değiştirmek gibi hızlı ilerleyen değil.</td>
</tr>
<tr>
    <td>Uygulanamaz</td>
    <td>Şu anda, üzerinden başarısız sunucuları gibi çoğaltma olması beklenen olmayan sunucuları.</td>
</tr>
</table>

Çoğaltma durumunu tarafından filtrelenmiş korumalı sunucularının bir listesini görmek için çoğaltma durumu açıklama halka yanındaki tıklayın. Tüm bölüm başlığı bağlantı kasa için çoğaltılan öğeler sayfası için kısayol görünümüdür. Tüm kasadaki tüm sunucular listesini görmek için bağlantı görünümünü kullanın.

### <a name="3-failover-test-success"></a>3. Yük devretme sınaması başarılı

Yük devretme testi başarılı bölümü test yük devretme durumu temelinde kasasında sonu yukarı sanal makinelerin sayısını gösterir. 

<table>
<tr>
    <td>Önerilen test</td>
    <td>Sanal makineler, yük devretme başarılı testi zamanından bu yana uygulanmamış bunlar korumalı bir duruma ulaştı.</td>
</tr>
<tr>
    <td>Başarıyla gerçekleştirildi</td>
    <td>Bir veya daha fazla başarılı yük devretme sınaması işlemlerini beklendiğinden sanal makineler.</td>
</tr>
<tr>
    <td>Uygulanamaz</td>
    <td>Yük devretme sınaması için şu anda uygun olmayan sanal makineler. Örnekler: sunucuları üzerinde başarısız oldu, sunucuları hangi ilk çoğaltma için olan ilerleme durumu, bir yük devretme sürmekte olduğu sunucuları, kendisi için bir sınama yük devretmesi sürüyor sunucuları.</td>
</tr>
</table>

Test yük devretme durumu halka yanındaki tıklayın, böylece test yük devretme durumlarına göre korumalı sunucular listesine bakın.
 
> [!IMPORTANT]
> En iyi uygulama, en az altı ayda korumalı sunucularda yük devretme testi gerçekleştirmek önerilir. Yük devretme testi gerçekleştirme sunucuları ve uygulamaları yalıtılmış bir ortama yük devretme sınamasını olmayan kesintiye uğratan yoludur ve iş sürekliliği hazırlığı değerlendirmenize yardımcı olur.

 Yalnızca test yük devretme işlemi ve temizleme test yük devretme işlemi başarıyla tamamlandıktan sonra bir sunucu veya bir kurtarma planı üzerinde bir test yük devretme işlemi başarılı olarak kabul edilir.

### <a name="4-configuration-issues"></a>4. Yapılandırma sorunları

Yapılandırma sorunları bölümü başarıyla yük devretme sanal makinelere yeteneğinizi etkileyebilir sorunların bir listesini gösterir. Bu bölümde listelenen sorunları sınıfları şunlardır:
 - **Eksik yapılandırmaları:** korunan sunucularda kurtarma ağına veya kurtarma kaynak grubu gibi gerekli yapılandırmaları eksik.
 - **Eksik kaynakları:** yapılandırılmış hedef/kurtarma kaynakları bulunamadı veya aboneliği mevcut değil. Örneğin, kaynak silindi veya farklı bir abonelik veya kaynak grubu için geçirilmiş. Aşağıdaki hedef/kurtarma yapılandırmaları için kullanılabilirlik izlenir: hedef kaynak grubu, hedef sanal ağ ve alt ağ, günlük/hedef depolama hesabı, hedef kullanılabilirlik kümesi, hedef IP adresi.
 - **Abonelik kotası:** kullanılabilir abonelik kaynak kotası dengelemek için yük devretme olması gereken Bakiye karşılaştırılır kasadaki tüm sanal makineler. Kullanılabilir Bakiye yetersiz bulunursa, yetersiz kota Bakiye bildirilir. Aşağıdaki Azure kaynakları için kotaları izlenen: sanal makine çekirdek sayısı, sanal makine ailesi çekirdek sayısı, ağ arabirim kartı (NIC) sayısı.
 - **Yazılım güncelleştirmeleri:** yazılım sürümleri zaman aşımına uğramak yeni yazılım güncelleştirmeleri kullanılabilirliği.

Yapılandırma sorunları (dışında kullanılabilirliğini yazılım güncelleştirmeleri), varsayılan olarak 12 saatte bir çalışan düzenli Doğrulayıcı işlemi tarafından algılanır. Hemen sonra Yenile simgesine tıklayarak çalıştırmak için Doğrulayıcı işlem zorlayabilirsiniz *yapılandırma sorunları* bölüm başlığı.

Listelenen sorunlar ve onlar tarafından etkilenen sanal makineler hakkında daha fazla bilgi almak için bağlantıları tıklatın. Belirli sanal makinelerin etkileyen sorunlar için tıklayarak daha fazla bilgi alabilirsiniz **dikkat gösterilmesi** bağlantı sanal makine için hedef yapılandırmaları sütununun altında. Ayrıntılar nasıl algılanan sorunları düzeltebilir ilişkin öneriler içerir.

### <a name="5-error-summary"></a>5. Hata özeti

Hata Özet bölümünde, çoğaltma sunucularının yanı sıra her bir hata nedeniyle etkilenen varlıkların sayısı kasasında etkileyebilir şu anda etkin çoğaltma hatası belirtileri gösterir.

Çoğaltma hatası belirtileri sunucuları kritik veya uyarılan çoğaltma sistem durumu için hata özet tabloda görülebilir. 

- Şirket içi yapılandırma sunucusu, VMM sunucusunun veya Hyper-V ana bilgisayar üzerinde çalışan Azure Site kurtarma Sağlayıcısı'ndan bir sinyal olmayan-alındığını gibi şirket içi altyapı bileşenlerini etkileyen hataları Özet hata başında listelenen Bölüm
- Çoğaltma hatası belirtileri korumalı etkileyen sunucuları listelenir sonraki. Hata Özet Tablo girişleri hata önem derecesi azalan ve ardından göre azalan etkilenen sunucuları sayısı sıralanır.
 

> [!NOTE]
> 
>  Birden çok çoğaltma hatası belirtileri tek bir sunucu üzerinde gözlenen. Tek bir sunucuda birden çok hata Belirtiler varsa her hata belirti etkilenen sunucular listesinde sunucu sayar. Bir hata belirti kaynaklanan temel sorun düzeltildikten sonra çoğaltma parametreleri geliştirmek ve hata sanal makineden temizlenir.
>
> > [!TIP]
> Etkilenen sunucuları sayısı, tek bir arka plandaki sorunu birden çok sunucu etkileyen olmadığını anlamak için kullanışlı bir yoludur. Örneğin, bir ağ sistemimizde sorun büyük olasılıkla bir şirket içi sitesinden Azure'a çoğaltma tüm sunucuları etkileyebilir. Bu görünüm, bu çoğaltma birden çok sunucu için bir arka plandaki sorunu düzeltir düzelttikten hızlı bir şekilde iletir.
>

### <a name="6-infrastructure-view"></a>6. Altyapı görünümü

Altyapı görünümü senaryo akıllıca görsel bir çoğaltmaya katılan infrastructural bileşenlerinin sağlar. Ayrıca görsel olarak, çeşitli sunucular arasında ve sunucuları ve çoğaltma söz konusu Azure Hizmetleri arasındaki bağlantının durumunu gösterir. 

Yeşil bir çizgi Kaplanmış hata simgesi kırmızı bir çizgiyle ilgili bileşenleri arasında bağlantı etkileyen bir veya daha fazla hata Belirtiler varlığını göstergesidir bağlantı sağlıklı, olduğunu gösterir. Fare işaretçisini satırında hata simgenin üzerine gelerek veya onları, hata ve etkilenen varlıkların sayısını gösterir. 

Hata simgesine tıklayarak etkilenen varlıklar hataları için filtre uygulanmış bir listesini gösterir.

![Site Recovery altyapısı görünümü (kasasına)](media/site-recovery-monitor-and-troubleshoot/site-recovery-vault-infra-view.png)

> [!TIP]
> Şirket içi altyapı bileşenleri (yapılandırma VMware sanal makineleri, Hyper-V konakları, VMM sunucuları çoğaltma sunucusu, ek işlem sunucuları) Azure Site Recovery yazılımının en son sürümü çalıştırdığınızdan emin olun. Altyapı görünümü tüm özelliklerini kullanabilmek için çalışıyor olması gerekir [güncelleştirme paketi 22](https://support.microsoft.com/help/4072852) veya sonraki sürümünü Azure Site kurtarma

Altyapı görünümü kullanmak için kaynak ortamınıza bağlı olarak uygun çoğaltma senaryosuna (Azure sanal makinelerinde, VMware sanal makinelerini/fiziksel sunucu veya Hyper-V) seçin. Kasa genel bakış sayfasında sunulan altyapı görünümü, kasa için toplu bir görünümdür. Ayrıntıya inebilir aşağı tek tek bileşenlerine kutularını tıklayarak ilgili daha fazla.

Tek bir çoğaltma makine bağlamı için kapsamlı bir altyapı görünümü çoğaltılmış öğesi genel bakış sayfasında kullanılabilir. Bir çoğaltma sunucusu için genel bakış sayfasına gitmek için kasa menüsünden çoğaltılan öğeler'e gidin ve ayrıntılarını görmek için sunucuyu seçin.

### <a name="infrastructure-view---faq"></a>Altyapı görünümü - SSS

**S.** Neden VM'im altyapı görünümünü görüyorum değil mi? </br>
**C.** Altyapı görünümü özelliği yalnızca Azure'a çoğaltma sanal makineler için kullanılabilir. Bu özellik şu anda şirket içi siteler arasında çoğaltma sanal makineler için kullanılabilir değil.

**S.** Neden kasası altyapı görünümünde toplam sayısını farklı sanal makinelerin sayısı çoğaltılan öğeler halka gösterilir?</br>
**C.** Kasa altyapı görünümü çoğaltma senaryolarla kapsamlıdır. Şu anda seçili çoğaltma senaryoda katılan yalnızca sanal makineler altyapı görünümde gösterilen sanal makinelerin sayısı dahil edilmiştir. Ayrıca, seçili senaryo için yalnızca şu anda Azure'a çoğaltma için yapılandırılmış makineleri altyapı görünümde gösterilen sanal makinelerin sayısı dahil edilen (için örneğin: devredilen sanal makinelerin, çoğaltılan sanal makineler bir şirket içi siteye altyapı görünümünde dahil edilmez.)

**S.** Genel bakış sayfasında halka grafik Panoda gösterilen çoğaltılan öğeler toplam hata sayısı farklı essentials çekmecesini gösterilen çoğaltılan öğe sayısı neden oluyor?</br>
**C.** Yalnızca sanal makineleri hangi başlangıç çoğaltmasını tamamlamış için essentials bölmesinde gösterilen sayı eklenir. Toplam çoğaltılan öğeler halka kasaya sunucuları dahil olmak üzere tüm sanal makineleri içerir hangi ilk çoğaltma için şu anda devam ediyor.

**S.** Hangi çoğaltma senaryolarını altyapısı görünüm için kullanılabilir? </br>
**C.**
>[!div class="mx-tdBreakAll"]
>|Çoğaltma senaryosu  | VM durumu  | Görünüm altyapısı kullanılabilir  |
>|---------|---------|---------|
>|İki arasında çoğaltılan sanal makineler şirket içi sitelere     | -        | Hayır      |
>|Tümü     | Üzerinde başarısız oldu         |  Hayır       |
>|İki Azure bölgeler arasında çoğaltılan sanal makineler     | İlk çoğaltma devam ediyor ya da korumalı         | Evet         |
>|VMware sanal makinelerini Azure'a çoğaltma     | İlk çoğaltma devam ediyor ya da korumalı        | Evet        |
>|VMware sanal makinelerini Azure'a çoğaltma     | Sanal makineler geri bir şirket içi VMware siteye çoğaltılmasını üzerinde başarısız oldu         | Hayır        |
>|Hyper-V sanal makinelerini Azure'a çoğaltma     | İlk çoğaltma devam ediyor ya da korumalı        | Evet       |
>|Hyper-V sanal makinelerini Azure'a çoğaltma     | Devredilir / yeniden çalışma sürüyor        |  Hayır       |


### <a name="7-recovery-plans"></a>7. Kurtarma planları

Kurtarma planları bölümüne kasaya kurtarma planları sayısını gösterir. Kurtarma planları listesi görmek, yeni kurtarma planları oluşturabilir veya var olanları düzenlemek üzere numarasını tıklatın. 

### <a name="8-jobs"></a>8. İşler

Azure Site Recovery işleri Azure Site kurtarma işlemlerinin durumunu izler. Azure Site kurtarma işlemlerinin çoğu, işlemin ilerlemesini izlemek için kullanılan bir izleme işi ile zaman uyumsuz olarak çalıştırılır.  Bir işlemin durumunu izlemek öğrenmek için bkz: [İzleyici Azure Site Recovery işleri/işlemleri](#monitor-azure-site-recovery-jobsoperations) bölümü.

Bu işleri bölümü aşağıdaki bilgileri sağlar:

<table>
<tr>
    <td>Başarısız</td>
    <td>Son 24 saat içindeki Azure Site Recovery işleri başarısız oldu</td>
</tr>
<tr>
    <td>Sürüyor</td>
    <td>Devam eden azure Site Recovery işleri</td>
</tr>
<tr>
    <td>Giriş bekleniyor</td>
    <td>Şu anda azure Site Recovery işleri kullanıcıdan bir girdi bekliyor duraklatıldı.</td>
</tr>
</table>

Tümünü Görüntüle bölüm başlığının yanındaki işleri liste sayfasına gitmek için bir kısayol bağlantıdır.

## <a name="monitor-and-troubleshoot-replication-issues"></a>İzleme ve çoğaltma sorunlarını giderme

Kasa panosu sayfasında kullanılabilir bilgi ek olarak, ek ayrıntılar ve sanal makineler listesi sayfası ve sanal makine Ayrıntıları sayfası içindeki sorun giderme bilgilerini alabilir. Seçerek kasaya korunan sanal makinelerin listesini görüntüleyebilirsiniz **öğeleri çoğaltılan** kasa menüsünden seçeneği. Alternatif olarak, herhangi bir kasa panosu sayfasında kullanılabilen kapsamlı kısayolları tıklayarak filtre uygulanmış bir korumalı öğeler listesine alabilirsiniz.

![Site Recovery çoğaltılan öğeleri liste görünümü](media/site-recovery-monitor-and-troubleshoot/site-recovery-virtual-machine-list-view.png)

Çoğaltılan öğe listesi sayfasında filtre seçeneğini çoğaltma sistem durumunu ve çoğaltma ilkesi gibi çeşitli filtreler uygulamanıza olanak sağlar. 

Sütun Seçici seçeneği RPO, hedef yapılandırma sorunları ve çoğaltma hataları gibi gösterilecek ek sütunlar belirtmenize olanak sağlar. Sanal makine ya da Görünüm hataları makineler listesinin belirli bir satırın sağ tıklayarak sanal makineyi etkileyen işlemler başlatabilir.

Daha fazla ayrıntıya için bir sanal makineye tıklayarak seçin. Bu sanal makine Ayrıntılar sayfasını açar. Sanal makine ayrıntıları altında genel bakış sayfasında makineye ilgili ek bilgilere burada bulabilirsiniz bir Pano içerir. 

Çoğaltma makinesi için genel bakış sayfasında, bulabilirsiniz:
- RPO (kurtarma noktası hedefi): sanal makine ve hangi RPO'su son hesaplanan saat geçerli RPO.
- Makine için en son kullanılabilir kurtarma noktaları
- Herhangi biri makinenin yük devretme hazırlığı etkileyebilir, yapılandırma sorunları. Daha fazla bilgi almak için bağlantıyı tıklatın.
- Hata ayrıntıları: olası nedenleri birlikte makinede gözlenen ve düzeltmeler önerilen şu anda çoğaltma hatası belirtileri listesi
- Olayları: Bir kronolojik makineyi etkileyen en son olayların listesi. Hata ayrıntılarını gösterir ancak şu anda observable hata Belirtiler makinede olayları kaydıdır geçmiş önceden makine fazlasını hata Belirtiler dahil olmak üzere Makine etkilenen çeşitli olayları.
- Azure'a çoğaltılan makineler için altyapı görünümü

![Site Recovery çoğaltılan öğe ayrıntıları/genel bakış](media/site-recovery-monitor-and-troubleshoot/site-recovery-virtual-machine-details.png)

Sayfanın üstünde eylem menüsü seçenekleri gibi çeşitli işlemleri gerçekleştirmek için sanal makinede yük devretme sınamasını sağlar. Eylem menüsünde hata ayrıntıları düğmesi çoğaltma hataları, yapılandırma sorunları ve yapılandırma en iyi yöntemler dayalı uyarılar sanal makine için de dahil olmak üzere tüm etkin hataları görmenizi sağlar.

> [!TIP]
> RPO veya kurtarma noktası hedefi en son kullanılabilir kurtarma noktasından farklı mı?
> 
>Azure Site Recovery sanal makinelerini Azure'a çoğaltma için bir çoklu adım zaman uyumsuz işlemi kullanır. Çoğaltma sondan adımda meta verileri birlikte sanal makine üzerinde yapılan son değişikliklerin bir günlük/önbellek depolama hesabına kopyalanır. Bu değişiklikler kurtarılabilir noktasını tanımlamak üzere etiket birlikte yazılmış sonra hedef bölgede depolama hesabı, Azure Site Recovery kurtarılabilir noktası sanal makine oluşturmak gerekli bilgileri vardır. Bu noktada, depolama hesabına bugüne kadarki karşıya değişikliklerin RPO karşılandığından. Diğer bir deyişle, bu noktada sanal makine için RPO zaman, birimi olarak ifade edilen eşit olduğu süre miktarını geçen kurtarılabilir noktasına karşılık gelen zaman damgası gelen.
>
>Arka planda çalışan Azure Site Recovery hizmeti depolama hesabından karşıya yüklenen veriler toplar ve bunları sanal makine için oluşturulan çoğaltma disklere uygular. Bir kurtarma noktası oluşturur ve bu noktaya yük devretme işleminde kurtarma için kullanılabilir hale getirir. En son kullanılabilir kurtarma noktası zaten işlendikten ve çoğaltma disklere uygulanan en son kurtarma noktasını karşılık gelen zaman damgasını gösterir.
>> [!WARNING]
> Çarpık saati veya yanlış sistem saatini çoğaltma kaynak makinede veya şirket içi altyapı sunucularını RPO değeri eğme. Doğru RPO'su, raporlama emin olmak için değerleri çoğaltmasında ilgili sunuculardaki sistem saatinin doğru olduğundan emin olun. 
>

## <a name="monitor-azure-site-recovery-jobsoperations"></a>İzleyici Azure Site kurtarma işleri/işlemleri

Azure Site Recovery zaman uyumsuz olarak belirttiğiniz işlemlerini yürütür. Gerçekleştirebileceğiniz işlemler örnekleri çoğaltmayı etkinleştirmek, kurtarma planı oluşturun, yük devretme sınamasını, vb. çoğaltma ayarlarını güncelleştirin. Her tür işlem izlemek ve işlem denetlemek için oluşturulan karşılık gelen bir işi var. İş nesnesi durumunu ve işlemin ilerlemesini izlemek için gerekli tüm bilgiler vardır. İşlerini sayfasından kasa için çeşitli Site kurtarma işlemlerinin durumunu izleyebilirsiniz. 

Git kasa için Site Recovery işleri listesini görmek için **izleme ve Raporlama** select işleri ve kasa menüsünden bölümünü > Site Recovery işleri. Belirtilen iş üzerinde daha fazla ayrıntı elde etmek üzere tıklayarak işleri sayfasında listesinden bir iş seçin. Bir işi başarıyla tamamlandı kurmadı veya hatalar varsa, daha fazla bilgi hata ve olası düzeltmesinin sayfanın üst kısmındaki iş ayrıntıları (aynı zamanda erişilebilir başarısız üzerinde sağ işleri listesi sayfasından hata ayrıntıları düğmesine tıklayarak görebilirsiniz İş.) Belirli bir ölçüte dayalı listesini filtrelemek için Eylem menüsünde işleri listesi sayfanın üst kısmındaki filtre seçeneğini kullanın ve dışa aktarma düğmesi seçilen işler excel ayrıntılarını dışarı aktarmak için kullanın. Site Recovery panosu sayfasında kullanılabilir kısayol işleri liste görünümü de erişebilirsiniz. 

 Azure portalından gerçekleştirdiğiniz işlemleri için oluşturulan işi ve geçerli durumunu da Azure portal bildirimleri bölümünden (zil simgesine sağ üstte) izlenebilir.

## <a name="subscribe-to-email-notifications"></a>E-posta bildirimlerine abone olma

Yerleşik e-posta bildirimi özellik kritik olaylar için e-posta bildirimleri almak için abone olanak sağlar. Abone, e-posta bildirimleri için aşağıdaki olayları gönderilir:
- Çoğaltma sistem durumunu kritik olarak düşürmesini çoğaltılan bir makine.
- Azure Site Recovery hizmeti ile şirket içi altyapı bileşenleri arasındaki bağlantı yok. Yapılandırma sunucusu (VMware) veya Sistem Merkezi sanal makine Manager(Hyper-V) gibi şirket içi altyapı bileşenleri hizmetinden kasaya kayıtlı Site Recovery bağlantı sinyal mekanizmasını kullanarak algılandı.
- Yük devretme işlemi hataları varsa.

Azure Site Recovery için e-posta bildirimleri almak için abone olmak için Git **izleme ve Raporlama** kasası menüsünün bölümünde ve:
1. Uyarı ve olayları seçin > Site Recovery olaylar.
2. Açılan olaylar sayfanın en üstünde menüsünden "E-posta bildirimleri"'ı seçin.
3. E-posta bildirim Sihirbazı'nı açmak veya kapatmak e-posta bildirimleri ve bildirimlerinin alıcılarını seçmek için kullanın. Tüm abonelik yöneticileri bildirim gönderilmesini belirtebilir ve/veya bildirimleri göndermek için e-posta adreslerinin listesini sağlayın. 