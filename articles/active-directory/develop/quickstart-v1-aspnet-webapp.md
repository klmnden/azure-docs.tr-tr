---
title: Azure AD v1 ASP.NET Web sunucusunu kullanmaya başlama | Microsoft Docs
description: Microsoft oturum açma standart Openıd Connect kullanarak geleneksel web tarayıcı tabanlı bir uygulama ile bir ASP.NET çözüm üzerinde uygulama
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/23/2018
ms.author: andret
ms.openlocfilehash: 5353e22d7ae77adecfe126bb589d08c808752550
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39579359"
---
<!--start-intro-->
# <a name="add-sign-in-with-microsoft-to-an-aspnet-web-app"></a>Oturum açma Microsoft ile bir ASP.NET web uygulamasına ekleme

Bu kılavuzda oturum Openıd Connect'i kullanarak geleneksel web tarayıcı tabanlı bir uygulama ile bir ASP.NET MVC çözümünü kullanarak Microsoft ile nasıl uygulanacağını gösterir. 

Bu kılavuzun sonunda, uygulamanızın kabul oturum açmalar Azure Active Directory ile tümleşik kuruluşların iş ve Okul hesapları.

> [!NOTE]
> Bu Kılavuzlu Kurulum ASP.NET uygulamanızı iş ve Okul hesaplarında oturum açma işlemleri etkinleştirmenize yardımcı olur. İş ve Okul hesaplarında yanı sıra kişisel hesapları için oturum açma etkinleştirmeyi düşünüyorsanız, kullanabileceğiniz [v2 uç noktası](azure-ad-endpoint-comparison.md). Bkz: [bu ASP.NET destekli Kurulumu v2 uç noktası için](tutorial-v2-asp-webapp.md) yanı [bu belgeyi](active-directory-v2-limitations.md) v2 uç noktanın geçerli sınırlamalar açıklayan.
<br/><br/>

<!--separator-->

> Bu kılavuz, Visual Studio 2015 güncelleştirme 3 veya Visual Studio 2017'yi gerektirir.  Yok?  [Visual Studio 2017'yi ücretsiz olarak indirin](https://www.visualstudio.com/downloads/)

## <a name="how-this-guide-works"></a>Bu kılavuz nasıl çalışır?

![Bu kılavuz nasıl çalışır?](./media/quickstart-v1-aspnet-webapp/aspnet-intro.png)

Bu kılavuzda isteyen bir kullanıcı oturum açma düğmesi aracılığıyla kimlik doğrulaması için bir tarayıcı bir ASP.NET web sitesine eriştiği senaryoya bağlıdır. Bu senaryoda, web sayfasını işlemek için işin çoğunu sunucu tarafında gerçekleşir.

> [!NOTE]
> Bu Kılavuzlu Kurulum, kullanıcılar boş bir şablondan başlayan bir ASP.NET Web uygulamasında oturum açın ve bir oturum açma düğmesi ve her denetleyici ve yöntemlerinde, ayrıca bazı kavramları açıklarken ekleme gibi adımlar gösterilmektedir. Alternatif olarak, ayrıca Azure Active Directory oturum açmak için bir proje oluşturabilirsiniz kullanarak kullanıcılar (iş ve Okul hesapları) [Visual Studio web şablonu](https://docs.microsoft.com/aspnet/visual-studio/overview/2013/creating-web-projects-in-visual-studio#organizational-account-authentication-options) seçerek *Kurumsal hesaplar* ve ardından bir ek denetleyicileri, metotları ve görünümleri ile bulut seçenekleri - bu seçenek daha zengin bir şablon kullanır.

## <a name="libraries"></a>Kitaplıkları

Bu kılavuz, aşağıdaki paketleri kullanır:

|Kitaplık|Açıklama|
|---|---|
|[Microsoft.Owin.Security.OpenIdConnect](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/)|Openıdconnect kimlik doğrulaması için kullanılacak bir uygulama sağlayan bir ara yazılım|
|[Microsoft.Owin.Security.Cookies](https://www.nuget.org/packages/Microsoft.Owin.Security.Cookies)|Tanımlama bilgilerini kullanarak kullanıcı oturumu korumak bir uygulama sağlayan bir ara yazılım|
|[Microsoft.Owin.Host.SystemWeb](https://www.nuget.org/packages/Microsoft.Owin.Host.SystemWeb)|IIS üzerinde ASP.NET istek ardışık düzenini kullanarak çalıştırmak OWIN tabanlı uygulamalar sağlar.|


<!--end-intro-->

<!--start-setup-->

## <a name="set-up-your-project"></a>Projenizi ayarlama

Bu bölüm, yükleme ve Openıd Connect'i kullanarak bir ASP.NET projesi üzerinde OWIN ara yazılımı üzerinden kimlik doğrulaması işlem hattı yapılandırma adımlarını gösterir. 

> Bunun yerine bu örneği ait Visual Studio projeyi indirmek tercih ediyorsunuz? [Bir projeyi indirirken](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/GuidedSetup.zip) ve atlamak [yapılandırma adımı](#configure-your-webconfig-and-register-an-application) çalıştırmadan önce kodu örneği yapılandırmak için.

## <a name="create-your-aspnet-project"></a>ASP.NET projenizi oluşturun
1. Visual Studio'da: `File` > `New` > `Project`<br/>
2. Altında *Visual C# \Web*seçin `ASP.NET Web Application (.NET Framework)`.
3. Uygulamanızı adlandırın ve tıklayın *Tamam*
4. Seçin `Empty` ve eklemek için onay kutusunu seçip `MVC` başvuruları

## <a name="add-authentication-components"></a>Kimlik doğrulama bileşenleri ekleme

1. Visual Studio'da: `Tools` > `Nuget Package Manager` > `Package Manager Console`
2. Ekleme *OWIN ara yazılımı NuGet paketlerini* Paket Yöneticisi konsolu penceresinde aşağıdakileri yazarak:

    ```powershell
    Install-Package Microsoft.Owin.Security.OpenIdConnect
    Install-Package Microsoft.Owin.Security.Cookies
    Install-Package Microsoft.Owin.Host.SystemWeb
    ```

<!--start-collapse-->
> ### <a name="about-these-packages"></a>Bu paketler hakkında
>Yukarıdaki kitaplıkları, çoklu oturum tanımlama bilgisi tabanlı kimlik doğrulaması Openıd Connect'i kullanarak açma (SSO) etkinleştirin. Kimlik doğrulaması tamamlandıktan sonra kullanıcıyı temsil eden simge uygulamanıza gönderilir, OWIN ara yazılımı oturum tanımlama bilgisi oluşturur. Tarayıcı daha sonra bu tanımlama bilgisi sonraki isteklerde authenticateasync kullanıcının yeniden kimlik doğrulamaya zorlayabilir gerekmez ve hiçbir ek doğrulama gerektiği şekilde kullanır.
<!--end-collapse-->

## <a name="configure-the-authentication-pipeline"></a>Kimlik doğrulaması işlem hattı yapılandırın
Bir OWIN ara yazılımı oluşturmak için aşağıdaki adımları kullanılan *başlangıç sınıfı* Openıd Connect kimlik doğrulamasını yapılandırmak için. Bu sınıf otomatik olarak yürütülür.

> [!TIP]
> Projeniz yoksa, bir `Startup.cs` kök klasöründeki dosya:<br/>
> 1. Projenin kök klasörüne sağ: >    `Add` > `New Item...` > `OWIN Startup class`<br/>
> 2. Adlandırın `Startup.cs`<br/>
>
>> Seçilen sınıf bir OWIN başlangıç sınıfı ve standart C# sınıfı değil olduğundan emin olun. Bunu görürseniz işaretleyerek onaylayın `[assembly: OwinStartup(typeof({NameSpace}.Startup))]` yukarıda ad alanı.


1. Ekleme *OWIN* ve *Microsoft.IdentityModel* ad alanlarına `Startup.cs`:

    [!code-csharp[main](../../../WebApp-OpenIDConnect-DotNet/WebApp-OpenIDConnect-DotNet\Startup.cs?name=AddedNameSpaces "Startup.cs")]

2. Başlangıç sınıfı aşağıdaki kodla değiştirin:

    [!code-csharp[main](../../../WebApp-OpenIDConnect-DotNet/WebApp-OpenIDConnect-DotNet\Startup.cs?name=Startup "Startup.cs")]
    
<!--start-collapse-->
> [!NOTE]
> İçinde sağladığınız parametreler *OpenIDConnectAuthenticationOptions* koordinatları uygulamanın Azure AD ile iletişim kurmak işlevi görür. Openıd Connect ara yazılım tanımlama bilgileri kullandığından, ayrıca önceki kodun gösterdiği gibi tanımlama bilgisi kimlik doğrulamasını ayarlama gerekir. *ValidateIssuer* değeri belirli bir kuruluşa erişimi kısıtlamak değil, Openıdconnect söyler.
<!--end-collapse-->

<!--end-setup-->

<!--start-use-->

## <a name="add-a-controller-to-handle-sign-in-and-sign-out-requests"></a>Oturum açma ve oturum kapatma istekleri işlemek üzere bir denetleyici ekleyin

Bu adım, oturum açma ve oturum kapatma yöntemlerini ortaya çıkarmak için yeni bir denetleyici oluşturma işlemi gösterilmektedir.

1.  Sağ `Controllers` klasörü ve seçin `Add` > `Controller`
2.  `MVC (.NET version) Controller – Empty` öğesini seçin.
3.  Tıklayın *Ekle*
4.  Adlandırın `HomeController` tıklatıp *Ekle*
5.  Ekleme *OWIN* sınıfa ad alanları:

    [!code-csharp[main](../../../WebApp-OpenIDConnect-DotNet/WebApp-OpenIDConnect-DotNet\Controllers\HomeController.cs?name=AddedNameSpaces "HomeController.cs")]

6. Bir kod aracılığıyla kimlik doğrulaması sınaması başlatarak, oturum açma ve oturum kapatma denetleyicisine işlemek için aşağıdaki yöntemleri ekleyin:

    [!code-csharp[main](../../../WebApp-OpenIDConnect-DotNet/WebApp-OpenIDConnect-DotNet\Controllers\HomeController.cs?name=SigInAndSignOut "HomeController.cs")]
    
## <a name="create-the-apps-home-page-to-sign-in-users-via-a-sign-in-button"></a>Kullanıcılar bir oturum açma düğmesi aracılığıyla oturum açmak için uygulama giriş sayfası oluşturma

Visual Studio'da oturum açma düğmesi ekleme ve kimlik doğrulamasından sonra kullanıcı bilgilerini görüntülemek için yeni bir görünüm oluşturun:

1.  Sağ `Views\Home` klasörü ve seçin `Add View`
2.  Bunu, `Index` olarak adlandırın.
3.  Oturum Aç düğmesini içeren HTML dosyasına ekleyin:

    [!code-html[main](../../../WebApp-OpenIDConnect-DotNet/WebApp-OpenIDConnect-DotNet/Views/Home/Index.cshtml "Index.cshtml")]

<!--start-collapse-->
> [!NOTE]
> Bu sayfa, siyah bir arka plan SVG biçiminde bir oturum açma düğmesi ekler:<br/>![Microsoft ile oturum açın](./media/quickstart-v1-aspnet-webapp/aspnetsigninbuttonsample.png)<br/> Daha fazla düğme'nın oturum açma Git [bu sayfayı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-branding-guidelines "marka yönergeleri").
<!--end-collapse-->

## <a name="display-users-claims-by-adding-a-controller"></a>Kullanıcı talepleri bir denetleyici ekleyerek görüntüleme
Bu denetleyici kullanımlarını gösterir `[Authorize]` bir denetleyici korumak için özniteliği. Bu öznitelik, yalnızca kimliği doğrulanmış kullanıcılar vererek denetleyiciye erişimi kısıtlar. Aşağıdaki kod yapar alınan kullanıcı taleplerini oturum açmanın bir parçası olarak görüntülenecek özniteliğini kullanın.

1.  Sağ `Controllers` klasörü: `Add` > `Controller`
2.  `MVC {version} Controller – Empty` öğesini seçin.
3.  Tıklayın *Ekle*
4.  Adlandırın `ClaimsController`
5.  Bu ekler, denetleyici sınıfının kodunu aşağıdaki kodla - değiştirin `[Authorize]` öznitelik sınıfı:

    [!code-csharp[main](../../../WebApp-OpenIDConnect-DotNet/WebApp-OpenIDConnect-DotNet\Controllers\ClaimsController.cs?name=ClaimsController "ClaimsController.cs")]

<!--start-collapse-->
> [!NOTE]
> Kullanımı nedeniyle `[Authorize]` özniteliği, bu denetleyicinin tüm yöntemleri, yalnızca kullanıcı kimlik doğrulaması gerekiyorsa çalıştırılabilir. Kullanıcı kimliği doğrulanmamış, ve denetleyiciye erişmeye OWIN kimlik doğrulaması sınaması başlatır ve kullanıcının kimlik doğrulamasını zorlar. Yukarıdaki kod, kullanıcının belirtecinde bulunan belirli öznitelikler için kullanıcı taleplerini koleksiyonunu bakar. Bu öznitelikler, kullanıcının tam adını ve kullanıcı adı yanı sıra, genel kullanıcı tanımlayıcısı konu içerir. Ayrıca içerdiği *Kiracı kimliği*, kullanıcının kuruluş Kimliğini temsil eder. 
<!--end-collapse-->

## <a name="create-a-view-to-display-the-users-claims"></a>Kullanıcı talepleri görüntülemek için bir görünüm oluşturma

Visual Studio'da bir web sayfasında kullanıcı talepleri görüntülemek için yeni bir görünüm oluşturun:

1.  Sağ `Views\Claims` klasörü ve: `Add View`
2.  Bunu, `Index` olarak adlandırın.
3.  Aşağıdaki HTML'yi dosyaya ekleyin:

    [!code-html[main](../../../WebApp-OpenIDConnect-DotNet/WebApp-OpenIDConnect-DotNet/Views/Claims/Index.cshtml "Index.cshtml")]
    
<!--end-use-->

<!--start-configure-->
## <a name="configure-your-webconfig-and-register-an-application"></a>Yapılandırma, *web.config* ve bir uygulamayı kaydetme

1. Visual Studio'da ekleyin `web.config` (kök klasöründe bulunur) bölümünde `configuration\appSettings`:

    ```xml
    <add key="ClientId" value="Enter_the_Application_Id_here" />
    <add key="RedirectUrl" value="Enter_the_Redirect_Url_here" />
    <add key="Tenant" value="common" />
    <add key="Authority" value="https://login.microsoftonline.com/{0}" /> 
    ```
2. Çözüm Gezgini'nde projeyi seçin ve bakmak <i>özellikleri</i> (bir Özellikler penceresinde, F4 tuşuna görmüyorsanız) penceresi
3. Değişiklik SSL etkin <code>True</code>
4. Projenin SSL URL panoya kopyalayın:<br/><br/>![Proje Özellikleri](./media/quickstart-v1-aspnet-webapp/visual-studio-project-properties.png)<br />
5. İçinde <code>web.config</code>, değiştirin <code>Enter_the_Redirect_URL_here</code> projenizin SSL URL'si 

### <a name="register-your-application-in-the-azure-portal-then-add-its-information-to-webconfig"></a>Azure Portalı'nda kaydedin, ardından kendi ilgili bilgileri *web.config*

1. Git [Microsoft Azure Portalı - Uygulama kayıtları](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) bir uygulamayı kaydetme
2. `New application registration` seçeneğini belirleyin
3. Uygulamanız için bir ad girin
4. Visual Studio projenin yapıştırın *SSL URL* içinde `Sign-on URL` (Bu URL de eklendiğinde otomatik olarak yanıt URL'leri listesi kaydettiriyor olduğunuz uygulama için)
5. Tıklayın `Create` uygulamayı kaydetme. Bu eylem, geri uygulamaların listesini alır
6. Şimdi, arama ve/veya özelliklerini açmak için oluşturduğunuz uygulamayı seçin
7. GUID altında kopyalayın `Application ID` panoya
8. Visual Studio ve içinde dönün `web.config`, değiştirin `Enter_the_Application_Id_here` yeni kaydettiğiniz uygulamadan uygulama kimliği

> [!TIP]
> Üst hesap adınıza tıklayarak kaydedilmesi için uygulama istediğiniz hesabınızı yapılandırılmışsa birden çok dizinlere erişim için kuruluş için doğru dizin seçtiğinizden emin olun Azure portalında sağ ve ardından doğrulanıyor Belirtilen olarak seçili dizin:<br/>![Doğru dizin seçme](./media/quickstart-v1-aspnet-webapp/tenantselector.png)

## <a name="configure-sign-in-options"></a>Oturum açma seçeneklerini yapılandırın

Uygulamanızın oturum açma için bir kuruluşun Azure Active Directory örneğine ait kullanıcılar izin verecek şekilde yapılandırabilirsiniz, veya herhangi bir kuruluşa ait olan kullanıcıların oturum açma işlemleri kabul edin. Lütfen aşağıdaki seçeneklerden birini yönergeleri izleyin:

### <a name="configure-your-application-to-allow-sign-ins-of-work-and-school-accounts-from-any-company-or-organization-multi-tenant"></a>İzin vermek için uygulamanızı yapılandırma oturum açmalar, iş ve Okul hesaplarında herhangi bir şirket veya kuruluş (çok kiracılı)

Oturum açmalar herhangi bir şirket veya Azure Active Directory ile tümleşik olan Kuruluş İş ve Okul hesapları kabul etmek istiyorsanız aşağıdaki adımları izleyin. Bu bir yaygın senaryodur *SaaS uygulamalarına*:

1. Geri Git [Microsoft Azure Portalı - Uygulama kayıtları](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) ve yeni kaydettiğiniz uygulamayı bulun
2. Altında `All Settings` seçin `Properties`
3. Değişiklik `Multi-tenanted` özelliğini `Yes` tıklayın `Save`

Bu ayar ve çok kiracılı uygulamaları kavramı hakkında daha fazla bilgi için bkz. [bu makalede](howto-convert-app-to-be-multi-tenant.md "çok kiracılı genel bakış").

### <a name="restrict-users-from-only-one-organizations-active-directory-instance-to-sign-in-to-your-application-single-tenant"></a>Uygulamanıza (tek kiracılı) oturum açmak için yalnızca bir kuruluşun Active Directory örneğinden kullanıcıları kısıtlama

Bu seçenek için sık karşılaşılan bir senaryodur *LOB uygulamaları*: oturum açma işlemleri yalnızca belirli bir Azure Active Directory örneğine ait hesapları kabul etmek için uygulamanızı isterseniz (dahil olmak üzere *konuk hesaplarını*bu örneğinin), değiştirin `Tenant` parametresinde *web.config* gelen `Common` kuruluşu – Kiracı adı ile örnek, *contoso.onmicrosoft.com*. Bundan sonra değiştirmek `ValidateIssuer` bağımsız değişkeni, [ *OWIN başlangıç sınıfı* ](#configure-the-authentication-pipeline) için `true`.

Yalnızca belirli kuruluşların listesi kullanıcılardan izin verecek şekilde ayarlanmış `ValidateIssuer` true ve kullanmak için `ValidIssuers` kuruluşlar listesini belirtmek için parametre.

Başka bir seçeneği kullanarak verenler doğrulamak için özel bir yönteme uygulamaktır *IssuerValidator* parametresi. Hakkında daha fazla bilgi için `TokenValidationParameters`, lütfen [bu MSDN makalesinde](https://msdn.microsoft.com/library/system.identitymodel.tokens.tokenvalidationparameters.aspx "tokenvalidationparameters değerini MSDN makalesi").

<!--end-configure-->

<!--start-configure-arp-->
<!--
## Configure your ASP.NET Web App with the application's registration information

In this step, you will configure your project to use SSL, and then use the SSL URL to configure your application’s registration information. After this, add the application’ registration information to your solution via *web.config*.

1.  In Solution Explorer, select the project and look at the `Properties` window (if you don’t see a Properties window, press F4)
2.  Change `SSL Enabled` to `True`
3.  Copy the value from `SSL URL` above and paste it in the `Redirect URL` field on the top of this page, then click *Update*:<br/><br/>![Project properties](./media/quickstart-v1-aspnet-webapp/vsprojectproperties.png)<br />
4.  Add the following in `web.config` file located in root’s folder, under section `configuration\appSettings`:

```xml
<add key="ClientId" value="[Enter the application Id here]" />
<add key="RedirectUri" value="[Enter the Redirect URL here]" />
<add key="Tenant" value="common" />
<add key="Authority" value="https://login.microsoftonline.com/{0}" /> 
```
-->
<!--end-configure-arp-->
<!--start-test-->
## <a name="test-your-code"></a>Kodunuzu test etme

Tuşuna `F5` projeyi Visual Studio'da çalıştırın. Tarayıcı açılır ve sizi için yönlendirir *http://localhost:{port}* gördüğünüz *Microsoft'ta oturum açma* düğmesi. Devam edin ve oturum açmak için tıklayın.

Ne zaman test edin ve oturum açmak için bir iş hesabı (Azure Active Directory) kullanmak hazırsınız. 

![Microsoft tarayıcı penceresi ile oturum açın](./media/quickstart-v1-aspnet-webapp/aspnetbrowsersignin.png)

![Microsoft tarayıcı penceresi ile oturum açın](./media/quickstart-v1-aspnet-webapp/aspnetbrowsersignin2.png)

#### <a name="expected-results"></a>Beklenen sonuçları
Kullanıcı oturum açma işleminden sonra uygulamanızın kayıt bilgileri Microsoft uygulama kayıt Portalı'nda belirtilen HTTPS URL'si, web sitenizin, giriş sayfasına yönlendirilir. Bu sayfa artık gösterir *Merhaba {User}* ve oturumu kapatmak için Authorize denetleyicisinin bağlantısını olan kullanıcı taleplerini görmek için bir bağlantı ve daha önce oluşturduğunuz.

### <a name="see-users-claims"></a>Kullanıcı talepleri bakın
Kullanıcı talepleri görmek için köprüyü seçin. Bu eylem, yalnızca kimliği doğrulanan kullanıcılar tarafından kullanılabilir görünüm ve denetleyici için yol gösterir.

#### <a name="expected-results"></a>Beklenen sonuçları
 Oturum açmış kullanıcının temel özelliklerini içeren bir tablo görmeniz gerekir:

| Özellik | Değer | Açıklama|
|---|---|---|
| Ad | {Kullanıcının tam adı} | Kullanıcı adı ve Soyadı
|Kullanıcı adı | <span>user@domain.com</span>| Oturum açmış kullanıcıyı tanımlamak için kullanılan kullanıcı adı
| Konu| {Subject}|Kullanıcı oturum açma web üzerinden benzersiz şekilde tanımlamak için bir dize|
| Kiracı Kimliği| {GUID}| A *GUID* benzersiz kullanıcının Azure Active Directory kuruluşu temsil etmek için.|

Ayrıca, kimlik doğrulama isteğine dahil tüm kullanıcı talepleri içeren bir tablo görürsünüz. Bir kimlik belirteci ve açıklamaları tüm talepleri bir listesi için bkz. Bu [makale](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims "kimliği belirteçteki talepleri listesini").


### <a name="test-accessing-a-method-that-has-an-authorize-attribute-optional"></a>Test erişimi olan bir yönteme bir *[Authorize]* özniteliği (isteğe bağlı)
Bu adımda, talep denetleyicisi anonim kullanıcı olarak erişmeyi test etme:<br/>
Kullanıcı oturum kapatma bağlantısını seçin ve oturum kapatma işlemini tamamlayın.<br/>
Artık tarayıcınızda yazın http://localhost:{port}/claims denetleyiciniz ile korunan erişmeye `[Authorize]` özniteliği

#### <a name="expected-results"></a>Beklenen sonuçları
Ayrıntılarını görmek için kimlik doğrulaması gerektiren istemi almanız gerekir.

## <a name="additional-information"></a>Ek bilgiler

<!--start-collapse-->
### <a name="protect-your-entire-web-site"></a>Tüm web sitenize koruyun
Tüm web sitenize korumak için ekleme `AuthorizeAttribute` için `GlobalFilters` içinde `Global.asax` `Application_Start` yöntemi:

```csharp
GlobalFilters.Filters.Add(new AuthorizeAttribute());
```
<!--end-collapse-->

<div></div>
<br/>

<!--end-test-->