---
title: "Azure Uygulama Hizmeti nasıl çalışır?"
description: "App Service’in nasıl çalıştığını öğrenin"
keywords: "app service, azure app service, ölçek, ölçeklenebilir, app service planı, app service maliyeti"
services: app-service
documentationcenter: 
author: yochay
manager: erikre
editor: 
ms.assetid: ae74fc32-969e-4580-8d61-02c922f1f184
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 02/23/2017
ms.author: yochayk
translationtype: Human Translation
ms.sourcegitcommit: edb3325414adf876548181243ddfa2d515aeb0b8
ms.openlocfilehash: 2d830963d3d2adba71a6ca99f79eac0fc8cbfb12
ms.lasthandoff: 02/24/2017


---
# <a name="how-app-service-works"></a>App Service nasıl çalışır?
Azure Uygulama Hizmeti, günümüzde mühendislerin karşılaştığı pratik sorunları çözmek için tasarlanmış bir bulut hizmetidir.
App Service, bulut ölçeğinde uygulama sağlama ihtiyacından ödün vermeden üstün geliştirici verimliliği sağlamaya odaklanır. 

App Service kurumsal iş uygulamaları oluşturmak için gerekli olan özellikleri ve çerçeveleri de sunar. App Service Java, PHP, Node.js, Python ve Microsoft .NET dilleri dahil olmak üzere en popüler geliştirme dillerinde uygulama geliştirmenizi sağlar. App Service ile yapabilecekleriniz:

* Yüksek oranda ölçeklenebilir web uygulamaları oluşturma.
* Veri arka uçları, kullanıcı kimlik doğrulaması ve anında iletme bildirimleri gibi kullanımı kolay mobil özellik gruplarıyla hızlı şekilde Mobile Apps arka uçları oluşturma.
* API Apps ile API’leri uygulama, dağıtma ve yayımlama.
* İş uygulamalarını iş akışlarında birbirine bağlayın ve Logic Apps ile verileri dönüştürün.

> [!INCLUDE [app-service-linux](../../includes/app-service-linux.md)]
> 
> 

Tüm uygulama türleri, geliştiricilere uygulama tasarımından uygulama bakımına en iyi duruma getirilmiş tam yaşam deneyimi sağlayan ölçeklenebilir ve esnek Web Apps platformuna dayanır. Yaşam döngüsü özellikleri aşağıdakileri mümkün kılar:

* **Hızlı Uygulama oluşturma**. Baştan başlayın veya Azure Market'ten bir işletimsel destek sistemi (OSS) paketi seçin.
* **Sürekli dağıtım**. TFS, GitHub ve Bitbucket gibi popüler kaynak denetimlerinden otomatik olarak yeni kod dağıtın ve OneDrive ve Dropbox gibi çevrimiçi depolama hizmetlerinde içerik eşitleyin.
* **Üretim ortamında test**. Sorunsuz üretim öncesi ortamlar oluşturun ve bunlara giden trafik miktarını yönetin. Gerektiğinde bulutta hata ayıklayın ve sorun bulunduğunda geri alın.
* **Zaman uyumsuz görevler ve toplu işler çalıştırma**. Arka planda kod çalıştırın ya da kodunuzu olaylar (örneğin, Azure Storage kuyruğuna gelen iletiler) zamanlanan süreleri (CRON) temel alarak etkinleştirin.
* **Uygulamayı ölçeklendirme**. Trafik ve kaynak kullanımı temelinde hizmetinizi otomatik olarak yatay ve dikey olarak ölçeklendirmek için birçok seçenekten birini kullanın. Uygulamalarınıza özgü özel ortamları yapılandırma.   
* **Uygulama koruma**. Sorunlara hazırlıksız yakalanmamak ve bunları gerçek zamanlı olarak (otomatik düzeltme ve dinamik hata ayıklama gibi) ya da günlükleri ve bellek dökümlerini çözümledikten sonra etkin şekilde çözmek için birçok hata ayıklama ve tanımlama özelliğini kullanın.

Bir bütün olarak bakıldığında, App Service özellikleri geliştiricilerin kendi kodlarına odaklanmasını ve kararlı, yüksek düzeyde ölçeklenebilir üretim durumuna hızlı şekilde ulaşmasını sağlar. API Apps ve Logic Apps özellikleri sayesinde geliştiriciler iş çözümlerinin yanı sıra şirket içi çözümler ile bulut tümleştirme arasındaki engeller için köprü oluşturarak gerçek kurumsal uygulamalar oluşturabilir. 

## <a name="videos"></a>Videolar
* [Azure Uygulama Hizmeti Mimarisi](https://azure.microsoft.com/documentation/videos/why-azure-web-sites-plus-architecture/)

## <a name="next-steps"></a>Sonraki adımlar

App Service hakkında daha fazla bilgi edinmek için aşağıdaki konu başlıklarına bakın:

* [Azure Uygulama Hizmeti nedir?](app-service-value-prop-what-is.md)
  * [Web Uygulaması](../app-service-web/app-service-web-overview.md)
  * [Mobil uygulama](../app-service-mobile/app-service-mobile-value-prop.md)
  * [API Uygulaması](../app-service-api/app-service-api-apps-why-best-platform.md)
* [Azure Uygulama Hizmeti Mimarisi (sunu)](http://www.slideshare.net/maartenba/windows-azure-web-sites-things-they-dont-teach-kids-in-school-comunity-day-2013)
* [Azure Uygulama Hizmeti, Cloud Services ve Sanal Makineler karşılaştırması](../app-service-web/choose-web-site-cloud-service-vm.md)
* [App Service Planları’nı anlama](azure-web-sites-web-hosting-plans-in-depth-overview.md)
* [App Service Ortamı’na giriş](../app-service-web/app-service-app-service-environment-intro.md)
  * [Alıştırma: App Service Ortamı oluşturma](../app-service-web/app-service-web-how-to-create-an-app-service-environment.md)
* [Azure Uygulama Hizmeti Geliştirme Yığınları Desteği](https://azure.microsoft.com/blog/windows-azure-websites-development-stacks-support/)




