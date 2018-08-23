---
title: Bir grubun veya koleksiyonun - Azure Logic Apps toplu işlem iletileri | Microsoft Docs
description: Gönderin ve Azure Logic apps'te toplu olarak ileti alma
services: logic-apps
ms.service: logic-apps
author: divyaswarnkar
ms.author: divswa
manager: jeconnoc
ms.topic: article
ms.date: 08/19/2018
ms.reviewer: estfan, LADocs
ms.suite: integration
ms.openlocfilehash: 5190e5d4191cb4d07b000920dd1be1b53e679350
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2018
ms.locfileid: "42057457"
---
# <a name="send-receive-and-batch-process-messages-in-azure-logic-apps"></a>Toplu işlem iletileri Azure Logic apps'te gönderme ve alma

Gönderin ve ileti grup olarak birlikte belirli bir şekilde işlemek üzere iletilere toplayan toplu bir çözüm oluşturabilirsiniz. bir *batch* serbest bırakarak ve toplu ileti işleme için belirtilen ölçütlerinize karşılanana kadar. Toplu işleme, ne sıklıkta mantıksal uygulamanızı iletileri işleyen azaltabilir. Bu makale aynı Azure aboneliğinde, iki mantıksal uygulamalar oluşturarak toplu bir çözüm oluşturmak nasıl Azure bölgesini ve bu belirli bir sıraya göre aşağıdaki: 

* ["Toplu alıcı"](#batch-receiver) kabul eder ve belirtilen ölçütlerinize karşılanıyorsa kadar serbest ve bu iletileri işlemek için bir toplu iş içine iletileri toplayan mantıksal uygulama.

  Batch gönderen oluşturduğunuzda, daha sonra batch hedef seçebilmeniz için batch alıcı ilk oluşturduğunuzdan emin olun.

* Bir veya daha fazla ["toplu sender"](#batch-sender) mantıksal uygulamalar, daha önce oluşturulan batch alıcısına göndermek. 

   Ayrıca, müşteri numarası gibi benzersiz bir anahtar belirtin, *bölümleri* veya hedef batch bu anahtarına göre mantıksal alt kümelerini böler. Bu şekilde, alıcı uygulamanın tüm öğeleri aynı anahtarla toplamak ve birlikte işlem.

Batch alıcı ve gönderen batch aynı Azure aboneliğinde paylaştığınızdan emin olun *ve* Azure bölgesi. Yoksa, bunların birbirlerine görünmeyen olmadığınızdan batch gönderen oluşturduğunuzda, batch alıcı seçemezsiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu örneği takip etmek için bu öğeler gerekir:

* Azure aboneliği. Bir aboneliğiniz yoksa [ücretsiz bir Azure hesabı ile başlayabilirsiniz](https://azure.microsoft.com/free/). Veya, [için Kullandıkça Öde aboneliğine kaydolun](https://azure.microsoft.com/pricing/purchase-options/).

* Herhangi bir e-posta hesabı [Azure Logic Apps tarafından desteklenen e-posta sağlayıcısı](../connectors/apis-list.md)

* Hakkında temel bilgilere [mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) 

* Azure portalı yerine Visual Studio kullanmak üzere emin olursunuz [Logic Apps ile çalışmak için Visual Studio'yu ayarlama](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md).

<a name="batch-receiver"></a>

## <a name="create-batch-receiver"></a>Batch alıcısı oluşturma

Bir toplu iletileri göndermeden önce toplu iletiler göndermek burada hedef olarak önce mevcut olması gerekir. Bu nedenle ilk olarak, ile başlar "toplu alıcı" mantıksal uygulama oluşturma **Batch** tetikleyici. "Toplu sender" mantıksal uygulama oluşturduğunuzda, bu şekilde, batch alıcı mantıksal uygulama seçebilirsiniz. Batch alıcı, ileti belirtilen ölçütlerinize karşılanıyorsa kadar serbest bırakarak ve bu iletileri için toplama devam eder. Batch alıcılar batch Gönderenler ilgili hiçbir şeyi bilmeniz gerekmez ancak toplu Gönderenler iletileri burada gönderdikleri hedef bilmeniz gerekir. 

1. İçinde [Azure portalında](https://portal.azure.com) veya Visual Studio, bu ada sahip bir mantıksal uygulama oluşturun: "BatchReceiver" 

2. Logic Apps Tasarımcısı'nda ekleme **Batch** mantıksal uygulama iş akışınızı başlatan bir tetikleyici. Arama kutusuna filtreniz olarak "toplu" girin. Şu tetikleyiciyi seçin: **toplu iletiler**

   !["İletileri toplu olarak" tetikleyici ekleme](./media/logic-apps-batch-process-send-receive-messages/add-batch-receiver-trigger.png)

3. Batch, alıcı özellikleri ayarlayın: 

   | Özellik | Açıklama | 
   |----------|-------------|
   | **Toplu iş modu** | - **Satır içi**: yayın ölçütü toplu tetikleyici içinde tanımlama <br>- **Tümleştirme hesabı**: birden çok yayın ölçütleri yapılandırmalarını tanımlamak için bir [tümleştirme hesabı](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md). Bir tümleştirme hesabıyla bu yapılandırmalar, tek bir yerde yerine ayrı mantıksal uygulamalar bulundurabilirsiniz. | 
   | **Toplu işlem adı** | Bu örnekte "TestBatch" olması ve yalnızca, batch, adı **satır içi** toplu iş modu |  
   | **Yayın ölçütü** | Yalnızca geçerli **satır içi** toplu iş modu ve her toplu işin işlenmeden önce karşılamak için ölçütü belirtir: <p>- **İleti sayısı tabanlı**: Toplu işlemde, örneğin, 10 ileti toplanacak ileti sayısı <br>- **Bağlı boyutu**: bayt, örneğin, 100 MB maksimum toplu iş boyutu <br>- **Zamanlama tabanlı**: aralık ve sıklık arasında toplu sürümleri, örneğin, 10 dakika. Başlangıç tarihi ve saati belirtebilirsiniz. <br>- **Tümünü Seç**: belirtilen ölçütleri kullanın. | 
   ||| 
   
   Bu örnekte, tüm ölçütlerini seçer:

   ![Toplu tetikleyici ayrıntılarını sağlayın](./media/logic-apps-batch-process-send-receive-messages/batch-receiver-criteria.png)

4. Şimdi her toplu işi bir veya daha fazla eylemler ekleyin. 

   Bu örnekte, batch Tetikleyici etkinleştirildiğinde bir e-posta gönderen bir eylem ekleyin. 
   Tetikleyici çalışır ve batch 10 ileti yok, 10 MB ulaşır ya da 10 dakika sonra başarılı bir e-posta gönderir.

   1. Toplu tetikleyici altında seçin **yeni adım**.

   2. Arama kutusuna filtreniz olarak "e-posta gönder" yazın.
   E-posta sağlayıcınız bağlı olarak, bir e-posta bağlayıcısını seçin.
      
      Örneğin, bir kişisel hesap gibi varsa @outlook.com veya @hotmail.com, Outlook.com bağlayıcısını seçin. 
      Gmail hesabına varsa, Gmail bağlayıcısını seçin. 
      Bu örnekte Office 365 Outlook kullanılmaktadır. 

   3. Şu eylemi seçin: **- e-posta gönderin <*e-posta sağlayıcısı*>**

      Örneğin:

      ![E-posta sağlayıcınız için "e-posta Gönder" eylemini seçin](./media/logic-apps-batch-process-send-receive-messages/batch-receiver-send-email-action.png)

5. İstenirse, e-posta hesabınızda oturum açın. 

6. Eklediğiniz bir eylemin özelliklerini ayarlayın.

   * **Alıcı** kutusuna alıcının e-posta adresini girin. 
   Test için kendi e-posta adresinizi kullanabilirsiniz.

   * İçinde **konu** kutusu, dinamik içerik listesi göründüğünde seçin **bölüm adı** alan.

     ![Dinamik içerik listesinden, "Bölüm adı" seçin](./media/logic-apps-batch-process-send-receive-messages/send-email-action-details.png)

     Bir sonraki bölümde, hedef toplu işlem iletileri burada gönderebilir için mantıksal alt kümelerini ayıran benzersiz bir bölüm anahtarının belirtebilirsiniz. 
     Her batch gönderen mantıksal uygulama tarafından oluşturulan benzersiz bir numara ayarlanmış. 
     Bu özellik, birden çok alt kümeleri ile tek bir toplu iş kullanın ve her bir alt kümedeki, sağladığınız adla tanımlamak olanak tanır.

     > [!IMPORTANT]
     > Bir bölüm 5.000 iletileri veya 80 MB sınırı vardır. Her iki koşul karşılanıyorsa bile, tanımlanmış sürüm koşul karşılanmasa, Logic Apps toplu işlem yayın yapabilir.

   * İçinde **gövdesi** kutusu, dinamik içerik listesi göründüğünde seçin **ileti kimliği** alan. 

     Bu eylem bir dizi girdi olarak kabul ettiğinden Logic Apps Tasarımcısı'nda otomatik olarak e-posta gönderme eylemi bir "For each" döngüsü ekler. 
     Bu döngü toplu işlemde her ileti için bir e-posta gönderir. 
     Toplu tetikleyici 10 iletileri olarak ayarlandığında, alabilmeniz tetikleyici 10 e-postaları her zaman.

     !["Body" için "İleti kimliği" seçin](./media/logic-apps-batch-process-send-receive-messages/send-email-action-details-for-each.png)

7.  Mantıksal uygulamanızı kaydedin. Bir batch alıcı oluşturdunuz.

    ![Mantıksal uygulamanızı kaydetme](./media/logic-apps-batch-process-send-receive-messages/save-batch-receiver-logic-app.png)

8. Visual Studio kullanıyorsanız, emin [batch alıcı mantıksal uygulamanızı Azure'a dağıtma](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md#deploy-logic-app-to-azure). Aksi halde, batch gönderen oluşturduğunuzda, batch alıcı seçemezsiniz.

<a name="batch-sender"></a>

## <a name="create-batch-sender"></a>Batch göndereni oluşturma

Şimdi toplu alıcı mantıksal uygulamaya iletileri gönderen bir veya daha fazla batch gönderen mantıksal uygulamalar oluşturun. Her batch gönderici batch alıcı ve toplu iş adı, ileti içeriği ve diğer ayarları belirtin. İsteğe bağlı olarak, bu anahtarla iletileri toplamak için mantıksal alt kümelerini toplu bölmek için benzersiz bir bölüm anahtarı sağlayabilir. 

* Seçtiğiniz emin zaten [batch Alıcınız oluşturulan](#batch-receiver) , batch gönderen oluşturduğunuzda, mevcut batch alıcı hedef toplu seçebilmeniz için. Batch alıcılar batch Gönderenler ilgili hiçbir şeyi bilmeniz gerekmez ancak toplu Gönderenler iletilerin gönderileceği bilmeniz gerekir. 

* Batch alıcı ve gönderen batch aynı Azure bölgesindeki paylaştığınızdan emin olun *ve* Azure aboneliği. Yoksa, bunların birbirlerine görünmeyen olmadığınızdan batch gönderen oluşturduğunuzda, batch alıcı seçemezsiniz.

1. Bu ada sahip başka bir mantıksal uygulama oluşturun: "BatchSender"

   1. Arama kutusuna filtreniz olarak "yinelenme" girin. 
   Şu tetikleyiciyi seçin: **yinelenme - zamanlama**

      !["– Zamanlama yinelenme" tetikleyicisi Ekle](./media/logic-apps-batch-process-send-receive-messages/add-schedule-trigger-batch-sender.png)

   2. Gönderen çalıştırma aralığı ve sıklığı ayarlama mantıksal uygulama her dakika.

      ![Frequency ve interval değerleriyle yineleme tetikleyicisi için ayarlayın](./media/logic-apps-batch-process-send-receive-messages/recurrence-trigger-batch-sender-details.png)

2. Bir toplu ileti göndermek için yeni bir eylem ekleyin.

   1. Yinelenme tetikleyicisini altında seçin **yeni adım**.

   2. Arama kutusuna filtreniz olarak "toplu" girin. 
   Seçin **eylemleri** listeleyin ve ardından şu eylemi seçin: **toplu işlem tetikleyicisi - toplu iletileri gönderme içeren bir Logic Apps iş akışı seçin**

      !["Toplu işlem tetikleyicisi içeren bir Logic Apps iş akışı seçin" seçin](./media/logic-apps-batch-process-send-receive-messages/send-messages-batch-action.png)

   3. Daha önce oluşturduğunuz batch alıcı mantıksal uygulamanızı seçin.

      !["Toplu alıcı" mantıksal uygulama seçin](./media/logic-apps-batch-process-send-receive-messages/batch-sender-select-batch-receiver.png)

      > [!NOTE]
      > Liste, batch tetikleyicilere sahip diğer logic apps de gösterir. 
      > 
      > Visual Studio kullanıyorsanız ve seçmek için kontrol herhangi toplu alıcılar görmüyorsanız için Azure batch alıcı dağıtıldı. Yapmadıysanız, bilgi nasıl [batch alıcı mantıksal uygulamanızı Azure'a dağıtma](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md#deploy-logic-app-to-azure). 

   4. Şu eylemi seçin: **Batch_messages - <*your-batch-alıcısı*>**

      ![Şu eylemi seçin: "Batch_messages - <-logic-uygulamanızı >"](./media/logic-apps-batch-process-send-receive-messages/batch-sender-select-batch.png)

3. Batch, gönderenin özellikleri ayarlayın:

   | Özellik | Açıklama | 
   |----------|-------------| 
   | **Toplu işlem adı** | Bu örnekte "TestBatch" olduğundan alıcı mantıksal uygulama tarafından tanımlanan toplu işlem adı <p>**Önemli**: toplu işlem adı, çalışma zamanında doğrulanacağı ve alıcı mantıksal uygulama tarafından belirtilen adla eşleşmelidir. Batch adının değiştirilmesi, batch gönderici başarısız olmasına neden olur. | 
   | **İleti içeriği** | Göndermek istediğiniz iletinin içeriği | 
   ||| 

   Bu örnekte, bu ifade geçerli tarih ve saat gönderdiğiniz toplu ileti içeriği ekler ekleyin:

   1. İçine tıklayın **ileti içerik** kutusu. 

   2. Dinamik içerik listesi göründüğünde **ifade**. 

   3. İfade girmek `utcnow()`ve ardından **Tamam**. 

      !["İçerik" iletisi, seçin "İfadesi", "utcnow()" girin ve "Tamam" ı seçin.](./media/logic-apps-batch-process-send-receive-messages/batch-sender-details.png)

4. Şimdi toplu işlemi için bir bölüm ayarlayın. "BatchReceiver" eylemini seçin **Gelişmiş Seçenekleri Göster** ve bu özellikleri ayarlayın:

   | Özellik | Açıklama | 
   |----------|-------------| 
   | **Bölüm adı** | Hedef bölme için kullanılacak bir isteğe bağlı benzersiz bir bölüm anahtarı mantıksal altkümelere, batch ve iletileri bu anahtarına göre Topla | 
   | **İleti kimliği** | Oluşturulan genel benzersiz tanıtıcısı (GUID), boş bir isteğe bağlı iletisini tanımlayıcı | 
   ||| 

   Bu örnekte, içinde **bölüm adı** kutusunda, bir ile beş arasında rastgele bir sayı oluşturan bir ifade ekleyin. Bırakın **ileti kimliği** kutusunu boş.
   
   1. İçine tıklayın **bölüm adı** dinamik içerik listesinde görünmesi kutusu. 

   2. Dinamik içerik listesinde **İfade**’yi seçin.
   
   3. İfade girmek `rand(1,6)`ve ardından **Tamam**.

      ![Bir bölüm için hedef toplu ayarlama](./media/logic-apps-batch-process-send-receive-messages/batch-sender-partition-advanced-options.png)

      Bu **rand** işlevi bir ile beş arasında bir sayı oluşturur. 
      Bu nedenle, bu toplu Bu ifadeyi dinamik olarak ayarlayan beş numaralı, bölümlemesinde.

5. Mantıksal uygulamanızı kaydedin. Gönderen mantıksal uygulamanız bu örnektekine benzer görünür:

   ![Gönderen mantıksal uygulamanızı kaydetme](./media/logic-apps-batch-process-send-receive-messages/batch-sender-finished.png)

## <a name="test-your-logic-apps"></a>Mantıksal uygulamalarınızı test edin

Toplu işlem çözümünüzü test etmek için birkaç dakikadan uzun süredir çalışıyor logic apps bırakın. Yakında gruplar aynı bölüm anahtarına sahip tüm beş e-postalar alma başlatın.

Batch gönderen mantıksal uygulamanız her dakika çalışan bir ile beş arasında rastgele bir sayı oluşturur ve iletilerinin nereye gönderildiğini hedef toplu işlemi için bölüm anahtarı olarak bu oluşturulan numarasını kullanır. Batch aynı bölüm anahtarına sahip beş öğeye sahip her zaman batch alıcı mantıksal uygulamanız başlatılır ve her ileti için posta gönderir.

> [!IMPORTANT]
> Bitirdiğinizde test BatchSender mantıksal uygulama gönderme durdurmak ve aşırı yükleme, gelen kutunuza önlemek için devre dışı bırakıldığından emin olun.

## <a name="next-steps"></a>Sonraki adımlar

* [JSON'ı kullanarak mantıksal uygulama tanımları üzerinde oluşturma](../logic-apps/logic-apps-author-definitions.md)
* [Visual Studio'da Azure Logic Apps ve işlevler ile sunucusuz uygulama oluşturma](../logic-apps/logic-apps-serverless-get-started-vs.md)
* [Logic apps için hata günlüğünü ve özel durum işleme](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
