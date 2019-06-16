---
title: Şablonlardan - Azure Logic Apps iş akışı oluşturma | Microsoft Docs
description: Azure Logic Apps'te mantıksal uygulama şablonları kullanarak iş akışları daha hızlı oluşturun
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: kevinlam1
ms.author: klam
ms.reviewer: estfan, LADocs
ms.topic: article
ms.assetid: 3656acfb-eefd-4e75-b5d2-73da56c424c9
ms.date: 10/15/2017
ms.openlocfilehash: 134a8f9625b45a8196ebd47f10286093f6ba0d46
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61459691"
---
# <a name="create-logic-app-workflows-from-prebuilt-templates"></a>Mantıksal uygulama iş akışları önceden oluşturulmuş şablonlardan oluşturma

İş akışları daha hızlı bir şekilde oluşturmaya başlamanıza yardımcı olmak için Logic Apps, yaygın olarak kullanılan desenler izleyen önceden oluşturulmuş mantıksal uygulamaların şablonları sağlar. Bu şablonlar sağladığı biçimde kullanmanız veya bunları kendi senaryonuza uyacak şekilde düzenleyin.

Bazı şablonu kategorileri şunlardır:

| Şablon türü | Açıklama | 
| ------------- | ----------- | 
| Kurumsal bulut şablonları | Azure Blob, Dynamics CRM, Salesforce, kutusunda tümleştirmek ve kurumsal bulut ihtiyaçları için diğer bağlayıcıları içerir. Örneğin, iş müşteri adayları düzenlemek veya Kurumsal dosya verilerinizi yedeklemek için bu şablonları kullanabilirsiniz. | 
| Kişisel üretkenlik şablonları | Günlük anımsatıcılar ayarlayarak kişisel üretkenliği artırın, önemli kapatma iş öğelerini Yapılacaklar listesi ve otomatikleştirerek verimsiz uzun görevlerden tek kullanıcı onay adım gösteriyor. | 
| Tüketici Bulutu şablonları | Twitter gibi sosyal medya hizmetlerinde Slack tümleştirme ve e-posta için. Sosyal medya girişimler pazarlama güçlendirmek için kullanışlıdır. Bu şablonlar, geleneksel olarak çoğu yinelenen görevlere zaman kaydederek üretkenliği artıran, kopyalama, bulut gibi görevleri de içerir. | 
| Kurumsal tümleştirme paketi şablonları | VETER yapılandırmak için (doğrulayın, ayıklayın, dönüştürme, verileri zenginleştirmesine, yönlendirmek) x X12 alma işlem hatlarını AS2 ve XML'e dönüştürme ve işleme X12, EDIFACT, belge EDI ve AS2 iletileri. | 
| Protokol desen şablonları | İstek-yanıt gibi protokol düzenleri HTTP ve tümleştirmeler üzerinden FTP ve SFTP uygulamak için. Sağlanan bu şablonları kullanın veya bunlar üzerinde karmaşık Protokolü modeller oluşturun. | 
||| 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). Mantıksal uygulama oluşturma hakkında daha fazla bilgi için bkz. [mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-logic-apps-from-templates"></a>Şablonlardan mantıksal uygulamalar oluşturma

1. Henüz yapmadıysanız, oturum [Azure portalında](https://portal.azure.com "Azure portalında").

2. Azure ana menüsünden **Kaynak oluştur** > **Kurumsal Tümleştirme** > **Mantıksal Uygulama**'yı seçin.

   ![Azure portalı, Yeni, Kurumsal Tümleştirme, Mantıksal Uygulama](./media/logic-apps-create-logic-apps-from-templates/azure-portal-create-logic-app.png)

3. Bu görüntünün altındaki tabloda yer alan ayarlarla mantıksal uygulamanızı oluşturun:

   ![Mantıksal uygulama ayrıntılarını sağlayın](./media/logic-apps-create-logic-apps-from-templates/logic-app-settings.png)

   | Ayar | Değer | Açıklama | 
   | ------- | ----- | ----------- | 
   | **Ad** | *mantıksal-uygulamanızın-adı* | Mantıksal uygulama için benzersiz bir ad girin. | 
   | **Abonelik** | *Azure-aboneliğinizin-adı* | Kullanmak istediğiniz Azure aboneliğini seçin. | 
   | **Kaynak grubu** | *Azure-kaynak-grubunuzun-adı* | Oluşturma veya seçme bir [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) bu mantıksal uygulamanın ve bu uygulamayla ilişkili tüm kaynakları düzenlemek için. | 
   | **Konum** | *Azure-veri-merkezi-bölgeniz* | Batı ABD gibi mantıksal uygulamanızın dağıtılacağı veri merkezi bölgesini seçin. | 
   | **Log Analytics** | **Kapalı** (varsayılan) veya **üzerinde** | Açma [tanılama günlüğüne kaydetme](../logic-apps/logic-apps-monitor-your-logic-apps.md#turn-on-diagnostics-logging-for-your-logic-app) mantıksal uygulamanızın [Azure İzleyicisi](../log-analytics/log-analytics-overview.md). Zaten bir Log Analytics çalışma alanı olmasını gerektirir. | 
   |||| 

4. Hazır olduğunuzda **Panoya sabitle**'yi seçin. Bu şekilde mantıksal uygulamanız otomatik olarak Azure panonuzda görüntülenir ve dağıtımdan sonra açılır. **Oluştur**’u seçin.

   > [!NOTE]
   > Mantıksal uygulamanızı sabitlemek istemezseniz öğreticiye devam edebilmek için dağıtım sonrasında mantıksal uygulamanızı el ile bulup açmanız gerekir.

   Azure mantıksal uygulamanızı dağıttıktan sonra Logic Apps Tasarımcısı açılır ve tanıtım videosu bulunan bir sayfa görüntülenir. 
   Videonun altında sık kullanılan mantıksal uygulama desenlerine ait şablonları bulabilirsiniz. 

5. Kaydırma için giriş videosunu ve sık kullanılan Tetikleyicileri geçmiş **şablonları**. Önceden oluşturulmuş bir şablonu seçin. Örneğin:

   ![Bir mantıksal uygulama şablonunu seçin](./media/logic-apps-create-logic-apps-from-templates/choose-logic-app-template.png)

   > [!TIP]
   > Mantıksal uygulamanızı sıfırdan oluşturmak için seçin **boş mantıksal uygulama**.

   Önceden oluşturulmuş bir şablonu seçtiğinizde şablon hakkında daha fazla bilgi görüntüleyebilirsiniz. 
   Örneğin:

   ![Önceden oluşturulmuş bir şablon seçin](./media/logic-apps-create-logic-apps-from-templates/logic-app-choose-prebuilt-template.png)

6. Seçilen şablonla devam etmeyi tercih **bu şablonu kullan**. 

7. Şablondaki bağlayıcıları bağlı olarak, aşağıdaki adımlardan birini yapmanız istenir:

   * Sistemleri veya şablon tarafından başvurulan services kimlik bilgilerinizle oturum açın.

   * Herhangi bir hizmet veya şablon tarafından başvurulan sistemleri için bağlantılar oluşturun. Bir bağlantı oluşturmak için bağlantınız için bir ad sağlayın ve gerekirse, kullanmak istediğiniz kaynak seçin. 

   * Bu bağlantıları ayarlarsanız, seçin **devam**.

   Örneğin:

   ![Bağlantılar oluşturun](./media/logic-apps-create-logic-apps-from-templates/logic-app-create-connection.png)

   İşiniz bittiğinde mantıksal uygulamanız açılır ve Logic Apps Tasarımcısı'nda görünür.

   > [!TIP]
   > Şablon Görüntüleyicisi'ne dönmek için seçin **şablonları** tasarımcı araç çubuğunda. İsteğinizi onaylamak için bir uyarı iletisi görüntülenir. Bu nedenle bu eylemi kaydedilmemiş değişiklikleri atar.

8. Mantıksal uygulamanızı oluşturmaya devam edin.

   > [!NOTE] 
   > Birçok şablonları gerekli özellikler doldurduk bağlayıcıları içerir. Ancak, bazı şablonlar hala doğru mantıksal uygulamayı dağıtmadan önce değerleri sağlayan gerektirebilir. Eksik özellik alanları tamamlamadan dağıtmayı denerseniz, bir hata iletisi alırsınız. 

## <a name="update-logic-apps-with-templates"></a>Logic apps ile şablonları güncelleştirme

1. İçinde [Azure portalında](https://portal.azure.com "Azure portalında"), bulmak ve mantıksal uygulamanızı açın th mantıksal Uygulama Tasarımcısı.

2. Tasarımcı araç çubuğunda **şablonları**. Devam etmek istediğinizi onaylamak için bir uyarı iletisi görüntülenir. Bu nedenle bu eylemi kaydedilmemiş değişiklikleri atar. Doğrulamak için şunu seçin **Tamam**. Örneğin:

   !["Şablon" seçin](./media/logic-apps-create-logic-apps-from-templates/logic-app-update-existing-with-template.png)

3. Kaydırma için giriş videosunu ve sık kullanılan Tetikleyicileri geçmiş **şablonları**. Önceden oluşturulmuş bir şablonu seçin. Örneğin:

   ![Bir mantıksal uygulama şablonunu seçin](./media/logic-apps-create-logic-apps-from-templates/choose-logic-app-template.png)

   Önceden oluşturulmuş bir şablonu seçtiğinizde şablon hakkında daha fazla bilgi görüntüleyebilirsiniz. 
   Örneğin:

   ![Önceden oluşturulmuş bir şablon seçin](./media/logic-apps-create-logic-apps-from-templates/logic-app-choose-prebuilt-template.png)

4. Seçilen şablonla devam etmeyi tercih **bu şablonu kullan**. 

5. Şablondaki bağlayıcıları bağlı olarak, aşağıdaki adımlardan birini yapmanız istenir:

   * Sistemleri veya şablon tarafından başvurulan services kimlik bilgilerinizle oturum açın.

   * Herhangi bir hizmet veya şablon tarafından başvurulan sistemleri için bağlantılar oluşturun. Bir bağlantı oluşturmak için bağlantınız için bir ad sağlayın ve gerekirse, kullanmak istediğiniz kaynak seçin. 

   * Bu bağlantıları ayarlarsanız, seçin **devam**.

   ![Bağlantılar oluşturun](./media/logic-apps-create-logic-apps-from-templates/logic-app-create-connection.png)

   Mantıksal uygulamanız artık açar ve Logic Apps Tasarımcısı'nda görünür.

8. Mantıksal uygulamanızı oluşturmaya devam edin. 

   > [!TIP]
   > Yaptığınız değişiklikleri kaydetmediniz iş atmak ve önceki mantıksal uygulamanıza dönün. Tasarımcı araç çubuğunda **at**.

> [!NOTE] 
> Birçok şablonları zaten gerekli özellikleri önceden doldurulmuş bağlayıcıları içerir. Ancak, bazı şablonlar hala doğru mantıksal uygulamayı dağıtmadan önce değerleri sağlayan gerektirebilir. Eksik özellik alanları tamamlamadan dağıtmayı denerseniz, bir hata iletisi alırsınız.

## <a name="deploy-logic-apps-built-from-templates"></a>Yerleşik şablonlardan mantıksal uygulama dağıtma

Şablona Değişikliklerinizi yaptıktan sonra değişikliklerinizi kaydedebilirsiniz. Bu eylem, mantıksal uygulamanızın da otomatik olarak yayımlar.

Tasarımcı araç çubuğunda **Kaydet**'i seçin.

![Kaydet ve mantıksal uygulamanızı yayımlama](./media/logic-apps-create-logic-apps-from-templates/logic-app-save.png)  

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

Örnekler, senaryolar, müşteri hikayeleri ve izlenecek yollar aracılığıyla mantıksal uygulamalar oluşturma hakkında bilgi edinin.

> [!div class="nextstepaction"]
> [Mantıksal uygulama örnekler, senaryolar ve Kılavuzlar gözden geçirin](../logic-apps/logic-apps-examples-and-scenarios.md)