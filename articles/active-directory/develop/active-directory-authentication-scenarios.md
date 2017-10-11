---
title: "Azure AD için kimlik doğrulama senaryoları | Microsoft Docs"
description: "Beş en yaygın kimlik doğrulama senaryoları için Azure Active Directory (AAD) genel bakış"
services: active-directory
documentationcenter: dev-center-name
author: skwan
manager: mbaldwin
editor: 
ms.assetid: 0c84e7d0-16aa-4897-82f2-f53c6c990fd9
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/27/2017
ms.author: skwan
ms.custom: aaddev
ms.openlocfilehash: 2f9410bdaa037f1839cf7c12c3532b51be669ed5
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
---
# <a name="authentication-scenarios-for-azure-ad"></a>Azure AD için Kimlik Doğrulama Senaryoları
Azure Active Directory (Azure AD) geliştiriciler için kimlik doğrulama kitaplıkları kodlama hızla başlamanıza yardımcı olması farklı platformlar için OAuth 2.0 ve Openıd Connect gibi endüstri standardı protokoller desteğiyle hizmet olarak açık kaynak olarak kimlik sağlayarak basitleştirir. Bu belge yardımcı olacak çeşitli senaryolar Azure AD destekler anlamak ve nasıl başlayacağınızı gösterir. Aşağıdaki bölümlere ayrılır:

* [Azure AD kimlik doğrulaması temelleri](#basics-of-authentication-in-azure-ad)
* [Azure AD güvenlik belirteçleri talepleri](#claims-in-azure-ad-security-tokens)
* [Azure AD'de uygulama kaydetme temelleri](#basics-of-registering-an-application-in-azure-ad)
* [Uygulama türleri ve senaryolar](#application-types-and-scenarios)
  
  * [Web uygulaması için Web tarayıcısı](#web-browser-to-web-application)
  * [Tek sayfalı uygulama (SPA)](#single-page-application-spa)
  * [Web API yerel uygulama](#native-application-to-web-api)
  * [Web uygulaması Web API'si](#web-application-to-web-api)
  * [Arka plan programı veya Web API için sunucu uygulaması](#daemon-or-server-application-to-web-api)

## <a name="basics-of-authentication-in-azure-ad"></a>Azure AD kimlik doğrulaması temelleri
Azure AD alanında kimlik doğrulaması temel kavramları hakkında bilginiz yoksa bu bölümü okuyun. Aksi takdirde, aşağı atlamak isteyebilirsiniz [uygulama türleri ve senaryolar](#application-types-and-scenarios).

Şimdi burada kimlik, gerekli en temel senaryo düşünün: bir kullanıcının bir web tarayıcısında bir web uygulaması için kimlik doğrulama yapılması gerekir. Bu senaryo, daha ayrıntılı biçimde açıklandığı [Web uygulamasına Web tarayıcısı](#web-browser-to-web-application) bölüm, ancak kullanıcının Azure AD özelliklerini göstermek ve senaryoyu nasıl çalıştığını conceptualize için yararlı bir başlangıç noktası. Bu senaryo için aşağıdaki çizime göz önünde bulundurun:

![Oturum açma web uygulamasına genel bakış](./media/active-directory-authentication-scenarios/basics_of_auth_in_aad.png)

Yukarıdaki diyagramda ile unutmayın, işte çeşitli bileşenleri hakkında bilmeniz gerekenler:

* Azure AD kimlik, kullanıcı ve kuruluşun dizininde mevcut uygulamaların kimliğini doğrulamak ve sonuçta bu kullanıcıların ve uygulamaların başarılı bir kimlik doğrulaması sırasında güvenlik belirteçleri verme sorumlu sağlayıcıdır.
* Azure ad kimlik doğrulama dış istediği bir uygulama kaydeder ve uygulama dizinindeki benzersiz olarak tanımlayan Azure AD'de kayıtlı olması gerekir.
* Geliştiriciler, kimlik doğrulama protokolü ayrıntıları işleyerek kolaylaştırmak için açık kaynaklı Azure AD kimlik doğrulama kitaplıkları kullanabilirsiniz. Bkz: [Azure Active Directory kimlik doğrulama kitaplıkları](active-directory-authentication-libraries.md) daha fazla bilgi için.

• Bir kullanıcının kimliği doğrulandıktan sonra uygulamayı hedeflenen taraflar için bu kimlik doğrulaması başarılı olmak için kullanıcının güvenlik belirteci doğrulamanız gerekir. Geliştiriciler, Azure AD'den JSON Web belirteçleri (JWT) veya SAML 2.0 dahil olmak üzere herhangi bir belirteci doğrulaması işlemek için sağlanan kimlik doğrulama kitaplıkları kullanabilirsiniz. Doğrulama el ile gerçekleştirmek istiyorsanız, bkz: [JWT belirteci işleyicisi](https://msdn.microsoft.com/library/dn205065.aspx) belgeleri.

> [!IMPORTANT]
> Azure AD Belirteçleri imzalamak ve bunların geçerli olduğunu doğrulamak için ortak anahtar şifrelemesi kullanır. Bkz: [önemli bilgiler hakkında imzalama anahtarı Rollover Azure AD'de](active-directory-signing-key-rollover.md) gerekli mantığı hakkında daha fazla bilgi, her zaman güncelleştirildiği son anahtarlarla emin olmak için uygulamanızda olması gerekir.
> 
> 

• Akış isteklerin ve yanıtların kimlik doğrulama işlemi için OAuth 2.0 gibi Openıd Connect, kullanılan kimlik doğrulama protokolü tarafından belirlenir WS-Federasyon veya SAML 2.0. Bu protokollerin daha ayrıntılı olarak ele alınmıştır [Azure Active Directory kimlik doğrulama protokolleri](active-directory-authentication-protocols.md) konu ve aşağıdaki bölümler.

> [!NOTE]
> Azure AD OAuth 2.0 destekler ve kapsamlı olun Openıd Connect standartları taşıyıcı belirteçler, taşıyıcı belirteçlerini Jwt'ler temsil dahil olmak üzere kullanabilirsiniz. Bir taşıyıcı belirteci korunan bir kaynağa "bearer" erişim veren bir basit güvenlik belirtecidir. Bu anlamda belirteç sunabilir herhangi bir tarafa "bearer" dir. İletim ve depolama belirteçte güvenli hale getirmek için gerekli adımları katılmaz varsa bir taraf ilk taşıyıcı belirteci almak için Azure AD ile kimlik doğrulaması yapması gereken ancak ele ve istenmeyen bir şahıs tarafından kullanılır. Bazı güvenlik belirteçleri kullanarak gelen yetkisiz tarafların engellemek için yerleşik bir mekanizma olmakla birlikte, taşıyıcı belirteçlerini bu düzenek yoksa ve Aktarım Katmanı Güvenliği (HTTPS) gibi güvenli bir kanal taşınan gerekir. Bir taşıyıcı belirteci açık bir şekilde iletilirse, bir adam-Orta saldırı kötü amaçlı bir tarafın belirtecini almak ve bir yetkisiz erişim korunan bir kaynağa için kullanmak üzere kullanılabilir. Depolama veya taşıyıcı belirteçlerini daha sonra kullanmak için önbelleğe alma aynı güvenlik ilkeleri uygulayın. Her zaman, uygulamanızın aktarır ve güvenli bir şekilde taşıyıcı belirteçleri depolar emin olun. Taşıyıcı belirteçlerini hakkında daha fazla güvenlik konuları için bkz: [RFC 6750 bölüm 5](http://tools.ietf.org/html/rfc6750).
> 
> 

Temel genel bir bakış sahip olduğunuza göre Azure AD'de sağlama nasıl çalıştığını anlamak için aşağıdaki bölümleri okuyun ve Azure AD genel senaryolarını destekler.

## <a name="claims-in-azure-ad-security-tokens"></a>Azure AD güvenlik belirteçleri talepleri
Azure AD tarafından yayınlanan güvenlik belirteçleri talep veya onaylar doğrulandı konu hakkındaki bilgileri içerir. Bu talep için çeşitli görevleri uygulama tarafından kullanılabilir. Örneğin, bunlar belirtecini Doğrula, ilgilinin dizin Kiracı tanımlamak, kullanıcı bilgilerini görüntülemek, ilgilinin yetkilendirme belirlemek ve benzeri için kullanılabilir. Tüm belirli güvenlik belirtecinde talep belirteci, kullanıcı ve uygulama yapılandırmasını doğrulamak için kullanılan kimlik bilgileri türünü türüne bağlıdır. Aşağıdaki tabloda her Azure AD tarafından gösterilen talep türünü kısa bir açıklaması sağlanır. Daha fazla bilgi için bkz [desteklenen belirteç ve talep türleri](active-directory-token-and-claims.md).

| İste | Açıklama |
| --- | --- |
| Uygulama Kimliği |Belirteci kullanarak uygulamayı tanımlar. |
| Hedef kitle |Belirteç yöneliktir alıcı kaynak tanımlar. |
| Uygulama kimlik doğrulama bağlamı sınıf başvurusu |Nasıl istemci kimliği doğrulanmış (ortak istemci gizli istemci karşılaştırması) gösterir. |
| Anlık kimlik doğrulaması |Kimlik doğrulaması gerçekleştiği saat ve tarihi kaydeder. |
| Kimlik Doğrulama Yöntemi |Belirteç konu nasıl doğrulandı gösterir (parola, sertifika, vb.). |
| Ad |Kullanıcının verilen adı kümesi olarak Azure AD içinde sağlar. |
| Gruplar |Kullanıcının üyesi olduğu nesne kimlikleri Azure AD grupları içerir. |
| Kimlik Sağlayıcısı |Belirtecin konusu kimlik doğrulaması kimlik sağlayıcısı kaydeder. |
| Çıkışı |Belirteç, belirteç yenilik için sık kullanılan düzenlendiği süreyi kaydeder. |
| Veren |Azure AD kiracısı yanı sıra belirteç yayılan STS tanımlar. |
| Soyadı |Kullanıcının soyadı kümesi olarak Azure AD içinde sağlar. |
| Ad |Belirteç konu tanımlayan bir insan okunabilir değer sağlar. |
| Nesne Kimliği |Konunun değişmez, benzersiz tanıtıcısı Azure AD'de içerir. |
| Roller |Kullanıcı verildi Azure AD uygulama rolleri kolay adını içerir. |
| Kapsam |İstemci uygulaması için verilen izinleri belirtir. |
| Konu |Hakkında bilgi belirteci onaylar asıl gösterir. |
| Kiracı kimliği |Değişmez, benzersiz bir tanımlayıcı belirtecin dizin Kiracı içerir. |
| Belirteç ömrü |Bir belirtecin geçerli olduğu zaman aralığını tanımlar. |
| Kullanıcı asıl adı |Konu kullanıcı asıl adını içerir. |
| Sürüm |Belirtecin sürüm numarasını içerir. |

## <a name="basics-of-registering-an-application-in-azure-ad"></a>Azure AD'de uygulama kaydetme temelleri
Azure ad kimlik doğrulama outsources herhangi bir uygulama bir dizinde kayıtlı olması gerekir. Bu adım, Azure AD nerede, kimlik doğrulamasından sonra uygulamanızı tanımlamak için URI yanıtları göndermek için URL'yi URL'si de uygulamanız ile ilgili söyleyen içerir. Bu bilgiler birkaç anahtar nedenlerle gereklidir:

* Azure AD oturum açma veya değiş tokuşu belirteçleri işlerken uygulamayla iletişim kurmak için koordinatları gerekir. Bunlar aşağıdakileri içerir:
  
  * Uygulama Kimliği URI: Bir uygulama için tanımlayıcı. Bu değer için bir belirteç çağıran hangi uygulamanın istediği belirtmek için kimlik doğrulaması sırasında Azure AD gönderilir. Ayrıca, böylece uygulamanın hedef edildi bilir bu değer belirteç dahil edilir.
  * URL ve yeniden yönlendirme URI'sini Yanıtla: bir web API ya da web uygulaması söz konusu olduğunda, yanıt URL'si için Azure AD kimlik doğrulama başarılı olursa bir belirteç de dahil olmak üzere kimlik doğrulaması yanıtını gönderecek konumdur. Yerel bir uygulamaya söz konusu olduğunda yeniden yönlendirme URI'sini Azure AD Kullanıcı aracısını bir OAuth 2.0 isteğindeki yönlendirir bir benzersiz bir tanımlayıcıdır.
  * Uygulama Kimliği: Uygulama kaydettirildiğini Azure AD tarafından oluşturulan bir uygulama kimliği. Bir yetkilendirme kodu veya belirteç isterken uygulama kimliği ve anahtarı kimlik doğrulaması sırasında Azure AD gönderilir.
  * Anahtar: uygulama kimliği ile birlikte doğrulanırken Azure AD ile bir web API'sini çağırmak için gönderilen anahtarı.
* Azure AD uygulama, dizin verilerini, kuruluşunuzdaki diğer uygulamaları erişmek ve benzeri için gerekli izinlere sahip olduğundan emin olun gerekiyor

Geliştirilmiş ve Azure AD ile tümleşik uygulamalar iki kategorisi vardır anladığınızda sağlama daha anlaşılır olur:

* Tek kiracılı uygulama: tek bir kiracı uygulama kullanan bir kuruluştaki yöneliktir. Bir kurumsal geliştirici tarafından yazılan genellikle satır iş kolu (LoB) uygulamaları şunlardır. Tek kiracılı uygulama yalnızca bir dizinde kullanıcılar tarafından erişilmesi gerekir ve sonuç olarak, yalnızca bir dizinde sağlanması gerekir. Bu uygulamalar genellikle kuruluştaki bir geliştirici tarafından kaydedilir.
* Çok kiracılı uygulama: çok kiracılı uygulama birçok kuruluşta kullanılmak üzere tasarlanmamıştır yalnızca bir kuruluş. Bunlar genellikle yazılım olarak-hizmet (SaaS) uygulamaları bir bağımsız yazılım satıcısı (ISV) tarafından yazılmış olan. Çok kiracılı uygulamalara, bunları kaydetmek için kullanıcı veya yönetici izni gerektiren burada bunlar kullanılacak, her dizin için hazırlanması gerekir. Bir uygulama dizinde kayıtlı ve erişim verilen grafik API'sini veya belki başka bir web API için bu onay işlemi başlar. Bir kullanıcı veya farklı bir kuruluşta yöneticisinden uygulamayı kullanmak kaydolduğunda, bunlar uygulama gerektiriyor izinleri görüntüleyen bir iletişim kutusu sunulur. Kullanıcı veya yönetici uygulama erişimi için belirtilen veri sağlar ve son olarak uygulamaları kendi dizininde kaydeder uygulamaya ardından onay. Daha fazla bilgi için bkz: [onayı Framework'e Genel Bakış](active-directory-integrating-applications.md#overview-of-the-consent-framework).

Bazı hususlar, çok kiracılı uygulama yerine tek bir kiracı Uygulama geliştirirken kaynaklanır. Uygulamanızın kullanılabilir birden fazla dizine kullanıcılara yapıyorsanız, örneğin, hangi Kiracı belirlemek için bir mekanizma gereken kullanıcılar oturum açtınız. Azure AD içinde tüm dizinlerinden belirli bir kullanıcıyı tanımlamak çok kiracılı uygulama gereken sırada bir kullanıcının kendi dizinde aramak bir tek kiracılı uygulama yeterlidir. Bu görevi gerçekleştirmek için Azure AD herhangi bir çok kiracılı uygulama oturum açma istekleri, bir kiracı özel uç noktası yerine burada yönlendirebilirsiniz ortak bir kimlik doğrulama uç noktası sağlar. Bir kiracı özel uç noktası https://login.microsoftonline.com/contoso.onmicrosoft.com olabilir ancak bu bitiş https://login.microsoftonline.com/common tüm dizinler için Azure AD'de noktasıdır. Ortak bir uç oturum açma, oturum kapatma ve belirteç doğrulama sırasında birden çok kiracıya işlemek için gerekli mantığı gerekir çünkü Uygulamanızı geliştirirken dikkat edilmesi özellikle önemlidir.

Şu anda bir tek kiracılı uygulama geliştirme, ancak çoğu kuruluş için kullanılabilir hale getirmek istediğiniz kolayca çok kiracılı yapmak için uygulama ve Azure AD yapılandırmasıyla özellikli değişiklik yapabilirsiniz. Ayrıca, kimlik doğrulaması bir çoklu kiracı ya da çok kiracılı uygulama sağlama olup Azure AD, tüm dizinlerde tüm belirteçleri aynı imzalama anahtarı kullanır.

Bu belgede listelenen her bir senaryo, sağlama gereksinimleri açıklanır alt bir bölüm içerir. Sağlama Azure AD'de bir uygulama ve tek ve çoklu kiracı uygulamaları arasındaki farklar hakkında daha ayrıntılı bilgi için bkz: [Azure Active Directory Tümleştirme uygulamalarla](active-directory-integrating-applications.md) daha fazla bilgi için. Azure AD'de uygulama senaryoları anlamak için okumaya devam edin.

## <a name="application-types-and-scenarios"></a>Uygulama türleri ve senaryolar
Bu belgede açıklanan senaryoların her biri, çeşitli diller ve platformlar kullanılarak geliştirilebilir. Bunlar tüm kullanılabilir tam kod örnekleri tarafından yedeklenen bizim [kod örnekleri Kılavuzu](active-directory-code-samples.md), veya doğrudan ilgili [GitHub örnek depoları](https://github.com/Azure-Samples?utf8=%E2%9C%93&query=active-directory). Uygulamanızı özgül ya da bir uçtan uca senaryoyu parçası gerekirse, ayrıca, çoğu durumda bu işlev bağımsız olarak eklenebilir. Örneğin, bir web API'si çağıran yerel bir uygulamanız varsa, ayrıca web API'si çağıran bir web uygulaması kolayca ekleyebilirsiniz. Aşağıdaki diyagram bu senaryoları ve uygulama türleri gösterir ve farklı bileşenler eklenebilir nasıl:

![Uygulama türleri ve senaryolar](./media/active-directory-authentication-scenarios/application_types_and_scenarios.png)

Azure AD tarafından desteklenen beş birincil uygulama senaryoları şunlardır:

* [Web tarayıcısı Web uygulamasına](#web-browser-to-web-application): bir kullanıcı Azure AD tarafından güvenliği sağlanan bir web uygulaması oturum açması gerekiyor.
* [Tek sayfa uygulama (SPA)](#single-page-application-spa): bir kullanıcı Azure AD tarafından güvenliği sağlanan bir tek sayfa uygulaması oturum açması gerekiyor.
* [Web API yerel uygulamaya](#native-application-to-web-api): Web'den Azure AD tarafından güvenli API kaynakları almak için bir kullanıcının kimliğini doğrulamak telefon, tablet veya PC çalıştıran yerel bir uygulama gerekiyor.
* [Web uygulaması Web API](#web-application-to-web-api): bir web uygulaması kaynakları web API'si Azure AD tarafından güvenlik altına almanız gerekir.
* [Arka plan programı veya Web API için sunucu uygulaması](#daemon-or-server-application-to-web-api): arka plan programı uygulama veya bir sunucu uygulaması web kullanıcı arabirimi ile kaynakları web API'si Azure AD tarafından güvenlik altına almanız gerekir.

### <a name="web-browser-to-web-application"></a>Web uygulaması için Web tarayıcısı
Bu bölümde bir web uygulaması için bir web tarayıcısında bir kullanıcının kimliğini doğrulayan bir uygulama açıklanmaktadır. Bu senaryoda, web uygulaması için Azure AD oturum açmak için kullanıcının tarayıcısına yönlendirir. Azure AD bir güvenlik belirteci kullanıcı hakkında talepleri içeren kullanıcının tarayıcısından oturum açma yanıt verir. WS-Federation, SAML 2.0 ve Openıd Connect protokolleri kullanarak oturum bu senaryoyu destekler.

#### <a name="diagram"></a>Diyagram
![Tarayıcıdan web uygulamasına yönelik kimlik doğrulama akışı](./media/active-directory-authentication-scenarios/web_browser_to_web_api.png)

#### <a name="description-of-protocol-flow"></a>Protokol akışı açıklaması
1. Bir kullanıcı oturum açmak için gereksinimleri ve uygulama ziyaret ettiğinde, bunlar bir oturum açma isteği kimlik doğrulama uç noktası aracılığıyla Azure AD içinde yönlendirilir.
2. Oturum açma sayfasında oturum açtığında.
3. Kimlik doğrulaması başarılı olursa, Azure AD kimlik doğrulama belirteci oluşturur ve Azure Portalı'nda yapılandırılan uygulamanın yanıt URL'si bir oturum açma yanıtı döndürür. Bir üretim uygulaması için HTTPS bu yanıt URL'si olmalıdır. Döndürülen belirteci belirteci doğrulamak için uygulama tarafından gerekli olan Azure AD ve kullanıcı hakkında talepleri içerir.
4. Uygulama, bir ortak imzalama anahtarı ve sertifikayı verenin bilgileri kullanılabilir Federasyon meta veri belgesi için Azure AD kullanarak belirteci doğrular. Uygulama belirteci doğrular sonra Azure AD kullanıcı ile yeni bir oturum başlatır. Bu oturumda, sona erene kadar uygulamaya erişim olanak tanır.

#### <a name="code-samples"></a>Kod Örnekleri
Kod örnekleri için Web tarayıcısı Web uygulaması senaryoları için bkz. Ve daha sonra yeniden sık--her zaman yeni örnekler eklediğimiz denetleyin. [Web tarayıcısı Web uygulamasına](active-directory-code-samples.md#web-browser-to-web-application).

#### <a name="registering"></a>Kaydetme
* Tek bir kiracı: yalnızca kuruluşunuz için bir uygulama geliştiriyorsanız, şirketinizin dizininde Azure Portalı'nı kullanarak kayıtlı olması gerekir.
* Çok Kiracılı: kuruluşunuzun dışındaki kullanıcılar tarafından kullanılan bir uygulama geliştiriyorsanız, şirketinizin dizinde kayıtlı olması gerekir, ancak da uygulamayı kullanarak her kuruluşun dizinde kayıtlı olması gerekir. Uygulamanızı directory'lerinde kullanılabilmesi için bunları uygulamanıza kabul etkinleştiren müşteriler için kaydolma işlemini içerebilir. Bunlar, uygulamanız için kaydolduğunuzda, uygulama gerektiriyor izinleri gösteren bir iletişim kutusu, ardından onay seçeneği ile sunulur. Gerekli izinleri bağlı olarak diğer kuruluştaki yönetici izni vermek için gerekebilir. Kullanıcı veya yönetici izin, uygulamanın kendi Directory'ye kaydedilir. Daha fazla bilgi için bkz: [Azure Active Directory Tümleştirme uygulamalarla](active-directory-integrating-applications.md).

#### <a name="token-expiration"></a>Belirteç süre sonu
Azure AD tarafından verilen belirtecin süresi sona erdiğinde kullanıcının oturum sona erer. Uygulamanızı bir süre işlem yapılmadığında üzerinde temel alarak kullanıcılara imzalama gibi isterseniz, bu süre içinde kısaltabilir. Oturum süresi sona erdiğinde, kullanıcının yeniden oturum açmanız istenir.

### <a name="single-page-application-spa"></a>Tek sayfalı uygulama (SPA)
Bu bölümde kimlik doğrulaması için tek bir sayfa, Azure AD kullanır ve OAuth 2.0 örtük yetkilendirme, web API geri bitiş güvenli vermek uygulama, açıklanmaktadır. Tek sayfa uygulamaları, genellikle tarayıcı ve sunucu üzerinde çalışır ve uygulamanın iş mantığını uygular Web API'si arka uç çalıştıran bir JavaScript sunu katmanı (ön uç) olarak yapılandırılmıştır. Örtük yetkilendirme verme hakkında daha fazla bilgi ve Uygulama senaryonuz için uygun olup olmadığına karar vermenize yardımcı olması için bkz: [örtük OAuth2 anlama izin akışı Azure Active Directory'de](active-directory-dev-understanding-oauth2-implicit-grant.md).

Bu senaryoda, kullanıcı, oturum açtığında JavaScript ön uç kullanır [Active Directory Authentication Library (ADAL JavaScript için. JS)](https://github.com/AzureAD/azure-activedirectory-library-for-js/tree/dev) ve Azure AD'den bir kimliği belirteci (id_token) almak için örtük yetkilendirme verme. Belirteç önbelleğe alınır ve istemci bu isteği taşıyıcı belirteci olarak kendi Web API çağrıları güvenliğinin OWIN ara yazılımı ile son yedekleme yaparken ekler. 

#### <a name="diagram"></a>Diyagram
![Tek sayfa uygulama diyagramı](./media/active-directory-authentication-scenarios/single_page_app.png)

#### <a name="description-of-protocol-flow"></a>Protokol akışı açıklaması
1. Kullanıcı web uygulaması gider.
2. Uygulama tarayıcıya JavaScript ön uç (sunu katmanı) döndürür.
3. Kullanıcı oturum açma bağlantısına tıklayarak oturum açma, örneğin başlatır. Tarayıcı bir GET kimliği belirteç istemek için Azure AD yetkilendirme uç noktasına gönderir. Bu istek uygulama kimliği ve yanıt URL'si sorgu parametreleri içerir.
4. Azure AD yanıt URL'si Azure Portalı'nda yapılandırılan kayıtlı yanıt URL'si karşı doğrular.
5. Oturum açma sayfasında oturum açtığında.
6. Kimlik doğrulaması başarılı olursa, Azure AD kimliği belirteci oluşturur ve uygulama yanıt URL'si (#) URL parçası döndürür. Bir üretim uygulaması için HTTPS bu yanıt URL'si olmalıdır. Döndürülen belirteci belirteci doğrulamak için uygulama tarafından gerekli olan Azure AD ve kullanıcı hakkında talepleri içerir.
7. Tarayıcıda çalışan JavaScript istemci kodu belirteç uygulamanın web API geri bitiş çağrıları güvenliğinde kullanılacak yanıt gelen ayıklar.
8. Tarayıcı uygulamanın web API geri yetkilendirme üst bilgi erişim belirteciyle bitiş çağırır.

#### <a name="code-samples"></a>Kod Örnekleri
Kod örnekleri tek sayfa uygulama (SPA) senaryoları için bkz. Geri sık--her zaman yeni örnekler eklediğimiz kontrol ettiğinizden emin olun. [Tek sayfa uygulama (SPA)](active-directory-code-samples.md#single-page-application-spa).

#### <a name="registering"></a>Kaydetme
* Tek bir kiracı: yalnızca kuruluşunuz için bir uygulama geliştiriyorsanız, şirketinizin dizininde Azure Portalı'nı kullanarak kayıtlı olması gerekir.
* Çok Kiracılı: kuruluşunuzun dışındaki kullanıcılar tarafından kullanılan bir uygulama geliştiriyorsanız, şirketinizin dizinde kayıtlı olması gerekir, ancak da uygulamayı kullanarak her kuruluşun dizinde kayıtlı olması gerekir. Uygulamanızı directory'lerinde kullanılabilmesi için bunları uygulamanıza kabul etkinleştiren müşteriler için kaydolma işlemini içerebilir. Bunlar, uygulamanız için kaydolduğunuzda, uygulama gerektiriyor izinleri gösteren bir iletişim kutusu, ardından onay seçeneği ile sunulur. Gerekli izinleri bağlı olarak diğer kuruluştaki yönetici izni vermek için gerekebilir. Kullanıcı veya yönetici izin, uygulamanın kendi Directory'ye kaydedilir. Daha fazla bilgi için bkz: [Azure Active Directory Tümleştirme uygulamalarla](active-directory-integrating-applications.md).

Uygulama kaydolduktan sonra onu OAuth 2.0 örtük verme protokolünü kullanmak üzere yapılandırılmış olması gerekir. Varsayılan olarak, bu protokolü uygulamalar için devre dışıdır. Uygulamanız için OAuth2 örtük Grant Protokolü etkinleştirmek için Azure Portalı'ndan kendi uygulama bildirimi düzenleyin ve "oauth2AllowImplicitFlow" değeri true olarak ayarlayın. Ayrıntılı yönergeler için bkz: [etkinleştirme OAuth 2.0 örtük verme tek sayfa uygulamaları için](active-directory-integrating-applications.md).

#### <a name="token-expiration"></a>Belirteç süre sonu
Azure AD ile kimlik doğrulamasını yönetmek için ADAL.js kullandığınızda, süresi dolmuş bir belirtecini yenilemeyi yanı sıra uygulama tarafından çağrılabilir ek web API kaynaklar için belirteçleri alma kolaylaştıran birkaç özelliklerinden yararlanır. Kullanıcı Azure AD ile başarıyla doğruladığında, tarayıcı ve Azure AD arasında kullanıcı için bir tanımlama bilgisi tarafından güvenli bir oturum oluşturulur. Oturum kullanıcı ve Azure AD arasında ve kullanıcı ve sunucu üzerinde çalışan web uygulaması arasında değil var olduğunu dikkate almak önemlidir. Bir belirtecinin süresi dolduğunda, ADAL.js bu oturumu sessizce başka bir belirteç elde etmek için kullanır. Bunu OAuth örtük verme protokolünü kullanarak isteği alıp göndermek için gizli bir iFrame kullanarak yapar. ADAL.js, bu aynı mekanizması sessiz bir şekilde erişim belirteçleri diğer web için Azure AD destek çıkış noktaları arası kaynak paylaşımı (CORS), bu kaynakları sürece uygulama çağrıları kullanıcının dizinde kaydedilir ve oturum açma sırasında kullanıcı tarafından gerekli tüm izin verildi API kaynakları almak üzere de kullanabilirsiniz.

### <a name="native-application-to-web-api"></a>Web API yerel uygulama
Bu bölümde, bir kullanıcı adına bir web API'si çağıran bir yerel uygulamayı açıklanmaktadır. Bu senaryo bir ortak istemci OAuth 2.0 yetkilendirme kodu verme türüyle, 4.1 bölümünde açıklandığı gibi üzerine inşa edilmiştir [OAuth 2.0 belirtimi](http://tools.ietf.org/html/rfc6749). Yerel uygulama OAuth 2.0 protokolünü kullanarak bir kullanıcı için erişim belirtecini alır. Bu erişim belirteci sonra kullanıcı yetkisi verir ve istenen kaynak döndüren API web isteği gönderilir.

#### <a name="diagram"></a>Diyagram
![Web API diyagramı yerel uygulama](./media/active-directory-authentication-scenarios/native_app_to_web_api.png)

#### <a name="authentication-flow-for-native-application-to-api"></a>Yerel uygulamadan API'ye yönelik kimlik doğrulama akışı
#### <a name="description-of-protocol-flow"></a>Protokol akışı açıklaması
AD kimlik doğrulama kitaplıkları kullanıyorsanız, aşağıda açıklanan protokol ayrıntılarını çoğunu sizin için tarayıcı açılır pencere, belirteç önbelleğe alma ve yenileme belirteçleri işlenmesi gibi işlenir.

1. Yerel uygulama yetkilendirme uç noktası için Azure AD içinde istekte açılan bir tarayıcı kullanarak. Bu istek, Azure Portalı'nı ve web API için uygulama kimliği URI'sini gösterildiği gibi uygulama kimliği ve yeniden yönlendirme URI'si yerel uygulamasının içerir. Kullanıcı zaten oturum kurmadı varsa, bunlar yeniden oturum açmanız istenir
2. Azure AD kullanıcının kimliğini doğrular. Çok kiracılı uygulama ise ve onay uygulamayı kullanmak için gerekli olan, kullanıcı Bunlar zaten yapmadıysanız onayını istemeniz gerekir. Azure AD, izin verme sonra ve temel kimlik doğrulaması başarılı bir kimlik doğrulama kodu yanıtı istemci uygulamanın yeniden yönlendirme URI'si dön verir.
3. Azure AD bir yetkilendirme kodu geri yanıt yeniden yönlendirme URI'si gönderdiğinde, istemci uygulaması tarayıcı etkileşimi durdurur ve yetkilendirme kodu yanıttan ayıklar. Bu yetkilendirme kodu kullanarak, istemci uygulamasının yetkilendirme kodu, istemci uygulaması (uygulama kimliği ve yeniden yönlendirme URI'si) ve istenen kaynak (uygulama kimliği URI'si web API'si için) hakkında ayrıntılar içeren Azure AD belirteç uç noktası için bir istek gönderir.
4. İstemci uygulaması ve web API'si hakkında bilgi ve yetkilendirme kodu Azure AD tarafından doğrulanır. Başarılı bir doğrulama sırasında Azure AD iki belirteci döndürür: JWT erişim belirteci ve bir JWT yenileme belirteci. Ayrıca, Azure AD görünen adı ve Kiracı kimliklerine gibi kullanıcıyla ilgili temel bilgileri döndürür
5. HTTPS üzerinden istemci uygulaması JWT dizesi "Bearer" tayin ile web API isteğinin yetkilendirme üst bilgi eklemek için döndürülen JWT erişim belirtecini kullanır. Web API JWT belirteci doğrular ve doğrulama başarılı olursa, istenen kaynak döndürür.
6. Erişim belirtecinin süresi dolduğunda, istemci uygulaması kullanıcının yeniden kimlik doğrulaması gerekiyorsa belirten bir hata alırsınız. Uygulamanın geçerli yenileme belirteci varsa, yeniden oturum açmak için kullanıcıya sormadan yeni bir erişim belirteci almak için kullanılabilir. Yenileme belirtecinin süresi dolarsa, uygulama etkileşimli kullanıcı bir kez yeniden kimlik doğrulaması gerekir.

> [!NOTE]
> Azure AD tarafından verilen yenileme belirteci birden çok kaynaklara erişmek için kullanılabilir. Örneğin, iki web API çağırma izni olan bir istemci uygulaması varsa, yenileme belirtecini bir erişim diğer web API de belirtecini almak için kullanılabilir.
> 
> 

#### <a name="code-samples"></a>Kod Örnekleri
Kod örnekleri, Web API senaryoları için yerel uygulama için bkz. Ve daha sonra yeniden sık--her zaman yeni örnekler eklediğimiz denetleyin. [Web API yerel uygulamaya](active-directory-code-samples.md#native-application-to-web-api).

#### <a name="registering"></a>Kaydetme
* Tek bir kiracı: Her iki yerel uygulaması ve web API kaydedilmelidir aynı dizinde Azure AD'de. Web API kaynaklarını yerel uygulamanın erişimi sınırlamak için kullanılan izinler kümesi kullanıma sunmak için yapılandırılabilir. İstemci uygulama, Azure Portalı'nda "İzinleri diğer uygulamaların" açılan menüsünden istenen izinleri seçer.
* Çok Kiracılı: İlk olarak, yerel uygulama her zaman sadece geliştirici veya publisher'ın dizinde kayıtlı. İkinci olarak, yerel uygulama işlev için gereken izinleri belirtmek için yapılandırılır. Bir kullanıcının veya yöneticinin hedef dizinde, kuruluşları için kullanılabilir hale getirir uygulamaya izin verdiğinde bu gerekli izinlerin listesi iletişim kutusunda gösterilir. Bazı uygulamalar kuruluştaki herhangi bir kullanıcı onayı yalnızca kullanıcı düzeyinde izinler gerektirir. Diğer uygulamalar kuruluştaki bir kullanıcı için onay veremez yönetici düzeyi izinler gerektirir. Yalnızca bir dizin Yöneticisi bu izin düzeyini gerektiren uygulamalar için izin verebilirsiniz. Kullanıcı veya yönetici izin, yalnızca web API kendi Directory'ye kaydedilir. Daha fazla bilgi için bkz: [Azure Active Directory Tümleştirme uygulamalarla](active-directory-integrating-applications.md).

#### <a name="token-expiration"></a>Belirteç süre sonu
Yerel uygulama, bir JWT belirteci erişmek için kendi yetkilendirme kodu kullandığında, ayrıca, bir JWT yenileme belirteci alır. Erişim belirtecinin süresi dolduğunda, yenileme belirteci kullanıcı yeniden oturum açmak için gerek kalmadan yeniden kimlik doğrulaması için kullanılabilir. Bu yenileme belirteci sonra kullanıcının kimliğini doğrulamak için kullanılan bir yeni bir erişim belirteci ve yenileme belirteci sonuçlanır.

### <a name="web-application-to-web-api"></a>Web uygulaması Web API'si
Bu bölümde bir web API kaynakları almak için gereken bir web uygulaması açıklanmaktadır. Bu senaryoda, web uygulaması web API'si çağırma ve kimlik doğrulamasını yapmak için kullanabileceğiniz iki kimlik türü vardır: uygulama kimliği ya da temsilci atanan kullanıcı kimliği.

*Uygulama Kimliği:* bu senaryo, uygulama olarak kimlik doğrulaması ve web API erişmek için OAuth 2.0 istemci kimlik bilgileri sağlama kullanır. Uygulama kimliği, API, yalnızca web uygulaması, aradığı algılayabilir web kullanırken web API kullanıcı hakkındaki tüm bilgileri almaz. Uygulamanın kullanıcı hakkındaki bilgileri alırsa, uygulama protokolü aracılığıyla gönderilir ve Azure AD tarafından imzalanmamış. Web API, web uygulaması için kullanıcı kimliklerinin güvenir. Bu nedenle, bu deseni güvenilir alt sistem adı verilir.

*Kullanıcı kimliği için temsilci:* bu senaryo iki yolla gerçekleştirilebilir: Openıd Connect ve OAuth 2.0 yetkilendirme kodu verme gizli istemcisi ile. Web uygulaması web kullanıcı, web uygulaması başarıyla kimlik doğrulaması ve web uygulaması web API'sini çağırmak için bir temsilci olarak atanan kullanıcı kimliğini alabilmiş API kanıtlar kullanıcı için bir erişim belirteci alır. Bu erişim belirteci kullanıcı yetkisi verir ve istenen kaynak döndüren API web isteği gönderilir.

#### <a name="diagram"></a>Diyagram
![Web uygulaması için Web API diyagramı](./media/active-directory-authentication-scenarios/web_app_to_web_api.png)

#### <a name="description-of-protocol-flow"></a>Protokol akışı açıklaması
Uygulama kimliği ve temsilci olarak atanan kullanıcı kimliğini türleri akışında ele alınmıştır. Aralarındaki en önemli fark, kullanıcı oturum açma ve web API erişmek önce temsilci atanan kullanıcı kimliğini ilk bir yetkilendirme kodu almalıdır ' dir.

##### <a name="application-identity-with-oauth-20-client-credentials-grant"></a>Uygulama Kimliği OAuth 2.0 istemci ile kimlik bilgileri verin
1. Bir kullanıcı Azure ad ile web uygulamasında oturum (bkz [Web uygulamasına Web tarayıcısı](#web-browser-to-web-application) yukarıda).
2. Web uygulaması, böylece web API kimlik doğrulaması ve istenen kaynak almak bir erişim belirteci alması gerekiyor. Kimlik bilgisi, uygulama kimliği ve web API uygulama kimliği URI'sini sağlayan Azure AD belirteç uç noktası için bir istek kolaylaştırır.
3. Azure AD uygulama kimliğini doğrular ve web API'sini çağırmak için kullanılan bir JWT erişim belirteci döndürür.
4. HTTPS üzerinden web uygulaması "Bearer" tayin JWT dizesiyle web API isteğinin yetkilendirme üst bilgi eklemek için döndürülen JWT erişim belirtecini kullanır. Web API JWT belirteci doğrular ve doğrulama başarılı olursa, istenen kaynak döndürür.

##### <a name="delegated-user-identity-with-openid-connect"></a>Openıd temsilci atanan kullanıcı kimliğiyle Bağlan
1. Bir kullanıcının Azure AD kullanarak bir web uygulaması oturum açtığı (bkz [Web uygulamasına Web tarayıcısı](#web-browser-to-web-application) yukarıdaki bölümde). Web uygulamasının kullanıcı henüz kendi adına web API'sini çağırmak web uygulaması izin vermek üzere seçtiği değil, kullanıcının onayını istemeniz gerekir. Uygulamanın gerektirdiği izinler görüntüler ve bunları herhangi biri yönetici düzeyi izinler varsa, dizininde normal bir kullanıcı onayı mümkün olmaz. Uygulamanın zaten gerekli izinlere sahip şekilde bu izni işlem yalnızca değil tek bir kiracı uygulamalar, çok kiracılı uygulamalara uygulanır. Kullanıcının oturum açtığı zaman web uygulamasına bir kimlik doğrulama kodu yanı sıra kullanıcı hakkında bilgi içeren bir kimliği belirteci aldı.
2. Web uygulamasını Azure AD tarafından verilen yetkilendirme kodu kullanarak, yetkilendirme kodu, istemci uygulaması (uygulama kimliği ve yeniden yönlendirme URI'si) ve istenen kaynak (uygulama kimliği URI'si web API'si için) hakkında ayrıntılar içeren Azure AD belirteç uç noktası için bir istek gönderir.
3. Web uygulaması ve web API hakkındaki bilgileri ve yetkilendirme kodu Azure AD tarafından doğrulanır. Başarılı bir doğrulama sırasında Azure AD iki belirteci döndürür: JWT erişim belirteci ve bir JWT yenileme belirteci.
4. HTTPS üzerinden web uygulaması "Bearer" tayin JWT dizesiyle web API isteğinin yetkilendirme üst bilgi eklemek için döndürülen JWT erişim belirtecini kullanır. Web API JWT belirteci doğrular ve doğrulama başarılı olursa, istenen kaynak döndürür.

##### <a name="delegated-user-identity-with-oauth-20-authorization-code-grant"></a>Temsilci atanan kullanıcı kimliği ile OAuth 2.0 yetkilendirme kodu verme
1. Bir kullanıcı zaten olan kimlik doğrulama mekanizması Azure AD bağımsız olan bir web uygulaması için imzalanır.
2. Web uygulaması bir tarayıcı isteğini uygulama kimliği sağlayan Azure AD yetkilendirme uç noktasına sorunları için bir erişim belirteci almak ve başarılı kimlik doğrulamasının ardından web uygulaması için yeniden yönlendirme URI'si için bir kimlik doğrulama kodu gerektirir. Azure AD ile oturum açtığında.
3. Web uygulamasının kullanıcı henüz kendi adına web API'sini çağırmak web uygulaması izin vermek üzere seçtiği değil, kullanıcının onayını istemeniz gerekir. Uygulamanın gerektirdiği izinler görüntüler ve bunları herhangi biri yönetici düzeyi izinler varsa, dizininde normal bir kullanıcı onayı mümkün olmaz. Bu izin, hem tek hem de çok kiracılı uygulama için geçerlidir.  Tek bir kiracı durumda da, bir yönetici, kullanıcılar adına yönetici onayı için onay gerçekleştirebilirsiniz.  Bu yapılabilir kullanarak `Grant Permissions` düğmesini [Azure Portal](https://portal.azure.com). 
4. Kullanıcının seçtiği sonra web uygulaması, bir erişim belirteci alması gereken yetkilendirme kodu alır.
5. Web uygulamasını Azure AD tarafından verilen yetkilendirme kodu kullanarak, yetkilendirme kodu, istemci uygulaması (uygulama kimliği ve yeniden yönlendirme URI'si) ve istenen kaynak (uygulama kimliği URI'si web API'si için) hakkında ayrıntılar içeren Azure AD belirteç uç noktası için bir istek gönderir.
6. Web uygulaması ve web API hakkındaki bilgileri ve yetkilendirme kodu Azure AD tarafından doğrulanır. Başarılı bir doğrulama sırasında Azure AD iki belirteci döndürür: JWT erişim belirteci ve bir JWT yenileme belirteci.
7. HTTPS üzerinden web uygulaması "Bearer" tayin JWT dizesiyle web API isteğinin yetkilendirme üst bilgi eklemek için döndürülen JWT erişim belirtecini kullanır. Web API JWT belirteci doğrular ve doğrulama başarılı olursa, istenen kaynak döndürür.

#### <a name="code-samples"></a>Kod Örnekleri
Web API senaryolarında Web uygulaması için kod örnekleri bakın. Ve daha sonra yeniden sık--her zaman yeni örnekler eklediğimiz denetleyin. Web [Web API uygulamasına](active-directory-code-samples.md#web-application-to-web-api).

#### <a name="registering"></a>Kaydetme
* Tek bir kiracı: uygulama kimliği hem temsilci atanan kullanıcı kimliğini çalışmaları için web uygulaması ve web API aynı dizinde Azure AD'de kayıtlı olması gerekir. Web API, web uygulamasının kaynaklarına erişimi sınırlamak için kullanılan izinler kümesi kullanıma sunmak için yapılandırılabilir. Temsilci atanan kullanıcı kimlik türü kullanılıyorsa, web uygulamasını Azure Portalı'nda "İzinleri diğer uygulamaların" açılan menüsünden istediğiniz izinleri seçin gerekiyor. Uygulama kimlik türü kullanılıyorsa bu adım gerekli değildir.
* Çok Kiracılı: İlk olarak, web uygulaması işlevsel olmasını gerektirir izinleri belirtmek için yapılandırılmış. Bir kullanıcının veya yöneticinin hedef dizinde, kuruluşları için kullanılabilir hale getirir uygulamaya izin verdiğinde bu gerekli izinlerin listesi iletişim kutusunda gösterilir. Bazı uygulamalar kuruluştaki herhangi bir kullanıcı onayı yalnızca kullanıcı düzeyinde izinler gerektirir. Diğer uygulamalar kuruluştaki bir kullanıcı için onay veremez yönetici düzeyi izinler gerektirir. Yalnızca bir dizin Yöneticisi bu izin düzeyini gerektiren uygulamalar için izin verebilirsiniz. Kullanıcı veya yönetici izin, web uygulaması ve web API hem de bunların dizinde kaydedilir.

#### <a name="token-expiration"></a>Belirteç süre sonu
Web uygulaması, bir JWT belirteci erişmek için kendi yetkilendirme kodu kullandığında, ayrıca, bir JWT yenileme belirteci alır. Erişim belirtecinin süresi dolduğunda, yenileme belirteci kullanıcı yeniden oturum açmak için gerek kalmadan yeniden kimlik doğrulaması için kullanılabilir. Bu yenileme belirteci sonra kullanıcının kimliğini doğrulamak için kullanılan bir yeni bir erişim belirteci ve yenileme belirteci sonuçlanır.

### <a name="daemon-or-server-application-to-web-api"></a>Arka plan programı veya Web API için sunucu uygulaması
Bu bölümde bir web API kaynakları almak için gereken bir arka plan programı veya sunucu uygulaması açıklanmaktadır. Bu bölüm için geçerli iki alt senaryo vardır: web OAuth 2.0 istemci kimlik bilgileri verme türünü; yerleşik API'si çağırmayı gerektiren bir arka plan programı ve web API'si, OAuth 2.0 On-Behalf-Of taslak belirtimi yerleşik çağırmayı gerektiren bir sunucu uygulaması (örneğin, bir web API).

Bir web API'sini çağırmak bir arka plan programı uygulaması gerektiğinde bu senaryo için birkaç anlamak önemlidir. İlk olarak, kullanıcı etkileşimi uygulama kendi kimlik olmasını gerektiriyor ve arka plan programı uygulama ile mümkün değildir. Arka plan programı uygulama toplu iş veya arka planda çalışan bir işletim sistemi hizmet örneğidir. Bu tür bir uygulama bir erişim belirteci kendi uygulama kimliğini kullanır ve uygulama kimliği, kimlik bilgisi (parola veya sertifika) ve uygulama sunma ister Azure ad kimliği URI. Başarılı kimlik doğrulamasının ardından, arka plan programı daha sonra web API'sini çağırmak için kullanılır, Azure AD'den bir erişim belirteci alır.

Senaryo için daha web API'sini çağırmak bir sunucu uygulaması ihtiyacı olduğunda bile bir örnek kullanacak şekilde yardımcı olur. Bir kullanıcı kimlik doğrulaması yerel bir uygulamaya ve bu yerel uygulama web API'si çağırmayı gerektiren düşünün. Azure AD web API'sini çağırmak için JWT erişim belirteci verir. Web API başka bir aşağı akış web API'si çağırma gerekiyorsa, kullanıcının kimliğini temsilci ve ikinci katmanlı web API kimliğini doğrulamak için üzerinde-temsili akış kullanabilirsiniz.

#### <a name="diagram"></a>Diyagram
![Arka plan programı veya sunucu uygulaması için Web API diyagramı](./media/active-directory-authentication-scenarios/daemon_server_app_to_web_api.png)

#### <a name="description-of-protocol-flow"></a>Protokol akışı açıklaması
##### <a name="application-identity-with-oauth-20-client-credentials-grant"></a>Uygulama Kimliği OAuth 2.0 istemci ile kimlik bilgileri verin
1. İlk olarak, sunucu uygulamasının bir etkileşimli oturum açma iletişim kutusu gibi herhangi bir insan etkileşimi olmadan kendisini olarak Azure AD ile kimlik doğrulaması gerekiyor. Kimlik bilgisi, uygulama kimliği ve uygulama kimliği URI'sini sağlayan Azure AD belirteç uç noktası için bir istek kolaylaştırır.
2. Azure AD uygulama kimliğini doğrular ve web API'sini çağırmak için kullanılan bir JWT erişim belirteci döndürür.
3. HTTPS üzerinden web uygulaması "Bearer" tayin JWT dizesiyle web API isteğinin yetkilendirme üst bilgi eklemek için döndürülen JWT erişim belirtecini kullanır. Web API JWT belirteci doğrular ve doğrulama başarılı olursa, istenen kaynak döndürür.

##### <a name="delegated-user-identity-with-oauth-20-on-behalf-of-draft-specification"></a>OAuth 2.0 üzerinde-adına-Of taslak belirtimiyle temsilci olarak atanan kullanıcı kimliği
Aşağıda açıklanan akış (örneğin, bir yerel uygulamayı) başka bir uygulama üzerinde bir kullanıcı kimliği ve kullanıcı kimliğini ilk katmanlı Web API'ye bir erişim belirteci almak için kullanılan varsayar.

1. Yerel uygulama erişim belirtecini ilk katmanlı web API gönderir.
2. İlk katmanlı web API uygulama Kimliğini ve kimlik bilgilerinin yanı sıra, kullanıcının erişim belirtecini sağlayan Azure AD belirteç uç noktası için bir isteği gönderir. Ayrıca, istek ile gönderilen bir web gösteren parametresi on_behalf_of API özgün kullanıcı adına bir aşağı akış web API'sini çağırmak için yeni belirteçleri istiyor.
3. Azure AD ilk katmanlı web API ikinci katmanlı web API'si erişim izni olduğundan ve bir JWT erişim belirteci döndürerek isteği doğrular ve bir JWT belirteci ilk katmanlı Web API'ye Yenile doğrular.
4. HTTPS üzerinden ilk katmanlı web API'si daha sonra ikinci katmanlı web API isteği yetkilendirme üstbilgisinde belirteç dizesini ekleyerek çağırır. İlk katmanlı web API, yenileme belirteçleri ve erişim belirteci geçerli olduğu sürece ikinci katmanlı web API'sini çağırmak devam edebilirsiniz.

#### <a name="code-samples"></a>Kod Örnekleri
Arka plan programı veya Web API senaryoları için sunucu uygulaması için kod örnekleri bakın. Ve daha sonra yeniden sık--her zaman yeni örnekler eklediğimiz denetleyin. [Sunucu veya Web API'si arka plan programı uygulamaya](active-directory-code-samples.md#server-or-daemon-application-to-web-api)

#### <a name="registering"></a>Kaydetme
* Tek bir kiracı: uygulama kimliği hem temsilci atanan kullanıcı kimliğini çalışmaları için arka plan programı veya sunucu uygulaması aynı dizinde Azure AD'de kayıtlı olması gerekir. Web API'si arka plan programı veya kendi kaynaklarına sunucunun erişimi sınırlamak için kullanılan izinler kümesi kullanıma sunmak için yapılandırılabilir. Temsilci atanan kullanıcı kimlik türü kullanılıyorsa, sunucu uygulaması, Azure Portalı'nda "İzinleri diğer uygulamaların" açılan menüsünden istediğiniz izinleri seçin gerekiyor. Uygulama kimlik türü kullanılıyorsa bu adım gerekli değildir.
* Çok Kiracılı: İlk olarak, arka plan programı veya sunucu uygulaması işlevsel olmasını gerektirir izinleri belirtmek için yapılandırılmış. Bir kullanıcının veya yöneticinin hedef dizinde, kuruluşları için kullanılabilir hale getirir uygulamaya izin verdiğinde bu gerekli izinlerin listesi iletişim kutusunda gösterilir. Bazı uygulamalar kuruluştaki herhangi bir kullanıcı onayı yalnızca kullanıcı düzeyinde izinler gerektirir. Diğer uygulamalar kuruluştaki bir kullanıcı için onay veremez yönetici düzeyi izinler gerektirir. Yalnızca bir dizin Yöneticisi bu izin düzeyini gerektiren uygulamalar için izin verebilirsiniz. Kullanıcı veya yönetici izin, web API'leri her ikisi de kendi dizininde kaydedilir.

#### <a name="token-expiration"></a>Belirteç süre sonu
İlk uygulama, bir JWT belirteci erişmek için kendi yetkilendirme kodu kullandığında, ayrıca, bir JWT yenileme belirteci alır. Erişim belirtecinin süresi dolduğunda, yenileme belirteci kullanıcı kimlik bilgileri olmadan yeniden kimlik doğrulaması için kullanılabilir. Bu yenileme belirteci sonra kullanıcının kimliğini doğrulamak için kullanılan bir yeni bir erişim belirteci ve yenileme belirteci sonuçlanır.

## <a name="see-also"></a>Ayrıca Bkz.
[Azure Active Directory Geliştirici Kılavuzu](active-directory-developers-guide.md)

[Azure Active Directory kod örnekleri](active-directory-code-samples.md)

[Azure AD'de anahtar geçişi imzalama hakkında önemli bilgiler](active-directory-signing-key-rollover.md)

[Azure AD'de OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx)

