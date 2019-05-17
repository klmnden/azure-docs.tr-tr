---
title: Xamarin Android konuları (Microsoft kimlik doğrulama kitaplığı .NET için) | Azure
description: Xamarin Android ile Microsoft kimlik doğrulama kitaplığı .NET (MSAL.NET) kullanırken hakkında belirli değerlendirmeler öğrenin.
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
ms.openlocfilehash: cb0cfb06e95cadbb549f669e5d59bdb0d795c896
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65545876"
---
# <a name="xamarin-android-specific-considerations-with-msalnet"></a>Xamarin Android özgü MSAL.NET hakkında konuları
Bu makalede, Xamarin Android ile Microsoft kimlik doğrulama kitaplığı .NET (MSAL.NET) kullanırken dikkate alınacak belirli noktalar açıklanmaktadır.

Bu makalede, MSAL.NET için olan 3.x. MSAL.NET ilgileniyorsanız 2.x bkz [Xamarin Android özellikleri, MSAL.NET 2.x](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Xamarin-Android-specifics-2x).

## <a name="set-the-parent-activity"></a>Üst etkinlik

Xamarin.Android üzerinde geri etkileşimi gerçekleşen sonra belirtecini alır, böylece üst etkinliği ayarlamanız gerekir.

```csharp
var authResult = AcquireTokenInteractive(scopes)
 .WithParentActivityOrWindow(parentActivity)
 .ExecuteAsync();
```

## <a name="ensuring-control-goes-back-to-msal-once-the-interactive-portion-of-the-authentication-flow-ends"></a>Denetim sağlamak için MSAL bir kez kimlik doğrulaması akışı bitti etkileşimli kısmı döner
Android, geçersiz kılmanız gerekir. `OnActivityResult` yöntemi `Activity` ve AuthenticationContinuationHelper MSAL sınıfının SetAuthenticationContinuationEventArgs yöntemi çağırın.

```csharp
protected override void OnActivityResult(int requestCode, 
                                         Result resultCode, Intent data)
{
 base.OnActivityResult(requestCode, resultCode, data);
 AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(requestCode,
                                                                         resultCode,
                                                                         data);
}

```
Bu satırı etkileşimli kimlik doğrulaması akışı bölümünü sona erdi sonra Denetim MSAL için geri gider sağlar.

## <a name="update-the-android-manifest"></a>Android bildirimini güncelleştir
`AndroidManifest.xml` Aşağıdaki değerleri içermesi gerekir:
```csharp
<activity android:name="microsoft.identity.client.BrowserTabActivity">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="msal{client_id}" android:host="auth" />
         </intent-filter>
</activity>
```

## <a name="use-the-embedded-web-view-optional"></a>(İsteğe bağlı) ekli web görünümünü kullanın

Varsayılan olarak, Web uygulamalarında SSO ve diğer uygulamalar almak sistem web tarayıcısı MSAL.NET kullanır. Bazı nadir durumlarda, katıştırılmış web görünümü kullanmak istediğinizi belirtmek isteyebilirsiniz. Daha fazla bilgi için [MSAL.NET kullanan bir Web tarayıcısı](msal-net-web-browsers.md) ve [Android sistem tarayıcı](msal-net-system-browser-android-considerations.md).

```csharp
bool useEmbeddedWebView = !app.IsSystemWebViewAvailable;

var authResult = AcquireTokenInteractive(scopes)
 .WithParentActivityOrWindow(parentActivity)
 .WithEmbeddedWebView(useEmbeddedWebView)
 .ExecuteAsync();
```

## <a name="troubleshooting"></a>Sorun giderme
Bu, yalnızca yeni bir Xamarin.Forms uygulaması oluşturma ve MSAL.Net NuGet paketine başvuru ekleyin, çalışır.
Ancak, yükseltmek istiyorsanız MSAL.NET varolan bir Xamarin.Forms uygulamaya 1.1.2 önizlemek veya daha sonra yapı sorunlarıyla karşılaşabilirsiniz.

Bu sorunları gidermek için şunları yapmalısınız:
- Mevcut MSAL.NET NuGet paketi MSAL.NET Önizleme 1.1.2 güncelleştirin veya üzeri
- Xamarin.Forms otomatik olarak sürüm 2.5.0.122203 (Aksi halde bu sürüme güncelleştirin) için emin olun
- Xamarin.Android.Support.v4 otomatik olarak sürüm 25.4.0.2 (Aksi halde bu sürüme güncelleştirin) için emin olun
- Xamarin.Android.Support paketlerinin sürüm 25.4.0.2 hedefliyor olabilir
- Temizleme/yeniden oluşturma
- Visual Studio'da 1 en fazla paralel proje ayarlamayı deneyin yapıları (Seçenekler -> Projeler ve çözümler -> derleme ve Çalıştır -> en fazla paralel projede derleme sayısı)
- Alternatif olarak, komut satırından oluşturuyorsanız bunu kullanıyorsanız, komutu /m kaldırmayı deneyin.


### <a name="error-the-name-authenticationcontinuationhelper-does-not-exist-in-the-current-context"></a>Hata: Adı 'AuthenticationContinuationHelper', geçerli bağlamda yok

Bunun nedeni, Visual Studio Android.csproj* dosyanın doğru güncelleştirmedi olabilir. Bazen **<HintPath>** filepath netstandard13 yerine hatalı içeren **monoandroid90**.

```xml
<Reference Include="Microsoft.Identity.Client, Version=3.0.4.0, Culture=neutral, PublicKeyToken=0a613f4dd989e8ae,
           processorArchitecture=MSIL">
  <HintPath>..\..\packages\Microsoft.Identity.Client.3.0.4-preview\lib\monoandroid90\Microsoft.Identity.Client.dll</HintPath>
</Reference>
```

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi ve örnekler sağlanır [Android belirli konuları](https://github.com/azure-samples/active-directory-xamarin-native-v2#android-specific-considerations) paragraf aşağıdaki örnek 's Benioku.MD dosyası:

| Örnek | Platform | Açıklama |
| ------ | -------- | ----------- |
|[https://github.com/Azure-Samples/active-directory-xamarin-native-v2](https://github.com/azure-samples/active-directory-xamarin-native-v2) | Xamarin iOS, Android, UWP | MSAL MSA ve AADD v2.0 uç noktası aracılığıyla Azure AD kimlik doğrulaması ve Microsoft Graph ile elde edilen belirteç erişmek için nasıl kullanılacağını gösteren basit Xamarin.Forms uygulaması. <br>![Topoloji](media/msal-net-xamarin-android-considerations/topology.png) |