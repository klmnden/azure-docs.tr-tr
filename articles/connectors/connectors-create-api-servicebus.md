---
title: Azure mantıksal uygulamaları için Azure Service Bus Mesajlaşma yukarı ayarlama | Microsoft Docs
description: İleti gönderme ve logic apps ile Azure Service Bus kullanarak alma
services: logic-apps
documentationcenter: ''
author: ecfan
manager: jeconnoc
editor: ''
tags: connectors
ms.assetid: d6d14f5f-2126-4e33-808e-41de08e6721f
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: logic-apps
ms.date: 02/06/2018
ms.author: ladocs
ms.openlocfilehash: aa6ab10dded541b352bdb7c8c3a47dbbbfe6a15c
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35295477"
---
# <a name="send-and-receive-messages-with-the-azure-service-bus-connector"></a>Azure Service Bus Bağlayıcısı ile ileti gönderme ve alma

Mantıksal uygulamanız ile ileti gönderme ve alma için bağlanın [Azure Service Bus](https://azure.microsoft.com/services/service-bus/). Bir kuyruk gönderme gibi eylemleri gerçekleştirmek için bir konu, bir kuyruktan alma, gönderip bir abonelik. Daha fazla bilgi edinmek [Azure Service Bus](../service-bus-messaging/service-bus-messaging-overview.md) ve [nasıl çalışır Logic Apps için fiyatlandırma tetikler](../logic-apps/logic-apps-pricing.md).

## <a name="prerequisites"></a>Önkoşullar

Hizmet veri yolu Bağlayıcısı'nı kullanmadan önce bu öğeler aynı Azure aboneliğindeki mevcut olması gerekir ve böylece birbirlerine görünür olması gerekir:

* A [Service Bus ad alanı ve bir sıraya gibi Mesajlaşma varlığı](../service-bus-messaging/service-bus-create-namespace-portal.md)
* A [mantıksal uygulama](../logic-apps/quickstart-create-first-logic-app-workflow.md)

<a name="permissions-connection-string"></a>

## <a name="connect-to-azure-service-bus"></a>Azure hizmet veri yoluna bağlayın

Oluşturmak zorunda mantıksal uygulamanızı herhangi bir hizmet erişebilmeniz için önce bir [ *bağlantı* ](./connectors-overview.md) mantıksal uygulamanızı ve henüz yapmadıysanız hizmetin arasında. Bu bağlantı verilere erişmek için mantıksal uygulamanızı yetkilendirir. Mantıksal uygulamanızı Service Bus hesabınıza erişmek izinleri denetleyin.

1. [Azure portalı](https://portal.azure.com "Azure portalı") oturumunu açın. 

2. Hizmet veri yolu gidin *ad alanı*, bir özel "Mesajlaşma varlıkla değil". Ad alanı sayfasında altında **ayarları**, seçin **paylaşılan erişim ilkeleri**. Altında **talep**, sahip olduğunuz onay **Yönet** bu ad alanı için izinleri.

   ![Hizmet veri yolu ad alanınız için izinleri yönetme](./media/connectors-create-api-azure-service-bus/azure-service-bus-namespace.png)

3. Daha sonra el ile bağlantı bilgilerinizi girmek isterseniz, bağlantı dizesi için hizmet veri yolu ad alanınız alın. Seçin **RootManageSharedAccessKey**. Birincil anahtar bağlantı dizenizi yanındaki Kopyala düğmesini seçin. Daha sonra kullanmak için bağlantı dizesini kaydedin.

   ![Hizmet veri yolu ad alanı bağlantı dizesini kopyalayın](./media/connectors-create-api-azure-service-bus/find-service-bus-connection-string.png)

   > [!TIP]
   > Bağlantı dizenizi hizmet veri yolu ad alanınız ile veya belirli bir varlık ile ilişkili olup olmadığını doğrulamak için bağlantı dizesini kontrol `EntityPath` parametresi. Bu parametre bulursanız, bağlantı dizesi için belirli bir varlık ve mantıksal uygulamanızı ile kullanılacak doğru dizesi değil.

## <a name="trigger-workflow-when-your-service-bus-gets-new-messages"></a>Hizmet veri yolu yeni iletiler aldığında iş akışını tetikleyen

A [ *tetikleyici* ](../logic-apps/logic-apps-overview.md#logic-app-concepts) mantıksal uygulamanızı bir iş akışı başlatır bir olaydır. Yeni iletiler, Service Bus hizmetine gönderildiğinde bir iş akışını başlatmak için bu iletiler algılar tetikleyici eklemek için aşağıdaki adımları izleyin.

1. İçinde [Azure portal](https://portal.azure.com "Azure portal"), var olan mantıksal uygulamanızı Git veya boş mantıksal uygulama oluşturma.

2. Logic Apps Tasarımcısı'nda, "service bus" arama kutusuna, filtre olarak girin. Seçin **Service Bus** bağlayıcı. 

   ![Hizmet veri yolu bağlayıcıyı seçin](./media/connectors-create-api-azure-service-bus/select-service-bus-connector.png) 

3. Kullanmak istediğiniz Tetikleyici seçin. Örneğin, yeni bir öğe için Service Bus kuyruğuna gönderilen, bir mantıksal uygulama çalıştırmak için bu Tetikleyici seçin: **(Otomatik Tamamlama) sıraya bir ileti alındığında, Service Bus -**

   ![Hizmet veri yolu Tetikleyici seçin](./media/connectors-create-api-azure-service-bus/select-service-bus-trigger.png)

   > [!NOTE]
   > Bazı tetikler dönüş biri veya iletileri gibi *Service Bus - bir veya daha fazla ileti (Otomatik Tamamlama) kuyrukta geldiğinde* tetikleyici.
   > Bu tetikleyicileri tetiklendiğinde biri ve tetikleyici tarafından belirtilen ileti sayısı arasında döndürmeleri **en fazla ileti sayısı** özelliği.

   1. Hizmet veri yolu ad alanınıza bağlantı sahip değilseniz, bu bağlantı artık oluşturmanız istenir. Bağlantınızı bir ad verin ve kullanmak istediğiniz hizmet veri yolu ad alanını seçin.

      ![Hizmet veri yolu bağlantı oluşturma](./media/connectors-create-api-azure-service-bus/create-service-bus-connection-1.png)

      Veya el ile bağlantı dizesini girmek için seçin **bağlantı bilgilerini el ile girebilirsiniz**. 
      Bilgi [bağlantı dizenizi bulmak nasıl](#permissions-connection-string).
      

   2. Şimdi kullanın ve seçmek için Service Bus ilkesini seçin **oluşturma**.

      ![Hizmet veri yolu bağlantı, bölüm 2 oluşturun](./media/connectors-create-api-azure-service-bus/create-service-bus-connection-2.png)

4. Kullanın ve aralığı ve sıra denetlemek ne zaman sıklığını ayarlamak için hizmet veri yolu kuyruğu seçin.

   ![Hizmet veri yolu kuyruğu seçin, yoklama aralığını ayarlayın](./media/connectors-create-api-azure-service-bus/select-service-bus-queue.png)

   > [!NOTE]
   > Tüm Service Bus Tetikleyiciler **uzun yoklama** tetikleyici başlatıldığında tetikleyici tüm iletileri işlediğinden kuyruk veya konu başlığı aboneliğinde görünmesi daha fazla ileti 30 saniye bekler anlamına gelir ve tetikler.
   > Hiçbir ileti 30 saniye içinde aldıysanız, tetikleyici Çalıştır atlanır. Aksi takdirde, tetikleyici, kuyruk veya konu başlığı aboneliği boş olana kadar iletileri okumak devam eder.
   > Sonraki tetikleyici yoklama tetikleyici özelliklerinde belirtilen yinelenme aralığını temel alır.

5. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet**'i seçin.

Şimdi, mantıksal uygulamanızı seçilen sıra denetler ve yeni bir ileti bulduğunda tetikleyici bulundu iletisi için mantığı uygulamanızda eylemleri çalıştırır.

## <a name="send-messages-from-your-logic-app-to-your-service-bus"></a>Mantıksal uygulamanızı, hizmet veri yolu ileti gönderme

[*Eylem*](../logic-apps/logic-apps-overview.md#logic-app-concepts), mantıksal uygulama iş akışınız tarafından gerçekleştirilen bir görevdir. Mantıksal uygulamanıza bir tetikleyici ekledikten sonra, bu tetikleyici tarafından oluşturulan işlemleri gerçekleştirmek için bir eylem ekleyebilirsiniz. Mantıksal uygulamanızı varlıktan Mesajlaşma, Service Bus ileti göndermek için aşağıdaki adımları izleyin.

1. Logic Apps Tasarımcısı'nda, tetikleyici altında seçin **+ yeni adım** > **Eylem Ekle**.

2. Arama kutusuna "service bus", filtre olarak girin. Bu bağlayıcıyı seçin: **hizmet veri yolu**

   ![Hizmet veri yolu bağlayıcıyı seçin](./media/connectors-create-api-azure-service-bus/select-service-bus-connector-for-action.png) 

3. Bu eylem seçin: **Service Bus - ileti gönderir**

   !["Service Bus - ileti gönder" seçin](./media/connectors-create-api-azure-service-bus/select-service-bus-send-message-action.png)

4. İletiyi göndermek nereye için kuyruk veya konu adı Mesajlaşma varlığı seçin. Ardından, ileti içeriği ve başka ayrıntılar girin.

   ![Mesajlaşma varlığı seçin ve ileti ayrıntılarını sağlayın](./media/connectors-create-api-azure-service-bus/service-bus-send-message-details.png)    

5. Mantıksal uygulamanızı kaydedin. 

Şimdi mantıksal uygulamanızı iletileri gönderen bir eylem kurdu. 

## <a name="connector-specific-details"></a>Bağlayıcı özgü ayrıntıları

Tetikleyiciler ve Eylemler Swagger dosyası ve herhangi bir sınır tarafından tanımlanan hakkında daha fazla bilgi edinmek için gözden [Bağlayıcısı ayrıntıları](/connectors/servicebus/).

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](http://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinmek [Azure mantıksal uygulamaları için diğer bağlayıcıları](../connectors/apis-list.md)