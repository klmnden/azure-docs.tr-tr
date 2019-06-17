---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: ce64047fd7490106790ea8bb1ad7963d82a87c24
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66238445"
---
| Resource | Boş | Paylaşılan | Temel | Standart | Premium (v2) | Yalıtılmış </th> |
| --- | --- | --- | --- | --- | --- | --- |
| [Web, mobil veya API uygulamaları](https://azure.microsoft.com/services/app-service/) başına [Azure App Service planı](../articles/app-service/overview-hosting-plans.md)<sup>1</sup> |10 |100 |Sınırsız<sup>2</sup> |Sınırsız<sup>2</sup> |Sınırsız<sup>2</sup> |Sınırsız<sup>2</sup>|
| [App Service planı](../articles/app-service/overview-hosting-plans.md) |Bölge başına 10 |kaynak grubu başına 10 |kaynak grubu başına 100 |kaynak grubu başına 100 |kaynak grubu başına 100 |kaynak grubu başına 100|
| İşlem örneği türü |Paylaşılan |Paylaşılan |Ayrılmış<sup>3</sup> |Ayrılmış<sup>3</sup> |Ayrılmış<sup>3</sup></p> |Ayrılmış<sup>3</sup>|
| [Ölçeği genişletme](../articles/app-service/web-sites-scale.md) (en fazla örnek) |Paylaşılan 1 |Paylaşılan 1 |3 adanmış<sup>3</sup> |ayrılmış 10<sup>3</sup> |ayrılmış 20<sup>3</sup>|ayrılmış 100<sup>4</sup>|
| Depolama<sup>5</sup> |1 GB<sup>5</sup> |1 GB<sup>5</sup> |10 GB<sup>5</sup> |50 GB<sup>5</sup> |250 GB<sup>5</sup></p> |1 TB<sup>5</sup>|
| CPU süresi (5 dakika)<sup>6</sup> |3 dakika |3 dakika |Sınırsız, standart, ödeme [oranları](https://azure.microsoft.com/pricing/details/app-service/)</a> |Sınırsız, standart, ödeme [oranları](https://azure.microsoft.com/pricing/details/app-service/)</a> |Sınırsız, standart, ödeme [oranları](https://azure.microsoft.com/pricing/details/app-service/)</a> |Sınırsız, standart, ödeme [oranları](https://azure.microsoft.com/pricing/details/app-service/)</a>|
| CPU süresi (gün)<sup>6</sup> |60 dakika |240 dakika |Sınırsız, standart, ödeme [oranları](https://azure.microsoft.com/pricing/details/app-service/)</a> |Sınırsız, standart, ödeme [oranları](https://azure.microsoft.com/pricing/details/app-service/)</a> |Sınırsız, standart, ödeme [oranları](https://azure.microsoft.com/pricing/details/app-service/)</a> |Sınırsız, standart, ödeme [oranları](https://azure.microsoft.com/pricing/details/app-service/)</a> |
| Bellek (1 saat) |App Service planı başına 1024 MB |Uygulama başına 1024 MB |Yok |Yok |Yok |Yok |
| Bant genişliği |165 MB |Sınırsız [veri aktarımı ücretleriyle](https://azure.microsoft.com/pricing/details/data-transfers/) Uygula |Sınırsız [veri aktarımı ücretleriyle](https://azure.microsoft.com/pricing/details/data-transfers/) Uygula |Sınırsız [veri aktarımı ücretleriyle](https://azure.microsoft.com/pricing/details/data-transfers/) Uygula |Sınırsız [veri aktarımı ücretleriyle](https://azure.microsoft.com/pricing/details/data-transfers/) Uygula |Sınırsız [veri aktarımı ücretleriyle](https://azure.microsoft.com/pricing/details/data-transfers/) Uygula |
| Uygulama mimarisi |32 bit |32 bit |32-bit/64-bit |32-bit/64-bit |32-bit/64-bit |32-bit/64-bit |
| Web yuvaları örnek başına<sup>7</sup> |5 |35 |350 |Sınırsız |Sınırsız |Sınırsız |
| Eş zamanlı [hata ayıklayıcı bağlantıları](../articles/app-service/troubleshoot-dotnet-visual-studio.md) uygulama başına |1 |1\. |1 |5 |5 |5 |
| App Service sertifikaları, abonelik başına<sup>10</sup>| Desteklenmiyor | Desteklenmiyor |10 |10 |10 |10 |
| Uygulama başına özel etki alanları</a> |0 (azurewebsites.net alt etki alanı yalnızca)|500 |500 |500 |500 |500 |
| Özel etki alanı [SSL desteği](../articles/app-service/app-service-web-tutorial-custom-ssl.md) |Desteklenen değil için joker karakter sertifikası *. azurewebsites.net varsayılan olarak kullanılabilir|Desteklenen değil için joker karakter sertifikası *. azurewebsites.net varsayılan olarak kullanılabilir|Sınırsız SNI SSL bağlantıları |Sınırsız SNI SSL ve 1 IP SSL bağlantıları dahildir |Sınırsız SNI SSL ve 1 IP SSL bağlantıları dahildir | Sınırsız SNI SSL ve 1 IP SSL bağlantıları dahildir|
| Tümleşik yük dengeleyici | |X |X |X |X |X<sup>9</sup> |
| [Her zaman açık](../articles/app-service/configure-common.md) | | |X |X |X |X |
| [Zamanlanmış yedeklemeler](../articles/app-service/manage-backup.md) | | | | Zamanlanmış yedeklemeleri her 2 saatte bir, en fazla 12 yedek her gün (el ile + zamanlanmış) | Zamanlanmış yedeklemeleri saatlik, en fazla 50 yedek her gün (el ile + zamanlanmış) | Zamanlanmış yedeklemeleri saatlik, en fazla 50 yedek her gün (el ile + zamanlanmış) |
| [Otomatik Ölçeklendirme](../articles/app-service/web-sites-scale.md) | | | |X |X |X |
| [WebJobs](../articles/app-service/webjobs-create.md)<sup>8</sup> |X |X |X |X |X |X |
| [Azure Zamanlayıcı](https://azure.microsoft.com/services/scheduler/) desteği | |X |X |X |X |X |
| [Uç Nokta izleme](../articles/app-service/web-sites-monitor.md) | | |X |X |X |X |
| [Hazırlama yuvası](../articles/app-service/deploy-staging-slots.md) | | | |5 |20 |20 |
| SLA | |  |%99,9 |%99,95|%99,95|%99,95|  

<sup>1</sup>uygulamaları ve depolama kotalarını aksi belirtilmediği sürece App Service planı markalarıdır.  
<sup>2</sup>etkinlik uygulamaları, makine örnekleri ve ilgili kaynak kullanımını boyutu, bu makinelere barındırabilirsiniz uygulamaları gerçek sayısına bağlıdır.  
<sup>3</sup>ayrılmış örnekler, farklı boyutlarda olabilir. Daha fazla bilgi için [App Service fiyatlandırması](https://azure.microsoft.com/pricing/details/app-service/).  
<sup>4</sup>istek üzerine daha fazla izin verilir.  
<sup>5</sup>depolama toplam içerik boyutu tüm uygulamalar arasında aynı App Service planında sınırlıdır.  
<sup>6</sup>bu kaynakları ayrılmış örneklerin (örnek boyutu ve örnek sayısını) üzerinde fiziksel kaynaklar tarafından kısıtlanmıştır.  
<sup>7</sup>iki örneği için temel katmanda bir uygulamayı ölçeklendirme, 350 eşzamanlı bağlantı her iki örneği vardır.  
<sup>8</sup>çalıştırma özel yürütülebilir uygulamalar ve/veya betikler isteğe bağlı, bir programa göre veya sürekli bir App Service arka plan görevi olarak örnek. WebJob'ların sürekli yürütülebilmesi için Her Zaman Açık özelliği gereklidir. Zamanlanan WebJob'lar için Azure Zamanlayıcı Ücretsiz veya Standart gereklidir. App Service örneği çalıştırmak WebJobs sayısı önceden tanımlanmış bir sınır yoktur. Hangi uygulama kodunun yapmaya çalıştığını bağımlı pratik sınırı yoktur.  
<sup>9</sup>app Service yalıtılmış SKU'ları sahip olması, dahili olarak yük dengeli (ILB) Azure Load Balancer ile internet'ten genel bağlantı olmasını olanağı. Bunun sonucunda, ILB Yalıtılmış App Service’in bazı özelliklerin, ILB ağ uç noktasına doğrudan erişimi olan makinelerden kullanılması gerekir.  
<sup>10</sup>App Service sertifikası kota sınırı abonelik başına 200 bir üst sınır bir destek isteği ile artırılabilir.  