# <a name="call-the-microsoft-graph-api-from-a-windows-desktop-app"></a>Windows Masaüstü uygulamasından Microsoft Graph API çağırma

Bu kılavuz, nasıl bir yerel Windows Masaüstü .NET (XAML) uygulama erişim belirteci almak ve Microsoft Graph API'si veya bir Azure Active Directory v2 uç noktasından erişim belirteçleri gerektiren diğer API'leri çağırmak gösterir.

Kılavuzu tamamladıktan sonra uygulamanız (outlook.com, live.com ve diğerleri dahil) kişisel hesapları kullanan korumalı bir API çağrısı kuramaz. Uygulama, ayrıca iş ve Okul hesapları herhangi bir şirket veya Azure Active Directory kullanan kuruluş.  

> [!NOTE] 
> Visual Studio 2015 güncelleştirme 3'ü veya Visual Studio 2017 Kılavuzu gerektirir.  Bu sürümlerden birini yok mu? [Visual Studio 2017 için ücretsiz olarak karşıdan](https://www.visualstudio.com/downloads/).

## <a name="how-this-guide-works"></a>Bu kılavuz nasıl çalışır?

![Bu kılavuz nasıl çalışır?](./media/active-directory-develop-guidedsetup-windesktop-intro/windesktophowitworks.png)

Bu kılavuzu ile oluşturduğunuz örnek uygulamayı Microsoft Graph API'si veya bir Azure Active Directory v2 uç noktasından belirteçleri kabul eder bir Web API sorgular bir Windows masaüstü uygulaması sağlar. Bu senaryo için HTTP isteklerine Authorization Üstbilgisi aracılığıyla bir belirteç ekler. Microsoft kimlik doğrulama kitaplığı (MSAL) belirteç edinme ve yenileme işler.

## <a name="handling-token-acquisition-for-accessing-protected-web-apis"></a>Korumalı Web API'lerine erişmek için belirteç edinme işleme

Kullanıcının kimliği doğrulandıktan sonra örnek uygulamayı Microsoft Graph API veya Azure Active Directory v2 ile güvenli bir Web API sorgulamak için kullanılan bir belirteç isteyip alıyor.

API Microsoft Graph gibi belirli kaynaklara erişmesine izin vermek için bir belirteç gerektirir. Örneğin, bir belirteç kullanıcı profilini okuma, bir kullanıcının Takvim erişmek veya e-posta göndermek için gereklidir. Uygulamanız, API kapsamları belirterek bu kaynaklara erişmek için MSAL kullanarak bir erişim belirteci isteyebilirsiniz. Bu erişim belirteci, ardından HTTP Authorization Üstbilgisi karşı korunan bir kaynağa yapılan her çağrı eklenir. 

MSAL önbelleğe alma ve erişim belirteçleri, yenileme yönetir uygulamanız gerekmez.

## <a name="nuget-packages"></a>NuGet paketleri

Bu kılavuz aşağıdaki NuGet paketlerini kullanır:

|Kitaplık|Açıklama|
|---|---|
|[Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)|Microsoft kimlik doğrulama kitaplığı (MSAL)|

