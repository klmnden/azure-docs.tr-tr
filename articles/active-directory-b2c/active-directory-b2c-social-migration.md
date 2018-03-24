---
title: 'Azure Active Directory B2C: sosyal kimlikleri kullanıcılarla geçirme'
description: Grafik API'sini kullanarak Azure AD B2C içine sosyal kimlikleri kullanıcılarla geçişini üzerinde temel kavramlar açıklanmaktadır
services: active-directory-b2c
documentationcenter: ''
author: davidmu
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 03/03/2018
ms.author: davidmu
ms.openlocfilehash: 76ed4dac40872bf6db07b26c5805a4db62dc9dfc
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="azure-active-directory-b2c-migrate-users-with-social-identities"></a>Azure Active Directory B2C: sosyal kimlikleri kullanıcılarla geçirme
Kimlik sağlayıcınızı Azure AD B2C'ye geçirmek planlama yaparken, sosyal kimlikleri kullanıcılarla geçirmek gerekebilir. Bu makalede, var olan sosyal kimlikleri hesapları gibi geçirmek açıklanmaktadır: Azure AD B2C Facebook, LinkedIn, Microsoft ve Google hesaplar. Bu geçiş daha az yaygın olan ancak bu makale Federasyon kimlikleri için de geçerlidir.

## <a name="prerequisites"></a>Önkoşullar
Bu makalede, bir kullanıcı geçiş makaleyi devamıdır ve sosyal kimliği geçişini odaklanır. Başlamadan önce okuyun [kullanıcı geçişi](active-directory-b2c-user-migration.md).

## <a name="social-identities-migration-introduction"></a>Sosyal kimlikleri geçiş giriş

* Azure AD B2C'de içinde **yerel hesaplar** oturum açma adları (kullanıcı adı veya e-posta adresi) depolanır `signInNames` kullanıcı kaydındaki koleksiyonu. `signInNames` Birini veya birkaçını içeriyor `signInName` kullanıcı için oturum açma adları belirtin kaydeder. Her oturum açma adı Kiracı arasında benzersiz olması gerekir.

* **Sosyal hesapları** kimlikleri depolanır `userIdentities` koleksiyonu. Girişi belirtir `issuer` (kimlik sağlayıcı adı) facebook.com gibi ve `issuerUserId`, dağıtımcı için benzersiz kullanıcı tanımlayıcısı olduğu. `userIdentities` Özniteliği sosyal hesap türünü ve sosyal kimlik sağlayıcısından benzersiz kullanıcı tanımlayıcısını belirtin bir veya daha fazla Userıdentity kayıtları içerir.

* **Yerel hesap sosyal kimlikle birleştirmek**. Belirtildiği gibi yerel hesap oturum açma adları ve sosyal hesap kimlikleri farklı öznitelikler depolanır. `signInNames` olan yerel hesabı için kullanılan, while `userIdentities` sosyal hesabı. Tek bir Azure AD B2C hesap bir yerel hesap, yalnızca sosyal hesabı olabilir veya bir kullanıcı kaydındaki sosyal kimliğe sahip bir yerel hesap birleştirebilirsiniz. Bu davranış, bir kullanıcının yerel hesabı credential(s) oturum veya sosyal kimliklerle imzalayabilirsiniz sırada tek bir hesap yönetmenize olanak sağlar.

* `UserIdentity` Tür - Azure AD B2C kiracısı bir sosyal hesabı kullanıcı kimliğini hakkında bilgiler içerir:
    * `issuer` Kullanıcı tanımlayıcısı facebook.com gibi verilen kimlik sağlayıcısı dize gösterimi.
    * `issuerUserId` Base64 biçiminde sosyal kimlik sağlayıcısı tarafından kullanılan benzersiz kullanıcı tanımlayıcısı.

    ```JSON
    "userIdentities": [{
            "issuer": "Facebook.com",
            "issuerUserId": "MTIzNDU2Nzg5MA=="
        }
    ]
    ```

* Kimlik sağlayıcısı bağlı olarak **sosyal kullanıcı kimliği** belirli bir kullanıcı için benzersiz bir değerdir `per application` veya geliştirme hesabı. Sosyal sağlayıcısı tarafından daha önce atanmış aynı uygulama kimliği ile Azure AD B2C ilkesini yapılandırın. Veya başka bir uygulama `within the same development account`.

## <a name="use-graph-api-to-migrate-users"></a>Kullanıcıları geçirmek için grafik API'sini kullanın
Aracılığıyla Azure AD B2C kullanıcı hesabı oluşturma [grafik API'si](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet). Grafik API'si ile iletişim kurmak için bir hizmet hesabı yönetici ayrıcalıklarına sahip olmalıdır. Azure AD içinde bir uygulama ve Azure ad kimlik doğrulama kaydedin. Uygulama kimlik bilgileridir. uygulama kimliği ve uygulama gizli anahtarı. Uygulama, grafik API'sini çağırmak için bir kullanıcı değil, olarak kendisini görür. Adım 1'ndaki yönergeleri izleyin [kullanıcı geçişi](https://docs.microsoft.com/en-us/azure/active-directory-b2c/active-directory-b2c-user-migration#step-1-use-graph-api-to-migrate-users) makalesi.

## <a name="required-properties"></a>Gerekli özellikleri
Aşağıdaki liste, bir kullanıcı oluştururken gerekli özellikleri gösterir.
* **accountEnabled** - true
* **displayName** -kullanıcının adres defterinde görüntülenecek ad.
* **passwordProfile** -kullanıcı için parola profil. 

> [!NOTE]
> Yalnızca (yerel hesap kimlik bilgileri olmadan) sosyal hesabı için parola hala belirtmeniz gerekir. Azure AD B2C sosyal hesaplar için belirttiğiniz parola yok sayar.

* **userPrincipalName** -kullanıcı asıl adı (someuser@contoso.com). Kullanıcı asıl adı doğrulanmış etki alanlarından biri Kiracı için içermelidir. UPN belirtmek için yeni GUID değeri ile Birleştir oluşturmak `@` ve Kiracı adı.
* **mailNickname** -kullanıcı için posta diğer adı. Bu değer, userPrincipalName için kullandığınız aynı kimliği olabilir. 
* **signInNames** -kullanıcı için oturum açma adları belirten bir veya daha fazla SignInName kaydeder. Her oturum açma adı şirket/Kiracı arasında benzersiz olması gerekir. Yalnızca sosyal hesabı için bu özellik boş bırakılabilir.
* **userIdentities** -hesap türü ve sosyal kimlik sağlayıcısından benzersiz kullanıcı tanımlayıcısı sosyal belirten bir veya daha fazla Userıdentity kaydeder.
* [isteğe bağlı] **otherMails** - yalnızca, sosyal hesabının kullanıcının e-posta adresleri 

Daha fazla bilgi için bkz: [grafik API'si başvurusu](https://msdn.microsoft.com/library/azure/ad/graph/api/users-operations#CreateLocalAccountUser)

## <a name="migrate-social-account-only"></a>(Yalnızca) sosyal hesabını geçirme
Yerel hesap kimlik bilgileri olmadan yalnızca, sosyal hesabı oluşturmak için. Grafik API'si için HTTPS POST isteği gönderin. İstek gövdesini oluşturmaya sosyal hesabı kullanıcı özelliklerini içerir. En azından gerekli özelliklerini belirtmeniz gerekir. 


**POST**  https://graph.windows.net/tenant-name.onmicrosoft.com/users

Aşağıdaki form verileri gönder: 

```JSON
{
    "objectId": null,
    "accountEnabled": true,
    "mailNickname": "c8c3d3b8-60cf-4c76-9aa7-eb3235b190c8",
    "signInNames": [],
    "creationType": null,
    "displayName": "Sara Bell",
    "givenName": "Sara",
    "surname": "Bell",
    "passwordProfile": {
        "password": "Test1234",
        "forceChangePasswordNextLogin": false
    },
    "passwordPolicies": null,
    "userIdentities": [{
            "issuer": "Facebook.com",
            "issuerUserId": "MTIzNDU2Nzg5MA=="
        }
    ],
    "otherMails": ["sara@live.com"],
    "userPrincipalName": "c8c3d3b8-60cf-4c76-9aa7-eb3235b190c8@tenant-name.onmicrosoft.com"
}
```
## <a name="migrate-social-account-with-local-account"></a>Yerel hesap sosyal hesabıyla geçirme
Sosyal kimliklerle birleşik bir yerel hesap oluşturmak için. Grafik API'si için HTTPS POST isteği gönderin. İstek gövdesini oluşturmaya sosyal hesabı kullanıcı özelliklerini içerir. En azından gerekli özelliklerini belirtmeniz gerekir. 

**POST**  https://graph.windows.net/tenant-name.onmicrosoft.com/users

Form verileri gönder: 

```JSON
{
    "objectId": null,
    "accountEnabled": true,
    "mailNickname": "5164db16-3eee-4629-bfda-dcc3326790e9",
    "signInNames": [{
            "type": "emailAddress",
            "value": "david@contoso.com"
        }
    ],
    "creationType": "LocalAccount",
    "displayName": "David Hor",
    "givenName": "David",
    "surname": "Hor",
    "passwordProfile": {
        "password": "1234567",
        "forceChangePasswordNextLogin": false
    },
    "passwordPolicies": "DisablePasswordExpiration,DisableStrongPassword",
    "userIdentities": [{
            "issuer": "contoso.com",
            "issuerUserId": "ZGF2aWRAY29udG9zby5jb20="
        }
    ],
    "otherMails": [],
    "userPrincipalName": "5164db16-3eee-4629-bfda-dcc3326790e9@tenant-name.onmicrosoft.com"
}
```

## <a name="frequently-asked-questions"></a>Sık sorulan sorular
### <a name="how-can-i-know-the-issuer-name"></a>Verenin adı nasıl öğrenebilirim?
Verenin adı veya kimlik sağlayıcı adı ilkenizde yapılandırılır. Değeri belirtmek üzere bilmiyorsanız `issuer`, bu yordamı izleyin:
1. Sosyal hesaplardan biriyle oturum oturum
2. JWT belirtecinden kopyalama `sub` değeri. `sub` Genellikle kullanıcının nesne kimliği Azure AD B2C içerir. Veya Azure portalından kullanıcının özelliklerini açın ve nesne kimliği kopyalayın.
3. Açık [Azure AD Graph Explorer'a](https://graphexplorer.azurewebsites.net)
4. Yöneticiniz oturum açın. N
5. GET isteğini çalıştırın. UserObjectId kopyaladığınız kullanıcı kimliği ile değiştirin. **AL** https://graph.windows.net/tenant-name.onmicrosoft.com/users/userObjectId
6. Bulun `userIdentities` öğesi Azure AD B2C JSON dönüş içinde.
7. [İsteğe bağlı] Kod çözme isteyebilirsiniz `issuerUserId` değeri.

> [!NOTE]
> B2C Kiracı için yerel bir B2C Kiracı yöneticisi hesabı kullanın. Hesap adı sözdizimi admin@tenant-name.onmicrosoft.com.

### <a name="is-it-possible-to-add-social-identity-to-an-existing-local-account"></a>Var olan bir yerel hesap sosyal kimlik eklemek mümkün mü?
Evet. Yerel hesap oluşturulduktan sonra sosyal kimlik ekleyebilirsiniz. HTTPS PATCH isteği çalıştırın. UserObjectId güncelleştirmek istediğiniz kullanıcı kimliği ile değiştirin. 

**DÜZELTME EKİ** https://graph.windows.net/tenant-name.onmicrosoft.com/users/userObjectId

Form verileri gönder: 

```JSON
{
    "userIdentities": [
        {
            "issuer": "Facebook.com",
            "issuerUserId": "MTIzNDU2Nzg5MA=="
        }
    ]
}
```

### <a name="is-it-possible-to-add-multiple-social-identities"></a>Birden çok sosyal kimlikleri eklemek mümkün mü?
Evet. Tek bir Azure AD B2C hesap için birden fazla sosyal kimlikleri ekleyebilirsiniz. HTTPS PATCH isteği çalıştırın. Kullanıcı kimliği ile userObjectId Değiştir 

**DÜZELTME EKİ** https://graph.windows.net/tenant-name.onmicrosoft.com/users/userObjectId

Form verileri gönder: 

```JSON
{
    "userIdentities": [
        {
            "issuer": "google.com",
            "issuerUserId": "MjQzMjE2NTc4NTQ="
        },
        {
            "issuer": "facebook.com",
            "issuerUserId": "MTIzNDU2Nzg5MA=="
        }
    ]
}
```

## <a name="optional-user-migration-application-sample"></a>[İsteğe bağlı] Kullanıcı Geçiş uygulama örneği
[V2 örnek uygulamasını indirme ve çalıştırma](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-user-migration). Örnek uygulaması V2 dahil olmak üzere, sahte kullanıcı verilerinizi içeren bir JSON dosyası kullanır: yerel hesap, sosyal hesabı ve tek hesap, yerel & Sosyal kimliği.  JSON dosyasının düzenlemek için açın `AADB2C.UserMigration.sln` Visual Studio çözümü. İçinde `AADB2C.UserMigration` Proje Aç `UsersData.json` dosya. Dosya kullanıcı varlıkları listesini içerir. Her kullanıcı varlık aşağıdaki özelliklere sahiptir:
* **signInName** - yerel hesap, oturum açmak için e-posta adresi
* **displayName** -kullanıcının görünen adı
* **firstName** -kullanıcının adı
* **Soyadı** -kullanıcının Soyadı
* **Parola** yerel hesap, kullanıcının parolasını (olabilir boş)
* **veren** - sosyal hesabının, kimlik sağlayıcısı adı
* **issuerUserId** - sosyal hesabının, sosyal kimlik sağlayıcısı tarafından kullanılan benzersiz bir kullanıcı tanımlayıcısı. Değer, düz metin olarak olmalıdır. Örnek uygulaması, bu değer base64 ile kodlar.
* **e-posta** sosyal hesabı yalnızca (olmayan birleştirilmiş), kullanıcı için e-posta adresi

```JSON
{
  "userType": "emailAddress",
  "Users": [
    {
      // Local account only
      "signInName": "James@contoso.com",
      "displayName": "James Martin",
      "firstName": "James",
      "lastName": "Martin",
      "password": "Pass!w0rd"
    },
    {
      // Social account only
      "issuer": "Facebook.com",
      "issuerUserId": "1234567890",
      "email": "sara@contoso.com",
      "displayName": "Sara Bell",
      "firstName": "Sara",
      "lastName": "Bell"
    },
    {
      // Combine local account with social identity
      "signInName": "david@contoso.com",
      "issuer": "Facebook.com",
      "issuerUserId": "0987654321",
      "displayName": "David Hor",
      "firstName": "David",
      "lastName": "Hor",
      "password": "Pass!w0rd"
    }
  ]
}
```

> [!NOTE]
> Örnek UsersData.json dosyasında verilerinizle güncelleştirmezseniz, oturum örnek yerel hesap kimlik bilgilerine sahip ancak sosyal hesap örnekleriyle açma. Sosyal hesaplarınızı geçirmek için gerçek veriler sağlar.

Daha fazla bilgi için örnek uygulamayı kullanma hakkında [Azure Active Directory B2C: Kullanıcı Geçişi](active-directory-b2c-user-migration.md)
