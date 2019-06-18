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
ms.topic: conceptual
ms.date: 10/01/2016
ms.author: crdun
ms.openlocfilehash: f3eb781e7f84e8cf03a975f7cb77f6a7ef074d44
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60861388"
---
# <a name="getting-started"> </a>Azure Uygulama Hizmetinde Mobile Apps Hakkında
Azure Uygulama Hizmeti, profesyonel geliştiricilere yönelik tam olarak yönetilen bir [hizmet olarak platform](https://azure.microsoft.com/overview/what-is-paas/) (PaaS) teklifidir. Bu hizmet, web, mobil ve tümleştirme senaryoları için zengin bir özellik kümesi sağlar. 

Azure Uygulama Hizmeti’ndeki Mobile Apps özelliği, kurumsal geliştiriciler ve sistem entegratörleri için yüksek düzeyde ölçeklenebilir ve küresel olarak kullanılabilir bir platformdur.

![Mobile Apps özelliklerine görsel bir genel bakış](./media/app-service-mobile-value-prop/overview.png)

## <a name="why-mobile-apps"></a>Neden Mobile Apps?
Mobile Apps özelliği ile şunları yapabilirsiniz:

* **Yerel ve platformlar arası uygulamalar oluşturma**: Yerel iOS, Android ve Windows oluşturmakta olduğunuz platformlar arası Xamarin veya Cordova (PhoneGap) uygulamaları, yerel SDK'ları kullanarak App Service'in avantajlarından faydalanabilirsiniz.
* **Kuruluş sistemlerinizi bağlama**: Mobile Apps özelliği ile dakikalar için Kurumsal oturum ekleyebilir ve kuruluşunuzu şirket içi için bağlanmak veya Bulut kaynaklarına.
* **Veri Eşitleme ile çevrimdışı kullanılmaya hazır uygulamalar derleme**: Mobil iş gücünüzü daha üretken çevrimdışı çalışma ve bağlantı (SaaS) Apı'leriniz hizmet olarak tüm kurumsal veri kaynaklarınız ya yazılım bulunduğunda arka planda verileri eşitlemek amacıyla Mobile Apps kullanan uygulamalar oluşturarak yapar.
* **Saniyeler içinde milyonlarca anında iletme bildirimleri**: Kendi ihtiyaçlarına özel ve zaman doğru gönderilen tüm cihazlarda anında anında iletme bildirimleri müşterilerinizle etkileşim kurun.

## <a name="mobile-apps-features"></a>Mobile Apps özellikleri
Aşağıdaki özellikler, bulut etkin mobil geliştirme için önemlidir:

* **Kimlik doğrulama ve yetkilendirme**: Azure Active Directory dahil, Kurumsal kimlik, kimlik sağlayıcıları gibi sosyal sağlayıcılardan Facebook, Google, Twitter ve Microsoft hesapları için destek. Mobile Apps tüm sağlayıcılar için bir OAuth 2.0 hizmeti sunar. Ayrıca sağlayıcıya özel işlev için kimlik sağlayıcısına SDK tümleştirebilirsiniz.

    [Kimlik doğrulama özelliklerimiz] hakkında daha fazlasını keşfedin.

* **Veri erişimi**: Mobile Apps, Azure SQL veritabanı veya şirket içi SQL server bağlı bir mobil aygıt dostu bir OData v3 veri kaynağı sağlar. Bu hizmet, [Azure Tablo depolama], MongoDB ve [Azure Cosmos DB]’nin yanı sıra Office 365 ve Salesforce.com gibi SaaS API’si sağlayıcıları dahil, diğer NoSQL ve SQL veri sağlayıcılarıyla kolayca tümleştirmenizi sağlayarak Entity Framework’ü temel alabilir.

* **Çevrimdışı eşitleme**: İstemci Sdk'leri çevrimdışı bir veri kümesi ile çalışan sağlam ve esnek mobil uygulamalar oluşturmanızı kolaylaştırır. Bu veri kümesini, çakışma çözümü desteği de dahil olmak üzere arka uç verileriyle otomatik olarak eşitleyebilirsiniz.

  [Veri özellikleri] hakkında daha fazlasını keşfedin.

* **Anında iletme bildirimleri**: Aynı anda milyonlarca kullanıcıya anında iletme bildirimleri gönderebilmek için İstemci SDK'ları, Azure Notification Hubs'ın kayıt özellikleriyle sorunsuz şekilde tümleştirin.

  [Anında iletme bildirimi özellikleri] hakkında daha fazlasını keşfedin.

* **İstemci SDK'ları**: Yerel geliştirme kapsayan istemci SDK'ların eksiksiz bir kümesini yoktur ([iOS], [Android], ve [Windows]), platformlar arası geliştirmeyi ([Xamarin.iOS ve Xamarin.Android], [Xamarin.Forms]) ve karma uygulama geliştirmeyi ([Apache Cordova]). Her istemci SDK’sı ile bir MIT lisansı ile birlikte sunulur ve açık kaynaklıdır.

## <a name="azure-app-service-features"></a>Azure Uygulama Hizmeti özellikleri
Aşağıdaki platform özellikleri mobil üretim siteleri için yararlıdır:

* **Otomatik ölçeklendirme**: App Service ile hızlı bir şekilde ölçeği büyütün veya herhangi bir gelen müşteri yükünün tamamını karşılamak için ölçeği genişletme. Yük ya da zamanlama temelinden mobil uygulamanızın arka ucunu ölçeklendirmek için VM’nin sayısını ya da boyutunu el ile seçin ya da otomatik ölçeklendirmeyi ayarlayın.

  [Otomatik ölçeklendirme] hakkında daha fazlasını keşfedin.

* **Hazırlama ortamları**: App Service, birden fazla sürümünü çalıştırabilir, sitenizin bir gerçekleştirebilmesi için a / B testini, daha büyük bir DevOps planının parçası olarak ve yeni bir arka ucu, yerinde.

  [hazırlık ortamları] hakkında daha fazlasını keşfedin.

* **Sürekli dağıtım**: App Service, ortak ile tümleştirilebilir _kaynak denetimi Yönetimi_ (SCM) sistemleri, arka ucunuza yeni bir sürümünü kolayca dağıtmanızı sağlar.

  [dağıtım seçenekleri](../app-service/deploy-local-git.md) hakkında daha fazlasını keşfedin.

* **Sanal ağ**: App Service, sanal ağ, Azure ExpressRoute ya da karma bağlantılar kullanarak şirket içi kaynaklara bağlanabilir.

  [karma bağlantılar], [sanal ağlar], ve [ExpressRoute] hakkında daha fazlasını keşfedin.

* **Yalıtılmış ve ayrılmış ortamlar**: Azure App Service uygulamalarını güvenli bir şekilde çalıştırmak için App Service tam yalıtılmış ve ayrılmış bir ortamda çalıştırabilirsiniz. Bu ortam, büyük ölçekli, yalıtım veya güvenli ağ erişimi gerektiren uygulama iş yükleri için idealdir.

  [App Service ortamları] hakkında daha fazlasını öğrenin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Uygulama Hizmeti'nde Mobile Apps kullanmaya başlamak için [başlarken] öğreticisini tamamlayın. Bu öğretici, tercih ettiğiniz mobil arka ucu ve istemciyi oluşturma konusunda temel kavramları kapsar. Ayrıca kimlik doğrulama, çevrimdışı eşitleme ve anında iletme bildirimlerini tümleştirme konularını ele alır. Öğreticiyi her istemci uygulaması için birden çok kez tamamlayabilirsiniz.

Mobile Apps hakkında daha fazla bilgi için [öğrenme haritamızı] gözden geçirin.
Azure Uygulama Hizmeti platformu hakkında daha fazla bilgi için bkz. [Azure App Service].

<!-- URLs. -->
[Migrate your mobile service to App Service]: app-service-mobile-migrating-from-mobile-services.md
[başlarken]: app-service-mobile-ios-get-started.md
[Azure Tablo Depolama]:../cosmos-db/table-storage-how-to-use-dotnet.md
[Azure Cosmos DB]: ../cosmos-db/sql-api-get-started.md
[Kimlik doğrulama özelliklerimiz]: ./app-service-mobile-auth.md
[veri özellikleri]: ./app-service-mobile-offline-data-sync.md
[anında iletme bildirimi özellikleri]: ../notification-hubs/notification-hubs-push-notification-overview.md
[iOS]: ./app-service-mobile-ios-how-to-use-client-library.md
[Android]: ./app-service-mobile-android-how-to-use-client-library.md
[Windows]: ./app-service-mobile-dotnet-how-to-use-client-library.md
[Xamarin.iOS ve Xamarin.Android]: ./app-service-mobile-dotnet-how-to-use-client-library.md
[Xamarin.Forms]: ./app-service-mobile-xamarin-forms-get-started.md
[Apache Cordova]: ./app-service-mobile-cordova-how-to-use-client-library.md
[otomatik ölçeklendirme]: ../app-service/web-sites-scale.md
[hazırlık ortamları]: ../app-service/deploy-staging-slots.md
[karma bağlantılar]: ../biztalk-services/integration-hybrid-connection-overview.md
[sanal ağlar]: ../app-service/web-sites-integrate-with-vnet.md
[ExpressRoute]: ../app-service/environment/app-service-app-service-environment-network-configuration-expressroute.md
[App Service ortamları]: ../app-service/environment/intro.md
[öğrenme haritamızı]: https://azure.microsoft.com/documentation/learning-paths/appservice-mobileapps/
