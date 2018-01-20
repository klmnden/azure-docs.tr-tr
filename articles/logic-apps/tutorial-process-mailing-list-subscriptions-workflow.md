---
title: "Posta listesi istekleri - Azure mantıksal uygulamaları işlemek için onay iş akışları oluşturmak | Microsoft Docs"
description: "Bu öğretici Azure Logic Apps ile posta listesi aboneliklerini işlemek için otomatik onay iş akışları oluşturmak nasıl gösterir"
author: ecfan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 01/12/2018
ms.author: LADocs; estfan
ms.openlocfilehash: 26ef6f69ef2f2d50628f4d0b021159526c9a04a7
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="manage-mailing-list-requests-with-a-logic-app"></a>Bir mantıksal uygulama ile posta listesi isteklerini yönet

Azure mantıksal uygulamaları, iş akışlarını otomatikleştirmek ve Azure Hizmetleri, Microsoft Hizmetleri, diğer hizmet olarak yazılım (SaaS) uygulamaları arasında veri tümleştirmenize yardımcı olur ve şirket içi sistemler. Bu öğretici nasıl oluşturabileceğinizi gösteren bir [mantıksal uygulama](../logic-apps/logic-apps-overview.md) tarafından yönetilen bir posta listesi abonelik isteklerini işler [MailChimp](https://mailchimp.com/) hizmet.
Bu mantıksal uygulama bir e-posta hesabı için bu istekleri izler, bu istekleri onaya gönderir ve onaylanan üyeler posta listesine ekler.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Boş bir mantıksal uygulama oluşturma.
> * Abonelik istekleri için e-posta izler bir tetikleyici ekleyin.
> * Onaylama veya reddetme bu istekleri için e-posta gönderen bir eylem ekleyin.
> * Onay yanıtı denetleyen bir koşul ekleyin.
> * Onaylanan üyeler posta listesine ekleyen bir eylem ekleyin.
> * Bu üyelerin listesi başarıyla katıldı olup olmadığını denetleyen bir koşul ekleyin.
> * Bu üyelerin listesi başarıyla katıldı olup olmadığını onaylama e-posta gönderen bir eylem ekleyin.

İşiniz bittiğinde, mantıksal uygulamanızı bu iş akışı yüksek bir düzeyde şuna benzer:

![Üst düzey tamamlanmış mantıksal uygulama](./media/tutorial-process-mailing-list-subscriptions-workflow/tutorial-overview.png)

Azure aboneliğiniz yoksa başlamadan önce <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>.

## <a name="prerequisites"></a>Önkoşullar

* Bir MailChimp hesabı. "Test-üyeleri-mantıksal uygulamanızı onaylanan üyeler için e-posta adresleri, ekleyebileceğiniz ML" adlı bir liste oluşturur. Bir hesabınız yoksa [ücretsiz bir hesap için kaydolun](https://login.mailchimp.com/signup/) ve öğrenin [listesini oluşturmak nasıl](https://us17.admin.mailchimp.com/lists/#). 

* Office 365 Outlook veya onay iş akışları destek Outlook.com, bir e-posta hesabı. Bu makalede, Office 365 Outlook kullanır. Farklı bir e-posta hesabı kullanıyorsanız, genel adımlar aynı kalır, ancak UI biraz farklı görünebilir.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Oturum <a href="https://portal.azure.com" target="_blank">Azure portal</a> Azure hesabı kimlik bilgilerinizle.

## <a name="create-your-logic-app"></a>Mantıksal uygulama oluşturma

1. Azure ana menüsünden **Yeni** > **Kurumsal Tümleştirme** > **Mantıksal Uygulama**'yı seçin.

   ![Mantıksal uygulama oluşturma](./media/tutorial-process-mailing-list-subscriptions-workflow/create-logic-app.png)

2. Altında **oluşturma mantıksal uygulama**, bu mantıksal uygulamanızı gösterilen ve tanımlandığı hakkında bilgi sağlar. İşiniz bittiğinde **Panoya sabitle** > **Oluştur**’u seçin.

   ![Mantıksal uygulama bilgileri sağlayın](./media/tutorial-process-mailing-list-subscriptions-workflow/create-logic-app-settings.png)

   | Ayar | Değer | Açıklama | 
   | ------- | ----- | ----------- | 
   | **Ad** | LA MailingList | Mantıksal uygulamanız için ad | 
   | **Abonelik** | <*Bilgisayarınızı-Azure-abonelik-adı*> | Azure aboneliğiniz için ad | 
   | **Kaynak grubu** | LA-MailingList-RG | İçin ad [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) ilgili kaynaklar düzenlemek için kullanılan | 
   | **Konum** | Doğu ABD 2 | Bölge mantıksal uygulamanızı hakkında bilgi depolanacağı konumu | 
   | **Log Analytics** | Kapalı | Tutmak **kapalı** tanılama günlük kaydını ayarlama. | 
   |||| 

3. Uygulamanızı Azure dağıtıldıktan sonra Logic Apps Tasarımcısı'nı açar ve video giriş ve ortak mantığı uygulama desenler için şablonlar içeren bir sayfa gösterir. **Şablonlar** bölümünde **Boş Mantıksal Uygulama**'yı seçin.

   ![Boş mantıksal uygulama şablonunu seçin](./media/tutorial-process-mailing-list-subscriptions-workflow/choose-logic-app-template.png)

Ardından, eklemek bir [tetikleyici](../logic-apps/logic-apps-overview.md#logic-app-concepts) abonelik isteklerle gelen e-postalar için dinler.
Her mantıksal uygulama ateşlenir belirli bir olay gerçekleştiğinde olur veya ne zaman yeni verileri belirli bir koşulunu tetikleyicisi ile başlamalıdır. Daha fazla bilgi için bkz: [ilk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="add-trigger-to-monitor-emails"></a>E-postaları izlemek için tetikleyici Ekle

1. "E-posta geldiğinde" designer'ı arama kutusuna girin. Tetikleyici için e-posta sağlayıcısı seçin:  **< *e-posta sağlayıcısı bilgisayarınızı*> - Yeni bir e-posta geldiğinde**
   
   ![E-posta sağlayıcısı için bu Tetikleyici seçin: "Yeni bir e-posta geldiğinde"](./media/tutorial-process-mailing-list-subscriptions-workflow/add-trigger-new-email.png)

   * Azure iş veya okul hesapları için Office 365 Outlook girişini seçin.
   * Kişisel Microsoft hesapları için Outlook.com girişini seçin.

2. Kimlik bilgilerini istendiyse, Logic Apps e-posta hesabınızı bağlantı oluşturabilmesi için e-posta hesabınızda oturum açın.

3. Şimdi tüm yeni e-posta tetikleyici denetler ölçütleri belirtin.

   1. Klasör, aralık ve e-postaları denetleme sıklığı belirtin.

      ![Klasör, aralık ve posta denetleme sıklığı belirtin.](./media/tutorial-process-mailing-list-subscriptions-workflow/add-trigger-set-up-email.png)

      | Ayar | Değer | Açıklama | 
      | ------- | ----- | ----------- | 
      | **Klasör** | Gelen kutusu | E-posta klasörünüze izlemek için | 
      | **Aralığı** | 1 | Denetimler bekleme aralıkların sayısı | 
      | **Sıklık** | Saat | Arasındaki her aralık için zaman birimi denetler  | 
      |  |  |  | 

   2. Seçin **Gelişmiş Seçenekleri Göster**. İçinde **konu filtre** kutusuna, bu e-posta konusunu bulmak tetikleyici için metin girin:```subscribe-test-members-ML```

      ![Gelişmiş seçenekleri ayarlama](./media/tutorial-process-mailing-list-subscriptions-workflow/add-trigger-set-advanced-options.png)

4. Şu an için tetikleyici ayrıntıları gizlemek için tetikleyici başlık çubuğunu tıklatın.

   ![Ayrıntıları gizlemek için Şekil Daralt](./media/tutorial-process-mailing-list-subscriptions-workflow/collapse-trigger-shape.png)

5. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet**'i seçin.

   Mantıksal uygulamanız artık canlı ancak gelen e-posta onay dışında her şey yapmaz. 
   Bu nedenle, tetikleyici başlatıldığında yanıt veren bir eylem ekleyin.

## <a name="send-approval-email"></a>Onay e-postası gönder

Bir tetikleyici sahip olduğunuza göre eklemek bir [eylem](../logic-apps/logic-apps-overview.md#logic-app-concepts) onaylamak veya isteği reddetmek için bir e-posta gönderir. 

1. Tetikleyici altında seçin **+ yeni adım** > **Eylem Ekle**. "Onay" araması yapın ve bu eylem seçin:  **< *e-posta sağlayıcısı bilgisayarınızı*>-onay e-posta Gönder**

   ![Seçin "< bilgisayarınızı-e-posta-sağlayıcısı > - onay e-posta Gönder"](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-send-approval-email.png)

2. Bu eylemin gösterilen ve açıklandığı gibi bilgileri sağlayın: 

   ![Onay e-posta ayarları ayarlayın](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-approval-email-settings.png)

   | Ayar | Değer | Açıklama | 
   | ------- | ----- | ----------- | 
   | **Alıcı** | <*Onaylayan e-posta adresi*> | Onaylayanın e-posta adresi. Test amacıyla, kendi adres kullanabilirsiniz. | 
   | **Kullanıcı seçenekleri** | Onayla, Reddet | Onaylayanın seçebilirsiniz yanıt seçenekleri. Varsayılan olarak, onaylayan seçebilir ya da "Onayla" veya "kendi yanıt olarak reddet". | 
   | **Konu** | Test üyeleri ML üye isteğini onayla | Açıklayıcı e-posta konusu | 
   |  |  |  | 

   Şimdilik, dinamik içerik listesi veya belirli düzenleme kutuları tıkladığınızda görüntülenen satır içi parametre listesi yoksay. 
   Bu liste, iş akışınızı girdi olarak kullanabilir önceki eylemlerine parametreleri seçmenizi sağlar. 
   Hangi listesi görüntülenir, tarayıcı genişliğini belirler. 
 
4. Mantıksal uygulamanızı kaydedin.

Ardından, onaylayanın seçilen yanıt kontrol etme koşulu ekleyin.

## <a name="check-approval-response"></a>Onay onay yanıtı

1. Altında **onay e-posta Gönder** eylemi seçin **+ yeni adım** > **bir koşul eklemek**.

   Koşul şekli iş akışınız için giriş olarak içerebilir herhangi bir kullanılabilir parametre birlikte görüntülenir. 

2. Koşulu daha iyi bir açıklama ile yeniden adlandırın.

   1. Koşulunun başlık çubuğunda seçin **üç nokta** (**...** ) düğmesini > **yeniden adlandırma**.

      Örneğin, tarayıcınız dar görünümünde ise:

      ![Koşulu yeniden adlandır](./media/tutorial-process-mailing-list-subscriptions-workflow/condition-rename.png)

      Tarayıcınızı uluslararası görünümünde ise ve dinamik içerik listesi erişimi için üç nokta düğmesini seçerek listeyi kapatın **dinamik içerik eklemek** koşul içinde.

   2. Bu açıklaması, durumu yeniden adlandırın:```If request approved```

3. Onaylayanın seçili olup olmadığını denetleyen bir koşul yapı **Onayla**:

   1. Koşul içinde içini tıklatın **bir değer seçin** sol (geniş Tarayıcı Görünümü) ya da üst (dar Tarayıcı Görünümü) kutusu.
   Parametre listesi veya dinamik içerik listesi seçin **SelectedOption** altında **onay e-posta Gönder**.

      Örneğin, geniş görünümünde çalışıyorsanız, koşulunuz aşağıdaki gibi görünür:

      !["Onay e-postası gönderme altında", "SelectedOption" seçin](./media/tutorial-process-mailing-list-subscriptions-workflow/build-condition-check-approval-response.png)

   2. Karşılaştırma işleci kutusunda, bu işleç seçin: **eşittir**

   3. Sağa (geniş görünümü) veya alt (dar görünümü) **bir değer seçin** kutusuna, bu değer girin:```Approve```

      İşiniz bittiğinde, koşulunuz aşağıdaki gibi görünür:

      ![Tam koşul](./media/tutorial-process-mailing-list-subscriptions-workflow/build-condition-check-approval-response-2.png)

4. Mantıksal uygulamanızı kaydedin.

Ardından, mantıksal uygulamanızı İnceleme isteği onayladığında gerçekleştiren eylemi belirtin. 

## <a name="add-member-to-mailchimp-list"></a>Üye MailChimp listesine ekleme

Şimdi, posta listenize onaylanan üye ekleyen bir eylem ekleyin.

1. Koşulunun içinde **true ise** dal, seçin **Eylem Ekle**.
"Mailchimp" için arama yapın ve bu eylem seçin: **MailChimp - üye listesine ekle**

   ![Seçin "MailChimp - listesine üye eklemek"](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-mailchimp-add-member.png)

3. MailChimp hesabınızda oturum açmak için sorulursa, MailChimp kimlik bilgilerinizle oturum açın.

4. Burada gösterilen ve açıklandığı gibi bu eylemle ilgili bilgi sağlar:

   !["Üye listesine ekle" için bilgileri sağlayın](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-mailchimp-add-member-settings.png)

   | Ayar | Değer | Açıklama | 
   | ------- | ----- | ----------- | 
   | **List Id** | test-members-ML | MailChimp posta listesi için ad | 
   | **Durum** | Abone | Yeni üye için abonelik durumu. Daha fazla bilgi için bkz: <a href="https://developer.mailchimp.com/documentation/mailchimp/guides/manage-subscribers-with-the-mailchimp-api/" target="_blank">aboneleri MailChimp API ile yönetme</a>. | 
   | **E-posta adresi** | <*new-member-email-address*> | Parametre listesi ya da dinamik içerik listesi, seçin **gelen** altında **yeni bir posta geldiğinde**, yeni üyesi için e-posta adresi geçirir. 
   |  |  |  | 

5. Mantıksal uygulamanızı kaydedin.

Ardından, yeni üye başarıyla posta listenizi katıldı denetleyebilmeniz bir koşulu ekleyin. Bu şekilde, mantıksal uygulamanızı bu işlem başarılı veya başarısız olduğunu bildirir.

## <a name="check-for-success-or-failure"></a>Başarı veya başarısızlık denetle

1. İçinde **true ise** altında dal **üye listesine Ekle** eylemi seçin **daha...**   >  **Bir koşul eklemek**.

2. Bu açıklama koşuluyla yeniden adlandırın:```If add member succeeded```

3. Onaylanan üye başarılı ya da posta listenizi katılma başarısız olup olmadığını denetleyen bir koşul oluşturun:

   1. Koşul içinde içini tıklatın **bir değer seçin** sol (geniş Tarayıcı Görünümü) ya da üst (dar Tarayıcı Görünümü) kutusu.
   Parametre listesi veya dinamik içerik listesi seçin **durum** altında **üye listesine Ekle**.

      Örneğin, geniş görünümünde çalışıyorsanız, koşulunuz aşağıdaki gibi görünür:

      ![Altında "üye listesine ekle", "Durum" seçin](./media/tutorial-process-mailing-list-subscriptions-workflow/build-condition-check-added-member.png)

   2. Karşılaştırma işleci kutusunda, bu işleç seçin: **eşittir**

   3. Sağa (geniş görünümü) veya alt (dar görünümü) **bir değer seçin** kutusuna, bu değer girin:```subscribed```

   İşiniz bittiğinde, koşulunuz aşağıdaki gibi görünür:

   ![Tamamlanan koşulu](./media/tutorial-process-mailing-list-subscriptions-workflow/build-condition-check-added-member-2.png)

Ardından, onaylanan üye başarılı veya posta listenizi katılma başarısız olduğunda göndermek için e-postaları ayarlayın.

## <a name="send-email-if-member-added"></a>Üye eklediyseniz e-posta Gönder

1. İçinde **true ise** koşul için şube **durumunda başarılı Üye Ekle**, seçin **Eylem Ekle**.

   ![İçindeki "true ise" şube koşulu için "Bir eylem Ekle" seçin](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-email-success.png)

2. "Outlook e-postası gönderme için" araması yapın ve bu eylem seçin:  **< *e-posta sağlayıcısı bilgisayarınızı*>-bir e-posta Gönder**

   !["Bir e-posta Gönder" için Eylem Ekle](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-email-success-2.png)

3. Bu açıklama eylemiyle yeniden adlandırın:```Send email on success```

4. Bu eylemin gösterilen ve açıklandığı gibi bilgileri sağlayın:

   ![Başarı e-posta için bilgileri sağlayın](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-email-success-settings.png)

   | Ayar | Değer | Açıklama | 
   | ------- | ----- | ----------- | 
   | **Alıcı** | <*e-posta adresi bilgisayarınızı*> | Where başarı e-posta göndermek e-posta adresi. Test için kendi e-posta adresinizi kullanabilirsiniz. | 
   | **Konu** | <*subject-for-success-email*> | Başarı e-posta konusu. Bu öğretici için bu metni girin ve belirtilen alanın altında seçin **üye listesine Ekle** parametre listesi ya da dinamik içerik listesi: <p>"Success! "Test-üyelerin-ML için' eklenen üye: **e-posta adresi**" | 
   | **Gövde** | <*Gövde için başarı eposta*> | Başarı e-posta gövdesi içeriği. Bu öğretici için bu metni girin ve altında belirtilen alanları seçin **üye listesine Ekle** parametre listesi ya da dinamik içerik listesi:  <p>"Yeni üye 'test-üyelerin-ML' katıldığı: **e-posta adresi**"</br>"Üye durumunu: **durum**" | 
   | | | | 

5. Mantıksal uygulamanızı kaydedin.

## <a name="send-email-if-member-not-added"></a>Üye eklenemiyor, e-posta Gönder

1. İçinde **false ise** koşul için şube **durumunda başarılı Üye Ekle**, seçin **Eylem Ekle**.

   ![İçindeki "false ise" şube koşulu için "Bir eylem Ekle" seçin](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-email-failed.png)

2. "Outlook e-postası gönderme için" araması yapın ve bu eylem seçin:  **< *e-posta sağlayıcısı bilgisayarınızı*>-bir e-posta Gönder**

   !["Bir e-posta Gönder" için Eylem Ekle](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-email-failed-2.png)

3. Bu açıklama eylemiyle yeniden adlandırın:```Send email on failure```

4. Burada gösterilen ve açıklandığı gibi bu eylemle ilgili bilgi sağlar:

   ![Başarısız bir e-posta için bilgileri sağlayın](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-email-failed-settings.png)

   | Ayar | Değer | Açıklama | 
   | ------- | ----- | ----------- | 
   | **Alıcı** | <*e-posta adresi bilgisayarınızı*> | Where hatası e-posta göndermek e-posta adresi. Test için kendi e-posta adresinizi kullanabilirsiniz. | 
   | **Konu** | <*konu için hata eposta*> | Hata e-posta konusu. Bu öğretici için bu metni girin ve belirtilen alanın altında seçin **üye listesine Ekle** parametre listesi ya da dinamik içerik listesi: <p>"Başarısız oldu, üye 'test-üyelerin-ML için' eklenmedi: **e-posta adresi**" | 
   | **Gövde** | <*Gövde için hata eposta*> | Hata e-posta gövdesi içeriği. Bu öğretici için bu metin girin: <p>"Üye zaten mevcut olabilir. MailChimp hesabınızı kontrol edin." | 
   | | | | 

5. Mantıksal uygulamanızı kaydedin. 

Ardından, artık bu örneğe benzer mantıksal uygulamanızı test edin:

 ![Tamamlanmış mantıksal uygulama](./media/tutorial-process-mailing-list-subscriptions-workflow/tutorial-complete.png)

## <a name="run-your-logic-app"></a>Mantıksal uygulamanızı çalıştırma

1. Kendiniz posta listenizi katılmak için bir e-posta isteği gönder.
İsteği gelen kutunuzda görünmesini bekleyin.

3. Mantıksal uygulamanızı el ile başlatmak için tasarımcı araç çubuğundan **Çalıştır**'ı seçin. 

   E-postanızı tetikleyici konu filtre ile eşleşen bir konu varsa, mantıksal uygulamanızı abonelik isteği onaylamak için e-posta gönderir.

4. Onay e-postada, seçin **Onayla**.

5. Abonenin e-posta adresi, posta listede yoksa, mantıksal uygulamanızı bu kişinin e-posta adresini ekler ve bu örnek gibi bir e-posta gönderir:

   ![Başarı e-posta](./media/tutorial-process-mailing-list-subscriptions-workflow/add-member-success.png)

   Mantıksal uygulamanızı abone eklenemiyor, bu örnek gibi bir e-posta alırsınız:

   ![Başarısız bir e-posta](./media/tutorial-process-mailing-list-subscriptions-workflow/add-member-failed.png)

   Tüm e-postaları alamazsanız, e-postanın Önemsiz klasörünü denetleyin. 
   E-posta Önemsiz filtre postalar bu tür yönlendirebilir. 
   Mantıksal uygulamanızın düzgün bir şekilde çalışıp çalışmadığından emin değilseniz bkz. [Mantıksal uygulama sorunlarını giderme](../logic-apps/logic-apps-diagnosing-failures.md).

Tebrikler, artık oluşturduğunuz ve bir mantıksal uygulama çalıştırmak, Azure, Microsoft Hizmetleri üzerinden bilgi ve diğer SaaS uygulamaları tümleştirir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olduğunda, mantıksal uygulama ve ilgili kaynakları içeren kaynak grubunu silin. Azure ana menüde Git **kaynak grupları**ve mantıksal uygulamanız için kaynak grubunu seçin. Seçin **kaynak grubu Sil**. Onay kaynak grubu adı girin ve seçin **silmek**.

!["Genel bakış" > "Kaynak grubu Sil"](./media/tutorial-process-mailing-list-subscriptions-workflow/delete-resource-group.png)

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](http://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, posta listesi istekleri için onayları yöneten bir mantıksal uygulama oluşturuldu. Şimdi, işler ve Azure Storage ve Azure işlevleri gibi Azure Hizmetleri ile tümleştirerek e-posta eklerini depolayan bir mantıksal uygulama oluşturmayı öğrenin.

> [!div class="nextstepaction"]
> [İşlem e-posta ekleri](../logic-apps/tutorial-process-email-attachments-workflow.md)