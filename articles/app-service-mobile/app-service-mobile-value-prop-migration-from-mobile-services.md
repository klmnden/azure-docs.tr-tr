<properties
    pageTitle="Mobile Services kullanıyorum, App Service bana nasıl yardımcı olur?"
    description="App Service’in mevcut Mobile Services projelerinize sağladığı avantajları öğrenin."
    services="app-service\mobile"
    documentationCenter="ios"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="05/03/2016"
    ms.author="krisragh"/>

# <a name="getting-started"> </a>Mobile Services kullanıyorum, App Service bana nasıl yardımcı olur?

##Genel Bakış
Mevcut Mobil Hizmetiniz güvendedir ve desteklenmeye devam edecektir. Ancak *Azure App Service* platformunun sağladığı, mobil uygulamanız için bugün Mobile Services’de bulunmayan çok sayıda avantaj vardır:

- Web ve mobil istemciler içeren uygulamalar için daha basit, daha kolay ve daha düşük maliyetli teklifler.
- Web İşleri, özel CNames, daha iyi izleme dahil yeni ana bilgisayara özellikleri.
- Traffic Manager ile anahtar teslimi tümleştirmesi
- Karma Bağlantılara ek olarak, VNet kullanarak şirket içi kaynaklarınıza ve VPN’lere bağlantı
- NewRelic veya AppInsights kullanarak uygulamanız için izleme, uyarma ve sorun giderme.
- Temel hesaplama kaynakları ve fiyatlandırma için daha zengin yelpaze
- Yerleşik otomatik ölçeklendirme, yük dengeleme ve performans izleme
- Yerleşik hazırlık, yedekleme, geri alma ve üretim sırasında test etme özellikleri

## Yeni barındırma özellikleri
*Azure App Service*’de *Mobil Uygulama* arka uç kodu; Web Uygulaması ve API Uygulaması ile aynı kapsayıcıda çalışır. Böylece, şu anda Mobile Services’de bulunmayan bazıları dahil, bu kapsayıcıdaki tüm özelliklerden faydalanabilirsiniz.

- Web İşleri aracılığıyla sürekli çalışan arka uç mantığı ekleyin
- Arka uç kodunuzun her zaman çalıştığından emin olun
- Mobil arka uç, uç noktalarınıza kolay ve kararlı adlar sağlamak için özel CNames kullanın
- Traffic Manager ile uygulamanızı coğrafi olarak ölçeklendirin
- İstediğiniz tüm kitaplıkları ve paketleri ekleyin.
- (.NET için) MVC dahil, ASP.NET’in her özelliğinden yararlanın
- (Node.js için) Ortak MVC kitaplıkları dahil, Düğüm ekosisteminin tüm saf JavaScript kitaplığından yararlanın.

##VNet kullanarak şirket içi verilere erişin
Günümüzde Mobile Services ile şirket için kaynaklara erişim için Karma Bağlantıları halihazırda kullanabilirsiniz. Ancak bir VPN çözümünün tercih edildiği durumlar vardır. *Azure App Service* ile Mobil Uygulama arka uç kodunuz için Azure VNet kullanabilirsiniz.

##Sık kullanılan arka uç dilinizi kullanın
*Azure App Service* ASP.NET ve Node.js platformları için, en son çalışma zamanları dahil, daha geniş ve daha zengin bir destek sunar.

##Otomatik ölçeklendirme ayarlayın
Mobile Services ile arka uç kodunuzun tm örnekleri küçük VM’lerde çalışıyordu. *Azure App Service*, VM boyutlarını çok daha zengin seçeneklerle seçmenizi sağlar. Ayrıca çeşitli performans ölçümleri temelinde, gelen müşteri yükünü işlemek için hızlı şekilde ölçeği artırabilir ya da genişletebilirsiniz.

##“Haberdar” olun
Sizi ve ekibinizi otomatik bilgilendiren izleme ve uyarılarla gerçek zamanlı olarak sorulara yanıt verin. Mobil uygulamanızın nasıl çalıştığına ilişkin daha zengin bir öngörü sahibi olmak için New Relic ve AppInsights’dan gelen gelişmiş uygulama analizi ve izleme özelliğini tümleştirin. *Azure App Service* ile programlı şekilde ya da Azure Portal aracılığıyla, çeşitli performans ölçümleri temelinde uyarılar ayarlayabilirsiniz.

##Varlıklarınızı güvende tutun
Arka ucunuzu ve veritabanınızı otomatik olarak yedekleyin. Kodunuz ve verileriniz olağanüstü durumlara karşı güvendedir ve kolayca geri yüklenebilir, böylece işletmenizi güvenle çalıştırabilirsiniz.

##Hazır Ol, Hazırla, Başla!
*Azure App Service* ile artık mobil uygulamalarınız için birden fazla özel test ve hazırlık ortamı oluşturabilirsiniz. Dağıtmadan önce, bunları test gerçekleştirmek için kullanın. Kesinti süresi olmaksızın üretime geçin. Web uygulamaları, en iyi müşteri deneyimi sağlayacak şekilde önceden yüklenmiştir.

Bu [öğreticiyi](app-service-mobile-migrating-from-mobile-services.md) izleyerek mevcut Mobil Hizmetiniz için *App Service* avantajından yararlanmaya başlayabilirsiniz.




<!----HONumber=Jun16_HO2-->


