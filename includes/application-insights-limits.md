Uygulama başına (yani, izleme anahtarı başına) ölçüm ve olay sayısı için bazı limitler mevcuttur. Limitler seçtiğiniz [fiyatlandırma planına](https://azure.microsoft.com/pricing/details/application-insights/) bağlıdır.

| **Kaynak** | **Varsayılan sınır** | **Not**
| --- | --- | --- |
| Günlük toplam veri | 100 GB | Bir uç ayarlayarak verileri azaltabilirsiniz. Daha fazla gerekiyorsa, kayıt sayısı sınırını artırabilirsiniz 1000 GB portalından. 1000 GB'den büyük olan kapasiteleri için posta gönderin AIDataCap@microsoft.com.
| Aylık ücretsiz veri<br/> (Temel fiyat planı) | 1 GB | Ek veriler gigabayt başına ücretlendirilir.
| Azaltma | 32 bin olay/saniye | Sınır bir dakika içinde ölçülür.
| Veri saklama | 90 gün | Bu kaynak [Search](../articles/application-insights/app-insights-diagnostic-search.md), [Analytics](../articles/application-insights/app-insights-analytics.md) ve [Ölçüm Gezgini](../articles/application-insights/app-insights-metrics-explorer.md) içindir.
| [Çok adımlı kullanılabilirlik testi](../articles/application-insights/app-insights-monitor-web-app-availability.md#multi-step-web-tests) ayrıntılı sonuçlarını saklama | 90 gün | Bu kaynak her adımın ayrıntılı sonuçlarını verir.
| En fazla olay boyutu | 64 K | 
| Özellik ve ölçüm adı uzunluğu | 150 | Bkz: [yazın şemaları](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/Schemas/Docs/)
| Özellik değeri dize uzunluğu | 8,192 | Bkz: [yazın şemaları](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/Schemas/Docs/)
| İzleme ve özel durum iletisi uzunluğu | 10 bin | Bkz: [yazın şemaları](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/Schemas/Docs/)
| Uygulama başına [kullanılabilirlik testi](../articles/application-insights/app-insights-monitor-web-app-availability.md) sayısı  | 100 |
| [Profil Oluşturucu](../articles/application-insights/app-insights-profiler.md) veri saklama | 5 gün |
| [Profil Oluşturucu](../articles/application-insights/app-insights-profiler.md) gün başına gönderilen veriler | 10GB |

Daha fazla bilgi için bkz. [Application Insights fiyatlandırma ve kotaları hakkında](../articles/application-insights/app-insights-pricing.md).

