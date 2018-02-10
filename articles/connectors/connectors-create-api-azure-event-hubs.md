---
title: "Azure mantıksal uygulamaları için Azure Event Hubs ile olay izleme işlevini ayarlama | Microsoft Docs"
description: "Veri akışlarının olayları gönderip olaylar logic apps ile Azure Event Hubs'ı kullanarak izleme"
services: logic-apps
keywords: "veri akışı, olay izleme, olay hub'ları"
author: ecfan
manager: anneta
editor: 
documentationcenter: 
tags: connectors
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/06/2018
ms.author: estfan; LADocs
ms.openlocfilehash: 076f7dd11ca8c153046727861ecb755e88f32b01
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="monitor-receive-and-send-events-with-the-event-hubs-connector"></a>İzleme, almak ve Event Hubs Bağlayıcısı ile olayları göndermek

Mantıksal uygulamanızı olayları algılamak, olayları alabilir ve olayları göndermek için bir Olay İzleyicisi ayarlamak üzere bağlanmak bir [Azure olay hub'ı](https://azure.microsoft.com/services/event-hubs) mantığı uygulamanızdan. Daha fazla bilgi edinmek [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) ve [fiyatlandırma Logic Apps bağlayıcıları için nasıl çalıştığını](../logic-apps/logic-apps-pricing.md).

## <a name="prerequisites"></a>Önkoşullar

Olay hub'ları Bağlayıcısı'nı kullanmadan önce bu öğeler sahip olmanız gerekir:

* Bir [Azure Event Hubs ad alanı ve olay hub'ı](../event-hubs/event-hubs-create.md)
* A [mantıksal uygulama](../logic-apps/quickstart-create-first-logic-app-workflow.md)

<a name="permissions-connection-string"></a>

## <a name="connect-to-azure-event-hubs"></a>Azure Event Hubs'a bağlama

Oluşturmak zorunda mantıksal uygulamanızı herhangi bir hizmet erişebilmeniz için önce bir [ *bağlantı* ](./connectors-overview.md) mantıksal uygulamanızı ve henüz yapmadıysanız hizmetin arasında. Bu bağlantı verilere erişmek için mantıksal uygulamanızı yetkilendirir. Olay Hub'ınızı erişmek mantığı uygulamanız için izinleri denetleyin ve için olay hub'ları ad alanınıza bağlantı dizesi alın.

1.  [Azure portalı](https://portal.azure.com "Azure portalı") oturumunu açın. 

2.  Olay hub'larınız gidin *ad alanı*, belirli bir Event Hub değil. Ad alanı sayfasında altında **ayarları**, seçin **paylaşılan erişim ilkeleri**. Altında **talep**, sahip olduğunuz onay **Yönet** bu ad alanı için izinleri.

    ![Olay hub'ı ad alanınız için izinleri yönetme](./media/connectors-create-api-azure-event-hubs/event-hubs-namespace.png)

3. Daha sonra el ile bağlantı bilgilerinizi girmek isterseniz, bağlantı dizesi için olay hub'ları ad alanınıza alın. Seçin **RootManageSharedAccessKey**. Birincil anahtar bağlantı dizenizi yanındaki Kopyala düğmesini seçin. Daha sonra kullanmak için bağlantı dizesini kaydedin.

    ![Olay hub'ları ad alanı bağlantı dizesini kopyalayın](media/connectors-create-api-azure-event-hubs/find-event-hub-namespace-connection-string.png)

    > [!TIP]
    > Bağlantı dizenizi bir belirli olay hub'ı veya olay hub'ları ad alanı ile ilişkili olup olmadığını doğrulamak için bağlantı dizesini kontrol `EntityPath` parametresi. Bu parametre bulursanız, bağlantı dizesi belirli olay hub için "varlığı" ve mantıksal uygulamanızı ile kullanılacak doğru dizesi değil.

## <a name="trigger-workflow-when-your-event-hub-gets-new-events"></a>Olay Hub'ınızı yeni olaylar aldığında iş akışını tetikleyen

A [ *tetikleyici* ](../logic-apps/logic-apps-overview.md#logic-app-concepts) mantıksal uygulamanızı bir iş akışı başlatır bir olaydır. Yeni olaylar için olay Hub'ınızı gönderildiğinde bir iş akışını başlatmak için bu olay algılar tetikleyici eklemek için aşağıdaki adımları izleyin.

1. İçinde [Azure portal](https://portal.azure.com "Azure portal"), var olan mantıksal uygulamanızı Git veya boş mantıksal uygulama oluşturma.

2. Logic Apps Tasarımcısı'nda, filtre olarak arama kutusuna "event hubs" girin. Bu tetikleyici seçin: **zaman olayların Event Hub'ında kullanılabilir**

   ![Olay Hub'ınızı yeni olayların ne zaman alan için tetikleyici seçin](./media/connectors-create-api-azure-event-hubs/find-event-hubs-trigger.png)

   1. Olay hub'ları ad alanınıza bağlantı sahip değilseniz, bu bağlantı artık oluşturmanız istenir. Bağlantınızı bir ad verin ve kullanmak istediğiniz olay hub'ları ad alanını seçin.

      ![Olay Hub bağlantısı oluşturma](./media/connectors-create-api-azure-event-hubs/create-event-hubs-connection-1.png)

      Veya el ile bağlantı dizesini girmek için seçin **bağlantı bilgilerini el ile girebilirsiniz**. 
      Bilgi [bağlantı dizenizi bulmak nasıl](#permissions-connection-string).

   2. Şimdi kullanın ve seçmek için olay hub'ları ilkesini seçin **oluşturma**.

      ![Olay hub'ı bağlantı, bölüm 2 oluşturun](./media/connectors-create-api-azure-event-hubs/create-event-hubs-connection-2.png)

3. İzleme ve olay hub'ı denetlemek ne zaman sıklık ve aralığı ayarlamak için olay hub'ı seçin.

    ![Olay hub'ı veya tüketici grubu belirtin](./media/connectors-create-api-azure-event-hubs/select-event-hub.png)

    > [!TIP]
    > İsteğe bağlı olarak olayları okumak için bir tüketici grubu seçmek için Seç **Gelişmiş Seçenekleri Göster**.

4. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet**'i seçin.

Şimdi, mantıksal uygulamanızı seçili olay hub'ı denetler ve yeni bir olay bulduğunda tetikleyicinin mantığını uygulamanızda bulunan olayı için eylemleri çalıştırır.

## <a name="send-events-to-your-event-hub-from-your-logic-app"></a>Olayları için olay Hub'ınızı mantığı uygulamanızdan Gönder

[*Eylem*](../logic-apps/logic-apps-overview.md#logic-app-concepts), mantıksal uygulama iş akışınız tarafından gerçekleştirilen bir görevdir. Mantıksal uygulamanıza bir tetikleyici ekledikten sonra, bu tetikleyici tarafından oluşturulan işlemleri gerçekleştirmek için bir eylem ekleyebilirsiniz. Bir olay için olay Hub'ınızı mantığı uygulamanızdan göndermek için aşağıdaki adımları izleyin.

1. Logic Apps Tasarımcısı'nda, tetikleyici altında seçin **yeni adım** > **Eylem Ekle**.

2. Arama kutusuna "event hubs", filtre olarak girin.
Bu eylem seçin: **Event Hubs - gönderme olayı**

   !["Event Hubs - gönderme olayı" seçin](./media/connectors-create-api-azure-event-hubs/select-event-hubs-send-event-action.png)

3. Olayın gönderileceği yeri için olay hub'ı seçin. Ardından, olay içeriği ve başka ayrıntılar girin.

   ![Olay hub'ı adı seçin ve olay içerik sağlayın](./media/connectors-create-api-azure-event-hubs/event-hubs-send-event-action.png)

4. Mantıksal uygulamanızı kaydedin.

Şimdi mantıksal uygulamanızı olayları gönderdiği eylem kurdu. 

## <a name="connector-specific-details"></a>Bağlayıcı özgü ayrıntıları

Tetikleyiciler ve Eylemler Swagger dosyası ve herhangi bir sınır tarafından tanımlanan hakkında daha fazla bilgi edinmek için gözden [Bağlayıcısı ayrıntıları](/connectors/eventhubs/).

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](http://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinmek [Azure mantıksal uygulamaları için diğer bağlayıcıları](../connectors/apis-list.md)