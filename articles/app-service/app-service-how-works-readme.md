---
title: "Azure Uygulama Hizmeti nasıl çalışır?"
description: "App Service’in nasıl çalıştığını öğrenin"
keywords: "app service, azure app service, ölçek, ölçeklenebilir, app service planı, app service maliyeti"
services: app-service
documentationcenter: 
author: yochay
manager: wpickett
editor: 
ms.assetid: ae74fc32-969e-4580-8d61-02c922f1f184
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 02/10/2016
ms.author: yochayk
translationtype: Human Translation
ms.sourcegitcommit: 6adb1dd25c24b18b834dd921c2586ef29d56dc81
ms.openlocfilehash: d1ab1ab2132d12bf06dbe2504b7d7111ef2ae851


---
# <a name="how-app-service-works"></a>App Service nasıl çalışır?
Azure Uygulama Hizmeti, günümüzde mühendislerin karşılaştığı pratik sorunları çözmek için tasarlanmış bir bulut hizmetidir.
App Service, bulut ölçeğinde uygulama sağlama ihtiyacından ödün vermeden üstün geliştirici verimliliği sağlamaya odaklanır.

App Service ayrıca, geliştiricileri en popüler geliştirme dilleri (Microsoft .NET, Java, PHP, Node.js ve Python gibi) desteği ile buluştururken, kurumsal iş kolu uygulamaları oluşturmak için gereken özellik ve çerçeveleri sağlar.
App Service ile geliştiriciler şunları yapabilir:

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
* **Üretim ortamında test**. Sorunsuz üretim öncesi ortamlar oluşturun ve bunlara giden trafik miktarını yönetin. Gerektiğinde bulutta hata ayıklayın ve sorun bulunursa geri alın.
* **Zaman uyumsuz görevler ve toplu işler çalıştırma**. Arka planda kod çalıştırın ya da kodunuzu olaylar (örneğin, Azure Storage kuyruğuna gelen iletiler) zamanlanan süreleri (CRON) temel alarak etkinleştirin.
* **Uygulamayı ölçeklendirme**. Trafik ve kaynak kullanımı temelinde hizmetinizi otomatik olarak yatay ve dikey olarak ölçeklendirmek için birçok seçenekten birini kullanın. Uygulamalarınıza özgü özel ortamları yapılandırma.   
* **Uygulama koruma**. Sorunlara hazırlıksız yakalanmamak ve bunları gerçek zamanlı olarak (otomatik düzeltme ve dinamik hata ayıklama gibi) ya da günlükleri ve bellek dökümlerini çözümledikten sonra etkin şekilde çözmek için birçok hata ayıklama ve tanımlama özelliğini kullanın.

Bir bütün olarak bakıldığında, App Service özellikleri geliştiricilerin kendi kodlarına odaklanmasını ve kararlı, yüksek düzeyde ölçeklenebilir üretim durumuna hızlı şekilde ulaşmasını sağlar. API Apps ve Logic Apps özellikleri sayesinde geliştiriciler iş çözümlerinin yanı sıra şirket içi çözümler ile bulut tümleştirme arasındaki engeller için köprü oluşturarak gerçek kurumsal uygulamalar oluşturabilir.  

[!INCLUDE [app-service-blueprint-how-app-service-works](../../includes/app-service-blueprint-how-app-service-works.md)]




<!--HONumber=Dec16_HO2-->


