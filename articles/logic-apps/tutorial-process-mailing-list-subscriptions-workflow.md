---
title: Posta listesi isteklerini işlemek için onay iş akışları oluşturma - Azure Logic Apps | Microsoft Docs
description: Öğretici - Azure Logic Apps ile posta listesi aboneliklerini işlemek için otomatik onay iş akışları oluşturma
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: tutorial
ms.custom: mvc
ms.date: 01/12/2018
ms.openlocfilehash: b48ecce1c87c0a29996e437d621c3ce396a84856
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60503564"
---
# <a name="manage-mailing-list-requests-with-azure-logic-apps"></a>Azure Logic Apps ile posta listesi isteklerini yönetme

Azure Logic Apps, Azure hizmetleri, Microsoft hizmetleri ve diğer hizmet olarak yazılım (SaaS) uygulamalarının yanı sıra şirket içi sistemler üzerindeki verileri tümleştirmenize ve iş akışlarını otomatikleştirmenize yardımcı olur. Bu öğreticide, [MailChimp](https://mailchimp.com/) hizmeti tarafından yönetilen posta listesi için abonelik isteklerini işleyen [mantıksal uygulamayı](../logic-apps/logic-apps-overview.md) nasıl oluşturabileceğiniz gösterilir.
Bu mantıksal uygulama e-posta hesabını bu istekler için izler, bu istekleri onaya gönderir ve onaylanan üyeleri posta listesine ekler.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Boş bir mantıksal uygulama oluşturma.
> * E-postalardaki abonelik isteklerini izleyen bir tetikleyici ekleme.
> * Bu istekleri onaylamak veya reddetmek için e-posta gönderen bir eylem ekleme.
> * Onay yanıtını denetleyen bir koşul ekleme.
> * Onaylanan üyeleri posta listesine ekleyen bir eylem ekleme.
> * Bu üyelerin listeye başarıyla katılıp katılmadığını denetleyen bir koşul ekleme.
> * Bu üyelerin listeye başarıyla katılıp katılmadığına ilişkin onay e-postaları gönderen bir eylem ekleme.

İşlemi tamamladığınızda, mantıksal uygulamanız bu yüksek düzeyli iş akışı gibi görünür:

![Üst düzey tamamlanmış mantıksal uygulama](./media/tutorial-process-mailing-list-subscriptions-workflow/tutorial-overview.png)

Azure aboneliğiniz yoksa başlamadan önce <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>.

## <a name="prerequisites"></a>Önkoşullar

* MailChimp hesabı. Mantıksal uygulamanızın onaylanan üyelerin e-posta adreslerini ekleyebileceği "test-members-ML" adlı bir liste oluşturun. Hesabınız yoksa, [ücretsiz bir hesap için kaydolun](https://login.mailchimp.com/signup/) ve [liste oluşturmayı](https://us17.admin.mailchimp.com/lists/#) öğrenin. 

* Office 365 Outlook veya Outlook.com'dan onay iş akışlarını destekleyen bir e-posta hesabı. Bu makalede Office 365 Outlook kullanılır. Farklı bir e-posta hesabı kullanırsanız genel adımlar aynı kalır, ancak kullanıcı arabiriminiz biraz farklı görünebilir.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Azure hesabınızın kimlik bilgileriyle <a href="https://portal.azure.com" target="_blank">Azure portalında</a> oturum açın.

## <a name="create-your-logic-app"></a>Mantıksal uygulamanızı oluşturma

1. Azure ana menüsünden **Kaynak oluştur** > **Kurumsal Tümleştirme** > **Mantıksal Uygulama**'yı seçin.

   ![Mantıksal uygulama oluşturma](./media/tutorial-process-mailing-list-subscriptions-workflow/create-logic-app.png)

2. **Mantıksal uygulama oluştur** bölümünde, gösterildiği ve açıklandığı gibi mantıksal uygulamanızla ilgili bu bilgileri sağlayın. İşiniz bittiğinde **Panoya sabitle** > **Oluştur**’u seçin.

   ![Mantıksal uygulama bilgilerini sağlama](./media/tutorial-process-mailing-list-subscriptions-workflow/create-logic-app-settings.png)

   | Ayar | Değer | Açıklama | 
   | ------- | ----- | ----------- | 
   | **Ad** | LA-MailingList | Mantıksal uygulamanızın adı | 
   | **Abonelik** | <*your-Azure-subscription-name*> | Azure aboneliğinizin adı | 
   | **Kaynak grubu** | LA-MailingList-RG | İlgili kaynakların düzenlenmesi için kullanılan [Azure kaynak grubunun](../azure-resource-manager/resource-group-overview.md) adı | 
   | **Konum** | Doğu ABD 2 | Mantıksal uygulamanızla ilgili bilgilerin depolanacağı bölge | 
   | **Log Analytics** | Kapalı | Tanılama günlüğüne kaydetme ayarını **Kapalı** durumda bırakın. | 
   |||| 

3. Uygulamanız Azure tarafından dağıtıldıktan sonra Logic Apps Tasarımcısı açılır ve genel mantıksal uygulama desenleri için şablonları ve tanıtım videosunu içeren bir sayfayı gösterir. **Şablonlar** bölümünde **Boş Mantıksal Uygulama**'yı seçin.

   ![Boş mantıksal uygulama şablonunu seçin](./media/tutorial-process-mailing-list-subscriptions-workflow/choose-logic-app-template.png)

Sonra abonelik isteklerinin bulunduğu gelen e-postaları dinleyen bir [tetikleyici](../logic-apps/logic-apps-overview.md#logic-app-concepts) ekleyin.
Her mantıksal uygulama, belirli bir olay gerçekleştiğinde veya yeni veriler belirli bir koşulu karşıladığında tetiklenen bir tetikleyiciyle başlamalıdır. Daha fazla bilgi için bkz. [İlk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="add-trigger-to-monitor-emails"></a>E-postaları izlemek için tetikleyici ekleme

1. Tasarımcıda arama kutusuna "e-posta geldiğinde" yazın. E-posta sağlayıcınızın tetikleyicisini seçin: **<*eposta-sağlayıcınız*> - Yeni e-posta geldiğinde**
   
   ![E-posta sağlayıcısı için şu tetikleyiciyi seçin: "Yeni bir e-posta geldiğinde"](./media/tutorial-process-mailing-list-subscriptions-workflow/add-trigger-new-email.png)

   * Azure iş veya okul hesapları için Office 365 Outlook girişini seçin.
   * Kişisel Microsoft hesapları için Outlook.com girişini seçin.

2. Kimlik bilgileriniz istenirse, Logic Apps’in e-posta hesabınıza yönelik bir bağlantı oluşturabilmesi için e-posta hesabınızda oturum açın.

3. Şimdi tetikleyicinin tüm yeni e-postalarda denetleyeceği ölçütleri belirtin.

   1. E-postaları denetlemeye ilişkin klasörü, aralığı ve sıklığı belirtin.

      ![Postaları denetlemeye ilişkin klasörü, aralığı ve sıklığı belirtin](./media/tutorial-process-mailing-list-subscriptions-workflow/add-trigger-set-up-email.png)

      | Ayar | Değer | Açıklama | 
      | ------- | ----- | ----------- | 
      | **Klasör** | Gelen Kutusu | İzlenecek e-posta klasörü | 
      | **Aralık** | 1 | Denetimler arasında beklenecek aralık sayısı | 
      | **Sıklık** | Saat | Denetimler arası her aralık için zaman birimi  | 
      |  |  |  | 

   2. **Gelişmiş seçenekleri göster**'i seçin. **Konu Filtresi** kutusunda, tetikleyici için e-posta konusunda bulunacak bu metni girin: ```subscribe-test-members-ML```

      ![Gelişmiş seçenekleri ayarlama](./media/tutorial-process-mailing-list-subscriptions-workflow/add-trigger-set-advanced-options.png)

4. Tetikleyicinin ayrıntılarını şimdilik gizlemek için tetikleyicinin başlık çubuğuna tıklayın.

   ![Ayrıntıları gizlemek için şekli daraltın](./media/tutorial-process-mailing-list-subscriptions-workflow/collapse-trigger-shape.png)

5. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet**'i seçin.

   Mantıksal uygulamanız çalışıyor ancak gelen e-postanızı denetleme dışında bir işlem gerçekleştirmiyor. 
   Şimdi, tetikleyici etkinleştirildiğinde gerçekleştirilecek bir eylem ekleyin.

## <a name="send-approval-email"></a>Onay e-postası gönderme

Artık tetikleyiciniz olduğuna göre, isteği onaylamak veya reddetmek üzere e-posta gönderen bir [eylem](../logic-apps/logic-apps-overview.md#logic-app-concepts) ekleyin. 

1. Tetikleyicinin altında **+ Yeni adım** > **Eylem ekle**'yi seçin. "Onay" araması yapın ve şu eylemi seçin: **<*eposta-sağlayıcınız*> - Onay e-postası gönder**

   !["<eposta-sağlayıcınız> - Onay e-postası gönder" öğesini seçin](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-send-approval-email.png)

2. Bu eylem için gösterildiği ve açıklandığı gibi bilgi sağlayın: 

   ![Onay e-postası ayarlarını belirleme](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-approval-email-settings.png)

   | Ayar | Değer | Açıklama | 
   | ------- | ----- | ----------- | 
   | **Alıcı** | <*onay-eposta-adresi*> | Onaylayanın e-posta adresi. Test için kendi adresinizi kullanabilirsiniz. | 
   | **Kullanıcı Seçenekleri** | Onayla, Reddet | Onaylayanın seçebileceği yanıt seçenekleri. Varsayılan olarak, onaylayan "Onayla" veya "Reddet" yanıtını seçebilir. | 
   | **Konu** | test-members-ML için üye isteğini kabul etme | Açıklayıcı bir e-posta konusu | 
   |  |  |  | 

   Şimdilik, belirli düzenleme kutularının içine tıkladığınızda görüntülenen satır içi parametre listesini veya dinamik içerik listesini yoksayın. 
   Bu liste, iş akışınızda giriş olarak kullanabileceğiniz önceki eylemlerdeki parametreleri seçmenizi sağlar. 
   Hangi listenin görüneceği tarayıcınızın genişliğine bağlıdır. 
 
4. Mantıksal uygulamanızı kaydedin.

Ardından, onaylayanın seçtiği yanıtı denetlemek için bir koşul ekleyin.

## <a name="check-approval-response"></a>Onay yanıtını denetleme

1. **Onay e-postası gönder** eyleminin altında **+ Yeni adım** > **Koşul ekle**'yi seçin.

   Koşul şekli, iş akışınıza giriş olarak ekleyebileceğiniz tüm kullanılabilir parametrelerle birlikte gösterilir. 

2. Koşulu daha iyi bir açıklama ile yeniden adlandırın.

   1. Koşulun başlık çubuğunda **üç nokta** (**...**) düğmesini > **Yeniden Adlandır**’ı seçin.

      Örneğin, tarayıcınız dar görünümdeyse:

      ![Koşulu yeniden adlandırın](./media/tutorial-process-mailing-list-subscriptions-workflow/condition-rename.png)

      Tarayıcınız geniş görünümdeyse ve dinamik içerik, üç nokta düğmesine erişimi engelliyorsa koşulun içinden **Dinamik içerik ekle**’yi seçip listeyi kapatın.

   2. Koşulunuzu şu açıklama ile yeniden adlandırın: ```If request approved```

3. Onaylayanın **Onayla**'yı seçip seçmediğini denetleyen bir koşul oluşturun:

   1. Koşul içinde, soldaki (geniş tarayıcı görünümü) veya üstteki (dar tarayıcı görünümü) **Bir değer seçin** kutusunun içine tıklayın.
   Parametre listesinden veya dinamik içerik listesinden, **Onay e-postası gönder**'in altında **SelectedOption** alanını seçin.

      Örneğin, geniş görünümde çalışıyorsanız koşulunuz şu örnekteki gibi görünür:

      !["Onay e-postası gönder" öğesinin altında "SelectedOption" seçin](./media/tutorial-process-mailing-list-subscriptions-workflow/build-condition-check-approval-response.png)

   2. Karşılaştırma işleci kutusunda şu işleci seçin: **eşittir**

   3. Sağdaki (geniş görünüm) veya alttaki (dar görünüm) **Bir değer seçin** kutusuna şu değeri girin: ```Approve```

      İşlem tamamlandığında koşulunuz şu örnekteki gibi görünür:

      ![Tamamlanan koşul](./media/tutorial-process-mailing-list-subscriptions-workflow/build-condition-check-approval-response-2.png)

4. Mantıksal uygulamanızı kaydedin.

Ardından, gözden geçiren bir isteği onayladığında mantıksal uygulamanızın gerçekleştireceği eylemi belirtin. 

## <a name="add-member-to-mailchimp-list"></a>MailChimp listesine üye ekleme

Şimdi, onaylanan üyeyi posta listenize ekleyen bir eylem ekleyin.

1. Koşulun **True ise** dalı içinde **Eylem ekle**’yi seçin.
"Mailchimp" için arama yapın ve şu eylemi seçin: **MailChimp - listeye üye Ekle**

   !["MailChimp - Listeye üye ekle" öğesini seçin](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-mailchimp-add-member.png)

3. MailChimp hesabınızda oturum açmanız istenirse, MailChimp kimlik bilgilerinizle oturum açın.

4. Bu eylem için burada gösterildiği ve açıklandığı gibi bilgi sağlayın:

   !["Listeye üye ekle" için bilgileri sağlayın](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-mailchimp-add-member-settings.png)

   | Ayar | Değer | Açıklama | 
   | ------- | ----- | ----------- | 
   | **Liste Kimliği** | test-members-ML | MailChimp posta listenizin adı | 
   | **Durum** | abone olundu | Yeni üyenin abonelik durumu. Daha fazla bilgi için bkz. <a href="https://developer.mailchimp.com/documentation/mailchimp/guides/manage-subscribers-with-the-mailchimp-api/" target="_blank">MailChimp API'siyle aboneleri yönetme</a>. | 
   | **E-posta Adresi** | <*yeni-üye-eposta-adresi*> | Parametre listesinden veya dinamik içerik listesinden, **Yeni posta geldiğinde** bölümünde **Kimden**’i seçin. Bu, yeni üyenin e-posta adresinde verilir. 
   |  |  |  | 

5. Mantıksal uygulamanızı kaydedin.

Ardından, yeni üyenin posta listenize başarıyla katılıp katılmadığını denetlerken kullanabileceğiniz bir koşul ekleyin. Bu şekilde, mantıksal uygulamanız bu işlemin başarılı veya başarısız olduğunu size bildirir.

## <a name="check-for-success-or-failure"></a>Başarı veya başarısızlık durumunu denetleme

1. **True ise** dalında, **Listeye üye ekle** eyleminin altında **Diğer...** > **Koşul ekle**'yi seçin.

2. Koşulu şu açıklama ile yeniden adlandırın: ```If add member succeeded```

3. Onaylanan üyenin posta listenize katılımının başarılı mı yoksa başarısız mı olduğunu denetleyen bir koşul oluşturun:

   1. Koşul içinde, soldaki (geniş tarayıcı görünümü) veya üstteki (dar tarayıcı görünümü) **Bir değer seçin** kutusunun içine tıklayın.
   Parametre listesinden veya dinamik içerik listesinden, **Listeye üye ekle**'nin altında **Status** alanını seçin.

      Örneğin, geniş görünümde çalışıyorsanız koşulunuz şu örnekteki gibi görünür:

      !["Listeye üye ekle" öğesinin altında "Durum" seçin](./media/tutorial-process-mailing-list-subscriptions-workflow/build-condition-check-added-member.png)

   2. Karşılaştırma işleci kutusunda şu işleci seçin: **eşittir**

   3. Sağdaki (geniş görünüm) veya alttaki (dar görünüm) **Bir değer seçin** kutusuna şu değeri girin: ```subscribed```

   İşlem tamamlandığında koşulunuz şu örnekteki gibi görünür:

   ![Tamamlanmış koşul](./media/tutorial-process-mailing-list-subscriptions-workflow/build-condition-check-added-member-2.png)

Ardından, onaylanan üyenin posta listenize katılımının başarılı veya başarısız olması durumunda gönderilecek e-postaları ayarlayın.

## <a name="send-email-if-member-added"></a>Üye eklendiyse e-posta gönderme

1. **Üye ekleme başarılı olduysa** koşulunun **True ise** dalında **Eylem ekle**'yi seçin.

   ![Koşulun "True ise" dalında "Eylem ekle" öğesini seçin](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-email-success.png)

2. "Outlook e-posta gönderme" araması yapın ve şu eylemi seçin: **<*eposta-sağlayıcınız*> - E-posta gönder**

   !["E-posta gönder" eylemini ekleme](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-email-success-2.png)

3. Eylemi şu açıklama ile yeniden adlandırın: ```Send email on success```

4. Bu eylem için gösterildiği ve açıklandığı gibi bilgi sağlayın:

   ![Başarılı oldu e-postası için bilgi sağlama](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-email-success-settings.png)

   | Ayar | Değer | Açıklama | 
   | ------- | ----- | ----------- | 
   | **Alıcı** | <*eposta-adresiniz*> | Başarı e-postasının gönderileceği e-posta adresi. Test için kendi e-posta adresinizi kullanabilirsiniz. | 
   | **Konu** | <*başarı-epostası-konusu*> | Başarı e-postasının konusu. Bu öğretici için, şu metni girin ve parametre listesinden veya dinamik içerik listesinden **Listeye üye ekle** altında belirlenen alanı seçin: <p>"Başarılı oldu! Üye 'test-members-ML için' eklendi: **E-posta adresi**" | 
   | **Gövde** | <*başarı-e-postası-gövdesi*> | Başarı e-postasının gövde içeriği. Bu öğretici için, şu metni girin ve parametre listesinden veya dinamik içerik listesinden **Listeye üye ekle** altında belirlenen alanları seçin:  <p>"Yeni üye 'test-members-ML ' Listesine katıldı: **E-posta adresi**"</br>"Üye katılım durumu: **Durum**" | 
   | | | | 

5. Mantıksal uygulamanızı kaydedin.

## <a name="send-email-if-member-not-added"></a>Üye eklenmediyse e-posta gönderme

1. **Üye ekleme başarılı olduysa** koşulunun **False ise** dalında **Eylem ekle**'yi seçin.

   ![Koşulun "False ise" dalında "Eylem ekle" öğesini seçin](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-email-failed.png)

2. "Outlook e-posta gönderme" araması yapın ve şu eylemi seçin: **<*eposta-sağlayıcınız*> - E-posta gönder**

   !["E-posta gönder" eylemini ekleme](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-email-failed-2.png)

3. Eylemi şu açıklama ile yeniden adlandırın: ```Send email on failure```

4. Bu eylem için burada gösterildiği ve açıklandığı gibi bilgi sağlayın:

   ![Başarısız oldu e-postası için bilgi sağlama](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-email-failed-settings.png)

   | Ayar | Değer | Açıklama | 
   | ------- | ----- | ----------- | 
   | **Alıcı** | <*eposta-adresiniz*> | Başarısızlık e-postasının gönderileceği e-posta adresi. Test için kendi e-posta adresinizi kullanabilirsiniz. | 
   | **Konu** | <*başarısızlık-epostası-konusu*> | Başarısızlık e-postasının konusu. Bu öğretici için, şu metni girin ve parametre listesinden veya dinamik içerik listesinden **Listeye üye ekle** altında belirlenen alanı seçin: <p>"Başarısız oldu, üye 'test-members-ML için' eklenmedi: **E-posta adresi**" | 
   | **Gövde** | <*başarısızlık-epostası-gövdesi*> | Başarısızlık e-postasının gövde içeriği. Bu öğretici için şu metni girin: <p>"Üye zaten eklenmiş olabilir. MailChimp hesabınızı denetleyin." | 
   | | | | 

5. Mantıksal uygulamanızı kaydedin. 

Ardından mantıksal uygulamanızı test edin; mantıksal uygulamanız şu örnek gibi görünür:

 ![Tamamlanmış mantıksal uygulama](./media/tutorial-process-mailing-list-subscriptions-workflow/tutorial-complete.png)

## <a name="run-your-logic-app"></a>Mantıksal uygulamanızı çalıştırın

1. Posta listenize katılmak için kendinize bir e-posta isteği gönderin.
İsteğin gelen kutunuzda gösterilmesini bekleyin.

3. Mantıksal uygulamanızı el ile başlatmak için tasarımcı araç çubuğundan **Çalıştır**'ı seçin. 

   E-postanızın konusu tetikleyicinin konu filtresiyle eşleşiyorsa, mantıksal uygulamanız abonelik isteğini onaylamak için size bir e-posta gönderir.

4. Onay e-postasında **Onayla**'yı seçin.

5. Abonenin e-posta adresi posta listenizde yoksa, mantıksal uygulamanız bu kişinin e-posta adresini ekler ve size şu örnekteki gibi bir e-posta gönderir:

   ![Başarılı oldu e-postası](./media/tutorial-process-mailing-list-subscriptions-workflow/add-member-success.png)

   Mantıksal uygulamanız aboneyi ekleyemezse, şu örnekteki gibi bir e-posta alırsınız:

   ![Başarısız oldu e-postası](./media/tutorial-process-mailing-list-subscriptions-workflow/add-member-failed.png)

   E-posta gelmezse istenmeyen e-posta klasörüne bakın. 
   E-postanızın istenmeyen posta filtresi bu tür postaları yeniden yönlendirebilir. 
   Mantıksal uygulamanızın düzgün bir şekilde çalışıp çalışmadığından emin değilseniz bkz. [Mantıksal uygulama sorunlarını giderme](../logic-apps/logic-apps-diagnosing-failures.md).

Tebrikler, Azure, Microsoft hizmetleri ve diğer SaaS uygulamaları arasında bilgileri tümleştiren bir mantıksal uygulama oluşturdunuz ve çalıştırdınız.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerek kalmadığında mantıksal uygulamanızı ve ilgili kaynakları içeren kaynak grubunu silin. Ana Azure menüsünde **Kaynak grupları**’na gidin ve mantıksal uygulamanızın kaynak grubunu seçin. **Kaynak grubunu sil**'i seçin. Onay olarak kaynak grubunun adını girip **Sil**’i seçin.

!["Genel Bakış" > "Kaynak grubunu sil"](./media/tutorial-process-mailing-list-subscriptions-workflow/delete-resource-group.png)

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, posta listesi istekleri için onayları yöneten bir mantıksal uygulama oluşturdunuz. Şimdi, Azure Depolama ve Azure İşlevleri gibi Azure hizmetlerini tümleştirerek e-posta eklerini işleyen ve depolayan bir mantıksal uygulama oluşturmayı öğrenin.

> [!div class="nextstepaction"]
> [E-posta eklerini işleme](../logic-apps/tutorial-process-email-attachments-workflow.md)