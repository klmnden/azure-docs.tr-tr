---
title: İzleme ve sorun giderme Azure Site Recovery | Microsoft Docs
description: İzleme ve Azure Site Recovery çoğaltma sorunlarını ve işlemleri portalı kullanarak sorun giderme
services: site-recovery
documentationcenter: ''
author: bsiva
manager: abhemraj
editor: raynew
ms.assetid: ''
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/06/2018
ms.author: bsiva
ms.openlocfilehash: 84b5bf3be09083a69216802fc7f557de1a7f0ee6
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37917542"
---
# <a name="monitoring-and-troubleshooting-azure-site-recovery"></a>İzleme ve Azure Site Recovery sorun giderme

Bu makalede, Azure Site Recovery'nin yerleşik İzleme özelliklerini izleme ve sorun giderme için kullanmayı öğrenin. Şunları nasıl yapacağınızı öğrenin:
> [!div class="checklist"]
> - Azure Site Recovery Pano (kasası genel bakış sayfası) kullanın
> - İzleme ve çoğaltma sorunlarını giderme
> - İzleyici Azure Site kurtarma işleri/işlemleri
> - E-posta bildirimlerine abone olma

## <a name="using-the-azure-site-recovery-dashboard"></a>Azure Site Recovery panosunu kullanma

Azure Site Recovery kasası genel bakış sayfası Panoda tek bir konumda kasa için tüm izleme bilgilerini birleştirir. Kasa panosunun ve Pano bölümlerinde giderek daha fazla bilgi almak için derin Dalış başlatın. Azure Site Recovery Pano önemli bölümleri aşağıdaki gibidir:

### <a name="1-switch-between-azure-backup-and-azure-site-recovery-dashboards"></a>1. Azure Backup ve Azure Site Recovery arasında panolar geçiş

Genel Bakış sayfasının üst kısmındaki iki durumlu düğme, yedekleme ve Site Recovery için Pano sayfaları arasında geçiş sağlar. Bir kez yapıldıktan sonra bu seçime anımsanacak ve kasa için genel bakış sayfası bir sonraki açışınızda için varsayılan olarak. Site Recovery panoyu görmek için Site kurtarma seçeneğini belirleyin. 

Pano en son kullanılabilir bilgiler yansıtması adına, Azure Site Recovery Pano sayfasının çeşitli bölümlerini her 10 dakikada otomatik olarak yenileyin.

![Azure Site Recovery genel bakış sayfasında izleme özellikleri](media/site-recovery-monitor-and-troubleshoot/site-recovery-overview-page.png)

### <a name="2-replicated-items"></a>2. Çoğaltılan öğeler

Pano çoğaltılan öğeler bölümünü kasada korunan sunucuların çoğaltma durumu hakkında genel bakış sunar. 

<table>
<tr>
    <td>Sorunsuz</td>
    <td>Çoğaltma, normalde bu sunucular ve hata için İlerliyor veya uyarı belirtileri tespit ettik.</td>
</tr>
<tr>
    <td>Uyarı </td>
    <td>Bir veya daha fazla uyarı belirtileri çoğaltma etkileyebilir veya normal çoğaltmanın ilerlediğini değil belirtmek için bu sunucuların algılandı.</td>
</tr>
<tr>
    <td>Kritik</td>
    <td>Bu sunucular için bir veya daha fazla kritik çoğaltma hatası belirtileri algılanmadı. Bu hata Belirtiler genellikle göstergeleri olduğundan çoğaltma ya da takılı veya veri değişim oranı bu sunuculara yönelik olarak hızlı ilerlemiyor.</td>
</tr>
<tr>
    <td>Uygulanamaz</td>
    <td>Şu anda, yük devretme sunucuları gibi çoğaltma olması beklenen olmayan sunucuları.</td>
</tr>
</table>

Korumalı sunucular çoğaltma sağlamlığa göre filtrelenmiş bir listesini görmek için çoğaltma durumu açıklaması halka yanındaki tıklayın. Bölüm başlığı tüm bağlantı görünümü, çoğaltılan öğeler sayfaya kasa için bir kısayoldur. Tüm kasasındaki tüm sunucular listesini görmek için bağlantı görünümünü kullanın.

### <a name="3-failover-test-success"></a>3. Yük devretme denemesi başarılı

Pano yük devretme testi başarı bölümünü test yük devretme durumuna dayalı kasadaki sonu yukarı sanal makinelerin sayısını gösterir. 

<table>
<tr>
    <td>Sınama önerilir</td>
    <td>Başarılı test yük devretme işleminden uygulanmamış bir sanal makine, korumalı bir duruma ulaştı.</td>
</tr>
<tr>
    <td>Başarıyla gerçekleştirildi</td>
    <td>Bir veya daha fazla başarılı test yük devretmeleri olan sanal makineler.</td>
</tr>
<tr>
    <td>Uygulanamaz</td>
    <td>Yük devretme testi için şu anda uygun olmayan sanal makineler. Örnekler: sunucuya başarısız, hangi ilk çoğaltma sunucuları olduğu ilerleme durumunu, sunucular bir yük devretme sürüyor olduğundan, sunucular için test yük devretmesi sürüyor.</td>
</tr>
</table>

Test yük devretme durumu halka yanındaki tıklayın, böylece test yük devretme durumlarına göre korumalı sunucular listesine bakın.
 
> [!IMPORTANT]
> En iyi uygulama, her altı aylık en az bir kez, korumalı sunucularda yük devretme testi gerçekleştirmeniz önerilir. Yük devretme testi gerçekleştirme yük devretmesi, sunucuları ve yalıtılmış bir ortam için uygulamalarınızı test etmek için karışıklığa neden olmayan bir yoldur ve iş sürekliliği duruma hazırlıklı olma değerlendirmenize yardımcı olur.

 Test yük devretme işlemini hem temizleme testi yük devretme işlemi yalnızca başarıyla tamamladıktan sonra bir sunucu veya bir kurtarma planı üzerinde bir sınama yük devretmesi işlemini başarılı olarak kabul edilir.

### <a name="4-configuration-issues"></a>4. Yapılandırma sorunları

Yapılandırma sorunlarını bölümü başarıyla yük devretme sanal makinelere yeteneğinizi etkileyebilir sorunların bir listesini gösterir. Bu bölümde listelenen sorunların sınıfları şunlardır:
 - **Eksik yapılandırmaları:** korunan sunucularda gerekli yapılandırmaları gibi bir kurtarma ağı ya da kurtarma kaynak grubu eksik.
 - **Eksik kaynaklar:** yapılandırılan hedef/kurtarma kaynakları bulunamadı ya da abonelikte mevcut değil. Örneğin, kaynak silindi veya farklı bir abonelik veya kaynak grubu için geçirilmiş. Aşağıdaki hedef/kurtarma yapılandırmaları için kullanılabilirlik izlenir: hedef kaynak grubu, hedef sanal ağ ve alt ağ, günlük/hedef depolama hesabı, hedef kullanılabilirlik kümesi, hedef IP adresi.
 - **Abonelik kotası:** kullanılabilir abonelik kaynak kotası dengelemek için yük devretme için gerekli dengeyi karşılaştırılır kasasındaki tüm sanal makineler. Yetersiz kota Bakiye kullanılabilir bakiye yetersiz bulunursa bildirilir. Aşağıdaki Azure kaynakları için kotaları izleniyor: sanal makine çekirdek sayısı, sanal makine ailesi çekirdek sayısı, ağ arabirim kartı (NIC) sayısı.
 - **Yazılım güncelleştirmeleri:** kullanılabilirliği, yeni yazılım güncelleştirmeleri, yazılım sürümleri sona erecek.

Yapılandırma sorunları (dışında kullanılabilirliğini yazılım güncelleştirmeleri), varsayılan olarak her 12 saatte bir çalışan düzenli Doğrulayıcı işlemi tarafından algılanır. Hemen yanında Yenile simgesine tıklayarak çalıştırmak için Doğrulayıcı işlemi zorlayabilirsiniz *yapılandırma sorunlarını* bölüm başlığı.

Listelenen sorunlar ve bunları tarafından etkilenen sanal makineleri hakkında daha fazla bilgi almak için bağlantıları tıklatın. Belirli sanal makinelere etkileyen sorunlar için tıklayarak daha fazla ayrıntıya ulaşabilirsiniz **dikkat etmeniz gereken** sanal makine için hedef yapılandırmaları sütunu altındaki bağlantıyı. Ayrıntılar nasıl algılanan sorunları çözmek öneriler içerir.

### <a name="5-error-summary"></a>5. Hata özeti

Hata Özet bölümünde sunucularını her bir hata nedeniyle etkilenen varlıkların sayısı ile birlikte bir kasada çoğaltma etkileyebilir şu anda etkin çoğaltma hatası belirtileri gösterir.

Çoğaltma hatası belirtileri kritik veya uyarı çoğaltma sistem durumu sunucuları için hata özeti tablosunda görülebilir. 

- Şirket içi yapılandırma sunucusu, VMM sunucusunun veya Hyper-V konağında çalışan Azure Site Recovery Sağlayıcısı'ndan bir sinyal olmayan-kaydınızda gibi şirket içi altyapı bileşenlerini etkileyen hataları hata özeti başında listelenen Bölüm
- Çoğaltma hatası belirtileri korumalı etkileyen sunucularının listelendiğini sonraki. Hata Özet Tablo girişleri ve etkilenen sunucuları sayısının azalan göre azalan hata önem derecesi olarak sıralanır.
 

> [!NOTE]
> 
>  Birden çok çoğaltma hatası belirtileri, tek bir sunucu üzerinde gözlemlenen. Tek bir sunucu üzerinde birden çok hata Belirtiler varsa her hata belirti etkilenen sunucularının listesinde sunucu sayılacaktır. İçinde bir hata belirti kaynaklanan temel sorun düzeltildikten sonra çoğaltma parametrelerini iyileştirmek ve hata sanal makineden kaldırılır.
>
> > [!TIP]
> Etkilenen sunucuları sayısı, tek bir arka plandaki sorunu birden çok sunucu etkiliyor olabilecek anlamak için kullanışlı bir yoldur. Örneğin, bir sorun büyük olasılıkla tüm sunucuların bir şirket içi siteden Azure'a çoğaltılması etkileyebilir. Bu görünüm, düzeltme, temel sorunlardan biri, birden çok sunucu için çoğaltma düzelttiğinizden emin hızlı bir şekilde yürütür.
>

### <a name="6-infrastructure-view"></a>6. Altyapı görünümü

Altyapı görünümü senaryo akıllıca görsel bir temsilini Çoğaltmada ilgili altyapısal bileşenleri sağlar. Ayrıca, görsel olarak, çeşitli sunucular arasında ve sunucu ve çoğaltma ilgili Azure Hizmetleri arasındaki bağlantının durumunu gösterir. 

Yeşil bir çizgi ile Kaplanmış hata simgesi kırmızı bir çizgi dahil olan bileşenleri arasındaki bağlantı etkileyen bir veya daha fazla hata belirtilerin varlığını çağrılırken bağlantı iyi ve olduğunu gösterir. Fare işaretçisi satırında hata simgenin üzerine geldiğinizde, hata ve etkilenen varlık sayısını gösterir. 

Hata simgesine tıklayarak, etkilenen varlıkların hataları için filtrelenmiş bir listesini gösterir.

![Site Recovery altyapı görünümü (kasa)](media/site-recovery-monitor-and-troubleshoot/site-recovery-vault-infra-view.png)

> [!TIP]
> Şirket içi altyapı bileşenleri (yapılandırma VMware sanal makinelerini Hyper-V konakları, VMM sunucuları çoğaltma sunucusu, ek işlem sunucularının) Azure Site Recovery yazılımının en son sürümü çalıştırdığınızdan emin olun. Altyapı görünümü özelliklerinin tümünü kullanabilmek için çalıştırılması gereken [güncelleştirme paketi 22](https://support.microsoft.com/help/4072852) veya Azure Site Recovery için sonraki

Altyapı görünümü kullanmak için kaynak ortamınıza bağlı olarak uygun çoğaltma senaryosu (Azure sanal makinelerini, VMware sanal makinelerini/fiziksel sunucu veya Hyper-V) seçin. Kasa genel bakış sayfasında sunulan altyapı görünümü, kasa için toplu bir görünümüdür. Ayrıntıya inebilir aşağı tek tek bileşenlere, kutularına tıklayarak daha fazla.

Tek bir çoğaltma makinesi bağlamı için kapsamlı bir altyapı görünümü, çoğaltılan öğeyi genel bakış sayfasında kullanılabilir. Bir çoğaltma sunucusu için genel bakış sayfasına gitmek için kasa menüsünden çoğaltılan öğeler'e gidin ve ayrıntılarını görmek için sunucuyu seçin.

### <a name="infrastructure-view---faq"></a>Altyapı görünümü - SSS

**S.** Neden sanal Makinem için altyapı görünümü görüyorum değil mi? </br>
**C.** Altyapı görünümü özelliği, yalnızca Azure'a çoğaltılan sanal makineler için kullanılabilir. Bu özellik şu anda şirket içi siteler arasında çoğaltılan sanal makineler için kullanılabilir değildir.

**S.** Kasa görünümünde altyapı toplam sayısından farklı sanal makinelerin sayısı, çoğaltılan öğeler halkada neden gösterilir?</br>
**C.** Kasa altyapı görünümü çoğaltma senaryoları tarafından belirlenir. Yalnızca sanal makineler şu anda seçili çoğaltma senaryoda katılan altyapı Görünümü'nde gösterilen sanal makinelerin sayısı dahil edilmiştir. Ayrıca, seçilen senaryo için yalnızca şu anda Azure'a çoğaltma için yapılandırılmış olan sanal makinelerin altyapı Görünümü'nde gösterilen sanal makinelerin sayısı dahil edilen (Fo örnek: sanal makineler üzerinde başarısız oldu, çoğaltılan sanal makineler yedekleme bir şirket içi siteye altyapı Görünümü'nde yer almaz.)

**S.** Genel bakış sayfasında toplam sayısını halka grafiği Panoda gösterilen çoğaltılan öğeler, farklı essentials çekmecede gösterilen çoğaltılmış öğe sayısı neden oluyor?</br>
**C.** Bu sanal makineler yalnızca hangi ilk çoğaltma tamamlanana için essentials çekmecede gösterilen sayısı eklenir. Çoğaltılan öğeler halka toplam kasaya sunucuları dahil olmak üzere tüm sanal makineleri içerir. şu anda hangi ilk çoğaltma için devam ediyor.

**S.** Hangi çoğaltma senaryoları, altyapı görünümü kullanılabilir? </br>
**C.**
>[!div class="mx-tdBreakAll"]
>|Çoğaltma senaryosu  | VM durumu  | Altyapı görünümü kullanılabilir  |
>|---------|---------|---------|
>|İki arasında çoğaltılan sanal makineler şirket içi site     | -        | Hayır      |
>|Tümü     | Yük devretti         |  Hayır       |
>|İki Azure bölgeleri arasında çoğaltılan sanal makineler     | İlk çoğaltma sürüyor veya korumalı         | Evet         |
>|VMware sanal makinelerini Azure'a çoğaltma     | İlk çoğaltma sürüyor veya korumalı        | Evet        |
>|VMware sanal makinelerini Azure'a çoğaltma     | Bir şirket içi VMware sitesinde yeniden çoğaltılan sanal makine yük devretti         | Hayır        |
>|Hyper-V sanal makinelerini Azure'a çoğaltma     | İlk çoğaltma sürüyor veya korumalı        | Evet       |
>|Hyper-V sanal makinelerini Azure'a çoğaltma     | Yük devretme / yeniden çalışma sürüyor        |  Hayır       |


### <a name="7-recovery-plans"></a>7. Kurtarma planları

Kurtarma planları bölümünde kasaya kurtarma planları sayısı gösterilir. Kurtarma planları listesini görmek için daha yeni kurtarma planları oluşturun veya varolanları düzenlemek için numarasına tıklayın. 

### <a name="8-jobs"></a>8. İşler

Azure Site Recovery işleri, Azure Site Recovery işlemlerini durumunu izleyin. Çoğu Azure Site Recovery işlemleri, işlemin ilerlemesini izlemek için kullanılan bir izleme işi ile zaman uyumsuz olarak yürütülür.  Bir işlemin durumunu izlemek öğrenmek için bkz: [İzleyici Azure Site Recovery işleri/işlemleri](#monitor-azure-site-recovery-jobsoperations) bölümü.

Panonun bu işleri bölümüne aşağıdaki bilgileri sağlar:

<table>
<tr>
    <td>Başarısız</td>
    <td>Azure Site Recovery işleri son 24 saat içindeki başarısız oldu</td>
</tr>
<tr>
    <td>Sürüyor</td>
    <td>Şu anda sürmekte olan azure Site Recovery işleri</td>
</tr>
<tr>
    <td>Giriş bekleniyor</td>
    <td>Şu anda azure Site Recovery işleri, bir kullanıcı girişi bekleniyor durduruldu.</td>
</tr>
</table>

Tümünü Görüntüle bağlantıyı bölüm başlığının yanındaki işler listesi sayfasına gitmek için bir kısayol bulunur.

## <a name="monitor-and-troubleshoot-replication-issues"></a>İzleme ve çoğaltma sorunlarını giderme

Kasa panosu sayfasında bulunan bilgilere ek olarak, ek ayrıntılar ve sanal makineler listesi sayfasına ve sanal makine Ayrıntıları sayfası sorun giderme bilgilerini alabilirsiniz. Kasada korunan sanal makinelerin listesini seçerek görüntüleyebilirsiniz **çoğaltılan öğeler** kasa menüsünden seçeneği. Alternatif olarak, kasa panosu sayfasında kullanılabilir kapsamlı kısayolları tıklayarak korumalı öğelerin filtrelenmiş bir listesini alabilirsiniz.

![Site Recovery çoğaltılan öğeleri listesi görünümü](media/site-recovery-monitor-and-troubleshoot/site-recovery-virtual-machine-list-view.png)

Çoğaltılan öğeler listesi sayfasında filtre seçeneğini çoğaltma durumu ve çoğaltma ilkesi gibi çeşitli filtreler uygulamanıza olanak tanır. 

Sütun Seçici seçeneği RPO, hedef yapılandırma sorunlarını ve çoğaltma hataları gibi gösterilecek ek sütunlar belirtmenize olanak sağlar. Bir sanal makine veya makinelerinin listesi belirli bir satıra sağ tıklayarak sanal makineyi etkileyen hataları görüntüleme işlemleri başlatabilirsiniz.

Daha fazla detaya gitmek için bir sanal makineye tıklayarak seçin. Bu, sanal makine Ayrıntıları sayfası açılır. Genel bakış sayfasında sanal makine ayrıntıları altında bir pano, makineye ilgili ek bilgileri burada bulabilirsiniz içerir. 

Çoğaltma makinesi için genel bakış sayfasında, aşağıdakileri bulacaksınız:
- RPO (kurtarma noktası hedefi): sanal makine ve hangi RPO son Hesaplandı saat için geçerli RPO.
- Makine için en son kullanılabilir kurtarma noktaları
- Yapılandırma sorunları, herhangi bir makinenin yük devretmeye hazırlık etkileyebilir. Daha ayrıntılı bilgi edinmek için bağlantıya tıklayın.
- Hata ayrıntıları: olası nedenleri birlikte makinede gözlemlenen ve önerilen düzeltmeler şu anda çoğaltma hatası belirtileri listesi
- Olayları: Makine etkileyen en son olayların kronolojik bir liste. Hata ayrıntılarını gösterir ancak şu anda gözlemlenebilir hatası belirtileri makinede olayları daha önce makine için etmişsinizdir hatası belirtileri dahil olmak üzere makine etkilemiş olabilecek çeşitli olayların bir geçmiş kaydını olduğu.
- Altyapı görünümü Azure'e çoğaltılan makineler için

![Site Recovery çoğaltılan öğe ayrıntıları/genel bakış](media/site-recovery-monitor-and-troubleshoot/site-recovery-virtual-machine-details.png)

Sayfanın üstünde eylem menü seçenekleri gibi çeşitli işlemleri gerçekleştirmek için sanal makine üzerinde yük devretme testi sağlar. Eylem menüsünde hata Ayrıntılar düğmesine çoğaltma hataları, yapılandırma sorunlarını ve sanal makine için en iyi temel uygulama uyarıları yapılandırma dahil olmak üzere tüm etkin hataları görmenizi sağlar.

> [!TIP]
> RPO veya kurtarma noktası hedefi en son kullanılabilir kurtarma noktasından farklı mı?
> 
>Azure Site Recovery, sanal makinelerini Azure'a çoğaltma için çok adımlı zaman uyumsuz bir işlemin kullanır. Çoğaltma sondan adımda, sanal makine meta verileri ile birlikte en son değişiklikleri bir günlük/önbellek depolama hesabına kopyalanır. Bu değişikliklerle birlikte kurtarılabilir noktasını tanımlamak üzere etiket yazılan sonra hedef bölgede depolama hesabına, Azure Site Recovery sanal makine kurtarılabilir bir noktası oluşturmak gereken bilgileri var. Bu noktada, şimdiye kadarki depolama hesabına yüklediniz değişikliklerin RPO karşılanıp karşılanmadığını. Diğer bir deyişle, bu noktada sanal makinenin RPO süresini birimleri ifade eşittir geçen süreyi gelen kurtarılabilir noktasına karşılık gelen zaman damgası.
>
>Arka planda çalışan Azure Site Recovery hizmeti, depolama hesabından karşıya yüklenen verileri alır ve bunları sanal makine için oluşturulan çoğaltma diskine uygular. Bir kurtarma noktası oluşturur ve bu noktaya yük devretme sırasında kurtarma için kullanılabilir hale getirir. En son kullanılabilir kurtarma noktası, zaten işlendikten ve çoğaltma diskleri için uygulanan en son kurtarma noktasına karşılık gelen zaman damgasını gösterir.
>> [!WARNING]
> Hesaplanan RPO değeri dengesiz saat veya çoğaltma kaynak makine ya da şirket içi altyapı sunucularını yanlış sistem saatiyle eğme. Doğru RPO raporlamayı emin olmak için değerleri, Çoğaltmada sunucular üzerindeki sistem saatinin doğru olduğundan emin olun. 
>

## <a name="monitor-azure-site-recovery-jobsoperations"></a>İzleyici Azure Site kurtarma işleri/işlemleri

Azure Site Recovery işlemlerini zaman uyumsuz olarak belirttiğiniz yürütür. Gerçekleştirebileceğiniz işlemleri örnekler çoğaltmayı etkinleştirin, kurtarma planı oluşturma, yük devretme testi, vb. çoğaltma ayarlarını güncelleştirin. Her bir işlem izleme ve denetim işlemi için oluşturduğunuz karşılık gelen bir işi var. İş nesnesi durum ve işlemin ilerlemesini izlemek için gereken tüm bilgileri var. Kasa için çeşitli Site Recovery işlemlerini durumunu işleri sayfasında izleyebilirsiniz. 

Git kasa için Site Recovery işleri listesini görmek için **izleme ve raporlar** bölümünü seçin işleri ve kasa menüsünden > Site Recovery işleri. Belirtilen işin ayrıntılı bilgi edinmek için tıklayarak işleri sayfasında listesinden bir iş seçin. Bir işi başarıyla tamamlandı dolmadığından veya hataları varsa, daha fazla bilgi hata ve olası düzeltme ile iş Ayrıntılar sayfası (Ayrıca erişilebilir başarısız üzerinde sağ tıklayarak işler listesi sayfasından üstünde hata Ayrıntılar düğmesine tıklayarak görebilirsiniz İş.) Belirli ölçütlere göre listesini filtrelemek için Eylem menüsünde işler listesi sayfanın üst kısmındaki filtre seçeneğini kullanın ve excel için seçilen işlerinin ayrıntılarını dışarı aktarmak için Dışarı Aktar düğmesini kullanın. Site Recovery panosu sayfasında kullanılabilir kısayol işleri liste görünümü de erişebilirsiniz. 

 Azure portalından gerçekleştirme işlemleri için oluşturulan işi ve bunların geçerli durumlarını ayrıca Azure portalındaki bildirimler bölümünden (zil simgesi sağ üst kısımdaki) izlenebilir.

## <a name="subscribe-to-email-notifications"></a>E-posta bildirimlerine abone olma

Kritik olaylar için e-posta bildirimleri almak için abone yerleşik e-posta bildirimi özelliği sağlar. Abone e-posta bildirimleri için aşağıdaki olaylar gönderilir:
- Çoğaltma durumu kritik olarak önemli bir çoğaltma makinesi.
- Azure Site Recovery hizmeti ile şirket içi altyapı bileşenleri arasındaki bağlantı yok. Site Recovery yapılandırma sunucusu (VMware) veya System Center sanal makine Manager(Hyper-V) gibi şirket içi altyapı bileşenlerini hizmetinden kasaya kaydedilen bağlantısı sinyal mekanizması kullanılarak algılandı.
- Yük devretme işlemi hataları varsa.

Azure Site Recovery için e-posta bildirimleri almak için abone olmak için Git **izleme ve raporlar** kasa menüsünden bölümünü ve:
1. Uyarıları ve olayları seçin > Site Recovery etkinlikleri.
2. "E-posta bildirimleri" menüsünde açılan olaylar sayfanın en üstünde seçin.
3. E-posta bildirimi Sihirbazı, e-posta bildirimleri açıp kapatabilir ve bildirimlerinin alıcılarını seçmek için kullanın. Tüm abonelik yöneticileri bildirimleri gönderilmesini belirtebilir ve/veya bildirimleri göndermek için e-posta adreslerinden oluşan bir liste sağlar. 
