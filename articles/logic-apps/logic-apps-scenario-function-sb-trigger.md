---
title: Senaryo - tetikleyici logic apps ile Azure işlevleri ve Azure Service Bus | Microsoft Docs
description: Azure işlevleri ve Azure Service Bus kullanarak bir mantıksal uygulama tetiklemek için bir işlev oluşturun
services: logic-apps,functions
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: jeconnoc
editor: ''
ms.assetid: 19cbd921-7071-4221-ab86-b44d0fc0ecef
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 05/23/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: 89fcd88643bd793935e7476ef32641ffa5ff4713
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35299802"
---
# <a name="scenario-trigger-a-logic-app-with-azure-functions-and-azure-service-bus"></a>Senaryo: bir mantıksal uygulama Azure işlevleri ve Azure Service Bus ile Tetikle

Azure işlevleri, bir uzun süre çalışan dinleyicisi veya görev dağıtmak gerektiğinde bir mantıksal uygulama için bir tetikleyici oluşturmak için kullanabilirsiniz. Örneğin, bir sıra üzerinde dinleyen bir işlev oluşturun ve hemen bir mantıksal uygulama itme tetikleyici olarak tetiklenecek.

## <a name="build-the-logic-app"></a>Mantıksal uygulama oluşturma
Bu örnekte, tetiklenmesi için gereken her mantıksal uygulama için çalışan bir işleve sahip. İlk olarak, bir HTTP isteği tetikleyicisine sahip bir mantıksal uygulama oluşturun. Bir kuyruk iletisi alındığında Bu uç işlevi çağırır.  

1. Bir mantıksal uygulama oluşturun.
2. Seçin **bir HTTP isteği alındığında el ile -** tetikleyici.
   İsteğe bağlı olarak, gibi bir araç kullanarak sıraya ileti ile kullanmak için JSON şeması belirtebilirsiniz [jsonschema.net](http://jsonschema.net). Şema tetikleyicinin yapıştırın. Şemalar, iş akışı aracılığıyla daha kolay veri ve akış özellikleri şeklini anlamanız Tasarımcısı yardımcı olur.
2. Bir kuyruk iletisi alındıktan sonra gerçekleştirilmesini istediğiniz herhangi bir ilave adımı ekleyin. Örneğin, Office 365 aracılığıyla bir e-posta gönderin.  
3. Bu mantıksal uygulama tetikleyiciye geri çağırma URL'si oluşturmak için mantıksal uygulama kaydedin. URL tetikleyici kart üzerinde görüntülenir.

![Geri çağırma URL'si tetikleyici kart üzerinde görüntülenir.][1]

## <a name="build-the-function"></a>İşlev oluşturma
Ardından, tetikleyici olarak davranır ve sıraya dinleyen bir işlev oluşturmanız gerekir.

1. İçinde [Azure işlevleri portalına](https://functions.azure.com/signin)seçin **yeni işlev**ve ardından **ServiceBusQueueTrigger - C#** şablonu.
   
    ![Azure işlevleri portalına][2]
2. Azure hizmet veri yolu SDK'sını kullanan Service Bus kuyruğuna bağlantıyı yapılandırmak `OnMessageReceive()` dinleyicisi.
3. Tetikleyici olarak kuyruk iletisini kullanarak (daha önce oluşturduğunuz) mantığı uygulama uç noktasını çağırmak için bir temel işlevi yazma. Bir işlev tam bir örneği burada verilmiştir. Örnek kullanan bir `application/json` ileti içerik türü, ancak bu tür gerektiğinde değiştirebilirsiniz.
   
   ```
   using System;
   using System.Threading.Tasks;
   using System.Net.Http;
   using System.Text;
   
   private static string logicAppUri = @"https://prod-05.westus.logic.azure.com:443/.........";
   
   public static void Run(string myQueueItem, TraceWriter log)
   {
   
       log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
       using (var client = new HttpClient())
       {
           var response = client.PostAsync(logicAppUri, new StringContent(myQueueItem, Encoding.UTF8, "application/json")).Result;
       }
   }
   ```

Test etmek için bir kuyruk iletisi gibi bir araç eklemek [hizmet veri yolu Gezgini](https://github.com/paolosalvatori/ServiceBusExplorer). Hemen işlevi iletiyi aldıktan sonra yangın mantıksal uygulama bakın.

<!-- Image References -->
[1]: ./media/logic-apps-scenario-function-sb-trigger/manualtrigger.png
[2]: ./media/logic-apps-scenario-function-sb-trigger/newqueuetriggerfunction.png
