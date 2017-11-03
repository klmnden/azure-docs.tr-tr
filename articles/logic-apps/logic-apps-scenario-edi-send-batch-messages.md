---
title: "Bir grubun veya koleksiyonun - Azure mantıksal uygulamaları toplu işlem EDI iletileri | Microsoft Docs"
description: "Toplu işleme logic apps içinde EDI ileti gönderme"
keywords: "Batch, toplu işlem, toplu kodlama"
author: divswa
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/21/2017
ms.author: LADocs; estfan; divswa
ms.openlocfilehash: 837cb0d9595da5b5bd4f01fb4576f75e98ab8912
ms.sourcegitcommit: ccb84f6b1d445d88b9870041c84cebd64fbdbc72
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/14/2017
---
# <a name="send-x12-messages-in-batch-to-trading-partners"></a>Ticaret ortakları için toplu iş gönderme X12 iletileri

İşletmeler için (B2B) senaryolarında ortakları grupları veya toplu iletiler genellikle exchange. Ticari ortaklar grupları veya toplu iletileri göndermek için birden çok öğe ve sonra bu öğeleri toplu iş olarak işlemek için kullanım X12 toplu eylem ile bir toplu iş oluşturabilirsiniz.


X12 için toplu işleme gibi diğer iletiler iletileri kullanır toplu tetikleyici ve eylem. Ayrıca X12 için toplu bir X12 gider iş ortağı veya başka bir hedef geçmeden önce kodla adım. Toplu tetikleyici ve eylem hakkında daha fazla bilgi için bkz: [toplu işlem iletilerinin](logic-apps-batch-process-send-receive-messages.md).

Bu konu, X12 işleminin nasıl gösterir. Bu görevleri gerçekleştirerek toplu olarak iletileri:
* [Öğeleri alır ve bir toplu oluşturan bir mantıksal uygulama oluşturma](#receiver). Bu "alıcı" mantıksal uygulama aşağıdaki eylemleri gerçekleştirir:
 
   * Toplu iş olarak öğeleri serbest bırakmadan önce karşılamak için toplu ad ve sürüm ölçütlerini belirtir.

   * İşler veya belirtilen X12 kullanarak toplu iş öğelerinde kodlar anlaşma veya iş ortağı kimlikleri.

* [Bir toplu iş öğeleri gönderen bir mantıksal uygulama oluşturma](#sender). Bu "gönderen" mantıksal uygulama, var olan bir alıcı mantığı uygulamaya olması gereken toplu işleme için öğeleri göndermek konumu belirtir.


## <a name="prerequisites"></a>Ön koşullar

Bu örnek takip etmek için bu öğeler gerekir:

* Azure aboneliği. Bir aboneliğiniz yoksa [ücretsiz bir Azure hesabı ile başlayabilirsiniz](https://azure.microsoft.com/free/). Ya da [Kullandıkça Öde aboneliğine kaydolabilirsiniz](https://azure.microsoft.com/pricing/purchase-options/).

* Bir [tümleştirme hesabını](logic-apps-enterprise-integration-create-integration-account.md) , daha önce tanımlanan ve Azure aboneliğinizle ilişkili

* En az iki [ortakları](logic-apps-enterprise-integration-partners.md) tümleştirme hesabınızı tanımladığınız. Her iş ortağı X12 (standart taşıyıcı alfa kodu) kullandığından emin olun ortağının özellikleri niteleyicisinde iş kimliği.

* Bir [X12 sözleşmesi](logic-apps-enterprise-integration-x12.md) tümleştirme hesabınızda tanımlanan zaten

<a name="receiver"></a>

## <a name="create-a-logic-app-that-receives-x12-messages-and-creates-a-batch"></a>Bir mantığı oluşturun X12 alan uygulaması iletileri ve toplu oluşturur

Bir toplu iletileri göndermeden önce "alıcı" mantıksal uygulama ile ilk oluşturmalısınız **toplu** tetikleyici. Gönderen mantıksal uygulama oluşturduğunuzda, bu şekilde, bu alıcı mantıksal uygulama seçebilirsiniz. Alıcı için toplu işlem adı belirtin, yayın ölçütleri X12 sözleşmesi ve diğer ayarlar. 


1. İçinde [Azure portal](https://portal.azure.com), bu ada sahip bir mantıksal uygulama oluşturma: "BatchX12Messages".

2. Logic Apps Tasarımcısı'nda eklemek **toplu** , logic app iş akışınızı başlatır tetikleyici. Arama kutusuna "toplu", filtre olarak girin. Bu tetikleyici seçin: **toplu – toplu iletileri**

   ![Toplu tetikleyicisi ekleyin](./media/logic-apps-scenario-EDI-send-batch-messages/add-batch-receiver-trigger.png)

3. Toplu işlem için bir ad ve toplu örneğin serbest ölçütlerini belirtin:

   * **Toplu işlemi adı**: Bu örnekte "TestBatch" olan toplu tanımlamak için kullanılan ad.

   * **Ölçüt yayın**: ileti sayısı, zamanlama veya her ikisi de göre toplu sürüm ölçütlerini.
   
     ![Toplu iş tetikleyici ayrıntılarını sağlayın](./media/logic-apps-batch-process-send-receive-messages/receive-batch-release-criteria.png)

   * **İleti sayısı**: Bu örnekte, "5" olan işleme serbest bırakmadan önce toplu iş olarak tutmak için ileti sayısı.

     ![Toplu iş tetikleyici ayrıntılarını sağlayın](./media/logic-apps-batch-process-send-receive-messages/receive-batch-count-based.png)

   * **Zamanlama**: "her 10 dakika" Bu örnekte işlemek için batch yayın zamanlaması.

     ![Toplu iş tetikleyici ayrıntılarını sağlayın](./media/logic-apps-scenario-EDI-send-batch-messages/receive-batch-schedule-based.png)


4. Başka bir tane eklemek gruplandırılmış kodlar veya iletileri toplu işlemleri ve bir X12 oluşturan eylem toplu ileti. 

   a. Seçin **+ yeni adım** > **Eylem Ekle**.

   b. Arama kutusuna "X 12 toplu", filtre olarak ve bir eylem seçin **X12-toplu kodlamak**. Gibi X12 bağlayıcı kodlamak, eylem kodlama toplu işlemi için birden çok farklılıkları yoktur. Bunlardan birini seçebilirsiniz.

   ![X12 seçin toplu kodlamak eylemi](./media/logic-apps-scenario-EDI-send-batch-messages/add-batch-encode-action.png)
   
5. Yeni eklediğiniz eylem özelliklerini ayarlayın.

   * İçinde **X12 adını anlaşma** kutusunda, anlaşmayı aşağı açılan listeden seçin. Listenizi boş ise, bir bağlantı tümleştirme hesabınıza oluşturulan emin olun.

   * İçinde **BatchName** kutusunda **Tpl** dinamik içerik listesinden alan.
   
   * İçinde **PartitionName** kutusunda **bölüm adı** dinamik içerik listesinden alan.

   * İçinde **öğeleri** kutusunda **toplu öğeleri** dinamik içerik listesinden.

   ![Toplu iş kodla eylem ayrıntıları](./media/logic-apps-scenario-EDI-send-batch-messages/batch-encode-action-details.png)

6. Test amacıyla, toplu ileti göndermek için HTTP Eylem Ekle [isteği depo hizmet](https://requestbin.fullcontact.com/). 

   1. Arama kutusuna filtre olarak "HTTP" girin. Bu eylem seçin: **HTTP - HTTP**
    
      ![HTTP eylemi seçin](./media/logic-apps-scenario-EDI-send-batch-messages/batch-receive-add-http-action.png)

   2. Gelen **yöntemi** listesinde **POST**. İçin **URI** kutusuna istek depo için URI oluşturmak ve bu URI girin. İçinde **gövde** dinamik listesi açıldığında kutusu seçin **gövde** altında **toplu kodlamak anlaşma adı tarafından** bölümü. Görmüyorsanız, **gövde**, seçin **daha fazla** yanına **toplu kodlamak anlaşma adı tarafından**.

      ![HTTP eylem ayrıntılarını sağlayın](./media/logic-apps-scenario-EDI-send-batch-messages/batch-receive-add-http-action-details.png)

7.  Bir alıcı mantıksal uygulama oluşturduğunuza göre mantıksal uygulamanızı kaydedin.

    ![Mantıksal uygulamanızı kaydetme](./media/logic-apps-scenario-EDI-send-batch-messages/save-batch-receiver-logic-app.png)

    > [!IMPORTANT]
    > Bir bölüm 5.000 iletileri veya 80 MB bir sınıra sahiptir. Her iki koşul karşılanıyorsa bile kullanıcı tarafından tanımlanan koşulu karşılanmadı toplu, serbest.

<a name="sender"></a>

## <a name="create-a-logic-app-that-sends-x12-messages-to-a-batch"></a>X12 gönderen bir mantıksal uygulama oluşturma toplu iletileri

Şimdi alıcı mantıksal uygulama tarafından tanımlanan toplu iş öğeleri göndermek bir veya daha fazla mantıksal uygulamalar oluşturun. Gönderen, alıcı mantıksal uygulama ve toplu işlem adı, ileti içeriği ve diğer ayarları belirtin. İsteğe bağlı olarak bu anahtarla öğeleri toplamak için alt kümelerini toplu bölmek için benzersiz bir bölüm anahtarı sağlayabilir.

Gönderen logic apps alıcı logic apps Gönderenler ilgili herhangi bir şey bilmek zorunda değilsiniz sırada öğeleri gönderileceği yeri bilmeniz gerekir.


1. Bu ada sahip başka bir mantıksal uygulama oluşturma: "X12MessageSender". Bu tetikleyici mantığı uygulamanıza ekleme: **istek / yanıt - isteği** 
   
   ![İstek tetikleyicisi ekleyin](./media/logic-apps-scenario-EDI-send-batch-messages/add-request-trigger-sender.png)

2. Bir toplu ileti göndermek için yeni bir adımı ekleyin.

   1. Seçin **+ yeni adım** > **Eylem Ekle**.

   2. Arama kutusuna "toplu", filtre olarak girin. 

3. Bu eylem seçin: **toplu – toplu tetikleyici Logic Apps akışıyla seçmek için ileti gönderme**

   !["Toplu iletileri gönder"'i seçin](./media/logic-apps-scenario-EDI-send-batch-messages/send-messages-batch-action.png)

4. Şimdi bir eylemi olarak artık görünen daha önce oluşturduğunuz "BatchX12Messages" mantıksal uygulamanızı seçin.

   !["Toplu alıcı" mantıksal uygulama seçin](./media/logic-apps-scenario-EDI-send-batch-messages/send-batch-select-batch-receiver.png)

   > [!NOTE]
   > Liste toplu Tetikleyicileri sahip herhangi bir mantıksal uygulamalar da gösterir.

5. Toplu özellikleri ayarlayın.

   * **Toplu işlemi adı**: "TestBatch" Bu örnekte ve çalışma zamanında doğrulanır alıcı mantıksal uygulama tarafından tanımlanan toplu işlem adı.

     > [!IMPORTANT]
     > Alıcı mantıksal uygulama tarafından belirtilen toplu işlem adı eşleşmelidir toplu işlem adı değişmez emin olun.
     > Toplu adının değiştirilmesi neden gönderen mantıksal uygulama başarısız.

   * **İleti içeriği**: Toplu göndermek istediğiniz ileti içeriği
   
   ![Toplu özelliklerini ayarlama](./media/logic-apps-scenario-EDI-send-batch-messages/send-batch-select-batch-properties.png)

6. Mantıksal uygulamanızı kaydedin. Gönderen mantıksal uygulamanızı şimdi bu örneğe benzer görünür:

   ![Gönderen mantıksal uygulamanızı kaydetme](./media/logic-apps-scenario-EDI-send-batch-messages/send-batch-finished.png)

## <a name="test-your-logic-apps"></a>Mantıksal uygulamalarınızı test etme

Toplu işleme çözümünüzü sınamak için post X12 gönderen mantığı uygulamanızdan iletileri için [Postman](https://www.getpostman.com/postman) veya benzer aracı. Bir süre sonra alma başlamalıdır X12 iletileri, istek depo - beş öğelerinin ya da her 10 dakikada toplu olarak ya da tümü aynı bölüm anahtarı.

## <a name="next-steps"></a>Sonraki adımlar

* [Toplu olarak iletilerini işleme](logic-apps-batch-process-send-receive-messages.md) 
* [Visual Studio'da Azure Logic Apps ve işlevleri ile sunucusuz bir uygulama oluşturun](../logic-apps/logic-apps-serverless-get-started-vs.md)
* [Özel durum işleme ve logic apps için hata günlüğü](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
