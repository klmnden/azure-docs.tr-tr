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
ms.date: 04/23/2019
tags: connectors
ms.openlocfilehash: 882bae14678d8bfff15b35c63c666a20aeee3d1d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64720046"
---
# <a name="monitor-receive-and-send-events-with-azure-event-hubs-and-azure-logic-apps"></a>İzleme, alabilir ve olayları Azure Event Hubs ve Azure Logic Apps ile gönderme

Bu makale nasıl izleyebilir ve gönderilen olayları yönetmek [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) gelen bir mantıksal uygulama ile Azure Event Hubs bağlayıcısını içinde. Böylece, görevler ve denetimi, gönderme ve olay Hub'ından olaylarını almak için iş akışlarını otomatik hale getiren mantıksal uygulamalar oluşturabilirsiniz. Bağlayıcısı özel teknik bilgiler için bkz. [Azure Event Hubs bağlayıcısını başvuru](https://docs.microsoft.com/connectors/eventhubs/)</a>.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). 

* Bir [Azure Event Hubs ad alanı ve olay hub'ı](../event-hubs/event-hubs-create.md)

* Olay Hub'ınıza erişmek istediğiniz mantıksal uygulaması. Mantıksal uygulamanızı bir Azure Event Hubs tetikleyici ile başlayın, gerek bir [boş mantıksal uygulama](../logic-apps/quickstart-create-first-logic-app-workflow.md).
Logic apps kullanmaya yeni başladıysanız gözden [Azure Logic Apps nedir](../logic-apps/logic-apps-overview.md) ve [hızlı başlangıç: İlk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

<a name="permissions-connection-string"></a>

## <a name="check-permissions-and-get-connection-string"></a>İzinleri denetleyin ve bağlantı dizesini alma

Mantıksal uygulamanızın olay Hub'ınıza erişmek izinleri denetleyin ve Event Hubs ad alanınızın bağlantı dizesini alın.

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Event Hubs'ınız Git *ad alanı*, belirli bir Event Hub değil. 

1. Ad alanı menüsünde altında **ayarları**seçin **paylaşılan erişim ilkeleri**. Altında **talep**, sahip olduğunuz denetimi **Yönet** bu ad alanı için izinler.

   ![Olay hub'ı ad alanınız için izinleri Yönet](./media/connectors-create-api-azure-event-hubs/event-hubs-namespace.png)

1. Bağlantı bilgilerinizi daha sonra el ile girmek istiyorsanız, Event Hubs ad alanınızın bağlantı dizesini alın.

   1. Altında **ilke**, seçin **RootManageSharedAccessKey**.

   1. Birincil anahtarın bağlantı dizesini bulun. Kopyala düğmesini seçin ve daha sonra kullanmak için bağlantı dizesini kaydedin.

      ![Event Hubs ad alanı bağlantı dizesini kopyalayın](media/connectors-create-api-azure-event-hubs/find-event-hub-namespace-connection-string.png)

      > [!TIP]
      > Bağlantı dizeniz ile Event Hubs ad alanınız veya belirli bir olay hub'ı ile ilişkili olup olmadığını doğrulamak için bağlantı dizesi yok emin olun `EntityPath`  parametresi. Bu parametre bulursanız, bağlantı dizesini bir özel olay hub'ı için "varlık" ve mantıksal uygulamanız ile kullanılacak doğru dizesi değil.

1. Artık devam [bir Event hubs'ı tetikleyicisi ekleme](#add-trigger) veya [Event Hubs Eylem Ekle](#add-action).

<a name="add-trigger"></a>

## <a name="add-event-hubs-trigger"></a>Event hubs'ı tetikleyicisi ekleme

Azure Logic Apps'te, her mantıksal uygulama ile başlamalıdır bir [tetikleyici](../logic-apps/logic-apps-overview.md#logic-app-concepts), belirli bir olay harekete geçirilir gerçekleşen veya belirli bir koşul karşılanıyorsa zaman. Her zaman tetikleyici Logic Apps altyapısı bir mantıksal uygulama örneği oluşturur ve uygulamanızın iş akışı çalışmaya başlar.

Bu örnek nasıl yeni olayları olay Hub'ınıza gönderildiğinde bir mantıksal uygulama iş akışı başlatabilirsiniz gösterir. 

1. Azure portalı ya da Visual Studio, Logic Apps Tasarımcısı açılır bir boş mantıksal uygulama oluşturun. Bu örnek, Azure portalını kullanır.

1. Arama kutusuna filtreniz olarak "event hubs" girin. Tetikleyiciler listesinden şu tetikleyiciyi seçin: **Event Hub - Event Hubs olay olduğunda**

   ![Tetikleyici seçin](./media/connectors-create-api-azure-event-hubs/find-event-hubs-trigger.png)

1. Bağlantı ayrıntıları için istenirse [artık Event Hubs bağlantınızı oluşturmak](#create-connection). 

1. Tetikleyici, izlemek istediğiniz olay hub'ı hakkında bilgi sağlar. Daha fazla özelliklerini açın **yeni parametre Ekle** listesi. Bu özellik, bir parametre seçtikten tetikleyici kartı ekler.

   ![Tetikleyici özellikleri](./media/connectors-create-api-azure-event-hubs/event-hubs-trigger.png)

   | Özellik | Gerekli | Açıklama |
   |----------|----------|-------------|
   | **Olay hub'ı adı** | Evet | İzlemek istediğiniz olay hub'ı adı |
   | **İçerik türü** | Hayır | Olayın içerik türü. Varsayılan değer: `application/octet-stream`. |
   | **Tüketici grubu adı** | Hayır | [Olay hub'ı tüketici grubu adı](../event-hubs/event-hubs-features.md#consumer-groups) olayları okumak için kullanılacak. Belirtilmezse, varsayılan bir tüketici grubu kullanılır. |
   | **En yüksek olay sayısı** | Hayır | En yüksek olay sayısı. Tetikleyici arasında bir ve bu özelliği tarafından belirtilen olay sayısını döndürür. |
   | **Aralık** | Evet | İş akışı sıklığı temel alarak çalışan ne sıklıkta tanımlayan pozitif bir tamsayı |
   | **Sıklık** | Evet | Yineleme için zaman birimi |
   ||||

   **Ek Özellikler**

   | Özellik | Gerekli | Açıklama |
   |----------|----------|-------------|
   | **İçerik şeması** | Hayır | İçerik için JSON şeması olayları olay Hub'ından okumak. Örneğin, içerik şeması belirtirseniz, yalnızca şema eşleşen olaylar için mantıksal uygulama tetikleyebilirsiniz. |
   | **En düşük bölüm anahtarı** | Hayır | En düşük girin [bölüm](../event-hubs/event-hubs-features.md#partitions) okumak için kimliği. Varsayılan olarak, tüm bölümleri okunur. |
   | **En yüksek bölüm anahtarı** | Hayır | En yüksek girin [bölüm](../event-hubs/event-hubs-features.md#partitions) okumak için kimliği. Varsayılan olarak, tüm bölümleri okunur. |
   | **Saat dilimi** | Hayır | Bu tetikleyiciyi UTC farkı kabul etmez çünkü yalnızca başlangıç zamanı belirttiğinizde geçerlidir. Uygulamak istediğiniz saat dilimini seçin. <p>Daha fazla bilgi için [oluşturma ve çalıştırma yinelenen görevleri ve Azure Logic Apps ile iş akışlarını](../connectors/connectors-native-recurrence.md). |
   | **Başlangıç saati** | Hayır | Bir başlangıç zamanı şu biçimde belirtin: <p>YYYY-MM-ddTHH bir saat dilimi seçerseniz<p>veya<p>YYYY-AA-saat dilimi seçmezseniz ssZ<p>Daha fazla bilgi için [oluşturma ve çalıştırma yinelenen görevleri ve Azure Logic Apps ile iş akışlarını](../connectors/connectors-native-recurrence.md). |
   ||||

1. Tasarımcı araç çubuğunda, işiniz bittiğinde seçin **Kaydet**.

1. Şimdi mantıksal uygulamanız tetikleme sonuçlarıyla gerçekleştirmek istediğiniz görevleri için bir veya daha fazla eylem ekleyerek devam edin. 

   Örneğin, bir kategori gibi belirli bir değere dayalı olarak olayları filtrelemek için bir koşul ekleyebilirsiniz böylece **Event Hubs - olay gönderme** eylem, koşulu karşılayan olayları gönderir. 

> [!NOTE]
> Tüm olay hub'ı Tetikleyiciler *uzun yoklaması* tetikleyiciler, bir Tetikleyici etkinleştirildiğinde, tetikleyici tüm olayları işler ve olay Hub'ında görünmesi 30 saniye için daha fazla olay bekler anlamına gelir.
> Hiçbir olay 30 saniye içinde alınırsa, tetikleyici çalıştırması atlanır. Aksi takdirde, tetikleyici olay Hub'ınıza boş olana kadar olayları okumaya devam eder.
> Sonraki tetikleyici yoklama tetikleyicinin özelliklerinde belirttiğiniz yineleme aralığına göre gerçekleşir.

<a name="add-action"></a>

## <a name="add-event-hubs-action"></a>Event Hubs Eylem Ekle

Azure Logic apps'te bir [eylem](../logic-apps/logic-apps-overview.md#logic-app-concepts) akışınıza bir tetikleyici veya başka bir eylem izleyen bir adımdır. Bu örnekte, mantıksal uygulama, olay Hub'ındaki yeni olayları denetler bir olay hub'ları tetikleyicisi ile başlatılır.

1. Azure portalı ya da Visual Studio, mantıksal uygulamanızı Logic Apps Tasarımcısı'nda açın. Bu örnek, Azure portalını kullanır.

1. Tetikleyici veya eylem altında seçin **yeni adım**.

   Var olan adımlar arasında bir eylem eklemek için bağlantı okun üzerine fareyi hareket ettirin. 
   Artı işaretini seçin ( **+** ), görünür ve ardından **Eylem Ekle**.

1. Arama kutusuna filtreniz olarak "event hubs" girin.
Eylem listesinden şu eylemi seçin: **Event Hubs - olay gönderme**

   !["Olay Gönder" eylemini seçin](./media/connectors-create-api-azure-event-hubs/find-event-hubs-action.png)

1. Bağlantı ayrıntıları için istenirse [artık Event Hubs bağlantınızı oluşturmak](#create-connection). 

1. Eylem göndermek istediğiniz olayları hakkında bilgi sağlar. Daha fazla özelliklerini açın **yeni parametre Ekle** listesi. Bu özellik, bir parametre seçtikten eylem kartı ekler.

   ![Olay hub'ı adını seçin ve etkinlik içeriği belirtin](./media/connectors-create-api-azure-event-hubs/event-hubs-send-event-action.png)

   | Özellik | Gerekli | Açıklama |
   |----------|----------|-------------|
   | **Olay hub'ı adı** | Evet | Olay göndermek için istediğiniz olay hub'ı |
   | **İçeriği** | Hayır | Göndermek istediğiniz içeriği olayı |
   | **Özellikleri** | Hayır | Uygulama özelliklerini ve değerlerini göndermek için |
   | **Bölüm anahtarı** | Hayır | [Bölüm](../event-hubs/event-hubs-features.md#partitions) olayın gönderileceği kimliği |
   ||||

   Örneğin, Event Hubs Tetikleyiciniz çıktıyı başka bir olay Hub'ına gönderebilirsiniz:

   ![Örnek olay gönderme](./media/connectors-create-api-azure-event-hubs/event-hubs-send-event-action-example.png)

1. Tasarımcı araç çubuğunda, işiniz bittiğinde seçin **Kaydet**.

<a name="create-connection"></a>

## <a name="connect-to-your-event-hub"></a>Olay Hub'ınıza bağlanın

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)] 

1. Bağlantı bilgileri sorulduğunda, bu ayrıntıları sağlayın:

   | Özellik | Gereklidir | Value | Açıklama |
   |----------|----------|-------|-------------|
   | **Bağlantı Adı** | Evet | <*Bağlantı adı*> | Adı, bağlantı oluşturmak için |
   | **Olay hub'ları Namespace** | Evet | <*Event hubs ad*> | Kullanmak istediğiniz Event Hubs ad alanını seçin. |
   |||||  

   Örneğin:

   ![Olay hub'ı bağlantı oluşturma](./media/connectors-create-api-azure-event-hubs/create-event-hubs-connection-1.png)

   Bağlantı dizesi el ile girmek için seçin **bağlantı bilgilerini el ile girin**. 
   Bilgi [bağlantı dizesini bulma](#permissions-connection-string).

2. Zaten seçili değilse kullanmak için Event hubs'ı ilkesi seçin. **Oluştur**’u seçin.

   ![2\. Bölüm olay hub'ı bağlantı oluşturma](./media/connectors-create-api-azure-event-hubs/create-event-hubs-connection-2.png)

3. Bağlantınızı oluşturduktan sonra devam [ekleme olay hub'ları tetiklemek](#add-trigger) veya [Event Hubs ekleme eylemi](#add-action).

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Tetikleyiciler ve Eylemler sınırları, bağlayıcının Openapı'nin açıklandığı gibi teknik ayrıntılar için (önceki adıyla Swagger) dosyası, bkz: [bağlayıcının başvuru sayfası](/connectors/eventhubs/).

## <a name="next-steps"></a>Sonraki adımlar

Diğer hakkında bilgi edinin [Logic Apps bağlayıcıları](../connectors/apis-list.md)