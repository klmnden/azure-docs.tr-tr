---
title: Xamarin iOS konuları (Microsoft kimlik doğrulama kitaplığı .NET için) | Azure
description: Xamarin iOS ile Microsoft kimlik doğrulama kitaplığı .NET (MSAL.NET) kullanırken hakkında belirli değerlendirmeler öğrenin.
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/24/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: bf236bff2300129ec97d3b8946c4c2a2748bca77
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65602139"
---
# <a name="xamarin-ios-specific-considerations-with-msalnet"></a>Xamarin iOS özgü MSAL.NET hakkında konuları
Xamarin iOS, MSAL.NET kullanırken dikkate almanız gereken birkaç nokta vardır

- [İOS 12 ve kimlik doğrulaması ile ilgili bilinen sorunlar](#known-issues-with-ios-12-and-authentication)
- [Geçersiz kılma ve uygulama `OpenUrl` işlevi `AppDelegate`](#implement-openurl)
- [Anahtar zinciri gruplarını etkinleştir](#enable-keychain-groups)
- [Belirteç önbelleği paylaşımını etkinleştir](#enable-token-cache-sharing-across-ios-applications)
- [Anahtarlık erişimi etkinleştir](#enable-keychain-access)

## <a name="known-issues-with-ios-12-and-authentication"></a>İOS 12 ve kimlik doğrulaması ile ilgili bilinen sorunlar
Microsoft .NET Framework bir [güvenlik danışma belgesi](https://github.com/aspnet/AspNetCore/issues/4647) iOS12 bazı kimlik doğrulama türleri arasındaki uyumsuzluk hakkında bilgi sağlamak için. Uyumsuzluk sonları sosyal, WSFed ve OIDC oturum açma bilgileri. Bu öneri geliştiricilerin uygulamalarına iOS12 ile uyumlu olmak için ASP.NET tarafından eklenen geçerli güvenlik kısıtlamaları kaldırmak için ne üzerinde yönergeler de sağlar.  

Xamarin iOS MSAL.NET uygulamaları geliştirirken, Web siteleri için iOS 12 oturum açmaya çalışırken, sonsuz bir döngüye görebilirsiniz (buna benzer [ADAL sorunu](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/issues/1329). 

Ayrıca, bir iOS 12 ile ASP.NET Core OIDC kimlik doğrulaması sonu görebilirsiniz açıklandığı Safari [WebKit sorunu](https://bugs.webkit.org/show_bug.cgi?id=188165).

## <a name="implement-openurl"></a>OpenUrl uygulayın

Geçersiz kılmak gereken ilk `OpenUrl` yöntemi `FormsApplicationDelegate` türetilmiş sınıf ve çağrı `AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs`.

```csharp
public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
{
    AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(url);
    return true;
}
```

Ayrıca gerekir URL düzeni tanımlamak için uygulamanızın başka bir uygulama arayın, belirli bir form için yeniden yönlendirme URL'si olması ve bu yeniden yönlendirme URL'sini kaydetmek izinler gerektirir [Azure portalında](https://portal.azure.com).

## <a name="enable-keychain-groups"></a>Anahtar zinciri gruplarını etkinleştir

Belirteç önbelleğe iş ve sahip olmak için `AcquireTokenSilentAsync` yöntemi iş birden çok adımlar izlenmelidir:
1. Anahtarlık erişimi etkinleştirmek, *`* Entitlements.plist* belirtin ve dosya **Anahtarlık grupları** paket tanımlayıcınızı içinde.
2. Seçin *`*Entitlements.plist*`* dosyası **özel yetkilendirmeler** iOS projesi seçenekleri pencerenin alanının **paket imzalama görünümü**.
3. Bir sertifika imzalama, XCode aynı Apple kimliğini kullanmadığından emin olun.

## <a name="enable-token-cache-sharing-across-ios-applications"></a>İOS uygulamaları arasında paylaşımı belirteç önbelleği etkinleştir

MSAL başlangıç 2.x, birden çok uygulamada belirteç önbelleği kalıcı hale getirilmesine yönelik kullanmak üzere bir Anahtarlık güvenlik grubu belirtebilirsiniz. Bu belirteç önbelleği ile geliştirilen dahil olmak üzere aynı anahtar zinciri güvenlik grubuna sahip çeşitli uygulamalar arasında paylaşmanıza olanak tanır [ADAL.NET](https://aka.ms/adal-net), MSAL.NET Xamarin.iOS uygulamaları ve geliştirilmiş yerel iOS uygulamaları ile [ADAL.objc](https://github.com/AzureAD/azure-activedirectory-library-for-objc) veya [MSAL.objc](https://github.com/AzureAD/microsoft-authentication-library-for-objc)).

Belirteç önbelleğini paylaşmak çoklu oturum açma (SSO) tüm aynı Anahtarlık güvenlik grubunu kullanan uygulamalar sağlar.

Çoklu oturum açmayı etkinleştirmek için ayarlamanız gerekir `PublicClientApplication.iOSKeychainSecurityGroup` özelliğini tüm uygulamalar aynı değeri.

Buna örnek olarak MSAL kullanarak v3.x olacaktır:
```csharp
var builder = PublicClientApplicationBuilder
     .Create(ClientId)
     .WithIosKeychainSecurityGroup("com.microsoft.msalrocks")
     .Build();
```

Buna örnek olarak MSAL kullanarak v2.7.x olacaktır:

```csharp
PublicClientApplication.iOSKeychainSecurityGroup = "com.microsoft.msalrocks";
```

> [!NOTE]
> `KeychainSecurityGroup` Özelliği kullanımdan kaldırıldı. MSAL, daha önce 2.x, geliştiricilerin kullanırken teamıd değeri önekini dahil etmeyi zorunlu `KeychainSecurityGroup` özelliği. 
> 
> Şimdi MSAL başlangıç 2.7.x, MSAL teamıd değeri önek çalışma zamanı sırasında kullanırken çözülecektir `iOSKeychainSecurityGroup` özelliği. Bu özelliği kullanırken, değer teamıd değeri önek içermemelidir. 
> 
> Yeni `iOSKeychainSecurityGroup` özelliği teamıd değeri sağlamak için geliştiriciler gerektirmez. `KeychainSecurityGroup` Özellik kullanılmıyor şimdi. 

## <a name="enable-keychain-access"></a>Anahtarlık erişimi etkinleştir

MSAL, 2.x ve ADAL 4.x teamıd değeri çoklu oturum açma (SSO) aynı yayımcının uygulamaları arasında sağlamak kimlik doğrulama kitaplıkları sağlayan anahtar zinciri erişim için kullanılır. 

Nedir [TeamIdentifierPrefix](/xamarin/ios/deploy-test/provisioning/entitlements?tabs=vsmac) (teamıd değeri)? App Store içinde benzersiz tanımlayıcı (şirket veya kişisel) var. Uygulama kimliği, bir uygulama için benzersizdir. Birden fazla uygulamanız varsa, tüm uygulamalar için teamıd değeri aynı olacaktır ancak AppID farklı olacaktır. Anahtarlık erişim grubunun teamıd değeri tarafından her grup için sistem tarafından otomatik olarak önekidir. Bu, nasıl işletim sistemini zorunlu kılar aynı yayımcıya ait uygulamalar paylaşılan Anahtarlıkta erişebildiğinizden emin olur. 

Başlatılırken `PublicClientApplication`, alırsanız bir `MsalClientException` iletisiyle: `TeamId returned null from the iOS keychain...`, Xamarin iOS uygulamasını aşağıdakileri yapmanız gerekir:

1. VS'de, hata ayıklama sekmesi altında nameOfMyApp.iOS özellikleri için Git...
2. Daha sonra iOS paket grubu imzalama gidin 
3. Özel yetkilendirmeler altında tıklayın... uygulamanızdan Entitlements.plist dosyası seçin
4. İOS uygulamasının csproj dosyasında artık dahil bu satırı olmalıdır: `<CodesignEntitlements>Entitlements.plist</CodesignEntitlements>`
5. **Yeniden** proje.

Bu *ayrıca* Anahtarlık erişimi etkinleştirmek için `Entitlements.plist` kullanarak dosya erişim grubu altında veya kendi:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>keychain-access-groups</key>
  <array>
    <string>$(AppIdentifierPrefix)com.microsoft.adalcache</string>
  </array>
</dict>
</plist>
```

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla ayrıntı sağlanır [iOS dikkate alınacak belirli noktalar](https://github.com/azure-samples/active-directory-xamarin-native-v2#ios-specific-considerations) paragraf aşağıdaki örnek 's Benioku.MD dosyası:

Örnek | Platform | Açıklama 
------ | -------- | -----------
[https://github.com/Azure-Samples/active-directory-xamarin-native-v2](https://github.com/azure-samples/active-directory-xamarin-native-v2) | Xamarin iOS, Android, UWP | MSAL MSA ve AAD V2.0 uç noktası aracılığıyla Azure AD kimlik doğrulaması ve Microsoft Graph ile elde edilen belirteç erişmek için nasıl kullanılacağını gösteren basit Xamarin.Forms uygulaması. <br>![Topoloji](media/msal-net-xamarin-ios-considerations/topology.png)