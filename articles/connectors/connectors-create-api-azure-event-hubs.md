---
title: Azure Event Hubs - Azure mantıksal uygulamaları bağlanma | Microsoft Docs
description: Yönetme ve Azure Event Hubs'a ve Azure mantıksal uygulamaları ile olayları izleme
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 05/21/2018
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 86fc1c3542bea1be840041bb73df15631c066c7e
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35294981"
---
# <a name="monitor-receive-and-send-events-with-azure-event-hubs-and-azure-logic-apps"></a>İzleme, almak ve Azure Event Hubs'a ve Azure mantıksal uygulamaları ile olayları Gönder 

Bu makalede nasıl izlemek ve yönetmek için gönderilen olaylar gösterilmektedir [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) gelen Azure Event Hubs Bağlayıcısı ile bir mantıksal uygulama içinde. Böylece, görevler ve denetleme, gönderme ve olayları olay Hub'ından alma için iş akışlarını otomatikleştirmek mantıksal uygulamalar oluşturabilirsiniz. 

Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>. Logic apps yeniyseniz, gözden [Azure Logic Apps nedir](../logic-apps/logic-apps-overview.md) ve [hızlı başlangıç: ilk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).
Bağlayıcı özgü teknik bilgi için bkz: <a href="https://docs.microsoft.com/connectors/eventhubs/" target="blank">Azure Event Hubs bağlayıcı başvuru</a>.

## <a name="prerequisites"></a>Önkoşullar

* Bir [Azure Event Hubs ad alanı ve olay hub'ı](../event-hubs/event-hubs-create.md)

* Olay Hub'ınızı erişmek istediğiniz mantıksal uygulama. Bir Azure Event Hubs tetikleyicisi ile mantıksal uygulamanızı başlatmak için gereken bir [boş mantıksal uygulama](../logic-apps/quickstart-create-first-logic-app-workflow.md). 

<a name="permissions-connection-string"></a>

## <a name="check-permissions-and-get-connection-string"></a>İzinleri denetleyin ve bağlantı dizesi Al

Olay Hub'ınızı erişmek mantığı uygulamanız için izinleri denetleyin ve için olay hub'ları ad alanınıza bağlantı dizesi alın.

1. <a href="https://portal.azure.com" target="_blank">Azure Portal</a>’da oturum açın. 

2. Olay hub'larınız gidin *ad alanı*, belirli bir Event Hub değil. Ad alanı sayfasında altında **ayarları**, seçin **paylaşılan erişim ilkeleri**. Altında **talep**, sahip olduğunuz onay **Yönet** bu ad alanı için izinleri.

   ![Olay hub'ı ad alanınız için izinleri yönetme](./media/connectors-create-api-azure-event-hubs/event-hubs-namespace.png)

3. Daha sonra el ile bağlantı bilgilerinizi girmek isterseniz, bağlantı dizesi için olay hub'ları ad alanınıza alın. 

   1. Altında **İlkesi**, seçin **RootManageSharedAccessKey**. 

   2. Birincil anahtarın bağlantı dizesini bulun. Kopyala düğmesini seçin ve daha sonra kullanmak için bağlantı dizesini kaydedin.

      ![Olay hub'ları ad alanı bağlantı dizesini kopyalayın](media/connectors-create-api-azure-event-hubs/find-event-hub-namespace-connection-string.png)

      > [!TIP]
      > Bağlantı dizenizi bir belirli olay hub'ı veya olay hub'ları ad alanı ile ilişkili olup olmadığını doğrulamak için bağlantı dizesi yok emin olun `EntityPath` parametresi. Bu parametre bulursanız, bağlantı dizesi belirli olay için bir merkez "varlığı" ve mantıksal uygulamanızı ile kullanılacak doğru dizesi değil.

4. Şimdi devam [bir olay hub'ları tetikleyicisi eklemek](#add-trigger) veya [olay hub'ları Eylem Ekle](#add-action).

<a name="add-trigger"></a>

## <a name="add-an-event-hubs-trigger"></a>Bir olay hub'ları tetikleyicisi ekleyin

Azure Logic Apps içinde her mantıksal uygulama başlamalı ve bir [tetikleyici](../logic-apps/logic-apps-overview.md#logic-app-concepts), belirli bir olay olduğunda etkinleşir gerçekleşen veya belirli bir koşul karşılanıyorsa zaman. Her tetikleyici ateşlenir Logic Apps altyapısı bir mantıksal uygulama örneği oluşturur ve uygulamanızın iş akışı çalışmaya başlar.

Bu örnek, yeni olaylar için olay Hub'ınızı gönderildiğinde mantığı uygulama iş akışı nasıl başlatabilirsiniz gösterir. 

1. Azure portal ya da Visual Studio Logic Apps Tasarımcısı açılır boş mantıksal uygulama oluşturun. Bu örnek, Azure portalını kullanır.

2. Arama kutusuna "event hubs", filtre olarak girin. Tetikleyiciler listeden istediğiniz Tetikleyici seçin. 

   Bu örnek, bu tetikleyici kullanır: **Event Hubs - olayların Event Hub'ında kullanılabilir olduğunda**

   ![Tetikleyici seçin](./media/connectors-create-api-azure-event-hubs/find-event-hubs-trigger.png)

3. Bağlantı ayrıntılarını istenirse [artık olay hub'ları bağlantınızı oluşturmak](#create-connection). Ya da bağlantı zaten varsa, tetikleyici için gerekli bilgileri sağlayın.

   1. Gelen **olay hub'ı adı** listesinde, izlemek istediğiniz olay hub'ı seçin.

      ![Olay hub'ı veya tüketici grubu belirtin](./media/connectors-create-api-azure-event-hubs/select-event-hub.png)

   2. Olay hub'ı denetlemek için tetikleyici hangi sıklıkta güncelleştirileceğini için sıklık ve aralığı seçin.
 
   3. İsteğe bağlı olarak bazı gelişmiş tetikleme seçeneklerini seçmek istediğinizde **Gelişmiş Seçenekleri Göster**.
   
      ![Gelişmiş Seçenekleri tetikleyici](./media/connectors-create-api-azure-event-hubs/event-hubs-trigger-advanced.png)

      | Özellik | Ayrıntılar | 
      |----------|---------| 
      | İçerik türü  | Olayın içerik türünü seçin. Varsayılan "application/octet-stream" dir. |
      | İçerik şeması | İçerik şeması, olay Hub'ından okuma olayları için JSON'da girin. |
      | Tüketici grubu adı | Olay hub'ı girin [tüketici grubu adı](../event-hubs/event-hubs-features.md#consumer-groups) olayları okumak için. Belirtilmezse, varsayılan bir tüketici grubu kullanılır. |
      | En düşük bölüm anahtarı | En düşük girin [bölüm](../event-hubs/event-hubs-features.md#partitions) okumak için kimliği. Varsayılan olarak, tüm bölümleri salt okunurdur. |
      | En fazla bölüm anahtarı | Maksimum girin [bölüm](../event-hubs/event-hubs-features.md#partitions) okumak için kimliği. Varsayılan olarak, tüm bölümleri salt okunurdur. |
      | En fazla olay sayısı | En fazla olay sayısı için bir değer girin. Bu özelliği tarafından belirtilen olayların sayısının bir arasındaki tetikleyici döndürür. |
      |||

4. Tasarımcı araç çubuğunda bitirdiniz seçin **kaydetmek**.

5. Şimdi mantıksal uygulamanızı tetikleyici sonuçlarıyla gerçekleştirmek istediğiniz görevlerle ilgili bir veya daha fazla eylem ekleme devam edin.

> [!NOTE]
> Tüm olay hub'ı Tetikleyiciler *uzun yoklama* tetikleyiciler, tetikleyici başlatıldığında tetikleyici tüm olayları işler ve olay Hub'ınıza görünmesi 30 saniye daha fazla olay için bekler anlamına gelir.
> Hiçbir olay 30 saniye içinde aldıysanız, tetikleyici Çalıştır atlanır. Aksi takdirde, tetikleyici olay Hub'ınızı boş olmadığı sürece olayları okumaya devam eder.
> Sonraki tetikleyici yoklama tetikleyici özelliklerinde belirttiğiniz tekrarlama aralığına göre gerçekleşir.

<a name="add-action"></a>

## <a name="add-an-event-hubs-action"></a>Olay hub'ları Eylem Ekle

Azure mantıksal uygulamaları içinde bir [eylem](../logic-apps/logic-apps-overview.md#logic-app-concepts) tetikleyicinin veya başka bir eylem izler, iş akışınızı bir adımdır. Bu örnekte, olay hub'ınızdaki yeni olaylar denetler bir olay hub'ları tetikleyicisi mantıksal uygulama başlar. 

1. Azure portal ya da Visual Studio Logic Apps Tasarımcısı'nda mantıksal uygulamanızı açın. Bu örnek, Azure portalını kullanır.

2. Tetikleyici veya eylem altında seçin **yeni adım** > **Eylem Ekle**.

   Varolan adımlar arasındaki bir eylem eklemek amacıyla bağlanan oku fareyi hareket ettirin. 
   Artı işaretini seçin (**+**) görünür ve ardından **Eylem Ekle**.

3. Arama kutusuna "event hubs", filtre olarak girin.
Eylemler listesinden istediğiniz eylemi seçin. 

   Bu örnek için bu eylem seçin: **Event Hubs - gönderme olayı**

   !["Event Hubs - gönderme olayı" seçin](./media/connectors-create-api-azure-event-hubs/find-event-hubs-action.png)

4. Bağlantı ayrıntılarını istenirse [artık olay hub'ları bağlantınızı oluşturmak](#create-connection). Ya da bağlantı zaten varsa, eylem için gerekli bilgileri sağlayın.

   | Özellik | Gerekli | Açıklama | 
   |----------|----------|-------------|
   | Olay Hub'ı adı | Evet | Olay göndermek istediğiniz olay hub'ı seçin | 
   | Olay içeriği | Hayır | İçerik göndermek istediğiniz olay için | 
   | Özellikler | Hayır | Uygulama özellikleri ve göndermek için değerleri | 
   |||| 

   Örneğin: 

   ![Olay hub'ı adı seçin ve olay içerik sağlayın](./media/connectors-create-api-azure-event-hubs/event-hubs-send-event-action.png)

5. Tasarımcı araç çubuğunda bitirdiniz seçin **kaydetmek**.

<a name="create-connection"></a>

## <a name="connect-to-your-event-hub"></a>Olay Hub'ınıza bağlanın

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)] 

1. Bağlantı bilgilerini istendiğinde, bu bilgileri sağlayın:

   | Özellik | Gerekli | Değer | Açıklama | 
   |----------|----------|-------|-------------|
   | Bağlantı Adı | Evet | <*Bağlantı adı*> | Ad, bağlantı oluşturmak için |
   | Event Hubs Ad Alanı | Evet | <*Olay hub'ları ad*> | Kullanmak istediğiniz olay hub'ları ad alanını seçin. | 
   |||||  

   Örneğin:

   ![Olay Hub bağlantısı oluşturma](./media/connectors-create-api-azure-event-hubs/create-event-hubs-connection-1.png)

   El ile bağlantı dizesini girmek için tercih **bağlantı bilgilerini el ile girebilirsiniz**. 
   Bilgi [bağlantı dizenizi bulmak nasıl](#permissions-connection-string).

2. Seçili değilse kullanmak için olay hub'ları ilkesini seçin. **Oluştur**’u seçin.

   ![Olay hub'ı bağlantı, bölüm 2 oluşturun](./media/connectors-create-api-azure-event-hubs/create-event-hubs-connection-2.png)

3. Bağlantınızı oluşturduktan sonra devam [ekleme olay hub'ları tetiklemek](#add-trigger) veya [olay hub'ı eklemek eylem](#add-action).

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bağlayıcı'nın Swagger dosyası tarafından açıklandığı gibi tetikleyiciler, Eylemler ve sınırları, gibi teknik ayrıntılar için bkz [bağlayıcı başvuru sayfası](/connectors/eventhubs/). 

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](http://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer hakkında bilgi edinin [Logic Apps bağlayıcılar](../connectors/apis-list.md)