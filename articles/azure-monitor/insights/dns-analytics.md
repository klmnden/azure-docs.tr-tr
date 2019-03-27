---
title: Azure İzleyici'de DNS analizi çözümü | Microsoft Docs
description: Ayarlama ve DNS altyapısında güvenlik, performans ve işlem öngörüleri toplamak için Azure İzleyici'de DNS analizi çözümü kullanın.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: f44a40c4-820a-406e-8c40-70bd8dc67ae7
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 03/20/2018
ms.author: magoedte
ms.openlocfilehash: 5eac6110cc021a749b787cd97c3a8872d8476668
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58499803"
---
# <a name="gather-insights-about-your-dns-infrastructure-with-the-dns-analytics-preview-solution"></a>DNS analizi Önizleme çözümü, DNS altyapısıyla ilgili Öngörüler toplayın

![DNS analizi simgesi](./media/dns-analytics/dns-analytics-symbol.png)

Bu makalede, ayarlama ve Azure DNS analizi çözümü, DNS altyapısı güvenlik, performans ve işlemleri hakkında Öngörüler elde etmek Azure İzleyici'de kullanın. açıklar.

DNS analiz etmenize yardımcı olur:

- Kötü amaçlı etki alanı adlarını çözümlemeye çalışan istemcileri belirleyin.
- Eski kaynak kayıtları belirleyin.
- Sık sık sorgulanan etki alanı adlarını ve DNS sık iletişim kuran istemcileri belirleyin.
- DNS sunucularındaki istek yükünü görüntüleme.
- Dinamik DNS kayıt hataları görüntüleyin.

Çözüm, toplar, çözümler ve Windows DNS Analitik ve Denetim günlükleri ve diğer ilgili verileri, DNS sunucularından ilişkilendirir.

## <a name="connected-sources"></a>Bağlı kaynaklar

Aşağıdaki tabloda bu çözüm tarafından desteklenen bağlı kaynaklar açıklanmaktadır:

| **Bağlı kaynak** | **Destek** | **Açıklama** |
| --- | --- | --- |
| [Windows aracıları](../platform/agent-windows.md) | Evet | Çözüm, Windows aracılarından DNS bilgilerini toplar. |
| [Linux aracıları](../learn/quick-collect-linux-computer.md) | Hayır | Çözüm, doğrudan Linux aracılarından DNS bilgi toplamaz. |
| [System Center Operations Manager yönetim grubu](../platform/om-agents.md) | Evet | Çözüm, bağlı Operations Manager yönetim grubundaki aracılardan DNS bilgilerini toplar. Azure İzleyici Operations Manager Aracısı'ndan doğrudan bir bağlantı gerekli değildir. Verileri yönetim grubundan Log Analytics çalışma alanına iletilir. |
| [Azure depolama hesabı](../platform/collect-azure-metrics-logs.md) | Hayır | Azure depolama çözümü tarafından kullanılmaz. |

### <a name="data-collection-details"></a>Veri koleksiyonu ayrıntıları

Çözüm, Log Analytics aracısını yüklendiği DNS sunucularından DNS envanteri ve DNS ilgili olay verilerini toplar. Bu veriler daha sonra Azure İzleyici karşıya ve çözüm panosunda görüntülenen. DNS sunucuları, bölge ve kaynak kayıtları sayısı gibi envanterle ilişkili veri DNS PowerShell cmdlet'lerini çalıştırarak toplanır. Verileri iki günde bir kez güncelleştirilir. İlgili olay verileri neredeyse gerçek zamanlı olarak toplanan [analiz ve Denetim günlükleri](https://technet.microsoft.com/library/dn800669.aspx#enhanc) Gelişmiş DNS günlüğe kaydetme ve tanılama Windows Server 2012 R2 tarafından sağlanan.

## <a name="configuration"></a>Yapılandırma

Çözümü yapılandırmak için aşağıdaki bilgileri kullanın:

- Olmalıdır bir [Windows](../platform/agent-windows.md) veya [Operations Manager](../platform/om-agents.md) aracıyı izlemek istediğiniz her bir DNS sunucusunda.
- DNS analizi çözümü Log Analytics çalışma alanınızdan ekleyebileceğiniz [Azure Marketi](https://aka.ms/dnsanalyticsazuremarketplace). Açıklanan işlemi ayrıca kullanabileceğiniz [Azure İzleyici'yi ekleyin çözüm galeri'sinden](solutions.md).

Çözüm, daha fazla yapılandırma gerek olmadan veri toplamaya başlar. Ancak, veri toplamayı Özelleştir için şu yapılandırmayı kullanabilirsiniz.

### <a name="configure-the-solution"></a>Çözümü yapılandırma

Çözüm panosunda **yapılandırma** DNS analizi yapılandırma sayfasını açın. Yaptığınız değişikliklerin iki tür vardır:

- **Güvenilen etki alanı adları**. Çözüm, tüm arama sorguları işlemez. Bu, etki alanı adı son eklerini bir beyaz liste tutar. Bu beyaz liste etki alanı adı sonekleri eşleşen etki alanı adlarını çözümlemeye arama sorguları, çözüm tarafından işlenmez. Azure İzleyici gönderilen verilerin iyileştirmek için izin verilenler listesinde etki alanı adları işlenmiyor yardımcı olur. Varsayılan Güvenilenler listesinde popüler genel etki alanı adları, www.google.com ve www.facebook.com gibi içerir. Kaydırarak tam varsayılan listesini görüntüleyebilir.

  Arama Öngörüler için görüntülemek istediğiniz herhangi bir etki alanı adı soneki eklemek üzere listeden değiştirebilirsiniz. İçin arama öngörülerini görüntülemek istemediğiniz herhangi bir etki alanı adı sonekini de kaldırabilirsiniz.

- **Sık iletişim kuran istemci eşiği**. Arama istekleri sayısının vurgulanmış için eşiğini aşan DNS istemcileri **DNS istemcileri** dikey penceresi. Varsayılan Eşik 1000'dir. Eşik düzenleyebilirsiniz.

    ![İzin verilenler listesinde etki alanı adları](./media/dns-analytics/dns-config.png)

## <a name="management-packs"></a>Yönetim paketleri

Log Analytics çalışma alanınıza bağlanmak için Microsoft Monitoring Agent'ı kullanıyorsanız, aşağıdaki yönetim paketi yüklenir:

- Microsoft DNS veri toplayıcı akıllı paketi (Microsoft.IntelligencePacks.Dns)

Operations Manager yönetim grubunuzu Log Analytics çalışma alanınıza bağlıysa, bu çözümü eklediğinizde, aşağıdaki yönetim paketlerini Operations Manager'da yüklenir. Gerekli yapılandırma veya bakım bu yönetim paketlerinin yoktur:

- Microsoft DNS veri toplayıcı akıllı paketi (Microsoft.IntelligencePacks.Dns)
- Microsoft System Center Advisor DNS Analizi Yapılandırması (Microsoft.IntelligencePack.Dns.Configuration)

Çözüm yönetim paketlerini güncelleştirme hakkında daha fazla bilgi için bkz. [Operations Manager'ı Log Analytics’e Bağlama](../platform/om-agents.md).

## <a name="use-the-dns-analytics-solution"></a>DNS analizi çözümü kullanın

[!INCLUDE [azure-monitor-solutions-overview-page](../../../includes/azure-monitor-solutions-overview-page.md)]


DNS kutucuğu, verilerin toplandığı DNS sunucularının sayısını içerir. Ayrıca, istemciler tarafından son 24 saatte kötü niyetli alanları çözmek için yapılan isteklerinin sayısı da içerir. Kutucuğa tıkladığınızda, çözüm panosu açılır.

![DNS analizi kutucuğu](./media/dns-analytics/dns-tile.png)

### <a name="solution-dashboard"></a>Çözüm panosu

Çözüm Panosu, çözümün çeşitli özelliklerinin özetlenen bilgileri gösterir. Ayrıca, adli analiz ve tanılama için ayrıntılı görünüm bağlantılarını içerir. Varsayılan olarak, son yedi güne ait veriler gösterilir. Tarih ve saat aralığı kullanarak değiştirebileceğiniz **tarih-saat seçimi denetimi**, aşağıdaki görüntüde gösterildiği gibi:

![Zaman seçim denetimi](./media/dns-analytics/dns-time.png)

Çözüm Panosu aşağıdaki dikey pencereleri gösterilir:

**DNS güvenliği**. Kötü amaçlı etki alanları ile iletişim kurmaya çalıştığınız DNS istemcileri bildirir. Microsoft tehdit bilgileri akışlarından kullanarak DNS analizi istemci kötü amaçlı etki alanlarına erişmeye çalıştığınız IP'ler algılayabilir. Çoğu durumda, kötü amaçlı yazılımdan etkilenen aygıtların "dışarı"komut ve Denetim"merkezi kötü amaçlı etki alanının için kötü amaçlı yazılım etki alanı adını çözümleme olarak arama".

![DNS güvenlik dikey penceresi](./media/dns-analytics/dns-security-blade.png)

Bir istemci IP listesinde tıkladığınızda, günlük araması açılır ve ilgili sorgu arama ayrıntılarını gösterir. Aşağıdaki örnekte, DNS analizi iletişimi ile yapıldığını algılamıştır bir [Ircbot](https://www.microsoft.com/wdsi/threats/malware-encyclopedia-description?Name=Backdoor:Win32/IRCbot):

![Ircbot gösteren günlük arama sonuçları](./media/dns-analytics/ircbot.png)

Bilgileri tanımlamanıza yardımcı olur:

- İstemci iletişimi başlatılan IP.
- Çözümlenen kötü amaçlı IP ile etki alanı adı.
- IP adresleri etki alanı adını çözer.
- Kötü amaçlı IP adresi.
- Sorunun önem derecesi.
- Kötü amaçlı IP kara nedeni.
- Algılama zamanı.

**Sorgulanan etki alanları**. Ortamınızda DNS istemcileri tarafından sorgulanan en sık kullanılan etki alanı adlarını sağlar. Sorgulanan etki alanı adlarının listesini görüntüleyebilirsiniz. Ayrıca belirli bir etki alanı adı günlük araması'nda arama isteği ayrıntılara detaya gidebilirsiniz.

![Etki alanları sorgulanan dikey penceresi](./media/dns-analytics/domains-queried-blade.png)

**DNS istemcileri**. İstemcilerin raporları *eşiği ihlal* için seçilen bir zaman dönemi içindeki sorgularının sayısı. Tüm DNS istemcilerini ve onlar tarafından günlük aramasında yapılan sorguları ayrıntılarını listesini görüntüleyebilirsiniz.

![DNS Clıents dikey](./media/dns-analytics/dns-clients-blade.png)

**Dinamik DNS kayıtları**. Kayıt hataları rapor adı. Tüm kayıt hataları adresi [kaynak kayıtları](https://en.wikipedia.org/wiki/List_of_DNS_record_types) (tür A ve AAAA) istemci kayıt istekleri IP'ler birlikte vurgulanır. Sonra bu bilgileri, aşağıdaki adımları izleyerek kayıt hatası kök nedenini bulmak için de kullanabilirsiniz:

1. İstemci güncelleştirilmeye çalışılırken ad için yetkiliyse bölge bulun.

1. Çözüm bölge Envanter bilgilerini kullanın.

1. Dinamik güncelleştirme bölgesi için etkin olduğunu doğrulayın.

1. Veya bölgenin güvenli dinamik güncelleştirme için yapılandırılmış olup olmadığını denetleyin.

    ![Dinamik DNS kayıtları dikey penceresi](./media/dns-analytics/dynamic-dns-reg-blade.png)

**Ad kaydı istekleri**. Üst kutucuk DNS dinamik güncelleştirme isteklerinin başarılı ve başarısız bir eğilim çizgisi gösterir. Daha düşük kutucuk hataları sayısına göre sıralanmış, DNS sunucularına başarısız DNS güncelleştirme istekleri gönderen üst 10 istemcileri de listeler.

![Ad kaydı istekleri dikey penceresi](./media/dns-analytics/name-reg-req-blade.png)

**Örnek DDI analiz sorguları**. Ham analiz verileri doğrudan getiren en yaygın arama sorgularının listesini içerir.


![Örnek sorgular](./media/dns-analytics/queries.png)

Özel raporlama için kendi sorgularınızı oluşturmak için başlangıç noktası olarak bu sorguları kullanabilirsiniz. Sorguların sonuçlarını görüntüleyen DNS Analytics günlük araması sayfaya bağlantı:

- **DNS sunucuları listesi**. Kendi ilişkili FQDN, etki alanı adı, orman adı ve sunucu IP'leri ile tüm DNS sunucularının bir listesini gösterir.
- **DNS bölgelerinin listesi**. Tüm DNS bölgelerinin ilişkili bölge adı, dinamik güncelleştirme durumu, ad sunucularını ve DNSSEC imzalama durumu ile bir listesini gösterir.
- **Kullanılmayan kaynak kayıtlarını**. Tüm kullanılmayan/eski kaynak kayıtlarının bir listesini gösterir. Bu liste, kaynak kayıt adı, kaynak kaydı türü, ilişkili bir DNS sunucusu, kayıt oluşturma süresi ve bölge adını içerir. Artık kullanımda olan DNS kaynak kayıtları tanımlamak için bu listeyi kullanabilirsiniz. Bu bilgilere dayanarak, DNS sunucularından girişler daha sonra kaldırabilirsiniz.
- **DNS sunucuları sorgu yükü**. DNS sunucularınızda DNS iş yükünün bir perspektif alabilmeniz bilgileri gösterir. Bu bilgiler sunucular için kapasite planlamaya yardımcı olabilir. Gidebilirsiniz **ölçümleri** görünümü değiştirmek için bir grafik görselleştirmesi için sekmesinde. Bu görünüm, DNS yük DNS sunucularınızın nasıl dağıtıldığını anlamanıza yardımcı olur. Bu DNS sorgusu her sunucu için oranı eğilimleri gösterir.

    ![DNS sunucuları sorgu günlük arama sonuçları](./media/dns-analytics/dns-servers-query-load.png)

- **DNS bölgeleri sorgu yükü**. Çözüm tarafından yönetilen DNS sunucularında tüm bölgeleri DNS bölgesi sorgu başına saniye istatistiklerini gösterir. Tıklayın **ölçümleri** sonuçlarını grafik bir görselleştirmeye ayrıntılı kayıtları görünümü değiştirmek için sekmesinde.
- **Yapılandırma olayları**. Tüm DNS yapılandırma değişikliği olayları ve ilişkili iletilerini gösterir. Daha sonra bu olayları olay, olay kimliği, DNS sunucusunun saatini temel alan filtre veya görev Kategorisi'ne. Verileri, belirli DNS sunucularına yönelik olarak belirli zamanlarda yapılan değişiklikleri denetim yardımcı olabilir.
- **DNS analitik günlüğü**. Çözüm tarafından yönetilen DNS sunucularında tüm analitik olaylarını gösterir. Daha sonra bu olayları olay, olay kimliği, DNS sunucusunun saatini temel alan filtre arama sorgu ve sorgu türünü görev Kategorisi'ne yapılan istemci IP adresi. DNS sunucusu analitik olaylarını etkinlik DNS sunucusunda izlemeyi etkinleştirin. Sunucu tarafından gönderilen veya alınan DNS bilgilerini her zaman analitik bir olay günlüğe kaydedilir.

### <a name="search-by-using-dns-analytics-log-search"></a>DNS analizi günlük araması'nı kullanarak arama

Günlük araması sayfasında, bir sorgu oluşturabilirsiniz. Model denetimlerini kullanarak arama sonuçlarınızı filtreleyebilirsiniz. Dönüştürme, filtre ve raporlamak için Gelişmiş sorgu sonuçlarınız üzerinde de oluşturabilirsiniz. Aşağıdaki sorgular kullanarak başlatın:

1. İçinde **arama sorgu kutusu**, türü `DnsEvents` çözüm tarafından yönetilen DNS sunucuları tarafından oluşturulan tüm DNS olayları görüntülemek için. Sonuçları arama sorguları, dinamik kayıtları ve yapılandırma değişiklikleri ile ilgili tüm olaylar için günlük verileri listeleyin.

    ![DnsEvents günlük araması](./media/dns-analytics/log-search-dnsevents.png)  

    a. Arama sorguları için günlük verileri görüntülemek için seçin **LookUpQuery** olarak **alt** sol tarafı denetim Filtresi. Seçili süre için tüm arama sorgu olaylarını listeleyen bir tablo görüntülenir.

    b. Dinamik kayıtları için günlük verileri görüntülemek için seçin **DynamicRegistration** olarak **alt** sol tarafı denetim Filtresi. Seçili süre için tüm dinamik kayıt olaylarını listeleyen bir tablo görüntülenir.

    c. Yapılandırma değişiklikleri için günlük verileri görüntülemek için seçin **ConfigurationChange** olarak **alt** sol tarafı denetim Filtresi. Seçili süre için tüm yapılandırma değişikliği olaylarını listeleyen bir tablo görüntülenir.

1. İçinde **arama sorgu kutusu**, türü `DnsInventory` tüm DNS envanterle ilişkili veri çözümü tarafından yönetilen DNS sunucuları görüntülemek için. Sonuçlar, DNS sunucuları, DNS bölgeleri ve kaynak kayıtları için günlük verileri listeler.

    ![DnsInventory günlük araması](./media/dns-analytics/log-search-dnsinventory.png)

## <a name="feedback"></a>Geri Bildirim

Geri bildirim size iki yolu vardır:

- **UserVoice**. DNS analizi özellikleri üzerinde çalışmak için fikir gönderin. Ziyaret [Log Analytics UserVoice sayfa](https://aka.ms/dnsanalyticsuservoice).
- **Bizim kohort katılın**. Her zaman yeni müşteriler yeni özelliklere erken erişim elde eder ve DNS analizi geliştirmemize yardımcı olmak için sunduğumuz kohortlar katılın etmeyle ilgilenen duyuyoruz. Bizim kohortlar katılımının ilgileniyorsanız doldurun [bu kısa bir ankete](https://aka.ms/dnsanalyticssurvey).

## <a name="next-steps"></a>Sonraki adımlar

[Sorgu günlükleri](../log-query/log-query-overview.md) ayrıntılı DNS günlük kayıtları görüntülemek için.
