---
title: Azure Service Bus - Azure Logic Apps ile iletiler gönderin ve alın | Microsoft Docs
description: Azure Logic Apps, Azure Service Bus mesajlaşması Kurumsal bulut ayarlama
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.assetid: d6d14f5f-2126-4e33-808e-41de08e6721f
ms.topic: article
tags: connectors
ms.date: 08/25/2018
ms.openlocfilehash: 68378c87e18df874059579445352b8fd1b2b6c13
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62105589"
---
# <a name="exchange-messages-in-the-cloud-with-azure-service-bus-and-azure-logic-apps"></a>Azure Service Bus ve Azure Logic Apps ile bulutta Exchange iletileri

Azure Logic Apps ve Azure Service Bus Bağlayıcısı ile otomatik görevler ve satış gibi verileri aktarmak ve kuruluşunuz için uygulamalar arasında satınalma siparişleri, günlükler ve envanter hareketleri iş akışları oluşturabilirsiniz. Bağlayıcı yalnızca izler, gönderen ve iletileri yönetir, ancak da örneğin kuyrukları, oturumlar, konular, abonelikler ve vb. Eylemler gerçekleştirir:

* İletiler (Otomatik Tamamlama) ulaşması veya alınan (gözlem kilidi) kuyruklar, konular ve konu abonelikleri zamana yönelik İzleyici. 
* İletileri gönderir.
* Oluşturun ve konu abonelikleri silin.
* İletileri kuyruklar ve konu abonelikleri yönetme, örneğin, alma, ertelenmiş, tamamlamak, erteleme, iptal et ve atılacak.
* İletileri ve Kuyruklar ve konu abonelikleri oturumlarda kilitler yenileyin.
* Kuyruklar ve konular oturumlarda kapatın.

Service Bus yanıtlar almak ve çıkış mantıksal uygulamalarınızdaki diğer eylemler için kullanılabilir hale Tetikleyicileri kullanabilirsiniz. Ayrıca, Service Bus eylemleri çıktısını kullanan diğer eylemler olabilir. Service Bus ve Logic Apps için yeni, gözden [Azure Service Bus nedir?](../service-bus-messaging/service-bus-messaging-overview.md) ve [Azure Logic Apps nedir?](../logic-apps/logic-apps-overview.md).

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>. 

* Bir Service Bus ad alanı ve kuyruk gibi Mesajlaşma varlığı. Bu öğelere sahip değilseniz, bilgi nasıl [Service Bus ad alanınızı ve Kuyruk oluşturma](../service-bus-messaging/service-bus-create-namespace-portal.md). 

  Bu öğeler bu öğeleri kullanan logic apps ile aynı Azure aboneliğinde mevcut olması gerekir.

* Hakkında temel bilgilere [mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Service Bus hizmetini kullanmak istediğiniz mantıksal uygulaması. Mantıksal uygulamanız, service bus aynı Azure aboneliğinde mevcut olması gerekir. Bir Service Bus tetikleyicisi ile başlatmak için [boş mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md). Bir hizmet veri yolu eylemi kullanmak için mantıksal uygulamanızı başka bir tetikleyici ile başlar, **yinelenme** tetikleyici.

<a name="permissions-connection-string"></a>

## <a name="check-permissions"></a>İzinleri denetleyin

Mantıksal uygulamanız için Service Bus ad alanınızı erişme izinleri olduğunu doğrulayın. 

1. [Azure Portal](https://portal.azure.com) oturum açın. 

2. Git, Service Bus *ad alanı*. Ad alanı sayfasında altında **ayarları**seçin **paylaşılan erişim ilkeleri**. Altında **talep**, sahip olduğunuz denetimi **Yönet** bu ad alanı için izinler

   ![Service Bus ad alanınız için izinleri Yönet](./media/connectors-create-api-azure-service-bus/azure-service-bus-namespace.png)

3. Service Bus ad alanınız için bağlantı dizesini alın. Mantıksal uygulamanızda bağlantı bilgilerinizi girdiğinizde bu dizesine ihtiyacınız vardır.

   1. Seçin **RootManageSharedAccessKey**. 
   
   1. Birincil bağlantı dizenizi yanındaki Kopyala düğmesini seçin. Daha sonra kullanmak için bağlantı dizesini kaydedin.

      ![Service Bus ad alanı bağlantı dizesini kopyalayın](./media/connectors-create-api-azure-service-bus/find-service-bus-connection-string.png)

   > [!TIP]
   > Bağlantı dizeniz, Service Bus ad alanı veya bir kuyruk gibi bir Mesajlaşma varlığı ile ilişkili olup olmadığını onaylamak için bağlantı dizesi için arama `EntityPath`  parametresi. Bu parametre bulursanız, bağlantı dizesi için belirli bir varlık ve mantıksal uygulamanız ile kullanılacak doğru dizesi değil.

## <a name="add-trigger-or-action"></a>Tetikleyici veya eylemi ekleme

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Oturum [Azure portalında](https://portal.azure.com)ve Logic Apps Tasarımcısı'nda mantıksal uygulamanızı açın, açık değilse.

1. Eklemek için bir *tetikleyici* boş mantıksal uygulama için arama kutusuna filtreniz olarak "Azure Service Bus" girin. Tetikleyiciler listesinde istediğiniz tetikleyicisini seçin. 

   Örneğin, mantıksal uygulamanızın yeni bir öğe bir Service Bus kuyruğuna gönderilen tetiklemek için şu tetikleyiciyi seçin: **(Otomatik Tamamlama) kuyrukta bir ileti alındığında**

   ![Service Bus tetikleyicisi seçin](./media/connectors-create-api-azure-service-bus/select-service-bus-trigger.png)

   > [!NOTE]
   > Bazı tetikleyici bir döndürebilir veya örneğin, tetikleyici iletileri **(Otomatik Tamamlama) kuyrukta veya daha fazla ileti ulaştığında**. Bu Tetikleyiciler tetiklendiğinde bir tetikleyici tarafından belirtilen ileti sayısı arasındaki döndürmeleri **en fazla ileti sayısı** özelliği.

   *Tüm Service Bus Tetikleyicileri uzun yoklama Tetikleyicileri olan*, tetikleyici, tetikleyici tüm iletileri işlediğinden ve daha fazla ileti kuyruk veya konu başlığı aboneliğindeki görünmesi 30 saniye bekler, anlamına gelir. 
   İleti 30 saniye içinde görünmüyorsa, tetikleyici çalıştırması atlanır. 
   Aksi takdirde, tetikleyici, kuyruk veya konu aboneliği boş olana kadar iletileri okumak devam eder. Sonraki yoklama tetikleyici tetikleyicinin özelliklerinde belirtilen yinelenme aralığını temel alır.

1. Eklemek için bir *eylem* var olan bir mantıksal uygulama için şu adımları izleyin: 

   1. Son adım, bir eylem eklemek istediğiniz altında seçin **yeni adım**. 

      Adımlar arasında bir eylem eklemek için işaretçinizi adımlar arasındaki okun üzerine getirin. 
      Artı işaretini seçin (**+**), görünür ve ardından **Eylem Ekle**.

   1. Arama kutusuna filtreniz olarak "Azure Service Bus" girin. 
   Eylemler listesinde, istediğiniz eylemi seçin. 
 
      Örneğin, şu eylemi seçin: **İleti Gönder**

      ![Hizmet veri yolu eylemi seçin](./media/connectors-create-api-azure-service-bus/select-service-bus-send-message-action.png) 

1. Mantıksal uygulamanız için Service Bus ad alanınızı ilk kez bağlanıyorsanız, mantıksal Uygulama Tasarımcısı, artık bağlantı bilgilerinizi ister. 

   1. Bağlantınız için bir ad sağlayın ve Service Bus ad alanınızı seçin.

      ![Service Bus bağlantısı, 1. bölüm oluşturma](./media/connectors-create-api-azure-service-bus/create-service-bus-connection-1.png)

      Bağlantı dizesi yerine el ile girmek için seçin **bağlantı bilgilerini el ile girin**. 
      Bağlantı dizenizi yoksa, bilgi [bağlantı dizesini bulma](#permissions-connection-string).

   1. Artık Service Bus ilkenizi seçin ve ardından **Oluştur**.

      ![Service Bus bağlantı 2. bölüm oluşturma](./media/connectors-create-api-azure-service-bus/create-service-bus-connection-2.png)

1. Bu örnekte, bir kuyruk veya konu gibi istediğiniz Mesajlaşma varlığı seçin. Bu örnekte, Service Bus kuyruğu seçin. 
   
   ![Service Bus kuyruğu seçin](./media/connectors-create-api-azure-service-bus/service-bus-select-queue.png)

1. Tetikleyici veya eylem için gerekli bilgileri sağlayın. Bu örnekte, tetikleyici veya eylemi için ilgili adımları izleyin: 

   * **Örnek tetikleyicinin**: Yoklama aralığı ve kuyruk denetleme sıklığını ayarlayın.

     ![Yoklama aralığı ayarlayın](./media/connectors-create-api-azure-service-bus/service-bus-trigger-details.png)

     İşiniz bittiğinde, istediğiniz eylemler ekleyerek mantıksal uygulamanızın iş akışı oluşturmaya devam edin. Örneğin, yeni bir ileti geldiğinde e-posta gönderen bir eylem ekleyebilirsiniz.
     Tetikleyicinize kuyruğunuzun denetler ve yeni bir iletiyi bulur, mantıksal uygulamanız seçili eylemlerinizi bulundu iletisi için çalışır.

   * **Örnek eylem**: İleti içeriği ve diğer ayrıntıları girin. 

     ![İleti içeriği ve ayrıntılarını sağlayın](./media/connectors-create-api-azure-service-bus/service-bus-send-message-details.png)

     İşiniz bittiğinde, istediğiniz diğer tüm eylemler ekleyerek mantıksal uygulamanızın iş akışı oluşturmaya devam edin. Örneğin, ileti gönderildi onaylayan bir e-posta gönderen bir eylem ekleyebilirsiniz.

1. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet**'i seçin.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Tetikleyiciler ve Eylemler sınırları hakkında teknik ayrıntılar için bağlayıcının Openapı'nin açıklanmıştır (önceki adıyla Swagger) açıklama, bağlayıcının gözden [başvuru sayfası](/connectors/servicebus/).

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer hakkında bilgi edinin [Logic Apps bağlayıcıları](../connectors/apis-list.md)