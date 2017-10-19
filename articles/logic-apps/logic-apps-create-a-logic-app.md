---
title: "Sistemler ile bulut hizmetleri arasında ilk otomatik iş akışınızı oluşturma - Azure Logic Apps | Microsoft Docs"
description: "Mantıksal uygulamalar oluşturup çalıştırarak, sistem tümleştirme ve kuruluş uygulaması tümleştirmeye (EAI) senaryoları için iş süreçlerini ve iş akışlarını otomatik hale getirin"
author: ecfan
manager: anneta
editor: 
services: logic-apps
keywords: "iş akışı, bulut uygulamaları, bulut hizmetleri, iş süreçleri, sistem tümleştirme, kuruluş uygulaması tümleştirme, EAI"
documentationcenter: 
ms.assetid: ce3582b5-9c58-4637-9379-75ff99878dcd
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/18/2017
ms.author: LADocs; estfan
ms.openlocfilehash: d62255ba6e3d5bdfbd792a47f3a92d4c88876742
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-your-first-logic-app-for-automating-workflows-and-processes-through-the-azure-portal"></a>İş akışlarını ve işlemleri Azure portalı aracılığıyla otomatikleştirmek için ilk mantıksal uygulamanızı oluşturun

Kod yazmanıza gerek kalmadan, [Azure Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md) ile otomatik iş akışları derleyip çalıştırarak sistemleri ve hizmetleri tümleştirebilirsiniz. Bu öğreticide görevleri bir iş akışıyla otomatikleştirmenin ne kadar kolay olduğunu göstermek için bir web sitesindeki yeni içerik RSS akışını kontrol eden ve akıştaki her yeni öğe için bir e-posta gönderen basit bir mantıksal uygulama oluşturulmaktadır. 

![Genel bakış - ilk mantıksal uygulama örneği](./media/logic-apps-create-a-logic-app/logic-app-overview.png)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Boş bir mantıksal uygulama oluşturma.
> * RSS akış öğesi yayımlandığında mantıksal uygulamanızın başlatılması için bir tetikleyici ekleme.
> * RSS akış öğesi hakkındaki ayrıntılı bilgileri içeren bir e-posta göndermek için bir eylem ekleme.
> * Mantıksal uygulamanızı çalıştırma ve test etme.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Bir aboneliğiniz yoksa [ücretsiz bir Azure hesabı ile başlayabilirsiniz](https://azure.microsoft.com/free/). Ya da [Kullandıkça Öde aboneliğine kaydolabilirsiniz](https://azure.microsoft.com/pricing/purchase-options/).

* Bildirim göndermek için [Azure Logic Apps tarafından desteklenen bir e-posta sağlayıcısından](../connectors/apis-list.md) alınmış bir e-posta hesabı. Örneğin Office 365 Outlook, Outlook.com, Gmail veya başka bir desteklenen sağlayıcı kullanabilirsiniz. Bu öğreticide Office 365 Outlook kullanılmaktadır.

  > [!TIP]
  > Kişisel bir [Microsoft hesabınız](https://account.microsoft.com/account) varsa Outlook.com hesabınız vardır. Aksi takdirde, bir Azure iş veya okul hesabınız varsa Office 365 Outlook hesabınız vardır.

* Web sitesinin RSS akışının bağlantısı. Bu örnekte [CNN.com web sitesindeki en önemli haberler için RSS akışı](http://rss.cnn.com/rss/cnn_topstories.rss) kullanılmıştır: `http://rss.cnn.com/rss/cnn_topstories.rss`

## <a name="1-create-a-blank-logic-app"></a>1. Boş bir mantıksal uygulama oluşturma 

1. [Azure portalı](https://portal.azure.com "Azure portalı") oturumunu açın.

2. Azure ana menüsünden **Yeni** > **Kurumsal Tümleştirme** > **Mantıksal Uygulama**'yı seçin.

   ![Azure portalı, Yeni, Kurumsal Tümleştirme, Mantıksal Uygulama](media/logic-apps-create-a-logic-app/azure-portal-create-logic-app.png)

3. Tabloda yer alan özelliklerle mantıksal uygulamanızı oluşturun.

   ![Mantıksal uygulama ayrıntılarını sağlayın](./media/logic-apps-create-a-logic-app/logic-app-settings.png)

   | Ayar | Önerilen değer | Açıklama | 
   | ------- | --------------- | ----------- | 
   | **Ad** | *mantıksal-uygulamanızın-adı* | Mantıksal uygulama için benzersiz bir ad girin. | 
   | **Abonelik** | *Azure-aboneliğiniz* | Kullanmak istediğiniz Azure aboneliğini seçin. | 
   | **Kaynak grubu** | *Azure-kaynak-grubunuz* | İlgili Azure kaynaklarını düzenleyip yönetmenize yardımcı olan bir Azure kaynak grubu oluşturun veya seçin. | 
   | **Konum** | *Azure-bölgeniz* | Mantıksal uygulamanızın dağıtılacağı veri merkezi bölgesini seçin. | 
   |||| 

4. Hazır olduğunuzda **Panoya sabitle**'yi ve ardından **Oluştur**'u seçin.

   Mantıksal uygulamanız için bir Azure kaynağı oluşturdunuz. 
   Azure mantıksal uygulamanızı dağıttıktan sonra Logic Apps Tasarımcısı'nda hızlı bir başlangıç yapmanıza yardımcı olacak ortak desen şablonları gösterilir.

   > [!NOTE] 
   > **Panoya sabitle**'yi seçtiğinizde, mantıksal uygulamanız dağıtıldıktan sonra Azure panosunda gösterilir ve otomatik olarak Logic Apps Tasarımcısı'nda açılır. Açılmazsa mantıksal uygulamanızı kendiniz bulup açabilirsiniz.

5. Mantıksal uygulamanızı sıfırdan oluşturabilmek için şimdilik **Şablonlar**'ın altından **Boş Mantıksal Uygulama**'yı seçin.

   ![Mantıksal uygulama şablonunu seçin](./media/logic-apps-create-a-logic-app/choose-logic-app-template.png)

   Logic Apps Tasarımcısı'nda mantıksal uygulama iş akışınızı başlatmak için kullanacağınız uygun [*bağlayıcılar*](../connectors/apis-list.md) ve [*tetikleyicileri*](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) gösterilir.

   ![Mantıksal uygulama tetikleyicileri](./media/logic-apps-create-a-logic-app/logic-app-triggers.png)

## <a name="2-add-a-trigger-for-starting-the-workflow"></a>2. İş akışını başlatmak için bir tetikleyici ekleme

Tüm mantıksal uygulamaların bir [*tetikleyici*](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) ile başlatılması gerekir. Tetikleyici, belirli bir olay meydana geldiğinde veya yeni veriler belirlediğiniz bir koşulu karşıladığında etkinleştirilir. Bu işlemin ardından Logic Apps altyapısı, iş akışınızı çalıştırmak için bir mantıksal uygulama örneği oluşturur. Tetikleyici her etkinleştirildiğinde altyapı, mantıksal uygulama iş akışınızı çalıştıran ayrı bir örnek oluşturur.

1. Arama kutusuna "rss" yazın. Şu tetikleyiciyi seçin: **RSS - Akış öğesi yayımlandığında** 

   ![Tetikleyiciyi seçin: "RSS - Akış öğesi yayımlandığında"](./media/logic-apps-create-a-logic-app/rss-trigger.png)

2. İzlemek istediğiniz web sitesinin RSS akışına ait bağlantıyı girin. Örneğin: `http://rss.cnn.com/rss/cnn_topstories.rss`. Yinelenme aralığını ve sıklığını ayarlayın. Bu örnekte bu özellikleri akışı her gün denetleyecek şekilde düzenleyin. 

   ![Tetikleyicinin RSS akışı, sıklık ve aralık ayarını yapma](./media/logic-apps-create-a-logic-app/rss-trigger-setup.png)

3. Çalışmanızı şimdilik kaydedin. Tasarımcı araç çubuğunda **Kaydet**'i seçin.
Tetikleyicinin ayrıntılarını daraltmak ve gizlemek için tetikleyici başlık çubuğunu seçin.

   ![Mantıksal uygulamanızı kaydetme](./media/logic-apps-create-a-logic-app/save-logic-app.png)

   Mantıksal uygulamanız artık yayında ancak iş akışına eylem ekleyene kadar RSS akışındaki yeni öğeleri denetleme dışında bir işlem gerçekleştirmeyecek. 

## <a name="3-add-an-action-that-responds-to-the-trigger"></a>3. Tetikleyiciye yanıt veren bir eylem ekleme

Şimdi mantıksal uygulama iş akışınızın gerçekleştirdiği görev olan bir [*eylem*](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) ekleyin. Bu örnekte RSS akışında yeni bir öğe göründüğünde e-posta gönderen bir eylem ekleyeceksiniz.

1. Logic Apps Tasarımcısı'nda tetikleyicinin altında **+ Yeni adım** > **Eylem ekle**'yi seçin.

   ![Eylem ekleme](./media/logic-apps-create-a-logic-app/add-new-action.png)

   Tasarımcı, tetikleyiciniz başlatıldığında gerçekleştirilecek bir eylem seçebilmeniz için [kullanılabilir bağlayıcıları](../connectors/apis-list.md) gösterir.

   ![Eylem listesinden seçim yapma](./media/logic-apps-create-a-logic-app/logic-app-actions.png)

2. Arama kutusuna "e-posta gönder" yazın. E-posta sağlayıcınıza uygun bağlayıcıyı bulun ve seçin. Ardından bağlayıcı için "e-posta gönder" eylemini seçin. Örneğin: 

   * Azure iş veya okul hesabı için Office 365 Outlook bağlayıcısını seçin. 
   * Kişisel Microsoft hesapları için Outlook.com bağlayıcısını seçin. 
   * Gmail hesapları için Gmail bağlayıcısını seçin. 

   İşleme Office 365 Outlook bağlayıcısıyla devam edeceğiz. 
   Farklı bir sağlayıcı kullandığınızda adımlar aynı olacaktır ancak kullanıcı arabirimi farklı olabilir. 

   ![Şu eylemi seçin: "Office 365 Outlook - E-posta gönder"](./media/logic-apps-create-a-logic-app/actions.png)

3. Kimlik bilgileriniz istendiğinde, e-posta hesabınıza ait kullanıcı adı ve parolayla oturum açın. 

4. Tabloda yer alan bilgileri kullanın ve e-postaya eklenmesini istediğiniz alanları seçin.

   | Alıcı | Adımlar | 
   | -- | ----- | 
   | İş akışınız için kullanılabilir alanları seçin. | Düzenleme kutusunun içine tıklayarak **Dinamik içerik** listesini açın veya **Dinamik içerik ekle**'yi seçin. | 
   | Diğer kullanılabilir alanları görüntüleyin. | **Dinamik içerik** listesinde her bölüm için **Daha fazla**'ya tıklayın.  | 
   | Bir düzenleme kutusuna boş satırlar ekleyin. | Shift + Enter tuşlarına basın. | 
   | **Dinamik içerik** listesini kapatın. | **Dinamik içerik ekle**'yi tekrar seçin. | 
   ||| 

   ![E-postaya eklenecek verileri seçme](./media/logic-apps-create-a-logic-app/rss-action-setup.png)

   | Ayar | Önerilen değer | Açıklama | 
   | ------- | --------------- | ----------- | 
   | **Alıcı** | *alıcının-e_posta-adresi* | Alıcının e-posta adresi girin. Test için kendi e-posta adresinizi kullanabilirsiniz. | 
   | **Konu** | Yeni CNN gönderisi: **Akış başlığı** | E-posta konusunun içeriğini girin. <p>Bu öğreticide önerilen metni girin ve tetikleyicinin akış öğesinin başlığını gösteren **Akış başlığı** alanını seçin. | 
   | **Gövde** | Başlık: **Akış başlığı** <p>Yayımlanma tarihi: **Birincil akış bağlantısı** <p>Bağlantı: **Birincil akış bağlantısı** | E-posta gövdesinin içeriğini girin. <p>Bu öğreticide önerilen metni girin ve şu tetikleyici alanlarını seçin: <p>- **Akış başlığı**: Akış öğesinin başlığını tekrar gösterir </br>- **Akış yayımlanma zamanı**: Öğenin yayımlanma tarihini ve saatini gösterir </br>- **Birincil akış bağlantısı**: Akış öğesinin URL'sini gösterir | 
   |||| 

   > [!NOTE] 
   > Dizi içeren bir alan seçerseniz tasarımcı eyleme otomatik olarak dizeye başvuran bir "For each" döngüsü ekler. Bu şekilde mantıksal uygulamanız ilgili eylemi dizideki tüm öğeler için gerçekleştirir.

5. İşiniz bittiğinde yaptığınız değişiklikleri kaydedin. Tasarımcı araç çubuğunda **Kaydet**'i seçin.

   ![Tamamlanan mantıksal uygulama](./media/logic-apps-create-a-logic-app/save-complete-logic-app.png)

   Mantıksal uygulamanızı şimdi test etmek için sonraki bölüme geçin.

## <a name="4-run-and-test-your-workflow"></a>4. İş akışınızı çalıştırma ve test etme

1. Mantıksal uygulamanızı test etmek üzere el ile çalıştırmak için tasarımcı araç çubuğundan **Çalıştır**'ı seçin. Ya da mantıksal uygulamanızın ayarladığınız zamanlamaya göre RSS akışını denetlemesine izin verebilirsiniz.

   ![Mantıksal uygulamanızı çalıştırma](./media/logic-apps-create-a-logic-app/run-complete-logic-app.png)

   Mantıksal uygulamanız yeni öğeler bulursa, mantıksal uygulama seçtiğiniz verileri içeren e-postayı gönderir. Örneğin:

   ![Yeni RSS akışı öğesi için gönderilen e-posta](./media/logic-apps-create-a-logic-app/rss-feed-email.png)

   Mantıksal uygulamanız yeni bir öğe bulamazsa e-posta gönderen eylemi atlar ve belirlenen aralık boyunca bekledikten sonra tekrar denetleme gerçekleştirir. 

2. Mantıksal uygulamanızın çalışma ve tetikleyici geçmişini gözden geçirmek için **Genel Bakış**'ı seçin.
Bir çalıştırma hakkında daha fazla bilgiye ulaşmak için çalıştırmayla ilgili satırı seçin.

   ![Mantıksal uygulamanın çalışma ve tetikleyici geçmişini izleme ve görüntüleme](./media/logic-apps-create-a-logic-app/logic-app-run-trigger-history.png)

   > [!TIP]
   > Beklediğiniz verileri bulamazsanız, araç çubuğunda **Yenile**'yi seçmeyi deneyin.

   Çalıştırma Ayrıntıları görünümünde çalıştırmanın başarılı veya başarısız olma durumunun yanı sıra başarılı ve başarısız olan adımlar gösterilir. 

   ![Bir mantıksal uygulama çalıştırmasının ayrıntılarını görüntüleme](./media/logic-apps-create-a-logic-app/logic-app-run-details.png)

   Mantıksal uygulamanızın durumu, çalıştırma geçmişi ve tetikleyici geçmişi hakkında daha fazla bilgi almak için bkz. [Mantıksal uygulamanızla ilgili sorun giderme](../logic-apps/logic-apps-diagnosing-failures.md).

3. Her bir adımın giriş ve çıkışlarını görüntülemek için gözden geçirmek istediğiniz adımı genişletin. Bu bilgiler mantıksal uygulamanızdaki sorunları tespit etmenize ve gidermenize yardımcı olabilir. Örneğin:

   ![Adım ayrıntıları görüntüleme](./media/logic-apps-create-a-logic-app/logic-app-run-details-expanded.png)

   Daha fazla bilgi için bkz. [Mantıksal uygulamanızı izleme](../logic-apps/logic-apps-monitor-your-logic-apps.md).

Tebrikler, ilk temel mantıksal uygulamanızı oluşturup çalıştırdınız. Bu örnek herhangi bir kod kullanmadan, sistemleri ve hizmetleri tümleştirme amacıyla süreçleri otomatik hale getiren iş akışlarını ne kadar kolay oluşturabileceğinizi öğrendiniz.

> [!NOTE]
> Mantıksal uygulamanız siz kapatana kadar çalışmaya devam eder. Uygulamanızı geçici olarak kapatmak için sonraki bölüme geçin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticide Azure aboneliğinize ücret uygulanmasına neden olabilecek kaynaklar kullanılmakta ve eylemler gerçekleştirilmektedir. Öğreticiyi ve testlerinizi tamamladıktan sonra ücret uygulanmasını istemediğiniz kaynakları devre dışı bırakmayı veya silmeyi unutmayın.

Mantıksal uygulamanızı silmeden çalışmasını ve e-posta göndermesini durdurabilirsiniz. Mantıksal uygulama menüsünden **Genel Bakış**'ı seçin. Araç çubuğunda **Devre dışı bırak**'ı seçin.

![Mantıksal uygulamanızı kapatma](./media/logic-apps-create-a-logic-app/turn-off-disable-logic-app.png)

## <a name="faq"></a>SSS

**S:** Mantıksal uygulamamla başka neler yapabilirim? </br>
**Y:** Gerçekleştirebileceğiniz diğer görevlere örnek olarak düzenleme, JSON tanımını görüntüleme, etkinlik günlüğünü gözden geçirme veya mantıksal uygulamanızı silme verilebilir.

Diğer mantıksal uygulama yönetim görevlerini bulmak için mantıksal uygulama menüsündeki şu komutları gözden geçirin:

| Görev | Adımlar | 
| ---- | ----- | 
| Uygulamanızın durumunu, çalıştırmalarını, tetikleyici geçmişini ve genel bilgilerini görüntüleme | **Genel Bakış**’ı seçin. | 
| Uygulamanızı düzenleme | **Mantıksal Uygulama Tasarımcısı**’nı seçin. | 
| Uygulamanızın iş akışı JSON tanımını görüntüleme | **Mantıksal Uygulama Kod Görünümü**’nü seçin. | 
| Mantıksal uygulamanız üzerinde gerçekleştirilen işlemleri görüntüleme | **Ekinlik günlüğü**’nü seçin. | 
| Mantıksal uygulamanızın geçmiş sürümlerini görüntüleme | **Sürümler**’i seçin. | 
| Uygulamanızı geçici olarak kapatma | **Genel Bakış**'ı seçin, ardından araç çubuğunda **Devre dışı bırak**'ı seçin. | 
| Uygulamanızı silme | **Genel Bakış**'ı seçin, ardından araç çubuğunda **Sil**'ı seçin. Mantıksal uygulamanızın adını girip **Sil**’i seçin. | 
||| 

## <a name="get-support"></a>Destek alın

* Azure Logic Apps hakkındaki sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.

* Azure Logic Apps ve bağlayıcılarını geliştirme konusunda yardımcı olmak mı istiyorsunuz? [Azure Logic Apps User Voice sitesinden](http://aka.ms/logicapps-wish) oy verin veya fikirlerinizi paylaşın.

## <a name="next-steps"></a>Sonraki adımlar

* [Visual Studio ile mantıksal uygulamanızı oluşturma](../logic-apps/logic-apps-deploy-from-vs.md)
* [Koşul ekleme ve iş akışları çalıştırma](../logic-apps/logic-apps-use-logic-app-features.md)
*   [Mantıksal uygulama şablonları](../logic-apps/logic-apps-use-logic-app-templates.md)
* [Azure Resource Manager şablonlarından mantıksal uygulama oluşturma](../logic-apps/logic-apps-arm-provision.md)
* [Logic Apps kullanım ölçümü](../logic-apps/logic-apps-pricing.md) 
* [Logic Apps fiyatlandırması](https://azure.microsoft.com/pricing/details/logic-apps)
