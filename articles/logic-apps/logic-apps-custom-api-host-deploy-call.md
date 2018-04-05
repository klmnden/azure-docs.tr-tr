---
title: Dağıtma ve Azure mantığı uygulamalardan web API'leri & REST API çağrısı | Microsoft Docs
description: Dağıtma ve API'leri & REST API'leri web sistemi için Azure Logic Apps tümleştirme akışlarında çağırın.
keywords: Web API'leri, REST API'leri, bağlayıcılar, iş akışları, sistem tümleştirmeler, kimlik doğrulaması
services: logic-apps
author: stepsic-microsoft-com
manager: anneta
editor: ''
documentationcenter: ''
ms.assetid: f113005d-0ba6-496b-8230-c1eadbd6dbb9
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: LADocs; stepsic
ms.openlocfilehash: c7a240bf5b7ed5e7780b90f438d2e336ee79f0b3
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="deploy-and-call-custom-apis-from-logic-app-workflows"></a>Dağıtma ve uygulama iş akışları mantığından özel API'leri çağırmak

Çalıştırdıktan sonra [özel API oluşturma](./logic-apps-create-api-app.md) mantığı uygulama iş akışlarında kullanım için bunları çağırmadan önce Apı'lerinizi dağıtmanız gerekir. API olarak dağıtabilir miyim [web uygulamaları](../app-service/app-service-web-overview.md), ancak API olarak dağıtmayı göz önünde bulundurun [API uygulamaları](../app-service/app-service-web-tutorial-rest-api.md), hangi olun işinizi daha kolay yapı, ana bilgisayar ve API'leri bulutta ve şirket içi kullanabilir. Tüm Apı'lerinizi kodda değişiklik - yalnızca kodunuzu bir API uygulamasına dağıtmak yok. Üzerinde Apı'lerinizi barındırabilir [Azure App Service](../app-service/app-service-web-overview.md), bir platform olarak-sağlayan yüksek düzeyde ölçeklenebilir, kullanımı kolay API barındırma sunan hizmet (PaaS).

Bir mantıksal uygulama, en iyi deneyim için herhangi bir API'yi çağırabilirsiniz rağmen eklemek [OpenAPI (daha önce Swagger) meta veri](http://swagger.io/specification/) , API'nin işlemlerini ve parametrelerini açıklar. Bu OpenAPI dosya API'nizi daha kolay tümleştirme ve daha iyi logic apps ile çalışmanıza yardımcı olur.

## <a name="deploy-your-api-as-a-web-app-or-api-app"></a>API'nizi bir web uygulaması veya API uygulaması olarak dağıtma

Özel API'nizi mantığı uygulamadan aramadan önce Azure App Service'e API'nizi bir web uygulaması veya API uygulaması olarak dağıtın. Ayrıca, Logic Apps tasarımcı tarafından okunabilir dosya, OpenAPI yapmak için API tanımı özelliklerini ayarlamak ve etkinleştirmek [çıkış noktaları arası kaynak paylaşımı (CORS)](../app-service/app-service-web-overview.md) web uygulaması veya API uygulaması için.

1. İçinde [Azure portal](https://portal.azure.com), web uygulaması veya API uygulaması seçin.

2. Açılır ve altında uygulama menüsünden **API**, seçin **API tanımı**. Ayarlama **API tanımı konumu** OpenAPI swagger.json dosyanızı URL'si.

   Genellikle, URL şöyle görünür: `https://{name}.azurewebsites.net/swagger/docs/v1)`

   ![Özel API için OpenAPI dosyasına bağlantı](./media/logic-apps-custom-api-deploy-call/custom-api-swagger-url.png)

3. Altında **API**, seçin **CORS**. CORS İlkesi'ni ayarlamak **çıkış izin** için **' *'** (tüm izin ver).

   Bu ayar mantığı Uygulama Tasarımcısı'ndan istekleri verir.

   ![İstekleri mantığı Uygulama Tasarımcısı'ndan özel API'nizi izin verir.](./media/logic-apps-custom-api-deploy-call/custom-api-cors.png)

Daha fazla bilgi için bkz: [bir RESTful API'sini Azure App Service CORS ile ana bilgisayar](../app-service/app-service-web-tutorial-rest-api.md).

## <a name="call-your-custom-api-from-logic-app-workflows"></a>Özel API'nizi mantığından uygulama iş akışları çağırın.

API tanımı özellikleri ve CORS ayarladıktan sonra özel API'nin tetikleyiciler ve Eylemler, logic app akışınıza eklemek kullanılabilir olması gerekir. 

*  OpenAPI URL'lere sahip Web siteleri görüntülemek için Logic Apps Tasarımcısı'nda, abonelik Web sitelerine göz atabilirsiniz.

*  Bir OpenAPI belge göstererek kullanılabilir eylemler ve girişleri görüntülemek için kullanın [HTTP + Swagger eylem](../connectors/connectors-native-http-swagger.md).

*  Yok veya bir OpenAPI belge kullanıma API'leri dahil olmak üzere herhangi bir API'yi çağırmak için her zaman bir istekle oluşturabilirsiniz [HTTP eylemi](../connectors/connectors-native-http.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Özel bağlayıcı genel bakış](../logic-apps/custom-connector-overview.md)