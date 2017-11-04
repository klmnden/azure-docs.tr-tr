---
title: "Kimlik doğrulaması özel API'leri - Azure mantıksal uygulamaları ekleme | Microsoft Docs"
description: "Kimlik doğrulaması mantığı uygulamalardan özel API çağrıları için ayarlama"
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
ms.openlocfilehash: 2528f4318d92bbfdc1008795876f0240a5e3e4f6
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="secure-calls-to-your-custom-apis-from-logic-apps"></a>Özel Apı'lerinizi mantığı uygulamalardan güvenli çağrılar

Kodunuzu güncelleştirmek zorunda kalmamak için API çağrıları güvenli hale getirmek için Azure Portalı aracılığıyla Azure Active Directory (Azure AD) kimlik doğrulaması ayarlayabilirsiniz. Veya gerektirir ve kimlik doğrulama, API'nin kodlarda zorlayın.

## <a name="authentication-options-for-your-api"></a>API'nizi için kimlik doğrulama seçenekleri

Bu şekilde, özel API çağrıları güvenliğini sağlayabilirsiniz:

* [Kod değişiklikleri](#no-code): API'nizi ile koruma [Azure Active Directory (Azure AD)](../active-directory/active-directory-whatis.md) Azure portalı üzerinden bu nedenle, kodunuzu güncelleştirin veya API'nizi yeniden dağıtmak zorunda değilsiniz.

  > [!NOTE]
  > Varsayılan olarak, Azure portalında kapatma Azure AD kimlik doğrulaması hassas yetkilendirme sağlamaz. Örneğin, bu kimlik doğrulama API'nizi yalnızca belirli bir kiracı için değil, belirli bir kullanıcı veya uygulama kilitler. 

* [API kodunu güncelleştirin](#update-code): zorlayarak API'nizi koruma [sertifika kimlik doğrulaması](#certificate), [temel kimlik doğrulaması](#basic), veya [Azure AD kimlik doğrulaması](#azure-ad-code) kodlarda.

<a name="no-code"></a>

### <a name="authenticate-calls-to-your-api-without-changing-code"></a>Kodunu değiştirmeden, API çağrıları kimlik doğrulaması

Bu yöntem için genel adımlar şunlardır:

1. İki Azure Active Directory (Azure AD) uygulama kimlikleri oluşturma: mantıksal uygulamanızı, diğeri web uygulaması (veya API uygulaması) için.

2. API çağrıları kimliğini doğrulamak için kimlik bilgilerini (istemci kimliği ve parolası) mantıksal uygulamanız için Azure AD uygulama kimliği ile ilişkili hizmet asıl kullanın.

3. Uygulama kimlikleri mantığı uygulama tanımınızı içerir.

#### <a name="part-1-create-an-azure-ad-application-identity-for-your-logic-app"></a>1. Kısım: mantıksal uygulamanız için bir Azure AD uygulama kimliği oluşturma

Mantıksal uygulamanızı Azure AD karşı kimlik doğrulaması için bu Azure AD uygulama kimliğini kullanır. Yalnızca bu kimliği dizininiz için bir kez ayarlamanız gerekir. Örneğin, her mantıksal uygulama için benzersiz kimlik Oluştur olsa bile, tüm mantıksal uygulamalar için aynı kimlik kullanmayı seçebilirsiniz. Azure portalında bu kimlikleri ayarlayın veya kullanmak [PowerShell](#powershell).

**Azure portalda mantıksal uygulamanızı için uygulama kimliği oluşturma**

1. İçinde [Azure portal](https://portal.azure.com "https://portal.azure.com"), seçin **Azure Active Directory**. 

2. Web uygulaması veya API uygulaması olarak aynı dizinde olduğunuzu doğrulayın.

   > [!TIP]
   > Dizinleri geçmek için profili seçin ve başka bir dizin seçin. Ya da seçin **genel bakış** > **anahtar dizini**.

3. Dizin menüsünde altında **Yönet**, seçin **uygulama kayıtlar** > **yeni uygulama kaydı**.

   > [!TIP]
   > Varsayılan olarak, dizininizdeki tüm uygulama kayıtları uygulama kayıtlar listesi gösterilir. Yalnızca, uygulama kayıtlar yanındaki Arama kutusunu görüntülemek için seçin **uygulamalarım**. 

   ![Yeni uygulama kaydı oluşturun](./media/logic-apps-custom-api-authentication/new-app-registration-azure-portal.png)

4. Uygulama kimliğinizi bir ad verin, bırakın **uygulama türü** kümesine **Web uygulaması / API**, benzersiz bir sağlamak için bir etki alanı olarak biçimlendirilmiş dize **oturum açma URL'si**ve seçin **Oluştur**.

   ![URL için uygulama kimliği oturum adı girin ve](./media/logic-apps-custom-api-authentication/logic-app-identity-azure-portal.png)

   Mantıksal uygulamanız artık oluşturduğunuz uygulama kimliğini uygulama kayıtlar listesinde görüntülenir.

   ![Mantıksal uygulamanız için uygulama kimliği](./media/logic-apps-custom-api-authentication/logic-app-identity-created.png)

5. Uygulama kayıtlar listesinde, yeni uygulama kimliğini seçin. Kopyalayıp kaydedin **uygulama kimliği** mantıksal uygulamanızı bölümü 3'te "istemci kimlik" olarak kullanılmak üzere.

   ![Kopyalayın ve mantıksal uygulama için uygulama kimliği kaydedin](./media/logic-apps-custom-api-authentication/logic-app-application-id.png)

6. Uygulama Kimliği ayarlarınızı görünür durumda değilse, seçin **ayarları** veya **tüm ayarları**.

7. Altında **API erişimini**, seçin **anahtarları**. Altında **açıklama**, anahtarınız için bir ad sağlayın. Altında **Expires**, anahtarınız için bir süre seçin.

   Oluşturmakta olduğunuz anahtar uygulama kimliğin "gizli" veya parolasını mantıksal uygulamanızı olarak görev yapar.

   ![Mantıksal uygulama kimliği için anahtar oluşturma](./media/logic-apps-custom-api-authentication/create-logic-app-identity-key-secret-password.png)

8. Araç çubuğunda seçin **kaydetmek**. Altında **değeri**, anahtarınızı artık görünür. 
**Kopyalama ve anahtarınızı kaydetmek emin olun** daha sonra kullanmak üzere anahtar gizli olduğu çıktığınızda **anahtarları** sayfası.

   Mantıksal uygulamanızı bölümü 3'te yapılandırdığınızda, bu anahtarı parola ya da "gizli" olarak belirtin.

   ![Kopyalayın ve anahtarı için daha sonra kaydedin](./media/logic-apps-custom-api-authentication/logic-app-copy-key-secret-password.png)

<a name="powershell"></a>

**PowerShell'de mantığı uygulamanız için uygulama kimliği oluşturma**

Bu görevi PowerShell ile Azure Resource Manager aracılığıyla gerçekleştirebilirsiniz. PowerShell'de aşağıdaki komutları çalıştırın:

1. `Switch-AzureMode AzureResourceManager`

2. `Add-AzureAccount`

3. `New-AzureADApplication -DisplayName "MyLogicAppID" -HomePage "http://mydomain.tld" -IdentifierUris "http://mydomain.tld" -Password "identity-password"`

4. Kopyaladığınızdan emin olun **Kiracı kimliği** (GUID) Azure AD kiracınız için **uygulama kimliği**ve kullandığınız parola.

Daha fazla bilgi için bilgi nasıl [kaynaklarına erişmek için PowerShell ile bir hizmet sorumlusu oluşturma](../azure-resource-manager/resource-group-authenticate-service-principal.md).

#### <a name="part-2-create-an-azure-ad-application-identity-for-your-web-app-or-api-app"></a>Bölüm 2: web uygulaması veya API uygulaması için bir Azure AD uygulama kimliği oluşturma

Web uygulaması veya API uygulama zaten dağıtılmışsa, kimlik doğrulamasını etkinleştirmek ve Azure portalında uygulama kimliği oluşturun. Aksi takdirde, şunları yapabilirsiniz [bir Azure Resource Manager şablonu ile dağıtırken kimlik doğrulamasını etkinleştirmek](#authen-deploy). 

**Uygulama Kimliği oluşturma ve kimlik doğrulama dağıtılan uygulamalar için Azure Portalı'nda Aç**

1. İçinde [Azure portal](https://portal.azure.com "https://portal.azure.com"), bulma ve web uygulaması veya API uygulaması seçin. 

2. Altında **ayarları**, seçin **kimlik doğrulama/yetkilendirme**. Altında **App Service kimlik doğrulaması**, kimlik doğrulamasını **üzerinde**. Altında **kimlik doğrulama sağlayıcıları**, seçin **Azure Active Directory**.

   ![Kimlik doğrulamasını etkinleştirmek](./media/logic-apps-custom-api-authentication/custom-web-api-app-authentication.png)

3. Artık web uygulaması veya API uygulaması için uygulama kimliği, aşağıda gösterildiği gibi oluşturun. Üzerinde **Azure Active Directory ayarları** sayfasında **yönetim modu** için **Express**. Seçin **yeni AD uygulaması oluştur**. Uygulama kimliğinizi bir ad verin ve seçin **Tamam**. 

   ![Web uygulaması veya API uygulaması için uygulama kimliği oluşturma](./media/logic-apps-custom-api-authentication/custom-api-application-identity.png)

4. Üzerinde **kimlik doğrulama / yetkilendirme** sayfasında, **kaydetmek**.

Artık web uygulaması veya API uygulaması ile ilişkili olan uygulama kimliği için istemci kimliği ve Kiracı kimliği bulmanız gerekir. Bu kimlikleri bölümü 3'te kullanın. Bu nedenle Azure portalı için aşağıdaki adımları devam edin.

**Web uygulaması veya API uygulaması için uygulama kimliği'nin istemci kimliği ve Kiracı kimliği Azure portalında Bul**

1. Altında **kimlik doğrulama sağlayıcıları**, seçin **Azure Active Directory**. 

   !["Azure Active Directory" seçin](./media/logic-apps-custom-api-authentication/custom-api-app-identity-client-id-tenant-id.png)

2. Üzerinde **Azure Active Directory ayarları** sayfasında **yönetim modu** için **Gelişmiş**.

3. Kopya **istemci kimliği**ve bölüm 3'te kullanmak için bu GUID'i kaydedin.

   > [!TIP] 
   > Varsa **istemci kimliği** ve **veren URL'si** yoksa, görünür, Azure portal'ı yenilemeyi deneyin ve 1. adım yineleyin.

4. Altında **veren URL'si**, kopyalamak ve yalnızca GUID için bölümü 3 kaydedin. Aynı zamanda bu GUID web uygulamanızı veya API uygulamasının dağıtım şablonu, gerekirse kullanabilirsiniz.

   Bu GUID belirli kiracının GUID ("Kiracı kimliği") ve bu URL'de görünmelidir:`https://sts.windows.net/{GUID}`

5. Değişikliklerinizi kaydetmeden kapatın **Azure Active Directory ayarları** sayfası.

<a name="authen-deploy"></a>

**Bir Azure Resource Manager şablonu ile dağıtırken kimlik doğrulamasını etkinleştirmek**

Hala mantığı uygulamanız için uygulama kimliği web uygulaması veya farklı bir API uygulaması için bir Azure AD uygulama kimliği da oluşturmanız gerekir. Uygulama kimliği oluşturmak için Azure portalı için Kısım 2 önceki adımları izleyin. 

Ayrıca, 1. Bölüm adımları ancak web uygulamanızı veya API uygulamasının gerçek kullandığınızdan emin olun `https://{URL}` için **oturum açma URL'si** ve **uygulama kimliği URI'si**. Bu adımları, istemci kimliği ve Kiracı kimliği, uygulamanızın dağıtım şablonu kullanmak için ve ayrıca bölümü 3 kaydetmek gerekir.

> [!NOTE]
> Web uygulaması veya API uygulaması için Azure AD uygulama kimliği oluşturduğunuzda, Azure portalı, değil PowerShell kullanmanız gerekir. PowerShell komutunu kullanıcılar bir Web sitesine oturum için gerekli izinleri ayarlayın değil.

İstemci kimliği ve Kiracı kimliği aldıktan sonra bu kimlikleri subresource web uygulaması veya API uygulaması için dağıtım şablonunda olarak şunları içerir:

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

Boş web uygulaması ve bir mantıksal uygulama Azure Active Directory kimlik doğrulaması ile birlikte otomatik olarak dağıtmak için [burada tam şablonu görüntüleme](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-custom-api/azuredeploy.json), veya **Azure'a Dağıt** burada:

[![Azure’a dağıtma](media/logic-apps-custom-api-authentication/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-custom-api%2Fazuredeploy.json)

#### <a name="part-3-populate-the-authorization-section-in-your-logic-app"></a>3. Kısım: mantıksal uygulamanızı yetkilendirme bölümünde doldurma

Önceki şablonu zaten bu yetkilendirmesi bölümünde ayarladığınız sahip, ancak mantıksal uygulama doğrudan yazıyorsanız, tam yetkilendirme bölümü içermelidir.

Açık mantığı uygulama tanımınızı kod görünümünde Git **HTTP** eylem bölümü Bul **yetkilendirme** bölümünde ve bu satırı ekleyin:

`{"tenant": "{tenant-ID}", "audience": "{client-ID-from-Part-2-web-app-or-API app}", "clientId": "{client-ID-from-Part-1-logic-app}", "secret": "{key-from-Part-1-logic-app}", "type": "ActiveDirectoryOAuth" }`

| Öğesi | Gerekli | Açıklama | 
| ------- | -------- | ----------- | 
| Kiracı | Evet | Azure AD kiracısı için GUID | 
| Hedef kitle | Evet | Web uygulaması veya API uygulaması için uygulama kimliği istemci Kimliğinden erişmek istediğiniz hedef kaynağı için GUID | 
| istemci kimliği | Evet | Mantıksal uygulamanız için uygulama kimliği gelen istemci kimliği erişim isteğinde bulunan istemci için GUID | 
| Gizli | Evet | Anahtar ya da erişim belirteci isteyen istemci için uygulama kimliği paroladan | 
| type | Evet | Kimlik doğrulama türü. ActiveDirectoryOAuth kimlik doğrulaması için değerdir `ActiveDirectoryOAuth`. | 
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

Web uygulaması veya API uygulaması için mantıksal uygulamanızı öğesinden gelen istekleri doğrulamak için istemci sertifikaları kullanabilirsiniz. Kodu ayarlamak için bilgi [TLS karşılıklı kimlik doğrulamasını yapılandırmak nasıl](../app-service/app-service-web-configure-tls-mutual-auth.md).

İçinde **yetkilendirme** bölümünde, bu satırı ekleyin: 

`{"type": "clientcertificate", "password": "password", "pfx": "long-pfx-key"}`

| Öğesi | Gerekli | Açıklama | 
| ------- | -------- | ----------- | 
| type | Evet | Kimlik doğrulama türü. SSL istemci sertifikaları için değer olmalıdır `ClientCertificate`. | 
| password | Evet | İstemci sertifikası (PFX dosyası) erişmek için parola | 
| PFX | Evet | İstemci sertifikası (PFX dosyası) base64 ile kodlanmış içeriği | 
|||| 

<a name="basic"></a>

#### <a name="basic-authentication"></a>Temel kimlik doğrulaması

Web uygulaması veya API uygulaması mantıksal uygulamanızı öğesinden gelen istekleri doğrulamak için bir kullanıcı adı ve parola gibi temel kimlik doğrulaması kullanabilirsiniz. Temel kimlik doğrulaması genel bir desen olduğundan ve web uygulaması veya API uygulaması oluşturmak için kullanılan herhangi bir dilde bu kimlik doğrulaması kullanabilirsiniz.

İçinde **yetkilendirme** bölümünde, bu satırı ekleyin:

`{"type": "basic", "username": "username", "password": "password"}`.

| Öğesi | Gerekli | Açıklama | 
| ------- | -------- | ----------- | 
| type | Evet | Kullanmak istediğiniz kimlik doğrulama türü. Temel kimlik doğrulaması için değer olmalıdır `Basic`. | 
| kullanıcı adı | Evet | Kimlik doğrulaması için kullanmak istediğiniz kullanıcı adı | 
| password | Evet | Kimlik doğrulaması için kullanmak istediğiniz parolayı | 
|||| 

<a name="azure-ad-code"></a>

#### <a name="azure-active-directory-authentication-through-code"></a>Kod aracılığıyla Azure Active Directory kimlik doğrulaması

Varsayılan olarak, Azure portalında kapatma Azure AD kimlik doğrulaması hassas yetkilendirme sağlamaz. Örneğin, bu kimlik doğrulama API'nizi yalnızca belirli bir kiracı için değil, belirli bir kullanıcı veya uygulama kilitler. 

Mantıksal uygulamanızı kodlarda için API erişimini kısıtlamak için JSON web token (JWT) sahip üstbilgi ayıklayın. Arayanın Kimliği denetleyin ve eşleşmeyen istekleri reddedecek.

<!-- Going further, to implement this authentication entirely in your own code, 
and not use the Azure portal, learn how to 
[authenticate with on-premises Active Directory in your Azure app](../app-service/app-service-authentication-overview.md).

To create an application identity for your logic app and use that identity to call your API, 
you must follow the previous steps. -->

## <a name="next-steps"></a>Sonraki adımlar

* [Dağıtma ve uygulama iş akışları mantığından özel API'leri çağırmak](../logic-apps/logic-apps-custom-api-host-deploy-call.md)