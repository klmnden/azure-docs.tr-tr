---
title: Bir grubun veya koleksiyonun - Azure Logic Apps işlem EDI iletilerini toplu | Microsoft Docs
description: EDI iletilerini toplu logic apps'te işleme için Gönder
services: logic-apps
ms.service: logic-apps
author: divyaswarnkar
ms.author: divswa
ms.reviewer: estfan, LADocs
ms.topic: article
ms.date: 08/19/2018
ms.openlocfilehash: d6d3a7111f3a5e49e32eba8ca4f09d692538cb87
ms.sourcegitcommit: 1289f956f897786090166982a8b66f708c9deea1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "64715795"
---
# <a name="send-edi-messages-in-batches-to-trading-partners-with-azure-logic-apps"></a>Azure Logic Apps ile ortaklar için EDI iletilerini toplu olarak gönder

İşletmeler arası (B2B) senaryolarında iş ortakları genellikle gruplardaki mesaj alışverişi veya *toplu*. Logic Apps ile toplu bir çözüm derlediğinizde, ticari iş ortakları için iletiler gönderin ve bu iletileri birlikte toplu işler. Bu makalede nasıl X12 örnek olarak, mantıksal uygulama "toplu sender" ve "toplu alıcı" mantıksal uygulama oluşturarak kullanarak EDI iletilerini işleme, işlem yapabileceğiniz açıklanır. 

İletileri works X12 toplu işleme gibi diğer iletiler; toplu işleme bir toplu işlem ve toplu iletiler gönderen bir toplu işlem iletileri toplayan bir toplu iş tetikleyicisi kullanırsınız. Ayrıca, X12 ticaret iş ortağı veya diğer hedef iletileri geçmeden önce adım kodlama x X12 toplu iş oluşturmayı içerir. Toplu tetikleyici ve eylem hakkında daha fazla bilgi için bkz: [toplu işlem iletileri](../logic-apps/logic-apps-batch-process-send-receive-messages.md).

Bu makalede, toplu işleme çözümünü aynı Azure aboneliği, Azure bölgesi ve aşağıdaki iki logic apps'te oluşturarak oluşturacaksınız bu belirli bir sıraya göre:

* A ["toplu alıcı"](#receiver) kabul eder ve belirtilen ölçütlerinize karşılanıyorsa kadar serbest ve bu iletileri işlemek için bir toplu iş içine iletileri toplayan mantıksal uygulama. Bu senaryoda, batch alıcı da toplu işlem iletileri belirtilen X12 kullanarak kodlar sözleşmesi ya da iş ortağı kimlikleri.

  Batch gönderen oluşturduğunuzda, daha sonra batch hedef seçebilmeniz için batch alıcı ilk oluşturduğunuzdan emin olun.

* A ["toplu sender"](#sender) mantıksal uygulama, daha önce oluşturulan batch alıcısına iletileri gönderir. 

Batch alıcı ve gönderen batch aynı Azure aboneliğinde paylaştığınızdan emin olun *ve* Azure bölgesi. Yoksa, bunların birbirlerine görünmeyen olmadığınızdan batch gönderen oluşturduğunuzda, batch alıcı seçemezsiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu örneği takip etmek için bu öğeler gerekir:

* Azure aboneliği. Bir aboneliğiniz yoksa [ücretsiz bir Azure hesabı ile başlayabilirsiniz](https://azure.microsoft.com/free/). Veya, [için Kullandıkça Öde aboneliğine kaydolun](https://azure.microsoft.com/pricing/purchase-options/).

* Hakkında temel bilgilere [mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Mevcut bir [tümleştirme hesabı](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) Azure aboneliğinizle ilişkili ve logic apps için bağlandı

* En az iki varolan [iş ortakları](../logic-apps/logic-apps-enterprise-integration-partners.md) tümleştirme hesabı. Her iş ortağı X12 (standart taşıyıcı alfa kodu) kullanmanız gerekir bir iş kimliği iş ortağının özellikleri olarak niteleyicisi.

* Mevcut bir [X12 sözleşmesi](../logic-apps/logic-apps-enterprise-integration-x12.md) tümleştirme hesabı

* Azure portalı yerine Visual Studio kullanmak üzere emin olursunuz [Logic Apps ile çalışmak için Visual Studio'yu ayarlama](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md).

<a name="receiver"></a>

## <a name="create-x12-batch-receiver"></a>X12 oluşturma batch alıcısı

Bir toplu iletileri göndermeden önce toplu iletiler göndermek burada hedef olarak önce mevcut olması gerekir. Bu nedenle ilk olarak, ile başlar "toplu alıcı" mantıksal uygulama oluşturma **Batch** tetikleyici. "Toplu sender" mantıksal uygulama oluşturduğunuzda, bu şekilde, batch alıcı mantıksal uygulama seçebilirsiniz. Batch alıcı, ileti belirtilen ölçütlerinize karşılanıyorsa kadar serbest bırakarak ve bu iletileri için toplama devam eder. Batch alıcılar batch Gönderenler ilgili hiçbir şeyi bilmeniz gerekmez ancak toplu Gönderenler iletileri burada gönderdikleri hedef bilmeniz gerekir. 

Bu batch alıcı için belirttiğiniz toplu iş modu, ad yayın ölçütü X12 sözleşmesi ve diğer ayarları. 

1. İçinde [Azure portalında](https://portal.azure.com) veya Visual Studio, bu ada sahip bir mantıksal uygulama oluşturun: "BatchX12Messages"

2. [Mantıksal uygulamanızı tümleştirme hesabınıza bağlayın](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md#link-account).

3. Logic Apps Tasarımcısı'nda ekleme **Batch** mantıksal uygulama iş akışınızı başlatan bir tetikleyici. Arama kutusuna filtreniz olarak "toplu" girin. Şu tetikleyiciyi seçin: **Toplu işlem iletileri**

   ![Toplu tetikleyici ekleme](./media/logic-apps-scenario-EDI-send-batch-messages/add-batch-receiver-trigger.png)

4. Batch, alıcı özellikleri ayarlayın: 

   | Özellik | Değer | Notlar | 
   |----------|-------|-------|
   | **Toplu iş modu** | Satır içi |  |  
   | **Toplu işlem adı** | TestBatch | Yalnızca **satır içi** toplu iş modu | 
   | **Yayın ölçütü** | Zamanlama tabanlı, ileti sayısı tabanlı | Yalnızca **satır içi** toplu iş modu | 
   | **İleti sayısı** | 10 | Yalnızca **ileti sayısı tabanlı** yayın ölçütleri | 
   | **Aralık** | 10 | Yalnızca **zamanlama tabanlı** yayın ölçütleri | 
   | **Sıklık** | Dakika | Yalnızca **zamanlama tabanlı** yayın ölçütleri | 
   ||| 

   ![Toplu tetikleyici ayrıntılarını sağlayın](./media/logic-apps-scenario-EDI-send-batch-messages/batch-receiver-release-criteria.png)

   > [!NOTE]
   > Aynı bölüm anahtarını her toplu işin kullanması için bu örnek toplu işlem için bir bölüm ayarlama değil. Bölümleri hakkında daha fazla bilgi için bkz. [toplu işlem iletileri](../logic-apps/logic-apps-batch-process-send-receive-messages.md#batch-sender).

5. Şimdi her toplu işin kodlayan bir eylem ekleyin: 

   1. Toplu tetikleyici altında seçin **yeni adım**.

   2. Arama kutusuna "X 12 batch" filtreniz olarak ve (tüm sürümler) şu eylemi seçin: **Toplu kodlama <*sürüm*>-X12** 

      ![X12 seçin eylem toplu kodlama](./media/logic-apps-scenario-EDI-send-batch-messages/add-batch-encode-action.png)

   3. Daha önce tümleştirme hesabınıza bağlanın alamadık, artık bağlantı oluşturun. Bağlantınız için bir ad girin ve ardından tümleştirme hesabı seçin **Oluştur**.

      ![Batch Kodlayıcısı ve tümleştirme hesabı arasında bağlantı oluşturma](./media/logic-apps-scenario-EDI-send-batch-messages/batch-encoder-connect-integration-account.png)

   4. Batch Kodlayıcı eylemleriniz için bu özellikleri ayarlayın:

      | Özellik | Açıklama |
      |----------|-------------|
      | **X12 adını Sözleşmesi** | Listeyi açın ve mevcut sözleşmenize seçin. <p>Listeniz boşsa, emin olun [mantıksal uygulama tümleştirme hesabı'na bağlantı](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md#link-account) istediğiniz sözleşmesi içeriyor. | 
      | **BatchName** | Bu kutusunun içine tıklayın ve dinamik içerik listesinde göründükten sonra seçin **Tpl** belirteci. | 
      | **PartitionName** | Bu kutusunun içine tıklayın ve dinamik içerik listesinde göründükten sonra seçin **bölüm adı** belirteci. | 
      | **Öğeleri** | Öğe ayrıntıları kutusunu kapatın ve ardından bu kutusunun içine tıklayın. Dinamik içerik listesinde göründükten sonra seçin **toplu öğeleri** belirteci. | 
      ||| 

      ![Batch kodla eylem ayrıntıları](./media/logic-apps-scenario-EDI-send-batch-messages/batch-encode-action-details.png)

      İçin **öğeleri** kutusunda:

      ![Batch kodla eylem öğeleri](./media/logic-apps-scenario-EDI-send-batch-messages/batch-encode-action-items.png)

6. Mantıksal uygulamanızı kaydedin. 

7. Visual Studio kullanıyorsanız, emin [batch alıcı mantıksal uygulamanızı Azure'a dağıtma](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md#deploy-logic-app-to-azure). Aksi halde, batch gönderen oluşturduğunuzda, batch alıcı seçemezsiniz.

### <a name="test-your-logic-app"></a>Mantıksal uygulamanızı test edin

Beklendiği gibi batch alıcı works emin olmak için test amacıyla bir HTTP eylemi ekleyebilir ve toplu ileti gönder [istek depo hizmeti](https://requestbin.fullcontact.com/). 

1. Eylem altında X12 kodlama öğesini **yeni adım**. 

2. Arama kutusuna filtreniz olarak "http" girin. Şu eylemi seçin: **HTTP - HTTP**
    
   ![HTTP eylemi seçin](./media/logic-apps-scenario-EDI-send-batch-messages/batch-receiver-add-http-action.png)

3. HTTP eylemi için özellikleri ayarlayın:

   | Özellik | Açıklama | 
   |----------|-------------|
   | **Yöntemi** | Bu listeden **POST**. | 
   | **URI** | Bir URI, istek depo oluşturun ve ardından bu URI, bu kutuya girin. | 
   | **Gövde** | Bu kutusunun içine tıklayın ve dinamik içerik listesinden açıldıktan sonra seçin **gövdesi** bölümünde görünen belirteci **sözleşme adına göre toplu kodlama**. <p>Görmüyorsanız **gövdesi** yanındaki simge **sözleşme adına göre toplu kodlama**seçin **daha fazla bilgi bkz**. | 
   ||| 

   ![HTTP eylem ayrıntılarını sağlayın](./media/logic-apps-scenario-EDI-send-batch-messages/batch-receiver-add-http-action-details.png)

4. Mantıksal uygulamanızı kaydedin. 

   Batch alıcı mantıksal uygulamanız şu örnekteki gibi görünür: 

   ![Batch alıcı mantıksal uygulamanızı kaydetme](./media/logic-apps-scenario-EDI-send-batch-messages/batch-receiver-finished.png)

<a name="sender"></a>

## <a name="create-x12-batch-sender"></a>X12 oluşturma batch gönderen

Artık ileti göndermek için batch alıcı mantıksal uygulama bir veya daha fazla mantıksal uygulamalar oluşturun. Her batch gönderici batch alıcı mantıksal uygulama ve toplu iş adı, ileti içeriği ve diğer ayarları belirtin. İsteğe bağlı olarak, bu anahtarla iletileri toplamak için alt kümelerini toplu bölmek için benzersiz bir bölüm anahtarı sağlayabilir. 

* Seçtiğiniz emin zaten [batch Alıcınız oluşturulan](#receiver) , batch gönderen oluşturduğunuzda, mevcut batch alıcı hedef toplu seçebilmeniz için. Batch alıcılar batch Gönderenler ilgili hiçbir şeyi bilmeniz gerekmez ancak toplu Gönderenler iletilerin gönderileceği bilmeniz gerekir. 

* Batch alıcı ve gönderen batch aynı Azure bölgesindeki paylaştığınızdan emin olun *ve* Azure aboneliği. Yoksa, bunların birbirlerine görünmeyen olmadığınızdan batch gönderen oluşturduğunuzda, batch alıcı seçemezsiniz.

1. Bu ada sahip başka bir mantıksal uygulama oluşturun: "SendX12MessagesToBatch" 

2. Arama kutusuna filtreniz olarak "http isteği," girin. Şu tetikleyiciyi seçin: **Bir HTTP isteği alındığında** 
   
   ![İstek tetikleyicisi Ekle](./media/logic-apps-scenario-EDI-send-batch-messages/add-request-trigger-sender.png)

3. Bir toplu iletiler gönderen bir eylem ekleme.

   1. HTTP isteği eylem altında seçin **yeni adım**.

   2. Arama kutusuna filtreniz olarak "toplu" girin. 
   Seçin **eylemleri** listeleyin ve ardından şu eylemi seçin: **Toplu işlem tetikleyicisi - toplu iletileri gönderme içeren bir Logic Apps iş akışı seçin**

      !["Toplu işlem tetikleyicisi içeren bir Logic Apps iş akışı seçin" seçin](./media/logic-apps-scenario-EDI-send-batch-messages/batch-sender-select-batch-trigger.png)

   3. Şimdi daha önce oluşturduğunuz "BatchX12Messages" mantıksal uygulamanızı seçin.

      !["Toplu alıcı" mantıksal uygulama seçin](./media/logic-apps-scenario-EDI-send-batch-messages/batch-sender-select-batch-receiver.png)

   4. Şu eylemi seçin: **Batch_messages - <*your-batch-alıcısı*>**

      !["Batch_messages" eylemini seçin](./media/logic-apps-scenario-EDI-send-batch-messages/batch-sender-select-batch-messages-action.png)

4. Batch, gönderenin özelliklerini ayarlayın.

   | Özellik | Açıklama | 
   |----------|-------------| 
   | **Toplu işlem adı** | Bu örnekte "TestBatch" olduğundan alıcı mantıksal uygulama tarafından tanımlanan toplu işlem adı <p>**Önemli**: Toplu işlem adı, çalışma zamanında doğrulanacağı ve alıcı mantıksal uygulama tarafından belirtilen adla eşleşmelidir. Batch adının değiştirilmesi, batch gönderici başarısız olmasına neden olur. | 
   | **İleti içeriği** | İçeriği olan göndermek istediğiniz iletinin **gövdesi** Bu örnekte belirteci | 
   ||| 
   
   ![Batch özelliklerini ayarlama](./media/logic-apps-scenario-EDI-send-batch-messages/batch-sender-set-batch-properties.png)

5. Mantıksal uygulamanızı kaydedin. 

   Batch gönderen mantıksal uygulamanız şu örnekteki gibi görünür:

   ![Batch gönderen mantıksal uygulamanızı kaydetme](./media/logic-apps-scenario-EDI-send-batch-messages/batch-sender-finished.png)

## <a name="test-your-logic-apps"></a>Mantıksal uygulamalarınızı test edin

Toplu işlem çözümünüzü test etmek için post X12 batch gönderen mantıksal uygulamanızdan iletileri [Postman](https://www.getpostman.com/postman) veya benzer bir araç. Yakında X12 alma başlattığınız her 10 dakikada, istek bin veya toplu işler halinde 10, aynı bölüm anahtarına sahip tüm iletileri.

## <a name="next-steps"></a>Sonraki adımlar

* [Toplu olarak işlem iletileri](../logic-apps/logic-apps-batch-process-send-receive-messages.md) 
