---
title: Zamanlama tabanlı otomatik iş akışları oluşturma - Azure Logic Apps | Microsoft Docs
description: Öğretici - Azure Logic Apps ile zamanlayıcı tabanlı, yinelenen, otomatik bir iş akışı oluşturma
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: tutorial
ms.custom: mvc
ms.date: 01/12/2018
ms.openlocfilehash: ebc6388f1ebc7546ffda07095ead50797bde4e8b
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58884695"
---
# <a name="check-traffic-on-a-schedule-with-azure-logic-apps"></a>Azure Logic Apps ile zamanlamaya göre trafik denetleme

Azure Logic Apps, bir zamanlamaya göre çalışan iş akışlarını otomatikleştirmenize yardımcı olur. Bu öğretici, hafta içi her sabah çalıştırılan ve trafik de dahil, iki yer arasındaki seyahat süresini denetleyen bir zamanlayıcı tetikleyicisi ile nasıl bir [mantıksal uygulama](../logic-apps/logic-apps-overview.md) oluşturabileceğinizi gösterir. Zaman belirli bir sınırı aşarsa mantıksal uygulama, hedefiniz için seyahat süresini ve gerekli ek süreyi içeren bir e-posta gönderir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Boş bir mantıksal uygulama oluşturma. 
> * Mantıksal uygulamanız için zamanlayıcı olarak çalışan bir tetikleyici ekleme.
> * Bir rota için seyahat süresini alan bir eylem ekleme.
> * Bir değişken oluşturan, seyahat süresini saniyelerden dakikalara dönüştüren ve bu sonucu değişkene kaydeden bir eylem ekleme.
> * Seyahat süresini belirtilen bir sınırla karşılaştıran bir koşul ekleme.
> * Seyahat süresi sınırını aşarsa e-posta gönderen bir eylem ekleme.

İşlemi tamamladığınızda, mantıksal uygulamanız bu yüksek düzeyli iş akışı gibi görünür:

![Üst düzey mantıksal uygulama](./media/tutorial-build-scheduled-recurring-logic-app-workflow/check-travel-time-overview.png)

Azure aboneliğiniz yoksa başlamadan önce <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>.

## <a name="prerequisites"></a>Önkoşullar

* Logic Apps tarafından desteklenen Office 365 Outlook, Outlook.com veya Gmail gibi bir e-posta sağlayıcıdan alınmış e-posta hesabı. Diğer sağlayıcılar için [buradaki bağlayıcı listesini inceleyin](https://docs.microsoft.com/connectors/). Bu hızlı başlangıç bir Outlook.com hesabını kullanır. Farklı bir e-posta hesabı kullanırsanız genel adımlar aynı kalır, ancak kullanıcı arabiriminiz biraz farklı görünebilir.

* Bir rotaya ilişkin seyahat süresini almak için, Bing Haritalar API’sinin erişim anahtarı gerekir. Bu anahtarı almak için <a href="https://msdn.microsoft.com/library/ff428642.aspx" target="_blank">Bing Haritalar anahtarını alma</a> adımlarını izleyin. 

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Azure hesabınızın kimlik bilgileriyle <a href="https://portal.azure.com" target="_blank">Azure portalında</a> oturum açın.

## <a name="create-your-logic-app"></a>Mantıksal uygulamanızı oluşturma

1. Azure ana menüsünden **Kaynak oluştur** > **Kurumsal Tümleştirme** > **Mantıksal Uygulama**'yı seçin.

   ![Mantıksal uygulama oluşturma](./media/tutorial-build-scheduled-recurring-logic-app-workflow/create-logic-app.png)

2. **Mantıksal uygulama oluştur** bölümünde, gösterildiği ve açıklandığı gibi mantıksal uygulamanızla ilgili bu bilgileri sağlayın. İşiniz bittiğinde **Panoya sabitle** > **Oluştur**’u seçin.

   ![Mantıksal uygulama bilgilerini sağlama](./media/tutorial-build-scheduled-recurring-logic-app-workflow/create-logic-app-settings.png)

   | Ayar | Değer | Açıklama | 
   | ------- | ----- | ----------- | 
   | **Ad** | LA-TravelTime | Mantıksal uygulamanızın adı | 
   | **Abonelik** | <*your-Azure-subscription-name*> | Azure aboneliğinizin adı | 
   | **Kaynak grubu** | LA-TravelTime-RG | İlgili kaynakların düzenlenmesi için kullanılan [Azure kaynak grubunun](../azure-resource-manager/resource-group-overview.md) adı | 
   | **Konum** | Doğu ABD 2 | Mantıksal uygulamanızla ilgili bilgilerin depolanacağı bölge | 
   | **Log Analytics** | Kapalı | Tanılama günlüğüne kaydetme ayarını **Kapalı** durumda bırakın. | 
   |||| 

3. Uygulamanız Azure tarafından dağıtıldıktan sonra Logic Apps Tasarımcısı açılır ve genel mantıksal uygulama desenleri için şablonları ve tanıtım videosunu içeren bir sayfayı gösterir. **Şablonlar** bölümünde **Boş Mantıksal Uygulama**'yı seçin.

   ![Boş mantıksal uygulama şablonunu seçin](./media/tutorial-build-scheduled-recurring-logic-app-workflow/choose-logic-app-template.png)

Ardından, belirtilen bir zamanlamaya göre tetiklenen yinelenme [tetikleyicisini](../logic-apps/logic-apps-overview.md#logic-app-concepts) ekleyin. Her mantıksal uygulama, belirli bir olay gerçekleştiğinde veya yeni veriler belirli bir koşulu karşıladığında tetiklenen bir tetikleyiciyle başlamalıdır. Daha fazla bilgi için bkz. [İlk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="add-scheduler-trigger"></a>Zamanlayıcı tetikleyicisi ekleme

1. Tasarımcıda arama kutusuna "yinelenme" yazın. Şu tetikleyiciyi seçin: **Zamanlama - yinelenme**

   !["Zamanlama - Yinelenme" tetikleyicisini bulup ekleyin](./media/tutorial-build-scheduled-recurring-logic-app-workflow/add-schedule-recurrence-trigger.png)

2. **Yinelenme** şeklinin üzerindeki **üç nokta** (**...**) düğmesini ve ardından **Yeniden Adlandır**’ı seçin. Tetikleyiciyi şu açıklama ile yeniden adlandırın:
```Check travel time every weekday morning```

   ![Tetikleyiciyi yeniden adlandırma](./media/tutorial-build-scheduled-recurring-logic-app-workflow/rename-recurrence-schedule-trigger.png)

3. Tetikleyicinin içinde **Gelişmiş seçenekleri göster**’i seçin.

4. Gösterildiği ve açıklandığı şekilde tetikleyiciniz için zamanlama ve yinelenme ayrıntılarını sağlayın:

   ![Zamanlama ve yinelenme ayrıntılarını sağlayın](./media/tutorial-build-scheduled-recurring-logic-app-workflow/schedule-recurrence-trigger-settings.png)

   | Ayar | Değer | Açıklama | 
   | ------- | ----- | ----------- | 
   | **Interval** | 1 | Denetimler arasında beklenecek aralık sayısı | 
   | **Sıklık** | Hafta | Yinelenme için kullanılacak zaman birimi | 
   | **Saat dilimi** | None | Yalnızca bir başlangıç zamanı belirttiğinizde geçerlidir. Yerel olmayan bir saat dilimi belirtmek için kullanışlıdır. | 
   | **Başlangıç saati** | None | Yinelenme, belirli bir tarih ve saate kadar ertelenir. Daha fazla bilgi için bkz. [Düzenli olarak çalıştırılan görevler ve iş akışları zamanlama](../connectors/connectors-native-recurrence.md). | 
   | **Şu günlerde** | Pazartesi,Salı,Çarşamba,Perşembe,Cuma | Yalnızca **Sıklık** "Hafta" olarak ayarlandığında kullanılabilir | 
   | **Şu saatlerde** | 7,8,9 | Yalnızca **Sıklık** "Hafta" veya "Gün" olarak ayarlandığında kullanılabilir. Bu yinelenmenin çalıştırılacağı günün saatlerini seçin. Bu örnek, 7, 8 ve 9 saat işaretlerinde çalıştırılır. | 
   | **Şu dakikalarda** | 0,15,30,45 | Yalnızca **Sıklık** "Hafta" veya "Gün" olarak ayarlandığında kullanılabilir. Bu yinelenmenin çalıştırılacağı günün dakikalarını seçin. Bu örnek, sıfır saat işaretinden başlayarak her 15 dakikada bir çalıştırılır. | 
   ||||

   Bu tetikleyici hafta içi her gün 7:00’da başlayıp 9:45’e kadar her 15 dakikada bir tetiklenir. 
   **Önizleme** kutusu, yinelenme zamanlamasını gösterir. 
   Daha fazla bilgi için bkz. [Görevleri ve iş akışlarını zamanlama](../connectors/connectors-native-recurrence.md) ve [İş akışı eylemleri ve tetikleyicileri](../logic-apps/logic-apps-workflow-actions-triggers.md#recurrence-trigger).

5. Tetikleyicinin ayrıntılarını şimdilik gizlemek için şeklin başlık çubuğunun içine tıklayın.

   ![Ayrıntıları gizlemek için şekli daraltın](./media/tutorial-build-scheduled-recurring-logic-app-workflow/collapse-trigger-shape.png)

6. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet**'i seçin. 

Şimdi mantıksal uygulamanız çalışıyor ancak yinelenme dışında bir işlem gerçekleştirmiyordur. Şimdi, tetikleyici etkinleştirildiğinde gerçekleştirilecek bir eylem ekleyin.

## <a name="get-the-travel-time-for-a-route"></a>Bir rota için seyahat süresi alma

Şimdi bir tetikleyiciniz olduğuna göre artık iki yer arasındaki seyahat süresini alan bir [eylem](../logic-apps/logic-apps-overview.md#logic-app-concepts) ekleyebilirsiniz. Logic Apps, bu bilgileri kolayca alabilmeniz için Bing Haritalar API’si için bir bağlayıcı sağlar. Bu göreve başlamadan önce, bu öğreticinin önkoşullarında açıklandığı şekilde Bing Haritalar API anahtarına sahip olduğunuzdan emin olun.

1. Logic Apps Tasarımcısı’nda tetikleyicinizin altında **+ Yeni adım** > **Eylem ekle**’yi seçin.

2. "Haritalar" için arama yapın ve şu eylemi seçin: **Bing Haritalar - rota Al**

3. Bing Haritalar bağlantınız yoksa bir bağlantı oluşturmanız istenir. Bu bağlantı ayrıntılarını sağlayın ve **Oluştur**’u seçin.

   !["Bing Haritalar - Rota al" eylemini seçin](./media/tutorial-build-scheduled-recurring-logic-app-workflow/create-maps-connection.png)

   | Ayar | Değer | Açıklama |
   | ------- | ----- | ----------- |
   | **Bağlantı Adı** | BingMapsConnection | Bağlantınıza bir ad verin. | 
   | **API Anahtarı** | <*your-Bing-Maps-key*> | Daha önce aldığınız Bing Haritalar anahtarını girin. Bing Haritalar anahtarınız yoksa <a href="https://msdn.microsoft.com/library/ff428642.aspx" target="_blank">nasıl anahtar alacağınızı</a> öğrenin. | 
   | | | |  

4. Eylemi şu açıklama ile yeniden adlandırın:
```Get route and travel time with traffic```

5. Burada gösterildiği ve açıklandığı gibi **Rota al** eylemi için ayrıntıları sağlayın; örneğin:

   !["Bing Haritalar - Rota al" eylemi için bilgileri sağlama](./media/tutorial-build-scheduled-recurring-logic-app-workflow/get-route-action-settings.png) 

   | Ayar | Değer | Açıklama |
   | ------- | ----- | ----------- |
   | **Güzergah noktası 1** | <*start-location*> | Rotanızın başlangıç noktası | 
   | **Güzergah noktası 2** | <*end-location*> | Rotanızın hedefi | 
   | **Kaçının** | None | Otoyollar, ücretli geçişler vb. gibi rotanızda kaçınılacak öğeler | 
   | **İyileştirme** | timeWithTraffic | Rotanızı iyileştirmeye yönelik bir parametre; örneğin, mesafe, mevcut trafik ile seyahat süresi vb. Şu parametreyi seçin: "timeWithTraffic" | 
   | **Mesafe birimi** | <*your-preference*> | Rotanız için mesafe birimi. Bu makalede şu birim kullanılmaktadır: "Mil"  | 
   | **Seyahat modu** | Sürüş | Rotanız için seyahat modu. Bu mod seçin: "Sürüş" | 
   | **Geçiş tarihi-saati** | None | Yalnızca toplu ulaşım modu için geçerlidir | 
   | **Tarih-saat türü** | None | Yalnızca toplu ulaşım modu için geçerlidir | 
   |||| 

   Bu parametreler hakkında daha fazla bilgi için bkz. [Rota hesaplama](https://msdn.microsoft.com/library/ff701717.aspx).

6. Mantıksal uygulamanızı kaydedin.

Ardından, geçerli seyahat süresini saniyeler olarak değil, dakikalar olarak dönüştürebilmeniz ve depolayabilmeniz için bir değişken oluşturun. Böylece dönüştürmeyi yinelemekten kaçınabilir ve sonraki adımlarda değeri daha kolayca kullanabilirsiniz. 

## <a name="create-variable-to-store-travel-time"></a>Seyahat süresini depolamak için değişken oluşturma

Bazen iş akışınızdaki veriler üzerinde işlemler gerçekleştirmek ve sonraki eylemlerde bu sonuçları kullanmak isteyebilirsiniz. Kolayca bu sonuçları yeniden kullanabilmek veya bu sonuçlara başvurabilmek amacıyla sonuçları kaydetmek için, sonuçları işledikten sonra depolamak üzere değişkenler oluşturabilirsiniz. Yalnızca mantıksal uygulamanızın en üst düzeyinde değişkenler oluşturabilirsiniz.

Varsayılan olarak, önceki **Rota al** eylemi, **Seyahat Süresi Trafik** alanı aracılığıyla trafik ile birlikte saniye cinsinden geçerli seyahat süresini getirir. Bunun yerine bu değeri dönüştürüp depolayarak daha sonra yeniden dönüştürme olmadan değerin yeniden kullanımını kolaylaştırırsınız.

1. **Rota al** eyleminin altında **+ Yeni adım** > **Eylem ekle** seçeneğini belirleyin.

2. "Değişkenler" için arama yapın ve şu eylemi seçin: **Değişkenler - değişken Başlat**

   !["Değişkenler - Değişken başlat" eylemini seçin](./media/tutorial-build-scheduled-recurring-logic-app-workflow/select-initialize-variable-action.png)

3. Bu eylemi şu açıklama ile yeniden adlandırın:
```Create variable to store travel time```

4. Burada açıklandığı gibi değişkeniniz için ayrıntıları sağlayın:

   | Ayar | Değer | Açıklama | 
   | ------- | ----- | ----------- | 
   | **Ad** | travelTime | Değişkeninizin adı | 
   | **Type** | Tamsayı | Değişkeninizin veri türü | 
   | **Değer** | Geçerli seyahat süresini saniyelerden dakikalara dönüştüren bir ifade (bu tablonun altındaki adımlara bakın). | Değişkeninizin ilk değeri | 
   |||| 

   1. **Değer** alanına ilişkin ifade oluşturmak için, alanın içine tıklayarak dinamik içerik listesini görüntüleyin. 
   Gerekirse liste görüntüleninceye kadar tarayıcınızı genişletin. 
   Dinamik içerik listesinde **İfade**’yi seçin. 

      !["Değişkenler - Değişken başlat" eylemi için bilgileri sağlayın](./media/tutorial-build-scheduled-recurring-logic-app-workflow/initialize-variable-action-settings.png)

      Bazı düzenleme kutularının içine tıkladığınızda, dinamik içerik listesi veya satır içi parametre listesi görüntülenir. Bu liste, iş akışınızda giriş olarak kullanabileceğiniz önceki eylemlerdeki parametreleri gösterir. 
      Dinamik içerik listesi, işlemleri gerçekleştirmek için işlevleri seçebileceğiniz bir ifade düzenleyicisi içerir. 
      Bu ifade düzenleyicisi yalnızca dinamik içerik listesinde görüntülenir.

      Hangi listenin görüneceği tarayıcınızın genişliğine bağlıdır. 
      Tarayıcınız genişse dinamik içerik listesi görüntülenir. 
      Tarayıcınız darsa, bir parametre listesi o anda odağı içeren düzenleme kutusunun altında satır içi görüntülenir.

   2. İfade düzenleyicisinde şu ifadeyi girin: ```div(,60)```

      ![Şu ifadeyi girin: "div(,60)"](./media/tutorial-build-scheduled-recurring-logic-app-workflow/initialize-variable-action-settings-2.png)

   3. İmlecinizi, sol parantez (**(**) ile virgül (**,**) arasındaki ifadeye getirin. 
   **Dinamik içerik** seçeneğini belirleyin.

      ![İmleci konumlandırın, "Dinamik içerik" seçeneğini belirleyin](./media/tutorial-build-scheduled-recurring-logic-app-workflow/initialize-variable-action-settings-3.png)

   4. Dinamik içerik listesinden **Seyahat Süresi Trafik** seçeneğini belirleyin.

      !["Seyahat Süresi Trafik" alanını seçin](./media/tutorial-build-scheduled-recurring-logic-app-workflow/initialize-variable-action-settings-4.png)

   5. Alan ifadenin içinde çözümlendikten sonra **Tamam**’ı seçin.

      !["Tamam"ı seçin](./media/tutorial-build-scheduled-recurring-logic-app-workflow/initialize-variable-action-settings-5.png)

      Şimdi **Değer** alanı burada gösterildiği şekilde görüntülenir:

      ![Çözümlenen ifadeyi içeren "Değer" alanı](./media/tutorial-build-scheduled-recurring-logic-app-workflow/initialize-variable-action-settings-6.png)

5. Mantıksal uygulamanızı kaydedin.

Ardından, geçerli seyahat süresinin belirli bir sınırdan büyük olup olmadığını denetleyen bir koşul ekleyin.

## <a name="compare-travel-time-with-limit"></a>Seyahat süresini sınır ile karşılaştırma

1. Önceki eylemde **+ Yeni adım** > **Koşul ekle** seçeneklerini belirleyin. 

2. Koşulu şu açıklama ile yeniden adlandırın: ```If travel time exceeds limit```

3. Burada açıklandığı ve gösterildiği gibi **travelTime** değerinin belirttiğiniz sınırı aşıp aşmadığını denetleyen bir koşul oluşturun:

   1. Koşul içinde, soldaki (geniş tarayıcı görünümü) veya üstteki (dar tarayıcı görünümü) **Bir değer seçin** kutusunun içine tıklayın.

   2. Dinamik içerik listesinden veya parametre listesinden, **Değişkenler** bölümündeki **travelTime** alanını seçin.

   3. Karşılaştırma kutusunda şu işleci seçin: **büyüktür**

   4. Sağdaki (geniş görünüm) veya alttaki (dar görünüm) **Bir değer seçin** kutusuna şu sınırı girin: ```15```

   Örneğin, dar görünümde çalışıyorsanız bu koşulu şöyle oluşturursunuz:

   ![Dar görünümde koşul oluşturma](./media/tutorial-build-scheduled-recurring-logic-app-workflow/build-condition-check-travel-time-narrow.png)

4. Mantıksal uygulamanızı kaydedin.

Ardından seyahat süresi, sınırınızı aştığında gerçekleştirilecek eylemi ekleyin.

## <a name="send-email-when-limit-exceeded"></a>Sınır aşıldığında e-posta gönder

Şimdi seyahat süresi sınırınızı aştığında size e-posta gönderen bir eylem ekleyin. Bu e-posta, geçerli seyahat süresini ve belirtilen rotada seyahat etmek için gerekli ek süreyi içerir. 

1. Koşulun **True ise** dalında **Eylem ekle**’yi seçin.

2. "E-posta gönder" araması yapın ve e-posta bağlayıcısını ve kullanmak istediğiniz "e-posta gönder eylemini" seçin.

   !["E-posta gönder" eylemini bulup seçin](./media/tutorial-build-scheduled-recurring-logic-app-workflow/add-action-send-email.png)

   * Kişisel Microsoft hesapları için **Outlook.com** girişini seçin. 
   * Azure iş veya okul hesapları için **Office 365 Outlook** girişini seçin.

3. Önceden bir bağlantınız yoksa e-posta hesabınızda oturum açmanız istenir.

   Logic Apps, e-posta hesabınıza bir bağlantı oluşturur.

4. Eylemi şu açıklama ile yeniden adlandırın:
```Send email with travel time```

5. **Alıcı** kutusuna alıcının e-posta adresini girin. Test amacıyla e-posta adresinizi kullanın.

6. **Konu** kutusunda e-postanın konusunu belirtin ve **travelTime** değişkenini dahil edin.

   1. Sonunda boşluk olacak şekilde ```Current travel time (minutes):``` metnini girin. 
   
   2. Parametre listesinden veya dinamik içerik listesinden, **Değişkenler** bölümündeki **travelTime** seçeneğini belirleyin. 
   
      Örneğin, tarayıcınız dar görünümdeyse:

      ![Konu metnini ve seyahat süresini döndüren ifadeyi girin](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-subject-settings.png)

7. **Gövde** kutusunda e-posta gövdesi için içeriği belirtin. 

   1. Sonunda boşluk olacak şekilde ```Add extra travel time (minutes):``` metnini girin. 
   
   2. Gerekirse dinamik içerik listesi görüntüleninceye kadar tarayıcınızı genişletin. 
   Dinamik içerik listesinde **İfade**’yi seçin.

      ![E-posta gövdesi için ifade oluşturma](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-body-settings.png)

   3. İfade düzenleyicisinde, sınırınızı aşan dakika sayısını hesaplayabilmeniz için şu ifadeyi girin: ```sub(,15)```

      ![Ek seyahat süresi dakikasını hesaplamak için ifade girin](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-body-settings-2.png)

   4. İmlecinizi, sol parantez (**(**) ile virgül (**,**) arasındaki ifadeye getirin. **Dinamik içerik** seçeneğini belirleyin.

      ![Ek seyahat süresi dakikasını hesaplamak için ifade oluşturmaya devam edin](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-body-settings-3.png)

   5. **Değişkenler** bölümünde **travelTime** seçeneğini belirleyin.

      ![İfadede kullanılacak "travelTime" alanını seçin](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-body-settings-4.png)

   6. Alan ifadenin içinde çözümlendikten sonra **Tamam**’ı seçin.

      ![Çözümlenen ifadeyi içeren "Gövde" alanı](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-body-settings-5.png)

      Şimdi **Gövde** alanı burada gösterildiği şekilde görüntülenir:

      ![Çözümlenen ifadeyi içeren "Gövde" alanı](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-body-settings-6.png)

8. Mantıksal uygulamanızı kaydedin.

Ardından mantıksal uygulamanızı test edin; mantıksal uygulamanız şu örnek gibi görünür:

![Tamamlanmış mantıksal uygulama](./media/tutorial-build-scheduled-recurring-logic-app-workflow/check-travel-time-finished.png)

## <a name="run-your-logic-app"></a>Mantıksal uygulamanızı çalıştırın

Mantıksal uygulamanızı el ile başlatmak için tasarımcı araç çubuğundan **Çalıştır**'ı seçin. Geçerli seyahat süresi, sınırınız dahilinde kalırsa mantıksal uygulamanız başka bir şey yapmaz ve tekrar denetlemek için bir sonraki zaman aralığını bekler.
Ancak geçerli seyahat süresi, sınırınızı aşarsa geçerli seyahat süresini ve sınırınızı aşan dakika sayısını içeren bir e-posta alırsınız. Mantıksal uygulamanızın gönderdiği örnek bir e-posta şöyledir:

![Seyahat süresi ile gönderilen e-posta](./media/tutorial-build-scheduled-recurring-logic-app-workflow/email-notification.png)

E-posta gelmezse istenmeyen e-posta klasörüne bakın. E-postanızın istenmeyen posta filtresi bu tür postaları yeniden yönlendirebilir. Mantıksal uygulamanızın düzgün bir şekilde çalışıp çalışmadığından emin değilseniz bkz. [Mantıksal uygulama sorunlarını giderme](../logic-apps/logic-apps-diagnosing-failures.md).

Tebrikler, şimdi zamanlama tabanlı yinelenen bir mantıksal uygulama oluşturup çalıştırdınız. 

**Zamanlama - Yinelenme** tetikleyicisini kullanan başka mantıksal uygulamalar oluşturmak için, mantıksal uygulama oluşturduktan sonra mevcut olan şu şablonlara göz atın:

* Size gönderilen günlük anımsatıcıları alın.
* Eski Azure bloblarını silin.
* Azure Depolama kuyruğuna bir ileti gönderin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerek kalmadığında mantıksal uygulamanızı ve ilgili kaynakları içeren kaynak grubunu silin. Ana Azure menüsünde **Kaynak grupları**’na gidin ve mantıksal uygulamanızın kaynak grubunu seçin. **Kaynak grubunu sil**'i seçin. Onay olarak kaynak grubunun adını girip **Sil**’i seçin.

!["Genel Bakış" > "Kaynak grubunu sil"](./media/tutorial-build-scheduled-recurring-logic-app-workflow/delete-resource-group.png)

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, belirtilen bir zamanlama (hafta içi sabahları) temelinde trafiği denetleyen ve seyahat süresi belirtilen bir sınırı aştığında eylem uygulayan (e-posta gönderen) bir mantıksal uygulama oluşturdunuz. Şimdi, Azure hizmetlerini, Microsoft hizmetlerini ve diğer SaaS uygulamalarını tümleştirerek posta listesi isteklerini onaya gönderen bir mantıksal uygulamanın nasıl oluşturulacağını öğreneceksiniz.

> [!div class="nextstepaction"]
> [Posta listesi isteklerini yönetme](../logic-apps/tutorial-process-mailing-list-subscriptions-workflow.md)
