---
title: "Sertifika kimlik bilgileri Azure AD içinde | Microsoft Docs"
description: "Bu makalede kayıt ve uygulama kimlik doğrulaması için sertifika kimlik bilgileri kullanımını açıklanır"
services: active-directory
documentationcenter: .net
author: navyasric
manager: mtillman
editor: 
ms.assetid: 88f0c64a-25f7-4974-aca2-2acadc9acbd8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/02/2017
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 9267a7d2b27930fbcd0aa70ec9e2ddc62b6b0b2a
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="certificate-credentials-for-application-authentication"></a>Uygulama kimlik doğrulaması için sertifika kimlik bilgileri

Azure Active Directory OAuth 2.0 istemci kimlik bilgilerini verme akışına ve On-temsili akış kimlik doğrulaması için örneğin, kendi kimlik bilgilerini kullanmak bir uygulama sağlar.
Bir kullanılabilir kimlik bilgisi uygulama sahip bir sertifika ile imzalanmış bir JSON Web Token(JWT) onaylama biçimidir.

## <a name="format-of-the-assertion"></a>Onaylama işlemi biçimi
Onaylama işlemi hesaplamak için büyük olasılıkla çok birini kullanmak istediğiniz [JSON Web belirteci](https://jwt.io/) kitaplıkları tercih ettiğiniz dilde. Belirtecin tarafından taşınan bilgiler verilmiştir:

#### <a name="header"></a>Üst bilgi

| Parametre |  Açıklama |
| --- | --- | --- |
| `alg` | Olmalıdır **RS256** |
| `typ` | Olmalıdır **JWT** |
| `x5t` | X.509 Sertifika SHA-1 parmak izi olmalıdır |

#### <a name="claims-payload"></a>Talep (yükü)

| Parametre |  Açıklama |
| --- | --- | --- |
| `aud` | İzleyici: olmalıdır  **https://login.microsoftonline.com/*tenant_Id*  /oauth2/token ** |
| `exp` | Sona erme tarihi: belirteç süresinin dolduğu tarih. Saat 1 Ocak 1970'ten saniye sayısı olarak temsil edilir (1970'ten-01-01T0:0:0Z) UTC belirteci geçerlilik süresinin dolduğu zaman kadar.|
| `iss` | Sertifikayı veren: client_id (istemci hizmeti uygulama kimliği) olması gerekir |
| `jti` | GUID: JWT kimliği |
| `nbf` | Önce değil: önce belirteç kullanılamaz tarih. Saat 1 Ocak 1970'ten saniye sayısı olarak temsil edilir (1970'ten-01-01T0:0:0Z) UTC belirteç yayımlandığında zamana kadar. |
| `sub` | Konu: olarak için `iss`, client_id (istemci hizmeti uygulama kimliği) olması gerekir |

#### <a name="signature"></a>İmza
İmza sertifikası açıklandığı gibi uygulama hesaplanır [JSON Web belirteci RFC7519 belirtimi](https://tools.ietf.org/html/rfc7519)

### <a name="example-of-a-decoded-jwt-assertion"></a>Kodu çözülmüş JWT onaylama örneği
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

### <a name="example-of-an-encoded-jwt-assertion"></a>Kodlanmış bir JWT onaylama örneği
Aşağıdaki dizeyi kodlanmış onaylama örneğidir. Dikkatle bakarsanız, nokta (.) ile ayrılmış üç bölüm görürsünüz.
İlk bölüm başlığı yükü ve son ilk iki bölümü içeriğinden sertifikalarla hesaplanan imza ikinci kodlar.
```
"eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJhdWQiOiJodHRwczpcL1wvbG9naW4ubWljcm9zb2Z0b25saW5lLmNvbVwvam1wcmlldXJob3RtYWlsLm9ubWljcm9zb2Z0LmNvbVwvb2F1dGgyXC90b2tlbiIsImV4cCI6MTQ4NDU5MzM0MSwiaXNzIjoiOTdlMGE1YjctZDc0NS00MGI2LTk0ZmUtNWY3N2QzNWM2ZTA1IiwianRpIjoiMjJiM2JiMjYtZTA0Ni00MmRmLTljOTYtNjVkYmQ3MmMxYzgxIiwibmJmIjoxNDg0NTkyNzQxLCJzdWIiOiI5N2UwYTViNy1kNzQ1LTQwYjYtOTRmZS01Zjc3ZDM1YzZlMDUifQ.
Gh95kHCOEGq5E_ArMBbDXhwKR577scxYaoJ1P{a lot of characters here}KKJDEg"
```

### <a name="register-your-certificate-with-azure-ad"></a>Azure AD ile sertifikanızı kaydedin
Sertifika kimlik bilgileri Azure AD istemci uygulamasında ilişkilendirmek için uygulama bildirimi düzenlemeniz gerekir.
Bir sertifikanın tutma sahip, işlem gerekir:
- `$base64Thumbprint`, base64 olduğu sertifikasını karma kodlama
- `$base64Value`, base64 olduğu sertifika ham verileri kodlama

Ayrıca uygulama bildiriminde anahtarını tanımlamak için bir GUID sağlamanız gerekir (`$keyId`)

Azure uygulaması kaydı istemci uygulaması için uygulama bildirimini açın ve değiştirmek *keyCredentials* aşağıdaki Şeması'nı kullanarak yeni sertifika bilgisi özelliğiyle:
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

Uygulama bildirimine düzenlemeleri kaydetmek ve Azure AD ile yükleyin. KeyCredentials özelliği birden çok değerli olduğundan daha zengin anahtar yönetimi için birden çok sertifika karşıya.
