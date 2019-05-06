---
title: Arka plan programı uygulama çağıran web API'leri (Uygulama Yapılandırması) - Microsoft kimlik platformu
description: Daemon uygulamasının nasıl oluşturulacağını öğrenin çağrıları veritabanını web API'leri (Uygulama Yapılandırması)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: d8d377db827a6548c380128624c21f4ae7896aff
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65075333"
---
# <a name="daemon-app-that-calls-web-apis---code-configuration"></a>Arka plan programı uygulama çağrıları web API'leri - kod yapılandırma

Kod çağrıları web API'leri arka plan programı uygulamanız için yapılandırmayı öğrenin.

## <a name="msal-libraries-supporting-daemon-apps"></a>MSAL kitaplıkları destekleyen arka plan programları

Arka plan programı uygulamaları destekleyen Microsoft kitaplıkları şunlardır:

  MSAL kitaplığı | Açıklama
  ------------ | ----------
  ![MSAL.NET](media/sample-v2-code/logo_NET.png) <br/> MSAL.NET  | Bir arka plan programı uygulaması derlemek için desteklenen platformları olan .NET Framework ve .NET Core platformlar (UWP değil, Xamarin.iOS ve Xamarin.Android bu platformları olarak ortak istemci uygulamaları oluşturmak için kullanılır)
  ![Python](media/sample-v2-code/logo_python.png) <br/> MSAL.Python | Genel önizlemeye sunuldu - sürmekte olan geliştirme
  ![Java](media/sample-v2-code/logo_java.png) <br/> MSAL.Java | Genel önizlemeye sunuldu - sürmekte olan geliştirme

## <a name="configuration-of-the-authority"></a>Yapılandırma yetkilisi

Temsilci izinleri, ancak uygulama izinleri, arka plan programı uygulamalarını kullanma koşuluyla, kendi *hesap türü desteklenen* olamaz *hesaplarında herhangi bir kuruluş dizinini ve kişisel Microsoft hesapları ( Örneğin, Skype, Xbox, Outlook.com)*. Aslında, kişisel Microsoft hesapları için arka plan programı uygulamaya izin vermek için bir kiracı Yöneticisi yok yoktur. Seçmeniz gerekir *hesapları Kuruluşumdaki* veya *hesapları herhangi bir kuruluştaki*.

Bu nedenle uygulama yapılandırmasında belirtilen yetkilisi Kiracı ed (Kiracı kimliği veya kuruluşunuz ile ilişkilendirilen bir etki alanı adı belirterek) olmalıdır. Bir ISV ve çok kiracılı bir araç sağlamak istiyorsanız, kullanabileceğiniz `organizations`. Ancak, müşterileriniz için yönetici onayı vermek anlatan gerekecek göz önünde bulundurun. Bkz: [tamamını bir kiracı için onay isteme](v2-permissions-and-consent.md#requesting-consent-for-an-entire-tenant) Ayrıntılar için

## <a name="application-configuration-and-instantiation"></a>Uygulama Yapılandırması ve örnek oluşturma

MSAL kitaplıklarında, istemci kimlik bilgileri (parola veya sertifika) gizli bir istemci uygulaması oluşturma işleminin bir parametre olarak geçirilir.

> [!IMPORTANT]
> Uygulamanızı bir konsol uygulaması olsa bile bir arka plan programı uygulamasıysa, bir hizmet olarak çalışan, gizli bir istemci uygulaması olması gerekir.

### <a name="msalnet"></a>MSAL.NET

Ekleme [Microsoft.IdentityClient](https://www.nuget.org/packages/Microsoft.Identity.Client) uygulamanıza NuGet paketi.

MSAL.NET ad alanı kullanın

```CSharp
using Microsoft.Identity.Client;
```

Arka plan programı uygulama tarafından sunulan bir `IConfidentialClientApplication`

```CSharp
IConfidentialClientApplication app;
```

Bir uygulama gizli anahtarı ile bir uygulama oluşturmak için kod aşağıdaki gibidir:

```CSharp
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
           .WithClientSecret(config.ClientSecret)
           .WithAuthority(new Uri(config.Authority))
           .Build();
```

Bir sertifika ile bir uygulama oluşturmak için kod aşağıdaki gibidir:

```CSharp
X509Certificate2 certificate = ReadCertificate(config.CertificateName);
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
    .WithCertificate(certificate)
    .WithAuthority(new Uri(config.Authority))
    .Build();
```

### <a name="msalpython"></a>MSAL.Python

```Python
# Create a preferably long-lived app instance which maintains a token cache.

app = msal.ConfidentialClientApplication(
    config["client_id"], authority=config["authority"],
    client_credential=config["secret"],
    # token_cache=...  # Default cache is in memory only.
                       # You can learn how to use SerializableTokenCache from
                       # https://msal-python.rtfd.io/en/latest/#msal.SerializableTokenCache

    )
```

### <a name="msaljava"></a>MSAL.Java

```Java
PrivateKey key = getPrivateKey();
X509Certificate publicCertificate = getPublicCertificate();

// create clientCredential with public and private key
IClientCredential credential = ClientCredentialFactory.create(key, publicCertificate);

ConfidentialClientApplication cca = ConfidentialClientApplication
  .builder(CLIENT_ID, credential)
  .authority(AUTHORITY_MICROSOFT)
  .build();
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Arka plan programı app - uygulama belirteçlerini almak](./scenario-daemon-acquire-token.md)