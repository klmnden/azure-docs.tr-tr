<properties
    pageTitle="Mobile Apps nedir?"
    description="App Service’in mevcut kurumsal mobil uygulamalarınıza sağladığı avantajları öğrenin."
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="05/03/2016"
    ms.author="krisagh"/>

# <a name="getting-started"> </a>Mobile Apps nedir?

Azure Uygulama Hizmeti, profesyonel geliştiricilere yönelik, web, mobil ve tümleştirme senaryolarına ilişin zengin özellik grupları sağlayan tam yönetilen bir [Hizmet olarak Platform](https://azure.microsoft.com/overview/what-is-paas/)’dur (PaaS). *Azure App Service* ’de *Mobile Apps*, Kurumsal Geliştiriciler ve Sistem Tümleştiricileri için mobil geliştiricilere zengin bir özellik kümesi sağlayan yüksek düzeyde ölçeklenebilir, genel olarak kullanılabilir bir mobil uygulama geliştirme platformu sunar.

![Mobile Apps](./media/app-service-mobile-value-prop/overview.png)

##Neden Mobile Apps?
*Azure App Service* ’de *Mobile Apps*, Kurumsal Geliştiriciler ve Sistem Tümleştiricileri için mobil geliştiricilere zengin bir özellik kümesi sağlayan yüksek düzeyde ölçeklenebilir, genel olarak kullanılabilir bir mobil uygulama geliştirme platformu sunar. Mobile Apps ile şunları yapabilirsiniz:

- **Yerel ve platformlar arası uygulamaları oluşturma** - Hem yerel iOS, Android ve Windows uygulamaları hem de platformlar arası Xamarin veya Cordova (Phonegap) uygulamaları oluştururken yerel SDK'ları kullanarak App Service’in avantajlarından faydalanabilirsiniz.
- **Kuruluş sistemlerinizi bağlama** - Mobile Apps ile dakikalar için kurumsal oturum ekleyebilir ve kuruluşunuzu şirket içi ya da bulut kaynaklarına bağlayabilirsiniz.
- **Veri eşitleme ile çevrimdışı kullanılmaya hazır uygulamalar oluşturma** - Kurumsal veri kaynaklarınız ya da SaaS API’leriniz ile bağlantı bulunduğunda arka planda verileri eşitlemek amacıyla çevrimdışı çalışan ve Mobile Apps kullanan uygulamalar oluşturarak mobil iş gücünüzü verimli kılın.
- **Milyonlarca kişiye Anında İletme Bildirimleri** - her cihazda, kendi ihtiyaçlarına özel ve doğru zamanda gönderilen anında iletme bildirimleriyle müşterilerinizle etkileşim kurun.

## Mobil Uygulama Özellikleri
Aşağıdaki özellikler, bulut etkin mobil geliştirme için önemlidir:

- **Kimlik Doğrulama ve Yetkilendirme** - Azure Active Directory dahil, sürekli büyüyen bir kurumsal kimlik sağlayıcısı listesinden ve Facebook, Google, Twitter ve Microsoft Hesabı gibi sosyal sağlayıcılardan seçim yapın.  Azure Mobile Apps tüm sağlayıcılar için OAuth 2.0 hizmeti sunar.  Ayrıca sağlayıcıya özel işlev için kimlik sağlayıcısına SDK tümleştirebilirsiniz.

  [Kimlik doğrulama özelliklerimiz] hakkında daha fazlasını keşfedin.

- **Veri Erişimi** - Azure Mobile Apps SQL Azure’a ya da şirket içi bir SQL Server’a bağlı mobil kullanımı kolay OData v3 veri kaynağı sağlar.  Bu hizmet, [Azure Table Storage], MongoDB, [DocumentDB] ve Office 365 ve Salesforce.com gibi SaaS API’si sağlayıcıları dahil, diğer NoSQL ve SQL veri sağlayıcılarıyla kolayca tümleştirmenizi sağlayarak Entity Framework’ü temel alabilir.
- **Çevrimdışı Eşitleme** - İstemci SDK’mız, çakışma çözümü desteği dahil, arka uç verileriyle otomatik olarak eşitlenebilecek çevrimdışı bir veri kümesiyle çalışan sağlam ve esnek mobil uygulamalar oluşturmanızı kolaylaştırır.

  [Veri özellikleri] hakkında daha fazlasını keşfedin.

- **Anında İletme Bildirimleri** - İstemci SDK'mız, aynı anda milyonlarca kullanıcıya anında iletme bildirimleri göndermenizi sağlayarak, Azure Notification Hubs'ın kayıt özellikleriyle sorunsuz şekilde tümleşir.

  [Anında iletme bildirimi özelliklerimiz] hakkında daha fazlasını keşfedin.

- **İstemci SDK'ları** - Yerel geliştirmeyi ([iOS], [Android] ve [Windows]), platformlar arası geliştirmeyi ([iOS ve Android için Xamarin], [Xamarin Forms]) ve karma uygulama geliştirmeyi ([Apache Cordova]) kapsayan SDK'ların eksiksiz bir kümesini sunuyoruz   Her istemci SDK’sı ile bir MIT lisansı ile birlikte sunulur ve açık kaynaklıdır.

## Azure App Service Özellikleri
Aşağıdaki platform özellikleri genellikle mobil üretim siteleri için yararlıdır.

- **Otomatik Ölçeklendirme** - App Service, gelen müşteri yükünü işlemek için hızlı şekilde ölçeği artırmanızı ya da genişletmenizi sağlar. Yük ya da zamanlama temelinden mobil uygulamanızın arka ucunu ölçeklendirmek için VM’nin sayısını ya da boyutunu el ile seçin ya da otomatik ölçeklendirmeyi ayarlayın.

  [Otomatik Ölçeklendirme] hakkında daha fazlasını keşfedin.

- **Hazırlık Ortamları** - App Service, yeni arka ucun daha büyük bir DevOps planının parçası olarak test olan A/B testini ve yerinde hazırlanmasını gerçekleştirmenizi sağlayarak, sitenizin birden fazla sürümünü çalıştırabilir.

  [Hazırlık Ortamları] hakkında daha fazlasını keşfedin.

- **Sürekli Dağıtım** - App Service, SCM sisteminizin bir dalına ileterek arka ucunuzun yeni bir sürümü otomatik olarak oluşturmanızı sağlayarak, ortak SCM sistemleriyle tümleştirilebilir.

  [Dağıtım seçenekleri] hakkında daha fazlasını keşfedin.

- **Sanal Ağ** - App Service, sanal ağ, ExpressRoute ya da karma bağlantılar kullanarak şirket içi kaynaklara bağlanabilir.

  [Karma bağlantılar], [sanal ağlar], ve [ExpressRoute] hakkında daha fazlasını keşfedin.

- **Yalıtılmış/Ayrılmış Ortamlar** - App Service, Azure App Service uygulamalarını yüksek ölçekte güvenli bir şekilde çalıştırılmaları için tam yalıtılmış ve ayrılmış bir ortamda çalıştırılabilir.  Bu, çok büyük ölçekli, yalıtım veya güvenli ağ erişimi gerektiren uygulama iş yükleri için idealdir.

  [App Service ortamları] hakkında daha fazlasını öğrenin.

## Başlarken ##
Mobile Apps’i kullanmaya başlamak için [Kullanmaya Başlama] öğreticisini izleyin.  Bu, istediğiniz mobil arka uç ve istemciyi oluşturmayı ve ardından kimlik doğrulama, çevrimdışı eşitleme ve anında bildirimlerini tümleştirmeye ilişkin temel kavramları kapsar.  Her istemci uygulaması için bir kez olmak üzere, [Kullanmaya Başlama] öğreticisini birkaç kez izleyebilirsiniz.

Azure Mobile Apps hakkında daha fazla bilgi için, lütfen [öğrenme haritamızı] gözden geçirin.
Azure App Service platformu hakkında daha fazla bilgi için bkz. [Azure App Service].

>[AZURE.NOTE] Azure hesabı için kaydolmadan önce Azure App Service’i kullanmaya başlamak isterseniz, App Service’de hemen kısa süreli bir başlangıç web uygulaması oluşturabileceğiniz [App Service’i Deneyin](https://tryappservice.azure.com/?appServiceName=mobile) sayfasına gidin. Kredi kartı ve taahhüt gerekmez.

<!-- URLs. -->
[Mobil Hizmetinizi App Service’e geçirme]: app-service-mobile-migrating-from-mobile-services.md
[Azure App Service]: ../app-service/app-service-value-prop-what-is.md
[Kullanmaya Başlama]: app-service-mobile-ios-get-started.md
[Azure Table Storage]: ../storage/storage-getting-started-guide.md
[DocumentDB]: ../documentdb/documentdb-get-started.md
[Kimlik doğrulama özelliklerimiz]: ./app-service-mobile-auth.md
[Veri özellikleri]: ./app-service-mobile-offline-data-sync.md
[Anında iletme bildirimi özelliklerimiz]: ../notification-hubs/notification-hubs-push-notification-overview.md
[iOS]: ./app-service-mobile-ios-how-to-use-client-library.md
[Android]: ./app-service-mobile-android-how-to-use-client-library.md
[Windows]: ./app-service-mobile-dotnet-how-to-use-client-library.md
[iOS ve Android için Xamarin]: ./app-service-mobile-dotnet-how-to-use-client-library.md
[Xamarin Forms]: ./app-service-mobile-xamarin-forms-get-started.md
[Apache Cordova]: ./app-service-mobile-cordova-how-to-use-client-library.md
[Otomatik Ölçeklendirme]: ../app-service-web/web-sites-scale.md
[Hazırlık Ortamları]: ../app-service-web/web-sites-staged-publishing.md
[Dağıtım seçenekleri]: ../app-service-web/web-sites-deploy.md
[Karma bağlantılar]: ../app-service-web/web-sites-hybrid-connection-get-started.md
[sanal ağlar]: ../app-service-web/web-sites-integrate-with-vnet.md
[ExpressRoute]: ../app-service/app-service-app-service-environment-network-configuration-expressroute.md
[App Service ortamları]: ../app-service-web/app-service-app-service-environment-intro.md
[öğrenme haritamızı]: https://azure.microsoft.com/en-us/documentation/learning-paths/appservice-mobileapps/



<!--HONumber=sep16_HO1-->


