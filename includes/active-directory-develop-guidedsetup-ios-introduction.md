
# <a name="call-the-microsoft-graph-api-from-an-ios-application"></a>Bir iOS uygulamasından Microsoft Graph API çağırma

Bu kılavuz, bir yerel iOS uygulaması (Swift) Microsoft Azure Active Directory (Azure AD) v2.0 uç noktasından erişim belirteçleri gerektiren API'ları nasıl çağırabilirsiniz gösterir. Kılavuzu, erişim belirteçleri almak ve bunları Microsoft Graph API ve diğer API çağrıları kullanma açıklanmaktadır.

Bu kılavuz sonraki alıştırmalarda tamamladıktan sonra uygulamanız herhangi bir şirket veya Azure AD sahip kuruluş korumalı bir API çağırabilir. Uygulamanızı korumalı API çağrıları, outlook.com, live.com ve diğerleri gibi kişisel hesapları gibi iş veya Okul hesaplarını kullanarak yapabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar
- XCode sürüm bu kılavuzda oluşturulan örnek 8.x gereklidir. XCode gelen indirebilirsiniz [iTunes Web sitesi](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12 "XCode yükleme URL'si").
- [Carthage](https://github.com/Carthage/Carthage) bağımlılık Yöneticisi paket yönetimi için gereklidir.

## <a name="how-this-guide-works"></a>Bu kılavuz nasıl çalışır?

![Bu kılavuz nasıl çalışır?](media/active-directory-develop-guidedsetup-ios-introduction/iosintro.png)

Bu kılavuzda, örnek uygulamayı Microsoft Graph API veya Azure AD v2.0 uç noktasından belirteçleri kabul eder API web sorgulamak bir iOS uygulaması sağlar. Bu senaryo için bir belirteç HTTP isteklerini kullanarak eklenir **yetkilendirme** üstbilgi. Belirteç edinme ve yenileme Microsoft kimlik doğrulama kitaplığı (MSAL) tarafından işlenir.


### <a name="handle-token-acquisition-for-access-to-protected-web-apis"></a>Erişim korumalı web API'leri için tanıtıcı belirteç edinme

Kullanıcı kimlik doğrulaması yaptıktan sonra örnek uygulamayı bir belirteç isteyip alıyor. Belirteç Microsoft Graph API veya Azure AD v2.0 uç noktası tarafından güvenli API web sorgulamak için kullanılır.

Microsoft Graph gibi API'leri belirli kaynaklara erişmesine izin vermek için bir erişim belirteci gerektirir. Belirteçleri, bir kullanıcının profilini okuma, bir kullanıcının Takvim erişim, bir e-posta göndermek ve benzeri için gereklidir. Uygulamanızı bir erişim belirteci MSAL kullanarak ve API kapsamları belirterek isteyebilir. Erişim belirteci HTTP eklenir **yetkilendirme** karşı korunan bir kaynağa yapılan her çağrı için üstbilgi.

Önbelleğe alma ve böylece uygulamanız gerek olmayan erişim belirteçleri, yenileme MSAL yönetir.


## <a name="libraries"></a>Kitaplıkları

Bu kılavuz aşağıdaki kitaplığı kullanır:

|Kitaplık|Açıklama|
|---|---|
|[MSAL.framework](https://github.com/AzureAD/microsoft-authentication-library-for-objc)|İOS için Microsoft kimlik doğrulama kitaplığı Önizleme|

