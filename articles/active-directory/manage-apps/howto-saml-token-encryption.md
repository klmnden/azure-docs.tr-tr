---
title: Azure Active Directory'de SAML belirteci şifreleme
description: Azure Active Directory SAML belirteci şifreleme yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/06/2019
ms.author: celested
ms.reviewer: paulgarn
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7de6705ad38133b8321caabb7b0f4093284af503
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60291509"
---
# <a name="how-to-configure-azure-ad-saml-token-encryption-preview"></a>Nasıl yapılır: Azure AD SAML belirteci şifreleme (Önizleme) yapılandırma

> [!NOTE]
> Belirteç şifreleme, bir Azure Active Directory (Azure AD) premium özelliğidir. Azure AD sürümleri, özellikler ve fiyatlandırma hakkında daha fazla bilgi için bkz. [Azure AD fiyatlandırma](https://azure.microsoft.com/pricing/details/active-directory/).

SAML belirteci şifreleme onu destekleyen bir uygulama ile şifrelenmiş SAML onaylamalarını kullanımını etkinleştirir. Bir uygulama için yapılandırıldığında, Azure AD için Azure AD'de depolanan bir sertifika elde ortak anahtarı kullanarak bu uygulama kendisini çıkaran derlemeninkinden SAML onaylamalarını şifreler. Uygulama belirteci kimlik kanıtı oturum açmış olan kullanıcıdan kullanılabilmesi şifresini çözmek için eşleşen özel anahtarı kullanmanız gerekir.

Azure AD arasında SAML onaylamalarını şifreleme ve uygulama belirteci içeriğini kesildi ve kişisel veya şirket verilerini tehlikeye ek güvence sağlar.

Belirteç şifreleme olmadan bile, Azure AD SAML belirteçlerini açık bir ağ üzerinde hiçbir zaman geçirilir. Azure AD, IDP, tarayıcı ve uygulama arasındaki iletişimler şifreli bağlantıları üzerinden gerçekleşmesi için şifrelenmiş bir HTTPS/TLS kanallar üzerinde gerçekleşmesi için belirteç istek/yanıt değişimleri gerektirir. Ek sertifikaların yönetilmesi yükünü ile karşılaştırıldığında durumunuza belirteci şifreleme değerini göz önünde bulundurun.   

Belirteç şifreleme yapılandırmak için uygulamanın gösteren Azure AD uygulama nesnesi için ortak anahtarı içeren bir X.509 sertifika dosyasını karşıya yüklemek gerekir. X.509 sertifikası almak için uygulamanın kendisi veya bunu uygulama satıcıdan burada uygulamanın satıcısına şifreleme anahtarları sağlar veya burada uygulama özel anahtarı sağlamanızı bekler durumlarda olabilir durumlarda get indirebilirsiniz Şifreleme dilediğinizle oluşturulan, uygulamanın anahtar deposu için özel anahtar bölümünü karşıya ve Azure AD'ye eşleşen ortak anahtar sertifika karşıya yüklendi.

Azure AD, SAML onaylama işlemi verileri şifrelemek için AES-256 kullanır.

## <a name="configure-saml-token-encryption"></a>SAML belirteci şifreleme yapılandırma

SAML belirteci şifreleme yapılandırmak için aşağıdaki adımları izleyin:

1. Uygulamada yapılandırılmış özel bir anahtarla eşleşen bir ortak anahtar sertifikası alın.

    Şifreleme için kullanılacak bir asimetrik anahtar çifti oluşturun. Ya da uygulama şifreleme için kullanılacak bir ortak anahtar sağlarsa, X.509 sertifikasını indirmek için uygulamanın yönergeleri izleyin.

    Ortak anahtarı bir X.509 sertifika dosyası .cer biçiminde depolanması gerekir.

    Uygulama, Örneğiniz için oluşturduğunuz bir anahtar kullanıyorsa, uygulamanın Azure AD kiracınızdan belirteçlerin şifresini çözmek için kullanacağı özel anahtarı yüklemek için uygulamanız tarafından sağlanan yönergeleri izleyin.

1. Sertifika, Azure AD'de uygulama yapılandırmasına ekleyin.

### <a name="to-configure-token-encryption-in-the-azure-portal"></a>Azure portalında belirteci şifreleme yapılandırmak için

Azure portalındaki uygulama yapılandırmanız için ortak sertifika ekleyebilirsiniz.

1. [Azure Portal](https://portal.azure.com) gidin.

1. Git **Azure Active Directory > Kurumsal uygulamalar** dikey penceresinde ve belirteç şifreleme için yapılandırmak istediğiniz uygulamayı seçin.

1. Uygulama sayfasında seçin **belirteç şifreleme**.

    ![Azure portalında belirteç şifreleme seçeneği](./media/howto-saml-token-encryption/token-encryption-option-small.png)

    > [!NOTE]
    > **Belirteç şifreleme** seçeneği, yalnızca gelen ayarlanan SAML uygulamaları için kullanılabilir **kurumsal uygulamalar** Azure portalındaki dikey penceresinde, uygulama Galeriden veya Galeri dışı uygulama. Diğer uygulamalar için bu menü seçeneği devre dışı bırakıldı. Uygulamalar üzerinden kayıtlı için **uygulama kayıtları** deneyimi Azure portalında Microsoft Graph veya PowerShell aracılığıyla SAML belirteçlerini kullanarak uygulama bildirimi için şifreleme yapılandırabilirsiniz.

1. Üzerinde **belirteç şifreleme** sayfasında **içe sertifikayı** , ortak bir X.509 sertifikasını içeren .cer dosyasını almak için.

    ![X.509 sertifikasını içeren .cer dosyasını alma](./media/howto-saml-token-encryption/import-certificate-small.png)

1. Sertifika içeri ve özel anahtar, uygulama tarafından kullanım için yapılandırıldıktan sonra şifreleme seçerek etkinleştirin **...**  sonraki parmak izi durumuna ve ardından **belirteci şifreleme etkinleştirme** açılır menüdeki seçeneklerden.

1. Seçin **Evet** belirteci şifreleme sertifikasının etkinleştirmenin onaylanması için.

1. Uygulama için yayılan SAML onaylamalarını şifrelenmesini onaylayın.

### <a name="to-deactivate-token-encryption-in-the-azure-portal"></a>Azure portalında belirteci şifreleme devre dışı bırakmak için

1. Azure portalında Git **Azure Active Directory > Kurumsal uygulamalar**ve ardından SAML belirteci şifreleme etkin olan bir uygulama seçin.

1. Uygulama sayfasında seçin **belirteç şifreleme**sertifikayı bulun ve ardından **...**  açılır menüsünde gösterilecek seçeneği.

1. Seçin **belirteci şifreleme devre dışı bırakma**.

## <a name="configure-saml-token-encryption-using-graph-api-powershell-or-app-manifest"></a>SAML belirteci şifreleme Graph API, PowerShell veya uygulama bildirimi kullanarak yapılandırma

Şifreleme sertifikaları ile Azure AD'de uygulama nesnesi üzerinde depolanır bir `encrypt` kullanım etiketi. Birden çok şifreleme sertifikası yapılandırabilirsiniz ve belirteçleri şifrelemek için etkin olan tarafından tanımlanan `tokenEncryptionKeyID` özniteliği.

Microsoft Graph API'si veya PowerShell kullanarak belirteci şifreleme yapılandırmak için uygulamanın nesne kimliği gerekir. Programlama yoluyla veya uygulamanın giderek bu değeri bulabilirsiniz **özellikleri** sayfasında Azure portalı ve Not **nesne kimliği** değeri.

Graf, PowerShell kullanarak bir keyCredential yapılandırırken veya uygulama bildiriminde Keyıd için kullanılacak bir GUID oluşturmanız gerekir.

### <a name="to-configure-token-encryption-using-microsoft-graph"></a>Microsoft Graph'ı kullanarak belirteci şifreleme yapılandırmak için

1. Uygulamanın güncelleştirme `keyCredentials` şifreleme için bir X.509 sertifikası ile. Aşağıdaki örnek bunun nasıl yapılacağı gösterilmektedir.

    ```
    Patch https://graph.microsoft.com/beta/applications/<application objectid>

    { 
       "keyCredentials":[ 
          { 
             "type":"AsymmetricX509Cert","usage":"Encrypt",
             "keyId":"fdf8c5d8-f727-43fd-beaf-0f1521cf3d35",    (Use a GUID generator to obtain a value for the keyId)
             "key": "MIICADCCAW2gAwIBAgIQ5j9/b+n2Q4pDvQUCcy3…"  (Base64Encoded .cer file)
          }
        ]
    }
    ```

1. Belirteçleri şifrelemek için etkin şifreleme sertifikası belirleyin. Aşağıdaki örnek bunun nasıl yapılacağı gösterilmektedir.

    ```
    Patch https://graph.microsoft.com/beta/applications/<application objectid> 

    { 
       "tokenEncryptionKeyId":"fdf8c5d8-f727-43fd-beaf-0f1521cf3d35" (The keyId of the keyCredentials entry to use)
    }
    ```

### <a name="to-configure-token-encryption-using-powershell"></a>PowerShell kullanarak belirteci şifreleme yapılandırmak için

Bu işlev yakında kullanıma sunulacaktır. 

<!--
1. Use the latest Azure AD PowerShell module to connect to your tenant.

1. Set the token encryption settings using the **[Set-AzureApplication](https://docs.microsoft.com/powershell/module/azuread/set-azureadapplication?view=azureadps-2.0-preview)** command.

    ```
    Set-AzureADApplication -ObjectId <ApplicationObjectId> -KeyCredentials “<KeyCredentialsObject>”  -TokenEncryptionKeyId <keyID>
    ```

1. Read the token encryption settings using the following commands.

    ```powershell
    $app=Get-AzureADApplication -ObjectId <ApplicationObjectId>
    $app.KeyCredentials
    $app.TokenEncryptionKeyId
    ```

-->

### <a name="to-configure-token-encryption-using-the-application-manifest"></a>Uygulama bildirimi kullanarak belirteci şifreleme yapılandırmak için

1. Azure portalından Git **Azure Active Directory > Uygulama kayıtları**.

1. Seçin **tüm uygulamalar** açılan tüm uygulamaları gör ve ardından yapılandırmak istediğiniz kuruluş uygulaması'ı seçin.

1. Uygulama sayfasında seçin **bildirim** düzenlenecek [uygulama bildirimini](../develop/reference-app-manifest.md).

1. Değerini `tokenEncryptionKeyId` özniteliği.

    Bir uygulama bildirimi iki şifreleme sertifikaları ile yapılandırılmış ve ikinci tokenEnryptionKeyId kullanarak etkin bir seçili aşağıdaki örnekte gösterilmektedir.

    ```json
    { 
      "id": "3cca40e2-367e-45a5-8440-ed94edd6cc35",
      "accessTokenAcceptedVersion": null,
      "allowPublicClient": false,
      "appId": "cb2df8fb-63c4-4c35-bba5-3d659dd81bf1",
      "appRoles": [],
      "oauth2AllowUrlPathMatching": false,
      "createdDateTime": "2017-12-15T02:10:56Z",
      "groupMembershipClaims": "SecurityGroup",
      "informationalUrls": { 
         "termsOfService": null, 
         "support": null, 
         "privacy": null, 
         "marketing": null 
      },
      "identifierUris": [ 
        "https://testapp"
      ],
      "keyCredentials": [ 
        { 
          "customKeyIdentifier": "Tog/O1Hv1LtdsbPU5nPphbMduD=", 
          "endDate": "2039-12-31T23:59:59Z", 
          "keyId": "8be4cb65-59d9-404a-a6f5-3d3fb4030351", 
          "startDate": "2018-10-25T21:42:18Z", 
          "type": "AsymmetricX509Cert", 
          "usage": "Encrypt", 
          "value": <Base64EncodedKeyFile> 
          "displayName": "CN=SAMLEncryptTest" 
        }, 
        {
          "customKeyIdentifier": "U5nPphbMduDmr3c9Q3p0msqp6eEI=",
          "endDate": "2039-12-31T23:59:59Z", 
          "keyId": "6b9c6e80-d251-43f3-9910-9f1f0be2e851",
          "startDate": "2018-10-25T21:42:18Z", 
          "type": "AsymmetricX509Cert", 
          "usage": "Encrypt", 
          "value": <Base64EncodedKeyFile> 
          "displayName": "CN=SAMLEncryptTest2" 
        } 
      ], 
      "knownClientApplications": [], 
      "logoUrl": null, 
      "logoutUrl": null, 
      "name": "Test SAML Application", 
      "oauth2AllowIdTokenImplicitFlow": true, 
      "oauth2AllowImplicitFlow": false, 
      "oauth2Permissions": [], 
      "oauth2RequirePostResponse": false, 
      "orgRestrictions": [], 
      "parentalControlSettings": { 
         "countriesBlockedForMinors": [], 
         "legalAgeGroupRule": "Allow" 
        }, 
      "passwordCredentials": [], 
      "preAuthorizedApplications": [], 
      "publisherDomain": null, 
      "replyUrlsWithType": [], 
      "requiredResourceAccess": [], 
      "samlMetadataUrl": null, 
      "signInUrl": "https://127.0.0.1:444/applications/default.aspx?metadata=customappsso|ISV9.1|primary|z" 
      "signInAudience": "AzureADMyOrg",
      "tags": [], 
      "tokenEncryptionKeyId": "6b9c6e80-d251-43f3-9910-9f1f0be2e851" 
    }  
    ```

## <a name="next-steps"></a>Sonraki adımlar

* Öğrenmek [nasıl Azure AD ve SAML protokolünü kullanır?](../develop/active-directory-saml-protocol-reference.md)
* Biçim, güvenlik özellikleri ve içeriğini öğrenin [Azure AD'de SAML belirteçleri](../develop/reference-saml-tokens.md)