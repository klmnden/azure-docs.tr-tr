---
title: Dağıtma ve Azure Logic Apps'ten web API'leri ve REST API'leri çağırma | Microsoft Docs
description: Dağıtma ve web API'leri ve REST API'leri için sistemi integratio Azure Logic Apps iş akışlarını çağırma
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, stepsic, LADocs
ms.topic: article
ms.assetid: f113005d-0ba6-496b-8230-c1eadbd6dbb9
ms.date: 05/26/2017
ms.openlocfilehash: a9049ba1fbd7d3bdce061d277f6a7a02d9b1e4b7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60740413"
---
# <a name="deploy-and-call-custom-apis-from-workflows-in-azure-logic-apps"></a>Dağıtma ve Azure Logic Apps iş akışlarında öğesinden özel API'ler çağırma

Çalıştırdıktan sonra [özel API'ler oluşturma](./logic-apps-create-api-app.md) mantıksal uygulama iş akışları içinde kullanım için çağrı yapmadan önce Apı'lerinizi dağıtmanız gerekir. Apı'lerinizi olarak dağıtabileceğiniz [web uygulamaları](../app-service/overview.md), ancak Apı'lerinizi olarak dağıtmayı göz önünde bulundurun [API apps](../app-service/app-service-web-tutorial-rest-api.md), hangi olmak işinizi daha kolay oluşturacağınızı, barındıracağınızı ve API'leri bulutta ve şirket içi olduğunda. Apı'lerinizi herhangi bir kod değişikliği - yalnızca kodunuzu API uygulamasına dağıtma yok. Apı'lerinizi barındırmak [Azure App Service](../app-service/overview.md), bir platform-a sağlayan yüksek düzeyde ölçeklenebilir, kolay API'sini barındıran sunan hizmet olarak (PaaS).

En iyi deneyim için herhangi bir API'yi bir mantıksal uygulamadan çağırabilirsiniz olsa da, ekleme [Openapı (daha önce Swagger) meta verileri](https://swagger.io/specification/) API'NİZİN işlemlerini ve parametrelerini açıklar. Bu Openapı dosyasını API'nizi daha kolay bir şekilde tümleştirin ve logic apps ile daha iyi çalışmasına yardımcı olur.

## <a name="deploy-your-api-as-a-web-app-or-api-app"></a>API'nizi API uygulaması veya web uygulaması dağıtma

API'nizi API uygulaması veya web uygulaması olarak, bir mantıksal uygulamadan özel API'nizi çağrı yapmadan önce Azure App Service'e dağıtın. Ayrıca, Logic Apps tasarımcısı tarafından okunabilir dosyası, Openapı yapmak için API tanımı özellikleri ayarlamak ve etkinleştirmek [çıkış noktaları arası kaynak paylaşımı (CORS)](../app-service/overview.md) , web veya API uygulaması.

1. İçinde [Azure portalında](https://portal.azure.com), web uygulamanızı veya API uygulaması seçin.

2. Altında açılan uygulama menüsündeki **API**, seçin **API tanımı**. Ayarlama **API tanımının konumu** Openapı swagger.json dosyanızı URL'si.

   Genellikle, URL şöyle görünür: `https://{name}.azurewebsites.net/swagger/docs/v1)`

   ![Özel API'niz için Openapı dosyasına bağlantı](./media/logic-apps-custom-api-deploy-call/custom-api-swagger-url.png)

3. Altında **API**, seçin **CORS**. İçin CORS ilkesini ayarlama **çıkış noktaları** için **' *'** (tüm izin verir).

   Bu ayar, mantıksal Uygulama Tasarımcısı'ndan istekleri verir.

   ![İstekleri, özel API'nizi mantıksal Uygulama Tasarımcısı'ndan izin ver](./media/logic-apps-custom-api-deploy-call/custom-api-cors.png)

Daha fazla bilgi için [Azure App Service'te CORS ile RESTful API barındırma](../app-service/app-service-web-tutorial-rest-api.md).

## <a name="call-your-custom-api-from-logic-app-workflows"></a>Özel API'nizi mantığından uygulama iş akışları çağırın.

API tanımı özellikleri ve CORS ayarlama sonra özel API'NİZİN tetikleyiciler ve Eylemler, mantıksal uygulama iş akışınızı dahil etmek için kullanılabilir olması gerekir. 

*  Openapı URL'lerinin Web sitelerini görüntülemek için Logic Apps Tasarımcısı'nda abonelik sitelerinizi göz atabilirsiniz.

*  Kullanılabilir eylemler ve girişler bir Openapı belgesini işaret ederek görüntülemek için kullanın [da HTTP + Swagger eylem](../connectors/connectors-native-http-swagger.md).

*  Veya yoksa bir Openapı belgesi kullanıma API'ler dahil olmak üzere herhangi bir API'yi çağırmak için her zaman bir istekle oluşturabilirsiniz [HTTP eylemi](../connectors/connectors-native-http.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Özel bağlayıcıya genel bakış](../logic-apps/custom-connector-overview.md)