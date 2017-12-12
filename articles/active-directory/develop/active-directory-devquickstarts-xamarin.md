---
title: "Azure AD Xamarin Başlarken | Microsoft Docs"
description: "Oturum açma için Azure AD ile tümleştirilebilen ve OAuth kullanan Azure AD korumalı API'leri çağırmak Xamarin uygulamaları oluşturun."
services: active-directory
documentationcenter: xamarin
author: jmprieur
manager: mtillman
editor: 
ms.assetid: 198cd2c3-f7c8-4ec2-b59d-dfdea9fe7d95
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: article
ms.date: 01/07/2017
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 87e58e8df16f4b87b66a9ac0846be20e09073826
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="integrate-azure-ad-with-xamarin-apps"></a>Azure AD Xamarin uygulamaları ile tümleştirme
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Xamarin ile C# ' ta, iOS, Android ve Windows (mobil cihazları ve bilgisayarları) üzerinde çalışan mobil uygulamalar yazabilirsiniz. Xamarin kullanarak bir uygulama oluşturuyorsanız, Azure Active Directory (Azure AD), kendi Azure AD hesapları olan kullanıcıların kimliklerini doğrulamak kolaylaştırır. Uygulama Office 365 API'leri veya Azure API'sini gibi Azure AD tarafından korunan tüm web API de güvenli bir şekilde kullanmasını sağlayabilirsiniz.

Korunan kaynaklara erişim için gereken Xamarin uygulamaları için Azure AD Active Directory Authentication Library (ADAL) sağlar. ADAL tek amacı, uygulamaların erişim belirteçleri almak kolay hale getirmektir. Ne kadar kolay olduğunu göstermek için bu makalede DirectorySearcher uygulamalar oluşturmak nasıl gösterilmektedir:

* İOS, Android, Windows Masaüstü, Windows Phone ve Windows Mağazası'nı çalıştırın.
* Kullanıcıların kimliğini doğrulamak ve Azure AD grafik API'si belirteçleri almak için tek taşınabilir sınıf kitaplığı (PCL) kullanın.
* Verilen UPN olan kullanıcılar için bir dizini arayın.

## <a name="before-you-get-started"></a>Başlamadan önce
* Karşıdan [çatı projesini](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/skeleton.zip), veya indirme [tamamlanan örnek](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip). Her yükleme Visual Studio 2013 çözümüdür.
* Ayrıca, kullanıcılar oluşturmak ve uygulamayı kaydetmek Azure AD kiracısı gerekir. Bir kiracı yoksa [edinebileceğinizi öğrenin](active-directory-howto-tenant.md).

Hazır olduğunuzda İleri dört bölümlerdeki yordamları izleyin.

## <a name="step-1-set-up-your-xamarin-development-environment"></a>1. adım: Xamarin geliştirme ortamınızı ayarlayın
Bu öğretici iOS, Android ve Windows için projeleri içerdiğinden, Visual Studio ve Xamarin gerekir. Gerekli ortamı oluşturmak için işlemin tamamlanması [ayarlamak ayarlama ve Visual Studio ve Xamarin yükleme](https://msdn.microsoft.com/library/mt613162.aspx) MSDN'de. Yönergeleri yüklemeleri tamamlanması beklerken Xamarin hakkında daha fazla bilgi için gözden geçirebilirsiniz malzemesini içerir.

Kurulumu tamamladıktan sonra çözümü Visual Studio'da açın. Burada, altı projeleri bulacaksınız: beş platforma özel Proje ve bir PCL, tüm platformlarda paylaşılan DirectorySearcher.cs.

## <a name="step-2-register-the-directorysearcher-app"></a>2. adım: DirectorySearcher uygulamayı Kaydet
Belirteçleri almak uygulamayı etkinleştirmek için önce Azure AD kiracınızda kaydetmek ve Azure AD grafik API'sine erişim izni vermek gerekir. Bunu yapmak için:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Üst çubuğunda hesabınızı tıklatın. Ardından, altında **Directory** listesinde, uygulama kaydetmek istediğiniz Active Directory Kiracı seçin.
3. Tıklatın **daha Hizmetleri** sol bölmesinde ve seçip **Azure Active Directory**.
4. Tıklatın **uygulama kayıtlar**ve ardından **Ekle**.
5. Yeni **yerel istemci uygulaması**, istemleri izleyin.
  * **Ad** kullanıcılara uygulamasının açıklar.
  * **Yeniden yönlendirme URI'si** belirteci yanıtları döndürmek için Azure AD kullanır ve dize şeması bir birleşimidir. Bir değer (örneğin, http://DirectorySearcher) girin.
6. Kayıt tamamladıktan sonra Azure AD uygulama benzersiz uygulama kimliği atar. Değerinden kopyalama **uygulama** sekmesinde, çünkü daha sonra ihtiyacınız olacak.
7. Üzerinde **ayarları** sayfasında, **gerekli izinler**ve ardından **Ekle**.
8. Seçin **Microsoft Graph** API olarak. Altında **izinlere temsilci**, ekleme **dizin verilerini okuma** izni.  
Bu eylem, kullanıcılar için grafik API'si sorgulamak uygulama sağlar.

## <a name="step-3-install-and-configure-adal"></a>3. adım: Yükleme ve ADAL yapılandırma
Azure AD'de bir uygulamaya sahip olduğunuza göre ADAL yükleyin ve kimlikle ilgili kodunuzu yazın. Azure AD ile iletişim kurmak ADAL etkinleştirmek için uygulama kaydı hakkında bazı bilgileri verin.

1. ADAL Paket Yöneticisi konsolu kullanılarak DirectorySearcher projeye ekleyin.

    `
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirectorySearcherLib
    `

    `
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Android
    `

    `
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Desktop
    `

    `
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-iOS
    `

    `
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Universal
    `

    Her proje için iki kitaplığı başvuru eklendiğine dikkat edin: ADAL ve platforma özgü bölümü PCL kısmı.
2. DirectorySearcherLib projesinde DirectorySearcher.cs açın.
3. Sınıf üye değerlerinin Azure portalında girdiğiniz değerleri değiştirin. ADAL kullandığında kodunuzu bu değerleri gösterir.

  * *Kiracı* Azure AD kiracınız (örneğin, contoso.onmicrosoft.com) etki alanıdır.
  * *ClientID* portalından kopyalandığından uygulama istemci Kimliğini gösterir.
  * *ReturnUri* yeniden yönlendirme (örneğin, http://DirectorySearcher) portalda girdiğiniz URI'si değil.

## <a name="step-4-use-adal-to-get-tokens-from-azure-ad"></a>4. adım: Kullanım Azure AD'den belirteçleri almak için ADAL
Neredeyse tüm uygulamanın kimlik doğrulaması mantığı arasındadır `DirectorySearcher.SearchByAlias(...)`. Platforma özgü projelerinde gerekli olan tek şey bağlamsal bir parametreye geçirmek üzere `DirectorySearcher` PCL.

1. DirectorySearcher.cs açın ve ardından yeni bir parametre eklemek `SearchByAlias(...)` yöntemi. `IPlatformParameters`ADAL kimlik doğrulaması gerçekleştirmesine gerek platforma özgü nesneleri yalıtan bağlamsal parametresidir.

    ```C#
    public static async Task<List<User>> SearchByAlias(string alias, IPlatformParameters parent)
    {
    ```

2. Initialize `AuthenticationContext`, ADAL birincil sınıfının olduğu.  
Bu eylem ADAL geçirir gereken Azure AD ile iletişim kurmak için koordinatları.
3. Çağrı `AcquireTokenAsync(...)`, hangi kabul `IPlatformParameters` nesne ve uygulama için bir belirteç döndürmek gerekli olan kimlik doğrulama akışı çağırır.

    ```C#
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

    `AcquireTokenAsync(...)`ilk (Bu durumda grafik API'si) istenen kaynak için bir belirteç (önbelleğe alma veya eski belirteçleri yenileme aracılığıyla) kimlik bilgilerini girmesini sormadan döndürmeyi dener. Gerekirse, bu kullanıcılar Azure AD oturum açma sayfası istenen belirtecini alma önce gösterir.
4. Grafik API'si istekte erişim belirteci ekleme **yetkilendirme** üstbilgisi:

    ```C#
    ...
        request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", authResult.AccessToken);
    ...
    ```

Tüm olan için `DirectorySearcher` PCL ve uygulama kimlikle ilgili kod. Kalan tek şey çağırmak için `SearchByAlias(...)` yöntemi her platformun görünümlerinde ve gerektiğinde doğru UI yaşam döngüsü işlemek için kod eklemek.

### <a name="android"></a>Android
1. MainActivity.cs içinde bir çağrı ekleyin `SearchByAlias(...)` işleyici düğmesini tıklatın:

    ```C#
    List<User> results = await DirectorySearcher.SearchByAlias(searchTermText.Text, new PlatformParameters(this));
    ```
2. Geçersiz kılma `OnActivityResult` yaşam döngüsü yöntemi herhangi bir kimlik doğrulaması iletmek için uygun yöntemine yönlendirir. ADAL yardımcı yöntem bu Android sağlar:

    ```C#
    ...
    protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
    {
        base.OnActivityResult(requestCode, resultCode, data);
        AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
    }
    ...
    ```

### <a name="windows-desktop"></a>Windows Masaüstü
MainWindow.xaml.cs içinde çağırmaya `SearchByAlias(...)` geçirerek bir `WindowInteropHelper` masaüstünün içinde `PlatformParameters` nesnesi:

```C#
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

#### <a name="ios"></a>iOS
DirSearchClient_iOSViewController.cs, iOS içinde `PlatformParameters` nesne görünüm denetleyicisini başvuru alır:

```C#
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

### <a name="windows-universal"></a>Windows Evrensel
Windows Evrensel MainPage.xaml.cs açın ve ardından uygulama `Search` yöntemi. Bu yöntem, kullanıcı Arabirimi gerektiği şekilde güncelleştirmek için paylaşılan bir proje yardımcı bir yöntem kullanır.

```C#
...
List<User> results = await DirectorySearcherLib.DirectorySearcher.SearchByAlias(SearchTermText.Text, new PlatformParameters(PromptBehavior.Auto, false));
...
```

## <a name="whats-next"></a>Sırada ne var?
Artık çalışan bir, kullanıcıların kimliğini doğrulamak ve güvenli bir şekilde beş farklı platformlarda OAuth 2.0 kullanarak web API çağırma Xamarin uygulaması sahipsiniz.

Kiracı kullanıcılarla zaten doldurulmuş yapmadıysanız, bunu yapmak için gereken süre sunulmuştur.

1. DirectorySearcher uygulamanızı çalıştırın ve ardından kullanıcılardan birine oturum açın.
2. Diğer kullanıcıların UPN Değerlerinin üzerinde göre arayın.

ADAL uygulamaya genel kimlik özellikleri eklemenizi kolaylaştırır. Bu tüm dirty iş sizin için önbellek yönetimi gibi OAuth protokol desteği, kullanıcı bir oturum açma kullanıcı Arabirimi, sunan mvc'deki ve yenileme belirteçleri süresi. Tek bir API çağrısı yalnızca, bilmeniz gereken `authContext.AcquireToken*(…)`.

Başvuru için karşıdan [tamamlanan örnek](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip) (yapılandırma değerleriniz olmadan).

Artık ek kimlik senaryolara geçebilirsiniz. Örneğin, [Azure AD ile .NET Web API'si güvenli](active-directory-devquickstarts-webapi-dotnet.md).

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
