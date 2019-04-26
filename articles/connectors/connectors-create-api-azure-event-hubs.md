---
title: Azure Event Hubs'a - Azure Logic Apps bağlanma
description: Yönetme ve olayları Azure Event Hubs ve Azure Logic Apps ile izleme
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 05/21/2018
tags: connectors
ms.openlocfilehash: a59f21478f85f238d91c01faed44d8e49cb15f0a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60313531"
---
# <a name="monitor-receive-and-send-events-with-azure-event-hubs-and-azure-logic-apps"></a>İzleme, alabilir ve olayları Azure Event Hubs ve Azure Logic Apps ile gönderme

Bu makale nasıl izleyebilir ve gönderilen olayları yönetmek [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) gelen bir mantıksal uygulama ile Azure Event Hubs bağlayıcısını içinde. Böylece, görevler ve denetimi, gönderme ve olay Hub'ından olaylarını almak için iş akışlarını otomatik hale getiren mantıksal uygulamalar oluşturabilirsiniz.

Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>. Logic apps kullanmaya yeni başladıysanız gözden [Azure Logic Apps nedir](../logic-apps/logic-apps-overview.md) ve [hızlı başlangıç: İlk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).
Bağlayıcısı özel teknik bilgiler için bkz. <a href="https://docs.microsoft.com/connectors/eventhubs/" target="blank">Azure Event Hubs bağlayıcısını başvuru</a>.

## <a name="prerequisites"></a>Önkoşullar

* Bir [Azure Event Hubs ad alanı ve olay hub'ı](../event-hubs/event-hubs-create.md)

* Olay Hub'ınıza erişmek istediğiniz mantıksal uygulaması. Mantıksal uygulamanızı bir Azure Event Hubs tetikleyici ile başlayın, gerek bir [boş mantıksal uygulama](../logic-apps/quickstart-create-first-logic-app-workflow.md).

<a name="permissions-connection-string"></a>

## <a name="check-permissions-and-get-connection-string"></a>İzinleri denetleyin ve bağlantı dizesini alma

Mantıksal uygulamanızın olay Hub'ınıza erişmek izinleri denetleyin ve Event Hubs ad alanınızın bağlantı dizesini alın.

1. <a href="https://portal.azure.com" target="_blank">Azure Portal</a> oturum açın.

2. Event Hubs'ınız Git *ad alanı*, belirli bir Event Hub değil. Ad alanı sayfasında altında **ayarları**, seçin **paylaşılan erişim ilkeleri**. Altında **talep**, sahip olduğunuz denetimi **Yönet** bu ad alanı için izinler.

   ![Olay hub'ı ad alanınız için izinleri Yönet](./media/connectors-create-api-azure-event-hubs/event-hubs-namespace.png)

3. Bağlantı bilgilerinizi daha sonra el ile girmek istiyorsanız, Event Hubs ad alanınızın bağlantı dizesini alın.

   1. Altında **ilke**, seçin **RootManageSharedAccessKey**.

   2. Birincil anahtarın bağlantı dizesini bulun. Kopyala düğmesini seçin ve daha sonra kullanmak için bağlantı dizesini kaydedin.

      ![Event Hubs ad alanı bağlantı dizesini kopyalayın](media/connectors-create-api-azure-event-hubs/find-event-hub-namespace-connection-string.png)

      > [!TIP]
      > Bağlantı dizeniz ile Event Hubs ad alanınız veya belirli bir olay hub'ı ile ilişkili olup olmadığını doğrulamak için bağlantı dizesi yok emin olun `EntityPath`  parametresi. Bu parametre bulursanız, bağlantı dizesini bir özel olay hub'ı için "varlık" ve mantıksal uygulamanız ile kullanılacak doğru dizesi değil.

4. Artık devam [bir Event hubs'ı tetikleyicisi ekleme](#add-trigger) veya [Event Hubs Eylem Ekle](#add-action).

<a name="add-trigger"></a>

## <a name="add-an-event-hubs-trigger"></a>Bir Event hubs'ı tetikleyicisi ekleme

Azure Logic Apps'te, her mantıksal uygulama ile başlamalıdır bir [tetikleyici](../logic-apps/logic-apps-overview.md#logic-app-concepts), belirli bir olay harekete geçirilir gerçekleşen veya belirli bir koşul karşılanıyorsa zaman. Her zaman tetikleyici Logic Apps altyapısı bir mantıksal uygulama örneği oluşturur ve uygulamanızın iş akışı çalışmaya başlar.

Bu örnek nasıl yeni olayları olay Hub'ınıza gönderildiğinde bir mantıksal uygulama iş akışı başlatabilirsiniz gösterir.

1. Azure portalı ya da Visual Studio, Logic Apps Tasarımcısı açılır bir boş mantıksal uygulama oluşturun. Bu örnek, Azure portalını kullanır.

2. Arama kutusuna filtreniz olarak "event hubs" girin. Tetikleyiciler listesinden istediğiniz tetikleyicisini seçin.

   Bu örnek, bu tetikleyici kullanır:

   **Event Hubs - olay Hub'ında kullanılabilir olaylar olduğunda**

   ![Tetikleyici seçin](./media/connectors-create-api-azure-event-hubs/find-event-hubs-trigger.png)

3. Bağlantı ayrıntıları için istenirse [artık Event Hubs bağlantınızı oluşturmak](#create-connection). Veya, bağlantınız zaten varsa, tetikleyici için gerekli bilgileri sağlayın.

   1. Gelen **olay hub'ı adı** listesinde, izlemek istediğiniz olay hub'ı seçin.

      ![Olay hub'ı veya tüketici grubu belirtin](./media/connectors-create-api-azure-event-hubs/select-event-hub.png)

   2. Aralığı ve olay hub'ı kontrol etmek için tetikleyici hangi sıklıkta güncelleştirileceğini sıklığını seçin.

   3. İsteğe bağlı olarak bazı gelişmiş tetikleyicisi seçenekleri seçmek için Seç **Gelişmiş Seçenekleri Göster**.

      ![Gelişmiş Seçenekleri tetikleyicisi](./media/connectors-create-api-azure-event-hubs/event-hubs-trigger-advanced.png)

      | Özellik | Ayrıntılar | 
      |----------|---------| 
      | İçerik türü  | Olayın içerik türünü seçin. "Application/octet-stream" varsayılandır. |
      | İçerik şeması | İçerik şeması, olayları olay Hub'ından okumak için JSON biçiminde girin. |
      | Tüketici grubu adı | Olay hub'ı girin [tüketici grubu adı](../event-hubs/event-hubs-features.md#consumer-groups) olayları okumak için. Belirtilmezse, varsayılan bir tüketici grubu kullanılır. |
      | En düşük bölüm anahtarı | En düşük girin [bölüm](../event-hubs/event-hubs-features.md#partitions) okumak için kimliği. Varsayılan olarak, tüm bölümleri okunur. |
      | En yüksek bölüm anahtarı | En yüksek girin [bölüm](../event-hubs/event-hubs-features.md#partitions) okumak için kimliği. Varsayılan olarak, tüm bölümleri okunur. |
      | En yüksek olay sayısı | En yüksek olay sayısı için bir değer girin. Tetikleyici arasında bir ve bu özelliği tarafından belirtilen olay sayısını döndürür. |
      |||

4. Tasarımcı araç çubuğunda, işiniz bittiğinde seçin **Kaydet**.

5. Şimdi mantıksal uygulamanız tetikleme sonuçlarıyla gerçekleştirmek istediğiniz görevleri için bir veya daha fazla eylem ekleyerek devam edin.

> [!NOTE]
> Tüm olay hub'ı Tetikleyiciler *uzun yoklaması* tetikleyiciler, bir Tetikleyici etkinleştirildiğinde, tetikleyici tüm olayları işler ve olay Hub'ında görünmesi 30 saniye için daha fazla olay bekler anlamına gelir.
> Hiçbir olay 30 saniye içinde alınırsa, tetikleyici çalıştırması atlanır. Aksi takdirde, tetikleyici olay Hub'ınıza boş olana kadar olayları okumaya devam eder.
> Sonraki tetikleyici yoklama tetikleyicinin özelliklerinde belirttiğiniz yineleme aralığına göre gerçekleşir.

<a name="add-action"></a>

## <a name="add-an-event-hubs-action"></a>Event Hubs Eylem Ekle

Azure Logic apps'te bir [eylem](../logic-apps/logic-apps-overview.md#logic-app-concepts) akışınıza bir tetikleyici veya başka bir eylem izleyen bir adımdır. Bu örnekte, mantıksal uygulama, olay Hub'ındaki yeni olayları denetler bir olay hub'ları tetikleyicisi ile başlatılır.

1. Azure portalı ya da Visual Studio, mantıksal uygulamanızı Logic Apps Tasarımcısı'nda açın. Bu örnek, Azure portalını kullanır.

2. Tetikleyici veya eylem altında seçin **yeni adım** > **Eylem Ekle**.

   Var olan adımlar arasında bir eylem eklemek için bağlantı okun üzerine fareyi hareket ettirin. 
   Artı işaretini seçin (**+**), görünür ve ardından **Eylem Ekle**.

3. Arama kutusuna filtreniz olarak "event hubs" girin.
Eylem listesinden istediğiniz eylemi seçin.

   Bu örnekte, şu eylemi seçin: **Event Hubs - olay gönderme**

   !["Event Hubs - gönderme olayı" seçin](./media/connectors-create-api-azure-event-hubs/find-event-hubs-action.png)

4. Bağlantı ayrıntıları için istenirse [artık Event Hubs bağlantınızı oluşturmak](#create-connection). Veya, bağlantınız zaten varsa, eylem için gerekli bilgileri sağlayın.

   | Özellik | Gerekli | Açıklama |
   |----------|----------|-------------|
   | Olay Hub'ı adı | Evet | Olay göndermek için istediğiniz olay hub'ı seçin |
   | Etkinlik içeriği | Hayır | Göndermek istediğiniz içeriği olayı |
   | Özellikler | Hayır | Uygulama özelliklerini ve değerlerini göndermek için |
   ||||

   Örneğin:

   ![Olay hub'ı adını seçin ve etkinlik içeriği belirtin](./media/connectors-create-api-azure-event-hubs/event-hubs-send-event-action.png)

5. Tasarımcı araç çubuğunda, işiniz bittiğinde seçin **Kaydet**.

<a name="create-connection"></a>

## <a name="connect-to-your-event-hub"></a>Olay Hub'ınıza bağlanın

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)] 

1. Bağlantı bilgileri sorulduğunda, bu ayrıntıları sağlayın:

   | Özellik | Gereklidir | Value | Açıklama |
   |----------|----------|-------|-------------|
   | Bağlantı Adı | Evet | <*Bağlantı adı*> | Adı, bağlantı oluşturmak için |
   | Event Hubs Ad Alanı | Evet | <*Event hubs ad*> | Kullanmak istediğiniz Event Hubs ad alanını seçin. |
   |||||  

   Örneğin:

   ![Olay hub'ı bağlantı oluşturma](./media/connectors-create-api-azure-event-hubs/create-event-hubs-connection-1.png)

   Bağlantı dizesi el ile girmek için seçin **bağlantı bilgilerini el ile girin**. 
   Bilgi [bağlantı dizesini bulma](#permissions-connection-string).

2. Zaten seçili değilse kullanmak için Event hubs'ı ilkesi seçin. **Oluştur**’u seçin.

   ![2. Bölüm olay hub'ı bağlantı oluşturma](./media/connectors-create-api-azure-event-hubs/create-event-hubs-connection-2.png)

3. Bağlantınızı oluşturduktan sonra devam [ekleme olay hub'ları tetiklemek](#add-trigger) veya [Event Hubs ekleme eylemi](#add-action).

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bağlayıcının Swagger dosyası tarafından açıklandığı gibi sınırları, tetikleyiciler ve Eylemler gibi teknik ayrıntılar için bkz [bağlayıcının başvuru sayfası](/connectors/eventhubs/).

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer hakkında bilgi edinin [Logic Apps bağlayıcıları](../connectors/apis-list.md)