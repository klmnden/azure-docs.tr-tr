---
title: Azure Uygulama Hizmetinde Mobile Apps Hakkında
description: App Service’in kurumsal mobil uygulamalarınıza sağladığı avantajları öğrenin.
services: app-service\mobile
documentationcenter: ''
author: conceptdev
manager: yochayk
editor: ''
ms.assetid: 4e96cb9d-a632-4cf6-8219-0810d8ade3f9
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-multiple
ms.devlang: na
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: crdun
ms.openlocfilehash: e84ac98508b791b4617ead2b6bf3b0edc549bdb6
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="getting-started"> </a>Azure Uygulama Hizmetinde Mobile Apps Hakkında
Azure Uygulama Hizmeti, profesyonel geliştiricilere yönelik tam olarak yönetilen bir [hizmet olarak platform](https://azure.microsoft.com/overview/what-is-paas/) (PaaS) teklifidir. Bu hizmet, web, mobil ve tümleştirme senaryoları için zengin bir özellik kümesi sağlar. 

Azure Uygulama Hizmeti’ndeki Mobile Apps özelliği, kurumsal geliştiriciler ve sistem entegratörleri için yüksek düzeyde ölçeklenebilir ve küresel olarak kullanılabilir bir platformdur.

![Mobile Apps özelliklerine görsel bir genel bakış](./media/app-service-mobile-value-prop/overview.png)

## <a name="why-mobile-apps"></a>Neden Mobile Apps?
Mobile Apps özelliği ile şunları yapabilirsiniz:

* **Yerel ve platformlar arası uygulamaları oluşturma**: Hem yerel iOS, Android ve Windows uygulamaları hem de platformlar arası Xamarin veya Cordova (Phonegap) uygulamaları oluştururken yerel SDK'ları kullanarak App Service’in avantajlarından faydalanabilirsiniz.
* **Kuruluş sistemlerinizi bağlama**: Mobile Apps ile dakikalar için kurumsal oturum ekleyebilir ve kuruluşunuzu şirket içi ya da bulut kaynaklarına bağlayabilirsiniz.
* **Veri eşitleme ile çevrimdışı kullanılmaya hazır uygulamalar oluşturma**: Kurumsal veri kaynaklarınız ya da hizmet olarak yazılım (SaaS) API’leriniz ile bağlantı bulunduğunda arka planda verileri eşitlemek amacıyla çevrimdışı çalışan ve Mobile Apps kullanan uygulamalar oluşturarak mobil iş gücünüzü verimli kılın.
* **Milyonlarca kişiye anında iletme bildirimleri**: Her cihazda, kendi ihtiyaçlarına özel ve doğru zamanda gönderilen anında iletme bildirimleriyle müşterilerinizle etkileşim kurun.

## <a name="mobile-apps-features"></a>Mobile Apps özellikleri
Aşağıdaki özellikler, bulut etkin mobil geliştirme için önemlidir:

* **Kimlik doğrulama ve yetkilendirme**: Kurumsal kimlik doğrulama için Azure Active Directory’nin dahil olduğu kimlik sağlayıcılara ek olarak Facebook, Google, Twitter ve Microsoft Hesabı gibi sosyal sağlayıcılara yönelik destek. Mobile Apps tüm sağlayıcılar için bir OAuth 2.0 hizmeti sunar. Ayrıca sağlayıcıya özel işlev için kimlik sağlayıcısına SDK tümleştirebilirsiniz.

    [Kimlik doğrulama özellikleri] hakkında daha fazlasını keşfedin.

* **Veri erişimi**: Azure Mobile Apps, Azure SQL Veritabanı’na ya da şirket içi bir SQL Sunucusu’na bağlı, mobil kullanıma uygun bir OData v3 veri kaynağı sağlar. Bu hizmet, [Azure Tablo depolama], MongoDB ve [Azure Cosmos DB]’nin yanı sıra Office 365 ve Salesforce.com gibi SaaS API’si sağlayıcıları dahil, diğer NoSQL ve SQL veri sağlayıcılarıyla kolayca tümleştirmenizi sağlayarak Entity Framework’ü temel alabilir.

* **Çevrimdışı eşitleme**: İstemci SDK’leri çevrimdışı bir veri kümesi ile çalışan sağlam ve esnek mobil uygulamalar oluşturmanızı kolaylaştırır. Bu veri kümesini, çakışma çözümü desteği de dahil olmak üzere arka uç verileriyle otomatik olarak eşitleyebilirsiniz.

  [Veri özellikleri] hakkında daha fazlasını keşfedin.

* **Anında İletme Bildirimleri**: İstemci SDK'leri, aynı anda milyonlarca kullanıcıya anında iletme bildirimleri göndermenizi sağlayarak, Azure Notification Hubs'ın kayıt özellikleriyle sorunsuz şekilde tümleşir.

  [Anında iletme bildirimi özellikleri] hakkında daha fazlasını keşfedin.

* **İstemci SDK'ları**: Yerel geliştirmeyi ([iOS], [Android] ve [Windows]), platformlar arası geliştirmeyi ([Xamarin.iOS ve Xamarin.Android], [Xamarin.Forms]) ve karma uygulama geliştirmeyi ([Apache Cordova]) kapsayan istemci SDK'larının eksiksiz bir kümesini sunuyoruz. Her istemci SDK’sı ile bir MIT lisansı ile birlikte sunulur ve açık kaynaklıdır.

## <a name="azure-app-service-features"></a>Azure Uygulama Hizmeti özellikleri
Aşağıdaki platform özellikleri mobil üretim siteleri için yararlıdır:

* **Otomatik ölçeklendirme**: App Service’i kullanarak gelen müşteri yükünü işlemek için hızlı şekilde ölçeği artırabilir ya da genişletebilirsiniz. Yük ya da zamanlama temelinden mobil uygulamanızın arka ucunu ölçeklendirmek için VM’nin sayısını ya da boyutunu el ile seçin ya da otomatik ölçeklendirmeyi ayarlayın.

  [Otomatik ölçeklendirme] hakkında daha fazlasını keşfedin.

* **Hazırlık ortamları**: App Service, yeni arka ucun daha büyük bir DevOps planının parçası olarak test olan A/B testini ve yerinde hazırlanmasını gerçekleştirmenizi sağlayarak, sitenizin birden fazla sürümünü çalıştırabilir.

  [hazırlık ortamları] hakkında daha fazlasını keşfedin.

* **Sürekli dağıtım**: App Service, ortak _kaynak denetimi yönetimi_ (SCM) sistemleriyle tümleştirilerek, arka ucunuzun yeni bir sürümünü kolayca dağıtmanızı sağlar.

  [dağıtım seçenekleri](../app-service/app-service-deploy-local-git.md) hakkında daha fazlasını keşfedin.

* **Sanal Ağ**: App Service; sanal ağ, Azure ExpressRoute ya da karma bağlantılar kullanarak şirket içi kaynaklara bağlanabilir.

  [karma bağlantılar], [sanal ağlar], ve [ExpressRoute] hakkında daha fazlasını keşfedin.

* **Yalıtılmış ve ayrılmış ortamlar**: Azure App Service uygulamalarını güvenli bir şekilde çalıştırmak için, App Service’i tam yalıtılmış ve ayrılmış bir ortamda çalıştırabilirsiniz. Bu ortam, büyük ölçekli, yalıtım veya güvenli ağ erişimi gerektiren uygulama iş yükleri için idealdir.

  [App Service ortamları] hakkında daha fazlasını öğrenin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Uygulama Hizmeti'nde Mobile Apps kullanmaya başlamak için [başlarken] öğreticisini tamamlayın. Bu öğretici, tercih ettiğiniz mobil arka ucu ve istemciyi oluşturma konusunda temel kavramları kapsar. Ayrıca kimlik doğrulama, çevrimdışı eşitleme ve anında iletme bildirimlerini tümleştirme konularını ele alır. Öğreticiyi her istemci uygulaması için birden çok kez tamamlayabilirsiniz.

Mobile Apps hakkında daha fazla bilgi için [öğrenme haritamızı] gözden geçirin.
Azure Uygulama Hizmeti platformu hakkında daha fazla bilgi için bkz. [Azure App Service].

<!-- URLs. -->
[Migrate your mobile service to App Service]: app-service-mobile-migrating-from-mobile-services.md
[başlarken]: app-service-mobile-ios-get-started.md
[Azure Tablo depolama]:../cosmos-db/table-storage-how-to-use-dotnet.md
[Azure Cosmos DB]: ../cosmos-db/sql-api-get-started.md
[Kimlik doğrulama özellikleri]: ./app-service-mobile-auth.md
[Veri özellikleri]: ./app-service-mobile-offline-data-sync.md
[Anında iletme bildirimi özellikleri]: ../notification-hubs/notification-hubs-push-notification-overview.md
[iOS]: ./app-service-mobile-ios-how-to-use-client-library.md
[Android]: ./app-service-mobile-android-how-to-use-client-library.md
[Windows]: ./app-service-mobile-dotnet-how-to-use-client-library.md
[Xamarin.iOS ve Xamarin.Android]: ./app-service-mobile-dotnet-how-to-use-client-library.md
[Xamarin.Forms]: ./app-service-mobile-xamarin-forms-get-started.md
[Apache Cordova]: ./app-service-mobile-cordova-how-to-use-client-library.md
[Otomatik ölçeklendirme]: ../app-service/web-sites-scale.md
[hazırlık ortamları]: ../app-service/web-sites-staged-publishing.md
[karma bağlantılar]: ../biztalk-services/integration-hybrid-connection-overview.md
[sanal ağlar]: ../app-service/web-sites-integrate-with-vnet.md
[ExpressRoute]: ../app-service/environment/app-service-app-service-environment-network-configuration-expressroute.md
[App Service ortamları]: ../app-service/environment/intro.md
[öğrenme haritamızı]: https://azure.microsoft.com/documentation/learning-paths/appservice-mobileapps/
