---
title: "Azure AD - Azure Logic Apps ile özel bağlayıcılar güvenli | Microsoft Docs"
description: "Azure Active Directory (Azure AD) ile API’nize ve bağlayıcınıza güvenlik ve kimlik doğrulaması ekleme"
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
ms.openlocfilehash: 5fd59677fc6abf80666caf90aa3da7e553648bca
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="add-security-to-your-api-and-connector-with-azure-active-directory-azure-ad"></a>API ve bağlayıcı Azure Active Directory (Azure AD) ile güvenlik ekleme

API’niz ile özel bağlayıcınız arasındaki çağrıların güvenliğini sağlamak için Web API’nize ve bağlayıcınıza Azure AD’yi ekleyin. Bu senaryo için iki Azure Active Directory (Azure AD) uygulaması oluşturursunuz; bir Azure AD uygulaması Web API’nizin güvenliğini sağlarken diğer Azure AD uygulaması bağlayıcı kaydınızın güvenliğini sağlar ve temsilci erişimi ekler. 

Bu öğreticide örnek olarak Azure Resource Manager API'si kullanır. Azure Resource Manager, Azure’da oluşturduğunuz bir çözümün veritabanları, sanal makineler ve web uygulamaları gibi bileşenlerini yönetmenize yardımcı olur. Azure kaynaklarını iş akışlarından yönetmek isteyenler için özel bir Azure Resource Manager bağlayıcısı kullanışlı olabilir. Daha fazla bilgi için bkz. [Azure Resource Manager'a Genel Bakış](../azure-resource-manager/resource-group-overview.md).

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Bir aboneliğiniz yoksa [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) ile başlayabilirsiniz. Aksi takdirde, bir [Kullandıkça Öde aboneliğine](https://azure.microsoft.com/pricing/purchase-options/) kaydolun.

* Bu öğretici için [Azure Resource Manager’a yönelik örnek OpenAPI dosyası](https://pwrappssamples.blob.core.windows.net/samples/AzureResourceManager.json)

  > [!NOTE] 
  > Örnek OpenAPI dosyası tüm Azure Resource Manager işlemlerini tanımlamaz ve şu anda yalnızca [Tüm abonelikleri listele](https://docs.microsoft.com/rest/api/resources/subscriptions#Subscriptions_List) işlemini içerir. [Çevrimiçi OpenAPI düzenleyicisini](http://editor.swagger.io/) kullanarak bu OpenAPI dosyasını düzenleyebilir veya başka OpenAPI dosyası oluşturabilirsiniz.
  >
  > Bu öğreticiyi Azure AD kimlik doğrulaması kullanmak istediğiniz herhangi bir RESTful API’ye uygulayabilirsiniz.

## <a name="1-create-your-first-azure-ad-app-for-securing-your-web-api"></a>1. Web API’nizin güvenliğini sağlamak için ilk Azure AD uygulamanızı oluşturma

Özel bağlayıcınız API’nizi (bu örnekte Azure Resource Manager API’si) çağırdığında ilk Azure AD uygulamanız kimlik doğrulaması gerçekleştirir.

1. [Azure Portal](https://portal.azure.com) oturum açın. Birden fazla Azure Active Directory kiracınız varsa, kullanıcı adınızın altındaki dizini denetleyerek doğru dizinde oturum açtığınızı doğrulayın. 

   ![Dizininizi doğrulayın](./media/custom-connector-azure-active-directory-authentication/check-user-directory.png)

   > [!TIP]
   > Dizinler arasında geçiş yapmak için kullanıcı adınızı seçip, istediğiniz dizini seçin.

2. Ana Azure menüsünde, geçerli dizininizi görüntülemek için **Azure Active Directory** seçeneğini belirleyin.

   !["Azure Active Directory" seçeneğini belirleyin](./media/custom-connector-azure-active-directory-authentication/azure-active-directory.png)

   > [!TIP]
   > Ana Azure menü görünmüyorsa **Azure Active Directory**, seçin **tüm hizmetleri**. **Filtre** kutusuna filtreniz olarak "Azure Active Directory" yazıp **Azure Active Directory** seçeneğini belirleyin.

3. Dizin menüsünde, **Yönet** bölümünden **Uygulama kayıtları**’nı seçin. Kayıtlı uygulama listesinde **+ Yeni uygulama kaydı**’nı seçin.

   ![“Uygulama kayıtları”, “+ Yeni uygulama kaydı”nı seçin](./media/custom-connector-azure-active-directory-authentication/add-app-registrations.png)

4. **Oluştur** bölümünden tabloda açıklandığı gibi Azure AD uygulamanızın ayrıntılarını sağlayın ve **Oluştur**’u seçin. 

   ![Azure AD uygulaması oluşturma](./media/custom-connector-azure-active-directory-authentication/create-app1-registration.png)

   | Ayar | Önerilen değer | Açıklama | 
   | ------- | --------------- | ----------- | 
   | **Ad** | *web-api-app-name* | Web API’nizin Azure AD uygulamasının adı | 
   | **Uygulama türü** | **Web uygulaması / API’si** | Uygulamanızın türü | 
   | **Oturum Açma URL'si** | `https://login.windows.net` | | 
   |||| 

5. Dizininizin **Uygulama kayıtları** listesine döndüğünüzde Azure AD uygulamanızı seçin.

   ![Listeden Azure AD uygulamanızı seçin](./media/custom-connector-azure-active-directory-authentication/app1-registration-created.png)

6. Uygulama ayrıntıları sayfası göründüğünde **uygulamanın *Uygulama Kimliği* değerini kopyalayıp güvenli bir yere kaydettiğinizden** emin olun. Bu kimliği daha sonra kullanmanız gerekecektir.

   !["Uygulama Kimliği"ni daha sonra kullanmak üzere kaydetme](./media/custom-connector-azure-active-directory-authentication/application-id-app1.png)

7. Şimdi Azure AD uygulamanız için bir yanıt URL’si sağlayın. Uygulamanın **Ayarlar** menüsünde **Yanıtlama URL’lerini** seçin. Bu URL’yi girip **Kaydet**’i seçin.

   ![Yanıt URL'leri](./media/custom-connector-azure-active-directory-authentication/add-reply-url1.png)

   | Ayar | Önerilen değer | Açıklama | 
   | ------- | --------------- | ----------- | 
   | **Yanıt URL'leri** | Kendi API’niz için bu URL'yi girin: </br>`https://{your-web-app-root-URL}/.auth/login/aad/callback` | | 
   | **Temsilcili izinler** | {gerekli değil} | | 
   | **İstemci anahtarı** | {gerekli değil} | | 
   |||| 

   > [!TIP]
   > Daha önce **Ayarlar** menüsü görünmediyse burada **Ayarlar**’ı seçin:
   >
   > ![“Ayarlar”ı seçin](./media/custom-connector-azure-active-directory-authentication/show-app1-settings-menu.png)

## <a name="2-create-your-second-azure-ad-app-for-your-custom-connector"></a>2. Özel bağlayıcınız için ikinci Azure AD uygulamanızı oluşturma

İkinci Azure AD uygulamanız, özel bağlayıcı kaydınızın güvenliğini sağlar ve ilk Azure AD uygulaması tarafından korunan Web API’ye temsilcili erişim ekler. 

> [!IMPORTANT]
> Her iki Azure AD uygulamanızın aynı dizinde olduğundan emin olun.

1. **Uygulama kayıtları** listesine dönüp yeniden **+ Yeni uygulama kaydı**’nı seçin.

   ![“Uygulama kayıtları”, “+ Yeni uygulama kaydı”nı seçin](./media/custom-connector-azure-active-directory-authentication/add-app-registrations2.png)

2. **Oluştur** bölümünden tabloda açıklandığı gibi ikinci Azure AD uygulamanızın ayrıntılarını sağlayın ve **Oluştur**’u seçin. 

   ![Azure AD uygulaması oluşturma](./media/custom-connector-azure-active-directory-authentication/create-app2-registration.png)

   | Ayar | Önerilen değer | Açıklama | 
   | ------- | --------------- | ----------- | 
   | **Ad** | *your-connector-name* | Bağlayıcınızın Azure AD uygulamasının adı | 
   | **Uygulama türü** | **Web uygulaması / API’si** | Uygulamanızın türü | 
   | **Oturum Açma URL'si** | `https://login.windows.net` | | 
   |||| 

3. Dizininizin **Uygulama kayıtları** listesine döndüğünüzde ikinci Azure AD uygulamanızı seçin.

   ![Listeden ikinci Azure AD uygulamanızı seçin](./media/custom-connector-azure-active-directory-authentication/app2-registration-created.png)

4. Uygulama ayrıntıları sayfası göründüğünde **bu uygulamanın da *Uygulama Kimliği* değerini kopyalayıp güvenli bir yere kaydettiğinizden** emin olun. Bu kimliği daha sonra kullanmanız gerekecektir.

   !["Uygulama Kimliği"ni daha sonra kullanmak üzere kaydetme](./media/custom-connector-azure-active-directory-authentication/application-id-app2.png)

5. Şimdi Azure AD uygulamanız için bir yanıt URL’si sağlayın. Uygulamanın **Ayarlar** menüsünde **Yanıtlama URL’lerini** seçin. Bu URL’yi girip **Kaydet**’i seçin.

   | Ayar | Önerilen değer | 
   | ------- | --------------- | 
   | **Yanıt URL'leri** | Azure Resource Manager özel bağlayıcısı için şu URL’yi girin: `https://msmanaged-na.consent.azure-apim.net/redirect` | 
   ||| 

   > [!TIP]
   > Daha önce **Ayarlar** menüsü görünmediyse burada **Ayarlar**’ı seçin:
   >
   > ![“Ayarlar”ı seçin](./media/custom-connector-azure-active-directory-authentication/show-app2-settings-menu.png)

6. **Ayarlar** menüsüne dönerek **Gerekli izinler** > **Ekle**’yi seçin.

   ![Gerekli izinler > Ekle](./media/custom-connector-azure-active-directory-authentication/add-api-access1-select-permissions.png)

7. **API erişimi ekle** menüsü açıldığında **Bir API seçin** > 
**Windows Azure Hizmet Yönetim API'si** > **Seç** seçeneğini belirleyin.

   ![Bir API seçin](./media/custom-connector-azure-active-directory-authentication/add-api-access2-select-api.png)

8. **API erişimi ekle** menüsünde **İzinleri seçin** seçeneğini belirleyin. **Temsilcili izinler** bölümünde **Azure Hizmet Yönetimi'ne kuruluş kullanıcıları olarak erişim** > **Seç** seçeneğini belirleyin.

   ![“Temsilcili izinler” > “Azure Hizmet Yönetimi'ne kuruluş kullanıcıları olarak erişim” seçeneğini belirleyin](./media/custom-connector-azure-active-directory-authentication/add-api-access3-select-permissions.png)

   Aksi takdirde, diğer seçenekler için:

   | Seçenek | Önerilen değer | Açıklama | 
   | ------ | --------------- | ----------- | 
   | **Temsilcili izinler** | | Web API’nize temsilcili erişim için izinleri seçme | 
   |||| 

9. Şimdi, **API erişimi ekle** menüsünde **Bitti** seçeneğini belirleyin.

   ![“API erişimi ekle” menüsü > “Bitti”](./media/custom-connector-azure-active-directory-authentication/add-api-access4-done.png)

10. Şimdi bağlayıcınızın Azure AD uygulaması için bir *istemci anahtarı* veya "gizli anahtar" oluşturun. 

    1. **Ayarlar** menüsüne döndüğünüzde **Anahtarlar**’ı seçin. 
    Anahtarınız için 16 veya daha az karakter içeren bir ad sağlayın, bir sona erme dönemi belirleyin ve **Kaydet**’i seçin.

       ![İstemci anahtarı oluşturma](./media/custom-connector-azure-active-directory-authentication/add-key.png)

    2. Oluşturduğunuz anahtar göründüğünde, **Anahtarlar** sayfasından çıkmadan önce **bu anahtarı kopyalayıp güvenli bir yere kaydettiğinizden** emin olun.
    
       ![Anahtarınızı el ile kopyalama ve kaydetme](./media/custom-connector-azure-active-directory-authentication/save-key.png)

11. Anahtarınızı kaydettikten sonra **Ayarlar** menüsünü güvenli bir şekilde kapatabilirsiniz.

## <a name="3-add-azure-ad-authentication-to-your-web-api-app"></a>3. Web API uygulamanıza Azure AD kimlik doğrulaması ekleme

Şimdi ilk Azure AD uygulamanızla Web API’niz için kimlik doğrulamasını açın. Daha fazla bilgi için bilgi [uygulama hizmeti uygulamanızı Azure Active Directory oturum açma kullanacak şekilde yapılandırmak nasıl](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).

1. İçinde [Azure portal](https://portal.azure.com), daha önce yayımlanan Web API uygulamanızı bulun [Web API'özel Bağlayıcıyı oluşturmak](../logic-apps/custom-connector-build-web-api-app-tutorial.md).

2. **Ayarlar** bölümünden **Kimlik Doğrulama / Yetkilendirme**’yi seçin. 

   1. **App Service Kimlik Doğrulaması** bölümünden **Açık**’ı seçin.
   2. **İsteğin kimliği doğrulanmamış olduğunda gerçekleştirilecek eylem** için **Azure Active Directory ile oturum aç**’ı seçin.
   3. **Kimlik Doğrulama Sağlayıcıları** için **Azure Active Directory**’yi seçin. 

   ![“Kimlik Doğrulama / Yetkilendirme”yi seçin](./media/custom-connector-azure-active-directory-authentication/web-api-app-authentication.png)

3. **Azure Active Directory Ayarları** sayfasında:

   1. **Yönetim modu** bölümünden **Hızlı**’yı seçin.

   2. Şimdi Web API’nizin kimlik doğrulaması için kullandığı Azure AD uygulamasını seçin. 
   **Var Olan AD Uygulamasını Seç** > **Azure AD Uygulaması**’nı seçin.

   3. **Azure AD Uygulamaları** bölümünden, daha önce Web API uygulamanız için oluşturduğunuz Azure AD uygulamasını seçin. 
   
   4. **Kimlik Doğrulama / Yetkilendirme** sayfasına dönene kadar **Tamam**’ı seçin.

   Örneğin:

   ![Web API uygulamanızı temsil eden Azure AD uygulamasını seçin](./media/custom-connector-azure-active-directory-authentication/web-api-app-select-azure-ad-app.png)

4. **Kimlik Doğrulama / Yetkilendirme** sayfasında **Kaydet**’i seçin.

   ![Kimlik doğrulama ayarlarını kaydetme](./media/custom-connector-azure-active-directory-authentication/web-api-app-azure-ad-app-save.png)

   Artık Web API uygulamanız kimlik doğrulaması için Azure AD’yi kullanabilir.

## <a name="4-set-up-azure-ad-authentication-in-your-web-apis-openapi-file"></a>4. Web API'nizin OpenAPI dosyasında Azure AD kimlik doğrulaması ayarlama

`host` özelliği altında `securityDefintions` nesnesini ve Azure AD kimlik doğrulamasını ekleyebilmek için Web API'nizin OpenAPI dosyasını açın. İşte `host` özelliğini ve `securityDefinitions` nesnesini gösteren bir örnek:

``` json
// Your OpenAPI file header appears here...

"host": "{your-web-api-app-root-url}",
"schemes": [
    "https" // Make sure this is https!
],
"securityDefinitions": {
    "AAD": {
        "type": "oauth2",
        "flow": "accessCode",
        "authorizationUrl": "https://login.windows.net/common/oauth2/authorize",
        "tokenUrl": "https://login.windows.net/common/oauth2/token",
        "scopes": {}
    }
},

// Your OpenAPI document continues here...
```
Artık özel bağlayıcınızı oluşturmaya ve kaydetmeye hazırsınız.

## <a name="next-steps"></a>Sonraki adımlar

* [Bağlayıcınızı kaydetme](../logic-apps/logic-apps-custom-connector-register.md)
* [Özel bağlayıcı hakkında SSS](../logic-apps/custom-connector-faq.md)
