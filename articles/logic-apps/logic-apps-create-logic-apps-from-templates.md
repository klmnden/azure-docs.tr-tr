---
title: "İş akışları şablonlardan - Azure Logic Apps oluşturmak | Microsoft Docs"
description: "Mantıksal uygulama şablonları kullanarak daha hızlı iş akışı yapılandırması"
author: kevinlam1
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 3656acfb-eefd-4e75-b5d2-73da56c424c9
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2017
ms.author: LADocs; klam
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 49b4bbfda4518b03ef6080bec1e2a493933af4f5
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="create-logic-app-workflows-from-prebuilt-templates"></a>Önceden oluşturulmuş şablonlardan uygulama iş mantığı oluşturun

İş akışları daha hızlı oluşturma başlamanıza yardımcı olmak için yaygın olarak kullanılan desenleri izleyin önceden oluşturulmuş mantıksal uygulamalar olan şablonları Logic Apps sağlar. Bu şablonları sağlanan gibi kullanın veya bunları senaryonuza uyacak şekilde düzenleyin.

Bazı şablon kategorileri şunlardır:

| Şablon türü | Açıklama | 
| ------------- | ----------- | 
| Kurumsal bulut şablonlar | Azure Blob, Dynamics CRM, Salesforce, kutusunda tümleştirmek için ve kurumsal bulut gereken diğer bağlayıcıları içerir. Örneğin, iş müşteri adayları düzenlemek veya Kurumsal dosya verilerinizi yedeklemek için bu şablonları kullanabilirsiniz. | 
| Kişisel üretkenlik şablonları | Günlük anımsatıcıları ayarlayarak kişisel üretkenlik geliştirmek için önemli kapatma iş öğelerini Yapılacaklar listesi ve otomatik otomatikleştirme uzun görevleri tek kullanıcı onayı adım aşağı kaydırın. | 
| Tüketici bulut şablonları | Twitter gibi sosyal medya Hizmetleri kayma tümleştirme ve e-posta için. Sosyal medya girişimleri pazarlama güçlendirme için kullanışlıdır. Bu şablonlar ayrıca geleneksel yinelenen görevleri zaman kaydederek verimliliğini artırır kopyalama, bulut gibi görevleri içerir. | 
| Kurumsal tümleştirme paketi şablonları | VETER yapılandırmak için (doğrulamak, ayıklama, dönüştürme, zenginleştirmek ve rota) bir X12 alma ardışık AS2 ve XML biçimine dönüştürme ve X12, EDIFACT, işleme üzerinden EDI belge ve AS2 iletileri. | 
| Protokol düzeni şablonları | İstek-yanıt gibi Protokolü düzenleri HTTP ve tümleştirmeler FTP ve SFTP arasında uygulamak için. Sağlanan bu şablonları kullanabilir veya bunlar üzerinde karmaşık Protokolü desenler için oluşturabilirsiniz. | 
||| 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). Bir mantıksal uygulama oluşturma hakkında daha fazla bilgi için bkz: [mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-logic-apps-from-templates"></a>Logic apps Şablondan Oluştur

1. Henüz yapmadıysanız, oturum [Azure portal](https://portal.azure.com "Azure portal").

2. Azure ana menüsünden **Yeni** > **Kurumsal Tümleştirme** > **Mantıksal Uygulama**'yı seçin.

   ![Azure portalı, Yeni, Kurumsal Tümleştirme, Mantıksal Uygulama](./media/logic-apps-create-logic-apps-from-templates/azure-portal-create-logic-app.png)

3. Bu görüntünün altındaki tabloda yer alan ayarlarla mantıksal uygulamanızı oluşturun:

   ![Mantıksal uygulama ayrıntılarını sağlayın](./media/logic-apps-create-logic-apps-from-templates/logic-app-settings.png)

   | Ayar | Değer | Açıklama | 
   | ------- | ----- | ----------- | 
   | **Ad** | *mantıksal-uygulamanızın-adı* | Mantıksal uygulama için benzersiz bir ad girin. | 
   | **Abonelik** | *Azure-aboneliğinizin-adı* | Kullanmak istediğiniz Azure aboneliğini seçin. | 
   | **Kaynak grubu** | *Azure-kaynak-grubunuzun-adı* | Oluşturma veya seçme bir [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) bu mantıksal uygulama ve bu uygulamayla ilişkili tüm kaynakları düzenlemek için. | 
   | **Konum** | *Azure-veri-merkezi-bölgeniz* | Batı ABD gibi mantıksal uygulamanızın dağıtılacağı veri merkezi bölgesini seçin. | 
   | **Log Analytics** | **Kapalı** (varsayılan) veya **üzerinde** | Aç [tanılama günlük](../logic-apps/logic-apps-monitor-your-logic-apps.md#turn-on-diagnostics-logging-for-your-logic-app) mantıksal uygulamanızı için [Azure günlük analizi](../log-analytics/log-analytics-overview.md). Zaten sahip olmasını gerektiren bir [Operations Management Suite](../operations-management-suite/operations-management-suite-overview.md) çalışma. | 
   |||| 

4. Hazır olduğunuzda **Panoya sabitle**'yi seçin. Bu şekilde mantıksal uygulamanız otomatik olarak Azure panonuzda görüntülenir ve dağıtımdan sonra açılır. **Oluştur**’u seçin.

   > [!NOTE]
   > Mantıksal uygulamanızı sabitlemek istemezseniz öğreticiye devam edebilmek için dağıtım sonrasında mantıksal uygulamanızı el ile bulup açmanız gerekir.

   Azure mantıksal uygulamanızı dağıttıktan sonra Logic Apps Tasarımcısı açılır ve tanıtım videosu bulunan bir sayfa görüntülenir. 
   Videonun altında sık kullanılan mantıksal uygulama desenlerine ait şablonları bulabilirsiniz. 

5. Kaydırma için giriş video ve ortak Tetikleyicileri geçmiş **şablonları**. Önceden oluşturulmuş bir şablonu seçin. Örneğin:

   ![Bir mantıksal uygulama şablonu seçin](./media/logic-apps-create-logic-apps-from-templates/choose-logic-app-template.png)

   > [!TIP]
   > Mantıksal uygulamanızı sıfırdan oluşturmak için seçtiğiniz **boş mantıksal uygulama**.

   Önceden oluşturulmuş bir şablonu seçtiğinizde, bu şablonu hakkında daha fazla bilgi görüntüleyebilirsiniz. 
   Örneğin:

   ![Önceden oluşturulmuş bir şablon seçin](./media/logic-apps-create-logic-apps-from-templates/logic-app-choose-prebuilt-template.png)

6. Seçili şablonla devam etmeyi seçerseniz **bu şablonu kullanmak**. 

7. Şablondaki bağlayıcılar bağlı olarak, bu adımları gerçekleştirin istenir:

   * Kimlik bilgilerinizi sistemleri veya şablon tarafından başvurulan Hizmetleri için oturum açın.

   * Herhangi bir hizmet veya şablon tarafından başvurulan sistemleri için bağlantıları oluşturun. Bir bağlantı oluşturmak için bağlantı için bir ad ve gerekirse, kullanmak istediğiniz kaynağı seçin. 

   * Bu bağlantıları zaten ayarladıysanız seçin **devam**.

   Örneğin:

   ![Bağlantıları oluşturma](./media/logic-apps-create-logic-apps-from-templates/logic-app-create-connection.png)

   İşiniz bittiğinde, mantıksal uygulamanızı açar ve Logic Apps Tasarımcısı'nda görünür.

   > [!TIP]
   > Şablon Görüntüleyicisi döndürülecek seçin **şablonları** tasarımcı araç. İsteğinizi onaylamak için bir uyarı iletisi görüntülenir şekilde bu eylem kaydedilmemiş tüm değişiklikleri atar.

8. Mantıksal uygulamanızı oluşturmaya devam edin.

   > [!NOTE] 
   > Birçok şablonları zaten gerekli özellikleri önceden doldurulmaz bağlayıcıları içerir. Ancak, bazı şablonlar hala mantıksal uygulama düzgün dağıtmadan önce değerleri sağlayın gerektirebilir. Eksik özellik alanları tamamlamadan dağıtmayı deneyin, hata iletisi alırsınız. 

## <a name="update-logic-apps-with-templates"></a>Güncelleştirme logic apps şablonları ile

1. İçinde [Azure portal](https://portal.azure.com "Azure portal"), bulma ve mantıksal uygulamanızı th içinde açma mantığı Uygulama Tasarımcısı.

2. Tasarımcı araç çubuğunda seçin **şablonları**. Devam etmek istediğinizi onaylamak için bir uyarı iletisi görünmesi Bu eylem kaydedilmemiş tüm değişiklikleri atar. Onaylamak için tercih **Tamam**. Örneğin:

   !["Şablon" seçin](./media/logic-apps-create-logic-apps-from-templates/logic-app-update-existing-with-template.png)

3. Kaydırma için giriş video ve ortak Tetikleyicileri geçmiş **şablonları**. Önceden oluşturulmuş bir şablonu seçin. Örneğin:

   ![Bir mantıksal uygulama şablonu seçin](./media/logic-apps-create-logic-apps-from-templates/choose-logic-app-template.png)

   Önceden oluşturulmuş bir şablonu seçtiğinizde, bu şablonu hakkında daha fazla bilgi görüntüleyebilirsiniz. 
   Örneğin:

   ![Önceden oluşturulmuş bir şablon seçin](./media/logic-apps-create-logic-apps-from-templates/logic-app-choose-prebuilt-template.png)

4. Seçili şablonla devam etmeyi seçerseniz **bu şablonu kullanmak**. 

5. Şablondaki bağlayıcılar bağlı olarak, bu adımları gerçekleştirin istenir:

   * Kimlik bilgilerinizi sistemleri veya şablon tarafından başvurulan Hizmetleri için oturum açın.

   * Herhangi bir hizmet veya şablon tarafından başvurulan sistemleri için bağlantıları oluşturun. Bir bağlantı oluşturmak için bağlantı için bir ad ve gerekirse, kullanmak istediğiniz kaynağı seçin. 

   * Bu bağlantıları zaten ayarladıysanız seçin **devam**.

   ![Bağlantıları oluşturma](./media/logic-apps-create-logic-apps-from-templates/logic-app-create-connection.png)

   Mantıksal uygulamanız artık açar ve Logic Apps Tasarımcısı'nda görünür.

8. Mantıksal uygulamanızı oluşturmaya devam edin. 

   > [!TIP]
   > Değişikliklerinizi kaydetmediyseniz, çalışmanızı atmak ve önceki mantıksal uygulamanızı döndürür. Tasarımcı araç çubuğunda seçin **atmak**.

> [!NOTE] 
> Birçok şablonları zaten gerekli özellikleri önceden doldurulmuş bağlayıcıları içerir. Ancak, bazı şablonlar hala mantıksal uygulama düzgün dağıtmadan önce değerleri sağlayın gerektirebilir. Eksik özellik alanları tamamlamadan dağıtmayı deneyin, hata iletisi alırsınız.

## <a name="deploy-logic-apps-built-from-templates"></a>Şablonlardan oluşturulan mantığı uygulamaları dağıtma

Şablona yaptığınız değişiklikleri yaptıktan sonra yaptığınız değişiklikleri kaydedebilirsiniz. Bu eylem mantıksal uygulamanızı da otomatik olarak yayımlar.

Tasarımcı araç çubuğunda **Kaydet**'i seçin.

![Kaydet ve mantıksal uygulamanızı yayımlama](./media/logic-apps-create-logic-apps-from-templates/logic-app-save.png)  

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](http://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

Logic apps örnekler, senaryoları, müşteri hikayeler ve izlenecek yollar oluşturma hakkında bilgi edinin.

> [!div class="nextstepaction"]
> [Gözden geçirme mantığı uygulama örnekler, senaryoları ve izlenecek yollar](../logic-apps/logic-apps-examples-and-scenarios.md)