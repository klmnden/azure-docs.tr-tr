---
title: "Zamanlayıcı tabanlı otomatik iş akışları - Azure mantıksal uygulamaları derleme | Microsoft Docs"
description: "Bu öğretici Azure Logic Apps ile Zamanlayıcı tabanlı, yinelenen, otomatik bir iş akışı oluşturmak nasıl gösterir"
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
ms.openlocfilehash: deb2572de363ca5d0dec0f78f2e30ad648e9b5f8
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="check-traffic-with-a-scheduler-based-logic-app"></a>Bir zamanlayıcı tabanlı mantıksal uygulama trafiğiyle denetleyin

Azure mantıksal uygulamaları bir zamanlamaya göre çalışan iş akışlarına otomatikleştirmenize yardımcı olur. Bu öğretici nasıl oluşturabileceğinizi gösteren bir [mantıksal uygulama](../logic-apps/logic-apps-overview.md) her gün sabah çalıştırılır ve trafiği dahil seyahat süresi denetler Zamanlayıcı tetikleyici ile iki arasında yerleştirir. Mantıksal uygulama zaman belirli bir sınırı aşarsa, seyahat saati ve ek saati içeren e-posta hedefi için gerekli olarak gönderir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Boş bir mantıksal uygulama oluşturma. 
> * Mantıksal uygulamanız için bir zamanlayıcı olarak çalışan bir tetikleyici ekleyin.
> * Bir rota için seyahat süresi alır bir eylem ekleyin.
> * Bir değişken oluşturur, seyahat saniyeden dakika saate dönüştürür ve sonucunda ortaya çıkan değişkenine kaydeder bir eylem ekleyin.
> * Belirli bir sınırı karşı seyahat süresi karşılaştıran bir koşul ekleyin.
> * Seyahat süresi sınırını aşarsa, e-posta gönderen bir eylem ekleyin.

İşiniz bittiğinde, mantıksal uygulamanızı bu iş akışı yüksek bir düzeyde şuna benzer:

![Üst düzey mantıksal uygulama](./media/tutorial-build-scheduled-recurring-logic-app-workflow/check-travel-time-overview.png)

Azure aboneliğiniz yoksa başlamadan önce <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>.

## <a name="prerequisites"></a>Önkoşullar

* Office 365 Outlook, Outlook.com veya Gmail gibi Logic Apps tarafından desteklenen bir e-posta Sağlayıcısı'ndan bir e-posta hesabı. Diğer sağlayıcılar için [bağlayıcılar listesi burada gözden](https://docs.microsoft.com/connectors/). Bu hızlı başlangıç Outlook.com hesabını kullanır. Farklı bir e-posta hesabı kullanıyorsanız, genel adımlar aynı kalır, ancak UI biraz farklı görünebilir.

* Bir rota için seyahat zamanı elde etmek için Bing haritaları API'si bir erişim anahtarı gerekir. Bu anahtarı almak için adımları izleyin <a href="https://msdn.microsoft.com/library/ff428642.aspx" target="_blank">Bing Haritalar anahtarının nasıl alındığını</a>. 

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Oturum <a href="https://portal.azure.com" target="_blank">Azure portal</a> Azure hesabı kimlik bilgilerinizle.

## <a name="create-your-logic-app"></a>Mantıksal uygulama oluşturma

1. Azure ana menüsünden **Yeni** > **Kurumsal Tümleştirme** > **Mantıksal Uygulama**'yı seçin.

   ![Mantıksal uygulama oluşturma](./media/tutorial-build-scheduled-recurring-logic-app-workflow/create-logic-app.png)

2. Altında **oluşturma mantıksal uygulama**, bu mantıksal uygulamanızı gösterilen ve tanımlandığı hakkında bilgi sağlar. İşiniz bittiğinde **Panoya sabitle** > **Oluştur**’u seçin.

   ![Mantıksal uygulama bilgileri sağlayın](./media/tutorial-build-scheduled-recurring-logic-app-workflow/create-logic-app-settings.png)

   | Ayar | Değer | Açıklama | 
   | ------- | ----- | ----------- | 
   | **Ad** | LA TravelTime | Mantıksal uygulamanız için ad | 
   | **Abonelik** | <*Bilgisayarınızı-Azure-abonelik-adı*> | Azure aboneliğiniz için ad | 
   | **Kaynak grubu** | LA TravelTime RG | İçin ad [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) ilgili kaynaklar düzenlemek için kullanılan | 
   | **Konum** | Doğu ABD 2 | Bölge mantıksal uygulamanızı hakkında bilgi depolanacağı konumu | 
   | **Log Analytics** | Kapalı | Tutmak **kapalı** tanılama günlük kaydını ayarlama. | 
   |||| 

3. Uygulamanızı Azure dağıtıldıktan sonra Logic Apps Tasarımcısı'nı açar ve video giriş ve ortak mantığı uygulama desenler için şablonlar içeren bir sayfa gösterir. **Şablonlar** bölümünde **Boş Mantıksal Uygulama**'yı seçin.

   ![Boş mantıksal uygulama şablonunu seçin](./media/tutorial-build-scheduled-recurring-logic-app-workflow/choose-logic-app-template.png)

Ardından, yineleme Ekle [tetikleyici](../logic-apps/logic-apps-overview.md#logic-app-concepts), belirtilen bir zamanlamaya göre hangi etkinleşir. Her mantıksal uygulama ateşlenir belirli bir olay gerçekleştiğinde olur veya ne zaman yeni verileri belirli bir koşulunu tetikleyicisi ile başlamalıdır. Daha fazla bilgi için bkz: [ilk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="add-scheduler-trigger"></a>Zamanlayıcı tetikleyicisi ekleyin

1. Designer'ı arama kutusuna "recurrence" girin. Bu tetikleyici seçin: **çizelgesi - yineleme**

   ![Bulun ve "Yinelemeyi Zamanla" tetikleyicisi ekleyin](./media/tutorial-build-scheduled-recurring-logic-app-workflow/add-schedule-recurrence-trigger.png)

2. Üzerinde **yineleme** şekil, seçin **üç nokta** (**...** ) düğmesine tıklayın ve seçin **yeniden adlandırma**. Bu açıklama tetikleyiciyle yeniden adlandırın:```Check travel time every weekday morning```

   ![Tetikleyici yeniden adlandırma](./media/tutorial-build-scheduled-recurring-logic-app-workflow/rename-recurrence-schedule-trigger.png)

3. Tetikleyici içinde seçin **Gelişmiş Seçenekleri Göster**.

4. Gösterilen ve açıklanan olarak tetikleyicinizin zamanlama ve yineleme bilgileri sağlayın:

   ![Zamanlama ve yineleme ayrıntılarını sağlayın](./media/tutorial-build-scheduled-recurring-logic-app-workflow/schedule-recurrence-trigger-settings.png)

   | Ayar | Değer | Açıklama | 
   | ------- | ----- | ----------- | 
   | **Aralığı** | 1 | Denetimler bekleme aralıkların sayısı | 
   | **Sıklık** | Hafta | Yineleme için kullanılacak zaman birimi | 
   | **Saat dilimi** | Hiçbiri | Yalnızca bir başlangıç saati belirttiğinizde uygulanır. Bir yerel saat dilimi belirtmek için kullanışlıdır. | 
   | **Başlangıç zamanı** | Hiçbiri | Belirli bir tarih ve saat kadar yineleme gecikmesi. Daha fazla bilgi için bkz: [zamanlama görevleri ve düzenli olarak çalışan iş akışlarına](../connectors/connectors-native-recurrence.md). | 
   | **Şu günlerde** | Pazartesi, Salı, Çarşamba, Perşembe, Cuma | Yalnızca **sıklığı** "Hafta" ayarlayın | 
   | **Bu saat** | 7,8,9 | Yalnızca **sıklığı** "Hafta" veya "Gün" olarak ayarlanmış. Bu yinelenme çalıştırmak için günün saati seçin. Bu örnek, 7, 8 ve 9 saatlik işaretleri çalışır. | 
   | **Bu dakika** | 0,15,30,45 | Yalnızca **sıklığı** "Hafta" veya "Gün" olarak ayarlanmış. Bu yinelenme çalıştırmak için günün dakika seçin. Bu örnek sıfır saatlik işaretinde başlangıç 15 dakikada bir çalışır. | 
   ||||

   Bu tetikleyici her bir iş günü, her 15 7: 00'da başlayıp 9: 45'te dakikada ateşlenir. 
   **Önizleme** kutusu yineleme zamanlamasını gösterir. 
   Daha fazla bilgi için bkz: [zamanlama görevleri ve iş akışları](../connectors/connectors-native-recurrence.md) ve [iş akışı eylemleri ve Tetikleyicileri](../logic-apps/logic-apps-workflow-actions-triggers.md#recurrence-trigger).

5. Şu an için tetikleyici ayrıntıları gizlemek için şeklin başlık çubuğu içinde'ı tıklatın.

   ![Ayrıntıları gizlemek için Şekil Daralt](./media/tutorial-build-scheduled-recurring-logic-app-workflow/collapse-trigger-shape.png)

6. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet**'i seçin. 

Mantıksal uygulamanız artık canlı ancak diğer Yinele hiçbir şey yapmaz. Bu nedenle, tetikleyici başlatıldığında yanıt veren bir eylem ekleyin.

## <a name="get-the-travel-time-for-a-route"></a>Bir rota için seyahat süresi Al

Bir tetikleyici sahip olduğunuza göre eklemek bir [eylem](../logic-apps/logic-apps-overview.md#logic-app-concepts) iki yerde arasında seyahat zaman alır. Logic Apps bağlayıcı için Bing haritaları API'si sunar, böylece bu bilgileri kolayca alabilirsiniz. Bu görev başlamadan önce bu öğreticinin önkoşullarını açıklandığı gibi bir Bing Haritalar API'si anahtarı olduğundan emin olun.

1. Tetikleyici altında mantığı Uygulama Tasarımcısı'nda seçin **+ yeni adım** > **Eylem Ekle**.

2. "Eşlemeleri" için arama yapın ve bu eylem seçin: **Bing Haritalar - Get yol**

3. Bing Haritalar bağlantı yoksa, bir bağlantı oluşturmak için sorulur. Bu bağlantı ayrıntıları sağlayın ve seçin **oluşturma**.

   !["Bing Haritalar - Get yol" seçin eylemi](./media/tutorial-build-scheduled-recurring-logic-app-workflow/create-maps-connection.png)

   | Ayar | Değer | Açıklama |
   | ------- | ----- | ----------- |
   | **Bağlantı Adı** | BingMapsConnection | Bağlantınız için bir ad sağlayın. | 
   | **API Anahtarı** | <*Bing-eşlemeleri-anahtarınız*> | Daha önce aldığınız Bing Haritalar anahtarını girin. Bing Haritalar anahtar yoksa, bilgi <a href="https://msdn.microsoft.com/library/ff428642.aspx" target="_blank">bir anahtar alma</a>. | 
   | | | |  

4. Bu açıklama eylemiyle yeniden adlandırın:```Get route and travel time with traffic```

5. Ayrıntılarını sağlamak **Get yol** gösterildiği gibi eylem ve burada, örneğin açıklanmıştır:

   !["Bing Haritalar - Get yol" için bilgileri sağlayın eylemi](./media/tutorial-build-scheduled-recurring-logic-app-workflow/get-route-action-settings.png) 

   | Ayar | Değer | Açıklama |
   | ------- | ----- | ----------- |
   | **Waypoint 1** | <*Başlangıç konumu*> | Rotanın kaynağı | 
   | **Waypoint 2** | <*Bitiş konumu*> | Rotanın hedef | 
   | **Avoid** | Hiçbiri | Otoyollar, tolls vb. gibi rota önlemek için tüm öğeler | 
   | **Optimize** | timeWithTraffic | Uzaklık gibi rota en iyi duruma getirmek için bir parametre seyahat zamanı geçerli trafiği ile ve benzeri. Bu parametreyi seçin: "timeWithTraffic" | 
   | **Uzaklık birimi** | <*Bilgisayarınızı tercihi*> | Uzaklık, rota için birimidir. Bu makalede bu birim kullanır: "Mil"  | 
   | **Seyahat modu** | Yürüten | Rota için seyahat modu. Bu mod seçin: "Yürüten" | 
   | **Transit tarih-saat** | None | Yalnızca aktarım modu için geçerlidir | 
   | **Geçiş türü tarih türü** | Hiçbiri | Yalnızca aktarım modu için geçerlidir | 
   |||| 

   Bu parametreler hakkında daha fazla bilgi için bkz: [bir rota hesaplamak](https://msdn.microsoft.com/library/ff701717.aspx).

6. Mantıksal uygulamanızı kaydedin.

Ardından, böylece dönüştürmek ve geçerli seyahat zamanı saniye yerine dakika olarak depolamak bir değişken oluşturun. Bu şekilde dönüştürme kaçınmanızı ve değer daha kolay daha sonraki adımlarda kullanın. 

## <a name="create-variable-to-store-travel-time"></a>Seyahat süresi depolamak için değişken oluşturma

Bazı durumlarda, veri akışında işlemleri ve daha sonra Eylemler sonuçları kullanmak isteyebilirsiniz. Bu sonuçları kolayca yeniden veya onları başvuru amacıyla kaydetmek için işledikten sonra bu sonuçlar depolamak için değişkenleri oluşturabilirsiniz. Yalnızca en üst düzeyde mantıksal uygulamanızı değişkenleri oluşturabilirsiniz.

Varsayılan olarak, önceki **Get yol** eylem saniye cinsinden trafiği ile geçerli seyahat saati döndürür **seyahat süresi trafiği** alan. Dönüştürme ve bunun yerine bu değer dakika depolamak, değer daha sonra yeniden dönüştürmeden yeniden kolaylaştırır.

1. Altında **Get yol** eylemi seçin **+ yeni adım** > **Eylem Ekle**.

2. "Değişkenleri" araması yapın ve bu eylem seçin: **değişkenleri - Initialize değişkeni**

   !["Değişkenleri - Initialize değişkeni" seçin eylemi](./media/tutorial-build-scheduled-recurring-logic-app-workflow/select-initialize-variable-action.png)

3. Bu eylem bu açıklama ile yeniden adlandırın:```Create variable to store travel time```

4. Ayrıntılar için değişken burada açıklandığı gibi sağlar:

   | Ayar | Değer | Açıklama | 
   | ------- | ----- | ----------- | 
   | **Ad** | travelTime | Değişken adı | 
   | **Tür** | Tamsayı | Değişken veri türü | 
   | **Değer** | Geçerli seyahat saniyeden (Bu tablonun altındaki adımları bakın) dakika saate dönüştürür bir ifade. | Değişkeniniz ilk değeri | 
   |||| 

   1. İfade için oluşturmak için **değeri** alan, dinamik içerik listesi görünmesi alanının içini tıklatın. 
   Listenin görünene kadar gerekirse, tarayıcınızı genişletir. 
   Dinamik içerik listede seçin **ifade**. 

      ![Bilgilerini "Değişkenleri - Initialize değişkeni" için eylem](./media/tutorial-build-scheduled-recurring-logic-app-workflow/initialize-variable-action-settings.png)

      Bazı düzenleme kutuları tıkladığınızda, dinamik içerik listesi veya bir satır içi parametre listesi görüntülenir. Bu liste, iş akışınızı girdi olarak kullanabilir önceki eylemlerine herhangi bir parametre gösterir. 
      Dinamik içerik listesi işlemleri gerçekleştirmek için işlevleri seçebileceğiniz bir ifade düzenleyici içerir. 
      Bu ifade Düzenleyicisi yalnızca dinamik içerik listesinde görüntülenir.

      Hangi listesi görüntülenir, tarayıcı genişliğini belirler. 
      Tarayıcınızı uluslararası ise, dinamik içerik listesi görüntülenir. 
      Tarayıcınız dar ise satır içi odağa sahip düzenleme kutusuna altında parametre listesi görüntülenir.

   2. İfade Düzenleyicisi'nde, bu deyim girin:```div(,60)```

      ![Bu ifade girin: "div(,60)"](./media/tutorial-build-scheduled-recurring-logic-app-workflow/initialize-variable-action-settings-2.png)

   3. İmlecinizi ifade sol parantez içine yerleştirin (**(**) ve virgül (**,**). 
   Seçin **dinamik içerik**.

      ![İmleç Konumlandır, "Dinamik içerik" seçin](./media/tutorial-build-scheduled-recurring-logic-app-workflow/initialize-variable-action-settings-3.png)

   4. Dinamik içerik listeden seçin **seyahat süresi trafiği**.

      !["Seyahat süresi trafiği" alanını seçin](./media/tutorial-build-scheduled-recurring-logic-app-workflow/initialize-variable-action-settings-4.png)

   5. İfadenin içine alan çözümler sonra tercih **Tamam**.

      !["Tamam" seçin](./media/tutorial-build-scheduled-recurring-logic-app-workflow/initialize-variable-action-settings-5.png)

      **Değeri** alan şimdi aşağıda gösterildiği gibi görünür:

      ![Çözümlenen ifade "Değeri" alanıyla](./media/tutorial-build-scheduled-recurring-logic-app-workflow/initialize-variable-action-settings-6.png)

5. Mantıksal uygulamanızı kaydedin.

Sonra geçerli seyahat süresi belirli bir sınırdan büyük olup olmadığını denetleyen bir koşul ekleyin.

## <a name="compare-travel-time-with-limit"></a>Seyahat zaman sınırı ile karşılaştırın

1. Önceki eylemi altında seçin **+ yeni adım** > **bir koşul eklemek**. 

2. Bu açıklama koşuluyla yeniden adlandırın:```If travel time exceeds limit```

3. Denetleyen bir koşul oluşturmak isteyip **travelTime** burada açıklanan ve gösterilen olarak belirtilen sınırınızı aşıyor:

   1. Koşul içinde içini tıklatın **bir değer seçin** sol (geniş Tarayıcı Görünümü) ya da üst (dar Tarayıcı Görünümü) kutusu.

   2. Dinamik içerik listesi veya parametre listesi seçin **travelTime** altında **değişkenleri**.

   3. Bu işleç karşılaştırma kutusunda seçin: **büyüktür:**

   4. İçinde **bir değer seçin** kutusunun sağındaki (geniş görünümü) veya alt (dar görünümü), bu sınırı girin:```15```

   Örneğin, dar görünümünde çalışıyorsanız, İşte bu durum nasıl oluşturur:

   ![Koşul dar görünümünde oluştur](./media/tutorial-build-scheduled-recurring-logic-app-workflow/build-condition-check-travel-time-narrow.png)

4. Mantıksal uygulamanızı kaydedin.

Ardından, seyahat süre sınırınızı aştığında gerçekleştirilecek eylem ekleyin.

## <a name="send-email-when-limit-exceeded"></a>Sınırı aşıldığında e-posta Gönder

Şimdi, seyahat zamanı sınırınızı aşarsa, e-postalar bir eylem ekleyin. Bu e-posta geçerli seyahat saat ile belirtilen yol için gereken ek süre içerir. 

1. Koşulunun içinde **true ise** dal, seçin **Eylem Ekle**.

2. "E-postası gönderme için" araması yapın ve e-posta Bağlayıcısı'nı ve "e-posta eylemi Gönder" öğesini seçin, kullanmak istediğiniz.

   ![Bulun ve "e-posta Gönder" eylemini seçin](./media/tutorial-build-scheduled-recurring-logic-app-workflow/add-action-send-email.png)

   * Kişisel Microsoft hesaplarını seçin **Outlook.com**. 
   * Azure için iş veya Okul hesapları, seçin **Office 365 Outlook**.

3. Bir bağlantı zaten sahip değilseniz, e-posta hesabınızda oturum açmayı istenir.

   Logic Apps, e-posta hesabınıza bir bağlantı oluşturur.

4. Bu açıklama eylemiyle yeniden adlandırın:```Send email with travel time```

5. **Alıcı** kutusuna alıcının e-posta adresini girin. Test amacıyla, e-posta adresinizi kullanın.

6. İçinde **konu** kutusuna e-postanın konusunu belirtin ve dahil **travelTime** değişkeni.

   1. Metin girin ```Current travel time (minutes): ``` bir boşluk ile. 
   
   2. Parametre listesi veya dinamik içerik listesi seçin **travelTime** altında **değişkenleri**. 
   
      Örneğin, tarayıcınız dar görünümünde ise:

      ![Konu metni ve seyahat süresi döndüren ifadesini girin](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-subject-settings.png)

7. İçinde **gövde** kutusunda, e-posta gövdesi içeriğini belirtin. 

   1. Metin girin ```Add extra travel time (minutes): ``` bir boşluk ile. 
   
   2. Dinamik içerik listesi görünene kadar gerekirse, tarayıcınızı genişletir. 
   Dinamik içerik listede seçin **ifade**.

      ![E-posta gövdesi için ifadeyi oluşturun](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-body-settings.png)

   3. Böylece, sınırı aşan dakika sayısı hesaplayabilirsiniz ifade Düzenleyicisi'nde, bu deyim girin:```sub(,15)```

      ![Ek dakika seyahat zamanı hesaplamak için ifadesi girin](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-body-settings-2.png)

   4. İmlecinizi ifade sol parantez içine yerleştirin (**(**) ve virgül (**,**). Seçin **dinamik içerik**.

      ![Ek dakika seyahat zamanı hesaplamak için ifade oluşturmaya devam etmek](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-body-settings-3.png)

   5. Altında **değişkenleri**seçin **travelTime**.

      ![İfadede kullanmak için "travelTime" alanını seçin](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-body-settings-4.png)

   6. İfadenin içine alan çözümler sonra tercih **Tamam**.

      ![Çözümlenen ifade "Body" alanıyla](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-body-settings-5.png)

      **Gövde** alan şimdi aşağıda gösterildiği gibi görünür:

      ![Çözümlenen ifade "Body" alanıyla](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-body-settings-6.png)

8. Mantıksal uygulamanızı kaydedin.

Ardından, artık bu örneğe benzer mantıksal uygulamanızı test edin:

![Tamamlanmış mantıksal uygulama](./media/tutorial-build-scheduled-recurring-logic-app-workflow/check-travel-time-finished.png)

## <a name="run-your-logic-app"></a>Mantıksal uygulamanızı çalıştırma

Mantıksal uygulamanızı el ile başlatmak için tasarımcı araç çubuğundan **Çalıştır**'ı seçin. Geçerli saat kalır sınırınızı altında seyahat ediyorsanız, mantıksal uygulamanızı başka hiçbir şey yapmaz ve yeniden denetlemeden önce sonraki aralığı bekler.
Ancak geçerli seyahat süresi sınırınızı aşıyor, sınırınızı geçerli seyahat saat ve dakika sayısını içeren bir e-posta alırsınız. Mantıksal uygulamanızı gönderir bir örnek e-posta şöyledir:

![Seyahat süresi ile gönderilen e-posta](./media/tutorial-build-scheduled-recurring-logic-app-workflow/email-notification.png)

Tüm e-postaları alamazsanız, e-postanın Önemsiz klasörünü denetleyin. E-posta Önemsiz filtre postalar bu tür yönlendirebilir. Mantıksal uygulamanızın düzgün bir şekilde çalışıp çalışmadığından emin değilseniz bkz. [Mantıksal uygulama sorunlarını giderme](../logic-apps/logic-apps-diagnosing-failures.md).

Tebrikler, artık oluşturduğunuz ve zamanlama tabanlı bir yinelenen mantıksal uygulama çalıştırın. 

Kullanan diğer mantığı uygulamalar oluşturmak için **çizelgesi - yinelenme** tetikleyin, sonra kullanılabilir bir mantıksal uygulama oluşturma bu şablonları denetleyin:

* Size gönderilen günlük anımsatıcıları alın.
* Azure BLOB'ları eski silin.
* Bir Azure Storage kuyruğuna bir ileti ekleyin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olduğunda, mantıksal uygulama ve ilgili kaynakları içeren kaynak grubunu silin. Azure ana menüde Git **kaynak grupları**ve mantıksal uygulamanız için kaynak grubunu seçin. Seçin **kaynak grubu Sil**. Onay kaynak grubu adı girin ve seçin **silmek**.

!["Genel bakış" > "Kaynak grubu Sil"](./media/tutorial-build-scheduled-recurring-logic-app-workflow/delete-resource-group.png)

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](http://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, belirtilen bir zamanlamayla (hafta içi günü mornings) göre trafiği denetleyen bir mantıksal uygulama oluşturulur ve sürer eylemi (gönderdiği e-posta) ne zaman seyahat süresi belirli bir sınırı aşıyor. Şimdi, Azure Hizmetleri, Microsoft Hizmetleri ve diğer SaaS uygulamaları ile tümleştirerek posta listesi istekleri onaya gönderir bir mantıksal uygulama oluşturmayı öğrenin.

> [!div class="nextstepaction"]
> [Posta listesi isteklerini yönet](../logic-apps/tutorial-process-mailing-list-subscriptions-workflow.md)