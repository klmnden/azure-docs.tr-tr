---
title: Azure AD Xamarin kullanmaya başlama | Microsoft Docs
description: Oturum açma için Azure AD ile tümleştirin ve OAuth kullanarak Azure AD ile korunan API'leri çağırmak Xamarin uygulamaları oluşturun.
services: active-directory
documentationcenter: xamarin
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 198cd2c3-f7c8-4ec2-b59d-dfdea9fe7d95
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: b0a20c2e6524b0c466f5c45578e0ba8eaad351ea
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60298861"
---
# <a name="quickstart-build-a-xamarin-app-that-integrates-microsoft-sign-in"></a>Hızlı Başlangıç: Microsoft oturum açma tümleşik bir Xamarin uygulaması derleme

[!INCLUDE [active-directory-develop-applies-v1-adal](../../../includes/active-directory-develop-applies-v1-adal.md)]

Xamarin, C# ' de iOS, Android ve Windows (mobil cihazlar ve bilgisayarlar) üzerinde çalışan mobil uygulamalar yazabilirsiniz. Xamarin kullanarak bir uygulama oluşturuyorsanız, Azure Active Directory (Azure AD) Azure AD hesaplarına sahip kullanıcıların kimlik doğrulamasını kolaylaştırır. Uygulama, Office 365 API'lerini veya Azure API gibi Azure AD tarafından korunan tüm web API'si de güvenli bir şekilde kullanabilir.

Korunan kaynaklara erişmesi gereken Xamarin uygulamaları için Azure AD Active Directory Authentication Library (ADAL) sağlar. ADAL'ın tek amacı, erişim belirteçlerini almak uygulamaları kolaylaştırır sağlamaktır. Ne kadar kolay olduğunu göstermek için bu makalede DirectorySearcher uygulamaları nasıl oluşturacağınızı gösterir:

* İOS, Android, Windows Masaüstü, Windows Phone ve Windows Store üzerinde çalıştırın.
* Kullanıcıların kimliğini doğrulama ve belirteçleri almak için Azure AD Graph API için bir tek taşınabilir sınıf kitaplığı (PCL) kullanın.
* Belirli bir UPN ile kullanıcılar için bir dizini arayın.

## <a name="prerequisites"></a>Önkoşullar

* İndirme [çatı projesini](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/skeleton.zip), veya indirme [tamamlanan örnek](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip). Her yükleme, bir Visual Studio 2013 çözümüdür.
* Ayrıca, kullanıcıların oluşturmak ve uygulamayı kaydetmek Azure AD kiracısı gerekir. Henüz bir kiracınız yoksa [nasıl kiracı alınabileceğini öğrenin](quickstart-create-new-tenant.md).

Hazır olduğunuzda, sıradaki dört bölüm konusundaki yordamları izleyin.

## <a name="step-1-set-up-your-xamarin-development-environment"></a>1. Adım: Xamarin geliştirme ortamınızı ayarlama

Bu öğreticide, iOS, Android ve Windows projeleri içerdiğinden, hem Visual Studio ve Xamarin gerekir. Gerekli ortam oluşturmak için işlemin tamamlanması [kümesi ayarlama ve Visual Studio ve Xamarin'i yükleme](https://msdn.microsoft.com/library/mt613162.aspx) MSDN'de. Yönergeleri yüklemeleri tamamlanması beklerken Xamarin hakkında daha fazla bilgi edinmek için gözden geçirebileceğiniz malzemesini içerir.

Kurulumu tamamladıktan sonra çözümü Visual Studio'da açın. Burada, altı projeleri bulacaksınız: beş platforma özel Proje ve bir PCL, tüm platformlar arasında paylaşılan DirectorySearcher.cs.

## <a name="step-2-register-the-directorysearcher-app"></a>2. Adım: DirectorySearcher uygulamayı kaydetme

Belirteçlerini almak sağlamak amacıyla, ilk Azure AD kiracınıza kaydetme ve Azure AD Graph API'sine erişim izni vermeniz gerekir. Bunu yapmak için:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Üst çubuğunda, hesabınıza tıklayın. Ardından, altında **dizin** listesinde, istediğiniz uygulamayı kaydetmek için Active Directory kiracısı seçin.
3. Sol bölmede **Tüm hizmetler**'e tıklayın ve ardından **Azure Active Directory**'yi seçin.
4. **Uygulama kayıtları**'na tıklayın ve **Ekle**'yi seçin.
5. Yeni bir **yerel istemci uygulaması**, yönergeleri izleyin.
   * **Adı** kullanıcılara uygulamayı açıklar.
   * **Yeniden Yönlendirme URI'si**, Azure AD'nin belirteç yanıtlarını döndürmek için kullandığı şema ve dize bileşimidir. Bir değer girin (örneğin, `http://DirectorySearcher`).
6. Azure AD kaydı tamamladıktan sonra uygulamayı benzersiz bir uygulama kimliği atar. Değeri Şuradan Kopyala: **uygulama** daha sonra gerekeceği için sekmesinde.
7. Üzerinde **ayarları** sayfasında **gerekli izinler**ve ardından **Ekle**.
8. Seçin **Microsoft Graph** API olarak. Altında **Temsilcili izinler**, ekleme **dizin verilerini okuma** izni. 
   Bu eylem, kullanıcılar için Graph API sorgulamak uygulamayı etkinleştirir.

## <a name="step-3-install-and-configure-adal"></a>3. Adım: Yükleme ve ADAL'ı yapılandırma

Azure AD'de bir uygulama edindikten sonra ADAL'ı yükleyebilir ve kimlikle ilgili kodunuzu yazın. Azure AD ile iletişim kurmak ADAL'ı etkinleştirmek için uygulama kaydı hakkında bazı bilgiler verir.

1. ADAL, Paket Yöneticisi konsolu kullanarak DirectorySearcher projeye ekleyin.

    ```
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirectorySearcherLib
    ```

    ```
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Android
    ```

    ```
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Desktop
    ```

    ```
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-iOS
    ```

    ```
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Universal
    ```

    Her proje için iki kitaplığı başvurularını eklendiğine dikkat edin: ADAL ve platforma özgü bölümü PCL kısmı.
2. DirectorySearcherLib projesinde DirectorySearcher.cs açın.
3. Sınıf üye değerlerinin Azure portalına girilen değerlerle değiştirin. ADAL kullandığında, kodunuzun bu değerleri gösterir.

   * *Kiracı* Azure AD kiracınızın (örneğin, contoso.onmicrosoft.com) etki alanıdır.
   * *ClientID* portaldan kopyaladığınız uygulamanın istemci kimliğidir.
   * *ReturnUri* yeniden yönlendirme URI'si portalda girdiğiniz olan (örneğin, `http://DirectorySearcher`).

## <a name="step-4-use-adal-to-get-tokens-from-azure-ad"></a>4. Adım: Azure AD belirteçlerini almak için ADAL'ı kullanın

Neredeyse tüm uygulamanın kimlik doğrulaması mantığı kaynaklandığını `DirectorySearcher.SearchByAlias(...)`. Platforma özgü projelerinde gerekli olan tek şey bir bağlam parametresine geçirilecek `DirectorySearcher` PCL.

1. DirectorySearcher.cs açın ve ardından yeni bir parametre ekleyin `SearchByAlias(...)` yöntemi. `IPlatformParameters` ADAL kimlik doğrulaması gerçekleştirmesine gerek platforma özgü nesneler kapsülleyen bağlamsal parametredir.

    ```csharp
    public static async Task<List<User>> SearchByAlias(string alias, IPlatformParameters parent)
    {
    ```

2. Başlatma `AuthenticationContext`, ADAL'ın birincil sınıf. Bu eylem ADAL geçirir Azure AD ile iletişim kurmak için ihtiyaç duyduğu koordinatlar.
3. Çağrı `AcquireTokenAsync(...)`, kabul eden `IPlatformParameters` nesnesi ve bir belirteç uygulamaya geri dönmek gerekli olan kimlik doğrulaması akışı çağırır.

    ```csharp
    ...
        AuthenticationResult authResult = null;
        try
        {
            AuthenticationContext authContext = new AuthenticationContext(authority);
            authResult = await authContext.AcquireTokenAsync(graphResourceUri, clientId, returnUri, parent);
        }
        catch (Exception ee)
        {
            results.Add(new User { error = ee.Message });
            return results;
        }
    ...
    ```

    `AcquireTokenAsync(...)` ilk (Bu durumda Graph API'si) istenen kaynak için bir belirteç (önbelleğe alma veya eski belirteçleri yenileme aracılığıyla), kimlik bilgilerini girmesini istemeden döndürmeyi dener. Gerekirse, bu kullanıcıları Azure AD oturum açma sayfasının istenen belirteci alınırken önce gösterir.
4. Graph API isteği erişim belirteci ekleme **yetkilendirme** üst bilgi:

    ```csharp
    ...
        request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", authResult.AccessToken);
    ...
    ```

Tüm için `DirectorySearcher` PCL ve uygulama kimlikle ilgili kod. Kalan tek şey çağrılacak `SearchByAlias(...)` yöntemi her platformun görünümlerinde ve UI yaşam döngüsü doğru bir şekilde işlemek için bir kod eklemeniz gerektiğinde.

### <a name="android"></a>Android
1. MainActivity.cs içinde bir çağrı ekleyin `SearchByAlias(...)` işleyici düğmesine tıklayın:

    ```csharp
    List<User> results = await DirectorySearcher.SearchByAlias(searchTermText.Text, new PlatformParameters(this));
    ```
2. Geçersiz kılma `OnActivityResult` yaşam döngüsü yöntemi herhangi bir kimlik doğrulaması iletmek için uygun yöntemin yönlendirir. ADAL bir yardımcı yöntem için bu Android sağlar:

    ```csharp
    ...
    protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
    {
        base.OnActivityResult(requestCode, resultCode, data);
        AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
    }
    ...
    ```

### <a name="windows-desktop"></a>Windows Masaüstü

MainWindow.xaml.cs içinde çağrı yapmak `SearchByAlias(...)` geçirerek bir `WindowInteropHelper` masaüstünde ın `PlatformParameters` nesnesi:

```csharp
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

#### <a name="ios"></a>iOS
DirSearchClient_iOSViewController.cs, iOS, `PlatformParameters` nesne görünüm denetleyicisi başvuru alır:

```csharp
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

### <a name="windows-universal"></a>Windows Evrensel
Windows Evrensel MainPage.xaml.cs açın ve ardından uygulama `Search` yöntemi. Bu yöntem, kullanıcı Arabirimi gerektiği şekilde güncelleştirmek için paylaşılan projede bir yardımcı yöntem kullanır.

```csharp
...
List<User> results = await DirectorySearcherLib.DirectorySearcher.SearchByAlias(SearchTermText.Text, new PlatformParameters(PromptBehavior.Auto, false));
...
```

Artık, kullanıcıların kimliklerini doğrulamak ve güvenli bir şekilde beş farklı platformlar arasında OAuth 2.0 kullanarak web API'leri çağırma Xamarin uygulamanız var.

## <a name="step-5-populate-your-tenant"></a>5. Adım: Kiracınızın Doldur 

Kullanıcılarla kiracınız zaten doldurulmuş yüklemediyseniz, bunu yapmak için zaman sunulmuştur.

1. DirectorySearcher uygulamanızı çalıştırın ve ardından kullanıcılardan birinin bilgilerinizle oturum açın.
2. UPN'lerine göre diğer kullanıcılar için arama yapın.

## <a name="next-steps"></a>Sonraki adımlar

ADAL ortak kimlik özellikleri uygulamanıza kolaylaştırır. Bunu tüm kirli çalışması, önbellek yönetimi gibi OAuth protokol desteği, kullanıcı bir oturum açma kullanıcı Arabirimi, sunma üstlenir ve yenileme belirteçleri süresi doldu. Tek bir API çağrısı yalnızca, bilmeniz gereken `authContext.AcquireToken*(…)`.

* İndirme [tamamlanan örnek](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip) (olmadan yapılandırma değerlerinize).
* Bilgi edinmek için nasıl [.NET Web API'si Azure AD ile güvenli](quickstart-v1-dotnet-webapi.md).
