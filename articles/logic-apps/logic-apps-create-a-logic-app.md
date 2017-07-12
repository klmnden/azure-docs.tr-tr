---
title: "Bulut uygulamaları ile bulut hizmetleri arasında ilk iş akışınızı oluşturma - Azure Logic Apps | Microsoft Docs"
description: "Azure Logic Apps’te iş akışları oluşturup çalıştırarak, sistem tümleştirmeden kuruluş uygulaması tümleştirmeye (EAI) kadar tüm senaryolar için iş süreçlerini otomatik hale getirin"
author: jeffhollan
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
ms.date: 03/31/2017
ms.author: LADocs; jehollan; estfan
ms.translationtype: Human Translation
ms.sourcegitcommit: c308183ffe6a01f4d4bf6f5817945629cbcedc92
ms.openlocfilehash: 204bf123509729b60b55c306050cef54aa7fecc5
ms.contentlocale: tr-tr
ms.lasthandoff: 05/17/2017

---

<a id="create-your-first-logic-app-workflow-to-automate-processes-between-cloud-apps-and-cloud-services" class="xliff"></a>

# Bulut uygulamaları ile bulut hizmetleri arasında süreçleri otomatik hale getirmek için ilk mantıksal uygulama iş akışınızı oluşturma

[Azure Logic Apps](logic-apps-what-are-logic-apps.md) ile iş akışları oluşturup çalıştırdığınızda, herhangi bir kod yazmadan iş süreçlerini daha kolay ve hızlı bir şekilde otomatik hale getirebilirsiniz. Bu ilk örnekte, bir web sitesindeki yeni içerikleri belirlemek için RSS akışını denetleyen temel bir mantıksal uygulama iş akışı oluşturma işlemi gösterilmektedir. Web sitesinin akışında yeni öğeler göründüğünde, mantıksal uygulama bir Outlook veya Gmail hesabından e-posta gönderir.

Bir mantıksal uygulama oluşturup çalıştırmak için şu öğeler gerekir:

* Azure aboneliği. Bir aboneliğiniz yoksa [ücretsiz bir Azure hesabı ile başlayabilirsiniz](https://azure.microsoft.com/free/). Ya da [Kullandıkça Öde aboneliğine kaydolabilirsiniz](https://azure.microsoft.com/pricing/purchase-options/).

  Azure aboneliğiniz, mantıksal uygulama kullanımını faturalamak için kullanılır. [Kullanım ölçümü](../logic-apps/logic-apps-pricing.md) ve [fiyatlandırma](https://azure.microsoft.com/pricing/details/logic-apps) işlemlerinin Azure Logic Apps’te nasıl işlediğini öğrenin.

Ayrıca, bu örnek şu öğeleri gerektirir:

* Bir Outlook.com, Office 365 Outlook veya Gmail hesabı

    > [!TIP]
    > Kişisel bir [Microsoft hesabınız](https://account.microsoft.com/account) varsa Outlook.com hesabınız vardır. Aksi takdirde, bir Azure iş veya okul hesabınız varsa **Office 365 Outlook** hesabınız vardır.

* Web sitesinin RSS akışının bağlantısı. Bu örnekte [CNN.com web sitesindeki en önemli haberler için RSS akışı](http://rss.cnn.com/rss/cnn_topstories.rss) kullanılmıştır: `http://rss.cnn.com/rss/cnn_topstories.rss`

<a id="add-a-trigger-that-starts-your-workflow" class="xliff"></a>

## İş akışınızı başlatan bir tetikleyici ekleme

[*Tetikleyici*](./logic-apps-what-are-logic-apps.md#logic-app-concepts), mantıksal uygulama iş akışınızı başlatan bir olaydır ve mantıksal uygulamanız için gereken ilk öğedir.

1. [Azure portalı](https://portal.azure.com "Azure portalı") oturumunu açın.

2. Resimde gösterildiği gibi, sol menüden **Yeni** > **Kurumsal Tümleştirme** > **Mantıksal Uygulama**’yı seçin:

     ![Azure portalı, Yeni, Kurumsal Tümleştirme, Mantıksal Uygulama](media/logic-apps-create-a-logic-app/azure-portal-create-logic-app.png)

   > [!TIP]
   > Ayrıca **Yeni**’yi seçebilir, ardından arama kutusuna `logic app` yazıp Enter tuşuna basabilirsiniz. Ardından **Mantıksal Uygulama** > **Oluştur**’u seçin.

3. Mantıksal uygulamanızı adlandırın ve Azure aboneliğinizi seçin. Şimdi, ilgili Azure kaynaklarını düzenleyip yönetmenize yardımcı olan bir Azure kaynak grubu oluşturun veya seçin. Son olarak, mantıksal uygulamanızı barındıracak veri merkezi konumunu seçin. Hazır olduğunuzda **Panoya sabitle** ve ardından **Oluştur**’u seçin.

     ![Mantıksal uygulama ayrıntıları](media/logic-apps-create-a-logic-app/logic-app-settings.png)

   > [!NOTE]
   > **Panoya sabitle**’yi seçtiğinizde, mantıksal uygulamanız dağıtıldıktan sonra Azure panosunda gösterilir ve otomatik olarak açılır. Mantıksal uygulamanız panoda görünmüyorsa, **Tüm kaynaklar** kutucuğunda **Daha Fazla Göster**’ü ve mantıksal uygulamanızı seçin. Veya sol menüden **Diğer hizmetler**’i seçin. **Kurumsal Tümleştirme** altında **Logic Apps**’ı belirleyip mantıksal uygulamanızı seçin.

4. Mantıksal uygulamanızı ilk kez açtığınızda, Mantıksal Uygulama Tasarımcısı başlamak için kullanabileceğiniz şablonları gösterir. Mantıksal uygulamanızı sıfırdan oluşturabilmek için şimdilik **Boş Mantıksal Uygulama**’yı seçin.

    Mantıksal Uygulama Tasarımcısı açılır ve mantıksal uygulamanızda kullanabileceğiniz hizmetler ile *tetikleyicileri* gösterir.

5. Arama kutusuna `RSS` yazıp şu tetikleyiciyi seçin: **RSS - Akış öğesi yayımlandığında** 

    ![RSS tetikleyicisi](media/logic-apps-create-a-logic-app/rss-trigger.png)

6. İzlemek istediğiniz web sitesinin RSS akışına ait bağlantıyı girin. 

     Ayrıca **Sıklık** ve **Aralık** değerlerini değiştirebilirsiniz. 
     Bu ayarlar mantıksal uygulamanızın yeni öğeleri ne sıklıkla denetleyeceğini belirler ve bu süre boyunca bulunan tüm öğeleri döndürür.

     Bu örnekte her gün CNN web sitesinde yayımlanan en önemli haberlere bakalım.

     ![Tetikleyicinin RSS akışı, sıklık ve aralık ayarını yapma](media/logic-apps-create-a-logic-app/rss-trigger-setup.png)

7. Çalışmanızı şimdilik kaydedin. (Tasarımcı komut çubuğunda **Kaydet**’i seçin.)

   ![Mantıksal uygulamanızı kaydetme](media/logic-apps-create-a-logic-app/save-logic-app.png)

   Mantıksal uygulamanızı kaydettiğinizde etkin hale gelir, ancak şu anda mantıksal uygulamanız yalnızca belirtilen RSS akışındaki yeni öğeleri denetler. 
   Bu örneği daha kullanışlı hale getirmek için tetikleyici başlatıldıktan sonra mantıksal uygulamanızın gerçekleştireceği bir eylem ekliyoruz.

<a id="add-an-action-that-responds-to-your-trigger" class="xliff"></a>

## Tetikleyicinize yanıt veren bir eylem ekleme

[*Eylem*](./logic-apps-what-are-logic-apps.md#logic-app-concepts), mantıksal uygulama iş akışınız tarafından gerçekleştirilen bir görevdir. Mantıksal uygulamanıza bir tetikleyici ekledikten sonra, bu tetikleyici tarafından oluşturulan işlemleri gerçekleştirmek için bir eylem ekleyebilirsiniz. Bu örnekte, web sitesinin RSS akışında yeni öğeler göründüğünde e-posta gönderen bir eylem ekleyeceğiz.

1. Tasarımcıda, resimde gösterildiği gibi tetikleyicinizin altındaki **Yeni adım** > **Eylem ekle** öğesini seçin:

   ![Eylem ekleme](media/logic-apps-create-a-logic-app/add-new-action.png)

   Tasarımcı, tetikleyiciniz başlatıldığında gerçekleştirilecek bir eylem seçebilmeniz için [kullanılabilir bağlayıcıları](../connectors/apis-list.md) gösterir.

2. E-posta hesabınıza bağlı olarak, Outlook veya Gmail’e yönelik adımları izleyin.

   * Outlook hesabınızdan e-posta göndermek için arama kutusuna `outlook` girin. **Hizmetler** altında kişisel Microsoft hesaplarınız için **Outlook.com** veya Azure iş ya da okul hesapları için **Office 365 Outlook**’u seçin. 
   **Eylemler** altında **E-posta gönder**’i seçin.

       ![Outlook "E-posta gönder" eylemini seçin](media/logic-apps-create-a-logic-app/actions.png)

   * Gmail hesabınızdan e-posta göndermek için arama kutusuna `gmail` girin. 
   **Eylemler** altında **E-posta gönder**’i seçin.

       !["Gmail - E-posta gönder" öğesini seçin](media/logic-apps-create-a-logic-app/actions-gmail.png)

3. Kimlik bilgileriniz istendiğinde, e-posta hesabınıza ait kullanıcı adı ve parolayla oturum açın. 

4. Bu eylemin hedef e-posta adresi gibi ayrıntılarını belirtin ve e-postaya eklenecek verilerin parametrelerini seçin, örneğin:

   ![E-postaya eklenecek verileri seçme](media/logic-apps-create-a-logic-app/rss-action-setup.png)

    Outlook’u seçerseniz mantıksal uygulamanız bu örnektekine benzer şekilde görünür:

    ![Tamamlanan mantıksal uygulama](media/logic-apps-create-a-logic-app/save-run-complete-logic-app.png)

5.    Yaptığınız değişiklikleri kaydedin. (Tasarımcı komut çubuğunda **Kaydet**’i seçin.)

6. Mantıksal uygulamanızı test için el ile çalıştırabilirsiniz. Tasarımcı komut çubuğunda **Çalıştır**’ı seçin. Ya da mantıksal uygulamanızın ayarladığınız zamanlamaya göre RSS akışını denetlemesine izin verebilirsiniz.

   Mantıksal uygulamanız yeni öğeler bulursa, mantıksal uygulama seçtiğiniz verileri içeren e-postayı gönderir. 
   Yeni öğe bulunmazsa, mantıksal uygulamanız e-posta gönderen eylemi atlar.

7. Mantıksal uygulamanızın çalışma ve tetikleyici geçmişini izlemek ve denetlemek için, mantıksal uygulama menüsünde **Genel Bakış**’ı seçin.

   ![Mantıksal uygulamanın çalışma ve tetikleyici geçmişini izleme ve görüntüleme](media/logic-apps-create-a-logic-app/logic-app-run-trigger-history.png)

   > [!TIP]
   > Beklediğiniz verileri bulamazsanız, komut çubuğunda **Yenile**’yi seçmeyi deneyin.

   Mantıksal uygulamanızın durumu veya çalışma ve tetikleyici geçmişi hakkında daha fazla bilgi almak için bkz. [Mantıksal uygulamanızla ilgili sorun giderme](logic-apps-diagnosing-failures.md).

      > [!NOTE]
      > Mantıksal uygulamanız siz kapatana kadar çalışmaya devam eder. Uygulamanızı şimdilik kapatmak için, mantıksal uygulama menüsünde **Genel Bakış**’ı seçin. Komut çubuğunda **Devre Dışı Bırak**’ı seçin.

Tebrikler, ilk temel mantıksal uygulamanızı oluşturup çalıştırdınız. Ayrıca, herhangi bir kod kullanmadan, süreçleri otomatik hale getiren iş akışlarını ne kadar kolay oluşturabileceğinizi ve bulut uygulamaları ile bulut hizmetlerini tümleştirmeyi öğrendiniz.

<a id="manage-your-logic-app" class="xliff"></a>

## Mantıksal uygulamanızı yönetme

Uygulamanızı yönetmek için durumu denetleme, düzenleme, geçmişi görüntüleme, kapatma ya da mantıksal uygulamanızı silme gibi görevler gerçekleştirebilirsiniz.

1. [Azure portalı](https://portal.azure.com "Azure portalı") oturumunu açın.

2. Sol menüden **Diğer hizmetler**’i seçin. **Kurumsal Tümleştirme** altında **Logic Apps**’ı seçin. Mantıksal uygulamanızı seçin. 

   Mantıksal uygulama menüsünde şu mantıksal uygulama yönetim görevlerini bulabilirsiniz:

   |Görev|Adımlar| 
   |:---|:---| 
   | Uygulamanızın durumunu, yürütme geçmişini ve genel bilgilerini görüntüleme| **Genel Bakış**’ı seçin.| 
   | Uygulamanızı düzenleme | **Mantıksal Uygulama Tasarımcısı**’nı seçin. | 
   | Uygulamanızın iş akışı JSON tanımını görüntüleme | **Mantıksal Uygulama Kod Görünümü**’nü seçin. | 
   | Mantıksal uygulamanız üzerinde gerçekleştirilen işlemleri görüntüleme | **Ekinlik günlüğü**’nü seçin. | 
   | Mantıksal uygulamanızın geçmiş sürümlerini görüntüleme | **Sürümler**’i seçin. | 
   | Uygulamanızı geçici olarak kapatma | **Genel Bakış**’ı seçin, ardından komut çubuğunda **Devre Dışı Bırak**’ı seçin. | 
   | Uygulamanızı silme | **Genel Bakış**’ı seçin, ardından komut çubuğunda **Sil**’i seçin. Mantıksal uygulamanızın adını girip **Sil**’i seçin. | 

<a id="get-help" class="xliff"></a>

## Yardım alın

Sorular sormak, soruları yanıtlamak ve diğer Azure Logic Apps kullanıcılarının neler yaptığını öğrenmek için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.

Azure Logic Apps ve bağlayıcıları geliştirmeye yardımcı olmak için, [Azure Logic Apps kullanıcı geri bildirim sitesinde](http://aka.ms/logicapps-wish) oy kullanın veya fikirlerinizi paylaşın.

<a id="next-steps" class="xliff"></a>

## Sonraki adımlar

*  [Koşul ekleme ve iş akışları çalıştırma](../logic-apps/logic-apps-use-logic-app-features.md)
*     [Mantıksal uygulama şablonları](../logic-apps/logic-apps-use-logic-app-templates.md)
*  [Azure Resource Manager şablonlarından mantıksal uygulama oluşturma](../logic-apps/logic-apps-arm-provision.md)

