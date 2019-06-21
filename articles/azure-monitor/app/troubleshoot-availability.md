---
title: Azure Application Insights kullanılabilirlik testleri sorunlarını giderme | Microsoft Docs
description: Azure Application Insights Web testlerinde sorun giderme. Web sitesi kullanılamaz duruma gelirse veya yavaş yanıt verirse uyarı alın.
services: application-insights
documentationcenter: ''
author: lgayhardt
manager: carmonm
ms.assetid: 46dc13b4-eb2e-4142-a21c-94a156f760ee
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 06/19/2019
ms.reviewer: sdash
ms.author: lagayhar
ms.openlocfilehash: 87bc87d7d105d581f0143e87044fb0337c0fd7f6
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67305127"
---
# <a name="troubleshooting"></a>Sorun giderme

Bu makalede, kullanılabilirlik izleme kullanırken oluşabilecek genel sorunları gidermenize yardımcı olur.

## <a name="ssltls-errors"></a>SSL/TLS hataları

|Belirti/hata iletisi| Olası nedenler|
|--------|------|
|SSL/TLS güvenli kanal oluşturulamadı  | SSL sürüm. Yalnızca TLS 1.0, 1.1 ve 1.2 desteklenir. **SSLv3'ü desteklenmiyor.**
|TLSv1.2 kayıt katmanı: Uyarı (düzeyi: Önemli açıklaması: Hatalı kayıt MAC)| Bkz: StackExchange iş parçacığı için [daha fazla bilgi](https://security.stackexchange.com/questions/39844/getting-ssl-alert-write-fatal-bad-record-mac-during-openssl-handshake).
|Başarısız olan bir CDN (Content Delivery Network) URL'dir | Bu, yanlış yapılandırma, cdn'de tarafından kaynaklanabilir |  

### <a name="possible-workaround"></a>Olası çözüm

* Sorunu yaşayan URL'leri her zaman için bağımlı kaynaklar varsa, devre dışı bırakmak için önerilir **bağımlı istekleri Ayrıştır** web testi için.

## <a name="test-fails-only-from-certain-locations"></a>Belirli konumlar yalnızca sınama başarısız

|Belirti/hata iletisi| Olası nedenler|
|----|---------|
|Bağlı taraf bir süre sonra düzgün yanıt vermediğinden bağlantı denemesi başarısız oldu.  | Test aracıları belirli konumlara bir güvenlik duvarı tarafından engellenir.|
|    |Belirli IP adreslerinin kullanılabilirliğin (yük dengeleyicileri, coğrafi trafik yöneticileri, Azure Express Route.) gerçekleşiyor 
|    |Azure ExpressRoute kullanıyorsanız, burada Bırakılan paketleri durumlarda senaryo vardır burada [asimetrik yönlendirme gerçekleşir](https://docs.microsoft.com/azure/expressroute/expressroute-asymmetric-routing).|

## <a name="intermittent-test-failure-with-a-protocol-violation-error"></a>Protokol ihlali hatası ile aralıklı test hatası

|Belirti/hata iletisi| Olası nedenler|
|----|---------|
protokol ihlali CR'den sonra LF gelmelidir | Bu durum, hatalı biçimlendirilmiş üst bilgiler algılanan oluşur. Özellikle bazı üst bilgiler CRLF HTTP belirtimini ihlal satırın sonuna belirten kullanmıyor olabilir ve bu nedenle .NET WebRequest düzeyinde doğrulama başarısız olur.
 || Bu aynı zamanda yük Dengeleyiciler veya Cdn'lerden kaynaklanabilir.

> [!NOTE]
> URL, HTTP üst bilgilerinin gevşek doğrulaması tarayıcılarda başarısız olmayabilir. Bu sorunun ayrıntılı bir açıklaması için bu blog gönderisine bakın: http://mehdi.me/a-tale-of-debugging-the-linkedin-api-net-and-http-protocol-violations/  

## <a name="common-troubleshooting-questions"></a>Genel sorun giderme soruları

### <a name="site-looks-okay-but-i-see-test-failures-why-is-application-insights-alerting-me"></a>Site sorunsuz görünüyor ancak görüyorum test hataları? Application Insights bana neden uyarı olduğu?

   * Testinizi sahip **bağımlı istekleri Ayrıştır** etkin mi? Komut dosyaları gibi kaynakları üzerinde katı denetimi sonuçlarını vb. görüntüler. Bu tür hataları tarayıcıda belirgin olmayabilir. Tüm görüntüleri, betikleri, stil sayfalarını ve sayfa tarafından yüklenen diğer dosyaları denetleyin. Herhangi biri başarısız olursa, ana HTML sayfası sorun yüklense bile test başarısız olarak raporlanır. Test tür kaynak hatalarına hassasiyetini ortadan kaldırmak için yalnızca bağımlı istekleri Ayrıştır test yapılandırmasından'seçeneğinin işaretini kaldırın.

   * Geçici ağ sinyalleri vb. kaynaklı gürültü olasılığını azaltmak için etkin deneme yapılandırmasının işaretlendiğinden test hataları için emin olun. Ayrıca, daha fazla konumdan test ve uygunsuz uyarılara neden olan konuma özgü sorunları önlemek için uyarı kuralı eşiğini uygun şekilde yönetin.

   * Herhangi bir kullanılabilirlik deneyiminden kırmızı nokta veya neden biz bildirilen hata ayrıntılarını görmek için tüm kullanılabilirlik hatasından arama Gezgini'ni tıklatın. Bağıntılı sunucu tarafı telemetri (etkinse) yanı sıra test sonucu testin neden başarısız anlamanıza yardımcı olacaktır. Sık karşılaşılan nedenleri geçici bir sorun, ağ veya bağlantı sorunlarıdır.

   * Test zaman aşımı mı? Biz, 2 dakika sonra testleri durdurur. Ping veya çok adımlı bir test 2 dakikadan uzun sürerse, biz hata olarak raporlanır. Kısa süre içinde tamamlayabilmeniz için birden fazla olanlar test parçalamak göz önünde bulundurun.

   * Tüm Konumlar raporu hata ya da yalnızca bazılarını mı? Yalnızca bazı hataları bildirilen ağ/CDN sorunlarından kaynaklanıyor olabilir. Yeniden üzerinde kırmızı noktaya tıklayarak neden konum bir başarısızlık bildirilmedi anlamanıza yardımcı olacaktır.

### <a name="i-did-not-get-an-email-when-the-alert-triggered-or-resolved-or-both"></a>Ben, uyarıyı tetikleyen veya çözümlendiğinde bir e-posta veya her ikisini de almadığını?

E-postanızı doğrudan listelenen veya bildirimleri almak için yapılandırılmış olan bir dağıtım listesi onaylamak için Klasik uyarılar yapılandırmasını denetleyin. Daha sonra ise, dış e-postalar alabilirsiniz onaylamak için dağıtım listesi yapılandırmasını denetleyin. Ayrıca, posta yöneticiniz bu soruna neden yapılandırılmış ilkeleri olup olmadığını kontrol edin.

### <a name="i-did-not-receive-the-webhook-notification"></a>Web kancası bildirim almadı?

Web kancası bildirim alma uygulama kullanılabilir ve Web kancası isteği başarıyla işler emin olun. Bkz: [bu](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log-webhook) daha fazla bilgi için.

### <a name="intermittent-test-failure-with-a-protocol-violation-error"></a>Protokol ihlali hatası ile aralıklı test hatası?

Hata ("protokol ihlali..CR’den sonra LF gelmelidir") sunucu (veya bağımlılıklar) ile ilgili bir sorun olduğunu gösterir. Bu durum, yanıtta hatalı biçimlendirilmiş üst bilgiler ayarlandığında meydana gelir. Yük dengeleyiciler veya CDN'lerden kaynaklanabilir. Özellikle bazı üst bilgiler CRLF HTTP belirtimini ihlal satır sonunu belirtmek ve bu nedenle .NET WebRequest düzeyinde doğrulama başarısız kullanmıyor olabilir. İhlal nokta üstbilgilerini yanıta inceleyin.

> [!NOTE]
> URL, HTTP üst bilgilerinin gevşek doğrulaması tarayıcılarda başarısız olmayabilir. Bu sorunun ayrıntılı bir açıklaması için bu blog gönderisine bakın: http://mehdi.me/a-tale-of-debugging-the-linkedin-api-net-and-http-protocol-violations/  

### <a name="i-dont-see-any-related-server-side-telemetry-to-diagnose-test-failures"></a>Tüm ilgili test hatalarını tanılamak için sunucu tarafı telemetrisi görmüyorum? *

Sunucu tarafı uygulamanız için Application Insights ayarlanmışsa, bunun nedeni [örnekleme](../../azure-monitor/app/sampling.md) işleminin devam ediyor olması olabilir. Farklı kullanılabilirlik sonucu seçin.

### <a name="can-i-call-code-from-my-web-test"></a>Web testimden kod çağırabilir miyim?

Hayır. Test adımları .webtest dosyasında olmalıdır. Bu nedenle, başka web testlerini çağıramaz, döngüleri kullanamazsınız. Ancak yararlı bulabileceğiniz bir dizi eklenti vardır.


### <a name="is-there-a-difference-between-web-tests-and-availability-tests"></a>"Web testleri" ve "kullanılabilirlik testleri" arasında bir fark var mı?

Bu iki terim birbirlerinin yerine kullanılabilir. Kullanılabilirlik testleri, çok adımlı web testlerine ek olarak tek URL ping testlerini de içeren daha genel bir terimdir.

### <a name="id-like-to-use-availability-tests-on-our-internal-server-that-runs-behind-a-firewall"></a>Kullanılabilirlik testlerini, güvenlik duvarının arkasında çalışan kendi dahili sunucumuzda kullanmayı tercih ediyorum.

   İki olası çözümü vardır:

   * Güvenlik duvarınızı, [Web testi aracılarımızın IP adreslerinden](../../azure-monitor/app/ip-addresses.md) gelen isteklere izin verecek şekilde yapılandırın.
   * İç sunucunuzu düzenli olarak test etmek için kendi kodunuzu yazın. Kodu, güvenlik duvarınızın arkasındaki bir test sunucusunda arka plan işlemi olarak çalıştırın. Test işleminiz, temel SDK paketindeki [TrackAvailability()](https://docs.microsoft.com/dotnet/api/microsoft.applicationinsights.telemetryclient.trackavailability) API’sini kullanarak sonuçları Application Insights’a gönderebilir. Bunun için test sunucunuzun Application Insights alım uç noktası ile giden bağlantısının olması gerekir, ancak bu, gelen isteklere izin vermeye göre çok daha küçük bir güvenlik riski oluşturur. Sonuçlar kullanılabilirlik web testi dikey pencerelerinde görünür, ancak Analytics, Search ve Ölçüm Gezgini’nde kullanılabilirlik sonuçları olarak görüntülenir.

### <a name="uploading-a-multi-step-web-test-fails"></a>Çok adımlı web testi yüklenemiyor

Bu durum bazı nedenler:
   * 300 K boyut sınırı vardır.
   * Döngüler desteklenmez.
   * Başka web testlerine başvurular desteklenmez.
   * Veri kaynakları desteklenmez.

### <a name="my-multi-step-test-doesnt-complete"></a>Çok adımlı testim tamamlanmadı

Test başına 100 istek sınırı var. Ayrıca, iki dakikadan uzun çalışırsa test durduruldu.

### <a name="how-can-i-run-a-test-with-client-certificates"></a>İstemci sertifikasıyla testi nasıl çalıştırırım?

Bu şu anda desteklenmiyor.

## <a name="who-receives-the-classic-alert-notifications"></a>Kimin (Klasik) Uyarı bildirimlerini alır?

Bu bölümde, yalnızca klasik uyarılar için geçerlidir ve yalnızca istenen alıcılarınız bildirimlerini aldığından emin olmak için Uyarı bildirimlerini iyileştirmenize yardımcı olur. Arasındaki fark hakkında daha fazla anlamak için [Klasik uyarılar](../platform/alerts-classic.overview.md)ve yeni uyarılar deneyimini başvurduğu [uyarılar genel bakış makalesi](../platform/alerts-overview.md). Yeni uyarılar bildiriminde uyarı denetlemek için kullanım deneyimi [Eylem grupları](../platform/action-groups.md).

* Klasik bir uyarı bildirimlerini belirli alıcılara kullanılmasını öneririz.

* X hatalardan Y konumları dışında ilgili uyarılar için **toplu/grup** yönetici/ortak yönetici rollerine sahip kullanıcılar için onay kutusu seçeneği etkinleştirilirse, gönderir.  Temelde _tüm_ yöneticileri _abonelik_ ilgili bildirimler alacaksınız.

* Kullanılabilirlik ölçümlerini ilgili uyarılar için **toplu/grup** abonelik sahibi, katkıda bulunan veya okuyucu rollerine sahip kullanıcılar için onay kutusu seçeneği etkinleştirilirse, gönderir. Aslında, _tüm_ abonelik Application Insights kaynağına erişimi olan kullanıcılar kapsamındaki ve ilgili bildirimler alacaksınız. 

> [!NOTE]
> Şu anda kullanıyorsanız **toplu/grup** onay kutusu seçeneğini ve devre dışı bırakmak, bu değişikliği geri almak mümkün olmayacaktır.

Yeni uyarı deneyimi/neredeyse gerçek zamanlı uyarılar, rollerine bağlı olarak kullanıcılara bildirmek gerekiyorsa kullanın. İle [Eylem grupları](../platform/action-groups.md), (tek bir seçenek olarak birlikte birleştirilmiş değil) sahip/katkıda bulunan/okuyucu rolüne sahip kullanıcılara e-posta bildirimleri yapılandırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Çok adımlı web testi](availability-multistep.md)
* [URL ping testlerini](monitor-web-app-availability.md)
