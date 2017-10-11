---
title: "Azure mantıksal uygulamaları için Azure Event Hubs ile Olay İzleyicisi ayarlama | Microsoft Docs"
description: "Veri akışları olaylarını almak ve Azure Logic Apps ile Azure Event Hubs için olayları göndermek için izleme"
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
ms.date: 03/31/2017
ms.author: estfan; LADocs
ms.openlocfilehash: 2ca27fb8269d1796fb1181fc4d0a8744a592d548
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-receive-and-send-events-with-the-event-hubs-connector"></a>İzleme, almak ve Event Hubs Bağlayıcısı ile olayları göndermek

Mantıksal uygulamanızı olayları algılamak, olayları alabilir ve olayları göndermek için bir Olay İzleyicisi ayarlamak üzere bağlanmak bir [Azure olay hub'ı](https://azure.microsoft.com/services/event-hubs) mantığı uygulamanızdan. Daha fazla bilgi edinmek [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md).

## <a name="requirements"></a>Gereksinimler

* Sahip olmanız bir [olay hub'ları ad alanı ve olay hub'ı](../event-hubs/event-hubs-create.md) azure'da. Bilgi [bir olay hub'ları ad alanı ve olay hub'ı nasıl oluşturulacağını](../event-hubs/event-hubs-create.md). 

* Kullanılacak [tüm bağlayıcıların](https://docs.microsoft.com/azure/connectors/apis-list) mantıksal uygulamanızı bir mantıksal uygulama ilk oluşturmanız gerekir. Bilgi [bir mantıksal uygulama oluşturma](../logic-apps/logic-apps-create-a-logic-app.md).

<a name="permissions-connection-string"></a>
## <a name="check-event-hubs-namespace-permissions-and-find-the-connection-string"></a>Olay hub'ları ad alanı izinleri denetleyin ve bağlantı dizesini bulun

Oluşturmak zorunda mantıksal uygulamanızı herhangi bir hizmete erişmek bir [ *bağlantı* ](./connectors-overview.md) mantıksal uygulamanızı ve henüz yapmadıysanız hizmetin arasında. Bu bağlantı verilere erişmek için mantıksal uygulamanızı yetkilendirir.
Mantıksal uygulamanızı olay Hub'ınızı erişmek sahip olmanız **Yönet** izinleri ve Event Hubs ad alanınıza bağlantı dizesi.

İzinlerinizi denetleyin ve bağlantı dizesini almak için aşağıdaki adımları izleyin.

1.  [Azure portalı](https://portal.azure.com "Azure portalı") oturumunu açın. 

2.  Olay hub'larınız gidin *ad alanı*, değil belirli olay hub'ı. Ad alanı dikey altında **ayarları**, seçin **paylaşılan erişim ilkeleri**. Altında **talep**, sahip olduğunuz onay **Yönet** bu ad alanı için izinleri.

    ![Olay hub'ı ad alanınız için izinleri yönetme](./media/connectors-create-api-azure-event-hubs/event-hubs-namespace.png)

3.  Olay hub'ları ad alanı için bağlantı dizesini kopyalayın tercih **RootManageSharedAccessKey**. Birincil anahtar bağlantı dizenizi yanındaki Kopyala düğmesini seçin.

    ![Olay hub'ları ad alanı bağlantı dizesini kopyalayın](media/connectors-create-api-azure-event-hubs/find-event-hub-namespace-connection-string.png)

    > [!TIP]
    > Bağlantı dizenizi olay hub'ları ad alanınız ile veya belirli bir Event Hub ile ilişkili olup olmadığını doğrulamak için bağlantı dizesini kontrol `EntityPath` parametresi. Bu parametre bulursanız, bağlantı dizesi belirli olay hub için "varlığı" ve mantıksal uygulamanızı ile kullanılacak doğru dizesi değil.

4.  Şimdi bir olay hub'ları tetikleyici veya eylem mantıksal uygulamanızı ekledikten sonra kimlik bilgileri istendiğinde, olay hub'ları ad alanına bağlanabilir. Bağlantınızı bir ad verin, kopyalanır ve seçin bağlantı dizesi **oluşturma**.

    ![Olay hub'ları ad alanı için bağlantı dizesi girin](./media/connectors-create-api-azure-event-hubs/event-hubs-connection.png)

    Bağlantınızı oluşturduktan sonra bağlantı adı olay hub'ları tetikleyici veya eylem görüntülenmelidir. 
    Ardından, mantıksal uygulamanızı diğer adımlarla devam edebilirsiniz.

    ![Oluşturulan olay hub'ları ad alanı bağlantı](./media/connectors-create-api-azure-event-hubs/event-hubs-connection-created.png)

## <a name="start-workflow-when-your-event-hub-receives-new-events"></a>Yeni olaylar olay Hub'ınızı aldığında, iş akışı Başlat

A [ *tetikleyici* ](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) mantıksal uygulamanızı bir iş akışı başlatır bir olaydır. Yeni olaylar için olay Hub'ınızı gönderildiğinde bir iş akışını başlatmak için bu olay algılar tetikleyici eklemek için aşağıdaki adımları izleyin.

1.  İçinde [Azure portal](https://portal.azure.com "Azure portal"), var olan mantıksal uygulamanızı Git veya boş mantıksal uygulama oluşturma.

2.  Mantıksal Uygulama Tasarımcısı'nı arama kutusuna girin `event hubs` filtreniz için. Bu tetikleyici seçin: **zaman olayların Event Hub'ında kullanılabilir**

    ![Olay Hub'ınızı yeni olayların ne zaman alan için tetikleyici seçin](./media/connectors-create-api-azure-event-hubs/find-event-hubs-trigger.png)

    Olay hub'ları ad alanınıza bağlantı sahip değilseniz, bu bağlantı artık oluşturmanız istenir. Bağlantınızı bir ad verin ve için olay hub'ları ad alanınıza bağlantı dizesi girin. 
    Gerekirse, bilgi [bağlantı dizenizi bulmak nasıl](#permissions-connection-string).

    ![Olay hub'ları ad alanı için bağlantı dizesi girin](./media/connectors-create-api-azure-event-hubs/event-hubs-connection.png)

    Bağlantı ayarlarını oluşturduktan sonra **bir olay gerçekleştiğinde bir olay Hub'ındaki kullanılabilir** tetikleyici görünür.

    ![Olay Hub'ınızı yeni olayların ne zaman alan için tetikleyici ayarları](./media/connectors-create-api-azure-event-hubs/event-hubs-trigger.png)

3.  Adını girin veya izlemek istediğiniz olay hub'ı seçin. Olay hub'ı denetlemek istediğiniz sıklıkta aralığı ve sıklığı seçin.

    > [!TIP]
    > İsteğe bağlı olarak olayları okumak için bir tüketici grubu seçmek için Seç **Gelişmiş Seçenekleri Göster**. 

    ![Olay hub'ı veya tüketici grubu belirtin](./media/connectors-create-api-azure-event-hubs/event-hubs-trigger-details.png)

    Şimdi bir tetikleyici mantığı uygulamanız için bir iş akışını başlatmak için ayarladığınız. 
    Mantıksal uygulamanızı belirtilen Event Hub'in belirlediğiniz bir zamanlamaya göre denetler. 
    Uygulamanızı yeni olaylar bulursa olay Hub'ı tetikleyici diğer eylemleri çalıştırır veya mantıksal uygulamanızı tetikler.

## <a name="send-events-to-your-event-hub-from-your-logic-app"></a>Olayları için olay Hub'ınızı mantığı uygulamanızdan Gönder

[*Eylem*](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts), mantıksal uygulama iş akışınız tarafından gerçekleştirilen bir görevdir. Mantıksal uygulamanıza bir tetikleyici ekledikten sonra, bu tetikleyici tarafından oluşturulan işlemleri gerçekleştirmek için bir eylem ekleyebilirsiniz. Bir olay için olay Hub'ınızı mantığı uygulamanızdan göndermek için aşağıdaki adımları izleyin.

1.  Mantıksal Uygulama Tasarımcısı'nda mantığı uygulama tetikleyici altında seçin **yeni adım** > **Eylem Ekle**.

    !["Yeni adım" sonra "Eylem Ekle" seçin](./media/connectors-create-api-azure-event-hubs/add-action.png)

    Şimdi Bul ve gerçekleştirmek istediğiniz eylemi seçin. 
    Bu örnek için herhangi bir eylem seçebilmenize rağmen olayları göndermek için olay hub'ları eylem istiyoruz.

2.  Arama kutusuna `event hubs` filtreniz için.
Bu eylem seçin: **gönderme olayı**

    !["Event Hubs - gönderme olayı" seçin eylemi](./media/connectors-create-api-azure-event-hubs/find-event-hubs-action.png)

3.  Olay göndermek istediğiniz olay hub'ı adı gibi olayı için gerekli ayrıntıları girin. Bu olay için içeriği gibi olaya ilişkin isteğe bağlı diğer ayrıntılarını girin.

    > [!TIP]
    > İsteğe bağlı olarak olay hub'ı bölüm olay gönderileceği yeri belirtmek için **Gelişmiş Seçenekleri Göster**. 

    ![Olay hub'ı adı ve isteğe bağlı olay ayrıntılarını girin](./media/connectors-create-api-azure-event-hubs/event-hubs-send-event-action.png)

6.  Yaptığınız değişiklikleri kaydedin.

    ![Mantıksal uygulamanızı kaydetme](./media/connectors-create-api-azure-event-hubs/save-logic-app.png)

    Şimdi, mantıksal uygulamanızı olayları göndermek için eylem ayarlama. 

## <a name="connector-specific-details"></a>Bağlayıcı özgü ayrıntıları

Tüm tetikleyiciler ve Eylemler swagger tanımlanan görüntüleyebilir ve ayrıca herhangi bir sınır bkz [Bağlayıcısı ayrıntıları](/connectors/eventhubs/). 

## <a name="get-help"></a>Yardım alın

Sorular sormak, soruları yanıtlamak ve diğer Azure Logic Apps kullanıcılarının neler yaptığını görmek için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.

Logic Apps ve bağlayıcıları geliştirmeye yardımcı olmak için, [Logic Apps kullanıcı geri bildirim sitesinde](http://aka.ms/logicapps-wish) oy kullanın veya fikirlerinizi paylaşın.

## <a name="next-steps"></a>Sonraki adımlar

*  [Azure mantıksal uygulamaları için diğer bağlayıcıları Bul](./apis-list.md)