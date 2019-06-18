---
title: Azure Active Directory B2C, sosyal medya kimliklerinden kullanıcıları geçirme | Microsoft Docs
description: Sosyal medya kimliklerinden Graph API'sini kullanarak Azure AD B2C kullanıcıları geçişini üzerinde temel kavramlar açıklanmaktadır.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/03/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: bca802bb0099b0d854d752db8341dfe74031ef3b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66508040"
---
# <a name="azure-active-directory-b2c-migrate-users-with-social-identities"></a>Azure Active Directory B2C: Kullanıcıları sosyal kimlikleriyle geçirme
Kimlik sağlayıcınız Azure AD B2C'ye geçirmek planlama yaparken, sosyal medya kimliklerinden kullanıcıları geçirme gerekebilir. Bu makalede, aşağıdakiler gibi mevcut sosyal kimlikleri hesapları geçirme açıklanmaktadır: Azure AD B2C'ye Facebook ve LinkedIn, Microsoft ve Google hesapları. Bu geçiş daha az yaygın olan ancak bu makale Federasyon kimlikleri için de geçerlidir.

## <a name="prerequisites"></a>Önkoşullar
Bu makalede, bir kullanıcı geçiş makalesini devamıdır ve sosyal kimlik geçişi üzerinde odaklanır. Başlamadan önce okumanız [kullanıcı geçişi](active-directory-b2c-user-migration.md).

## <a name="social-identities-migration-introduction"></a>Sosyal kimlikleri geçiş giriş

* Azure AD B2C'yi de **yerel hesaplar** oturum açma adları (kullanıcı adı veya e-posta adresi) depolanır `signInNames` kullanıcı kaydındaki koleksiyonu. `signInNames` Birini veya daha fazlasını içeren `signInName` kullanıcı için oturum açma adlarını kaydeder. Her oturum açma adı, bir kiracıda benzersiz olması gerekir.

* **Sosyal medya hesaplarını** kimlikleri depolanır `userIdentities` koleksiyonu. Giriş belirtir `issuer` (kimlik sağlayıcı adı) facebook.com gibi ve `issuerUserId`, dağıtımcı için benzersiz kullanıcı tanımlayıcısı olan. `userIdentities` Özniteliği sosyal hesap türünü ve sosyal kimlik sağlayıcısından alınan benzersiz kullanıcı tanımlayıcısını belirten bir veya daha fazla Userıdentity kayıtları içerir.

* **Yerel hesap sosyal kimlik ile birleştirerek**. Belirtildiği gibi yerel hesap oturum açma adları ve sosyal hesap kimlikleri farklı öznitelikler depolanır. `signInNames` olan yerel hesabı için kullanılan, while `userIdentities` sosyal hesap için. Tek bir Azure AD B2C hesabı yalnızca yerel hesap, yalnızca sosyal hesap olabilir veya bir kullanıcı kaydındaki sosyal kimlik yerel bir hesap birleştirin. Bu davranış, bir kullanıcının yerel hesabı credential(s) oturum veya sosyal kimliklerle oturum sırasında tek bir hesap yönetmenize olanak sağlar.

* `UserIdentity` Tür - Azure AD B2C kiracısı bir sosyal hesap kullanıcının kimlik bilgilerini içerir:
  * `issuer` Kullanıcı tanımlayıcısı facebook.com gibi verilen kimlik sağlayıcısı dize gösterimi.
  * `issuerUserId` Base64 biçimindeki sosyal kimlik sağlayıcısı tarafından kullanılan benzersiz kullanıcı tanımlayıcısı.

    ```JSON
    "userIdentities": [{
          "issuer": "Facebook.com",
          "issuerUserId": "MTIzNDU2Nzg5MA=="
      }
    ]
    ```

* Kimlik sağlayıcısına bağlı olarak **sosyal kullanıcı kimliği** uygulama ya da geliştirme hesap başına belirli bir kullanıcı için benzersiz bir değerdir. Sosyal sağlayıcılar tarafından daha önce atanan aynı uygulama kimliği ile Azure AD B2C ilkesi yapılandırın. Veya başka bir uygulama geliştirme hesabın aynısını içinde.

## <a name="use-graph-api-to-migrate-users"></a>Kullanıcıları geçirme için Graph API'sini kullanın
Azure AD B2C kullanıcı hesabı aracılığıyla oluşturma [Graph API'si](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet). Graph API ile iletişim kurmak için öncelikle bir hizmet hesabı yönetici ayrıcalıklarına sahip olması gerekir. Azure AD'de bir uygulama ve kimlik doğrulaması Azure AD'ye kaydedin. Uygulama kimlik bilgileridir. uygulama kimliği ve uygulama gizli anahtarı. Uygulama, kendisini, Graph API'sini çağırmak için bir kullanıcı değil, olarak görür. Adım 1'ndaki yönergeleri izleyin [kullanıcı geçişi](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-user-migration) makalesi.

## <a name="required-properties"></a>Gerekli özellikler
Aşağıdaki listede, bir kullanıcı oluşturduğunuzda, gerekli olan özellikleri gösterir.
* **accountEnabled** - true
* **displayName** -kullanıcının adres defterinde kullanıcı için görüntülenecek ad.
* **passwordProfile** -kullanıcı için parola profil. 

> [!NOTE]
> Yalnızca (yerel hesap kimlik bilgileri olmadan) sosyal hesap için parolayı yine de belirtmeniz gerekir. Azure AD B2C, sosyal hesaplar için belirttiğiniz parolayı yok sayar.

* **userPrincipalName** -kullanıcı asıl adı (someuser@contoso.com). Kullanıcı asıl adı bir doğrulanmış etki alanları için Kiracı içermelidir. UPN belirtmek için yeni GUID değeri ile Birleştir oluşturmak `@` ve Kiracı adı.
* **mailNickname** -kullanıcı için posta diğer adı. Bu değer, userPrincipalName için kullandığınız aynı kimliği olabilir. 
* **signInNames** -kullanıcı oturum açma adlarını belirten bir veya daha fazla SignInName kaydeder. Her oturum açma adı şirket/Kiracı benzersiz olmalıdır. Yalnızca sosyal hesap için bu özellik boş bırakılabilir.
* **userIdentities** -sosyal belirten bir veya daha fazla Userıdentity kayıtları hesap türü ve sosyal kimlik sağlayıcısından alınan benzersiz kullanıcı tanımlayıcısı.
* [isteğe bağlı] **otherMails** - yalnızca sosyal hesap kullanıcının e-posta adresleri 

Daha fazla bilgi için bkz. [Graph API Başvurusu](/previous-versions/azure/ad/graph/api/users-operations#CreateLocalAccountUser)

## <a name="migrate-social-account-only"></a>Sosyal hesap (yalnızca) geçirme
Yerel hesap kimlik bilgileri olmadan yalnızca sosyal hesap oluşturmak için. Graph API'si için HTTPS POST isteği gönderin. İstek gövdesi, oluşturulacak sosyal hesap kullanıcı özelliklerini içerir. En azından gerekli özelliklerini belirtmeniz gerekir. 


**POST**  https://graph.windows.net/tenant-name.onmicrosoft.com/users

Aşağıdaki form verileri gönderme: 

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
## <a name="migrate-social-account-with-local-account"></a>Sosyal hesap yerel hesapla geçirme
Sosyal kimliklerle birleşik bir yerel hesap oluşturmak için. Graph API'si için HTTPS POST isteği gönderin. İstek gövdesi, oluşturulacak sosyal hesap kullanıcı özelliklerini içerir. En azından gerekli özelliklerini belirtmeniz gerekir. 

**POST**  https://graph.windows.net/tenant-name.onmicrosoft.com/users

Form verileri gönderme: 

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
### <a name="how-can-i-know-the-issuer-name"></a>Veren adı nasıl öğrenebilirim?
Veren adı veya kimlik sağlayıcı adı, ilkede yapılandırılır. Değeri belirtmek için bilmiyorsanız `issuer`, bu yordamı izleyin:
1. Sosyal hesaplarından birini bilgilerinizle oturum açın
2. JWT belirtecini Kopyala `sub` değeri. `sub` Genellikle kullanıcının nesne kimliği Azure AD B2C'yi içerir. Veya Azure portalından kullanıcının özelliklerini açın ve nesne kimliği kopyalayın.
3. Açık [Azure AD Graph Gezgini](https://graphexplorer.azurewebsites.net)
4. Yöneticiniz bilgilerinizle oturum açın.
5. GET isteğini takip çalıştırın. UserObjectId kopyaladığınız kullanıcı kimliği ile değiştirin. **AL** https://graph.windows.net/tenant-name.onmicrosoft.com/users/userObjectId
6. Bulun `userIdentities` Azure AD B2C JSON dönüş içinde öğe.
7. [İsteğe bağlı] Kod çözme isteyebilirsiniz `issuerUserId` değeri.

> [!NOTE]
> B2C kiracısı için yerel bir B2C Kiracı yönetici hesabı kullanın. Hesap adı sözdizimi admin@tenant-name.onmicrosoft.com.

### <a name="is-it-possible-to-add-social-identity-to-an-existing-local-account"></a>Sosyal kimlik için var olan bir yerel hesap eklemek mümkündür?
Evet. Yerel hesap oluşturulduktan sonra sosyal kimlik ekleyebilirsiniz. HTTPS PATCH isteği çalıştırın. UserObjectId güncelleştirmek istediğiniz kullanıcı kimliği ile değiştirin. 

**DÜZELTME EKİ** https://graph.windows.net/tenant-name.onmicrosoft.com/users/userObjectId

Form verileri gönderme: 

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

### <a name="is-it-possible-to-add-multiple-social-identities"></a>Birden çok sosyal kimlik eklemek mümkündür?
Evet. Birden çok sosyal kimlikler için tek bir Azure AD B2C hesabı ekleyebilirsiniz. HTTPS PATCH isteği çalıştırın. UserObjectId kullanıcı kimliği ile değiştirin. 

**DÜZELTME EKİ** https://graph.windows.net/tenant-name.onmicrosoft.com/users/userObjectId

Form verileri gönderme: 

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
[V2 örnek uygulamasını indirme ve çalıştırma](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-user-migration). V2 örnek uygulamasını dahil olmak üzere, işlevsiz kullanıcı verilerinizi içeren bir JSON dosyası kullanır: yerel hesap, sosyal hesap ve tek hesap, yerel ve sosyal kimlikleri.  JSON dosyasını düzenlemek için açın `AADB2C.UserMigration.sln` Visual Studio çözümü. İçinde `AADB2C.UserMigration` projesini açarsanız `UsersData.json` dosya. Dosyayı kullanıcı varlıkların bir listesini içerir. Her kullanıcı varlığı, aşağıdaki özelliklere sahiptir:
* **signInName** - yerel hesap, oturum açma e-posta adresi
* **displayName** -kullanıcının görünen adı
* **firstName** -kullanıcının adı
* **Soyadı** -son kullanıcının adı
* **Parola** yerel hesabı için kullanıcının parolasını (boş olabilir)
* **veren** - sosyal hesap, kimlik sağlayıcısının adı
* **issuerUserId** - sosyal hesap, sosyal kimlik sağlayıcısı tarafından kullanılan benzersiz kullanıcı tanımlayıcısı. Düz metin olarak değer olmalıdır. Örnek uygulama, bu değer Base64 kodlar.
* **e-posta** sosyal hesap yalnızca (değil birleştirilmiş), kullanıcı için e-posta adresi

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
> Örnek UsersData.json dosyasında verilerinizle güncelleştirmezseniz örnek yerel hesap kimlik bilgileri ile ancak bir sosyal hesap örnekleriyle oturum. Sosyal medya hesaplarını geçirmek için gerçek veriler sağlar.

Daha fazla bilgi edinmek için örnek uygulama kullanma, bkz [Azure Active Directory B2C: Kullanıcı Geçişi](active-directory-b2c-user-migration.md)
