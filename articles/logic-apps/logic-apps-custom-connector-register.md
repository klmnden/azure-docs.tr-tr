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
ms.openlocfilehash: 9e3d88abe751b37700590cc68c458f208d5868d2
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="register-custom-connectors-in-azure-logic-apps"></a>Azure Logic Apps içinde özel bağlayıcılar kaydetme

REST API'nizi oluşturmak, kimlik doğrulamasını kurma ve OpenAPI tanım dosyası veya Postman koleksiyonu almak sonra Bağlayıcınızı kaydetmek hazırsınız. 

## <a name="prerequisites"></a>Ön koşullar

Özel Bağlayıcınızı kaydetmek için bu öğeler gerekir:

* Ad, Azure abonelik, Azure kaynak grubu ve kullanmak istediğiniz konum gibi Azure içinde Bağlayıcınızı kaydettirmek için Ayrıntılar

* Bir OpenAPI (Swagger) dosyası ya da API'nizi tanımlayan bir Postman koleksiyonu

  Bu öğretici için kullandığınız [örnek Azure Kaynak Yöneticisi OpenAPI dosyası](http://pwrappssamples.blob.core.windows.net/samples/AzureResourceManager.json).

* Bağlayıcınızı temsil eden bir simge

* Bağlayıcınızı kısa bir açıklaması

* API'nizi ana bilgisayar konumu

## <a name="1-create-your-connector"></a>1. Bağlayıcınızı oluşturun

1. Azure portalında, ana Azure menüsünden seçin **yeni**. Arama kutusuna "logic apps bağlayıcı", filtre olarak girin ve Enter tuşuna basın.

   !["Logic apps bağlayıcı" Yeni arama](./media/logic-apps-custom-connector-register/create-logic-apps-connector.png)

2. Sonuçları listesinden seçin **Logic Apps bağlayıcı** > **oluşturma**.

   ![Logic Apps bağlayıcı oluşturun](./media/logic-apps-custom-connector-register/choose-logic-apps-connector.png)

3. Tabloda açıklandığı gibi Bağlayıcınızı kayıt için ayrıntılı bilgiler sağlar. İşiniz bittiğinde seçin **panoya Sabitle** > **oluşturma**.

   ![Mantıksal uygulama özel bağlayıcı ayrıntıları](./media/logic-apps-custom-connector-register/logic-apps-connector-details.png)

   | Özellik | Önerilen değer | Açıklama | 
   | -------- | --------------- | ----------- | 
   | **Ad** | *özel bağlayıcı adı* | Bağlayıcı için bir ad sağlayın. | 
   | **Abonelik** | *Azure abonelik adı* | Azure aboneliğinizi seçin. | 
   | **Kaynak grubu** | *Azure kaynak grubu adı* | Azure kaynaklarınızı düzenlemek için bir Azure grubu seçin veya oluşturun. | 
   | **Konum** | *Dağıtım bölgesi* | Bağlayıcınızı için dağıtım bölgeyi seçin. | 
   |||| 

   Azure Bağlayıcınızı dağıtıldıktan sonra özel bağlayıcı menüsü otomatik olarak açılır. 
   Aksi durumda, Azure panodan özel Bağlayıcınızı seçin.

## <a name="2-define-your-connector"></a>2. Bağlayıcınızı tanımlayın

Şimdi OpenAPI dosya ya da Bağlayıcınızı, Bağlayıcınızı kullandığı kimlik doğrulama, Eylemler ve özel Bağlayıcınızı sağlar tetikleyiciler ve Eylemler ve Tetikleyicileri kullanabileceğiniz parametreler oluşturmak için Postman koleksiyonu belirtin.

### <a name="2a-specify-the-openapi-file-or-postman-collection-for-your-connector"></a>2a. OpenAPI dosya veya Bağlayıcınızı Postman toplamalarında belirtin

1. Seçili değilse, bağlayıcı'nın menüde seçin **Logic Apps bağlayıcı**. Araç çubuğunda seçin **Düzenle**.

   ![Özel bağlayıcısını Düzenle](./media/logic-apps-custom-connector-register/edit-custom-connector.png)

2. Seçin **genel** oluşturmak için bu tablolar Ayrıntılar sağlayabilir böylece güvenli hale getirme ve özel Bağlayıcınızı tetikleyiciler ve Eylemler tanımlama.

   1. İçin **özel Bağlayıcılar**, API'nizi tanımlayan OpenAPI (Swagger) dosyasını sağlayabilmesi için bir seçenek belirleyin.

      ![API için OpenAPI dosyası sağlayın](./media/logic-apps-custom-connector-register/provide-openapi-file.png)

      | Seçenek | Biçimi |Açıklama | 
      | ------ | ------ | ----------- | 
      | **OpenAPI dosyasını karşıya yükle** | *OpenAPI (Swagger) - json - dosya* | OpenAPI dosyasının konumuna göz atın ve bu dosyayı seçin. | 
      | **Bir OpenAPI URL'yi kullanın** | http://*yolu swagger-json-dosyaya* | URL için API OpenAPI dosyası sağlar. | 
      | **Postman koleksiyonu V1 karşıya yükle** | *dışa aktarılan-Postman-koleksiyon-V1-dosyası* | V1 biçiminde dışarı aktarılan bir Postman koleksiyonu için konumuna göz atın. | 
      |||| 

   2. İçin **genel bilgiler**, Bağlayıcınızı için bir simgeyi karşıya yükleyin. 
   Genellikle, **açıklama**, **konak**, ve **taban URL** alanları OpenAPI dosyanızdan otomatik olarak doldurulur. 
   Ancak değilseniz, bu bilgileri tabloda açıklandığı şekilde ekleyin ve seçin **devam**. 

      ![Bağlayıcı ayrıntıları](./media/logic-apps-custom-connector-register/add-connector-details.png)

      | Seçenek veya ayarı | Biçimi | Açıklama | 
      | ----------------- | ------ | ----------- | 
      | **Simgeyi karşıya yükleyin** | *PNG-or-jpg-File-Under-1-MB* | Bağlayıcınızı temsil eden bir simge <p>Renk: Tercihen beyaz logosu arka plan rengi. <p>Boyutlar: 230 piksel kare içinde ~ 160 piksel logosu bir | 
      | **Simge arka plan rengi** | *simge-brand-renk-onaltılık-code* | <p>Arka plan rengi, simge dosyanızdaki eşleşen simge arkasındaki rengi. <p>Biçim: onaltılık. Örneğin, #007ee5 mavi rengi temsil eder. | 
      | **Açıklama** | *Bağlayıcı açıklaması* | Bağlayıcınızı için kısa bir açıklama sağlayın. | 
      | **Ana bilgisayar** | *Bağlayıcı ana bilgisayar* | Ana bilgisayar etki alanı için Web API sağlar. | 
      | **Temel URL** | *Bağlayıcı taban URL'si* | Temel URL için Web API sağlar. | 
      |||| 

### <a name="2b-describe-the-authentication-that-your-connector-uses"></a>2B. Bağlayıcınızı kullandığı kimlik doğrulama açıklayın

1. Artık seçim **güvenlik** gözden geçirmek ya da Bağlayıcınızı kullandığı kimlik doğrulama açıklanmaktadır. Kimlik doğrulaması, kullanıcıların kimliklerini hizmetiniz ve istemciler arasında uygun şekilde akış emin olur.

   ![Kimlik doğrulama türü](./media/logic-apps-custom-connector-register/security.png)

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
     | **İstemci kimliği** | *application-ID-for-connector-Azure-AD-app* | Bağlantı ucunun Azure AD uygulaması için daha önce kaydettiğiniz uygulama kimliği | 
     | **İstemci parolası** | *client-key-for-connector-Azure-AD-app* | Bağlantı ucunun Azure AD uygulaması için istemci anahtarı | 
     | **Oturum açma URL'si** | `https://login.windows.net` | | 
     | **Kaynak URL** | `https://management.core.windows.net/` | Sondaki eğik çizgi dahil olmak üzere tam olarak belirtilen URL'ye, eklediğinizden emin olun. | 
     |||| 

   * Bir OpenAPI dosyası sizin için otomatik olarak oluşturur, bir Postman koleksiyonu kimlik doğrulama türünü otomatik olarak doldurur *yalnızca* OAuth 2.0 veya Basic gibi desteklenen kimlik doğrulama türlerini kullandığınızda.

2. Sayfanın üstündeki güvenlik bilgileri girdikten sonra Bağlayıcınızı kaydetmek üzere seçim yapın **güncelleştirme bağlayıcı**, ardından **devam**. 

### <a name="2c-review-update-or-define-actions-and-triggers-for-your-connector"></a>2c. Gözden geçirin, güncelleştirmeniz ya da eylemleri ve Bağlayıcınızı Tetikleyicileri tanımlayın

1. Artık seçim **tanımı** gözden geçirebilmeniz için düzenleyin veya yeni eylemleri ve kullanıcılar kendi iş akışlarına ekleyebilirler tetikleyicilerini tanımlayın.

   Eylemler ve tetikleyicileri, otomatik olarak doldurulması OpenAPI dosya veya Postman koleksiyonu içinde tanımlanan işlemleri üzerinde dayalı **tanımı** sayfasında ve istek ve yanıt değerlerini içerir. Gerekli işlemlerini zaten burada görünüyorsa, bu nedenle, kayıt işleminin sonraki adımına bu sayfada değişiklik yapmadan gidebilirsiniz.

   ![Bağlayıcı tanımı](./media/logic-apps-custom-connector-register/definition.png)

2. İsteğe bağlı olarak, var olan eylemler ve Tetikleyicileri düzenleyin veya yenilerini eklemek istiyorsanız, bu adımlara devam edin.

<a name="add-action-or-trigger"></a> 

#### <a name="edit-or-add-actions-for-your-connector"></a>Düzenleme veya Bağlayıcınızı eylemlerinde ekleme

1. Varolan eylem düzenlemek için bu eylemi numarası seçin. OpenAPI dosya ya da Postman koleksiyonu altında bulunamadı bir eylem eklemek amacıyla **Eylemler**, seçin **yeni eylem**.

2. Altında **genel**, sağlayın veya eylem için bu bilgileri düzenleyin:
   
   | Ayar | Önerilen değer | Açıklama | 
   | ------- | --------------- | ----------- | 
   | **Özet** | *işlem adı* | Bu eylem için başlık | 
   | **Açıklama** | *işlemi açıklaması* | Bu eylem açıklaması. <p>**İpucu**: açıklamanızı bir nokta ile biten emin olun. |
   | **İşlem kimliği** | *işlem tanımlayıcı* | Bu eylem tanımlamak için benzersiz bir ad. Ortası büyük, örneğin kullanın: "GetPullRequest" | 
   |**Görünürlüğü**| **Hiçbiri**, **Gelişmiş**, **iç**, veya **önemli** | Bu eylem için kullanıcı dönük görünürlük. Daha fazla bilgi için bkz: [OpenAPI uzantıları](../logic-apps/custom-connector-openapi-extensions.md#visibility). | 
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
   Seçin **varsayılan yanıt eklemek**.

   2. Üzerinde **örnek alma** sayfasında, bir örnek yanıt yapıştırın ve ardından **alma**.

5. Son olarak, altında **doğrulama**, gözden geçirin ve eylem için tanımlanan tüm olası sorunları giderin.

#### <a name="edit-or-add-triggers-for-your-connector"></a>Düzenleme veya Bağlayıcınızı Tetikleyicileri ekleme

1. Varolan bir tetikleyicinin düzenlemek için Bu tetikleyici numarası seçin. OpenAPI dosya ya da Postman koleksiyonu altında bulunamadı bir tetikleyici eklemek için **Tetikleyicileri**, seçin **Yeni Tetikleyici**.

2. Altında **genel**, sağlayın veya tetikleyici için bu bilgileri düzenleyin:

   | Ayar | Önerilen değer | Açıklama | 
   | ------- | --------------- | ----------- | 
   | **Özet** | *işlem adı* | Bu tetikleyicinin başlığı | 
   | **Açıklama** | *işlemi açıklaması* | Bu tetikleyicinin açıklaması. <p>**İpucu**: açıklamanızı bir nokta ile biten emin olun. | 
   | **İşlem kimliği** | *işlem tanımlayıcı* | Bu tetikleyici tanımlamak için benzersiz bir ad. Ortası büyük, örneğin kullanın: "TriggerOnGitHubPushEvent" | 
   |**Görünürlüğü**| **Hiçbiri**, **Gelişmiş**, **iç**, veya **önemli** | Bu tetikleyicinin kullanıcı dönük görünürlük. Daha fazla bilgi için bkz: [OpenAPI uzantıları](../logic-apps/custom-connector-openapi-extensions.md#visibility). | 
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
   Seçin **varsayılan yanıt eklemek**.

   2. Üzerinde **örnek alma** sayfasında, bir örnek yanıt yapıştırın ve ardından **alma**.

   3. Altında **tetikleyici yapılandırma**, değer parametresi için geçirilecek sorgu parametresi belirtin ve bir *tetikleyici İpucu* sonraki istek için uygun bir yoklama aralığı belirtir.

5. Son olarak, altında **doğrulama**, gözden geçirin ve tetikleyici için tanımlanan tüm olası sorunları giderin.

#### <a name="review-reference-definitions-for-your-connector"></a>Gözden geçirme başvuru Bağlayıcınızı tanımlarında

**Başvuruları** bölümde otomatik olarak API açıklamasından doldurur ve eylemleri ve Tetikleyicileri başvurabilir herhangi bir parametre alan açıklanmaktadır. 

1. Altında **başvuruları**, gözden geçirmek istediğiniz başvuru tanımına numarası seçin.

2. Altında **adı**, gözden geçirmek veya başvuru tanımı adını güncelleştirin.

3. Altında **doğrulama**, gözden geçirin ve referans tanımı tanımlanan olası sorunları giderin.

## <a name="3-finish-creating-your-connector"></a>3. Bağlayıcınızı oluşturmayı tamamlayın

Hazır olduğunuzda, seçin **oluşturma** Bağlayıcınızı dağıtabilmek için. Var olan bağlayıcının güncelleştiriyorsanız seçin **güncelleştirme bağlayıcı**.

Tebrikler! Şimdi mantıksal uygulama oluşturduğunuzda, Bağlayıcınızı Logic Apps Tasarımcısı'nda bulmak ve bu bağlayıcı mantıksal uygulamanızı ekleyin.

![Logic Apps Tasarımcısı'nda Bağlayıcınızı Bul](./media/logic-apps-custom-connector-register/custom-connector-created.png)

## <a name="share-your-connector-with-other-logic-apps-users"></a>Bağlayıcınızı diğer Logic Apps kullanıcılarla paylaşma

Kayıtlı ancak onaylı olmayan özel bağlayıcılar Microsoft tarafından yönetilen bağlayıcıların gibi çalışır, ancak görünür ve kullanılabilir *yalnızca* bağlayıcı'nın yazar ve aynı Azure Active Directory kiracısına ve Azure aboneliğine sahip kullanıcılar Bu uygulamaları'nın dağıtıldığı bölgede Logic apps için. Paylaşımı isteğe bağlı olsa da, bağlayıcılar diğer kullanıcılarla paylaşmak istediğiniz senaryolar olabilir. 

> [!IMPORTANT]
> Bir bağlayıcı paylaşıyorsanız, diğerlerinin bu bağlayıcıda bağımlı başlayabilir. 
> ***Bağlayıcınızı silindiğinde, bu bağlayıcı için tüm bağlantılar silinir.***
 
Bağlayıcınızı harici kullanıcılar bu bu gibi bir durumda sınırları dışında tüm Logic Apps, akış ve PowerApps kullanıcılarla paylaşmak için [Bağlayıcınızı Microsoft sertifika için gönderme](../logic-apps/custom-connector-submit-certification.md).

## <a name="faq"></a>SSS

**S:** özel bağlayıcıların herhangi bir sınır vardır? </br>
**Y:** Evet, bkz: [özel bağlayıcı sınırlar burada](../logic-apps/logic-apps-limits-and-config.md#custom-connector-limits).

## <a name="get-support"></a>Destek alın

* Geliştirme ve ekleme veya Kayıt Sihirbazı'nda bulunmayan istek özellikleri için destek için iletişim [ condevhelp@microsoft.com ](mailto:condevhelp@microsoft.com). Microsoft developer sorular ve sorunlar için bu hesabı izler ve bunları uygun ekibine yönlendirir.

* İsteyin veya soruyu yanıtlayın veya diğer Azure mantıksal uygulamaları kullanıcıların ne yaptıklarını bakın, ziyaret [Azure Logic Apps Forumu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

* Logic Apps geliştirmeye yardımcı olmak için oy veya fikir gönderme [Logic Apps kullanıcı geri bildirim sitesi](http://aka.ms/logicapps-wish). 

## <a name="next-steps"></a>Sonraki adımlar

* [İsteğe bağlı: Bağlayıcınızı Onayla](../logic-apps/custom-connector-submit-certification.md)
* [Özel bağlayıcı SSS](../logic-apps/custom-connector-faq.md)