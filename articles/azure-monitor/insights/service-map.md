---
title: Hizmet eşlemesi çözümünü kullanarak Azure'da | Microsoft Docs
description: Hizmet Eşlemesi, Windows ve Linux sistemleri üzerindeki uygulama bileşenlerini otomatik olarak bulan ve hizmetler arasındaki iletişimi eşleyen bir Azure çözümüdür. Bu makalede, ortamınızda hizmet eşlemesi dağıtmak ve çeşitli senaryoları de kullanım için Ayrıntılar sağlanır.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: 3ceb84cc-32d7-4a7a-a916-8858ef70c0bd
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/28/2018
ms.author: magoedte
ms.openlocfilehash: 0c654070e2bbeb8ee5dbc64fe9b4f58ee97f2e47
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60000734"
---
# <a name="using-service-map-solution-in-azure"></a>Azure'da hizmet eşlemesi çözümünü kullanma
Hizmet Eşlemesi, Windows ve Linux sistemleri üzerindeki uygulama bileşenlerini otomatik olarak bulur ve hizmetler arasındaki iletişimi eşler. Hizmet Eşlemesi ile, sunucularınızı planladığınız şekilde kullanabilirsiniz: kritik hizmetler sunabilen birbirine bağlı sistemler. Sunucu Eşlemesi, aracının yüklenmesi dışında herhangi bir yapılandırma gerektirmeden sunucular, işlemler, gelen ve giden bağlantıların gecikme süresi ile TCP aracılığıyla bağlı mimarilerdeki bağlantı noktaları arasındaki bağlantıları gösterir.

Bu makalede, ekleme ve hizmet eşlemesi kullanarak ayrıntılarını açıklar. Hizmet eşlemesi ve aracı ekleme işlemi yapılandırma hakkında daha fazla bilgi için bkz: [yapılandırma hizmet eşlemesi çözümünü azure'da]( service-map-configure.md).

>[!NOTE]
>Hizmet eşlemesi zaten dağıttıysanız, artık Ayrıca, maps Azure İzleyici'de VM'ler için VM durumunu ve performansını izlemek için ek özellikler içeren görüntüleyebilirsiniz. Daha fazla bilgi için bkz. [Vm'lere genel bakış için Azure İzleyici](../../azure-monitor/insights/vminsights-overview.md).


## <a name="sign-in-to-azure"></a>Azure'da oturum açma
[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="enable-service-map"></a>Hizmet eşlemesini etkinleştir
1. Azure portalında **+ kaynak Oluştur**.
2. Arama çubuğuna yazın **hizmet eşlemesi** basın **Enter**.
3. Market arama sonuçları sayfasını seçin **hizmet eşlemesi** listeden.<br><br> ![Azure Market araması sonuçlarından hizmet eşlemesi çözümünü seçin](./media/service-map/marketplace-search-results.png)<br>
4. Üzerinde **hizmet eşlemesi** genel bakış bölmesinde, çözüm ayrıntıları gözden geçirin ve ardından **Oluştur** Log Analytics çalışma alanınıza onboarding işlemine başlamak için.<br><br> ![Yerleşik hizmet eşlemesi çözümünü](./media/service-map/service-map-onboard.png).
5. İçinde **çözüm yapılandırma** bölmesinde mevcut bir seçin veya yeni bir Log Analytics çalışma alanı oluşturun.  Yeni bir çalışma alanı oluşturma hakkında daha fazla bilgi için bkz: [Azure portalında Log Analytics çalışma alanı oluşturma](../../azure-monitor/learn/quick-create-workspace.md). Gerekli bilgileri girdikten sonra tıklayın **Oluştur**.  

Bilgiler doğrulanır ve çözümün dağıtılır, ancak altında ilerleme durumunu izleyebilirsiniz **bildirimleri** menüsünde. 

Hizmet eşlemesi, Log Analytics çalışma alanını Azure portalından erişmek ve seçeneğini **çözümleri** sol bölmeden.<br><br> ![Çalışma alanındaki çözümler seçeneğini](./media/service-map/select-solution-from-workspace.png).<br> Çözümleri listesinden seçin **ServiceMap(workspaceName)** ve hizmet eşlemesi Özet kutucuğu hizmet eşlemesi çözümü genel bakış sayfasını tıklatın.<br><br> ![Hizmet eşlemesi Özet kutucuğu](./media/service-map/service-map-summary-tile.png).

## <a name="use-cases-make-your-it-processes-dependency-aware"></a>Kullanım örnekleri: BT olun bağımlılık kullanan işler

### <a name="discovery"></a>Bulma
Hizmet eşlemesi, sunucu, işlemler ve üçüncü taraf hizmetleri arasında ortak başvuru haritasında bağımlılıkları otomatik olarak oluşturur. Bulur ve şaşkınlık bağlantılar, uzak üçüncü taraf sistemlerde, bağımlı ve ağınızın Active Directory gibi geleneksel koyu alanlarını bağımlılıklarını tanımlama, tüm TCP bağımlılıkları eşler. Hizmet eşlemesi, olası sunucu yanlış yapılandırma, hizmet kesintisi ve ağ sorunları belirlemenize yardımcı olur, yönetilen sistemlerinizdeki etmek çalıştığınız başarısız ağ bağlantıları bulur.

### <a name="incident-management"></a>Olay yönetimi
Hizmet eşlemesi, sistemleri nasıl bağlandığını gösteren ve birbirlerine etkileyen sorun yalıtım kararın ortadan yardımcı olur. Başarısız bağlantılar belirlemeye ek olarak, yanlış yapılandırılmış yük Dengeleyiciler, kritik Hizmetleri şaşırtıcı veya aşırı yük belirlenmesi ve istemcileri, üretim sistemlerine konuşan bir geliştirici makineleri gibi standart dışı yardımcı olur. Değişiklik izleme ile tümleşik iş akışlarını kullanarak, değişiklik olayı olup bir arka uç makinede de görebilirsiniz ya da hizmet bir olayın kök nedenini açıklar.

### <a name="migration-assurance"></a>Geçiş güvencesi
Hizmet eşlemesi kullanarak, etkili bir şekilde planlayabilir, hızlandırın ve Azure'a geçişleri, hiçbir şey geride bıraktığı ve şaşkınlık kesintiler meydana gelmediğinden emin olun yardımcı olan doğrulayın. Geçişini birlikte, sistem yapılandırmanıza ve kapasite değerlendirmenize ve çalışan bir sistemi hala kullanıcıya hizmet veren veya geçiş yerine yetkisinin alınması için bir adaydır olup olmadığını belirlemek için gereken tüm bağımlı sistemlere bulabilir. Taşıma tamamlandıktan sonra istemci yükleme ve test sistemleri ve müşterilerin bağlandığını doğrulamak için kimliği kontrol edebilirsiniz. Alt ağ planlaması ve güvenlik duvarı tanımlarınızın sorunları varsa, hizmet eşlemesi Maps başarısız bağlantılar bağlantı gerektiren sistemleri gelin.

### <a name="business-continuity"></a>İş sürekliliği
Azure Site Recovery kullanıyorsanız ve uygulama ortamınızı, hizmet eşlemesi için kurtarma dizisi tanımlama Yardım otomatik olarak size nasıl sistemleri birbirine emin olmak için kullanan gösterebilir, Kurtarma planınızın güvenilirdir. Kritik bir sunucunun veya Grup seçme ve istemcilerine görüntüleme, hangi ön uç sistemleri sunucunun geri yüklenen ve kullanılabilir olduktan sonra kurtarılır tanımlayabilirsiniz. Buna karşılık, kritik sunucularının arka uç bağımlılıkları bakarak hangi sistemlerin odak sistemlerinizi geri yüklenmeden önce kurtarılır tanımlayabilirsiniz.

### <a name="patch-management"></a>Düzeltme Eki Yönetimi
Hizmet eşlemesi, düzeltme eki uygulama sistemlerinizi aşağı yapmadan önce bunları önceden bildirebilir bırakmayarak diğer ekipler ve sunucuları, hizmete bağlıdır göstererek Sistem Güncelleştirme değerlendirmesi kullanımını geliştirir. Hizmet eşlemesi, hizmetlerinizi kullanılabilir ve sonra düzgün şekilde bağlı olup olmadığını, düzeltme eki yeniden ve göstererek düzeltme eki yönetimi de geliştirir.

## <a name="mapping-overview"></a>Eşleme genel bakış
Hizmet eşlemesi Aracısı tüm TCP bağlantılı işlemleri hakkındaki bilgileri burada yüklü sunucuda ve her işlem için gelen ve giden bağlantılar hakkında bilgi toplayın.

Sol bölmedeki listeden makine ya da belirtilen zaman aralığı üzerinde bağımlılıkları görselleştirmek için hizmet eşlemesi aracıları olan grupları seçebilirsiniz. Makine bağımlılık belirli bir makinede odak eşler ve bunlar doğrudan TCP istemciler veya sunucular bu makinenin olan tüm makineler gösterin.  Makine grubu haritalar, sunucuları ve bağımlılıklarını kümesi gösterir.

![Hizmet eşlemesi genel bakış](media/service-map/service-map-overview.png)

Makineleri çalıştığını göstermek için haritada genişletilebileceğini seçilen zaman aralığında grupları ve etkin ağ bağlantılarıyla işlemleri işleyin. Hizmet eşlemesi Aracısı ile bir uzak makine, işlem ayrıntıları göstermek için genişletildiğinde odak makineyle iletişim kuran işlemleri gösterilmektedir. Odak makineye bağlanın ve ön uç aracısız makinelerin sayısı, bağlandıkları işlemlerin sol tarafta gösterilir. Odak makine bağlantısı aracı olan bir arka uç makine yapıyor, arka uç sunucu diğer bağlantıları aynı bağlantı noktası numarası ile birlikte bir sunucu bağlantı noktası grubu dahil edilir.

Varsayılan olarak, hizmet eşlemesi haritalar son 30 dakika bağımlılık bilgileri gösterir. Üst sol zamanı denetimleri kullanarak, maps geçmiş zaman aralıkları (örneğin, bir olay sırasında veya bir değişikliği oluşmadan önce) bağımlılıkları geçmişte nasıl baktığı göstermek için bir saat için sorgulayabilirsiniz. Hizmet eşlemesi veriler, 7 gün içinde ücretsiz çalışma alanı yanı sıra, 30 gün boyunca Ücretli çalışma alanlarında depolanır.

## <a name="status-badges-and-border-coloring"></a>Durumu rozetlerinin ve kenarlık renkleri
Haritadaki her bir sunucunun altındaki sunucu hakkındaki durum bilgilerini yaygınlaşmıştır durumu rozetlerinin listesi olabilir. İşaretler, çözüm tümleştirmeler sunucudan ilgili bazı bilgiler olduğunu gösterir. Rozet tıkladığınızda sağ bölmede durumuna ayrıntıları için doğrudan yönlendirilirsiniz. Şu anda durumu rozetlerinin uyarılar, hizmet Masası, değişiklikler, güvenlik ve güncelleştirmeleri içerir.

Durumu rozetlerinin önem derecesine bağlı olarak, makine düğümünü Kenarlıklar renkli kırmızı (kritik), sarı (uyarı) olması veya mavi (bilgilendirme). Renk herhangi bir durumu rozetlerinin en önemlisi durumunu temsil eder. Bir gri kenarlık hiç durum göstergesi olan bir düğüm gösterir.

![Durumu rozetlerinin](media/service-map/status-badges.png)

## <a name="process-groups"></a>İşlem gruplarının
İşlem gruplarının bir işlem gruba bir ortak ürün veya hizmeti ile ilişkili işlemler birleştirin.  Makine bir düğüm genişletildiğinde işlem gruplarının yanı sıra tek başına işlemleri görüntüler.  Herhangi bir işlem içinde bir işlem grubu gelen ve giden bağlantı başarısız oldu, ardından bağlantı için tüm işlem grubu başarısız olarak gösterilir.

## <a name="machine-groups"></a>Makine Grupları
Makine grupları sunucular, yalnızca bir tane değil bir eşlem içindeki bir çok katmanlı uygulaması veya sunucu kümesinin tüm üyeleri görebilmeniz için bir dizi etrafında ortalanmış haritalar görmenize olanak sağlar.

Kullanıcılar hangi sunucularla birlikte bir gruba ait ve grup için bir ad seçin'ı seçin.  Ardından, tüm işlemler ve bağlantıları grubuyla veya yalnızca işlemler ve doğrudan grubun diğer üyeleri arasında bir ilişki bağlantılarını görüntüleyin seçebilirsiniz.

![Makine grubu](media/service-map/machine-group.png)

### <a name="creating-a-machine-group"></a>Makine grubu oluşturma
Bir grup oluşturmak için makine veya makinelerin listesinde ve tıklayın istediğiniz makineleri seçin **gruba Ekle**.

![Grup Oluşturma](media/service-map/machine-groups-create.png)

Burada, seçebileceğiniz **Yeni Oluştur** ve grubun bir ad verin.

![Ad Grup](media/service-map/machine-groups-name.png)

>[!NOTE]
>Makine grupları 10 sunucularına sınırlıdır.

### <a name="viewing-a-group"></a>Grup görüntüleme
Bazı grupları oluşturduktan sonra grupları sekmesini seçerek görüntüleyebilirsiniz.

![Gruplar sekmesinde](media/service-map/machine-groups-tab.png)

Ardından bu makine grubu için haritada görüntülemek için Grup adını seçin.
![Makine grubu](media/service-map/machine-group.png) grubuna ait makineleri harita beyaz özetlenen.

Grup genişletme makine grubu oluşturan makineleri listeler.

![Makine grubu makineler](media/service-map/machine-groups-machines.png)

### <a name="filter-by-processes"></a>İşlemlere göre filtreleme
Grup ve makine grubu için doğrudan ilgili olanları tüm işlemleri ve bağlantıları gösteren arasında eşleme görünüme geçiş yapabilirsiniz.  Varsayılan görünüm, tüm işlemler göstermektir.  Haritanın üstünde filtre simgesine tıklayarak görünümü değiştirebilirsiniz.

![Filtre grubu](media/service-map/machine-groups-filter.png)

Zaman **tüm işlemler** olan seçili haritanın tüm işlemleri ve bağlantıları her makine grubunda içerecektir.

![Makine grubu tüm işler](media/service-map/machine-groups-all.png)

Yalnızca gösterecek şekilde görünümü değiştirirseniz **Grup bağlantılı işlemler**, eşleme yalnızca bu işlemleri ve Basitleştirilmiş bir görünümü oluşturma, gruptaki diğer makinelere doğrudan bağlı bağlantılar daraltabilir.

![Makine grubu işlemleri filtrelendi](media/service-map/machine-groups-filtered.png)
 
### <a name="adding-machines-to-a-group"></a>Gruba makine ekleme
Varolan bir gruba makine eklemek için'a tıklayın ve istediğiniz makineleri yanındaki kutuları işaretleyin **gruba Ekle**.  Ardından makineleri eklemek istediğiniz grubu seçin.
 
### <a name="removing-machines-from-a-group"></a>Makineleri gruptan kaldırma
Grupları listesinde makine grubundaki makineler listelemek için Grup adını genişletin.  Sonra çıkarmak istediğiniz makineyi yanındaki üç nokta menüsünü tıklatın **Kaldır**.

![Makine grubundan Kaldır](media/service-map/machine-groups-remove.png)

### <a name="removing-or-renaming-a-group"></a>Bir grubu yeniden adlandırma ya da kaldırma
Grup listesinde grup adının yanındaki üç nokta menüsünü tıklatın.

![Makine grubu menüsü](media/service-map/machine-groups-menu.png)


## <a name="role-icons"></a>Rol simgeleri
Bazı işlemleri belirli rolleri makinelerde hizmet: web sunucuları, uygulama sunucuları, veritabanı ve benzeri. Hizmet eşlemesi, işlem ve makine kutuları bir işlem rol veya sunucu uyumlu bir bakışta belirlemenize yardımcı olması için rol simgelerle açıklama ekler.

| Rol simgesi | Açıklama |
|:--|:--|
| ![Web sunucusu](media/service-map/role-web-server.png) | Web sunucusu |
| ![Uygulama sunucusu](media/service-map/role-application-server.png) | Uygulama sunucusu |
| ![Veritabanı sunucusu](media/service-map/role-database.png) | Veritabanı sunucusu |
| ![LDAP sunucusu](media/service-map/role-ldap.png) | LDAP sunucusu |
| ![SMB sunucusu](media/service-map/role-smb.png) | SMB sunucusu |

![Rol simgeleri](media/service-map/role-icons.png)


## <a name="failed-connections"></a>Başarısız bağlantılar
Başarısız bağlantılar gösterilir işlemleri ve bilgisayarlar, hizmet eşlemesi Maps ile bir işlem veya bağlantı noktası ulaşmak bir istemci sistem başarısız olduğunu gösteren bir kesikli kırmızı çizgi. Sistemin başarısız bağlantı denemesi ise başarısız bağlantılar dağıtılan bir hizmet eşlemesi Aracısı ile herhangi bir sistemden raporlanır. Hizmet eşlemesi, bu işlem bir bağlantı kurmak için başarısız TCP yuvaları gözlemleyerek ölçer. Bu hata, bir güvenlik duvarı, istemci veya sunucu veya kullanılamayan bir uzak hizmeti hatalı yapılandırılması kadar neden olabilir.

![Başarısız bağlantılar](media/service-map/failed-connections.png)

Başarısız bağlantılar sorun giderme ile geçiş doğrulama, güvenlik analizi ve genel mimari anlama yardımcı anlama. Başarısız bağlantılar bazen zararsız, ancak bunlar genellikle doğrudan aniden ulaşılamaz hale bir yük devretme ortam veya Bulut geçişten sonra konuşmaya çözememesi iki uygulama katmanları gibi bir sorun üzerine gelin.

## <a name="client-groups"></a>İstemci Grupları
İstemci grupları bağımlılık aracınız yok istemci makineler temsil eden harita üzerinde kutularıdır. Tek bir istemci grubundaki istemcileri için bir tek bir işlem veya makine temsil eder.

![İstemci Grupları](media/service-map/client-groups.png)

Bir istemci grubundaki sunucuların IP adreslerini görmek için grubu seçin. Grup içeriğini listelenen **istemci grubu özellikleri** bölmesi.

![İstemci grubu özellikleri](media/service-map/client-group-properties.png)

## <a name="server-port-groups"></a>Sunucu bağlantı noktası grupları
Sunucu bağlantı noktası grupları bağımlılık Aracısı olmayan sunucular sunucu bağlantı noktalarını temsil eden kutularıdır. Kutuya sunucu bağlantı noktası ve bağlantıları Bu bağlantı noktasına sunucularıyla sayısını içerir. Kutunun tek tek sunucuların ve bağlantıları görmek için genişletin. Kutuya yalnızca bir sunucu varsa, ad veya IP adresi listelenir.

![Sunucu bağlantı noktası grupları](media/service-map/server-port-groups.png)

## <a name="context-menu"></a>Bağlam menüsü
Üst üç nokta (…) tıklatarak sağında herhangi bir sunucu bu sunucu için bağlam menüsünü görüntüler.

![Başarısız bağlantılar](media/service-map/context-menu.png)

### <a name="load-server-map"></a>Sunucu haritasını yükle
Tıklayarak **sunucu haritasını Yükle** yeni harita seçilen sunucusu olarak yeni odak makine götürür.

### <a name="show-self-links"></a>Kendine bağlantıları göster
Tıklayarak **Göster Self-Links** , başlangıç ve bitiş süreçlere Sunucusu'nda TCP bağlantıları dahil sunucu düğümü kendine bağlantılar, yuva. Kendine bağlantılar gösterilir; Bu, menü komutu değişiklikler **Gizle Self-Links**, böylece bunları kapatabilirsiniz.

## <a name="computer-summary"></a>Bilgisayar özeti
**Makine özeti** bölmesinde, sunucu işletim sistemi, bağımlılık sayıları ve diğer çözümleri verileri bir genel bakış içerir. Bu tür veriler, performans ölçümleri, hizmet Masası biletleri, değişiklik izleme, güvenlik ve güncelleştirmeleri içerir.

![Makine Özet bölmesi](media/service-map/machine-summary.png)

## <a name="computer-and-process-properties"></a>Bilgisayar ve işlem özellikleri
Hizmet eşlemesi harita gittiğinizde, makineleri ve özellikleri hakkında ek bağlam sağlamak için süreçleri seçebilirsiniz. Makine DNS adı, IPv4 adresleri, CPU ve bellek kapasitesi, VM türü, işletim sistemi ve sürümü, son yeniden başlatma süresi ve OMS ve hizmet eşlemesi aracılarının kimlikleri hakkında bilgi sağlar.

![Makine özellikleri bölmesi](media/service-map/machine-properties.png)

Çalışan işlemleri, işlem adı, işlem açıklaması, kullanıcı adı ve etki alanı (Windows üzerinde) şirket adı, ürün adı, ürün sürümü, çalışma dizini, komut satırı ve işlem de dahil olmak üzere ilgili işletim sistemi meta verilerindeki işlem ayrıntılarını toplama Başlangıç zamanı.

![İşlem özellikleri bölmesi](media/service-map/process-properties.png)

**İşlem özeti** bölmesinde işlemin bağlantı, onun bağlı bağlantı noktaları, gelen ve giden bağlantılar dahil olmak üzere hakkında ek bilgi sağlar ve bağlantı başarısız oldu.

![İşlem özeti bölmesi](media/service-map/process-summary.png)

## <a name="alerts-integration"></a>Uyarılar tümleştirme
Hizmet eşlemesi, Azure Uyarıları'nın seçili zaman aralığında sonra seçtiğiniz sunucu için tetiklenen uyarılar gösterilecek ile entegre olur. Geçerli uyarıları varsa bir simge sunucu görüntüler ve **makine uyarılar** bölmesi uyarılarını listeler.

![Uyarılar bölmesinde makine](media/service-map/machine-alerts.png)

Hizmet eşlemesi, ilgili uyarıları görüntülemek etkinleştirmek için belirli bir bilgisayar için tetiklenen uyarı kuralı oluşturun. Uygun uyarılar oluşturmak için:
- Bilgisayar tarafından bir yan tümce grubuna ekleyin (örneğin, **bilgisayar aralığı 1 dakika**).
- Uyarı için ölçüm ölçüsü üzerinde temel'ı seçin.

## <a name="log-events-integration"></a>Günlük olayları tümleştirmesi
Hizmet eşlemesi, seçilen zaman aralığında sonra seçtiğiniz sunucu için tüm kullanılabilir günlük olayların sayısını göstermek için günlük araması ile entegre olur. Listedeki herhangi bir satıra günlük araması atlamak ve tek tek günlüğü olaylarını görmek için olay sayısı, tıklayabilirsiniz.

![Makine günlüğü olaylarını bölmesi](media/service-map/log-events.png)

## <a name="service-desk-integration"></a>Hizmet Masası tümleştirme
Her iki çözüm de etkin ve Log Analytics çalışma alanınızda yapılandırılmış hizmet eşlemesi tümleştirme BT Hizmet Yönetimi Bağlayıcısı ile otomatik olarak gerçekleşir. Hizmet eşlemesi tümleştirmesi "Hizmet Masası" olarak etiketlenmiş Daha fazla bilgi için [BT Hizmet Yönetimi Bağlayıcısı'nı kullanarak ITSM iş öğeleri merkezi olarak yönetin](https://docs.microsoft.com/azure/log-analytics/log-analytics-itsmc-overview).

**Makine hizmet Masası** bölmesi seçili zaman aralığındaki seçili sunucu için tüm BT hizmet yönetimi olaylarını listeler. Geçerli öğe ve bunları makine hizmet Masası bölmesinde listeler sunucunun bir simge görüntüler.

![Makine hizmet Masası bölmesi](media/service-map/service-desk.png)

Bağlı ITSM çözümünüzde öğesi'ni açmak için **iş öğesini görüntüle**.

Günlük aramasında öğenin ayrıntılarını görüntülemek için tıklayın **günlük aramasında Göster**.
Log analytics'te iki yeni tablolar bağlantı ölçümü yazılır 

## <a name="change-tracking-integration"></a>İzleme tümleştirme değiştirme
Her iki çözüm de etkin ve Log Analytics çalışma alanınızda yapılandırılan değişiklik izleme ile hizmet eşlemesi tümleştirme otomatik olarak gerçekleşir.

**Makine değişiklik izleme** bölmesi ek ayrıntılar için günlük araması kadar detaya gitmek için bir bağlantı yanı sıra en son ilk tüm değişiklikleri listeler.

![Makine değişiklik izleme bölmesinde](media/service-map/change-tracking.png)

Aşağıdaki görüntüde görebileceğiniz bir ConfigurationChange olayının ayrıntılı bir görünüm olduğundan seçtikten sonra **Göster Log Analytics'te**.

![ConfigurationChange olay](media/service-map/configuration-change-event-01.png)

## <a name="performance-integration"></a>Performans tümleştirme
**Makine performansını** bölmesi seçilen sunucu için standart performans metrikleri görüntüler. Ölçümler, CPU kullanımı, bellek kullanımı, gönderilen ve alınan ağ bayt ve üst işlemler listesi tarafından gönderilen ve alınan ağ bayt içerir.

![Makine performans bölmesi](media/service-map/machine-performance.png)

Performans verilerini görmek için gerekebilir [uygun Log Analytics performans sayaçlarını etkinleştirmek](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-performance-counters).  Etkinleştirmek istediğiniz sayaçları:

Windows:
- Ýþlemci(*)\\% işlemci zamanı
- Bellek\\% kullanılan Kaydedilmiş Bayt yüzdesi
- Adapter(*) ağ\\gönderilen bayt/sn
- Adapter(*) ağ\\alınan bayt/sn

Linux:
- Ýþlemci(*)\\% işlemci zamanı
- Memory(*)\\% kullanılan bellek
- Adapter(*) ağ\\gönderilen bayt/sn
- Adapter(*) ağ\\alınan bayt/sn

Ağ performansı verileri almak için ayrıca kablo Data 2.0 çözümünü çalışma alanınızda etkinleştirmiş olmanız gerekir.
 
## <a name="security-integration"></a>Güvenlik tümleştirmesi
Her iki çözüm de etkin ve Log Analytics çalışma alanınızda yapılandırılmış güvenlik ve Denetim ile hizmet eşlemesi tümleştirme otomatik olarak gerçekleşir.

**Makine güvenliği** bölmesinde seçtiğiniz sunucu için güvenlik ve denetim çözümü verileri gösterir. Bölmesi, seçilen zaman aralığında sunucu için bekleyen güvenlik sorunları özetini listeler. Tüm güvenlik sorunları tatbikatları aşağı bunları hakkındaki ayrıntılar için günlük araması ne tıkladığınızda.

![Makine güvenlik bölmesi](media/service-map/machine-security.png)

## <a name="updates-integration"></a>Güncelleştirmeleri tümleştirme
Güncelleştirme yönetimi ile tümleştirme hizmet eşlemesi, her iki çözüm de etkin ve Log Analytics çalışma alanınızda yapılandırılmış otomatiktir.

**Makine güncelleştirmeleri** bölmesinde seçtiğiniz sunucu için güncelleştirme yönetimi çözümünü verileri görüntüler. Bölmesi, seçilen zaman aralığında sunucusu için eksik güncelleştirmeleri özetini listeler.

![Makine değişiklik izleme bölmesinde](media/service-map/machine-updates.png)

## <a name="log-analytics-records"></a>Log Analytics kayıtları
Hizmet eşlemesi bilgisayar ve envanter verileri için kullanılabilir [arama](../../azure-monitor/log-query/log-query-overview.md) Log analytics'te. Geçiş planlaması kapasite analizi, bulma ve isteğe bağlı performans sorunlarını giderme senaryoları için bu verileri uygulayabilirsiniz.

Her bir benzersiz bilgisayar ve işlem, bir işlem veya bilgisayar başlatıldığında veya hizmet eşlemesi eklendiğinden, oluşturulan kayıtlarına ek olarak saat başına tek bir kayıt oluşturulur. Bu kayıtları aşağıdaki tablolarda özelliklere sahiptir. Alanları ve değerleri ServiceMapComputer_CL olaylar ServiceMap Azure Resource Manager API'si makine kaynak alanları eşleyin. Alanları ve değerleri ServiceMapProcess_CL olaylar ServiceMap Azure Resource Manager API'si işlem kaynak alanlarını eşleyin. ResourceName_s alanın, karşılık gelen Resource Manager kaynak adı alanında eşleşir. 

>[!NOTE]
>Hizmet eşlemesi özellikleri büyüdükçe, bu alanlar değişikliğe tabi olduğu.

Benzersiz işlemleri ve bilgisayarları tanımlamak için kullanabileceğiniz dahili olarak oluşturulan özellikler vardır:

- Bilgisayar: Kullanım *ResourceId* veya *ResourceName_s* Log Analytics çalışma alanındaki bir bilgisayarı benzersiz olarak tanımlanabilmesi için.
- İşlem: Kullanım *ResourceId* bir işlem bir Log Analytics çalışma alanı içinde benzersiz olarak tanımlanabilmesi için. *ResourceName_s* işlemi çalıştığı (MachineResourceName_s) makine bağlamında benzersizdir 

Sorgular, birden fazla kayıt aynı bilgisayarda veya işlem için belirtilen işlem ve belirtilen zaman aralığında bilgisayar için birden çok kayıt var olabileceğinden döndürebilir. Yalnızca en son kayıt eklemek için Ekle "| Yinelenenleri kaldırma ResourceId"sorgulanamıyor.

### <a name="connections"></a>Bağlantılar
Bağlantı ölçümü, yeni bir Log Analytics - VMConnection tabloya yazılır. Bu tablo, bir makine (gelen ve giden) için bağlantıları hakkında bilgi sağlar. Bağlantı ölçümü, ayrıca bir zaman penceresi boyunca belirli bir ölçüyü elde bulunmasını sağlayan API'ler ile sunulur.  TCP bağlantıları kaynaklanan "*kabul*dinleme yuva ing - tarafından oluşturulanlar çalışırken gelen *bağlanmak*ing - belirli bir IP ve bağlantı noktası için giden. Bir bağlantı yönünü olarak ayarlanabilir Direction özelliği tarafından temsil edilen **gelen** veya **giden**. 

Bu tablolarındaki kayıtlara bağımlılık aracısı tarafından bildirilen verilerden oluşturulur. Tüm kayıtların bir dakikalık zaman aralığında gözlemi temsil eder. TimeGenerated özelliği zaman aralığını başlangıcını gösterir. Her kayıt, diğer bir deyişle ilgili varlığı tanımlayan bilgiler, bağlantı veya bağlantı noktası yanı sıra söz konusu varlıkla ilişkili ölçümleri içerir. Şu anda, TCP IPv4 kullanarak oluşan ağ etkinliği bildirilir.

Maliyetini ve karmaşıklığını yönetmek için tek bir fiziksel ağ bağlantıları bağlantısı kayıtlarını göstermez. Birden fazla fiziksel ağ bağlantıları, ardından ilgili tablodaki yansıtılır mantıksal bir bağlantı içinde gruplandırılır.  Yani kayıt içinde *VMConnection* mantıksal bir gruplandırmasını ve uyulması gereken değil ayrı ayrı fiziksel bağlantılar tablosunu temsil eder. Aşağıdaki öznitelikler için aynı değeri belirli bir dakikalık bir zaman aralığı boyunca, paylaşımı fiziksel ağ bağlantısı toplu tek bir mantıksal kayıt içine *VMConnection*. 

| Özellik | Açıklama |
|:--|:--|
| `Direction` |Yön bağlantının değerdir *gelen* veya *giden* |
| `Machine` |FQDN bilgisayar |
| `Process` |İşlem ya da işlemleri, bağlantıyı başlatan/kabul grupları kimliği |
| `SourceIp` |Kaynak IP adresi |
| `DestinationIp` |Hedef IP adresi |
| `DestinationPort` |Hedef bağlantı noktası numarası |
| `Protocol` |Bağlantı için kullanılan protokol.  Değerler *tcp*. |

Gruplandırma etkisini için hesap için kaydın şu özelliklerde gruplanmış bir fiziksel bağlantı sayısı hakkında bilgi sağlanır:

| Özellik | Açıklama |
|:--|:--|
| `LinksEstablished` |Raporlama zaman penceresi boyunca kurulmuş fiziksel ağ bağlantısı sayısı |
| `LinksTerminated` |Raporlama zaman penceresi boyunca sonlandırıldı fiziksel ağ bağlantısı sayısı |
| `LinksFailed` |Raporlama zaman penceresi sırasında başarısız olan fiziksel ağ bağlantılarının sayısı. Bu bilgiler, şu anda yalnızca giden bağlantıları için kullanılabilir. |
| `LinksLive` |Raporlama zaman penceresi sonunda açık olan fiziksel ağ bağlantısı sayısı|

#### <a name="metrics"></a>Ölçümler

Bağlantı sayısı ölçümü yanı sıra alınıp verilen bir mantıksal bağlantı üzerinde gönderilen veri hacmi hakkında bilgi veya ağ bağlantı noktası kaydın şu özelliklerde de dahil edilir:

| Özellik | Açıklama |
|:--|:--|
| `BytesSent` |Raporlama zaman penceresi boyunca gönderilen bayt sayısı |
| `BytesReceived` |Raporlama zaman penceresi boyunca alınan bayt sayısı |
| `Responses` |Raporlama zaman penceresi boyunca gözlemlenen yanıtlarının sayısı. 
| `ResponseTimeMax` |Raporlama zaman penceresi boyunca gözlemlenen en büyük yanıt süresi (milisaniye).  Değer, boş bir özelliktir.|
| `ResponseTimeMin` |Raporlama zaman penceresi boyunca gözlemlenen en küçük yanıt süresi (milisaniye).  Değer, boş bir özelliktir.|
| `ResponseTimeSum` |Tüm yanıt süreleri toplamı gözlemlenen Raporlama zaman penceresi boyunca (milisaniye).  Değer, özellik boştur|

Yanıt süresi üçüncü bildirilen veri türü, - ne kadar süreyle çağıran işlenmesi ve uzak uç tarafından yanıt için bir bağlantı üzerinden gönderilen bir istek için bekleniyor harcama. Bildirilen yanıt süresi, temel alınan uygulama protokolü doğru yanıt süresinin bir tahmindir. İlk olarak bir fiziksel ağ bağlantısının kaynak ve hedef sonu arasındaki veri akışını göre buluşsal yöntemler kullanılarak hesaplanır. Kavramsal olarak, bu son bayt bir isteği gönderen çıkışında ve yanıtın son baytını geri geldiğinde saat arasındaki farktır. Bu iki zaman damgaları, belirli bir fiziksel bağlantı istek ve yanıt olaylarına ayırmak için kullanılır. Aralarındaki fark, tek bir istek yanıt süresini temsil eder. 

Bu ilk sürümünde bu özellik, bizim başarı belirli ağ bağlantı için kullanılan gerçek uygulama protokolü bağlı olarak değişen dereceli çalışabilir bir yaklaştırma algoritmasıdır. Örneğin, gibi HTTP (S), istek-yanıt tabanlı protokoller için de geçerli yaklaşımın çalışır ancak tek yönlü çalışmak veya kuyruk tabanlı protokoller iletisi.

Dikkate alınması gereken bazı önemli noktalar şunlardır:

1. Her arabirim için ayrı bir kayıt işlemi aynı IP adresinde ancak birden çok ağ arabirimi üzerinden bağlantı kabul ederse bildirilir. 
2. Joker karakter IP kayıtlarla hiçbir etkinlik içerir. Bunlar, makinede bir bağlantı noktası açık gelen trafiği olduğunu göstermek için dahil edilir.
3. Eşleşen bir kayıt (için aynı işlem, bağlantı noktası ve protokol) olduğunda ayrıntı ve veri hacmini azaltmak için joker karakter IP kayıtlarla belirli bir IP adresi ile atlanacak. Bir joker karakter IP kaydı atlandığında, belirli bir IP adresi IsWildcardBind kayıt özelliğiyle ayarlanır "True" bağlantı noktası raporlama makinenin her bir arabirim üzerinden kullanıma sunulduğunu belirtir.
4. Yalnızca belirli arabirime bağlı bağlantı noktaları, "False" olarak IsWildcardBind vardır.

#### <a name="naming-and-classification"></a>Adlandırma ve sınıflandırma
Kolaylık olması için bir bağlantı uzak bitiş IP adresi RemoteIp özelliği eklenmiştir. Gelen bağlantılar için RemoteIp aynıdır Sourceıp giden bağlantılara karşın, bu DestinationIp aynıdır. RemoteDnsCanonicalNames özelliği RemoteIp için makine tarafından bildirilen DNS kurallı adlarını temsil eder. RemoteDnsQuestions ve RemoteClassification özellikler gelecekte kullanılmak üzere ayrılmıştır. 

#### <a name="geolocation"></a>Coğrafi Konum
*VMConnection* coğrafi konum bilgilerini ve uzak uç her bağlantı kaydın kaydın şu özelliklerde de içerir: 

| Özellik | Açıklama |
|:--|:--|
| `RemoteCountry` |RemoteIp barındırma ülke adı.  Örneğin, *Amerika Birleşik Devletleri* |
| `RemoteLatitude` |Coğrafi konum enlem.  Örneğin, *47.68* |
| `RemoteLongitude` |Coğrafi konum boylam.  Örneğin, *-122.12* |

#### <a name="malicious-ip"></a>Kötü amaçlı IP
Her RemoteIp özelliğinde *VMConnection* tablo bilinen kötü amaçlı etkinliği ile bir dizi IP'ler karşı denetlenir. Aşağıdaki özellikler RemoteIp kötü amaçlı olarak tanımlanması durumunda doldurulur (IP kötü amaçlı olarak kabul edilmez, boş oldukları) kaydın aşağıdaki özellikleri:

| Özellik | Açıklama |
|:--|:--|
| `MaliciousIp` |Uzak IP adresi |
| `IndicatorThreadType` |Algılanan tehdit göstergesidir şu değerlerden birini *Botnet*, *C2*, *CryptoMining*, *Darknet*, *DDos* , *MaliciousUrl*, *kötü amaçlı yazılım*, *kimlik avı*, *Proxy*, *PUA*, *İzleme*.   |
| `Description` |Gözlemlenen tehdit açıklaması. |
| `TLPLevel` |Trafik ışığı Protokolü (TLP) düzeyi tanımlanmış değerlerden biridir *beyaz*, *yeşil*, *Amber*, *kırmızı*. |
| `Confidence` |Değerler *0-100*. |
| `Severity` |Değerler *0 – 5*burada *5* en ciddi ve *0* hiç önemli değil. Varsayılan değer *3*.  |
| `FirstReportedDateTime` |İlk kez sağlayıcısı göstergesi bildirdi. |
| `LastReportedDateTime` |Son zaman göstergesi tarafından Interflow görüldü. |
| `IsActive` |Göstergeleri ile devre dışı gösteren *True* veya *False* değeri. |
| `ReportReferenceLink` |Belirli bir observable için ilgili raporları bağlar. |
| `AdditionalInformation` |Uygunsa, gözlemlenen tehdit hakkında ek bilgi sağlar. |

### <a name="servicemapcomputercl-records"></a>ServiceMapComputer_CL kayıtları
Kayıt türü ile *ServiceMapComputer_CL* Envanter verileri için hizmet eşlemesi Aracısı sunucularıyla sahip. Bu kayıtlar aşağıdaki tabloda özelliklere sahiptir:

| Özellik | Açıklama |
|:--|:--|
| `Type` | *ServiceMapComputer_CL* |
| `SourceSystem` | *OpsManager* |
| `ResourceId` | Çalışma alanı içindeki bir makine için benzersiz tanımlayıcı |
| `ResourceName_s` | Çalışma alanı içindeki bir makine için benzersiz tanımlayıcı |
| `ComputerName_s` | FQDN bilgisayar |
| `Ipv4Addresses_s` | Bir listesi sunucunun IPv4 adresleri |
| `Ipv6Addresses_s` | Bir listesini sunucu IPv6 adresleri |
| `DnsNames_s` | Bir dizi DNS adları |
| `OperatingSystemFamily_s` | Windows veya Linux |
| `OperatingSystemFullName_s` | İşletim sisteminin tam adı  |
| `Bitness_s` | Bit genişliği makinenin (32 bit veya 64-bit)  |
| `PhysicalMemory_d` | MB fiziksel bellek |
| `Cpus_d` | CPU sayısı |
| `CpuSpeed_d` | CPU hızı MHz|
| `VirtualizationState_s` | *Bilinmeyen*, *fiziksel*, *sanal*, *hiper yönetici* |
| `VirtualMachineType_s` | *hyperv*, *vmware*, vb. |
| `VirtualMachineNativeMachineId_g` | VM, hiper yönetici tarafından atanan kimliği |
| `VirtualMachineName_s` | VM adı |
| `BootTime_t` | Önyükleme zamanı |

### <a name="servicemapprocesscl-type-records"></a>ServiceMapProcess_CL türü kayıtları
Kayıt türü ile *ServiceMapProcess_CL* hizmet eşlemesi aracılarıyla sunucularında Envanter verileri için TCP bağlantılı işlemleri sahip. Bu kayıtlar aşağıdaki tabloda özelliklere sahiptir:

| Özellik | Açıklama |
|:--|:--|
| `Type` | *ServiceMapProcess_CL* |
| `SourceSystem` | *OpsManager* |
| `ResourceId` | Çalışma alanı içinde bir işlem için benzersiz tanımlayıcı |
| `ResourceName_s` | Üzerinde çalıştığı makinenin içinde bir işlem için benzersiz tanımlayıcı|
| `MachineResourceName_s` | Makinenin kaynak adı |
| `ExecutableName_s` | İşlem yürütülebilir dosyası adı |
| `StartTime_t` | İşlem havuzu başlangıç saati |
| `FirstPid_d` | İşlem havuzunda ilk PID |
| `Description_s` | İşlem açıklaması |
| `CompanyName_s` | Şirketin adı |
| `InternalName_s` | İç adı |
| `ProductName_s` | Ürün adı |
| `ProductVersion_s` | Ürün sürümü |
| `FileVersion_s` | Dosya sürümü |
| `CommandLine_s` | Komut satırı |
| `ExecutablePath _s` | Yürütülebilir dosya yolu |
| `WorkingDirectory_s` | Çalışma dizini |
| `UserName` | Hesabın altında işlemi yürütülüyor |
| `UserDomain` | Etki alanı altında işlemi yürütülüyor |

## <a name="sample-log-searches"></a>Örnek günlük aramaları

### <a name="list-all-known-machines"></a>Bilinen tüm makinelerin listesi
ServiceMapComputer_CL | Özetleme arg_max(TimeGenerated, *) ResourceId tarafından

### <a name="list-the-physical-memory-capacity-of-all-managed-computers"></a>Tüm yönetilen bilgisayarların fiziksel bellek kapasitesi listeleyin.
ServiceMapComputer_CL | Özetleme arg_max(TimeGenerated, *) ResourceId tarafından | Proje PhysicalMemory_d, ComputerName_s

### <a name="list-computer-name-dns-ip-and-os"></a>Liste bilgisayar adı, DNS, IP ve işletim sistemi.
ServiceMapComputer_CL | Özetleme arg_max(TimeGenerated, *) ResourceId tarafından | Proje ComputerName_s, OperatingSystemFullName_s, DnsNames_s, Ipv4Addresses_s

### <a name="find-all-processes-with-sql-in-the-command-line"></a>Komut satırında "sql" ile tüm işlemler bulun
ServiceMapProcess_CL | Burada CommandLine_s contains_cs "sql" | Özetleme arg_max(TimeGenerated, *) ResourceId tarafından

### <a name="find-a-machine-most-recent-record-by-resource-name"></a>Bir makine (en son kayıt) kaynak adına göre bulma
(ServiceMapComputer_CL) "m-4b9c93f9-bc37-46df-b43c-899ba829e07b" araması | Özetleme arg_max(TimeGenerated, *) ResourceId tarafından

### <a name="find-a-machine-most-recent-record-by-ip-address"></a>(En son kayıt) bir makine IP adresine göre Bul
(ServiceMapComputer_CL) "10.229.243.232" araması | Özetleme arg_max(TimeGenerated, *) ResourceId tarafından

### <a name="list-all-known-processes-on-a-specified-machine"></a>Belirtilen bir makinedeki tüm bilinen işlemlere listesi
ServiceMapProcess_CL | where MachineResourceName_s == "m-559dbcd8-3130-454d-8d1d-f624e57961bc" | summarize arg_max(TimeGenerated, *) by ResourceId

### <a name="list-all-computers-running-sql"></a>SQL çalıştıran tüm bilgisayarları listeleyin
ServiceMapComputer_CL | Burada ResourceName_s içinde (((ServiceMapProcess_CL) arama "\*sql\*" | farklı MachineResourceName_s)) | ayrı ComputerName_s

### <a name="list-all-unique-product-versions-of-curl-in-my-datacenter"></a>Curl my veri merkezinde tüm benzersiz ürün sürümlerini listeleme
ServiceMapProcess_CL | Burada ExecutableName_s "curl" == | ayrı ProductVersion_s

### <a name="create-a-computer-group-of-all-computers-running-centos"></a>CentOS çalıştıran tüm bilgisayarların bir bilgisayar grubu oluşturun
ServiceMapComputer_CL | where OperatingSystemFullName_s contains_cs "CentOS" | distinct ComputerName_s

### <a name="summarize-the-outbound-connections-from-a-group-of-machines"></a>Bir gruptaki makinelerin giden bağlantılar özetleme
```
// the machines of interest
let machines = datatable(m: string) ["m-82412a7a-6a32-45a9-a8d6-538354224a25"];
// map of ip to monitored machine in the environment
let ips=materialize(ServiceMapComputer_CL
| summarize ips=makeset(todynamic(Ipv4Addresses_s)) by MonitoredMachine=ResourceName_s
| mvexpand ips to typeof(string));
// all connections to/from the machines of interest
let out=materialize(VMConnection
| where Machine in (machines)
| summarize arg_max(TimeGenerated, *) by ConnectionId);
// connections to localhost augmented with RemoteMachine
let local=out
| where RemoteIp startswith "127."
| project ConnectionId, Direction, Machine, Process, ProcessName, SourceIp, DestinationIp, DestinationPort, Protocol, RemoteIp, RemoteMachine=Machine;
// connections not to localhost augmented with RemoteMachine
let remote=materialize(out
| where RemoteIp !startswith "127."
| join kind=leftouter (ips) on $left.RemoteIp == $right.ips
| summarize by ConnectionId, Direction, Machine, Process, ProcessName, SourceIp, DestinationIp, DestinationPort, Protocol, RemoteIp, RemoteMachine=MonitoredMachine);
// the remote machines to/from which we have connections
let remoteMachines = remote | summarize by RemoteMachine;
// all augmented connections
(local)
| union (remote)
//Take all outbound records but only inbound records that come from either //unmonitored machines or monitored machines not in the set for which we are computing dependencies.
| where Direction == 'outbound' or (Direction == 'inbound' and RemoteMachine !in (machines))
| summarize by ConnectionId, Direction, Machine, Process, ProcessName, SourceIp, DestinationIp, DestinationPort, Protocol, RemoteIp, RemoteMachine
// identify the remote port
| extend RemotePort=iff(Direction == 'outbound', DestinationPort, 0)
// construct the join key we'll use to find a matching port
| extend JoinKey=strcat_delim(':', RemoteMachine, RemoteIp, RemotePort, Protocol)
// find a matching port
| join kind=leftouter (VMBoundPort 
| where Machine in (remoteMachines) 
| summarize arg_max(TimeGenerated, *) by PortId 
| extend JoinKey=strcat_delim(':', Machine, Ip, Port, Protocol)) on JoinKey
// aggregate the remote information
| summarize Remote=makeset(iff(isempty(RemoteMachine), todynamic('{}'), pack('Machine', RemoteMachine, 'Process', Process1, 'ProcessName', ProcessName1))) by ConnectionId, Direction, Machine, Process, ProcessName, SourceIp, DestinationIp, DestinationPort, Protocol
```

## <a name="rest-api"></a>REST API
Tüm hizmet eşlemesi sunucu, işlem ve bağımlılık verilerde aracılığıyla kullanılabilir [hizmet eşlemesi REST API'si](https://docs.microsoft.com/rest/api/servicemap/).

## <a name="diagnostic-and-usage-data"></a>Tanılama ve kullanım verileri
Microsoft hizmet eşlemesi hizmeti kullanımınız vasıtasıyla kullanım ve performans verilerini otomatik olarak toplar. Microsoft, kalite, güvenlik ve hizmet eşlemesi hizmeti bütünlüğünü geliştirmek için bu verileri kullanır. Doğru ve etkili sorun giderme özellikleri sağlamak için veriler işletim sistemi ve sürümü, IP adresi, DNS adı ve iş istasyonu adı gibi yazılımınızın yapılandırması hakkında bilgiler içerir. Microsoft, ad, adres veya diğer iletişim bilgilerinizi toplamaz.

Veri toplama ve kullanım hakkında daha fazla bilgi için bkz: [Microsoft Online Services gizlilik bildirimi](https://go.microsoft.com/fwlink/?LinkId=512132).


## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinin [günlük aramaları](../../azure-monitor/log-query/log-query-overview.md) hizmet eşlemesi tarafından toplanan verileri almak için Log analytics'te.


## <a name="troubleshooting"></a>Sorun giderme
Bkz: [hizmet eşlemesi yapılandırma belgesinin bölümü sorun giderme]( service-map-configure.md#troubleshooting).


## <a name="feedback"></a>Geri Bildirim
Herhangi bir Geri bildiriminiz bizim için hizmet eşlemesi ya da bu belgeleri konusunda var?  Ziyaret bizim [User Voice sayfa](https://feedback.azure.com/forums/267889-log-analytics/category/184492-service-map), buradan özellikler önerme veya mevcut önerileri oy verin.
