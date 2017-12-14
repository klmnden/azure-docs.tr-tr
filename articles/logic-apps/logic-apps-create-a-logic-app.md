---
title: "Sistemler ile bulut hizmetleri arasındaki iş akışlarını otomatikleştirme - Azure Logic Apps | Microsoft Docs"
description: "Sistem tümleştirme ve kuruluş uygulaması tümleştirme (EAI) senaryoları için iş akışlarını otomatikleştirme amacıyla mantıksal uygulamalar oluşturma"
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
ms.date: 10/20/2017
ms.author: LADocs; estfan
ms.openlocfilehash: 5906605192f9b03f612e6ca3a445434a23713d7f
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2017
---
# <a name="automate-your-first-workflow-to-process-data-with-a-logic-app"></a>Bir mantıksal uygulamayla veri işlemek için ilk iş akışınızı otomatikleştirme

Kuruluşunuz için sistemleri ve hizmetleri tümleştirme amacıyla iş akışlarını ve iş süreçlerini [Azure Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md) kullanarak otomatikleştirebilirsiniz. Bu hızlı başlangıç, mantıksal uygulama oluşturarak otomatikleştirilmiş iş akışı derleme ve çalıştırma süreçlerinin ne kadar kolay olduğunu göstermektedir. Örnek uygulama bir web sitesinin RSS akışındaki yeni öğeleri denetleyen ve her yeni öğe için e-posta gönderen bir iş akışını nasıl otomatikleştireceğinizi gösterir.

Bu örnek mantıksal uygulama bu örnekteki gibi bir e-posta gönderir:

![Yeni RSS akışı öğesi için gönderilen e-posta](./media/logic-apps-create-a-logic-app/rss-feed-email.png)

Derleyeceğiniz yüksek düzeyli mantıksal uygulama iş akışı şudur:

![Genel bakış - mantıksal uygulama örneği](./media/logic-apps-create-a-logic-app/logic-app-simple-overview.png)

Bu hızlı başlangıçta şunları yapmayı öğrenirsiniz:

> [!div class="checklist"]
> * Boş bir mantıksal uygulama oluşturma.
> * RSS akışında yeni bir öğe göründüğünde iş akışını başlatacak bir tetikleyici ekleme.
> * RSS akış öğesi hakkındaki ayrıntılı bilgileri içeren bir e-posta göndermek için bir eylem ekleme.
> * Mantıksal uygulama iş akışınızı çalıştırma.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Ön koşullar

* Bildirim göndermek için Azure Logic Apps tarafından desteklenen bir e-posta sağlayıcısından alınmış bir e-posta hesabı. Örneğin Office 365 Outlook, Outlook.com veya Gmail kullanabilirsiniz. Desteklenen diğer e-posta bağlayıcıları için [bağlayıcı listesini gözden geçirin](https://docs.microsoft.com/connectors/). Bu hızlı başlangıçta Office 365 Outlook kullanılmaktadır.

  > [!TIP]
  > Kişisel bir [Microsoft hesabınız](https://account.microsoft.com/account) varsa Outlook.com hesabınız vardır. Aksi takdirde, bir Azure iş veya okul hesabınız varsa Office 365 Outlook hesabınız vardır.

* Web sitesinin RSS akışının bağlantısı. Bu örnekte [Reuters web sitesindeki en önemli haberler için RSS akışı](http://feeds.reuters.com/reuters/topNews) kullanılmıştır: `http://feeds.reuters.com/reuters/topNews`

Bu hızlı başlangıç için kod yazmanız gerekmez ancak Logic Apps, [Azure İşlevleri](../azure-functions/functions-overview.md) ile bir mantıksal uygulamadan kendi kodunuzu çalıştırma gibi kod kullanılan diğer senaryoları da destekler.

## <a name="create-a-blank-logic-app"></a>Boş bir mantıksal uygulama oluşturma 

1. [Azure portalı](https://portal.azure.com "Azure portalı") oturumunu açın. 

2. Azure ana menüsünden **Yeni** > **Kurumsal Tümleştirme** > **Mantıksal Uygulama**'yı seçin.

   ![Azure portalı, Yeni, Kurumsal Tümleştirme, Mantıksal Uygulama](./media/logic-apps-create-a-logic-app/azure-portal-create-logic-app.png)

3. Bu görüntünün altındaki tabloda yer alan ayarlarla mantıksal uygulamanızı oluşturun:

   ![Mantıksal uygulama ayrıntılarını sağlayın](./media/logic-apps-create-a-logic-app/logic-app-settings.png)

   | Ayar | Önerilen değer | Açıklama | 
   | ------- | --------------- | ----------- | 
   | **Ad** | *mantıksal-uygulamanızın-adı* | Mantıksal uygulama için benzersiz bir ad girin. | 
   | **Abonelik** | *Azure-aboneliğinizin-adı* | Kullanmak istediğiniz Azure aboneliğini seçin. | 
   | **Kaynak grubu** | *Azure-kaynak-grubunuzun-adı* | Bu mantıksal uygulama ve bu uygulamayla ilişkilendirilmiş olan tüm kaynakları düzenlemek için bir [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) oluşturun. | 
   | **Konum** | *Azure-veri-merkezi-bölgeniz* | Batı ABD gibi mantıksal uygulamanızın dağıtılacağı veri merkezi bölgesini seçin. | 
   | **Log Analytics** | Kapalı | Mantıksal uygulamanız için tanılama günlüğüne kaydetme özelliğini açın ancak bu hızlı başlangıç için **Kapalı** ayarını kullanın. | 
   |||| 

4. Hazır olduğunuzda **Panoya sabitle**'yi seçin. Bu şekilde mantıksal uygulamanız otomatik olarak Azure panonuzda görüntülenir ve dağıtımdan sonra açılır. **Oluştur**’u seçin.

   > [!NOTE]
   > Mantıksal uygulamanızı sabitlemek istemezseniz öğreticiye devam edebilmek için dağıtım sonrasında mantıksal uygulamanızı el ile bulup açmanız gerekir.

   Azure mantıksal uygulamanızı dağıttıktan sonra Logic Apps Tasarımcısı açılır ve tanıtım videosu bulunan bir sayfa görüntülenir. 
   Videonun altında sık kullanılan mantıksal uygulama desenlerine ait şablonları bulabilirsiniz. 
   Bu hızlı başlangıçta mantıksal uygulama sıfırdan oluşturulmaktadır. 

5. Sayfayı kaydırarak tanıtım videosunu ve sık kullanılan tetikleyicileri atlayın. **Şablonlar** bölümünde **Boş Mantıksal Uygulama**'yı seçin.

   ![Boş mantıksal uygulama şablonunu seçin](./media/logic-apps-create-a-logic-app/choose-logic-app-template.png)

   Logic Apps Tasarımcısı'nda mantıksal uygulama iş akışlarını başlatmak için kullanılan uygun bağlayıcılar ve tetikleyicileri gösterilir.

   ![Mantıksal uygulama tetikleyicileri](./media/logic-apps-create-a-logic-app/logic-app-triggers.png)

## <a name="add-a-trigger-to-detect-new-items"></a>Yeni öğeleri algılamak için bir tetikleyici ekleme

Her mantıksal uygulama iş akışı bir [tetikleyici](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) ile başlar. Tetikleyici, belirli bir olay meydana geldiğinde veya yeni veriler belirlediğiniz bir koşulu karşıladığında etkinleştirilir. Tetikleyici her etkinleştirildiğinde Logic Apps altyapısı iş akışınızı başlatan ve çalıştıran bir mantıksal uygulama örneği oluşturur.

1. Arama kutusuna "rss" girin. Şu tetikleyiciyi seçin: **RSS - Akış öğesi yayımlandığında** 

   ![Tetikleyiciyi seçin: "RSS - Akış öğesi yayımlandığında"](./media/logic-apps-create-a-logic-app/rss-trigger.png)

2. İzlemek istediğiniz RSS akışına ait bağlantıyı girin. Örneğin: `http://feeds.reuters.com/reuters/topNews`. Yinelenme aralığını ve sıklığını ayarlayın. Bu örnekte akış beş dakikada bir kontrol edilmektedir.

   ![Tetikleyicinin RSS akışı, sıklık ve aralık ayarını yapma](./media/logic-apps-create-a-logic-app/rss-trigger-setup.png)

   Logic Apps, RSS akışına bir bağlantı oluşturur.

   > [!TIP]
   > Tasarımcı görünümünü sadeleştirmek için bir şeklin ayrıntılarını daraltabilir ve gizleyebilirsiniz. Bunun için şeklin başlık çubuğuna tıklamanız yeterlidir.

3. Çalışmanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet**'i seçin. 

   ![Mantıksal uygulamanızı kaydetme](./media/logic-apps-create-a-logic-app/save-logic-app.png)

   Mantıksal uygulamanız çalışıyor ancak RSS akışını kontrol etme dışında bir işlem gerçekleştirmiyor. Şimdi tetikleyici etkinleştirildiğinde gerçekleştirilecek bir eylem ekleyelim.

## <a name="add-an-action-to-send-email"></a>E-posta göndermek için eylem ekleme

Artık bir tetikleyiciniz olduğu için RSS akışında yeni bir öğe göründüğünde e-posta gönderen bir [eylem](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) ekleyebilirsiniz. İş akışınız tetikleyici etkinleştirildiğinde bu eylemi gerçekleştirir.

1. Logic Apps Tasarımcısı'nda tetikleyicinin altında **+ Yeni adım** > **Eylem ekle**'yi seçin.

   ![Eylem ekleme](./media/logic-apps-create-a-logic-app/add-new-action.png)

   Tasarımcı, mantıksal uygulamanızın tetikleyici etkinleştirildiğinde gerçekleştirebileceği eylemleri gösterir.

   ![Eylem listesinden seçim yapma](./media/logic-apps-create-a-logic-app/logic-app-actions.png)

2. Arama kutusuna "e-posta gönder" yazın. Kullanmak istediğiniz e-posta bağlayıcısını bulun ve seçin. Ardından bağlayıcı için "e-posta gönder" eylemini seçin. Örneğin: 

   * Azure iş veya okul hesapları için Office 365 Outlook girişini seçin. 
   * Kişisel Microsoft hesapları için Outlook.com girişini seçin. 
   * Gmail hesapları için Gmail girişini seçin. 

   Bu hızlı başlangıçta Office 365 Outlook kullanılmaktadır. 
   Farklı bir e-posta sağlayıcısı kullandığınızda adımlar aynı olacaktır ancak kullanıcı arabirimi farklı olabilir. 

   ![Şu eylemi seçin: "Office 365 Outlook - E-posta gönder"](./media/logic-apps-create-a-logic-app/actions.png)

3. Kimlik bilgileriniz istendiğinde, e-posta hesabınıza ait kullanıcı adı ve parolayla oturum açın. 

   Logic Apps, e-posta hesabınıza bir bağlantı oluşturur.

4. Şimdi e-postaya dahil etmek istediğiniz verileri belirleyin. 

   1. **Alıcı** kutusuna alıcının e-posta adresini girin. 
   Test için kendi e-posta adresinizi kullanabilirsiniz.

   2. **Konu** kutusuna e-postanın konusunu girin. 
   Bu örnek için aşağıda gösterildiği gibi "Yeni RSS öğesi: " yazın:

      ![E-posta konusunu girin](./media/logic-apps-create-a-logic-app/logic-app-add-subject.png)

      Düzenleme kutusunun içine tıkladığınızda açılan **Dinamik içerik ekleme** listesinden eyleminize dahil edilecek uygun veri alanlarını seçebilirsiniz. 
      Dinamik içerik listesi açılmazsa ilgili düzenleme kutusunun altındaki **Dinamik içerik ekle** bağlantısını seçin.

   3. **Dinamik içerik ekle** listesinden öğe başlığını e-postaya ekleyen **Akış başlığı** öğesini seçin.

      ![E-posta konusunu girin](./media/logic-apps-create-a-logic-app/logic-app-select-field.png)

      İşlem tamamlandığında e-posta konusu şu örnekteki gibi görünür:

      ![Akış başlığı eklendi](./media/logic-apps-create-a-logic-app/added-feed-title.png)

      > [!NOTE] 
      > **categories-item** gibi dizi içeren bir alan seçerseniz tasarımcı eyleme otomatik olarak ilgili alana başvuran bir "For each" döngüsü ekler. Bu şekilde mantıksal uygulamanız ilgili eylemi dizideki tüm öğeler için gerçekleştirebilir. 
      > 
      > Döngüyü kaldırmak için döngünün başlık çubuğundaki üç nokta (**...**) simgesini ve ardından **Sil**'i seçin.

   4. **Gövde** kutusuna e-posta gövdesinin içeriğini girin. 
   Bu örnekte bu metni girin ve şu alanları seçin:

      ![E-posta gövdesi için içerik ekleme](./media/logic-apps-create-a-logic-app/logic-app-complete.png)

      | Alan | Açıklama | 
      | ----- | ----------- | 
      | **Akış başlığı** | Öğenin başlığını gösterir. | 
      | **Akış yayımlanma zamanı** | Öğenin yayımlandığı tarihi ve saati gösterir. | 
      | **Birincil akış bağlantısı** | Öğenin URL'sini gösterir. | 
      ||| 

      > [!TIP]
      > Bir düzenleme kutusuna boş satır eklemek için Shift + Enter tuşlarını kullanın. 
      
5. Çalışmanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet**'i seçin.

   ![Tamamlanan mantıksal uygulama](./media/logic-apps-create-a-logic-app/save-complete-logic-app.png)

## <a name="run-your-logic-app-workflow"></a>Mantıksal uygulama iş akışınızı çalıştırma

Mantıksal uygulamanızı el ile başlatmak için tasarımcı araç çubuğundan **Çalıştır**'ı seçin. Bunu yapmak istemezseniz mantıksal uygulamanızın belirlediğiniz zamanda çalışması için bekleyebilirsiniz.

![Mantıksal uygulamanızı çalıştırma](./media/logic-apps-create-a-logic-app/run-complete-logic-app.png)

RSS akışında yeni öğeler olduğunda mantıksal uygulamanız her yeni öğe için bir e-posta gönderir. Bu mantıksal uygulamanın gönderdiği örnek Outlook iletisi aşağıda verilmiştir:

![Yeni RSS akışı öğesi için gönderilen e-posta](./media/logic-apps-create-a-logic-app/rss-feed-email.png)

Akışta yeni öğe yoksa mantıksal uygulamanız e-posta gönderen adımı atlar ve belirlenen aralık boyunca bekledikten sonra tekrar denetleme gerçekleştirir. 

> [!TIP]
> E-posta gelmezse istenmeyen e-posta klasörüne bakın. Mantıksal uygulamanızın düzgün bir şekilde çalışıp çalışmadığından emin değilseniz bkz. [Mantıksal uygulama sorunlarını giderme](../logic-apps/logic-apps-diagnosing-failures.md).

Tebrikler, ilk mantıksal uygulamanızı oluşturup çalıştırdınız. Bu hızlı başlangıçta, sistem ve hizmet tümleştirmesi için otomatikleştirilmiş iş akışları oluşturmanın ne kadar kolay ve hızlı olduğu gösterilmiştir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Mantıksal uygulamanız kapatılana veya silinene kadar çalışmaya ve Azure aboneliğinize ücret yansıtmaya devam eder. Ayrıca mantıksal uygulamanız için oluşturduğunuz bağlantılar, mantıksal uygulamanızı silseniz bile kalır. 

İşlemleri tamamladıktan sonra ücret uygulanmasını veya tutmak istemediğiniz kaynakları devre dışı bırakmayı veya silmeyi unutmayın. Bu hızlı başlangıç için oluşturduğunuz tüm kaynakları silmek isterseniz bu mantıksal uygulama için oluşturduğunuz Azure kaynak grubunu silin. 

### <a name="delete-resource-group"></a>Kaynak grubunu silme

Mantıksal uygulamanızla ilişkilendirilmiş kaynakları saklamak istemiyorsanız bu hızlı başlangıç için oluşturduğunuz kaynak grubunu ve ilgili tüm kaynakları silin. [Azure kaynak gruplarını yönetme](../azure-resource-manager/resource-group-portal.md#manage-resources) hakkında daha fazla bilgi edinin.

1. Azure menüsünden **Kaynak grupları**'nı seçin.

2. Silmek istediğiniz kaynak grubunu belirtin. Kaynak grubu menüsünde seçili değilse **Genel Bakış**'ı seçin. 

3. Grup içinde silmek istediğiniz tüm kaynakları gözden geçirin. Hazır olduğunuzda kaynak grubu araç çubuğundaki **Kaynak grubunu sil**'i seçin.

### <a name="turn-off-logic-app"></a>Mantıksal uygulamayı kapatma

Çalışmanızı silmeden mantıksal uygulamanızı durdurmak için uygulamanızı devre dışı bırakın. 

Mantıksal uygulama menüsünden **Genel Bakış**'ı seçin. Araç çubuğunda **Devre dışı bırak**'ı seçin.

  ![Mantıksal uygulamanızı kapatma](./media/logic-apps-create-a-logic-app/turn-off-disable-logic-app.png)

  > [!TIP]
  > Mantıksal uygulama menüsü görünmüyorsa Azure panosuna dönüp mantıksal uygulamanızı yeniden açmayı deneyin.

### <a name="delete-logic-app"></a>Mantıksal uygulamayı silme

Mantıksal uygulamanızı silip oluşturduğunuz bağlantılar gibi ilgili diğer tüm kaynakları tutabilirsiniz.

1. Mantıksal uygulama menüsünden **Genel Bakış**'ı seçin. Araç çubuğunda **Sil**'i seçin. 

   ![Mantıksal uygulamanızı silme](./media/logic-apps-create-a-logic-app/delete-logic-app.png)

   > [!TIP]
   > Mantıksal uygulama menüsü görünmüyorsa Azure panosuna dönüp mantıksal uygulamanızı yeniden açmayı deneyin.

2. Mantıksal uygulamanızı silmek istediğinizi onaylayın ve **Sil**'i seçin.

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](http://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Önceden oluşturulmuş şablonlardan mantıksal uygulama iş akışları oluşturma](../logic-apps/logic-apps-create-logic-apps-from-templates.md)