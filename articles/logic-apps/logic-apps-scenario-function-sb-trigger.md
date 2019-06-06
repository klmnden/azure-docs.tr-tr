---
title: Çağrı yapma veya logic apps ile Azure işlevleri ve Azure Service Bus tetikleyicisi
description: Çağrı yapma veya logic apps, Azure Service Bus'ı kullanarak tetikleme Azure işlevleri oluşturun
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: jehollan, klam, LADocs
ms.topic: article
ms.assetid: 19cbd921-7071-4221-ab86-b44d0fc0ecef
ms.date: 06/04/2019
ms.openlocfilehash: 3d4f642ae25a179ea2c3241240996da774cd8c23
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66494944"
---
# <a name="call-or-trigger-logic-apps-by-using-azure-functions-and-azure-service-bus"></a>Çağrı yapma veya logic apps, Azure işlevleri ve Azure Service Bus'ı kullanarak tetikleme

Kullanabileceğiniz [Azure işlevleri](../azure-functions/functions-overview.md) bir uzun süre çalışan dinleyici veya görev dağıtmak istediğinizde bir mantıksal uygulama tetiklemek için. Örneğin, üzerinde dinleyen bir Azure işlevi oluşturabilir bir [Azure Service Bus](../service-bus-messaging/service-bus-messaging-overview.md) sıraya almak ve hemen bir anında iletme tetikleyici olarak mantıksal uygulama başlatılır.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Bir Azure Service Bus ad alanı. Bir ad alanı yoksa, [önce ad alanınızı oluşturmanız](../service-bus-messaging/service-bus-create-namespace-portal.md).

* Azure işlevleri için bir kapsayıcı bir Azure işlev uygulaması. Bir işlev uygulaması yoksa [ilk işlev uygulamanızı oluşturmak](../azure-functions/functions-create-first-azure-function.md)ve çalışma zamanı yığını olarak .NET seçtiğinizden emin olun.

* Hakkında temel bilgilere [mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)

## <a name="create-logic-app"></a>Mantıksal uygulama oluşturma

Bu senaryo için tetiklemek istediğiniz her mantıksal uygulama çalışan bir işleviniz oldu. İlk olarak bir HTTP isteği tetikleyicisi ile başlayan bir mantıksal uygulama oluşturun. Bir kuyruk iletisi alındığında Bu uç nokta işlevini çağırır.  

1. Oturum [Azure portalında](https://portal.azure.com)ve boş mantıksal uygulama oluşturun.

   Logic apps kullanmaya yeni başladıysanız gözden [hızlı başlangıç: İlk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

1. Arama kutusuna "http isteği" girin. Tetikleyiciler listesinden şu tetikleyiciyi seçin: **Bir HTTP isteği alındığında**

   ![Tetikleyici seçin](./media/logic-apps-scenario-function-sb-trigger/when-http-request-received-trigger.png)

   İstek tetikleyicisinde ile isteğe bağlı olarak bir kuyruk iletisi ile kullanmak üzere bir JSON şema girebilirsiniz. JSON şemalarının anlamak için giriş veri yapısı ve çıkışlar, iş akışında kullanabilmeniz için kolaylaştırmak mantıksal Uygulama Tasarımcısı yardımcı olur.

1. Bir şema belirtmek için şemada girin **istek gövdesi JSON şeması** kutusunda, örneğin:

   ![JSON şemasını belirtin](./media/logic-apps-scenario-function-sb-trigger/when-http-request-received-trigger-schema.png)

   Bir şema yok, ancak bir örnek yük JSON biçiminde varsa, bir şema bu yükü oluşturabilir.

   1. İstek tetikleyicisinde seçin **şema oluşturmak için örnek yük kullanma**.

   1. Altında **girin veya yapıştırın örnek JSON yükü**, örnek yükünüzü girin ve ardından **Bitti**.

      ![Örnek yük girin](./media/logic-apps-scenario-function-sb-trigger/enter-sample-payload.png)

   Bu örnek yük tetikleyici görünen bu şema oluşturur:

   ```json
   {
      "type": "object",
      "properties": {
         "address": {
            "type": "object",
            "properties": {
               "number": {
                  "type": "integer"
               },
               "street": {
                  "type": "string"
               },
               "city": {
                  "type": "string"
               },
               "postalCode": {
                  "type": "integer"
               },
               "country": {
                  "type": "string"
               }
            }
         }
      }
   }
   ```

1. Kuyruk iletisini aldıktan sonra çalıştırmak istediğiniz diğer tüm eylemler ekleyin.

   Örneğin, Office 365 Outlook Bağlayıcısı'nı içeren bir e-posta gönderebilirsiniz.

1. Tetikleyici için geri çağırma URL'si bu mantıksal uygulama oluşturur. mantıksal uygulamanızı kaydedin. Daha sonra bu geri çağırma URL'si için Azure Service Bus kuyruğu tetikleyicisi kodu kullanın.

   Geri çağırma URL'si görünür **HTTP POST URL'si** özelliği.

   ![Tetikleyici için oluşturulan geri çağırma URL'si](./media/logic-apps-scenario-function-sb-trigger/callback-URL-for-trigger.png)

## <a name="create-azure-function"></a>Azure işlevi oluşturma

Ardından, tetikleyici olarak davranır ve kuyruğa dinleyen bir işlev oluşturun.

1. Azure portalında açın ve işlev uygulamanızı genişletin, açık değilse. 

1. İşlev uygulaması adınızın altındaki genişletin **işlevleri**. Üzerinde **işlevleri** bölmesinde seçin **yeni işlev**.

   !["İşlevleri" genişletin ve "İşlev" seçin](./media/logic-apps-scenario-function-sb-trigger/create-new-function.png)

1. Bu şablon, yeni bir işlev uygulaması, çalışma zamanı yığını olarak .NET burada seçtiğiniz oluşturduğunuz üzerinde göre seçin veya var olan bir işlev uygulaması kullanıyorsanız.

   * Yeni işlev uygulamaları için bu şablonu seçin: **Service Bus kuyruğu tetikleyicisi**

     ![Yeni işlev uygulaması için bir şablon seçin](./media/logic-apps-scenario-function-sb-trigger/current-add-queue-trigger-template.png)

   * Var olan bir işlev uygulaması için bu şablonu seçin: **Service Bus kuyruğu tetikleyicisi-C#**

     ![İşlev uygulamanız için şablon seçin](./media/logic-apps-scenario-function-sb-trigger/legacy-add-queue-trigger-template.png)

1. Üzerinde **Azure Service Bus kuyruğu tetikleyicisi** bölmesinde tetikleyicinizin bir ad verin ve ayarlayın **Service Bus bağlantısı** kuyruğu için Azure Service Bus SDK'sı kullanan `OnMessageReceive()` dinleyici ve seçin **Oluşturma**.

1. Tetikleyici olarak kuyruk iletisini kullanarak önceden oluşturulmuş mantıksal uygulama uç noktasını çağırmak için temel bir işlev yazın. Bu örnekte `application/json` ileti içerik türü, ancak bu tür gerektiği şekilde değiştirebilirsiniz. Mümkünse, HTTP istemcilerini örneğini yeniden kullanın. Daha fazla bilgi için [Azure işlevleri'nde bağlantıları yönetme](../azure-functions/manage-connections.md).

   ```CSharp
   using System;
   using System.Threading.Tasks;
   using System.Net.Http;
   using System.Text;

   // Callback URL for previously created Request trigger
   private static string logicAppUri = @"https://prod-05.westus.logic.azure.com:443/workflows/<remaining-callback-URL>";

   // Reuse the instance of HTTP clients if possible
   private static HttpClient httpClient = new HttpClient();

   public static void Run(string myQueueItem, ILogger log)
   {
       log.LogInformation($"C# ServiceBus queue trigger function processed message: {myQueueItem}");

       var response = httpClient.PostAsync(logicAppUri, new StringContent(myQueueItem, Encoding.UTF8, "application/json")).Result;
   }
   ```

1. İşlevi test etmek için bir aracı gibi kullanarak bir kuyruğa ileti eklemek [hizmet veri yolu Gezgini](https://github.com/paolosalvatori/ServiceBusExplorer).

   Hemen işlevi iletiyi aldıktan sonra mantıksal uygulama Tetikleyicileri.

## <a name="next-steps"></a>Sonraki adımlar

[Çağrı, tetikleyici veya iş akışı HTTP uç noktaları kullanarak iç içe](../logic-apps/logic-apps-http-endpoint.md)