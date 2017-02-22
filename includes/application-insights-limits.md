Uygulama başına (yani, izleme anahtarı başına) ölçüm ve olay sayısı için bazı limitler mevcuttur. Limitler seçtiğiniz [fiyatlandırma planına](https://azure.microsoft.com/pricing/details/application-insights/) bağlıdır.

| **Kaynak** | **Varsayılan sınır** | **Not**
| --- | --- | --- |
| Günlük toplam veri | 500 GB | Bir uç ayarlayarak verileri azaltabilirsiniz. Daha fazla gerekirse AIDataCap@microsoft.com adresine posta gönderin.
| Aylık ücretsiz veri<br/> (Temel fiyat planı) | 1 GB | Ek veriler gigabayt başına ücretlendirilir.
| Azaltma | 32 bin olay/saniye | Sınır bir dakika içinde ölçülür.
| Veri saklama | 90 gün | Bu kaynak [Search](../articles/application-insights/app-insights-diagnostic-search.md), [Analytics](../articles/application-insights/app-insights-analytics.md) ve [Ölçüm Gezgini](../articles/application-insights/app-insights-metrics-explorer.md) içindir.
| [Çok adımlı kullanılabilirlik testi](../articles/application-insights/app-insights-monitor-web-app-availability.md#multi-step-web-tests) ayrıntılı sonuçlarını saklama | 90 gün | Bu kaynak her adımın ayrıntılı sonuçlarını verir.
| Özellik ve ölçüm adı uzunluğu | 150 |
| Özellik değeri dize uzunluğu | 8,192 |
| İzleme ve özel durum iletisi uzunluğu | 10 bin |
| Uygulama başına [kullanılabilirlik testi](../articles/application-insights/app-insights-monitor-web-app-availability.md) sayısı  | 10 |

Daha fazla bilgi için bkz. [Application Insights fiyatlandırma ve kotaları hakkında](../articles/application-insights/app-insights-pricing.md).


<!--HONumber=Feb17_HO2-->


