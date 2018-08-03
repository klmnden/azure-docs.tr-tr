---
title: Bir grubun veya koleksiyonun - Azure Logic Apps işlem EDI iletilerini toplu | Microsoft Docs
description: EDI iletilerini toplu logic apps'te işleme için Gönder
keywords: Toplu işlem, toplu işlem, toplu kodlama
author: divswa
manager: jeconnoc
editor: ''
services: logic-apps
documentationcenter: ''
ms.assetid: ''
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/21/2017
ms.author: LADocs; estfan; divswa
ms.openlocfilehash: fb15688968cb29039fc669ed6b8685ba64df9e81
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39432142"
---
# <a name="send-x12-messages-in-batch-to-trading-partners"></a>Ticari ortaklar için toplu iş'te iletileri gönderebilir X12

İşletmeler arası (B2B) senaryolarında iş ortakları, genellikle grupları veya toplu olarak mesaj alışverişi. Ticari ortaklar için gruplar veya toplu ileti göndermek için toplu birden çok öğe ve ardından maddelerini toplu olarak işlemek için batch eylemini kullanın X12 oluşturabilirsiniz.


X12 için toplu işlem iletileri, diğer iletiler gibi kullanır toplu tetikleyici ve eylem. Ayrıca batch X12 için x X12 giden iş ortağı veya diğer herhangi bir hedefe geçmeden önce kodla adım. Toplu tetikleyici ve eylem hakkında daha fazla bilgi için bkz: [toplu işlem iletileri](logic-apps-batch-process-send-receive-messages.md).

Bu konu, X12 işleminin nasıl gösterir. Bu görevleri gerçekleştirerek toplu olarak iletileri:
* [Öğeleri alır ve bir toplu iş oluşturan bir mantıksal uygulama oluşturma](#receiver). Bu "alıcı" mantıksal uygulama, aşağıdaki eylemleri gerçekleştirir:
 
   * Toplu iş olarak öğeleri bırakmadan önce karşılamak üzere batch adı ve sürüm ölçütünü belirtir.

   * İşleyen veya belirtilen X12 kullanarak batch öğeleri kodlar sözleşmesi ya da iş ortağı kimlikleri.

* [Öğe için bir batch gönderen bir mantıksal uygulama oluşturma](#sender). Bu "sender" mantıksal uygulama, var olan bir alıcı mantıksal uygulama olması için toplu işleme, öğeleri gönderileceği belirtir.


## <a name="prerequisites"></a>Önkoşullar

Bu örneği takip etmek için bu öğeler gerekir:

* Azure aboneliği. Bir aboneliğiniz yoksa [ücretsiz bir Azure hesabı ile başlayabilirsiniz](https://azure.microsoft.com/free/). Ya da [Kullandıkça Öde aboneliğine kaydolabilirsiniz](https://azure.microsoft.com/pricing/purchase-options/).

* Bir [tümleştirme hesabı](logic-apps-enterprise-integration-create-integration-account.md) zaten tanımlanmış ve Azure aboneliğinizle ilişkili

* En az iki [iş ortakları](logic-apps-enterprise-integration-partners.md) , tümleştirme hesabınızdaki tanımladınız. Her iş ortağı X12 (standart taşıyıcı alfa kodu) kullandığından emin olun olarak bir iş kimliği iş ortağının özelliklerinde niteleyicisi.

* Bir [X12 sözleşmesi](logic-apps-enterprise-integration-x12.md) , tümleştirme hesabında zaten tanımlı

<a name="receiver"></a>

## <a name="create-a-logic-app-that-receives-x12-messages-and-creates-a-batch"></a>Bir mantık oluşturabilirsiniz X12 alan uygulaması iletileri ve bir toplu iş oluşturur

Bir toplu iletileri göndermeden önce ilk "alıcı" mantıksal uygulama ile oluşturmalısınız **Batch** tetikleyici. Bu şekilde, gönderen mantıksal uygulama oluşturduğunuzda bu alıcı mantıksal uygulama seçebilirsiniz. Alıcı için toplu işlem adını belirtin, yayın ölçütleri X12 sözleşmesi ve diğer ayarları. 


1. İçinde [Azure portalında](https://portal.azure.com), bu ada sahip bir mantıksal uygulama oluşturun: "BatchX12Messages".

1. Logic Apps Tasarımcısı'nda ekleme **Batch** mantıksal uygulama iş akışınızı başlatan bir tetikleyici. Arama kutusuna filtreniz olarak "toplu" girin. Şu tetikleyiciyi seçin: **Batch – toplu iletileri**

   ![Toplu tetikleyici ekleme](./media/logic-apps-scenario-EDI-send-batch-messages/add-batch-receiver-trigger.png)

1. Batch için bir ad sağlayın ve toplu işlem, örneğin serbest ölçütlerini belirtin:

   * **Batch adı**: Bu örnekte "TestBatch" olan batch tanımlamak için kullanılan ad.

   * **Yayın ölçütleri**: ileti sayısı, zamanlama veya her ikisi de dayalı toplu sürüm ölçütlerini.
   
     ![Toplu tetikleyici ayrıntılarını sağlayın](./media/logic-apps-batch-process-send-receive-messages/receive-batch-release-criteria.png)

   * **İleti sayısı**: Bu örnekte, "5" olan işleme bırakmadan önce toplu iş olarak tutmak için ileti sayısı.

     ![Toplu tetikleyici ayrıntılarını sağlayın](./media/logic-apps-batch-process-send-receive-messages/receive-batch-count-based.png)

   * **Zamanlama**: "10 dakikada" Bu örnekte olan işleme için batch yayın zamanlaması.

     ![Toplu tetikleyici ayrıntılarını sağlayın](./media/logic-apps-scenario-EDI-send-batch-messages/receive-batch-schedule-based.png)


1. Başka bir gruplandırılmış kodlar veya iletileri toplu işlemleri ve x X12 oluşturur eylem toplu ileti. 

   a. Seçin **+ yeni adım** > **Eylem Ekle**.

   b. Arama kutusuna "X 12 batch" filtreniz olarak ve bir eylem seçin **X12-toplu kodlama**. İster X12 bağlayıcı kodlayın, birden çok eylem kodlama batch çeşitliliğe. Bunlardan birini seçebilirsiniz.

   ![X12 seçin eylem toplu kodlama](./media/logic-apps-scenario-EDI-send-batch-messages/add-batch-encode-action.png)
   
1. Yeni eklediğiniz bir eylemin özelliklerini ayarlayın.

   * İçinde **X12 adını sözleşmesi** kutusunda, anlaşmayı aşağı açılan listeden seçin. Listeniz boşsa, tümleştirme hesabınıza bir bağlantı oluşturulduğundan emin olun.

   * İçinde **BatchName** kutusunda **Tpl** dinamik içerik listesinden alan.
   
   * İçinde **PartitionName** kutusunda **bölüm adı** dinamik içerik listesinden alan.

   * İçinde **öğeleri** kutusunda **toplu öğeleri** dinamik içerik listesinden.

   ![Batch kodla eylem ayrıntıları](./media/logic-apps-scenario-EDI-send-batch-messages/batch-encode-action-details.png)

1. Test amacıyla, toplu ileti göndermek için HTTP Eylem Ekle [istek depo hizmeti](https://requestbin.fullcontact.com/). 

   1. Arama kutusuna filtreniz olarak "HTTP" girin. Şu eylemi seçin: **HTTP - HTTP**
    
      ![HTTP eylemi seçin](./media/logic-apps-scenario-EDI-send-batch-messages/batch-receive-add-http-action.png)

   1. Gelen **yöntemi** listesinden **POST**. İçin **URI** kutusuna, istek depo için bir URI oluşturmayı ve bu URI'yi girin. İçinde **gövdesi** dinamik listesi açıldığında kutusunu seçin **gövdesi** altında **sözleşme adına göre toplu kodlama** bölümü. Görmüyorsanız **gövdesi**, seçin **daha fazla bilgi bkz** yanındaki **sözleşme adına göre toplu kodlama**.

      ![HTTP eylem ayrıntılarını sağlayın](./media/logic-apps-scenario-EDI-send-batch-messages/batch-receive-add-http-action-details.png)

1.  Bir alıcı mantıksal uygulama oluşturdunuz, mantıksal uygulamanızı kaydedin.

    ![Mantıksal uygulamanızı kaydetme](./media/logic-apps-scenario-EDI-send-batch-messages/save-batch-receiver-logic-app.png)

    > [!IMPORTANT]
    > Bir bölüm 5.000 iletileri veya 80 MB sınırı vardır. Her iki koşul karşılanıyorsa, hatta kullanıcı tanımlı koşul karşılanmadığında batch, kullanıma.

<a name="sender"></a>

## <a name="create-a-logic-app-that-sends-x12-messages-to-a-batch"></a>X12 gönderen bir mantıksal uygulama oluşturma bir toplu olarak işlenecek iletiler

Artık alıcı mantıksal uygulama tarafından tanımlanan toplu iş öğelerini göndermek bir veya daha fazla mantıksal uygulamalar oluşturun. Gönderenin alıcı mantıksal uygulama ve toplu iş adı, ileti içeriği ve diğer ayarları belirtin. İsteğe bağlı olarak, toplu iş öğeleri bu anahtarla toplamak için alt kümelerini bölmek için benzersiz bir bölüm anahtarı sağlayabilir.

Gönderen logic apps, alıcı logic apps Gönderenler ilgili hiçbir şeyi bilmeniz gerekmez ancak öğeleri gönderileceği bilmeniz gerekir.


1. Bu ada sahip başka bir mantıksal uygulama oluşturun: "X12MessageSender". Mantıksal uygulamanız Bu tetikleyici ekleme: **istek / yanıt - istek** 
   
   ![İstek tetikleyicisi Ekle](./media/logic-apps-scenario-EDI-send-batch-messages/add-request-trigger-sender.png)

1. Bir toplu ileti göndermek için yeni bir adım ekleyin.

   1. Seçin **+ yeni adım** > **Eylem Ekle**.

   1. Arama kutusuna filtreniz olarak "toplu" girin. 

1. Şu eylemi seçin: **toplu – toplu işlem tetikleyicisi içeren bir Logic Apps iş akışı seçin için ileti gönderme**

   !["Toplu iletileri gönder" öğesini seçin](./media/logic-apps-scenario-EDI-send-batch-messages/send-messages-batch-action.png)

1. Artık bir eylemi olarak artık görünen daha önce oluşturduğunuz "BatchX12Messages" mantıksal uygulamanızı seçin.

   !["Toplu alıcı" mantıksal uygulama seçin](./media/logic-apps-scenario-EDI-send-batch-messages/send-batch-select-batch-receiver.png)

   > [!NOTE]
   > Liste, batch tetikleyicilere sahip diğer logic apps de gösterir.

1. Batch özelliklerini ayarlayın.

   * **Batch adı**: Bu örnekte "TestBatch" olan ve çalışma zamanında doğrulanır alıcı mantıksal uygulama tarafından tanımlanan toplu işlem adı.

     > [!IMPORTANT]
     > Alıcı mantıksal uygulama tarafından belirtilen toplu iş adı eşleşmelidir batch adı değişmez emin olun.
     > Batch adının değiştirilmesi, gönderen neden başarısız için mantıksal uygulama.

   * **İleti içeriği**: Toplu göndermek istediğiniz iletinin içeriği
   
   ![Batch özelliklerini ayarlama](./media/logic-apps-scenario-EDI-send-batch-messages/send-batch-select-batch-properties.png)

1. Mantıksal uygulamanızı kaydedin. Gönderen mantıksal uygulamanız bu örnektekine benzer görünür:

   ![Gönderen mantıksal uygulamanızı kaydetme](./media/logic-apps-scenario-EDI-send-batch-messages/send-batch-finished.png)

## <a name="test-your-logic-apps"></a>Mantıksal uygulamalarınızı test edin

Toplu işlem çözümünüzü test etmek için post X12 gönderen mantıksal uygulamanızdan iletileri [Postman](https://www.getpostman.com/postman) veya benzer bir araç. Bir süre sonra başlama başlamalıdır X12 iletilerini, istek Kutusu'ndaki - beş öğelerin veya her 10 dakikada toplu olarak ya da tümü aynı bölüm anahtarına.

## <a name="next-steps"></a>Sonraki adımlar

* [Toplu olarak işlem iletileri](logic-apps-batch-process-send-receive-messages.md) 
* [Visual Studio'da Azure Logic Apps ve işlevler ile sunucusuz uygulama oluşturma](../logic-apps/logic-apps-serverless-get-started-vs.md)
* [Logic apps için hata günlüğünü ve özel durum işleme](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
