---
title: "Operations Management Suite içinde hizmet Haritası çözümü kullanın | Microsoft Docs"
description: "Hizmet eşlemesi otomatik olarak sistemlerde, Windows ve Linux uygulama bileşenleri bulur ve Hizmetleri arasındaki iletişimi eşleyen bir Operations Management Suite çözümüdür. Bu makalede hizmet Haritası ortamınıza dağıtmak ve çeşitli senaryolarda içinde kullanma ile ilgili ayrıntıları sağlar."
services: operations-management-suite
documentationcenter: 
author: daveirwin1
manager: jwhit
editor: tysonn
ms.assetid: 3ceb84cc-32d7-4a7a-a916-8858ef70c0bd
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/22/2016
ms.author: daseidma;bwren;dairwin
ms.openlocfilehash: 993dff7657a73803ca21677e19b08946fb89bfa2
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2018
---
# <a name="use-the-service-map-solution-in-operations-management-suite"></a>Operations Management Suite hizmet Haritası çözümde kullanın
Hizmet Eşlemesi, Windows ve Linux sistemleri üzerindeki uygulama bileşenlerini otomatik olarak bulur ve hizmetler arasındaki iletişimi eşler. Hizmet eşlemesi ile bunları düşündüğünüz şekilde sunucularınızı görüntüleyebilirsiniz: kritik Hizmetleri sunmak birbirine bağlı sistemler. Bir aracı yüklemesini dışındaki bağlantı noktaları üzerinden tüm TCP bağlı mimarisi herhangi bir yapılandırma gerekli ve hizmet eşlemesi sunucuları, işlemleri arasındaki bağlantıları gösterir.

Bu makalede hizmet eşlemesi kullanarak ilişkin ayrıntılar açıklanmaktadır. Hizmet Haritası ve ekleme aracıları yapılandırma hakkında daha fazla bilgi için bkz: [Operations Management Suite yapılandırma hizmet Haritası çözümde](operations-management-suite-service-map-configure.md).


## <a name="use-cases-make-your-it-processes-dependency-aware"></a>Kullanım: BT'niz olun bağımlılık kullanan işler

### <a name="discovery"></a>Bulma
Hizmet eşlemesi bağımlılıkları ortak başvuru haritasını sunucularınızı, işlemleri ve üçüncü taraf hizmetleri otomatik olarak oluşturur. Bulur ve beklenmedik biçimde bağlantıları, uzak üçüncü taraf sistemleri üzerinde bağlı ve ağınızın Active Directory gibi geleneksel koyu alanlara bağımlılıkları tanımlayan tüm TCP bağımlılıkları eşler. Hizmet eşlemesi olası sunucu yanlış yapılandırma, hizmet kesintisi ve ağ sorunları belirlemenize yardımcı olmak için yönetilen sistemlerinizde çalıştığınız başarısız ağ bağlantıları bulur.

### <a name="incident-management"></a>Olay yönetimi
Hizmet eşlemesi sistemleri nasıl bağlandığını gösteren ve birbirlerine etkileyen sorun yalıtım konusunda güvenilir bilgiler önlenmesine yardımcı olur. Başarısız bağlantı tanımlayan ek olarak, yanlış yapılandırılmış yük Dengeleyiciler, önemli hizmetler, şaşırtıcı veya aşırı yükünü belirleyin ve istemciler, geliştirici makineleri üretim sistemlerine konuşuyormuş gibi standart dışı yardımcı olur. Tümleşik iş akışları Operations Management Suite değiştirmek izleme ile birlikte kullanarak, bir olayın kök nedeni hizmeti açıklar veya değişiklik olayı olup bir arka uç makinede de görebilirsiniz.

### <a name="migration-assurance"></a>Geçiş güvence
Hizmet eşlemesi kullanarak, etkili bir şekilde planlayabilir, hızlandırmak ve hiçbir şey geride bıraktığı beklenmedik biçimde kesintiler meydana gelmediğinden emin olun ve yardımcı olan Azure geçişler doğrulayın. Birlikte geçirmek, sistem yapılandırmanıza ve kapasite değerlendirmek ve çalışan bir sistemi kullanıcılar hala sunma veya geçiş yerine yetkisini için bir adaydır olup olmadığını belirlemek için gereken tüm bağımlı sistemleri bulabilir. Taşıma işlemi tamamlandıktan sonra istemci yükleme ve test sistemleri ve müşteriler bağlandığını doğrulamak için kimliği kontrol edebilirsiniz. Alt ağ planlama ve güvenlik duvarı tanımlarınızın sorunlarınız varsa, hizmet Haritası Maps başarısız bağlantıları, bağlantı gereken sistemleri gelin.

### <a name="business-continuity"></a>İş sürekliliği
Azure Site Recovery kullanıyorsanız ve uygulama ortamınız hizmet eşlemesi için kurtarma dizisi tanımlama Yardım otomatik olarak size nasıl sistemleri birbirine emin olmak için kullanır gösterebilir kurtarma planını güvenilirdir. Veya bir grup kritik sunucu seçme ve istemcilerine görüntüleme, sunucunun geri yüklenen ve kullanılabilir olduktan sonra kurtarmak için hangi ön uç sistemleri tanımlayabilirsiniz. Buna karşılık, kritik sunucularının arka uç bağımlılıkları bakarak odak sistemlerinizi geri önce kurtarmak için hangi sistemleri tanımlayabilirsiniz.

### <a name="patch-management"></a>Düzeltme Eki Yönetimi
Hizmet eşlemesi, düzeltme eki uygulama sistemlerinizi aşağı olabilmesi bunları önceden bildirebilir şekilde diğer ekipler ve sunucular hizmetinizi üzerinde bağlı olan göstererek Operations Management Suite Sistem Güncelleştirme değerlendirmesi kullanımınız geliştirir. Hizmet eşlemesi, hizmetlerinizin kullanılabilir ve sonra düzgün biçimde bağlı olup bunların düzeltme eki yeniden ve göstererek Operations Management Suite, düzeltme eki yönetimi de geliştirir.


## <a name="mapping-overview"></a>Eşleme genel bakış
Hizmet eşlemesi aracıları, yüklü sunucu üzerindeki tüm TCP bağlı işlemler hakkında bilgi ve her işlem için gelen ve giden bağlantılar hakkında ayrıntılı bilgileri toplayın. Sol bölmede listesinde makine ya da belirtilen zaman aralığı üzerinde bağımlılıklarını görselleştirmek için hizmet Haritası aracıları olan grupları seçebilirsiniz. Belirli bir makine odaklanmak makine bağımlılık eşler ve doğrudan TCP istemciler veya sunucular bu makinenin tüm makinelerde göster.  Makine grubu eşlemeleri sunucular ve onların bağımlılıkları kümelerini gösterir.

![Hizmet eşlemesi'ne genel bakış](media/oms-service-map/service-map-overview.png)

Makineler çalışmasını göstermek için eşlemesinde Genişletilebilir işlem grupları ve etkin ağ bağlantıları işlemlerle seçilen zaman aralığı içinde. İşlem ayrıntıları göstermek için bir hizmet Haritası Aracısı ile uzak makineye genişletildiğinde odak makineyle iletişim kuran işlemler gösterilir. Odağı makinesine bağlanma aracısız ön uç makineler sayısı bağlandıkları işlemleri sol tarafında belirtilir. Odak makine aracı olan bir arka uç makineye bağlantı değiştirirken, aynı bağlantı noktası numarası için diğer bağlantılar ile birlikte bir sunucu bağlantı noktası grubu arka uç sunucu bulunmaktadır.

Varsayılan olarak, hizmet Haritası eşlemeleri son 30 dakika bağımlılık bilgi gösterir. Sol üst zamanı denetimlerini kullanarak geçmiş zaman aralıkları (örneğin, bir olay sırasında veya bir değişiklik oluşmadan önce) bağımlılıkları geçmişte nasıl Aranan göstermek için bir saat için maps sorgulayabilirsiniz. Hizmet eşlemesi veri Ücretli çalışma alanlarında 30 gün ve ücretsiz çalışma alanlarında 7 gün için depolanır.

## <a name="status-badges-and-border-coloring"></a>Durum rozetleri ve kenarlık renklendirme
Eşlemesindeki her sunucunun altındaki server hakkındaki durum bilgilerini saymayı durum rozetleri listesi olabilir. Rozetleri Operations Management Suite çözüm tümleştirmeler birini sunucudan ilgili bazı bilgiler olduğunu gösterir. Bir gösterge tıklamak sizi doğrudan sağ bölmede durum ayrıntılarını götürür. Şu anda kullanılabilir durum rozetleri uyarılar, hizmet Masası, değişiklikleri, güvenlik ve güncelleştirmeleri içerir.

Durum rozetleri önem derecesine bağlı olarak, makine düğümünü kenarlık renkli kırmızı (kritik), sarı (uyarı) olması veya (bilgilendirme) mavi. Renk herhangi birinin durumu rozetleri en ciddi durumunu temsil eder. Gri kenarlık hiç durum göstergesi olan bir düğüm gösterir.

![Durum rozetleri](media/oms-service-map/status-badges.png)

## <a name="process-groups"></a>İşlem grupları
İşlem gruplarının bir işlem gruba bir ortak ürün veya hizmet ile ilişkili işlemler birleştirin.  Bir makine düğüm genişletildiğinde tek başına işlemler işlem gruplarının birlikte görüntüler.  Tüm gelen ve giden bağlantılara bir işlem grubundaki bir işlem başarısız oldu, ardından bağlantı için bütün işlem grubu başarısız olarak gösterilir.

## <a name="machine-groups"></a>Makine grupları
Makine gruplar, sunucular, yalnızca bir tek eşleminde çok katmanlı uygulama veya sunucu kümesinin tüm üyelerinin görebilmeleri kümesi kalmaz eşlemeleri görmenize olanak sağlar.

Kullanıcıların hangi sunucuların birlikte bir gruba ait ve grup için bir ad seçin seçin.  Ardından, tüm işlemleri ve bağlantıları grubuyla veya yalnızca işlemleri ve doğrudan grubunun diğer üyeleriyle ilgili bağlantılarıyla görüntüleyin seçebilirsiniz.

![Makine grubu](media/oms-service-map/machine-group.png)

### <a name="creating-a-machine-group"></a>Makine grubu oluşturma
Bir grup oluşturmak için makine ya da makinelerinizde listelemek ve'ı tıklatın, istediğiniz makineleri seçin **gruba Ekle**.

![Grup Oluştur](media/oms-service-map/machine-groups-create.png)

Burada, seçebileceğiniz **Yeni Oluştur** ve grubuna bir ad verin.

![Ad grubu](media/oms-service-map/machine-groups-name.png)

>[!NOTE]
>Makine grubu için 10 sunucu şu anda sınırlı, ancak yakında bu sınırı artırmak planlıyoruz.

### <a name="viewing-a-group"></a>Bir grup görüntüleme
Bazı grupları oluşturduktan sonra bunları gruplar sekmesini seçerek görüntüleyebilirsiniz.

![Grupları sekmesi](media/oms-service-map/machine-groups-tab.png)

Ardından bu makine grubu için harita görüntülemek için Grup adı seçin.
![Makine grubu](media/oms-service-map/machine-group.png) grubuna ait makineleri eşlemesi beyaz özetlenen.

Grup genişletme makine grubu oluşturan makineleri listeler.

![Makine grubu makineleri](media/oms-service-map/machine-groups-machines.png)

### <a name="filter-by-processes"></a>İşleme göre filtre uygulama
Grup ve makine grubu için doğrudan ilgili olanları tüm işlemleri ve bağlantıları gösteren arasında eşleme görünümüne geçiş yapabilirsiniz.  Varsayılan görünüm, tüm işlemler göstermektir.  Haritanın üstündeki filtre simgesini tıklatarak görünümü değiştirebilirsiniz.

![Filtre grubu](media/oms-service-map/machine-groups-filter.png)

Zaman **tüm işlemler** olan seçili harita tüm işlemleri ve her makinede bağlantılarında grubunda dahil edilir.

![Makine grubu tüm işler](media/oms-service-map/machine-groups-all.png)

Görünümü yalnızca gösterecek şekilde değiştirirseniz **Grup bağlı işlemler**, harita yalnızca bu işlemleri ve Basitleştirilmiş bir görünümü oluşturma gruptaki diğer makinelere doğrudan bağlı bağlantıları daraltabilir.

![Makine grubu işlemleri filtrelenir](media/oms-service-map/machine-groups-filtered.png)
 
### <a name="adding-machines-to-a-group"></a>Makineler gruba ekleme
Makineler varolan bir gruba eklemek istediğiniz ve ardından makineler yanındaki kutuları işaretleyin **gruba Ekle**.  Ardından, makinelerine eklemek istediğiniz grubu seçin.
 
### <a name="removing-machines-from-a-group"></a>Makineler bir gruptan kaldırma
Grupları listesinde makine grubu makinelerinizde listelemek için Grup adını genişletin.  Çıkarmak istediğiniz makinede yanındaki üç nokta menüsünde ardından **kaldırmak**.

![Makineyi grubundan Kaldır](media/oms-service-map/machine-groups-remove.png)

### <a name="removing-or-renaming-a-group"></a>Kaldırma veya bir grubu yeniden adlandırma
Grup listesinde grup adının yanındaki üç nokta menüsünde'ı tıklatın.

![Makine grubu menüsü](media/oms-service-map/machine-groups-menu.png)


## <a name="role-icons"></a>Rol simgesi
Belirli işlemleri belirli rolleri makinelerde hizmet: web sunucuları, uygulama sunucuları, veritabanı ve benzeri. Hizmet eşlemesi bir bakışta bir işlem rol veya sunucu yürütür tanımlamaya yardımcı olmak üzere rol simgeler işlem ve makine kutularıyla açıklama ekler.

| Rol simgesi | Açıklama |
|:--|:--|
| ![Web sunucusu](media/oms-service-map/role-web-server.png) | Web sunucusu |
| ![Uygulama sunucusu](media/oms-service-map/role-application-server.png) | Uygulama sunucusu |
| ![Veritabanı sunucusu](media/oms-service-map/role-database.png) | Veritabanı sunucusu |
| ![LDAP sunucusu](media/oms-service-map/role-ldap.png) | LDAP sunucusu |
| ![SMB sunucusu](media/oms-service-map/role-smb.png) | SMB sunucusu |

![Rol simgesi](media/oms-service-map/role-icons.png)


## <a name="failed-connections"></a>Başarısız bağlantı sayısı
Başarısız bağlantı gösterilen işlemleri ve bilgisayarlar, hizmet Haritası Maps ile bir işlem veya bağlantı noktası ulaşmak bir istemci sistemi başarısız olduğunu gösteren bir kesikli kırmızı satır. Bu sistem başarısız bağlantı kurmaya çalışan bir ise başarısız bağlantıları dağıtılan bir hizmet Haritası Aracısı ile herhangi bir sistemden raporlanır. Hizmet eşlemesi bağlantı kurmak için başarısız TCP yuvaları izlenerek bu işlem ölçer. Bu hata, bir güvenlik duvarı, istemci veya sunucu ya da kullanılabilir durumda uzak bir hizmet yetersizliğini'ndan neden olabilir.

![Başarısız bağlantı sayısı](media/oms-service-map/failed-connections.png)

Başarısız bağlantı sorunlarını giderme ile geçiş doğrulama, güvenlik analizi ve genel mimari anlama yardımcı olabilir anlama. Başarısız bağlantı bazen zararsız, ancak bunlar genellikle doğrudan aniden ulaşılamaz olmadan bir yük devretme ortamında veya Bulut geçişten sonra konuşun erişememe iki uygulama katmanı gibi bir sorun üzerine gelin.

## <a name="client-groups"></a>İstemci grupları
İstemci, bağımlılık aracıları olmayan istemci makineler temsil eden kutuları harita üzerinde gruplarıdır. Tek bir istemci grubundaki tek tek işlem veya makine istemcilerde temsil eder.

![İstemci grupları](media/oms-service-map/client-groups.png)

Bir istemci grubundaki sunucuların IP adreslerini görmek için grubu seçin. Grubun içeriğini listelenen **istemci grubu özellikleri** bölmesi.

![İstemci grubu özellikleri](media/oms-service-map/client-group-properties.png)

## <a name="server-port-groups"></a>Sunucu bağlantı noktası grupları
Sunucu bağlantı noktası, sunucu bağlantı noktaları bağımlılık aracıları olan değil sunucularda temsil eden kutuları gruplarıdır. Kutunun sunucu bağlantı noktası ve bağlantıları Bu bağlantı noktasına sahip sunucuları sayısını içerir. Tek tek sunucular ve bağlantıları görmek için kutusunu genişletin. Kutuya yalnızca bir sunucu ise, adı veya IP adresi listelenir.

![Sunucu bağlantı noktası grupları](media/oms-service-map/server-port-groups.png)

## <a name="context-menu"></a>Bağlam menüsü
Üst üç nokta (...) tıklayarak herhangi bir sunucu sağındaki bu sunucu için bağlam menüsünde görüntüler.

![Başarısız bağlantı sayısı](media/oms-service-map/context-menu.png)

### <a name="load-server-map"></a>Yük sunucu eşleme
Tıklatarak **yük Server haritasını** yeni odak makine seçilen sunucuyla yeni bir eşleme için alır.

### <a name="show-self-links"></a>Göster kendine bağlantılar
Tıklatarak **Göster Self-Links** başlangıç ve bitiş sunucu süreçlerinde üzerinde TCP bağlantısı olan herhangi bir de dahil olmak üzere sunucu düğümü kendine bağlantılar, çizer. Varsa kendine bağlantılar gösterilir, menü komutu değişiklikleri **Gizle Self-Links**, böylece, bunları kapatabilirsiniz.

## <a name="computer-summary"></a>Bilgisayar özeti
**Makine Özet** bölmesinde bir sunucunun işletim sistemi, bağımlılık sayısı ve diğer Operations Management Suite çözümleri verilerden genel bakış içerir. Bu tür veriler performans ölçümleri, hizmet Masası anahtarları, değişiklik izleme, güvenlik ve güncelleştirmeleri içerir.

![Makine Özet bölmesi](media/oms-service-map/machine-summary.png)

## <a name="computer-and-process-properties"></a>Bilgisayar ve işlem özellikleri
Bir hizmet eşlemini gittiğinizde makineler ve bunların özelliklerini hakkında ek bağlam kazanmak için işlemleri seçebilirsiniz. Makineler DNS adı, IPv4 adresleri, CPU ve bellek kapasitesi, VM türü, işletim sistemi ve sürümü, en son yeniden zaman ve bunların Operations Management Suite ve hizmet eşlemesi aracıları kimlikleri hakkında bilgi sağlar.

![Makine Özellikler bölmesi](media/oms-service-map/machine-properties.png)

Çalışan işlemleri, işlem adı, işlem açıklaması, kullanıcı adı ve etki alanına katana (Windows), şirket adı, ürün adı, ürün sürümü, çalışma dizini, komut satırı ve işlem de dahil olmak üzere ilgili işletim sistemi meta verilerden işlem ayrıntılarını toplayın Başlangıç zamanı.

![İşlem Özellikler bölmesi](media/oms-service-map/process-properties.png)

**İşlem özeti** bölmesinde işlemin bağlantı, ilişkili bağlantı noktaları, gelen ve giden bağlantılar dahil olmak üzere ilgili ek bilgi sağlar ve bağlantıları başarısız oldu.

![İşlem Özet bölmesi](media/oms-service-map/process-summary.png)

## <a name="operations-management-suite-alerts-integration"></a>Operations Management Suite uyarıları tümleştirme
Hizmet eşlemesi seçilen zaman aralığı içinde seçili sunucu için Mazotlu uyarıları göstermek için Operations Management Suite uyarıları ile tümleştirir. Geçerli uyarıların varsa bir simge sunucu görüntüler ve **makine uyarıları** bölmesi uyarıları listeler.

![Makine uyarıları bölmesi](media/oms-service-map/machine-alerts.png)

İlgili uyarıları görüntülemek hizmet Haritası etkinleştirmek için belirli bir bilgisayar için tetiklenen uyarı kuralı oluşturun. Uygun uyarıları oluşturmak için:
- Bilgisayar tarafından bir yan tümcesi grubuna ekleyin (örneğin, **bilgisayar aralığı 1 dakika**).
- Uyarı için ölçüm Ölçümde göre seçin.

![Uyarı yapılandırması](media/oms-service-map/alert-configuration.png)


## <a name="operations-management-suite-log-events-integration"></a>Operations Management Suite günlük olayları tümleştirme
Hizmet Haritası günlük seçilen zaman aralığı içinde seçili sunucu için tüm kullanılabilir günlük olaylarının sayısını göstermek için arama ile tümleştirir. Listedeki tüm satır günlük arama atlamak ve tek tek günlük olayları görmek için olay sayısının tıklatabilirsiniz.

![Makine günlük olaylarının bölmesi](media/oms-service-map/log-events.png)

## <a name="operations-management-suite-service-desk-integration"></a>Operations Management Suite hizmeti Masası tümleştirme
Her iki çözüm de etkin ve Operations Management Suite çalışma alanınızda yapılandırılan hizmet Haritası tümleştirme BT Hizmet Yönetimi Bağlayıcısı ile otomatik olarak yapılır. Hizmet eşlemesinde tümleştirme "Hizmet Masasına." olarak etiketlenmiş Daha fazla bilgi için bkz: [merkezi olarak ITSM iş öğelerini BT Hizmet Yönetimi Bağlayıcısı'nı kullanarak yönetme](https://docs.microsoft.com/azure/log-analytics/log-analytics-itsmc-overview).

**Makine hizmet Masası** bölmesi, seçilen zaman aralığındaki seçili sunucu için tüm BT Hizmet Yönetimi olayları listeler. Sunucu bir simge geçerli öğeler varsa ve bu makine hizmet masasına bölmesinde bunları listeler görüntüler.

![Makine hizmet masasına bölmesi](media/oms-service-map/service-desk.png)

Bağlı ITSM çözümünüzde öğesini açmak için tıklatın **görünümü iş öğesi**.

Günlük aramada öğenin ayrıntılarını görüntülemek için **Göster günlük aramada**.


## <a name="operations-management-suite-change-tracking-integration"></a>Operations Management Suite değişiklik izleme tümleştirme
Her iki çözüm de etkin ve Operations Management Suite çalışma alanınızda yapılandırılan değişiklik izleme ile hizmet Haritası tümleştirme otomatik olarak yapılır.

**Makine değişiklik izleme** bölmesi, ek ayrıntılar için günlük arama aşağıya doğru incelemek için bir bağlantı birlikte en son ilk ile tüm değişiklikleri listeler.

![Makine değişiklik izleme bölmesi](media/oms-service-map/change-tracking.png)

Aşağıdaki resimde görebileceğiniz bir ConfigurationChange olay ayrıntılı görünümüdür seçtikten sonra **Göster günlük analizi**.

![ConfigurationChange olayı](media/oms-service-map/configuration-change-event.png)


## <a name="operations-management-suite-performance-integration"></a>Operations Management Suite performans tümleştirme
**Makine performansını** bölmesi seçilen sunucu için standart performans ölçümleri görüntüler. Ölçümleri tarafından gönderilen ve alınan ağ bayt CPU kullanımı, bellek kullanımı, gönderilen ve alınan ağ bayt ve üst işlemlerin bir listesini içerir.

![Makine performans bölmesi](media/oms-service-map/machine-performance.png)

Performans verileri görmek için gerekebilir [uygun günlük analizi performans sayaçlarını etkinleştirme](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-performance-counters).  Etkinleştirmek istediğiniz sayaçları:

Windows:
- Ýþlemci(*)\\% işlemci zamanı
- Bellek\\% Kaydedilmiş Bayt yüzdesi
- Adapter(*) ağ\\gönderilen bayt/sn
- Adapter(*) ağ\\alınan bayt/sn

Linux:
- Ýþlemci(*)\\% işlemci zamanı
- Memory(*)\\% kullanılan bellek
- Adapter(*) ağ\\gönderilen bayt/sn
- Adapter(*) ağ\\alınan bayt/sn

Ağ performans verilerini almak için ayrıca Operations Management Suite kablo verileri 2.0 çözümde etkinleştirmiş olmanız gerekir.
 
## <a name="operations-management-suite-security-integration"></a>Operations Management Suite güvenlik tümleştirme
Her iki çözüm de etkin ve Operations Management Suite çalışma alanınızda yapılandırılmış güvenlik ve Denetim ile hizmet Haritası tümleştirme otomatik olarak yapılır.

**Makine güvenliği** bölmesi seçilen sunucu için Operations Management Suite güvenlik ve denetim çözümü verileri gösterir. Bölmesi, seçilen zaman aralığı içinde sunucu için bekleyen güvenlik sorunları özetini listeler. Güvenlik sorunları ayrıntısına hiçbirini aşağı içine bunları hakkındaki ayrıntılar için günlük arama'yı tıklatın.

![Makine güvenlik bölmesi](media/oms-service-map/machine-security.png)


## <a name="operations-management-suite-updates-integration"></a>Operations Management Suite güncelleştirmeleri tümleştirme
Her iki çözüm de etkin ve Operations Management Suite çalışma alanınızda yapılandırılan güncelleştirme yönetimi ile hizmet Haritası tümleştirme otomatik olarak yapılır.

**Makinesi Güncelleştirmeleri** bölmesi seçilen sunucu için Operations Management Suite güncelleştirme yönetimi çözümü verileri görüntüler. Bölmesi, seçilen zaman aralığı içinde sunucu için eksik güncelleştirmeleri özetini listeler.

![Makine değişiklik izleme bölmesi](media/oms-service-map/machine-updates.png)

## <a name="log-analytics-records"></a>Log Analytics kayıtları
Hizmet eşlemesi bilgisayar ve işlem Envanter verileri için kullanılabilir [arama](../log-analytics/log-analytics-log-searches.md) günlük analizi içinde. Geçiş planlaması, kapasite analizi, bulma ve isteğe bağlı performans sorunlarını giderme senaryoları için bu verileri uygulayabilirsiniz.

Her benzersiz bir bilgisayar ve bir işlem veya bilgisayar başlatıldığında veya hizmet Haritası dahil edilmiş oluşturulan kayıtları yanı sıra işlemi için saatte bir kayıt oluşturulur. Bu kayıtları aşağıdaki tablolarda özelliklere sahip. Alanlar ve değerler ServiceMapComputer_CL olaylarda ServiceMap Azure Kaynak Yöneticisi API'si makine kaynak alanlarının eşlenir. Alanlar ve değerler ServiceMapProcess_CL olaylarda ServiceMap Azure Kaynak Yöneticisi API'si işlem kaynak alanlarını eşleme. ResourceName_s alan ad alanında karşılık gelen Resource Manager kaynak eşleşir. 

>[!NOTE]
>Hizmet eşleme özellikleri arttıkça, bu alanlar değiştirilebilir bağlıdır.

Benzersiz işlemleri ve bilgisayarları tanımlamak için kullanabileceğiniz dahili olarak oluşturulan özellikleri şunlardır:

- Bilgisayar: Kullanım ResourceId veya ResourceName_s bir bilgisayara bir Operations Management Suite çalışma alanı içinde benzersiz şekilde tanımlamak için.
- İşlem: bir işlemi bir Operations Management Suite çalışma alanı içinde benzersiz şekilde tanımlamak için kullanım ResourceId. ResourceName_s (MachineResourceName_s) işlemin çalıştığı makine bağlamı içinde benzersizdir 

Belirtilen işlem ve belirtilen zaman aralığında bilgisayar için birden çok kayıt bulunabileceğinden sorguları birden fazla kayıt aynı bilgisayarda veya işlem için geri dönebilirsiniz. Yalnızca en son kaydı eklemek için Ekle "| Yinelenenleri kaldırma ResourceId"sorgulanamıyor.

### <a name="servicemapcomputercl-records"></a>ServiceMapComputer_CL kayıtları
Bir tür kayıtlarıyla *ServiceMapComputer_CL* hizmet Haritası aracıları olan sunucular için Envanter verilerini sahip. Bu kayıtları aşağıdaki tabloda özelliklere sahiptir:

| Özellik | Açıklama |
|:--|:--|
| Tür | *ServiceMapComputer_CL* |
| SourceSystem | *OpsManager* |
| ResourceId | Çalışma alanı içinde bir makine için benzersiz tanımlayıcı |
| ResourceName_s | Çalışma alanı içinde bir makine için benzersiz tanımlayıcı |
| ComputerName_s | Bilgisayar FQDN'si |
| Ipv4Addresses_s | Bir listesi sunucunun IPv4 adresleri |
| Ipv6Addresses_s | Bir liste sunucunun IPv6 adresleri |
| DnsNames_s | DNS adları dizisi |
| OperatingSystemFamily_s | Windows veya Linux |
| OperatingSystemFullName_s | İşletim sisteminin tam adı  |
| Bitness_s | Verileri makinenin (32 bit veya 64 bit)  |
| PhysicalMemory_d | MB fiziksel bellek |
| Cpus_d | CPU sayısı |
| CpuSpeed_d | MHz CPU hızı|
| VirtualizationState_s | *Bilinmeyen*, *fiziksel*, *sanal*, *hiper yönetici* |
| VirtualMachineType_s | *hyperv*, *vmware*, vb. |
| VirtualMachineNativeMachineId_g | VM, hiper yönetici tarafından atanan kimliği |
| VirtualMachineName_s | VM adı |
| BootTime_t | Önyükleme saati |



### <a name="servicemapprocesscl-type-records"></a>ServiceMapProcess_CL türünde kayıtları
Bir tür kayıtlarıyla *ServiceMapProcess_CL* hizmet Haritası aracılarıyla sunucularında TCP bağlı işlemler için Envanter verilerini sahip. Bu kayıtları aşağıdaki tabloda özelliklere sahiptir:

| Özellik | Açıklama |
|:--|:--|
| Tür | *ServiceMapProcess_CL* |
| SourceSystem | *OpsManager* |
| ResourceId | Çalışma alanı içinde bir işlem için benzersiz tanımlayıcı |
| ResourceName_s | Üzerinde çalıştığı makine içinde bir işlem için benzersiz tanımlayıcı|
| MachineResourceName_s | Makinenin kaynak adı |
| ExecutableName_s | İşlem yürütülebilir dosyası adı |
| StartTime_t | İşlem havuzu başlangıç zamanı |
| FirstPid_d | İşlem havuzunda ilk PID |
| Description_s | İşlem açıklaması |
| CompanyName_s | Şirket adı |
| InternalName_s | İç adı |
| ProductName_s | Ürün adı |
| ProductVersion_s | Ürün sürümü |
| FileVersion_s | Dosya sürümü |
| CommandLine_s | Komut satırı |
| ExecutablePath _Yanları | Yürütülebilir dosya yolu |
| WorkingDirectory_s | Çalışma dizini |
| UserName | Hesabın altında işlemi yürütülüyor |
| UserDomain | Etki alanı altında işlemi yürütülüyor |


## <a name="sample-log-searches"></a>Örnek günlük aramaları

### <a name="list-all-known-machines"></a>Bilinen tüm makineler listesi
ServiceMapComputer_CL | özetlemek arg_max(TimeGenerated, *) ResourceId tarafından

### <a name="list-the-physical-memory-capacity-of-all-managed-computers"></a>Yönetilen tüm bilgisayarlara fiziksel bellek kapasitesini listeleyin.
ServiceMapComputer_CL | özetlemek arg_max(TimeGenerated, *) ResourceId tarafından | Proje PhysicalMemory_d, ComputerName_s

### <a name="list-computer-name-dns-ip-and-os"></a>Liste bilgisayar adı, DNS, IP ve işletim sistemi.
ServiceMapComputer_CL | özetlemek arg_max(TimeGenerated, *) ResourceId tarafından | Proje ComputerName_s, OperatingSystemFullName_s, DnsNames_s, Ipv4Addresses_s

### <a name="find-all-processes-with-sql-in-the-command-line"></a>Komut satırında "sql" tüm işlemlerle Bul
ServiceMapProcess_CL | Burada CommandLine_s contains_cs "sql" | özetlemek arg_max(TimeGenerated, *) ResourceId tarafından

### <a name="find-a-machine-most-recent-record-by-resource-name"></a>Bir makine (en son kayıt) tarafından kaynak adı bulunamadı
(ServiceMapComputer_CL) "m-4b9c93f9-bc37-46df-b43c-899ba829e07b" aramada | özetlemek arg_max(TimeGenerated, *) ResourceId tarafından

### <a name="find-a-machine-most-recent-record-by-ip-address"></a>IP adresini kullanarak bir makine (en son kayıt) bulunamadı
(ServiceMapComputer_CL) "10.229.243.232" aramada | özetlemek arg_max(TimeGenerated, *) ResourceId tarafından

### <a name="list-all-known-processes-on-a-specified-machine"></a>Belirtilen bir makinedeki tüm bilinen işlemleri listele
ServiceMapProcess_CL | Burada MachineResourceName_s "m-559dbcd8-3130-454d-8d1d-f624e57961bc" == | özetlemek arg_max(TimeGenerated, *) ResourceId tarafından

### <a name="list-all-computers-running-sql"></a>SQL çalıştıran tüm bilgisayarları listeleyin
ServiceMapComputer_CL | Burada, ResourceName_s (((ServiceMapProcess_CL) aramada "\*sql\*" | farklı MachineResourceName_s)) | ayrı ComputerName_s

### <a name="list-all-unique-product-versions-of-curl-in-my-datacenter"></a>My veri merkezinde curl tüm benzersiz ürün sürümleri listesi
ServiceMapProcess_CL | Burada ExecutableName_s "curl" == | farklı ProductVersion_s

### <a name="create-a-computer-group-of-all-computers-running-centos"></a>CentOS çalıştıran tüm bilgisayarları bir bilgisayar grubu oluştur
ServiceMapComputer_CL | Burada OperatingSystemFullName_s contains_cs "CentOS" | farklı ComputerName_s


## <a name="rest-api"></a>REST API
Tüm hizmet Haritası sunucu, işlem ve bağımlılık verilerde aracılığıyla kullanılabilir [hizmet eşlemesi REST API](https://docs.microsoft.com/rest/api/servicemap/).


## <a name="diagnostic-and-usage-data"></a>Tanılama ve kullanım verileri
Microsoft otomatik olarak hizmet Haritası hizmet kullanımınız vasıtasıyla kullanım ve performans verilerini toplar. Microsoft bu verileri sağlamak ve kalitesini, güvenlik ve hizmet eşlemesi hizmet bütünlüğünü geliştirmek için kullanır. Doğru ve etkili sorun giderme özellikleri sağlamak için veriler işletim sistemi ve sürümü, IP adresi, DNS adı ve iş istasyonu adı gibi yazılımınızın yapılandırması hakkında bilgi içerir. Microsoft, ad, adres veya diğer kişi bilgilerini toplamaz.

Veri toplama ve kullanım hakkında daha fazla bilgi için bkz: [Microsoft Online Services gizlilik bildirimi](https://go.microsoft.com/fwlink/?LinkId=512132).


## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinmek [oturum aramaları](../log-analytics/log-analytics-log-searches.md) hizmet Haritası tarafından toplanan verileri almak için günlük analizi içinde.


## <a name="troubleshooting"></a>Sorun giderme
Bkz: [sorun giderme bölümüne yapılandırma hizmet Haritası belgenin](operations-management-suite-service-map-configure.md#troubleshooting).


## <a name="feedback"></a>Geri Bildirim
Tüm geri bildirim bize hizmet haritası veya bu belge hakkında var?  Ziyaret bizim [kullanıcı sesi sayfa](https://feedback.azure.com/forums/267889-log-analytics/category/184492-service-map), buradan özellikleri önermek veya varolan önerileri oy verin.
