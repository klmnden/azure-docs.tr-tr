---
title: Bir grubun veya koleksiyonun - Azure mantıksal uygulamaları toplu işlem iletilerinin | Microsoft Docs
description: Toplu işleme logic apps içinde ileti gönderme ve alma
keywords: Batch, toplu işlem
author: jonfancey
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
ms.date: 08/7/2017
ms.author: LADocs; estfan; jonfan
ms.openlocfilehash: 2815ce7fe0e10aadb60eaa77b58e5395fb5c98d8
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35298024"
---
# <a name="send-receive-and-batch-process-messages-in-logic-apps"></a>Gönderme, alma ve logic apps işlem iletileri toplu

Birlikte gruplarındaki iletileri işlemek için veri öğeleri ya da iletileri için gönderebilirsiniz bir *toplu*ve ardından bu öğeleri toplu iş olarak işlem. Bu yaklaşım, veri öğelerini birlikte işlenir ve belirli bir şekilde gruplama emin olmak istediğinizde yararlıdır. 

Öğeleri kullanarak toplu iş olarak almayı mantıksal uygulamalar oluşturabileceğiniz **toplu** tetikleyici. Daha sonra öğeleri kullanarak bir toplu iş gönderme logic apps oluşturabilirsiniz **toplu** eylem.

Bu konu, bu görevleri gerçekleştirerek toplu bir çözümü nasıl oluşturabilirsiniz gösterir: 

* [Alan ve öğeleri toplu iş olarak toplayan bir mantıksal uygulama oluşturma](#batch-receiver). Bu "toplu alıcı" mantıksal uygulama alıcı mantıksal uygulama serbest bırakır ve öğelerini işler önce karşılamak için toplu ad ve sürüm ölçütlerini belirtir. 

* [Bir toplu iş öğeleri gönderen bir mantıksal uygulama oluşturma](#batch-sender). Bu "toplu gönderen" mantıksal uygulama, var olan bir toplu alıcı mantığı uygulamaya olmalıdır öğeleri göndermek konumu belirtir. Ayrıca, "Bölüm" veya bölmek, bu anahtarı temel alt kümeler halinde hedef toplu için ek olarak, müşteri numarası gibi benzersiz bir anahtar belirtebilirsiniz. Böylece, tüm öğeleri aynı anahtarla toplanan ve birlikte işlenebilir. 

## <a name="requirements"></a>Gereksinimler

Bu örnek takip etmek için bu öğeler gerekir:

* Azure aboneliği. Bir aboneliğiniz yoksa [ücretsiz bir Azure hesabı ile başlayabilirsiniz](https://azure.microsoft.com/free/). Ya da [Kullandıkça Öde aboneliğine kaydolabilirsiniz](https://azure.microsoft.com/pricing/purchase-options/).

* Hakkındaki temel bilgileri [mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) 

* Herhangi bir e-posta hesabı [Azure mantıksal uygulamaları tarafından desteklenen e-posta sağlayıcısı](../connectors/apis-list.md)

<a name="batch-receiver"></a>

## <a name="create-logic-apps-that-receive-messages-as-a-batch"></a>Toplu iş olarak iletilerini mantıksal uygulamalar oluşturma

Bir toplu iletileri göndermeden önce "toplu alıcı" mantıksal uygulama ile ilk oluşturmalısınız **toplu** tetikleyici. Gönderen mantıksal uygulama oluşturduğunuzda, bu şekilde, bu alıcı mantıksal uygulama seçebilirsiniz. Alıcı için toplu işlem adı, sürüm ölçütlerini ve diğer ayarları belirtin. 

Gönderen logic apps alıcı logic apps Gönderenler ilgili herhangi bir şey bilmek zorunda değilsiniz sırada öğeleri gönderileceği yeri bilmeniz.

1. İçinde [Azure portal](https://portal.azure.com), bu ada sahip bir mantıksal uygulama oluşturma: "BatchReceiver" 

2. Logic Apps Tasarımcısı'nda eklemek **toplu** , logic app iş akışınızı başlatır tetikleyici. Arama kutusuna "toplu", filtre olarak girin. Bu tetikleyici seçin: **toplu – toplu iletileri**

   ![Toplu tetikleyicisi ekleyin](./media/logic-apps-batch-process-send-receive-messages/add-batch-receiver-trigger.png)

3. Toplu işlem için bir ad ve toplu örneğin serbest ölçütlerini belirtin:

   * **Toplu işlemi adı**: Bu örnekte "TestBatch" olan toplu tanımlamak için kullanılan ad.
   * **Ölçüt yayın**: ileti sayısı, zamanlama veya her ikisi de göre toplu sürüm ölçütlerini.
   
     ![Toplu iş tetikleyici ayrıntılarını sağlayın](./media/logic-apps-batch-process-send-receive-messages/receive-batch-release-criteria.png)

   * **İleti sayısı**: Bu örnekte, "5" olan işleme serbest bırakmadan önce toplu iş olarak tutmak için ileti sayısı.

     ![Toplu iş tetikleyici ayrıntılarını sağlayın](./media/logic-apps-batch-process-send-receive-messages/receive-batch-count-based.png)

   * **Zamanlama**: "5 dakikada bir" Bu örnekte olduğu işlemek için batch yayın zamanlaması.

     ![Toplu iş tetikleyici ayrıntılarını sağlayın](./media/logic-apps-batch-process-send-receive-messages/receive-batch-schedule-based.png)


4. Toplu tetikleyici başlatıldığında, bir e-posta gönderen başka bir eylem ekleyin. Toplu beş öğeler veya kendi son 5 dakika sahip her zaman mantıksal uygulama bir e-posta gönderir.

   1. Toplu tetikleyici altında seçin **+ yeni adım** > **Eylem Ekle**.

   2. Arama kutusuna filtreniz olarak "e-posta" yazın.
   E-posta sağlayıcınız üzerinde bağlı olarak, bir e-posta Bağlayıcısı'nı seçin.
   
      Örneğin, bir iş veya Okul hesabınız varsa, Office 365 Outlook Bağlayıcısı'nı seçin. 
      Gmail hesabınız varsa, Gmail Bağlayıcısı'nı seçin.

   3. Bu eylem, bağlayıcı için seçin: **{*e-posta sağlayıcısı*}-bir e-posta Gönder**

      Örneğin:

      ![E-posta sağlayıcınız için "bir e-posta Gönder" eylemini seçin](./media/logic-apps-batch-process-send-receive-messages/add-send-email-action.png)

5. E-posta sağlayıcınız için daha önce bir bağlantı oluşturmadıysanız, istendiğinde kimlik doğrulaması için e-posta kimlik bilgilerinizi sağlayın. Daha fazla bilgi edinmek [e-posta kimlik bilgilerinizi kimlik doğrulaması](../logic-apps/quickstart-create-first-logic-app-workflow.md).

6. Yeni eklediğiniz eylem özelliklerini ayarlayın.

   * **Alıcı** kutusuna alıcının e-posta adresini girin. 
   Test için kendi e-posta adresinizi kullanabilirsiniz.

   * İçinde **konu** kutusu, ne zaman **dinamik içerik** listesi görüntülenir, seçin **bölüm adı** alan.

     !["Dinamik içerik" listesinden "Bölüm adı"](./media/logic-apps-batch-process-send-receive-messages/send-email-action-details.png)

     Sonraki bölümde, hedef toplu iletileri burada göndermek amacıyla mantıksal kümelerini böler benzersiz bölüm anahtarı belirtebilirsiniz. 
     Her küme gönderen mantıksal uygulama tarafından oluşturulan benzersiz bir numara vardır. 
     Bu özelliği tek bir toplu iş ile birden çok alt kümelerini kullanın ve her alt sağladığınız ada sahip tanımlayın olanak sağlar.

   * İçinde **gövde** kutusu, ne zaman **dinamik içerik** listesi görüntülenir, seçin **ileti kimliği** alan.

     !["Body" için "İleti kimliği" seçin](./media/logic-apps-batch-process-send-receive-messages/send-email-action-details-for-each.png)

     Giriş gönderme e-posta eylemi için bir dizi olduğundan Tasarımcısı otomatik olarak ekler bir **her** geçici döngü **bir e-posta Gönder** eylem. 
     Bu döngü iç eylem her toplu iş öğesinde gerçekleştirir. 
     Toplu tetikleyici beş öğelerine ayarlamak almak için tetikleyici ateşlenir beş e-postaları her zaman.

7.  Bir toplu alıcı mantıksal uygulama oluşturduğunuza göre mantıksal uygulamanızı kaydedin.

    ![Mantıksal uygulamanızı kaydetme](./media/logic-apps-batch-process-send-receive-messages/save-batch-receiver-logic-app.png)

    > [!IMPORTANT]
    > Bir bölüm 5.000 iletileri veya 80 MB bir sınıra sahiptir. Her iki koşul karşılanıyorsa bile kullanıcı tarafından tanımlanan koşulu karşılanmadı toplu, serbest.


<a name="batch-sender"></a>

## <a name="create-logic-apps-that-send-messages-to-a-batch"></a>İleti göndermek için bir toplu iş mantığı uygulamaları oluşturma

Şimdi alıcı mantıksal uygulama tarafından tanımlanan toplu iş öğeleri göndermek bir veya daha fazla mantıksal uygulamalar oluşturun. Gönderen, alıcı mantıksal uygulama ve toplu işlem adı, ileti içeriği ve diğer ayarları belirtin. İsteğe bağlı olarak bu anahtarla öğeleri toplamak için alt kümelerini toplu bölmek için benzersiz bir bölüm anahtarı sağlayabilir.

Gönderen logic apps alıcı logic apps Gönderenler ilgili herhangi bir şey bilmek zorunda değilsiniz sırada öğeleri gönderileceği yeri bilmeniz.

1. Bu ada sahip başka bir mantıksal uygulama oluşturma: "BatchSender"

   1. Arama kutusuna "recurrence", filtre olarak girin. 
   Şu tetikleyiciyi seçin: **Zamanlama - Yinelenme**

      !["Yinelemeyi Zamanla" tetikleyicisi ekleyin](./media/logic-apps-batch-process-send-receive-messages/add-schedule-trigger-batch-receiver.png)

   2. Gönderenin çalıştırma aralığı ve sıklığı ayarlama mantıksal uygulama dakikada.

      ![Sıklık ve aralığı yinelenme tetikleyici için ayarlayın](./media/logic-apps-batch-process-send-receive-messages/recurrence-trigger-batch-receiver-details.png)

2. Bir toplu ileti göndermek için yeni bir adımı ekleyin.

   1. Yineleme tetikleyici altında seçin **+ yeni adım** > **Eylem Ekle**.

   2. Arama kutusuna "toplu", filtre olarak girin. 

   3. Bu eylem seçin: **toplu – toplu tetikleyici Logic Apps akışıyla seçmek için ileti gönderme**

      !["Toplu iletileri gönder"'i seçin](./media/logic-apps-batch-process-send-receive-messages/send-messages-batch-action.png)

   4. Şimdi bir eylemi olarak artık görünen daha önce oluşturduğunuz "BatchReceiver" mantıksal uygulamanızı seçin.

      !["Toplu alıcı" mantıksal uygulama seçin](./media/logic-apps-batch-process-send-receive-messages/send-batch-select-batch-receiver.png)

      > [!NOTE]
      > Liste toplu Tetikleyicileri sahip herhangi bir mantıksal uygulamalar da gösterir.

3. Toplu özellikleri ayarlayın.

   * **Toplu işlemi adı**: "TestBatch" Bu örnekte ve çalışma zamanında doğrulanır alıcı mantıksal uygulama tarafından tanımlanan toplu işlem adı.

     > [!IMPORTANT]
     > Alıcı mantıksal uygulama tarafından belirtilen toplu işlem adı eşleşmelidir toplu işlem adı değişmez emin olun.
     > Toplu adının değiştirilmesi neden gönderen mantıksal uygulama başarısız.

   * **İleti içeriği**: göndermek istediğiniz ileti içeriği. 
   Bu örnekte, geçerli tarih ve saat için toplu Gönder ileti içeriği ekler bu deyim ekleyin:

     1. Zaman **dinamik içerik** listesi görüntülenir, seçin **ifade**. 
     2. İfade girin **utcnow()** ve seçin **Tamam**. 

        !["İletisi içeriği", "İfadesi" seçin. "Utcnow()" girin.](./media/logic-apps-batch-process-send-receive-messages/send-batch-receiver-details.png)

4. Toplu işlem için bir bölüm şimdi ayarlayın. "BatchReceiver" eylemini seçin **Gelişmiş Seçenekleri Göster**.

   * **Bölüm adı**: hedef toplu bölme için kullanılacak bir isteğe bağlı benzersiz bir bölüm anahtarı. Bu örnekte, bir ile beş arasında rastgele bir sayı oluşturan bir deyim ekleyin.
   
     1. Zaman **dinamik içerik** listesi görüntülenir, seçin **ifade**.
     2. Bu ifade girin: **rand(1,6)**

        ![Hedef toplu işlemi için bir bölüm ayarlama](./media/logic-apps-batch-process-send-receive-messages/send-batch-receiver-partition-advanced-options.png)

        Bu **rand** işlevi bir ile beş arasında bir sayı oluşturur. 
        Bu nedenle, bu toplu Bu ifade dinamik olarak ayarlayan beş numaralı, bölümlemesinde.

   * **İleti kimliği**: isteğe bağlı ileti tanımlayıcısı ve boş olduğunda oluşturulan bir GUID. 
   Bu örnekte, bu kutuyu boş bırakın.

5. Mantıksal uygulamanızı kaydedin. Gönderen mantıksal uygulamanızı şimdi bu örneğe benzer görünür:

   ![Gönderen mantıksal uygulamanızı kaydetme](./media/logic-apps-batch-process-send-receive-messages/send-batch-receiver-details-finished.png)

## <a name="test-your-logic-apps"></a>Mantıksal uygulamalarınızı test etme

Toplu çözümünüzü test etmek için bir kaç dakika çalıştıran logic apps bırakın. Bir süre sonra aynı bölüm anahtarına sahip tüm beş gruplarındaki e-postaları almaya başlayın.

BatchSender mantıksal uygulamanızı dakikada çalışır, bir ile beş arasında rastgele bir sayı oluşturur ve iletileri gönderildiği hedef toplu bölüm anahtarı olarak oluşturulan bu numarayı kullanır. Toplu işlem aynı bölüm anahtarına sahip beş öğelerine sahip her zaman BatchReceiver mantıksal uygulamanızı başlatılır ve her ileti için posta gönderir.

> [!IMPORTANT]
> Bitirdiğinizde test, ileti gönderilmesini durdurmak ve gelen aşırı önlemek için BatchSender mantıksal uygulama devre dışı bırakıldığından emin olun.

## <a name="next-steps"></a>Sonraki adımlar

* [JSON kullanarak mantıksal uygulama tanımları oluşturma](../logic-apps/logic-apps-author-definitions.md)
* [Visual Studio'da Azure Logic Apps ve işlevleri ile sunucusuz bir uygulama oluşturun](../logic-apps/logic-apps-serverless-get-started-vs.md)
* [Özel durum işleme ve logic apps için hata günlüğü](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
