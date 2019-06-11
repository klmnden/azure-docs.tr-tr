---
title: Özel API - Azure Logic Apps için kimlik doğrulaması ekleme | Microsoft Docs
description: Azure Logic Apps'ten özel API'leri çağırmak için kimlik doğrulaması ayarlama
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 09/22/2017
ms.openlocfilehash: 555083235aff08476e82f0daa81203b66591f3cc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66167390"
---
# <a name="secure-calls-to-custom-apis-from-azure-logic-apps"></a>Azure mantıksal uygulamalardan özel API'lere giden çağrıların güvenliğini sağlama

Kodunuzu güncelleştirmek zorunda kalmamak için API'lere giden çağrıların güvenliğini sağlamak için Azure portalı üzerinden Azure Active Directory (Azure AD) kimlik ayarlayabilirsiniz. İsterseniz, API'nizin kodu aracılığıyla kimlik doğrulamasının gerekli ve zorunlu olmasını da sağlayabilirsiniz.

## <a name="authentication-options-for-your-api"></a>API'niz için kimlik doğrulama seçenekleri

Özel API'nizi aşağıdaki şekillerde çağrıları güvenli hale getirebilirsiniz:

* [Kod değişikliği yapmadan](#no-code): İle API'nizi koruma [Azure Active Directory (Azure AD)](../active-directory/fundamentals/active-directory-whatis.md) Azure portalı üzerinden, bu nedenle yazmanız gerekmez kodunuzu güncelleştirin veya API'nizi yeniden dağıtın.

  > [!NOTE]
  > Varsayılan olarak, hassas yetkilendirme Azure Portalı'nda açmanız Azure AD kimlik doğrulaması sağlamaz. Örneğin, bu kimlik doğrulaması yalnızca belirli bir kiracıda değil, belirli bir kullanıcı veya uygulama API'nizi kilitler. 

* [API'NİZİN kodu güncelleştirmeniz](#update-code): Zorlayarak API'nizi koruma [sertifika kimlik doğrulaması](#certificate), [temel kimlik doğrulaması](#basic), veya [Azure AD kimlik doğrulaması](#azure-ad-code) kod aracılığıyla.

<a name="no-code"></a>

### <a name="authenticate-calls-to-your-api-without-changing-code"></a>API çağrıları kodunu değiştirmeden kimlik doğrulaması

Bu yöntem için genel adımlar şunlardır:

1. İki Azure Active Directory (Azure AD) uygulama kimlikleri oluşturma: bir mantıksal uygulamanız, biri web uygulamanızı (veya API uygulaması).

2. Apı'nize çağrı kimliğini doğrulamak için mantıksal uygulamanız için Azure AD uygulama kimliği ile ilişkili hizmet sorumlusunun kimlik bilgilerini (istemci kimliği ve parolası) kullanın.

3. Mantıksal uygulama tanımınızı uygulama kimliklerini içerir.

#### <a name="part-1-create-an-azure-ad-application-identity-for-your-logic-app"></a>1. Bölüm: Mantıksal uygulamanız için bir Azure AD uygulama kimliği oluşturma

Mantıksal uygulamanızı karşı Azure AD kimlik doğrulaması için bu Azure AD uygulama kimliğini kullanır. Yalnızca bir kez dizininiz için bu kimlik ayarlamak zorunda. Örneğin, her mantıksal uygulama için benzersiz bir kimlik oluşturabilirsiniz olsa bile, tüm logic apps için aynı kimlik kullanmayı da tercih edebilirsiniz. Azure portalında bu kimlikleri ayarlayın veya kullanın [PowerShell](#powershell).

**Azure portalında mantıksal uygulamanız için uygulama kimliği oluşturma**

1. İçinde [Azure portalında](https://portal.azure.com "https://portal.azure.com"), seçin **Azure Active Directory**. 

2. Web uygulamanızı veya API uygulaması olarak aynı dizinde olduğunuzdan emin olun.

   > [!TIP]
   > Dizinleri Değiştir için profilinizi seçin ve başka bir dizin seçin. Ya da seçin **genel bakış** > **dizini Değiştir**.

3. Dizin menüsünde altında **Yönet**, seçin **uygulama kayıtları** > **yeni uygulama kaydı**.

   > [!TIP]
   > Varsayılan olarak, uygulama kayıtları listesi, dizininizdeki tüm uygulama kayıtlarını gösterir. Arama kutusunun yanındaki yalnızca, uygulama kayıtları görüntülemek için seçin **uygulamalarım**. 

   ![Yeni uygulama kaydı oluşturma](./media/logic-apps-custom-api-authentication/new-app-registration-azure-portal.png)

4. Uygulama kimliğinizi bir ad verin, bırakın **uygulama türü** kümesine **Web uygulaması / API**, sağlayan benzersiz bir biçimlendirilmiş dize için bir etki alanı olarak **oturum açma URL'si**, seçin **Oluşturma**.

   ![URL için uygulama kimliği oturum adı girin ve](./media/logic-apps-custom-api-authentication/logic-app-identity-azure-portal.png)

   Şimdi mantıksal uygulamanız için oluşturduğunuz uygulama kimliği, uygulama kayıtları listesi görüntülenir.

   ![Mantıksal uygulamanız için uygulama kimliği](./media/logic-apps-custom-api-authentication/logic-app-identity-created.png)

5. Uygulama kayıtları listesi, yeni uygulama kimliğini seçin. Kopyalayıp kaydedin **uygulama kimliği** mantıksal uygulamanızda 3. kısım "İstemci kimliği" kullanılacak.

   ![Kopyalayın ve mantıksal uygulama için uygulama kimliği kaydedin](./media/logic-apps-custom-api-authentication/logic-app-application-id.png)

6. Uygulama Kimliği ayarlarınızı görünmüyorsa seçin **ayarları** veya **tüm ayarlar**.

7. Altında **API erişimi**, seçin **anahtarları**. Altında **açıklama**, anahtarınız için bir ad sağlayın. Altında **Expires**, anahtarınız için bir süre seçin.

   Oluşturmakta olduğunuz anahtarı, uygulama kimliğin "gizli dizisini" ya da mantıksal uygulamanızın parola olarak görev yapar.

   ![Mantıksal uygulama kimliği için anahtar oluşturma](./media/logic-apps-custom-api-authentication/create-logic-app-identity-key-secret-password.png)

8. Araç çubuğunda **Kaydet**. Altında **değer**, anahtarınızı görünür. 
**Kopyala ve anahtarınızı kaydetmek emin olun** daha sonra kullanmak üzere anahtarı gizli olduğu çıktığınızda **anahtarları** sayfası.

   3. Kısım mantıksal uygulamanızı yapılandırdığınızda, bu anahtarı parola ya da "gizli" olarak belirtin.

   ![Kopyalayıp anahtarı daha sonra kullanmak üzere kaydedin](./media/logic-apps-custom-api-authentication/logic-app-copy-key-secret-password.png)

<a name="powershell"></a>

**PowerShell'de mantıksal uygulamanız için uygulama kimliği oluşturma**

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

PowerShell ile Azure Resource Manager aracılığıyla bu görevi gerçekleştirebilirsiniz. PowerShell'de şu komutları çalıştırın:

1. `Add-AzAccount`

2. `$SecurePassword = Read-Host -AsSecureString` (Bir parola girin ve ENTER tuşuna basın)

3. `New-AzADApplication -DisplayName "MyLogicAppID" -HomePage "http://mydomain.tld" -IdentifierUris "http://mydomain.tld" -Password $SecurePassword`

4. Kopyaladığınızdan emin olun **Kiracı kimliği** (GUID), Azure AD kiracınız için **uygulama kimliği**ve parolayı.

Daha fazla bilgi için bilgi nasıl [kaynaklarına erişmek için PowerShell ile hizmet sorumlusu oluşturma](../active-directory/develop/howto-authenticate-service-principal-powershell.md).

#### <a name="part-2-create-an-azure-ad-application-identity-for-your-web-app-or-api-app"></a>2. Bölüm: Web uygulamanızı veya API uygulaması için bir Azure AD uygulama kimliği oluşturma

Web uygulamanızı veya API uygulaması zaten dağıtılmışsa, kimlik doğrulamasını etkinleştirmek ve Azure portalında uygulama kimliği oluşturun. Aksi takdirde [bir Azure Resource Manager şablonu ile dağıttığınızda kimlik doğrulamasını etkinleştirmek](#authen-deploy). 

**Uygulama Kimliği oluşturma ve kimlik doğrulaması dağıtılan uygulamalar için Azure portalında açın**

1. İçinde [Azure portalında](https://portal.azure.com "https://portal.azure.com")bulup web uygulamanızı veya API uygulaması seçin. 

2. Altında **ayarları**, seçin **kimlik doğrulama/yetkilendirme**. Altında **App Service kimlik doğrulaması**, kimlik doğrulaması kapatma **üzerinde**. Altında **kimlik doğrulama sağlayıcıları**, seçin **Azure Active Directory**.

   ![Kimlik doğrulamasını etkinleştirmek](./media/logic-apps-custom-api-authentication/custom-web-api-app-authentication.png)

3. Şimdi burada gösterildiği gibi bir uygulama kimliği, web veya API uygulaması oluşturun. Üzerinde **Azure Active Directory ayarları** sayfasında **yönetim modu** için **Express**. Seçin **yeni AD uygulaması oluştur**. Uygulama kimliğinizi bir ad verin ve seçin **Tamam**. 

   ![Web uygulamanızı veya API uygulaması için uygulama kimliği oluşturma](./media/logic-apps-custom-api-authentication/custom-api-application-identity.png)

4. **Kimlik Doğrulama / Yetkilendirme** sayfasında **Kaydet**’i seçin.

Artık web uygulamanızı veya API uygulaması ile ilişkili uygulama kimliği için istemci kimliği ve Kiracı kimliği bulmanız gerekir. 3. Kısım'de bu kimliği kullanın. Bu nedenle bu adımları Azure portalı ile devam edin.

**Kimliğin uygulama istemci kimliği ve Kiracı kimliği, web uygulamanızı veya API uygulaması için Azure portalında bulun**

1. Altında **kimlik doğrulama sağlayıcıları**, seçin **Azure Active Directory**. 

   !["Azure Active Directory" seçeneğini belirleyin](./media/logic-apps-custom-api-authentication/custom-api-app-identity-client-id-tenant-id.png)

2. Üzerinde **Azure Active Directory ayarları** sayfasında **yönetim modu** için **Gelişmiş**.

3. Kopyalama **istemci kimliği**ve 3. Kısım GUID kullanmak için kaydedin.

   > [!TIP] 
   > Varsa **istemci kimliği** ve **veren URL'si** yoksa, görünür, Azure portalı yenilemeyi deneyin ve 1. adımı yineleyin.

4. Altında **veren URL'si**kopyalayın ve 3. kısım için GUID kaydedin. Bu GUID web uygulamanızı veya API uygulamasının dağıtım şablonu, gerekirse kullanabilirsiniz.

   Bu GUID, belirli bir kiracının GUID ("Kiracı kimliği") ve bu URL'de görünmesi gerekir: `https://sts.windows.net/{GUID}`

5. Değişikliklerinizi kaydetmeden kapatmak **Azure Active Directory ayarları** sayfası.

<a name="authen-deploy"></a>

**Bir Azure Resource Manager şablonu ile dağıttığınızda kimlik doğrulamasını etkinleştirmek**

Yine de uygulama kimliğinin mantıksal uygulamanız için web uygulaması ya da farklı bir API uygulaması için bir Azure AD uygulama kimliği oluşturmalısınız. Uygulama kimliği oluşturmak için 2. bölüm önceki adımlarda Azure portalın izleyin. 

Ayrıca, bölüm 1 adımları izleyerek ancak web uygulamanızı veya API uygulamasının gerçek kullandığınızdan emin olun `https://{URL}` için **oturum açma URL'si** ve **uygulama kimliği URI'si**. Bu adımları, istemci kimliği ve Kiracı kimliği, uygulamanızın dağıtım şablonu kullanmak için ve ayrıca 3. Kısım kaydetmek gerekir.

> [!NOTE]
> Azure AD uygulama kimliği, web veya API uygulaması oluşturduğunuzda, Azure portalı, PowerShell değil kullanmanız gerekir. PowerShell komutu, kullanıcıların bir Web sitesine oturum açmak için gerekli izinleri ayarlamaz.

İstemci kimliği ve Kiracı kimliği aldıktan sonra web uygulaması veya API uygulaması için dağıtım şablonunda bir alt kaynak olarak bu kimlikleri şunları içerir:

``` json
"resources": [ {
    "apiVersion": "2015-08-01",
    "name": "web",
    "type": "config",
    "dependsOn": ["[concat('Microsoft.Web/sites/','parameters('webAppName'))]"],
    "properties": {
        "siteAuthEnabled": true,
        "siteAuthSettings": {
            "clientId": "{client-ID}",
            "issuer": "https://sts.windows.net/{tenant-ID}/",
        }
    }
} ]
```

Bir boş web uygulaması ve bir mantıksal uygulama Azure Active Directory kimlik doğrulaması ile birlikte otomatik olarak dağıtılacak [şablonun tamamını burada görüntülemek](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-custom-api/azuredeploy.json), veya **azure'a Dağıt** burada:

[![Azure’a dağıtma](media/logic-apps-custom-api-authentication/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-custom-api%2Fazuredeploy.json)

#### <a name="part-3-populate-the-authorization-section-in-your-logic-app"></a>3. Bölüm: Mantıksal uygulamanızı yetkilendirme bölümünde Doldur

Önceki şablon zaten ayarlanmış bu yetkilendirme bölüm vardır, ancak doğrudan mantıksal uygulama yazıyorsanız, tam yetkilendirme bölümü içermelidir.

Açık mantıksal uygulama tanımınızı kod Görünümü'nde Git **HTTP** eylem bölümü bulun **yetkilendirme** bölümünde ve bu satırı ekleyin:

`{"tenant": "{tenant-ID}", "audience": "{client-ID-from-Part-2-web-app-or-API app}", "clientId": "{client-ID-from-Part-1-logic-app}", "secret": "{key-from-Part-1-logic-app}", "type": "ActiveDirectoryOAuth" }`

| Öğe | Gerekli | Açıklama | 
| ------- | -------- | ----------- | 
| tenant | Evet | Azure AD kiracısı için GUID | 
| audience | Evet | İstemci kimlik, web veya API uygulaması için uygulama kimliği, erişmek istediğiniz hedef kaynağı için GUID | 
| clientId | Evet | Uygulama Kimliği mantıksal uygulamanızın istemci kimliği olan erişim isteğinde bulunan istemci için GUID | 
| secret | Evet | Anahtar veya parolayı erişim belirteci istenirken istemci uygulama kimliği | 
| type | Evet | Kimlik doğrulaması türü. ActiveDirectoryOAuth kimlik doğrulaması için değerdir `ActiveDirectoryOAuth`. | 
|||| 

Örneğin:

``` json
{
   "actions": {
      "some-action": {
         "conditions": [],
         "inputs": {
            "method": "post",
            "uri": "https://your-api-azurewebsites.net/api/your-method",
            "authentication": {
               "tenant": "tenant-ID",
               "audience": "client-ID-from-azure-ad-app-for-web-app-or-api-app",
               "clientId": "client-ID-from-azure-ad-app-for-logic-app",
               "secret": "key-from-azure-ad-app-for-logic-app",
               "type": "ActiveDirectoryOAuth"
            }
         },
      }
   }
}
```

<a name="update-code"></a>

### <a name="secure-api-calls-through-code"></a>Kod aracılığıyla güvenli API çağrıları

<a name="certificate"></a>

#### <a name="certificate-authentication"></a>Sertifika kimlik doğrulaması

Web uygulamanızı veya API uygulaması için mantıksal uygulamadan gelen istekleri doğrulamak için istemci sertifikaları kullanabilirsiniz. Kodunuzu oluşturan ayarlamak için bilgi [TLS karşılıklı kimlik doğrulamayı yapılandırma](../app-service/app-service-web-configure-tls-mutual-auth.md).

İçinde **yetkilendirme** bölümünde, bu satırı ekleyin: 

`{"type": "clientcertificate", "password": "password", "pfx": "long-pfx-key"}`

| Öğe | Gerekli | Açıklama | 
| ------- | -------- | ----------- | 
| type | Evet | Kimlik doğrulaması türü. SSL istemci sertifikaları için bir değer olmalıdır `ClientCertificate`. | 
| password | Evet | İstemci sertifikası (PFX dosyası) erişmek için parola | 
| PFX | Evet | İstemci sertifikası (PFX dosyası) base64 ile kodlanmış içeriği | 
|||| 

<a name="basic"></a>

#### <a name="basic-authentication"></a>Temel kimlik doğrulaması

Web uygulamanızı veya API uygulaması, mantıksal uygulamadan gelen istekleri doğrulamak için bir kullanıcı adı ve parola gibi temel kimlik doğrulaması kullanabilirsiniz. Temel kimlik doğrulaması ortak desendir ve web uygulamanızı veya API uygulaması oluşturmak için kullanılan herhangi bir dilde bu kimlik doğrulaması kullanabilirsiniz.

İçinde **yetkilendirme** bölümünde, bu satırı ekleyin:

`{"type": "basic", "username": "username", "password": "password"}`.

| Öğe | Gerekli | Açıklama | 
| ------- | -------- | ----------- | 
| type | Evet | Kullanmak istediğiniz kimlik doğrulaması türü. Temel kimlik doğrulaması için bir değer olmalıdır `Basic`. | 
| username | Evet | Kimlik doğrulaması için kullanmak istediğiniz kullanıcı adı | 
| password | Evet | Kimlik doğrulaması için kullanmak istediğiniz parolayı | 
|||| 

<a name="azure-ad-code"></a>

#### <a name="azure-active-directory-authentication-through-code"></a>Kod aracılığıyla Azure Active Directory kimlik doğrulaması

Varsayılan olarak, hassas yetkilendirme Azure Portalı'nda açmanız Azure AD kimlik doğrulaması sağlamaz. Örneğin, bu kimlik doğrulaması yalnızca belirli bir kiracıda değil, belirli bir kullanıcı veya uygulama API'nizi kilitler. 

Kod aracılığıyla mantıksal uygulamanız için API erişimini kısıtlamak için JSON web token (JWT) olan üst ayıkla. Çağıranının kimliğini denetleyin ve eşleşmeyen istekleri reddet.

<!-- Going further, to implement this authentication entirely in your own code, 
and not use the Azure portal, learn how to 
[authenticate with on-premises Active Directory in your Azure app](../app-service/overview-authentication-authorization.md).

To create an application identity for your logic app and use that identity to call your API, 
you must follow the previous steps. -->

## <a name="next-steps"></a>Sonraki adımlar

* [Dağıtma ve özel API'ler, uygulama iş akışları mantığından çağırma](../logic-apps/logic-apps-custom-api-host-deploy-call.md)
