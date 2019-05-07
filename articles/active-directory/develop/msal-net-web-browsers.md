---
title: Web tarayıcılar Microsoft kimlik doğrulama kitaplığı .NET | Azure
description: Xamarin Android ile Microsoft kimlik doğrulama kitaplığı .NET (MSAL.NET) kullanırken hakkında belirli değerlendirmeler öğrenin.
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: celested
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
ms.openlocfilehash: 350cb3fec4d325d6cf5848733c0bae18d5efacca
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65076848"
---
# <a name="using-web-browsers-in-msalnet"></a>Web tarayıcısı MSAL.NET kullanarak
Web tarayıcılar için etkileşimli kimlik doğrulaması gereklidir. Varsayılan olarak, MSAL.NET destekler [sistem web tarayıcısı](#system-web-browser-on-xamarinios-and-xamarinandroid) üzerinde Xamarin.iOS ve [Xamarin.Android](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/system-browser). Ancak [katıştırılmış bir Web tarayıcısı da etkinleştirebilirsiniz](#enable-embedded-webviews) gereksinimlerinizi (UX, çoklu oturum açma (SSO), güvenlik gereksinimini) bağlı olarak [Xamarin.iOS](#choosing-between-embedded-web-browser-or-system-browser-on-xamarinios) ve [Xamarin.Android](#choosing-between-embedded-web-browser-or-system-browser-on-xamarinandroid) uygulamaları. Ve getirebilirsiniz [dinamik olarak seçin](#detecting-the-presence-of-custom-tabs-on-xamarinandroid) kullanmak için hangi web tarayıcısı, Chrome veya Chrome özel sekmeler Android tarayıcı varlığını temel.

## <a name="web-browsers-in-msalnet"></a>Web tarayıcılarında MSAL.NET

Etkileşimli bir belirteç alınırken, iletişim kutusunun içeriği kitaplığı ancak (güvenlik belirteci hizmeti) STS tarafından sağlanmadı olduğunu anlamak önemlidir. Kimlik doğrulama uç noktası, bazı HTML ve bir web tarayıcısı ya da web denetimi işlenen etkileşimi denetimlerini JavaScript geri gönderir. HTML etkileşim işlemek STS sağlayan birçok avantaj sunar:

- Parola (yazılmışsa) hiçbir zaman uygulama ya da kimlik doğrulama kitaplığı tarafından depolanır.
- Diğer kimlik sağlayıcılardan (örneği için oturum açma iş Okul hesabı veya bir kişisel hesap MSAL veya Azure AD B2C ile bir sosyal hesap ile birlikte) için yeniden yönlendirmelerini sağlar.
- STS, kullanıcı birden çok faktörlü kimlik doğrulaması (MFA) (Windows Hello PIN girmek veya telefon numaraları veya bir kimlik doğrulama uygulaması telefonundan çağrılan) kimlik doğrulama aşamasında sağlayarak koşullu erişim denetimi sağlar. Burada gerekli çok faktörlü kimlik doğrulamasını, henüz ayarlanmamış durumlarda, kullanıcı bunu zamanında aynı iletişim ayarlayabilirsiniz.  Kullanıcı kullanıcıdan birincil telefonu numarasını girer ve kimlik doğrulaması uygulama yükleyip hesabını eklemek için bir QR etiket tarama yönlendirilir. Etkileşim temelli bu sunucu, harika bir deneyim oldu!
- Kullanıcının parola (eski parolayı ve yeni için ek alanlar vererek) süresi dolduğunda aynı bu iletişim kutusunda parola değiştirmesini sağlar.
- Azure AD Kiracı Yöneticisi tarafından denetlenen Kiracı veya uygulama (görüntüler) marka etkinleştirir / uygulama sahibi.
- Kullanıcıların kaynaklarına erişmesine izin vermek için onay etkinleştirir / adında yalnızca kimlik doğrulamasından sonra kapsamları.

## <a name="system-web-browser-on-xamarinios-and-xamarinandroid"></a>Xamarin.iOS ve Xamarin.Android sistem web tarayıcısı

Varsayılan olarak, MSAL.NET Xamarin.iOS ve Xamarin.Android sistem web tarayıcısı destekler. Kullanan yalnızca ADAL.NET STS ile etkileşimin barındırmak için **katıştırılmış** web tarayıcısı. Kullanıcı Arabirimi (diğer bir deyişle, değil .NET Core) sağlayan tüm platformlar için bir iletişim kutusu ekleme bir Web tarayıcı denetimi kitaplığı tarafından sağlanır. MSAL.NET ayrıca katıştırılmış web görünümü .NET Masaüstü ve WAB için UWP platformu kullanır. Ancak, bu varsayılan olarak yararlanır **sistem web tarayıcısı** Xamarin iOS ve Xamarin Android uygulamaları için. İOS, hatta işletim sistemi sürümüne bağlı olarak kullanmak üzere web görünümüne seçer (iOS12, iOS11 ve önceki sürümler).

Sistem tarayıcınızı kullanarak bir aracı gerek kalmadan web uygulamaları ve diğer uygulamalar ile SSO durum paylaşımı önemli avantajı vardır (Şirket portalı / Authenticator). Sistem, varsayılan olarak, Xamarin iOS ve Xamarin Android platformlarına yönelik MSAL.NET için kullanılan tarayıcıya bu platformlardaki sistem web tarayıcısı ekranın tamamını kaplar ve kullanıcı deneyimini daha iyidir. Sistem WebView iletişim kutusundan ayrılabilen değil. İOS, yine de kullanıcı sinir bozucu olabilecek uygulama geri çağrılacak tarayıcı için onay vermeniz gerekebilir.

### <a name="uwp-does-not-use-the-system-webview"></a>UWP System Webview kullanmaz

Nerede Bunlar zaten açılmış diğer sekmeleri olabilir tarayıcı kullanıcının gördüğü gibi Masaüstü uygulamaları için ancak System Webview başlatma subpar kullanıcı deneyimine yol açar. Ve kullanıcı kimlik doğrulaması oluştu, bu pencereyi kapatmak için bunları isteyen bir sayfa alır. Kullanıcı dikkat değil, bunlar (kimlik doğrulaması ilişkisiz diğer sekmeleri dahil) tüm işlem kapatabilirsiniz. Masaüstünde sistem tarayıcı yararlanarak de yerel bağlantı noktaları açma ve bunlar üzerinde dinleme uygulama için gelişmiş izinler yapmanızı şart koşabileceği gerektirir. Bir geliştirici, kullanıcı veya yönetici olarak, bu gereksinim hakkında isteksiz olabilir.

## <a name="enable-embedded-webviews"></a>Ekli Web görünümleri etkinleştirin 
Xamarin.iOS ve Xamarin.Android uygulamalarında ekli Web görünümleri de etkinleştirebilirsiniz. MSAL.NET 2.0.0-preview ile başlayarak, MSAL.NET de kullanmayı destekler **katıştırılmış** webview seçeneği. ADAL.NET için katıştırılmış webview desteklenen tek bir seçenektir.

Xamarin hedefleyen MSAL.NET kullanarak bir geliştirici olarak, ekli Web görünümleri ya da sistem tarayıcılar kullanmayı tercih edebilirsiniz. Seçtiğiniz hedeflemek istediğiniz kullanıcı deneyimi ve güvenlik endişelerini bağlı olarak budur.

Şu anda, MSAL.NET, Android ve iOS aracıları henüz desteklememektedir. Bu nedenle çoklu oturum açma (SSO) sağlamanız gerekiyorsa, sistem tarayıcı yine de daha iyi bir seçenek olabilir. Katıştırılmış bir web tarayıcısı ile aracıları destekleyen MSAL.NET biriktirme listesinde var.

### <a name="differences-between-embedded-webview-and-system-browser"></a>Katıştırılmış webview ve sistem tarayıcı arasındaki farklar 
MSAL.NET katıştırılmış webview ve sistem tarayıcı arasındaki visual bazı farklılıklar vardır.

**Etkileşimli oturum açma MSAL.NET kullanarak katıştırılmış Webview ile:**

![Katıştırılmış](media/msal-net-web-browsers/embedded-webview.png)

**Etkileşimli oturum açma sistemi tarayıcınızı kullanarak MSAL.NET ile:**

![Sistem tarayıcı](media/msal-net-web-browsers/system-browser.png)

### <a name="developer-options"></a>Geliştirici seçenekleri

MSAL.NET kullanarak bir geliştirici olarak, STS etkileşimli iletişim kutusunda görüntülemek için birkaç seçeneğiniz vardır:

- **Sistem tarayıcı.** Sistem tarayıcı Kitaplığı'nda varsayılan olarak ayarlanır. Android kullanıyorsanız, okuma [sistem tarayıcılar](msal-net-system-browser-android-considerations.md) belirli bilgileri hangi tarayıcılar kimlik doğrulaması için desteklenir. Android sistem tarayıcı kullanarak, Chrome özel sekmeler destekleyen bir tarayıcı cihaz sahip öneririz.  Aksi takdirde, kimlik doğrulaması başarısız olabilir. 
- **Katıştırılmış webview.** WebView MSAL.NET içinde gömülü kullanabilmeniz için vardır aşırı yüklemeleri `UIParent()` Oluşturucusu Android ve iOS için kullanılabilir.

    iOS:

    ```csharp
    public UIParent(bool useEmbeddedWebview)
    ```

    Android:

    ```csharp
    public UIParent(Activity activity, bool useEmbeddedWebview)
    ```

#### <a name="choosing-between-embedded-web-browser-or-system-browser-on-xamarinios"></a>Katıştırılmış bir web tarayıcısı veya sistem tarayıcıda Xamarin.iOS arasında seçim yapma

İOS uygulamanızda içinde `AppDelegate.cs` sistem tarayıcı veya katıştırılmış webview kullanabilirsiniz.

```csharp
// Use only embedded webview
App.UIParent = new UIParent(true);

// Use only system browser
App.UIParent = new UIParent();
```

#### <a name="choosing-between-embedded-web-browser-or-system-browser-on-xamarinandroid"></a>Katıştırılmış bir web tarayıcısı veya Xamarin.Android sistem tarayıcıda arasında seçim yapma

Android uygulamanıza içinde `MainActivity.cs` webview seçenekleri uygulamak nasıl karar verebilirsiniz.

```csharp
// Use only embedded webview
App.UIParent = new UIParent(Xamarin.Forms.Forms.Context as Activity, true);

// or
// Use only system browser
App.UIParent = new UIParent(Xamarin.Forms.Forms.Context as Activity);
```

#### <a name="detecting-the-presence-of-custom-tabs-on-xamarinandroid"></a>Özel sekmeler Xamarin.Android üzerinde varlığını algılama

Sistem web tarayıcısı ile tarayıcıda çalışan uygulamalar SSO'yu etkinleştirmek üzere kullanmak istediğiniz, ancak özel sekme destekli bir tarayıcıya kalmadan Android cihazlar için kullanıcı deneyimi hakkında kaygılı seçeneğini çağırarak karar varsa `IsSystemWebViewAvailable()` yönteminde < c 2 > `UIParent` . Bu yöntem döndürür `true` PackageManager özel sekmeler algılarsa ve `false` cihazda algılanan değil.

Bu yöntem ve gereksinimlerinizi tarafından döndürülen değere göre bir karar yapabilirsiniz:

- Kullanıcıya bir özel hata iletisi döndürebilir. Örneğin: "Lütfen Chrome ile kimlik doğrulaması devam etmek için Yükle" - veya-
- Katıştırılmış webview seçeneğine sıfırlamaya ve katıştırılmış bir webview olarak UI'ı başlatın.

Aşağıdaki kod, katıştırılmış webview seçeneğini gösterir:

```csharp
bool useSystemBrowser = UIParent.IsSystemWebviewAvailable();
if (useSystemBrowser)
{
    // A browser with custom tabs is present on device, use system browser
    App.UIParent = new UIParent(Xamarin.Forms.Forms.Context as Activity);
}
else
{
    // A browser with custom tabs is not present on device, use embedded webview
    App.UIParent = new UIParent(Xamarin.Forms.Forms.Context as Activity, true);
}

// Alternative:
App.UIParent = new UIParent(Xamarin.Forms.Forms.Context as Activity, !useSystemBrowser);

```

## <a name="net-core-does-not-support-interactive-authentication"></a>.NET core etkileşimli kimlik doğrulamasını desteklemez

.NET Core için belirteç edinme etkileşimli olarak kullanılamaz. Aslında, .NET Core UI henüz sağlamaz. Bir .NET Core uygulaması için etkileşimli oturum açma sağlamak istiyorsanız, bir kod ve etkileşimli olarak oturum açmak için gitmek için bir URL kullanıcıya sunmak uygulaması izin verebilirsiniz (bkz [cihaz kod akış](msal-authentication-flows.md#device-code)).