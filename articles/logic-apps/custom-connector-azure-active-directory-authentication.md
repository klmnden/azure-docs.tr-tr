---
title: "Azure AD - Azure Logic Apps ile özel bağlayıcılar güvenli | Microsoft Docs"
description: "API ve bağlayıcı Azure Active Directory (Azure AD) ile güvenlik ve kimlik doğrulama ekleme"
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
ms.openlocfilehash: bcee842cb880002daaa3ae9d9110ee1734941b6d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="add-security-to-your-api-and-connector-with-azure-active-directory-azure-ad"></a>API ve bağlayıcı Azure Active Directory (Azure AD) ile güvenlik ekleme

API ve özel bağlayıcı arasındaki aramaları güvenliğini sağlamak için Azure AD kimlik doğrulaması, Web API ve Bağlayıcınızı ekleyin. Bu senaryo için iki Azure Active Directory (Azure AD) uygulamaları oluşturma - diğer Azure AD uygulaması bağlayıcı kaydınızı güvenliğini sağlar ve atanmış erişim ekler çalışırken bir Azure AD uygulaması, Web API güvenliğini sağlar. 

Bu öğretici, Azure Kaynak Yöneticisi API'si örnek olarak kullanır. Azure Resource Manager veritabanları, sanal makineler ve web uygulamaları gibi Azure içinde oluşturuncaya bir çözüm için bileşenlerin yönetmenize yardımcı olur. Akışlarından Azure kaynaklarını yönetmek istediğiniz özel bir bağlayıcı Azure kaynak yöneticisi için yararlı olabilir. Daha fazla bilgi için bkz. [Azure Resource Manager'a Genel Bakış](../azure-resource-manager/resource-group-overview.md).

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Bir aboneliğiniz yoksa ile başlayabilirsiniz bir [ücretsiz Azure hesabı](https://azure.microsoft.com/free/). Aksi takdirde kaydolun bir [Kullandıkça Öde aboneliğine](https://azure.microsoft.com/pricing/purchase-options/).

* Bu öğretici için [örnek OpenAPI dosyası için Azure Resource Manager](https://pwrappssamples.blob.core.windows.net/samples/AzureResourceManager.json)

  > [!NOTE] 
  > Örnek OpenAPI dosyası tüm Azure Resource Manager işlemleri tanımlamıyor ve işlem için yalnızca şu anda içeren [tüm abonelikleri listesinde](https://docs.microsoft.com/rest/api/resources/subscriptions#Subscriptions_List). Bu OpenAPI düzenlemek ya da başka bir OpenAPI dosyası kullanarak oluşturmak [çevrimiçi OpenAPI Düzenleyicisi](http://editor.swagger.io/).
  >
  > Bu öğretici RESTful tüm Azure AD kimlik doğrulaması kullanmak istediğiniz API'sine uygulayabilirsiniz.

## <a name="1-create-your-first-azure-ad-app-for-securing-your-web-api"></a>1. Web API güvenliğini sağlamak için ilk Azure AD uygulaması oluştur

Özel Bağlayıcınızı Azure Resource Manager API Bu örnekte, API çağırdığında ilk Azure AD uygulamanıza kimlik doğrulaması gerçekleştirir.

1. [Azure Portal](https://portal.azure.com) oturum açın. Birden fazla Azure Active Directory Kiracı varsa, doğru dizinine kullanıcı adınızı dizininde denetleyerek oturum açtınız olduğunu onaylayın. 

   ![Dizininizi onaylayın](./media/custom-connector-azure-active-directory-authentication/check-user-directory.png)

   > [!TIP]
   > Dizinleri değiştirmek için kullanıcı adınızı seçin, sonra istediğiniz dizini seçin.

2. Ana Azure menüsünde, **Azure Active Directory** geçerli dizininiz görmesine olanak tanır.

   !["Azure Active Directory" seçin](./media/custom-connector-azure-active-directory-authentication/azure-active-directory.png)

   > [!TIP]
   > Ana Azure menü görünmüyorsa **Azure Active Directory**, seçin **daha fazla hizmet**. İçinde **filtre** kutusunda, filtre "Azure Active Directory" yazın ve ardından seçin **Azure Active Directory**.

3. Dizin menüsünde altında **Yönet**, seçin **uygulama kayıtlar**. Kayıtlı uygulamalar listesinde seçin **+ yeni uygulama kaydı**.

   !["Uygulama kayıtlar", "+ Yeni uygulama kaydı"'ı seçin.](./media/custom-connector-azure-active-directory-authentication/add-app-registrations.png)

4. Altında **oluşturma**, tabloda açıklandığı gibi Azure AD uygulama için ayrıntıları belirtin ve ardından **oluşturma**. 

   ![Azure AD uygulaması oluştur](./media/custom-connector-azure-active-directory-authentication/create-app1-registration.png)

   | Ayar | Önerilen değer | Açıklama | 
   | ------- | --------------- | ----------- | 
   | **Ad** | *Web API uygulamanızın adı* | Web API Azure AD uygulaması adı | 
   | **Uygulama türü** | **Web uygulaması / API** | Uygulamanızın türü | 
   | **Oturum açma URL'si** | `https://login.windows.net` | | 
   |||| 

5. Dizinin döndüğünüzde **uygulama kayıtlar** listesinde, Azure AD uygulaması seçin.

   ![Azure AD uygulaması listeden seçin](./media/custom-connector-azure-active-directory-authentication/app1-registration-created.png)

6. Uygulamanın Ayrıntılar sayfası görüntülendiğinde, emin olun, **kopyalayın ve uygulamanın kaydedin *uygulama kimliği* güvenli bir yerde**. Daha sonra kullanmak üzere bu kimliği gerekir.

   ![Daha sonra kullanmak için "Uygulama kimliği" Kaydet](./media/custom-connector-azure-active-directory-authentication/application-id-app1.png)

7. Şimdi, Azure AD uygulama için bir yanıt URL'si belirtin. Uygulamasının **ayarları** menüsünde seçin **yanıt URL'leri**. Bu URL'yi girin ve ardından **kaydetmek**.

   ![Yanıt URL'leri](./media/custom-connector-azure-active-directory-authentication/add-reply-url1.png)

   | Ayar | Önerilen değer | Açıklama | 
   | ------- | --------------- | ----------- | 
   | **Yanıt URL'leri** | Kendi API için bu URL'yi girin: </br>`https://{your-web-app-root-URL}/.auth/login/aad/callback` | | 
   | **Temsilci izinleri** | {gerekli olmayan} | | 
   | **İstemci anahtarı** | {gerekli olmayan} | | 
   |||| 

   > [!TIP]
   > Varsa **ayarları** menüsünde kaydetmedi önceden görünür, seçin **ayarları** burada:
   >
   > !["Ayarlar"'ı seçin](./media/custom-connector-azure-active-directory-authentication/show-app1-settings-menu.png)

## <a name="2-create-your-second-azure-ad-app-for-your-custom-connector"></a>2. Özel Bağlayıcınızı için ikinci, Azure AD uygulaması oluştur

İkinci Azure AD uygulamanızı özel bağlayıcı kaydınızı güvenliğini sağlar ve İlk Azure AD uygulama tarafından korunan Web API atanmış erişim ekler. 

> [!IMPORTANT]
> Her iki, Azure AD uygulamalarınız aynı dizinde bulunduğundan emin olun.

1. Geri dönüp **uygulama kayıtlar** listesinde ve seçin **+ yeni uygulama kaydı** yeniden.

   !["Uygulama kayıtlar", "+ Yeni uygulama kaydı"'ı seçin.](./media/custom-connector-azure-active-directory-authentication/add-app-registrations2.png)

2. Altında **oluşturma**, tabloda açıklandığı gibi ikinci Azure AD uygulamanız için ayrıntıları sağlayın, sonra seçin **oluşturma**. 

   ![Azure AD uygulaması oluştur](./media/custom-connector-azure-active-directory-authentication/create-app2-registration.png)

   | Ayar | Önerilen değer | Açıklama | 
   | ------- | --------------- | ----------- | 
   | **Ad** | *Bağlayıcı adı bilgisayarınızı* | Bağlantı ucunun Azure AD uygulaması adı | 
   | **Uygulama türü** | **Web uygulaması / API** | Uygulamanızın türü | 
   | **Oturum açma URL'si** | `https://login.windows.net` | | 
   |||| 

3. Dizinin döndüğünüzde **uygulama kayıtlar** listesinde, ikinci Azure AD uygulamanızı seçin.

   ![İkinci Azure AD uygulaması listeden seçin](./media/custom-connector-azure-active-directory-authentication/app2-registration-created.png)

4. Uygulamanın Ayrıntılar sayfası görüntülendiğinde, emin olun, ayrıca **kopyalayın ve bu uygulamanın kaydedin *uygulama kimliği* güvenli bir yerde çok**. Daha sonra kullanmak üzere bu kimliği gerekir.

   ![Daha sonra kullanmak için "Uygulama kimliği" Kaydet](./media/custom-connector-azure-active-directory-authentication/application-id-app2.png)

5. Şimdi, Azure AD uygulama için bir yanıt URL'si belirtin. Uygulamasının **ayarları** menüsünde seçin **yanıt URL'leri**. Bu URL'yi girin ve ardından **kaydetmek**.

   | Ayar | Önerilen değer | 
   | ------- | --------------- | 
   | **Yanıt URL'leri** | Azure Resource Manager özel connector için bu URL'yi girin:`https://msmanaged-na.consent.azure-apim.net/redirect` | 
   ||| 

   > [!TIP]
   > Varsa **ayarları** menüsünde kaydetmedi önceden görünür, seçin **ayarları** burada:
   >
   > !["Ayarlar"'ı seçin](./media/custom-connector-azure-active-directory-authentication/show-app2-settings-menu.png)

6. Geri **ayarları** menüsünde seçin **gerekli izinleri** > **Ekle**.

   ![Gerekli izinler > Ekle](./media/custom-connector-azure-active-directory-authentication/add-api-access1-select-permissions.png)

7. Zaman **API Ekle erişim** menüsünü açar, seçin **bir API seçin** > 
**Windows Azure Hizmet Yönetimi API'si** > **seçin **.

   ![Bir API seçin](./media/custom-connector-azure-active-directory-authentication/add-api-access2-select-api.png)

8. Üzerinde **API Ekle erişim** menüsünde seçin **seçin izinleri**. Altında **izinleri atanmış**, seçin **erişim Azure Hizmet Yönetimi kuruluş kullanıcılar olarak** > **seçin**.

   !["Atanan izinleri" seçin > "erişim kuruluş kullanıcı olarak Azure Hizmetleri Yönetimi"](./media/custom-connector-azure-active-directory-authentication/add-api-access3-select-permissions.png)

   Aksi takdirde, diğer seçenekler için:

   | Seçenek | Önerilen değer | Açıklama | 
   | ------ | --------------- | ----------- | 
   | **Temsilci izinleri** | | Web apı'nize atanmış erişim izinlerini seçin | 
   |||| 

9. Şimdi üzerinde **API Ekle erişim** menüsünde seçin **Bitti**.

   ![Menü "API erişimini Ekle" > "Tamamlandı"](./media/custom-connector-azure-active-directory-authentication/add-api-access4-done.png)

10. Şimdi oluşturmak bir *istemci anahtar*, ya da "gizli", bağlayıcı'nın Azure AD uygulaması için. 

    1. Geri **ayarları** menüsünde seçin **anahtarları**. 
    16 veya daha az karakterli anahtarınız için ad, sona erme dönemini seçin ve ardından **kaydetmek**.

       ![Bir istemci anahtarı oluşturun](./media/custom-connector-azure-active-directory-authentication/add-key.png)

    2. Oluşturulan anahtarınızı göründüğünde, **kopyalayın ve bu anahtarı güvenli bir yerde kaydedin emin olun** çıkmadan önce **anahtarları** sayfası.
    
       ![El ile kopyalayın ve anahtarınızı kaydedin](./media/custom-connector-azure-active-directory-authentication/save-key.png)

11. Anahtarınızı kaydedildikten sonra güvenli bir şekilde kapatabilir **ayarları** menüsü.

## <a name="3-add-azure-ad-authentication-to-your-web-api-app"></a>3. Web API uygulamanızı Azure AD kimlik doğrulaması ekleme

Şimdi, Web API, ilk Azure AD uygulaması ile kimlik doğrulamasını etkinleştirin. Daha fazla bilgi için bilgi [uygulama hizmeti uygulamanızı Azure Active Directory oturum açma kullanacak şekilde yapılandırmak nasıl](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).

1. İçinde [Azure portal](https://portal.azure.com), daha önce yayımlanan Web API uygulamanızı bulun [Web API'özel Bağlayıcıyı oluşturmak](../logic-apps/custom-connector-build-web-api-app-tutorial.md).

2. Altında **ayarları**, seçin **kimlik doğrulama / yetkilendirme**. 

   1. Altında **App Service kimlik doğrulaması**, seçin **üzerinde**.
   2. İçin **isteğin kimliği doğrulanmamış olduğunda gerçekleştirilecek eylem**seçin **Azure Active Directory ile oturum aç**.
   3. İçin **kimlik doğrulama sağlayıcıları**seçin **Azure Active Directory**. 

   ![Seçin "kimlik doğrulama / yetkilendirme"](./media/custom-connector-azure-active-directory-authentication/web-api-app-authentication.png)

3. Üzerinde **Azure Active Directory ayarları** sayfa:

   1. Altında **yönetim modu**, seçin **Express**.

   2. Şimdi Web API uygulamanız için kimlik doğrulaması kullanan Azure AD uygulaması seçin. 
   Seçin **var olan AD uygulamasını Seç** > **Azure AD uygulaması**.

   3. Altında **Azure AD uygulamaları**, daha önce Web API uygulamanız için oluşturduğunuz Azure AD uygulaması seçin. 
   
   4. Seçin **Tamam** geri kadar **kimlik doğrulama / yetkilendirme** sayfası.

   Örneğin:

   ![Web API uygulamanızı temsil eden Azure AD uygulaması seçin](./media/custom-connector-azure-active-directory-authentication/web-api-app-select-azure-ad-app.png)

4. Üzerinde **kimlik doğrulama / yetkilendirme** sayfasında, **kaydetmek**.

   ![Kimlik doğrulama ayarlarını Kaydet](./media/custom-connector-azure-active-directory-authentication/web-api-app-azure-ad-app-save.png)

   Şimdi Web API uygulamanızı Azure AD kimlik doğrulaması için kullanabilirsiniz.

## <a name="4-set-up-azure-ad-authentication-in-your-web-apis-openapi-file"></a>4. Azure AD kimlik doğrulaması Web API'nin OpenAPI dosyasında ayarlanan

Ekleyebilirsiniz böylece Web API uygulamanızın OpenAPI dosyasını açın `securityDefintions` nesne ve Azure AD kimlik doğrulamasında `host` özelliği. İşte gösteren bir örnek `host` özelliği ve `securityDefinitions` nesnesi:

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
Artık oluşturmak ve özel Bağlayıcınızı kaydetmek hazırsınız.

## <a name="next-steps"></a>Sonraki adımlar

* [Bağlayıcınızı kaydetme](../logic-apps/logic-apps-custom-connector-register.md)
* [Özel bağlayıcı SSS](../logic-apps/custom-connector-faq.md)
