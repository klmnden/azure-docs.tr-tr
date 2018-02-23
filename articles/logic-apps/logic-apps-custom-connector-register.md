---
title: "Özel bağlayıcılar - Azure Logic Apps kaydetme | Microsoft Docs"
description: "Azure mantıksal uygulamaları kullanmak için özel bağlayıcılar ayarlama"
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
ms.topic: article
ms.date: 09/22/2017
ms.author: LADocs; estfan
ms.openlocfilehash: e019b990a3f79eca0b4119e97d44eee3632e8a33
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="register-custom-connectors-in-azure-logic-apps"></a>Azure Logic Apps içinde özel bağlayıcılar kaydetme

REST API'nizi oluşturmak, kimlik doğrulamasını kurma ve OpenAPI tanım dosyası veya Postman koleksiyonu almak sonra Bağlayıcınızı kaydetmek hazırsınız. 

## <a name="prerequisites"></a>Önkoşullar

Özel Bağlayıcınızı kaydetmek için bu öğeler gerekir:

* Ad, Azure abonelik, Azure kaynak grubu ve kullanmak istediğiniz konum gibi Azure içinde Bağlayıcınızı kaydettirmek için Ayrıntılar

* Bir OpenAPI (Swagger) dosyası ya da API'nizi tanımlayan bir Postman koleksiyonu

  Bu öğretici için kullandığınız [örnek Azure Kaynak Yöneticisi OpenAPI dosyası](http://pwrappssamples.blob.core.windows.net/samples/AzureResourceManager.json).

* Bağlayıcınızı temsil eden bir simge

* Bağlayıcınızı kısa bir açıklaması

* API'nizi ana bilgisayar konumu

## <a name="1-create-your-connector"></a>1. Bağlayıcınızı oluşturma

1. Azure portalında, ana Azure menüsünden seçin **kaynak oluşturma**. Arama kutusuna filtreniz olarak “logic apps bağlayıcısı” yazın ve Enter tuşuna basın.

   ![Yeni, “logic apps bağlayıcısı” ifadesini aratın](./media/logic-apps-custom-connector-register/create-logic-apps-connector.png)

2. Sonuç listesinden **Logic Apps Bağlayıcısı** > **Oluştur**’u seçin.

   ![Logic Apps bağlayıcısı oluşturma](./media/logic-apps-custom-connector-register/choose-logic-apps-connector.png)

3. Bağlayıcınızı kaydetmek için tabloda açıklandığı gibi ayrıntıları sağlayın. İşiniz bittiğinde **Panoya sabitle** > **Oluştur**’u seçin.

   ![Mantıksal Uygulama özel bağlayıcı ayrıntıları](./media/logic-apps-custom-connector-register/logic-apps-connector-details.png)

   | Özellik | Önerilen değer | Açıklama | 
   | -------- | --------------- | ----------- | 
   | **Ad** | *özel bağlayıcı adı* | Bağlayıcınıza bir ad verin. | 
   | **Abonelik** | *Azure-subscription-name* | Azure aboneliğinizi seçin. | 
   | **Kaynak grubu** | *Azure-resource-group-name* | Azure kaynaklarınızı düzenlemek için bir Azure grubu oluşturun veya seçin. | 
   | **Konum** | *deployment-region* | Bağlayıcınız için bir dağıtım bölgesi seçin. | 
   |||| 

   Azure bağlayıcınızı dağıttıktan sonra, özel bağlayıcı menüsü otomatik olarak açılır. 
   Aksi durumda, Azure panosundan özel bağlayıcınızı seçin.

## <a name="2-define-your-connector"></a>2. Bağlayıcınızı tanımlama

Şimdi OpenAPI dosya ya da Bağlayıcınızı, Bağlayıcınızı kullandığı kimlik doğrulama, Eylemler ve özel Bağlayıcınızı sağlar tetikleyiciler ve Eylemler ve Tetikleyicileri kullanabileceğiniz parametreler oluşturmak için Postman koleksiyonu belirtin.

### <a name="2a-specify-the-openapi-file-or-postman-collection-for-your-connector"></a>2a. OpenAPI dosya veya Bağlayıcınızı Postman toplamalarında belirtin

1. Bağlayıcınızın menüsünde **Logic Apps Bağlayıcısı** zaten seçili değilse bunu seçin. Araç çubuğunda **Düzenle**'yi seçin.

   ![Özel bağlayıcısını Düzenle](./media/logic-apps-custom-connector-register/edit-custom-connector.png)

2. Seçin **genel** oluşturmak için bu tablolar Ayrıntılar sağlayabilir böylece güvenli hale getirme ve özel Bağlayıcınızı tetikleyiciler ve Eylemler tanımlama.

   1. İçin **özel Bağlayıcılar**, API'nizi tanımlayan OpenAPI (Swagger) dosyasını sağlayabilmesi için bir seçenek belirleyin.

      ![API için OpenAPI dosyası sağlayın](./media/logic-apps-custom-connector-register/provide-openapi-file.png)

      | Seçenek | Biçim |Açıklama | 
      | ------ | ------ | ----------- | 
      | **OpenAPI dosyasını karşıya yükle** | *OpenAPI (Swagger)-json-file* | OpenAPI dosyasının konumuna göz atın ve bu dosyayı seçin. | 
      | **Bir OpenAPI URL'yi kullanın** | http://*path-to-swagger-json-file* | URL için API OpenAPI dosyası sağlar. | 
      | **Postman koleksiyonu V1 karşıya yükle** | *exported-Postman-collection-V1-file* | V1 biçiminde dışarı aktarılan bir Postman koleksiyonu için konumuna göz atın. | 
      |||| 

   2. **Genel bilgiler** için bağlayıcınıza ait bir simgeyi karşıya yükleyin. 
   Genellikle, **açıklama**, **konak**, ve **taban URL** alanları OpenAPI dosyanızdan otomatik olarak doldurulur. 
   Aksi takdirde, bu bilgileri tabloda açıklandığı gibi ekleyin ve **Devam**’ı seçin. 

      ![Bağlayıcı ayrıntıları](./media/logic-apps-custom-connector-register/add-connector-details.png)

      | Seçenek veya ayar | Biçim | Açıklama | 
      | ----------------- | ------ | ----------- | 
      | **Karşıya Simge Yükleme** | *png-or-jpg-file-under-1-MB* | Bağlayıcınızı temsil eden bir simge <p>Renk: Tercihen renkli bir arka plan üzerine beyaz logo. <p>Boyutlar: 230 piksel bir kare içinde yaklaşık 160 piksel boyutunda logo | 
      | **Simge arka plan rengi** | *icon-brand-color-hexadecimal-code* | <p>Simgenizin arkasındaki, simge dosyanızın arka plan rengiyle eşleşen renk. <p>Biçim: Onaltılık. Örneğin, #007ee5 mavi rengi temsil eder. | 
      | **Açıklama** | *connector-description* | Bağlayıcınız için kısa bir açıklama sağlayın. | 
      | **Ana Bilgisayar** | *connector-host* | Ana bilgisayar etki alanı için Web API sağlar. | 
      | **Temel URL** | *connector-base-URL* | Temel URL için Web API sağlar. | 
      |||| 

### <a name="2b-describe-the-authentication-that-your-connector-uses"></a>2b. Bağlayıcınızın kullandığı kimlik doğrulamasını açıklayın

1. Şimdi bağlayıcınızın kullandığı kimlik doğrulamasını gözden geçirmek veya açıklamak için **Güvenlik**’i seçin. Kimlik doğrulaması, hizmetiniz ile herhangi bir istemci arasında kullanıcılarınızın kimliklerinin uygun şekilde akışının gerçekleştirilmesini sağlar.

   ![Kimlik doğrulaması türü](./media/logic-apps-custom-connector-register/security.png)

   * Bir OpenAPI dosyasını karşıya yükleyin, Kayıt Sihirbazı'nı, Web API'sini kullanır, kimlik doğrulama türü otomatik olarak algılar ve otomatik olarak doldurur **güvenlik** göre sihirbazın bölümünde `securityDefinitions` , nesne dosya. Örneğin, OAuth2.0 kullanarak belirten bir bölüm şöyledir:

     ``` json
     "securityDefinitions": {
       "AAD": {
       "type": "oauth2",
       "flow": "accessCode",
       "authorizationUrl": "https://login.windows.net/common/oauth2/authorize",
       "tokenUrl": "https://login.windows.net/common/oauth2/token",
       "scopes": {}
       }
     },
     ```

   * OpenAPI dosyanızı kimlik doğrulama türü ve özelliklerini doldurmak kaydetmedi kaldığınızda **Düzenle** bu bilgi ekleyebilmesi için. 
   
     Örneğin, varsa, daha önce [Bu öğreticide Azure AD kimlik doğrulama ayarlama](../logic-apps/custom-connector-azure-active-directory-authentication.md), Azure AD uygulamaları, Web API ve Bağlayıcınızı güvenliğini sağlama için oluşturduğunuz. 
     Şimdi daha önce kaydettiğiniz uygulama kimliği ve istemci anahtarını sağlayabilirsiniz:
    
     | Ayar | Önerilen değer | Açıklama | 
     | ------- | --------------- | ----------- | 
     | **Kimlik doğrulama türü** | OAuth 2.0 | | 
     | **Kimlik sağlayıcısı** | Azure Active Directory | | 
     | **istemci kimliği** | *application-ID-for-connector-Azure-AD-app* | Bağlantı ucunun Azure AD uygulaması için daha önce kaydettiğiniz uygulama kimliği | 
     | İstemci parolası | *client-key-for-connector-Azure-AD-app* | Bağlantı ucunun Azure AD uygulaması için istemci anahtarı | 
     | **Oturum açma URL'si** | `https://login.windows.net` | | 
     | **Kaynak URL** | `https://management.core.windows.net/` | Sondaki eğik çizgi dahil olmak üzere tam olarak belirtilen URL'ye, eklediğinizden emin olun. | 
     |||| 

   * Bir OpenAPI dosyası sizin için otomatik olarak oluşturur, bir Postman koleksiyonu kimlik doğrulama türünü otomatik olarak doldurur *yalnızca* OAuth 2.0 veya Basic gibi desteklenen kimlik doğrulama türlerini kullandığınızda.

2. Güvenlik bilgilerini girdikten sonra bağlayıcınızı kaydetmek için sayfanın üst kısmından **Bağlayıcıyı güncelleştir**’i ve sonra **Devam**’ı seçin. 

### <a name="2c-review-update-or-define-actions-and-triggers-for-your-connector"></a>2c. Bağlayıcınızın eylem ve tetikleyicilerini gözden geçirme, güncelleştirme veya tanımlama

1. Şimdi kullanıcılarınızın iş akışlarına ekleyebileceği eylem ve tetikleyicileri gözden geçirmek, düzenlemek veya yenilerini tanımlamak için **Tanım**’ı seçin.

   Eylemler ve tetikleyicileri, otomatik olarak doldurulması OpenAPI dosya veya Postman koleksiyonu içinde tanımlanan işlemleri üzerinde dayalı **tanımı** sayfasında ve istek ve yanıt değerlerini içerir. Bu nedenle, bu sayfada gerekli işlemler zaten görünüyorsa herhangi bir değişiklik yapmadan kayıt işleminin bir sonraki adımına geçebilirsiniz.

   ![Bağlayıcı tanımı](./media/logic-apps-custom-connector-register/definition.png)

2. İsteğe bağlı olarak, var olan eylemler ve Tetikleyicileri düzenleyin veya yenilerini eklemek istiyorsanız, bu adımlara devam edin.

<a name="add-action-or-trigger"></a> 

#### <a name="edit-or-add-actions-for-your-connector"></a>Düzenleme veya Bağlayıcınızı eylemlerinde ekleme

1. Varolan eylem düzenlemek için bu eylemi numarası seçin. OpenAPI dosya ya da Postman koleksiyonu altında bulunamadı bir eylem eklemek amacıyla **Eylemler**, seçin **yeni eylem**.

2. Altında **genel**, sağlayın veya eylem için bu bilgileri düzenleyin:
   
   | Ayar | Önerilen değer | Açıklama | 
   | ------- | --------------- | ----------- | 
   | **Özet** | *işlem adı* | Bu eylem için başlık | 
   | **Açıklama** | *operation-description* | Bu eylem açıklaması. <p>**İpucu**: açıklamanızı bir nokta ile biten emin olun. |
   | **İşlem kimliği** | *operation-identifier* | Bu eylem tanımlamak için benzersiz bir ad. Ortası büyük, örneğin kullanın: "GetPullRequest" | 
   |**Görünürlük**| **Hiçbiri**, **Gelişmiş**, **iç**, veya **önemli** | Bu eylem için kullanıcı dönük görünürlük. Daha fazla bilgi için bkz: [OpenAPI uzantıları](../logic-apps/custom-connector-openapi-extensions.md#visibility). | 
   |||| 

3. Şimdi eylemi için istek tanımlayın.
  
   1. İçinde **isteği** bölümünde, seçin **örnek alma**. 

   2. Üzerinde **örnek alma** sayfasında, bir örnek istek yapıştırın. 

      Genellikle, örnek istekleri nereden edinebileceğiniz bilgi için API belgelerinde kullanılabilir **fiili**, **URL**, **üstbilgileri**, ve **gövde**alanları. Örneğin, [metin Analytics API belgelerine](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7).

      ![İçeri aktarma örnek istek](./media/logic-apps-custom-connector-register/import-sample-operation-request.png)

      > [!IMPORTANT]
      > Postman koleksiyondan bir bağlayıcı oluşturursanız, kaldırdığınız emin `Content-type` eylemleri ve Tetikleyicileri başlığından. Logic Apps bu başlığı otomatik olarak ekler. Ayrıca, tanımlanan kimlik doğrulama üstbilgileri kaldırın `Security` eylemleri ve Tetikleyicileri bölümünden.

      Gelişmiş OpenAPI işlevselliği için bkz: [OpenAPI uzantıları özel bağlayıcıların](../logic-apps/custom-connector-openapi-extensions.md).

   3. İstek tanımının son tercih **alma**.

4. Şimdi eylem için yanıt tanımlayın.

   1. Altında **yanıt**, varsayılan bir yanıt belirtebilirsiniz. 
   **Varsayılan yanıt ekle**’yi seçin.

   2. Üzerinde **örnek alma** sayfasında, bir örnek yanıt yapıştırın ve ardından **alma**.

5. Son olarak, altında **doğrulama**, gözden geçirin ve eylem için tanımlanan tüm olası sorunları giderin.

#### <a name="edit-or-add-triggers-for-your-connector"></a>Düzenleme veya Bağlayıcınızı Tetikleyicileri ekleme

1. Varolan bir tetikleyicinin düzenlemek için Bu tetikleyici numarası seçin. OpenAPI dosya ya da Postman koleksiyonu altında bulunamadı bir tetikleyici eklemek için **Tetikleyicileri**, seçin **Yeni Tetikleyici**.

2. Altında **genel**, sağlayın veya tetikleyici için bu bilgileri düzenleyin:

   | Ayar | Önerilen değer | Açıklama | 
   | ------- | --------------- | ----------- | 
   | **Özet** | *işlem adı* | Bu tetikleyicinin başlığı | 
   | **Açıklama** | *operation-description* | Bu tetikleyicinin açıklaması. <p>**İpucu**: açıklamanızı bir nokta ile biten emin olun. | 
   | **İşlem kimliği** | *operation-identifier* | Bu tetikleyici tanımlamak için benzersiz bir ad. Ortası büyük, örneğin kullanın: "TriggerOnGitHubPushEvent" | 
   |**Görünürlük**| **Hiçbiri**, **Gelişmiş**, **iç**, veya **önemli** | Bu tetikleyicinin kullanıcı dönük görünürlük. Daha fazla bilgi için bkz: [OpenAPI uzantıları](../logic-apps/custom-connector-openapi-extensions.md#visibility). | 
   | **Tetikleyici türü** | **Web kancası** veya **yoklama** | Bu tetikleyicinin türü. Örneğin, bir Web kancası tetikleyici belirli bir olay tetikleme önce olmasını bekler. Yoklama tetikleyici, belirtilen zaman aralığı ve sıklığı temel alan bir hizmet uç noktası düzenli olarak denetler. <p>Web kancası ve tetikleyici desenleri yoklama hakkında daha fazla bilgi için bkz: [özel API oluşturma](../logic-apps/logic-apps-create-api-app.md). | 
   |||| 

3. Şimdi tetikleyici oluşturur isteğin tanımlayın. 

   1. İçinde **isteği** bölümünde, seçin **örnek alma**.

   2. Üzerinde **örnek alma** sayfasında, bir örnek istek yapıştırın. 

      Genellikle, örnek istekleri nereden edinebileceğiniz bilgi için API belgelerinde kullanılabilir **fiili**, **URL**, **üstbilgileri**, ve **gövde**alanları. Örneğin, [metin Analytics API belgelerine](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7).

      ![İçeri aktarma örnek istek](./media/logic-apps-custom-connector-register/import-sample-operation-request.png)

      > [!IMPORTANT]
      > Postman koleksiyondan bir bağlayıcı oluşturursanız, kaldırdığınız emin `Content-type` eylemleri ve Tetikleyicileri başlığından. Logic Apps bu başlığı otomatik olarak ekler. Ayrıca, tanımlanan kimlik doğrulama üstbilgileri kaldırın `Security` eylemleri ve Tetikleyicileri bölümünden.

      Gelişmiş OpenAPI işlevselliği için bkz: [OpenAPI uzantıları özel bağlayıcıların](../logic-apps/custom-connector-openapi-extensions.md).

   3. İstek tanımının son tercih **alma**. 

4. Şimdi tetikleyicinin yanıt tanımlayın. İçinde **yanıt** tetikleyici türüne göre bölümünde, aşağıdaki adımları izleyin:

   **Web kancası tetikleyici**
   1. İçinde **Web kancası yanıt**, seçin **örnek alma**. 

   2. Üzerinde **örnek alma** sayfasında, bir örnek yanıt yapıştırın ve ardından **alma**. Bir örnek yanıt görüntülemek için bkz: [bir Web kancası oluşturma için GitHub API başvuru](https://developer.github.com/v3/repos/hooks/#create-a-hook).

   3. Altında **tetikleyici yapılandırma**, Web kancası oluşturma isteği kullanmak için bir parametre seçin. Logic Apps, tetikleyici ile iletişim kurmak için hizmetiniz tarafından kullanılan geri çağırma URL'si doldurmak için bu parametre değeri kullanır.

   **Yoklama tetikleyici**
   1. Altında **yanıt**, varsayılan bir yanıt belirtebilirsiniz. 
   **Varsayılan yanıt ekle**’yi seçin.

   2. Üzerinde **örnek alma** sayfasında, bir örnek yanıt yapıştırın ve ardından **alma**.

   3. Altında **tetikleyici yapılandırma**, değer parametresi için geçirilecek sorgu parametresi belirtin ve bir *tetikleyici İpucu* sonraki istek için uygun bir yoklama aralığı belirtir.

5. Son olarak, altında **doğrulama**, gözden geçirin ve tetikleyici için tanımlanan tüm olası sorunları giderin.

#### <a name="review-reference-definitions-for-your-connector"></a>Gözden geçirme başvuru Bağlayıcınızı tanımlarında

**Başvuruları** bölümde otomatik olarak API açıklamasından doldurur ve eylemleri ve Tetikleyicileri başvurabilir herhangi bir parametre alan açıklanmaktadır. 

1. Altında **başvuruları**, gözden geçirmek istediğiniz başvuru tanımına numarası seçin.

2. Altında **adı**, gözden geçirmek veya başvuru tanımı adını güncelleştirin.

3. Altında **doğrulama**, gözden geçirin ve referans tanımı tanımlanan olası sorunları giderin.

## <a name="3-finish-creating-your-connector"></a>3. Bağlayıcınızı oluşturmayı tamamlama

Hazır olduğunuzda, seçin **oluşturma** Bağlayıcınızı dağıtabilmek için. Var olan bağlayıcının güncelleştiriyorsanız seçin **güncelleştirme bağlayıcı**.

Tebrikler! Artık bir mantıksal uygulama oluşturduğunuzda bağlayıcınızı Logic Apps Tasarımcısı’nda bulabilir ve mantıksal uygulamanıza ekleyebilirsiniz.

![Logic Apps Tasarımcısı’nda bağlayıcınızı bulma](./media/logic-apps-custom-connector-register/custom-connector-created.png)

## <a name="share-your-connector-with-other-logic-apps-users"></a>Bağlayıcınızı diğer Logic Apps kullanıcılarıyla paylaşma

Kayıtlı ve sertifikasız özel bağlayıcılar, Microsoft tarafından yönetilen bağlayıcılar gibi çalışır, ancak *yalnızca* bağlayıcının yazarı ve mantıksal uygulamalar için bu uygulamaların dağıtıldığı bölgede aynı Azure Active Directory kiracısına ve Azure aboneliğine sahip olanlar tarafından görüntülenip kullanılabilir. Paylaşım isteğe bağlı olsa da, bağlayıcılarınızı başka kullanıcılarla paylaşmak istediğiniz senaryolarınız olabilir. 

> [!IMPORTANT]
> Bir bağlayıcı paylaştığınızda, başkaları da bu bağlayıcıya bağımlı olmaya başlayabilir. 
> ***Bağlayıcınız silindiğinde, bu bağlayıcıya olan tüm bağlantılar silinir.***
 
Bağlayıcınızı harici kullanıcılar bu bu gibi bir durumda sınırları dışında tüm Logic Apps, akış ve PowerApps kullanıcılarla paylaşmak için [Bağlayıcınızı Microsoft sertifika için gönderme](../logic-apps/custom-connector-submit-certification.md).

## <a name="faq"></a>SSS

**S:** Özel bağlayıcılar için herhangi bir sınırlama var mı? </br>
**C:** Evet, bkz. [özel bağlayıcı sınırlamaları](../logic-apps/logic-apps-limits-and-config.md#custom-connector-limits).

## <a name="get-support"></a>Destek alın

* Geliştirme ve ekleme konusunda destek almak ya da kayıt sihirbazında bulunmayan özellikleri istemek için [condevhelp@microsoft.com](mailto:condevhelp@microsoft.com) adresine başvurun. Microsoft, bu hesaba gönderilen geliştirici sorularını ve sorunlarını izleyip bunları uygun ekibe yönlendirir.

* Soru sormak veya soruları yanıtlamak ya da diğer Azure Logic Apps kullanıcılarının neler yaptığını görmek için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.

* Logic Apps’in geliştirilmesine yardımcı olmak için, [Logic Apps kullanıcı geri bildirim sitesinde](http://aka.ms/logicapps-wish) oy kullanın veya fikirlerinizi paylaşın. 

## <a name="next-steps"></a>Sonraki adımlar

* [İsteğe bağlı: Bağlayıcınızı Onayla](../logic-apps/custom-connector-submit-certification.md)
* [Özel bağlayıcı hakkında SSS](../logic-apps/custom-connector-faq.md)