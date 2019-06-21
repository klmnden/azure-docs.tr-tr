---
title: Web sitelerinin kullanılabilirlik ve yanıt hızını izleme | Microsoft Docs
description: Application Insights’ta web testleri ayarlayın. Web sitesi kullanılamaz duruma gelirse veya yavaş yanıt verirse uyarı alın.
services: application-insights
author: mrbullwinkle
manager: carmonm
ms.assetid: 46dc13b4-eb2e-4142-a21c-94a156f760ee
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 06/19/2019
ms.reviewer: sdash
ms.author: mbullwin
ms.openlocfilehash: abe55007aa8a8719d6b6f1659e00a089a2e28771
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67303781"
---
# <a name="monitor-the-availability-of-any-website"></a>Tüm Web sitelerinin kullanılabilirliğini izleyin

Web uygulaması/Web dağıttıktan sonra kullanılabilirlik ve yanıt hızını izlemek için yinelenen testleri ayarlayın. [Azure Application Insights](../../azure-monitor/app/app-insights-overview.md), dünyanın her yerindeki noktalarından uygulamanıza düzenli aralıklarla web istekleri gönderir. Bu, uygulamanızın yanıt vermiyor veya çok yavaş yanıtlarsa uyarabilir.

Genel İnternet'ten erişilebilen herhangi bir HTTP veya HTTPS uç noktası için kullanılabilirlik testleri ayarlayabilirsiniz. Test ettiğiniz Web sitesi için değişiklik gerekmez. Aslında, bu da size ait bir site olması gerekmez. Hizmetinize bağlı olduğu bir REST API kullanılabilirliğini test edebilirsiniz.

### <a name="types-of-availability-tests"></a>Kullanılabilirlik testi türleri:

Üç tür kullanılabilirlik testi vardır:

* [URL ping testi](#create-a-url-ping-test): Azure portalında oluşturabileceğiniz basit bir test.
* [Çok adımlı web testi](availability-multistep.md): Bir kaydı, daha karmaşık senaryoları test etmek için çalınabilecek web istekleri, bir dizi. Çok adımlı web testleri Visual Studio Enterprise'da oluşturulur ve yürütme için portala karşıya yüklendi.
* [Özel İzleme kullanılabilirlik testleri](https://docs.microsoft.com/dotnet/api/microsoft.applicationinsights.telemetryclient.trackavailability?view=azure-dotnet): `TrackAvailability()` Yöntemi, kendi özel kullanılabilirlik testleri oluşturmak için kullanılabilir.

**Application Insights kaynağı başına 100'e kadar kullanılabilirlik testi oluşturabilirsiniz.**

## <a name="create-an-application-insights-resource"></a>Application Insights kaynağı oluşturma

Kullanılabilirlik testi oluşturabilmek için öncelikle bir Application Insights kaynağı oluşturmanız gerekir. Bir kaynak zaten oluşturduysanız, sonraki bölüme geçin [URL Ping testi oluşturma](#create-a-url-ping-test).

Azure portalından seçin **kaynak Oluştur** > **Geliştirici Araçları** > **Application Insights** ve [oluşturma bir Application Insights kaynağı](create-new-resource.md).

## <a name="create-a-url-ping-test"></a>URL ping testi oluşturma

Adı "URL ping testi" bir misnomer biraz ' dir. Gerekirse bu test ICMP (Internet Denetim İletisi sitenizin kullanılabilirliğini denetlemek için Protokolü) kullanımı yapıyor değil. Bunun yerine bir uç nokta yanıt olup olmadığını doğrulamak için daha gelişmiş HTTP isteği işlevlerini kullanır. Ayrıca, Yanıtla ilişkili performans ve bağımlı istekleri ayrıştırma ve yeniden deneme sayısı için izin verme gibi daha gelişmiş özellikler ile birlikte özel başarı ölçütleri ayarlamanıza olanak ekler.

İlk kullanılabilirlik isteğinizi oluşturmak için kullanılabilirlik bölmesini açın ve seçin **oluşturma Test**.

![En azından web sitenizin URL'sini doldurma](./media/monitor-web-app-availability/availability-create-test-001.png)

### <a name="create-a-test"></a>Test oluşturma

|Ayar| Açıklama
|----|----|----|
|**URL** |  Test etmek istediğiniz herhangi bir web sayfasında URL olabilir, ancak ortak internet'ten görünür olmalıdır. URL bir sorgu dizesi içerebilir. Bu nedenle, örneğin, veritabanınızla biraz alıştırma yapabilirsiniz. URL yeniden yönlendirme adresine çözümlenirse, en fazla 10 yeniden yönlendirmeyi izleriz.|
|**Bağımlı istekleri Ayrıştır**| Test görüntüleri, betikleri, Stil dosyaları ve test kapsamındaki web sitesine bir parçası olan diğer dosyaları ister. Kayıtlı yanıt süresi, bu dosyaları almak için geçen süreyi içerir. Şu kaynaklara başarıyla testin tamamının zaman aşımı süresi içinde yüklenemezse test başarısız olur. Seçenek işaretlenmezse, test yalnızca belirttiğiniz URL’deki dosyayı ister. Bu seçenek sonuçları katı denetimi etkinleştirme. Testi el ile site göz atarken belirgin olmayabilir çalışmaları için başarısız olabilir.
|**Yeniden denemeyi etkinleştir**|test başarısız olduğunda kısa bir süre sonra yeniden denenir. Art arda üç deneme başarısız olursa bir hata bildirilir. Sonraki testler bundan sonra her zamanki test sıklığında gerçekleştirilir. Bir sonraki başarılı olana kadar yeniden deneme geçici olarak askıya alınır. Bu kural her test konuma bağımsız olarak uygulanır. **Bu seçenek önerilir**. Ortalama olarak hataların yaklaşık %80’i yeniden deneme sırasında kaybolur.|
|**Sınama sıklığı**| Testin her test konumdan ne sıklıkta çalıştırılacağını ayarlar. Beş dakikalık varsayılan sıklıkta ve beş test konumuyla, siteniz ortalama olarak dakikada bir test edilir.|
|**Test konumları**| Burada sunucularımızın URL'nize web istekleri göndermek gelen yerlerdir. **Önerilen test konumları bizim en düşük sayısı beştir** , sorunları, Web sitenizdeki ağ sorunlarını ayırt edebilirsiniz emin olmak amacıyla. En fazla 16 konum seçebilirsiniz.

**URL'niz ortak internet'ten görünür değilse, seçmeli olarak yalnızca test işlemleri aracılığıyla izin vermek için güvenlik duvarını açmak seçebilirsiniz**. Bizim kullanılabilirlik test aracıları için güvenlik duvarı özel durumları hakkında daha fazla bilgi için danışın [IP adresi Kılavuzu](https://docs.microsoft.com/azure/azure-monitor/app/ip-addresses#availability-tests).

> [!NOTE]
> İle birden fazla konumdan test öneririz **en az beş**. Bu geçici bir sorun belirli bir konum neden olabilir yanlış alarm önlemek içindir. Buna ek olarak en uygun yapılandırma sahip olduğunu bulduk **test konumları sayısı için uyarı konumu eşiği + 2 eşit**.

### <a name="success-criteria"></a>Başarı ölçütleri

|Ayar| Açıklama
|----|----|----|
| **Test zaman aşımı** |Yavaş yanıtlar hakkında uyarı almak için bu değeri azaltın. Yanıtlar sitenizden bu süre içinde alınmadıysa test başarısız sayılır. **Bağımlı istekleri ayrıştır**’ı seçtiyseniz; tüm görüntüler, stil dosyaları, betikler ve diğer bağımlı kaynaklar bu süre içinde alınmış olmalıdır.|
| **HTTP yanıtı** | Başarılı sayılan döndürüldü durum kodu. 200, normal web sayfası döndürüldüğünü belirten koddur.|
| **İçerik eşleşmesi** | "Hoş Geldiniz!" gibi bir dize Her yanıtta büyük küçük harfe duyarlı bir tam eşleşme oluştuğunu test edebiliriz. Joker karakter bulunmayan düz bir dize olmalıdır. Sayfanızın içeriği değişirse bunu güncelleştirmeniz gerektiğini unutmayın. **İçerik eşleşmesi ile yalnızca İngilizce karakterler desteklenir** |

### <a name="alerts"></a>Uyarılar

|Ayar| Açıklama
|----|----|----|
|**Neredeyse gerçek zamanlı (Önizleme)** | Neredeyse gerçek zamanlı uyarılar kullanmanızı öneririz. Bu tür bir uyarı yapılandırma, bir kullanılabilirlik testi oluşturduktan sonra yapılır.  |
|**Klasik** | Artık, yeni kullanılabilirlik testleri için Klasik uyarılar kullanılması önerilir.|
|**Uyarı konumu eşiği**|En az 3/5 konumları öneririz. Uyarı konumu eşiği ve test konumları sayısı arasındaki en iyi ilişki **uyarı konumu eşiği** = **sayısı test konumları - 2, en az beş test konumuyla.**|

## <a name="see-your-availability-test-results"></a>Kullanılabilirlik testi sonuçlarınızı görme

Kullanılabilirlik testi sonucu hem satır hem de dağılım çizim görünümlerle görselleştirilebilir.

Birkaç dakika sonra tıklayın **Yenile** test sonuçlarını görmek için.

![Satır görünümü](./media/monitor-web-app-availability/availability-refresh-002.png)

Dağılım Grafiği, tanılama testi adım ayrıntılarını içeren test sonuçlarının örnekleri görüntüler. Test altyapısı, hata içeren testler için tanılama ayrıntılarını depolar. Başarılı testlerde, yürütmelerin bir alt kümesi için tanılama ayrıntıları depolanır. Test görmek için ad ve konum test yeşil/kırmızı noktalardan herhangi üzerine gelin.

![Satır görünümü](./media/monitor-web-app-availability/availability-scatter-plot-003.png)

Belirli bir testi veya konumu seçin ya da ilgilendiğiniz dönemle ilgili daha fazla sonuç görmek için zaman dilimini küçültün. Arama Gezgini’ni kullanarak tüm yürütmelerden alınan sonuçları görün veya Analytics sorgularını kullanarak bu veriler üzerinde özel raporlar çalıştırın.

## <a name="inspect-and-edit-tests"></a>Testleri inceleme ve düzenleme

Bir test düzenlemek, geçici olarak devre dışı veya silmek için bir test adı yanındaki üç noktaya tıklayın. Bu, bir değişiklik yapıldıktan sonra tüm test aracılarına yaymak yapılandırma değişiklikleri 20 dakikaya kadar sürebilir.

![Test ayrıntılarını görüntüleyin. Düzenle ve bir web testi devre dışı bırakma](./media/monitor-web-app-availability/edit.png)

Hizmetinizde bakım gerçekleştirdiğiniz sırada kullanılabilirlik testlerini veya bunlarla ilişkili uyarıları devre dışı bırakmak isteyebilirsiniz.

## <a name="if-you-see-failures"></a>Hata görürseniz

Kırmızı noktaya tıklayın.

![Kırmızı noktaya tıklama](./media/monitor-web-app-availability/open-instance-3.png)

Bir kullanılabilirlik testi sonucundan tüm bileşenler genelinde işlem ayrıntılarını görebilirsiniz. Burada şunları yapabilirsiniz:

* Sunucunuzdan alınan yanıtı denetleme.
* Hata ile başarısız kullanılabilirlik testi işlenirken toplanan bağıntılı sunucu tarafı telemetri tanılayın.
* Bir sorun oturum veya bir sorunu izlemek için Git veya Azure panoları iş öğesi. Hata, bu olayın bir bağlantısını içerir.
* Web testi sonucunu Visual Studio’da açın.

Deneyimi uçtan uca işlem tanılamaları hakkında daha fazla bilgi edinin [burada](../../azure-monitor/app/transaction-diagnostics.md).

Sentetik kullanılabilirlik testin başarısız olmasına neden olan sunucu tarafı özel durumun ayrıntılarını görmek için özel durum satırına tıklayın. Ayrıca Al [hata ayıklama anlık görüntüsünü](../../azure-monitor/app/snapshot-debugger.md) daha zengin kod düzeyi tanılama için.

![Sunucu tarafı tanılamalarını](./media/monitor-web-app-availability/open-instance-4.png)

Ham sonuçlara ek olarak da iki temel kullanılabilirlik ölçümlerini görüntüleyebilirsiniz [ölçüm Gezgini](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-getting-started):

1. Kullanılabilirlik: Tüm test yürütmelerinde başarılı testlerin yüzdesi.
2. Test süresi: Tüm test yürütme ortalama test süresi.

## <a name="automation"></a>Otomasyon

* Otomatik olarak [kullanılabilirlik testi ayarlamak için PowerShell betiklerini kullanın](../../azure-monitor/app/powershell.md#add-an-availability-test).
* Bir uyarı ortaya çıktığında çağrılan bir [web kancası](../../azure-monitor/platform/alerts-webhooks.md) ayarlayın.

## <a name="troubleshooting"></a>Sorun giderme

Ayrılmış [sorunlarını giderme makalesine](troubleshoot-availability.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Kullanılabilirlik uyarıları](availability-alerts.md)
* [Çok adımlı web testleri](availability-multistep.md)


