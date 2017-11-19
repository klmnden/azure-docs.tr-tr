| Kaynak | Ücretsiz | Paylaşılan (Önizleme) | Temel | Standart | Premium (Önizleme)</th> |
| --- | --- | --- | --- | --- | --- |
| [Web, mobil ve API apps](https://azure.microsoft.com/services/app-service/) başına [uygulama hizmeti planı](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)<sup>1</sup> |10 |100 |Sınırsız<sup>2</sup> |Sınırsız<sup>2</sup> |Sınırsız<sup>2</sup> |
| [Logic apps](https://azure.microsoft.com/services/app-service/logic/) başına [uygulama hizmeti planı](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)</a><sup>1</sup> |10 |10 |10 |Çekirdek başına 20 |Çekirdek başına 20 |
| [App Service planı](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) |Bölge başına 1 |kaynak grubu başına 10 |kaynak grubu başına 100 |kaynak grubu başına 100 |kaynak grubu başına 100 |
| İşlem örneği türü |Paylaşılan |Paylaşılan |Ayrılmış<sup>3</sup> |Ayrılmış<sup>3</sup> |Ayrılmış<sup>3</sup></p> |
| [Genişleme](../articles/app-service/web-sites-scale.md) (max örnekleri) |Paylaşılan 1 |Paylaşılan 1 |ayrılmış 3<sup>3</sup> |ayrılmış 10<sup>3</sup> |20 ayrılmış (ana 50)<sup>3,4</sup> |
| Depolama<sup>5</sup> |1 GB<sup>5</sup> |1 GB<sup>5</sup> |10 GB<sup>5</sup> |50 GB<sup>5</sup> |500 GB<sup>4,5</sup></p> |
| CPU süresi (5 dk.)<sup>6</sup> |3 dakika |3 dakika |Sınırsız, standart, ödeme [hızları](https://azure.microsoft.com/pricing/details/app-service/)</a> |Sınırsız, standart tarifelere ödeme |Sınırsız, standart tarifelere ödeme |
| CPU süresi (gün)<sup>6</sup> |60 dakika |240 dakika |Sınırsız, standart, ödeme [hızları](https://azure.microsoft.com/pricing/details/app-service/)</a> |Sınırsız, standart tarifelere ödeme |Sınırsız, standart tarifelere ödeme |
| Bellek (1 saat) |Uygulama hizmeti planı başına 1024 MB |Uygulama başına 1024 MB |Yok |Yok |Yok |
| Bant Genişliği |165 MB |Sınırsız [veri aktarımı oranları](https://azure.microsoft.com/pricing/details/data-transfers/) Uygula |Sınırsız veri aktarımı ücretler geçerlidir |Sınırsız veri aktarımı ücretler geçerlidir |Sınırsız veri aktarımı ücretler geçerlidir |
| Uygulama mimarisi |32 bit |32 bit |32-bit/64-bit |32-bit/64-bit |32-bit/64-bit |
| Yuva başına örnek Web<sup>7</sup> |5 |35 |350 |Sınırsız |Sınırsız |
| Eşzamanlı [hata ayıklayıcı bağlantıları](../articles/app-service/web-sites-dotnet-troubleshoot-visual-studio.md) uygulama başına |1 |1 |1 |5 |5 |
| [FTP/S ve SSL ile azurewebsites.NET alt etki alanı](../articles/app-service/app-service-web-tutorial-custom-ssl.md) |X |X |X |X |X |
| [Özel etki alanı](../articles/app-service/app-service-web-tutorial-custom-domain.md) desteği | |X |X |X |X |
| Özel etki alanı [SSL desteği](../articles/app-service/app-service-web-tutorial-custom-ssl.md) | | |Sınırsız SNI SSL bağlantıları |Sınırsız SNI SSL ve 1 IP SSL bağlantılar dahil |Sınırsız SNI SSL ve 1 IP SSL bağlantılar dahil |
| Tümleşik Yük Dengeleyici | |X |X |X |X |
| [Her zaman açık](../articles/app-service/web-sites-configure.md) | | |X |X |X |
| [Zamanlanmış yedeklemeleri](../articles/app-service/web-sites-backup.md) | | | |gün başına 12 |Her 5 dakikada<sup>8</sup> |
| [Otomatik ölçeklendirme](../articles/app-service/web-sites-scale.md) | | | |X |X |
| [Web işleri](../articles/app-service/web-sites-create-web-jobs.md)<sup>9</sup> |X |X |X |X |X |
| [Azure Zamanlayıcı](https://azure.microsoft.com/services/scheduler/) desteği | |X |X |X |X |
| [Uç Nokta izleme](../articles/app-service/web-sites-monitor.md) | | |X |X |X |
| [Hazırlama yuvalarına](../articles/app-service/web-sites-staged-publishing.md) | | | |5 |20 |
| Uygulama başına düşen özel etki alanı</a> | |500 |500 |500 |500 |
| SLA | |<p> |%99,9 |99.95%<sup>10</sup> |99.95%<sup>10</sup> |

<sup>1</sup>uygulamaları ve depolama kotaları aksi belirtilmediği sürece uygulama hizmeti plan başına şunlardır.  
<sup>2</sup>bu makinelerde barındırabilir uygulamalar gerçek sayısını uygulamaları etkinliğini, makine örnekleri ve karşılık gelen kaynak kullanımı boyutunu bağlıdır.  
<sup>3</sup>ayrılmış örnekleri farklı boyutlarda olabilir. Bkz: [App Service fiyatlandırması](https://azure.microsoft.com/pricing/details/data-transfers/pricing/details/app-service/) daha fazla ayrıntı için.  
<sup>4</sup>premium katmanı sağlayan en çok 50, örnekleri (tabi kullanılabilirlik) ve 500 GB disk alanı App Service ortamları kullanırken hesaplar ve 20 işlem örnekleri ile 250 GB depolama Aksi takdirde.  
<sup>5</sup>depolama sınırı toplam içerik tüm uygulamalar aynı uygulama hizmeti planında boyutudur. Daha fazla depolama seçenekleri kullanılabilir [uygulama hizmeti ortamı](../articles/app-service/environment/app-service-web-configure-an-app-service-environment.md#storage)  
<sup>6</sup>bu kaynakları (örnek boyutu ve örnek sayısı) adanmış örneklerinde fiziksel kaynaklar tarafından kısıtlanmıştır.  
<sup>7</sup>temel katmandaki iki örneği için bir uygulama ölçeklendirme, her iki örneği 350 eşzamanlı bağlantı vardır.  
<sup>8</sup>App Service ortamları kullanırken ve 50 kez günde Aksi takdirde her 5 dakikada bir kadar aşağı yedekleme aralıklarını premium katmanı sağlar.  
<sup>9</sup>çalıştırmak özel yürütülebilir dosyaları ve/veya komut dosyalarını bir zamanlamaya göre isteğe bağlı veya sürekli olarak App Service içinde bir arka plan görev örneği. WebJob'ların sürekli yürütülebilmesi için Her Zaman Açık özelliği gereklidir. Zamanlanan WebJob'lar için Azure Zamanlayıcı Ücretsiz veya Standart gereklidir. Uygulama hizmeti örneğini çalıştırabilirsiniz WebJobs sayısı önceden tanımlanmış bir sınır yoktur ancak ne uygulama kodu tarafından gerçekleştirilmeye çalışılan bağımlı pratik sınırlarına vardır.   
<sup>10</sup>99,95 yük devretme için yapılandırılmış Azure trafik Yöneticisi ile birden çok örneği kullanan dağıtımlar için sağlanan % SLA.  

