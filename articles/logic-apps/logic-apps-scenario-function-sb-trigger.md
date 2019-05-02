---
title: Senaryo - Azure işlevleri ve Azure Service Bus ile tetiklenen logic apps | Microsoft Docs
description: Logic apps, Azure işlevleri ve Azure Service Bus'ı kullanarak tetikleme işlevler oluştur
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: jehollan, klam, LADocs
ms.topic: article
ms.assetid: 19cbd921-7071-4221-ab86-b44d0fc0ecef
ms.date: 08/25/2018
ms.openlocfilehash: 89e1330dae65e0cea891407764a0ef20a2f41d81
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64916421"
---
# <a name="scenario-trigger-logic-apps-with-azure-functions-and-azure-service-bus"></a>Senaryo: Azure işlevleri ve Azure Service Bus ile tetiklenen logic apps

Bir uzun süre çalışan dinleyici veya görev dağıtmak istediğinizde bir mantıksal uygulama için bir tetikleyici oluşturmak için Azure işlevleri'ni kullanabilirsiniz. Örneğin, bir kuyruğu dinler bir işlev oluşturun ve bir anında iletme tetikleyici olarak bir mantıksal uygulamayı hemen harekete.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>. 

* Hakkında temel bilgilere [mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) 

* Bir Azure işlev oluşturabilmeniz için önce [bir işlev uygulaması oluşturma](../azure-functions/functions-create-function-app-portal.md).

## <a name="create-logic-app"></a>Mantıksal uygulama oluşturma

Bu örnekte, tetiklenmesi gereken her mantıksal uygulama için çalışan bir işleviniz oldu. İlk olarak bir HTTP isteği tetikleyicisi olan bir mantıksal uygulama oluşturun. Bir kuyruk iletisi alındığında Bu uç nokta işlevini çağırır.  

1. Oturum [Azure portalında](https://portal.azure.com)ve boş mantıksal uygulama oluşturun. 

   Logic apps kullanmaya yeni başladıysanız gözden [hızlı başlangıç: İlk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

1. Arama kutusuna "http isteği" girin. Tetikleyiciler listesinde şu tetikleyiciyi seçin: **Bir HTTP isteği alındığında**

   ![Tetikleyici seçin](./media/logic-apps-scenario-function-sb-trigger/when-http-request-received-trigger.png)

1. İçin **istek** tetikleyici, kuyruk iletisi ile kullanmak için isteğe bağlı olarak bir JSON şeması girebilirsiniz. JSON şemalarının Logic Apps Tasarımcısı'nin daha kolay iş akışı genelinde seçebilmeniz için giriş verileri ve yapar çıkışları yapısını anlamanıza yardımcı olur. 

   Bir şema belirtmek için şemada girin **istek gövdesi JSON şeması** kutusunda, örneğin: 

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

1. Kuyruk iletisini aldıktan sonra olmasını istediğiniz diğer tüm eylemler ekleyin. 

   Örneğin, Office 365 Outlook Bağlayıcısı'nı içeren bir e-posta gönderebilirsiniz.

1. Tetikleyici için geri çağırma URL'si bu mantıksal uygulama oluşturur. mantıksal uygulamanızı kaydedin. Bu URL'yi görünür **HTTP POST URL'si** özelliği.

   ![Tetikleyici için oluşturulan geri çağırma URL'si](./media/logic-apps-scenario-function-sb-trigger/callback-URL-for-trigger.png)

## <a name="create-azure-function"></a>Azure işlevi oluşturma

Ardından, tetikleyici olarak davranır ve kuyruğa dinleyen bir işlev oluşturun. 

1. Azure portalında açın ve işlev uygulamanızı genişletin, açık değilse. 

1. İşlev uygulaması adınızın altındaki genişletin **işlevleri**. Üzerinde **işlevleri** bölmesinde seçin **yeni işlev**. Bu şablonu seçin: **Service Bus kuyruğu tetikleyicisi-C#**
   
   ![Azure işlevleri portalına seçin](./media/logic-apps-scenario-function-sb-trigger/newqueuetriggerfunction.png)

1. Tetikleyici için bir ad belirtin ve ardından Azure Service Bus SDK'sı kullanan Service Bus kuyruğuna bağlantıyı yapılandırın `OnMessageReceive()` dinleyicisi.

1. Tetikleyici olarak, örneğin kuyruk iletisini kullanarak önceden oluşturulmuş mantıksal uygulama uç noktasını çağırmak için temel bir işlev yazın: 
   
   ```CSharp
   using System;
   using System.Threading.Tasks;
   using System.Net.Http;
   using System.Text;
   
   private static string logicAppUri = @"https://prod-05.westus.logic.azure.com:443/.........";
   
   // Re-use instance of http clients if possible - https://docs.microsoft.com/azure/azure-functions/manage-connections
   private static HttpClient httpClient = new HttpClient();
   
   public static void Run(string myQueueItem, TraceWriter log)
   {
       log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");

       var response = httpClient.PostAsync(logicAppUri, new StringContent(myQueueItem, Encoding.UTF8, "application/json")).Result;
   }
   ```

   Bu örnekte `application/json` ileti içerik türü, ancak bu tür gerektiği şekilde değiştirebilirsiniz.

1. İşlevi test etmek için bir aracı gibi kullanarak bir kuyruğa ileti eklemek [hizmet veri yolu Gezgini](https://github.com/paolosalvatori/ServiceBusExplorer). 

   Hemen işlevi iletiyi aldıktan sonra mantıksal uygulama Tetikleyicileri.

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

