---
title: Azure AD'de kimlik bilgileri sertifikası | Microsoft Docs
description: Bu makalede kayıt ve uygulama kimlik doğrulaması için sertifika kimlik bilgileri kullanımını ele alınmaktadır.
services: active-directory
documentationcenter: .net
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: 88f0c64a-25f7-4974-aca2-2acadc9acbd8
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/21/2019
ms.author: ryanwi
ms.reviewer: nacanuma, jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: ed4e7559ff6c3b76bbdf49b538ffebf3ad09cc58
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66001226"
---
# <a name="certificate-credentials-for-application-authentication"></a>Uygulama kimlik doğrulaması için sertifika kimlik bilgileri

Azure Active Directory (Azure AD) bir uygulama, kimlik doğrulama, örneğin, OAuth 2.0 istemci kimlik bilgileri verme akışı kendi kimlik bilgilerini kullanmayı sağlar ([v1.0](v1-oauth2-client-creds-grant-flow.md), [v2.0](v2-oauth2-client-creds-grant-flow.md)) ve On-Behalf-Of Akış ([v1.0](v1-oauth2-on-behalf-of-flow.md), [v2.0](v2-oauth2-on-behalf-of-flow.md)).

Bir form kimlik doğrulaması için bir uygulama kullanan kimlik bilgisinin, uygulama sahibi olan bir sertifikayla imzalanan bir JSON Web Token (jwt) olaydır.

## <a name="assertion-format"></a>Onaylama işlemi biçimi
Onaylama hesaplamak için çok birini kullanabilirsiniz [JSON Web belirteci](https://jwt.ms/) tercih ettiğiniz dilde kitaplıkları. Belirteç tarafından taşınan bilgi aşağıdaki gibidir:

### <a name="header"></a>Üstbilgi

| Parametre |  Açıklama |
| --- | --- |
| `alg` | Olmalıdır **RS256** |
| `typ` | Olmalıdır **JWT** |
| `x5t` | X.509 Sertifika SHA-1 parmak izi olmalıdır |

### <a name="claims-payload"></a>Talepleri (yükü)

| Parametre |  Açıklamalar |
| --- | --- |
| `aud` | Hedef kitle: Olmalıdır  **https://login.microsoftonline.com/ *Kiracı*  /oauth2/belirteç** |
| `exp` | Süre sonu: belirteç süresinin dolduğu tarih. Zamanı saniye sayısı 1 Ocak 1970'ten gösterilir (1970-01-01T0:0:0Z) kadar belirtecin geçerlilik süresinin dolduğu zamanı UTC.|
| `iss` | Veren: ' % s ' client_id (uygulama kimliği ile istemci hizmeti) olmalıdır |
| `jti` | GUID: JWT kimliği |
| `nbf` | Önceki: belirteç önce kullanılamaz tarih. Zamanı saniye sayısı 1 Ocak 1970'ten gösterilir (1970-01-01T0:0:0Z) belirteci verildiği zamana kadar UTC. |
| `sub` | Konu: Olarak `iss`, client_id (uygulama kimliği ile istemci hizmeti) olmalıdır |

### <a name="signature"></a>İmza

İmza sertifikası bölümünde anlatıldığı gibi uygulama hesaplanan [JSON Web belirteci RFC7519 belirtimi](https://tools.ietf.org/html/rfc7519)

## <a name="example-of-a-decoded-jwt-assertion"></a>Kodu çözülen bir JWT onaylama örneği

```
{
  "alg": "RS256",
  "typ": "JWT",
  "x5t": "gx8tGysyjcRqKjFPnd7RFwvwZI0"
}
.
{
  "aud": "https: //login.microsoftonline.com/contoso.onmicrosoft.com/oauth2/token",
  "exp": 1484593341,
  "iss": "97e0a5b7-d745-40b6-94fe-5f77d35c6e05",
  "jti": "22b3bb26-e046-42df-9c96-65dbd72c1c81",
  "nbf": 1484592741,  
  "sub": "97e0a5b7-d745-40b6-94fe-5f77d35c6e05"
}
.
"Gh95kHCOEGq5E_ArMBbDXhwKR577scxYaoJ1P{a lot of characters here}KKJDEg"

```

## <a name="example-of-an-encoded-jwt-assertion"></a>Kodlanmış JWT onaylama örneği

Aşağıdaki dizeyi kodlanmış onaylama örneğidir. Dikkatli bir şekilde bakarsanız, nokta (.) ile ayrılmış üç bölümü dikkat edin:
* İlk bölüm başlığı kodlar
* İkinci bölüm yükü kodlar
* Son bölümde ilk iki bölümü içeriğini sertifikalarla hesaplanan imza değil

```
"eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJhdWQiOiJodHRwczpcL1wvbG9naW4ubWljcm9zb2Z0b25saW5lLmNvbVwvam1wcmlldXJob3RtYWlsLm9ubWljcm9zb2Z0LmNvbVwvb2F1dGgyXC90b2tlbiIsImV4cCI6MTQ4NDU5MzM0MSwiaXNzIjoiOTdlMGE1YjctZDc0NS00MGI2LTk0ZmUtNWY3N2QzNWM2ZTA1IiwianRpIjoiMjJiM2JiMjYtZTA0Ni00MmRmLTljOTYtNjVkYmQ3MmMxYzgxIiwibmJmIjoxNDg0NTkyNzQxLCJzdWIiOiI5N2UwYTViNy1kNzQ1LTQwYjYtOTRmZS01Zjc3ZDM1YzZlMDUifQ.
Gh95kHCOEGq5E_ArMBbDXhwKR577scxYaoJ1P{a lot of characters here}KKJDEg"
```

## <a name="register-your-certificate-with-azure-ad"></a>Sertifikanız Azure AD'ye kaydetme

Sertifika kimlik bilgileri, aşağıdaki yöntemlerden birini kullanarak Azure portalından Azure AD'de istemci uygulaması ile ilişkilendirebilirsiniz:

### <a name="uploading-the-certificate-file"></a>Sertifika dosyası karşıya yükleniyor

İstemci uygulaması Azure uygulama kaydında:
1. Seçin **sertifikaları ve parolaları**. 
2. Tıklayarak **sertifikayı karşıya yükle** ve karşıya yüklemek için sertifika dosyasını seçin.
3. **Ekle**'yi tıklatın.
  Sertifika karşıya yüklendikten sonra parmak izi, başlangıç tarihi ve bitiş değerleri görüntülenir. 

### <a name="updating-the-application-manifest"></a>Uygulama bildirimi güncelleştiriliyor

Bir sertifikanın basılı olan, işlem yapmanız gerekir:

- `$base64Thumbprint`, base64 olduğu Sertifika Karması kodlama
- `$base64Value`, base64 olduğu sertifika ham verileri kodlama

Ayrıca uygulama bildiriminde anahtarı tanımlayan bir GUID sağlamanız gerekir (`$keyId`).

İstemci uygulaması Azure uygulama kaydında:
1. Seçin **bildirim** uygulama bildirimini açın.
2. Değiştirin *keyCredentials* aşağıdaki şemayı kullanarak yeni sertifika bilgisi özelliği.

   ```
   "keyCredentials": [
       {
           "customKeyIdentifier": "$base64Thumbprint",
           "keyId": "$keyid",
           "type": "AsymmetricX509Cert",
           "usage": "Verify",
           "value":  "$base64Value"
       }
   ]
   ```
3. Uygulama bildirimine düzenlemeleri kaydetmek ve bildirim Azure AD'ye yükleyin. 

   `keyCredentials` Özelliği olduğundan birden çok değerli birden çok daha zengin anahtar yönetim sertifikalarını karşıya yükleyebilirsiniz.
   
## <a name="code-sample"></a>Kod örneği

Kod örneğinde [sertifikaları ile arka plan programı uygulamalarında Azure AD'ye kimlik doğrulaması](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential) nasıl uygulama kimlik doğrulaması için kendi kimlik bilgilerini kullandığını gösterir. Ayrıca nasıl gösterir [otomatik olarak imzalanan bir sertifika oluşturmak](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential#create-a-self-signed-certificate) kullanarak `New-SelfSignedCertificate` Powershell komutu. Ayrıca yararlanın ve kullanabilirsiniz [uygulama oluşturma betikleri](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential/blob/master/AppCreationScripts/AppCreationScripts.md) sertifikaları oluşturmak, parmak izini işlem ve benzeri.
