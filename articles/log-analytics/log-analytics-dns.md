---
title: Azure günlük analizi DNS Analytics çözümde | Microsoft Docs
description: Ayarlama ve güvenlik, performans ve işlem DNS altyapısı Öngörüler toplamak için günlük analizi DNS analiz çözümü kullanın.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: f44a40c4-820a-406e-8c40-70bd8dc67ae7
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/20/2018
ms.author: magoedte
ms.component: na
ms.openlocfilehash: a6f63fac85008425f473f431ae85d04f62eed667
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37127600"
---
# <a name="gather-insights-about-your-dns-infrastructure-with-the-dns-analytics-preview-solution"></a>DNS altyapınızın DNS Analytics Önizleme çözümü ile ilgili Öngörüler toplayın

![DNS Analytics simgesi](./media/log-analytics-dns/dns-analytics-symbol.png)

Bu makalede ayarlamak ve Azure DNS analiz çözümü DNS altyapısının güvenlik, performans ve işlemleri hakkında fikir toplamak için Azure günlük analizi nasıl kullanılacağını açıklar.

DNS Analytics size yardımcı olur:

- Kötü amaçlı etki alanı adları çözümlemeye istemcileri belirleyin.
- Eski kaynak kayıtları tanımlayın.
- Sık sık sorgulanan etki alanı adları ve talkative DNS istemcileri belirleyin.
- DNS sunucuları üzerindeki görünüm isteği yükü.
- Dinamik DNS kayıt hataları görüntüleyin.

Çözüm toplar, analiz eder ve Windows DNS Analitik ve Denetim günlükleri ve diğer ilgili veriler, DNS sunucularınızın karşılık gelen.

## <a name="connected-sources"></a>Bağlı kaynaklar

Aşağıdaki tabloda bu çözüm tarafından desteklenen bağlı kaynakları açıklanmaktadır:

| **Bağlı kaynak** | **Destek** | **Açıklama** |
| --- | --- | --- |
| [Windows aracıları](log-analytics-windows-agent.md) | Evet | Çözüm Windows aracılardan DNS bilgilerini toplar. |
| [Linux aracıları](log-analytics-linux-agents.md) | Hayır | Çözüm, doğrudan Linux aracılarını DNS bilgilerini toplamaz. |
| [System Center Operations Manager yönetim grubu](log-analytics-om-agents.md) | Evet | Çözüm aracıları bağlı Operations Manager yönetim grubunda DNS bilgilerini toplar. Operations Management Suite Operations Manager aracısı arasında doğrudan bağlantı gerekli değildir. Veri yönetim grubundaki Operations Management Suite depoya iletilir. |
| [Azure depolama hesabı](log-analytics-azure-storage.md) | Hayır | Azure depolama çözümü tarafından kullanılmaz. |

### <a name="data-collection-details"></a>Veri toplama ayrıntıları

Çözüm DNS envanteri ve DNS olay ilgili verileri DNS sunucularından günlük analizi Aracısı yüklendiği toplar. Bu veriler daha sonra günlük analizi için karşıya ve çözüm panosunda görüntülenir. DNS sunucuları, bölge ve kaynak kayıtları, sayısı gibi envanterle ilişkili veri DNS PowerShell cmdlet'lerini çalıştırarak toplanır. Verileri iki günde bir kez güncelleştirilir. Yakın gerçek zamanlı toplanan olay ile ilgili veriler [Analitik ve Denetim günlükleri](https://technet.microsoft.com/library/dn800669.aspx#enhanc) Gelişmiş DNS günlüğe kaydetme ve tanılama Windows Server 2012 R2 tarafından sağlanan.

## <a name="configuration"></a>Yapılandırma

Çözümü yapılandırmak için aşağıdaki bilgileri kullanın:

- Bilmeniz gereken bir [Windows](log-analytics-windows-agent.md) veya [Operations Manager](log-analytics-om-agents.md) Aracısı'nı izlemek istediğiniz her DNS sunucusu.
- Operations Management Suite alanınızdan DNS analiz çözümü ekleyebilirsiniz [Azure Marketi](https://aka.ms/dnsanalyticsazuremarketplace). Açıklanan işlemi de kullanabilirsiniz [Çözümleri Galerisi eklemek günlük analizi çözümleri](log-analytics-add-solutions.md).

Çözüm ek yapılandırma gerekmeden veri toplamayı başlatır. Ancak, veri toplama özelleştirmek için aşağıdaki yapılandırma kullanabilirsiniz.

### <a name="configure-the-solution"></a>Çözümü yapılandırma

Çözüm panosunda tıklatın **yapılandırma** DNS Analytics yapılandırma sayfasını açın. Yaptığınız yapılandırma değişikliklerini iki tür vardır:

- **Güvenilenler listesine etki alanı adları**. Çözüm, tüm arama sorguları işlemez. Etki alanı adı soneklerinin beyaz liste tutar. Etki alanı adı soneklerinin bu beyaz liste içinde eşleşen etki alanı adları çözümlemek arama sorguları çözümü tarafından işlenmez. Günlük analizi için gönderilen veri iyileştirmek için Güvenilenler listesine etki alanı adları işlenmiyor yardımcı olur. Varsayılan beyaz liste www.google.com ve www.facebook.com gibi popüler ortak etki alanı adlarını içerir. Kaydırarak tam varsayılan listesini görüntüleyebilirsiniz.

 Arama öngörüleri için görüntülemek istediğiniz herhangi bir etki alanı adı soneki eklemek üzere listeden değiştirebilirsiniz. İçin arama ınsights görüntülemek için istemediğiniz tüm etki alanı adı soneki da kaldırabilirsiniz.

- **Talkative istemci eşiği**. Arama istekleri sayısı içinde vurgulanmıştır için eşiğini aşan DNS istemcilerini **DNS istemcileri** dikey. Varsayılan Eşik 1. 000 ' dir. Eşik düzenleyebilirsiniz.

    ![Güvenilenler listesine etki alanı adları](./media/log-analytics-dns/dns-config.png)

## <a name="management-packs"></a>Yönetim paketleri

Microsoft Monitoring Agent, Operations Management Suite çalışma alanına bağlanmak için kullanıyorsanız, aşağıdaki yönetim paketi yüklenir:

- Microsoft DNS veri toplayıcı zeka Paketi'ni (Microsft.IntelligencePacks.Dns)

Operations Manager yönetim grubu için Operations Management Suite çalışma alanınızı bağlıysa, bu çözüm eklediğinizde, aşağıdaki yönetim paketleri Operations Manager'da yüklenir. Gerekli yapılandırma veya bu yönetim paketlerinin bakım yoktur:

- Microsoft DNS veri toplayıcı zeka Paketi'ni (Microsft.IntelligencePacks.Dns)
- Microsoft System Center Advisor DNS Analizi Yapılandırması (Microsoft.IntelligencePack.Dns.Configuration)

Çözüm yönetim paketlerini güncelleştirme hakkında daha fazla bilgi için bkz. [Operations Manager'ı Log Analytics’e Bağlama](log-analytics-om-agents.md).

## <a name="use-the-dns-analytics-solution"></a>DNS analiz çözümü kullanın

Bu bölümde, tüm Pano açıklanmaktadır işlevleri ve nasıl kullanıldıklarını.

Sunucunuzdan çalışma alanınıza çözümü ekledikten sonra çözüm döşemenin Operations Management Suite'e genel bakış sayfasında DNS altyapınızın kısa bir Özet sağlar. Burada veri toplanırken DNS sunucularının sayısını içerir. Son 24 saatte kötü amaçlı etki alanlarını çözümlemek için istemcileri tarafından yapılan isteklerin sayısı ayrıca içerir. Kutucuğa tıkladığınızda, çözüm panosu açılır.

![DNS Analytics döşeme](./media/log-analytics-dns/dns-tile.png)

### <a name="solution-dashboard"></a>Çözüm panosu

Çözüm Panosu çözümün çeşitli özellikleri için özet bilgilerini gösterir. Ayrıca, adli çözümleme ve tanılama için ayrıntılı görünümü bağlantılarını içerir. Varsayılan olarak, verileri son yedi gün için gösterilir. Tarih ve saat aralığı kullanarak değiştirebilirsiniz **tarih-saat seçim denetimi**aşağıdaki görüntüde gösterildiği gibi:

![Zamanı seçim denetimi](./media/log-analytics-dns/dns-time.png)

Çözüm Panosu aşağıdaki dikey pencereleri gösterilir:

**DNS güvenliği**. Kötü amaçlı etki alanları ile iletişim kurmaya DNS istemcileri bildirir. Microsoft tehdit bilgisi akışlarını kullanarak DNS Analytics istemcisi kötü amaçlı etki alanları erişmeye çalıştığınız IP'leri algılayabilir. Çoğu durumda, kötü amaçlı yazılımdan etkilenen aygıtların "kötü amaçlı etki alanının"komut ve Denetim"merkezi kötü amaçlı yazılım etki alanı adı çözülerek çevirme".

![DNS güvenlik dikey penceresi](./media/log-analytics-dns/dns-security-blade.png)

İstemci IP listesinde tıkladığınızda, günlük arama açar ve ilgili sorgu arama ayrıntılarını gösterir. Aşağıdaki örnekte, DNS Analytics iletişimi ile yapıldığını algıladı bir [Ircbot](https://www.microsoft.com/security/portal/threat/encyclopedia/entry.aspx?Name=Win32/IRCbot):

![Ircbot gösteren bir günlük arama sonuçları](./media/log-analytics-dns/ircbot.png)

Bilgileri belirlemenize yardımcı olabilir:

- İstemci iletişimi başlatan IP.
- Kötü amaçlı IP çözümler etki alanı adı.
- Etki alanı adı çözümler IP adresleri.
- Kötü amaçlı IP adresi.
- Sorunun önem derecesini.
- Kötü amaçlı IP kara liste nedeni.
- Algılama zamanı.

**Sorgulanan etki alanları**. Ortamınızda DNS istemcileri tarafından sorgulanan en sık rastlanan etki alanı adlarını sağlar. Sorgulanan tüm etki alanı adlarının listesini görüntüleyebilirsiniz. Ayrıca aşağı günlük arama belirli etki alanı adının arama istek ayrıntılarını ayrıntılarına geçebilir.

![Etki alanları sorgulanan dikey penceresi](./media/log-analytics-dns/domains-queried-blade.png)

**DNS istemcileri**. İstemcilerin raporları *eşiği ihlal* seçilen zaman aralığı içinde sorgu sayısı. Tüm DNS istemcilerini ve onlar tarafından günlük aramada yapılan sorguları ayrıntılarını listesini görüntüleyebilirsiniz.

![DNS istemcileri dikey penceresi](./media/log-analytics-dns/dns-clients-blade.png)

**Dinamik DNS kayıtları**. Raporları kayıt hataları adı. Adres için tüm kayıt hataları [kaynak kayıtları](https://en.wikipedia.org/wiki/List_of_DNS_record_types) (tür A ve AAAA) kaydı istekleri IP'leri istemci birlikte vurgulanır. Bu bilgiler daha sonra aşağıdaki adımları izleyerek kayıt hatası kök nedenini bulmak için de kullanabilirsiniz:

1. İstemci güncelleştirilmeye çalışılırken ad için yetkiliyse bölge bulun.

2. Bu bölge envanter bilgilerinin denetlemek için çözümü kullanın.

3. Dinamik güncelleştirme bölge için etkin olduğunu doğrulayın.

4. Bölgenin güvenli dinamik güncelleştirme için yapılandırılmış olup olmadığı olup olmadığını denetleyin.

    ![Dinamik DNS kayıtları dikey penceresi](./media/log-analytics-dns/dynamic-dns-reg-blade.png)

**Ad kaydı istekleri**. Üst bölme başarılı ve başarısız DNS dinamik güncelleştirme isteklerinin bir eğilim çizgisi gösterir. Alt bölme hataları sayısına göre sıralanmış DNS sunucuları için güncelleştirme isteklerinin başarısız DNS gönderme üst 10 istemcilerini listeler.

![Ad kaydı istekleri dikey penceresi ](./media/log-analytics-dns/name-reg-req-blade.png)

**Örnek DDI analitik sorguları**. Ham analiz verileri doğrudan fetch en yaygın arama sorguları listesini içerir.


![Örnek sorgular](./media/log-analytics-dns/queries.png)

Özelleştirilmiş raporlama için kendi sorguları oluşturmak için bu sorguları bir başlangıç noktası olarak kullanabilirsiniz. Sorgu sonuçları görüntülendiği DNS Analytics günlük arama sayfasına bağlantı:

- **DNS sunucularının listesini**. Bunların ilişkili FQDN, etki alanı adı, orman adı ve sunucu IP'leri ile tüm DNS sunucularının bir listesini gösterir.
- **DNS bölgelerinin listesi**. Tüm DNS bölgelerinin ilişkili bölge adı, dinamik güncelleştirme durumu, ad sunucuları ve DNSSEC imzalama durumu ile bir listesini gösterir.
- **Kullanılmayan kaynak kayıtları**. Tüm kullanılmayan/eski kaynak kayıtlarının bir listesini gösterir. Bu liste, kaynak kayıt adı, kaynak kaydı türü, ilişkili DNS sunucusu, kayıt oluşturma zamanı ve bölge adını içerir. Artık kullanımda olmayan DNS kaynak kayıtlarını tanımlamak için bu listeyi kullanın. Bu bilgilere dayanarak, ardından girişler DNS sunucularından silebilirsiniz.
- **DNS sunucularını sorgulamak yük**. Böylece, DNS sunucularında DNS yük bir perspektif alabilir bilgileri gösterir. Bu bilgiler sunucular için kapasite planlama yapmanıza yardımcı olabilir. Gidebilirsiniz **ölçümleri** grafik görselleştirme için görünümü değiştirmek için sekmesi. Bu görünüm, DNS yük, DNS sunucularınızın arasında nasıl dağıtıldığını anlamanıza yardımcı olur. Bu DNS sorgusu her sunucu için oranı eğilimlerini gösterir.

    ![DNS sunucuları sorgu günlüğü arama sonuçları](./media/log-analytics-dns/dns-servers-query-load.png)

- **DNS bölgelerini sorgu yük**. Çözümü tarafından yönetilen DNS sunucularındaki tüm bölgeler DNS bölge sorgu başına saniye istatistiklerini gösterir. Tıklatın **ölçümleri** Sonuçları grafik görselleştirme için ayrıntılı kayıtları görünümü değiştirmek için sekmesi.
- **Yapılandırma olayları**. Tüm DNS yapılandırma değişikliği olayları ve ilişkili iletileri gösterir. Daha sonra bu olayları olay, olay kimliği, DNS sunucusu zamanında göre filtre ya da görev kategorisi. Verileri, belirli DNS sunucularına belirli zamanlarda yapılan değişiklikler Denetim yardımcı olabilir.
- **DNS analitik günlük**. Çözümü tarafından yönetilen tüm DNS sunucularına analitik tüm olayları gösterir. Daha sonra bu olayları olay, olay kimliği, DNS sunucusu zamanında göre filtre arama sorgusu ve sorgu türü görev kategorisi yapılan istemci IP'si. DNS sunucusu analitik olaylarını etkinlik DNS sunucusunda izlemeyi etkinleştirin. Sunucu gönderdiğinde veya DNS bilgilerini aldığında her zaman analitik bir olay günlüğe kaydedilir.

### <a name="search-by-using-dns-analytics-log-search"></a>DNS Analytics günlük arama kullanarak arama

Günlük arama sayfasında, bir sorgu oluşturabilirsiniz. Model denetimlerini kullanarak arama sonuçlarınızı filtreleyebilirsiniz. Dönüştürme, filtre ve raporlamak için Gelişmiş sorgular sonuçlarınıza de oluşturabilirsiniz. Aşağıdaki sorgularda kullanarak başlatın:

1. İçinde **arama sorgusu kutusunu**, türü `DnsEvents` çözümü tarafından yönetilen DNS sunucuları tarafından oluşturulan tüm DNS olayları görüntülemek için. Sonuçlar arama sorguları, dinamik kayıtlar ve yapılandırma değişiklikleri ile ilgili tüm olaylar için günlük verileri listeler.

    ![DnsEvents günlük arama](./media/log-analytics-dns/log-search-dnsevents.png)  

    a. Arama sorguları için günlük verileri görüntülemek için seçin **LookUpQuery** olarak **alt** soldaki modeli denetiminden filtre. Seçilen zaman aralığı için tüm arama sorgusu olayları listeleyen bir tablo görüntülenir.

    b. Dinamik kayıtlar için günlük verileri görüntülemek için seçin **DynamicRegistration** olarak **alt** soldaki modeli denetiminden filtre. Seçilen zaman aralığı için tüm dinamik kayıt olayları listeleyen bir tablo görüntülenir.

    c. Yapılandırma değişiklikleri için günlük verileri görüntülemek için seçin **ConfigurationChange** olarak **alt** soldaki modeli denetiminden filtre. Seçilen zaman aralığı için tüm yapılandırma değişikliği olayları listeleyen bir tablo görüntülenir.

2. İçinde **arama sorgusu kutusunu**, türü `DnsInventory` tüm DNS envanterle ilişkili veri çözümü tarafından yönetilen DNS sunucuları görüntülemek için. Sonuçlar DNS sunucuları, DNS bölgeleri ve kaynak kayıtları için günlük verileri listeler.

    ![DnsInventory günlük arama](./media/log-analytics-dns/log-search-dnsinventory.png)

## <a name="feedback"></a>Geri Bildirim

Geri bildirimde iki yolu vardır:

- **UserVoice**. Üzerinde çalışmak üzere DNS Analytics özelliklerinin fikirleri gönderin. Ziyaret [Operations Management Suite UserVoice sayfa](https://aka.ms/dnsanalyticsuservoice).
- **Bizim kohort katılma**. Her zaman yeni özellikler erken erişim edinmek ve DNS Analytics geliştirmemize yardımcı olmak için bizim cohorts katılma yeni müşteriler elde etmeyle ilgilenen çalışıyoruz. Bizim cohorts birleştirme ilgileniyorsanız doldurmak [hızlı anketi](https://aka.ms/dnsanalyticssurvey).

## <a name="next-steps"></a>Sonraki adımlar

[Arama günlüklerini](log-analytics-log-searches.md) ayrıntılı DNS günlük kayıtları görüntülemek için.
