# <a name="add-sign-in-with-microsoft-to-an-aspnet-web-app"></a>Oturum açma Microsoft ile bir ASP.NET web uygulamasına ekleme

Bu kılavuz, Openıd Connect kullanarak geleneksel web tarayıcı tabanlı bir uygulama ile ASP.NET MVC çözümünü kullanarak Microsoft ile oturum açma uygulamak gösterilmiştir. 

Bu kılavuzun sonunda uygulamanız kişisel bileşenleri (outlook.com, live.com ve diğerleri dahil) hesaplarının yanı sıra iş oturum kabul edin ve Okul hesapları herhangi bir şirket veya Azure Active Directory ile tümleşik olan Kuruluş mümkün olacaktır. 

> Bu kılavuz, Visual Studio 2015 güncelleştirme 3'ü veya Visual Studio 2017 gerektirir.  Sahip değilseniz?  [Visual Studio 2017 ücretsiz indirme](https://www.visualstudio.com/downloads/)

## <a name="how-this-guide-works"></a>Bu kılavuz nasıl çalışır?

![Bu kılavuz nasıl çalışır?](media/active-directory-develop-guidedsetup-aspnetwebapp-intro/aspnetbrowsergeneral.png)

Bu kılavuz, bir oturum açma düğmesi kimlik doğrulaması için bir kullanıcı isteyen burada bir tarayıcı bir ASP.NET web sitesine erişen senaryosunda temel alır. Bu senaryoda, işlerin çoğunu web sayfasını işlemek için sunucu tarafında gerçekleşir.

## <a name="libraries"></a>Kitaplıkları

Bu kılavuz aşağıdaki kitaplıklarını kullanır:

|Kitaplık|Açıklama|
|---|---|
|[Microsoft.Owin.Security.OpenIdConnect](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/)|Openıdconnect kimlik doğrulaması için kullanılacak bir uygulama sağlar Ara|
|[Microsoft.Owin.Security.Cookies](https://www.nuget.org/packages/Microsoft.Owin.Security.Cookies)|Tanımlama bilgilerini kullanarak kullanıcı oturumu korumak bir uygulama sağlar Ara|
|[Microsoft.Owin.Host.SystemWeb](https://www.nuget.org/packages/Microsoft.Owin.Host.SystemWeb)|IIS üzerinde ASP.NET istek ardışık düzenini kullanarak çalıştırmak OWIN tabanlı uygulamalar sağlar|

