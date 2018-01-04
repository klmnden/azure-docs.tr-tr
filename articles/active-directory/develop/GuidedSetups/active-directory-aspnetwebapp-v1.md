---
title: "Azure AD v1 ASP.NET Web sunucusu Başlarken | Microsoft Docs"
description: "Microsoft oturum açma Openıd Connect standardını kullanan geleneksel web tarayıcı tabanlı bir uygulama ile ASP.NET çözümünü uygulama"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/08/2017
ms.author: andret
ms.openlocfilehash: b23afd26f7ac1828381a0410d2455206c8f43c88
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2018
---
<!--start-intro-->
# <a name="add-sign-in-with-microsoft-to-an-aspnet-web-app"></a>Oturum açma Microsoft ile bir ASP.NET web uygulamasına ekleme

Bu kılavuz, Openıd Connect kullanarak geleneksel web tarayıcı tabanlı bir uygulama ile ASP.NET MVC çözümünü kullanarak Microsoft ile oturum açma uygulamak gösterilmiştir. 

Bu kılavuzun sonunda uygulamanız kabul edecekse, Azure Active Directory ile tümleşik kuruluşların iş ve Okul hesapları, gerçekleştirilen oturum açma işlemleri.

> [!NOTE]
> Bu Destekli Kurulum oturum açma işlemleri ASP.NET uygulamanızın iş ve Okul hesapları arasından etkinleştirmek için yardımcı olur. İş ve Okul hesapları yanı sıra kişisel hesapları için oturum açma işlemlerini etkinleştirmek ilgileniyorsanız kullanabileceğiniz [v2 endpoint](../active-directory-v2-compare.md). Bkz: [bu ASP.NET destekli v2 uç noktası için Kurulum](./active-directory-aspnetwebapp.md) yanı [bu belgeyi](../active-directory-v2-limitations.md) v2 uç noktasının geçerli sınırlamalar açıklayan.
<br/><br/>

<!--separator-->

> Bu kılavuz, Visual Studio 2015 güncelleştirme 3'ü veya Visual Studio 2017 gerektirir.  Sahip değilseniz?  [Visual Studio 2017 ücretsiz indirme](https://www.visualstudio.com/downloads/)

## <a name="how-this-guide-works"></a>Bu kılavuz nasıl çalışır?

![Bu kılavuz nasıl çalışır?](media/active-directory-aspnetwebapp-v1/aspnet-intro.png)

Bu kılavuz, bir oturum açma düğmesi kimlik doğrulaması için bir kullanıcı isteyen burada bir tarayıcı bir ASP.NET web sitesine erişen senaryosunda temel alır. Bu senaryoda, işlerin çoğunu web sayfasını işlemek için sunucu tarafında gerçekleşir.

> [!NOTE]
> Bu Destekli Kurulum oturum açma boş bir şablondan başlangıç ASP.NET Web uygulaması kullanıcılar ve ayrıca bazı kavramları açıklayan sırasında bir oturum düğmesi ve her denetleyici ve yöntemleri ekleme gibi adımlar içerir gösterir. Alternatif olarak, ayrıca Azure Active Directory oturum açmak için bir proje oluşturabilirsiniz kullanarak kullanıcılar (iş ve Okul hesapları) [Visual Studio web şablonu](https://docs.microsoft.com/aspnet/visual-studio/overview/2013/creating-web-projects-in-visual-studio#organizational-account-authentication-options) ve seçerek *Kurumsal hesaplar* ve ardından bir ek denetleyicileri, yöntemleri ve görünümler ile bulut seçenekleri - bu seçenek daha zengin bir şablonu kullanır.

## <a name="libraries"></a>Kitaplıkları

Bu kılavuz, aşağıdaki paketleri kullanır:

|Kitaplık|Açıklama|
|---|---|
|[Microsoft.Owin.Security.OpenIdConnect](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/)|Openıdconnect kimlik doğrulaması için kullanılacak bir uygulama sağlar Ara|
|[Microsoft.Owin.Security.Cookies](https://www.nuget.org/packages/Microsoft.Owin.Security.Cookies)|Tanımlama bilgilerini kullanarak kullanıcı oturumu korumak bir uygulama sağlar Ara|
|[Microsoft.Owin.Host.SystemWeb](https://www.nuget.org/packages/Microsoft.Owin.Host.SystemWeb)|IIS üzerinde ASP.NET istek ardışık düzenini kullanarak çalıştırmak OWIN tabanlı uygulamalar sağlar|


<!--end-intro-->

<!--start-setup-->

## <a name="set-up-your-project"></a>Projenizin kurulumunu

Bu bölümde yüklemek ve Openıd Connect kullanarak bir ASP.NET projede OWIN ara yazılımı üzerinden kimlik doğrulaması ardışık düzenini yapılandırmak için gereken adımları gösterir. 

> Bunun yerine bu örneği ait Visual Studio projesi indirmeyi tercih ediyorsunuz? [Bir proje indirme](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/GuidedSetup.zip) ve geçin [yapılandırma adımı](#configure-your-webconfig-and-register-an-application) kod örneği çalıştırmadan önce yapılandırmak için.

## <a name="create-your-aspnet-project"></a>ASP.NET projesi oluşturma
1. Visual Studio'da:`File` > `New` > `Project`<br/>
2. Altında *Visual C# \Web*seçin `ASP.NET Web Application (.NET Framework)`.
3. Uygulamanızı adlandırın ve tıklayın *Tamam*
4. Seçin `Empty` ve eklemek için onay kutusunu seçip `MVC` başvuruları

## <a name="add-authentication-components"></a>Kimlik doğrulaması Bileşenleri Ekle

1. Visual Studio'da:`Tools` > `Nuget Package Manager` > `Package Manager Console`
2. Ekleme *OWIN ara yazılımı NuGet paketlerini* Paket Yöneticisi konsolu penceresinde aşağıdakileri yazarak:

    ```powershell
    Install-Package Microsoft.Owin.Security.OpenIdConnect
    Install-Package Microsoft.Owin.Security.Cookies
    Install-Package Microsoft.Owin.Host.SystemWeb
    ```

<!--start-collapse-->
> ### <a name="about-these-packages"></a>Bu paketleri hakkında
>Çoklu oturum tanımlama bilgisi tabanlı kimlik doğrulaması Openıd Connect kullanarak açma (SSO) yukarıdaki kitaplıkları etkinleştirin. Kimlik doğrulama tamamlandıktan sonra kullanıcıyı temsil eden simge uygulamanıza gönderilir, OWIN ara yazılımı bir oturum tanımlama bilgisi oluşturur. Tarayıcı daha sonra bu tanımlama bilgisi sonraki isteklerde kullanıcının yeniden kimlik doğrulamaya gerekmez ve hiçbir ek doğrulama gerektiği şekilde kullanır.
<!--end-collapse-->

## <a name="configure-the-authentication-pipeline"></a>Kimlik doğrulama ardışık düzenini yapılandırın
Bir OWIN ara yazılımını oluşturmak için aşağıdaki adımları kullanılan *başlangıç sınıfı* Openıd Connect kimlik doğrulamasını yapılandırmak için. Bu sınıf otomatik olarak yürütülür.

> [!TIP]
> Projenizi yoksa, bir `Startup.cs` dosyası kök klasöründe:<br/>
> 1. Projenin kök klasörü sağ tıklatın: >`Add` > `New Item...` > `OWIN Startup class`<br/>
> 2. Adlandırın`Startup.cs`<br/>
>
>> OWIN başlangıç sınıfı ve değil bir standart C# sınıf seçilen sınıf olduğundan emin olun. Bu görürseniz denetleyerek doğrulayabilirsiniz `[assembly: OwinStartup(typeof({NameSpace}.Startup))]` ad alanı üzerinde.


1. Ekleme *OWIN* ve *Microsoft.IdentityModel* ad alanlarına `Startup.cs`:

    [!code-csharp[main](../../../../WebApp-OpenIDConnect-DotNet/WebApp-OpenIDConnect-DotNet\Startup.cs?name=AddedNameSpaces "Startup.cs")]

2. Başlangıç sınıfı aşağıdaki kodla değiştirin:

    [!code-csharp[main](../../../../WebApp-OpenIDConnect-DotNet/WebApp-OpenIDConnect-DotNet\Startup.cs?name=Startup "Startup.cs")]
    
<!--start-collapse-->
> [!NOTE]
> Sağladığınız içinde parametreleri *OpenIDConnectAuthenticationOptions* Azure AD ile iletişim kurmak uygulamanın koordinatları olarak hizmet. Openıd Connect ara yazılım tanımlama bilgilerini kullandığından, ayrıca önceki kod gösterildiği gibi tanımlama bilgisi kimlik doğrulamasını ayarlamanız gerekir. *Validateıssuer* değer belirli bir kuruluş için erişimi kısıtlamak değil Openıdconnect söyler.
<!--end-collapse-->

<!--end-setup-->

<!--start-use-->

## <a name="add-a-controller-to-handle-sign-in-and-sign-out-requests"></a>Oturum açma ve oturum kapatma isteklerini işlemek için bir denetleyici ekleyin

Bu adım, oturum açma ve oturum kapatma yöntemlerini kullanıma sunmak için yeni bir denetleyici oluşturulacağını gösterir.

1.  Sağ `Controllers` klasörü ve seçin`Add` > `Controller`
2.  `MVC (.NET version) Controller – Empty` öğesini seçin.
3.  Tıklatın *Ekle*
4.  Bu ad `HomeController` tıklatıp *Ekle*
5.  Ekleme *OWIN* sınıfı için ad alanları:

    [!code-csharp[main](../../../../WebApp-OpenIDConnect-DotNet/WebApp-OpenIDConnect-DotNet\Controllers\HomeController.cs?name=AddedNameSpaces "HomeController.cs")]

6. Oturum açma ve oturum kapatma denetleyiciniz için bir kimlik doğrulaması sınaması kodu aracılığıyla başlatarak işlemek için aşağıdaki yöntemleri ekleyin:

    [!code-csharp[main](../../../../WebApp-OpenIDConnect-DotNet/WebApp-OpenIDConnect-DotNet\Controllers\HomeController.cs?name=SigInAndSignOut "HomeController.cs")]
    
## <a name="create-the-apps-home-page-to-sign-in-users-via-a-sign-in-button"></a>Kullanıcılar bir oturum açma düğmesi aracılığıyla imzalamak için uygulamanın giriş sayfası oluşturun

Visual Studio'da Oturum Aç düğmesini ekleyin ve kimlik doğrulamasından sonra kullanıcı bilgilerini görüntülemek için yeni bir görünüm oluşturun:

1.  Sağ `Views\Home` klasörü ve seçin`Add View`
2.  Bunu, `Index` olarak adlandırın.
3.  Oturum açma düğmesi içerir, aşağıdaki HTML dosyaya ekleyin:

    [!code-html[main](../../../../WebApp-OpenIDConnect-DotNet/WebApp-OpenIDConnect-DotNet/Views/Home/Index.cshtml "Index.cshtml")]

<!--start-collapse-->
> [!NOTE]
> Bu sayfa bir oturum açma düğmesi siyah bir arka plan ile SVG biçiminde ekler:<br/>![Microsoft ile oturum aç](media/active-directory-aspnetwebapp-v1/aspnetsigninbuttonsample.png)<br/> Daha fazla düğme'nın oturum açma Git [bu sayfayı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-branding-guidelines "yönergeleri marka").
<!--end-collapse-->

## <a name="display-users-claims-by-adding-a-controller"></a>Bir denetleyici ekleyerek kullanıcının talepleri görüntüleme
Bu denetleyici kullanımını gösteren `[Authorize]` bir denetleyici korumak için öznitelik. Bu öznitelik, yalnızca kimliği doğrulanan kullanıcılar vererek, denetleyiciye erişimi sınırlandırır. Aşağıdaki kod yapar özniteliği alındı kullanıcı talepleri, oturum açma parçası olarak görüntülemek için kullanın.

1.  Sağ `Controllers` klasörü:`Add` > `Controller`
2.  `MVC {version} Controller – Empty` öğesini seçin.
3.  Tıklatın *Ekle*
4.  Adlandırın`ClaimsController`
5.  Bu ekler, denetleyici sınıfının kodu aşağıdaki kodla - değiştirin `[Authorize]` öznitelik sınıfı:

    [!code-csharp[main](../../../../WebApp-OpenIDConnect-DotNet/WebApp-OpenIDConnect-DotNet\Controllers\ClaimsController.cs?name=ClaimsController "ClaimsController.cs")]

<!--start-collapse-->
> [!NOTE]
> Kullanımı nedeniyle `[Authorize]` özniteliği, bu denetleyicinin tüm yöntemleri, yalnızca kullanıcının kimliği doğrulanırsa çalıştırılabilir. Kullanıcı kimliği doğrulanmamış ve denetleyici erişmeye çalışırsa, OWIN kimlik doğrulaması sınaması başlatır ve kullanıcının kimlik doğrulamasını zorlar. Yukarıdaki kod kullanıcı kullanıcının belirtecine eklenen özel öznitelikler için talep koleksiyonunu bakar. Bu öznitelikler, kullanıcının tam adını ve kullanıcı adı yanı sıra, genel kullanıcı tanımlayıcısı konu içerir. Ayrıca içerdiği *Kiracı kimliği*, kullanıcının kuruluş Kimliğini temsil eder. 
<!--end-collapse-->

## <a name="create-a-view-to-display-the-users-claims"></a>Kullanıcının talepleri görüntülemek için bir görünüm oluşturma

Visual Studio'da bir web sayfasında kullanıcının talepleri görüntülemek için yeni bir görünüm oluşturun:

1.  Sağ `Views\Claims` klasörü ve:`Add View`
2.  Bunu, `Index` olarak adlandırın.
3.  Aşağıdaki HTML dosyaya ekleyin:

    [!code-html[main](../../../../WebApp-OpenIDConnect-DotNet/WebApp-OpenIDConnect-DotNet/Views/Claims/Index.cshtml "Index.cshtml")]
    
<!--end-use-->

<!--start-configure-->
## <a name="configure-your-webconfig-and-register-an-application"></a>Yapılandırma, *web.config* ve bir uygulamayı kaydetme

1. Visual Studio'da aşağıdakileri ekleyin `web.config` (kök klasöründe bulunur) bölümünde `configuration\appSettings`:

    ```xml
    <add key="ClientId" value="Enter_the_Application_Id_here" />
    <add key="RedirectUrl" value="Enter_the_Redirect_Url_here" />
    <add key="Tenant" value="common" />
    <add key="Authority" value="https://login.microsoftonline.com/{0}" /> 
    ```
2. Çözüm Gezgini'nde, proje seçip bakmak <i>özellikleri</i> (bir özellik penceresinde F4 tuşuna basın görmüyorsanız) penceresi
3. Değişiklik SSL etkin<code>True</code>
4. Projenin SSL URL'yi panoya kopyalayın:<br/><br/>![Proje Özellikleri](media/active-directory-aspnetwebapp-v1/visual-studio-project-properties.png)<br />
5. İçinde <code>web.config</code>, yerine <code>Enter_the_Redirect_URL_here</code> projenizin SSL URL'si 

### <a name="register-your-application-in-the-azure-portal-then-add-its-information-to-webconfig"></a>Azure Portalı'nda uygulamanızı kaydetme ve kendi bilgi eklemek *web.config*

1. Git [Microsoft Azure Portalı - Uygulama kayıtlar](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) bir uygulamayı kaydetmek için
2. `New application registration` seçeneğini belirleyin
3. Uygulamanız için bir ad girin
4. Visual Studio Proje Yapıştır *SSL URL* içinde `Sign-on URL` (Bu URL de otomatik olarak eklenir yanıt URL'leri listesine kaydetmekte olduğunuz uygulama için)
5. Tıklatın `Create` uygulama kaydetmek için. Bu eylem, geri uygulamaların listesini alır
6. Şimdi, arama ve/veya özelliklerini açmak için oluşturduğunuz uygulamayı seçin
7. GUID altında kopyalayın `Application ID` panoya
8. Visual Studio ve içinde dönmek `web.config`, yerine `Enter_the_Application_Id_here` yalnızca kayıtlı uygulamasından uygulama kimliği

> [!TIP]
> Hesabınız yapılandırılmış birden fazla dizine erişmek için kuruluşunuz için doğru dizin seçili olduğundan emin olun, hesap adınızı üst tıklayarak kaydedilmesi için uygulama istiyorsanız sağ Azure Portalı'nda ve ardından doğrulanıyor Seçili dizin belirtildiği gibi:<br/>![Sağ dizini seçme](media/active-directory-aspnetwebapp-v1/tenantselector.png)

## <a name="configure-sign-in-options"></a>Oturum açma seçeneklerini yapılandırma

Oturum açmak için bir kuruluşun Azure Active Directory örneğine ait olan kullanıcıların izin vermek için uygulamanızın yapılandırabilirsiniz, veya herhangi bir kuruluşa ait kullanıcıların oturum açma işlemlerini kabul edin. Lütfen aşağıdaki seçeneklerden birini yönergeleri izleyin:

### <a name="configure-your-application-to-allow-sign-ins-of-work-and-school-accounts-from-any-company-or-organization-multi-tenant"></a>İzin vermek için uygulamanızın yapılandırma oturum açmalar iş ve Okul hesaplarını herhangi bir şirket veya kuruluş (çok kiracılı)

Oturum açmalar herhangi bir şirket veya Azure Active Directory ile tümleşik olan kuruluşunuzun iş ve Okul hesaplarını kabul etmek istiyorsanız aşağıdaki adımları izleyin. Yaygın bir senaryo için budur *SaaS uygulamaları*:

1. Geri dönerek [Microsoft Azure Portalı - Uygulama kayıtlar](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) ve yalnızca kaydedilmiş uygulamanın bulun
2. Altında `All Settings` seçin`Properties`
3. Değişiklik `Multi-tenanted` özelliğine `Yes` ve'ı tıklatın`Save`

Bu ayar ve çok kiracılı uygulamalara kavramı hakkında daha fazla bilgi için bkz: [bu makalede](../active-directory-devhowto-multi-tenant-overview.md "çok kiracılı genel bakış").

### <a name="restrict-users-from-only-one-organizations-active-directory-instance-to-sign-in-to-your-application-single-tenant"></a>Uygulamanıza (tek-Kiracı) oturum açmak için yalnızca bir kuruluşun Active Directory örneğinden kullanıcılarını kısıtlayın

Bu seçenek için ortak bir senaryodur *LOB uygulamaları*: oturum açma işlemleri yalnızca belirli bir Azure Active Directory örneğine ait hesaplarından kabul etmek için uygulamanızın isteyip istemediğinizi (de dahil olmak üzere *konuk hesaplarını*bu örneğinin), yerini `Tenant` parametresinde *web.config* gelen `Common` kuruluşun – Kiracı adı ile örnek, *contoso.onmicrosoft.com*. Bundan sonra değiştirmek `ValidateIssuer` bağımsız değişkeni, [ *OWIN başlangıç sınıfı* ](#configure-the-authentication-pipeline) için `true`.

Yalnızca belirli kuruluşların listesi kullanıcılardan izin verecek şekilde ayarlamanız `ValidateIssuer` true ve kullanmak için `ValidIssuers` kuruluşların listesini belirtmek için parametre.

Başka bir seçeneği kullanarak verenler doğrulamak için özel bir yöntem uygulamaktır *IssuerValidator* parametresi. Hakkında daha fazla bilgi için `TokenValidationParameters`, lütfen bkz [bu MSDN makalesine](https://msdn.microsoft.com/library/system.identitymodel.tokens.tokenvalidationparameters.aspx "tokenvalidationparameters değerini MSDN makalesine").

<!--end-configure-->

<!--start-configure-arp-->
<!--
## Configure your ASP.NET Web App with the application's registration information

In this step, you will configure your project to use SSL, and then use the SSL URL to configure your application’s registration information. After this, add the application’ registration information to your solution via *web.config*.

1.  In Solution Explorer, select the project and look at the `Properties` window (if you don’t see a Properties window, press F4)
2.  Change `SSL Enabled` to `True`
3.  Copy the value from `SSL URL` above and paste it in the `Redirect URL` field on the top of this page, then click *Update*:<br/><br/>![Project properties](media/active-directory-aspnetwebapp-v1/vsprojectproperties.png)<br />
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
## <a name="test-your-code"></a>Kodunuzu test

Tuşuna `F5` Visual Studio'da projeyi çalıştırın. Tarayıcı açılır ve size yönlendiren *http://localhost: {port}* gördüğünüz *oturum oturum Microsoft* düğmesi. Bir tane oturum açmak için tıklatın.

Ne zaman oturum açmak için bir iş hesabı (Azure Active Directory) kullanmak, test hazırsınız. 

![Microsoft tarayıcı penceresi ile oturum açın](media/active-directory-aspnetwebapp-v1/aspnetbrowsersignin.png)

![Microsoft tarayıcı penceresi ile oturum açın](media/active-directory-aspnetwebapp-v1/aspnetbrowsersignin2.png)

#### <a name="expected-results"></a>Beklenen sonuçları
Kullanıcı oturum açma işleminden sonra web sitenizin HTTPS uygulamanızın kayıt bilgileri Microsoft uygulama kayıt Portalı'nda belirtilen URL olan, giriş sayfasına yönlendirilir. Bu sayfayı artık gösterir *Hello {User}* ve oturumu için bir bağlantı ve bir bağlantıdır Authorize denetleyiciye kullanıcının taleplerini görmek için bir bağlantı oluşturduğunuz.

### <a name="see-users-claims"></a>Kullanıcının talepleri bakın
Kullanıcının talepleri görmek için köprü seçin. Bu eylem yalnızca kimliği doğrulanan kullanıcılar tarafından kullanılabilir görünüm ve denetleyici için yol gösterir.

#### <a name="expected-results"></a>Beklenen sonuçları
 Oturum açmış olan kullanıcının temel özelliklerini içeren bir tablo görmeniz gerekir:

| Özellik | Değer | Açıklama|
|---|---|---|
| Ad | {Tam} kullanıcı adı | Kullanıcı adı ve Soyadı
|Kullanıcı adı | <span>user@domain.com</span>| Oturum açmış kullanıcıyı tanımlamak için kullanılan kullanıcı adı
| Konu| {Konu}|Kullanıcı oturum açma web üzerinden benzersiz şekilde tanımlamak için bir dize|
| Kiracı Kimliği| {GUID}| A *GUID* kullanıcının Azure Active Directory kuruluş benzersiz olarak gösterecek.|

Ayrıca, kimlik doğrulama isteğine dahil tüm kullanıcı talepleri de dahil olmak üzere bir tabloya bakın. Bu, tüm talepler kimliği belirteci ve açıklamaları listesi için bkz [makale](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims "kimliği belirteçte talep listesi").


### <a name="test-accessing-a-method-that-has-an-authorize-attribute-optional"></a>Test içeren bir yöntem erişen bir *[Authorize]* özniteliği (isteğe bağlı)
Bu adımda, bir anonim kullanıcı olarak talep denetleyicisi erişimi test:<br/>
Kullanıcı oturum kapatma bağlantısını seçin ve oturum kapatma işlemini tamamlayın.<br/>
Şimdi, tarayıcıda http://localhost yazın: {port} / ile korunan denetleyicinizi erişmek talepleri `[Authorize]` özniteliği

#### <a name="expected-results"></a>Beklenen sonuçları
Görmek için kimlik doğrulaması gerektirmek istemi almanız gerekir.

## <a name="additional-information"></a>Ek bilgiler

<!--start-collapse-->
### <a name="protect-your-entire-web-site"></a>Tüm web sitenize koruma
Tüm web sitenize korumak için add `AuthorizeAttribute` için `GlobalFilters` içinde `Global.asax` `Application_Start` yöntemi:

```csharp
GlobalFilters.Filters.Add(new AuthorizeAttribute());
```
<!--end-collapse-->

<div></div>
<br/>

<!--end-test-->