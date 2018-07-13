| Kaynak | Ücretsiz | Paylaşılan (Önizleme) | Temel | Standart | Premium </th> |
| --- | --- | --- | --- | --- | --- |
| [Web, mobil veya API uygulamaları](https://azure.microsoft.com/services/app-service/) başına [App Service planı](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)<sup>1</sup> |10 |100 |Sınırsız<sup>2</sup> |Sınırsız<sup>2</sup> |Sınırsız<sup>2</sup> |
| [Logic apps](https://azure.microsoft.com/services/app-service/logic/) başına [App Service planı](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)</a><sup>1</sup> |10 |10 |10 |Çekirdek başına 20 |Çekirdek başına 20 |
| [App Service planı](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) |Bölge başına 1 |kaynak grubu başına 10 |kaynak grubu başına 100 |kaynak grubu başına 100 |kaynak grubu başına 100 |
| İşlem örneği türü |Paylaşılan |Paylaşılan |Ayrılmış<sup>3</sup> |Ayrılmış<sup>3</sup> |Ayrılmış<sup>3</sup></p> |
| [Ölçek genişletme](../articles/app-service/web-sites-scale.md) (en fazla örnek) |Paylaşılan 1 |Paylaşılan 1 |3 adanmış<sup>3</sup> |ayrılmış 10<sup>3</sup> |20 ayrılmış (50 ase'deki)<sup>3,4</sup> |
| Depolama<sup>5</sup> |1 GB<sup>5</sup> |1 GB<sup>5</sup> |10 GB<sup>5</sup> |50 GB<sup>5</sup> |500 GB<sup>4,5</sup></p> |
| CPU süresi (5 dk.)<sup>6</sup> |3 dakika |3 dakika |Sınırsız, standart, ödeme [oranları](https://azure.microsoft.com/pricing/details/app-service/)</a> |Sınırsız, ödeme standart fiyatları |Sınırsız, ödeme standart fiyatları |
| CPU süresi (gün)<sup>6</sup> |60 dakika |240 dakika |Sınırsız, standart, ödeme [oranları](https://azure.microsoft.com/pricing/details/app-service/)</a> |Sınırsız, ödeme standart fiyatları |Sınırsız, ödeme standart fiyatları |
| Bellek (1 saat) |App Service planı başına 1024 MB |Uygulama başına 1024 MB |Yok |Yok |Yok |
| Bant Genişliği |165 MB |Sınırsız [veri aktarımı ücretleriyle](https://azure.microsoft.com/pricing/details/data-transfers/) Uygula |Sınırsız, veri aktarımı ücretleri uygulanır |Sınırsız, veri aktarımı ücretleri uygulanır |Sınırsız, veri aktarımı ücretleri uygulanır |
| Uygulama mimarisi |32 bit |32 bit |32-bit/64-bit |32-bit/64-bit |32-bit/64-bit |
| Web yuvaları örnek başına<sup>7</sup> |5 |35 |350 |Sınırsız |Sınırsız |
| Eş zamanlı [hata ayıklayıcı bağlantıları](../articles/app-service/web-sites-dotnet-troubleshoot-visual-studio.md) uygulama başına |1 |1 |1 |5 |5 |
| [FTP/S ve SSL içeren azurewebsites.NET alt etki alanı](../articles/app-service/app-service-web-tutorial-custom-ssl.md) |X |X |X |X |X |
| [Özel etki alanı](../articles/app-service/app-service-web-tutorial-custom-domain.md) desteği | |X |X |X |X |
| Özel etki alanı [SSL desteği](../articles/app-service/app-service-web-tutorial-custom-ssl.md) | | |Sınırsız SNI SSL bağlantıları |Sınırsız SNI SSL ve 1 IP SSL bağlantıları dahildir |Sınırsız SNI SSL ve 1 IP SSL bağlantıları dahildir |
| Tümleşik Yük Dengeleyici | |X |X |X |X |
| [Her zaman açık](../articles/app-service/web-sites-configure.md) | | |X |X |X |
| [Zamanlanmış yedeklemeler](../articles/app-service/web-sites-backup.md) | | | | Zamanlanmış yedeklemeleri her 2 saatte bir, en fazla 12 yedeklemeleri her gün (el ile + zamanlanmış) | Zamanlanmış yedeklemeleri saatlik, en fazla 50 yedeklemeleri her gün (el ile + zamanlanmış) |
| [Otomatik ölçeklendirme](../articles/app-service/web-sites-scale.md) | | | |X |X |
| [WebJobs](../articles/app-service/web-sites-create-web-jobs.md)<sup>8</sup> |X |X |X |X |X |
| [Azure Zamanlayıcı](https://azure.microsoft.com/services/scheduler/) desteği | |X |X |X |X |
| [Uç Nokta izleme](../articles/app-service/web-sites-monitor.md) | | |X |X |X |
| [Hazırlama yuvaları](../articles/app-service/web-sites-staged-publishing.md) | | | |5 |20 |
| Uygulama başına özel etki alanları</a> | |500 |500 |500 |500 |
| SLA | |<p> |%99,9 |99.95%<sup>10</sup> |99.95%<sup>9</sup> |

<sup>1</sup>uygulamaları ve depolama kotalarını aksi belirtilmediği sürece App Service planı markalarıdır.  
<sup>2</sup>etkinlik uygulamaları, makine örnekleri ve ilgili kaynak kullanımını boyutu, bu makinelere barındırabilirsiniz uygulamaları gerçek sayısına bağlıdır.  
<sup>3</sup>ayrılmış örnekler, farklı boyutlarda olabilir. Bkz: [App Service fiyatlandırması](https://azure.microsoft.com/pricing/details/app-service/) daha fazla ayrıntı için.  
<sup>4</sup>premium katmanı sağlayan en fazla 50, örneği (kullanılabilirliğe bağlıdır) ve 500 GB disk alanı App Service ortamları kullanılırken hesaplar ve 20 işlem örneği ve 250 GB depolama Aksi takdirde.  
<sup>5</sup>depolama toplam içerik boyutu tüm uygulamalar arasında aynı App Service planında sınırlıdır. Daha fazla depolama seçenekleri kullanılabilir [App Service ortamı](../articles/app-service/environment/app-service-web-configure-an-app-service-environment.md#storage)  
<sup>6</sup>bu kaynakları ayrılmış örneklerin (örnek boyutu ve örnek sayısını) üzerinde fiziksel kaynaklar tarafından kısıtlanmıştır.  
<sup>7</sup>iki örneği için temel katmanda bir uygulamayı ölçeklendirme, 350 eşzamanlı bağlantı her iki örneği vardır.  
<sup>8</sup>çalıştırma özel yürütülebilir uygulamalar ve/veya betikler isteğe bağlı, bir programa göre veya sürekli bir App Service arka plan görevi olarak örnek. WebJob'ların sürekli yürütülebilmesi için Her Zaman Açık özelliği gereklidir. Zamanlanan WebJob'lar için Azure Zamanlayıcı Ücretsiz veya Standart gereklidir. App Service örneği çalıştırmak WebJobs sayısı önceden tanımlanmış bir sınır yoktur ancak hangi uygulama kodunun yapmaya çalıştığını bağımlı pratik sınırları vardır.   
<sup>9</sup>sağlanan yapılandırılan Azure Traffic Manager ile yük devretme için çoklu örnekler kullanan dağıtımlar için % 99,95 oranında SLA sunulur.  

