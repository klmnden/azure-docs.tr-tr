---
title: Herhangi bir Azure AD kullanıcısıyla oturum bir uygulama oluşturma
description: Bir kullanıcı için herhangi bir Azure Active Directory kiracısında oturum çok kiracılı bir uygulama oluşturma işlemi gösterilmektedir.
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: 35af95cb-ced3-46ad-b01d-5d2f6fd064a3
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/22/2019
ms.author: ryanwi
ms.reviewer: jmprieur, lenalepa, sureshja
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2e6a5ecd704aabb4994337cb7b7df9e84677348d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66235290"
---
# <a name="how-to-sign-in-any-azure-active-directory-user-using-the-multi-tenant-application-pattern"></a>Nasıl yapılır: Çok kiracılı uygulama desenini kullanarak herhangi bir Azure Active Directory kullanıcısı ile oturum açın

Çoğu kuruluş için bir hizmet (SaaS) uygulaması olarak bir yazılım teklifi sunuyorsanız, uygulamanızı herhangi bir Azure Active Directory (Azure AD) kiracısı oturum açma işlemleri kabul edecek şekilde yapılandırabilirsiniz. Bu yapılandırma olarak adlandırılır *, uygulamanın çok kiracılı yapmadan*. Tüm Azure AD kiracısında kullanıcı hesaplarına uygulamanızda kullandığınız onaylanıyor sonra uygulamanızı oturum açmak mümkün olacaktır.

Kendi hesap sistemi sahip veya bu diğer diğer bulut sağlayıcılarına ait oturum açma türünü destekler var olan bir uygulamanız varsa, ekleyerek Azure AD oturum açma herhangi bir kiracıdaki basit bir işlemdir. Yalnızca uygulamanızı kaydetmeniz, OAuth2, Openıd Connect veya SAML ile oturum açma kod ekleyin ve put bir ["Sign in ile Microsoft" düğmesi] [ AAD-App-Branding] uygulamanızdaki.

> [!NOTE]
> Bu makalede, zaten Azure AD için tek kiracılı bir uygulama oluşturma ile ilgili bilgi sahibi olduğunuz kabul edilmektedir. Değilseniz, üzerinde hızlı başlangıçları biriyle başlayın [Geliştirici Kılavuzu giriş sayfası][AAD-Dev-Guide].

Uygulamanızı Azure AD çok kiracılı uygulamaya dönüştürmek için dört basit adım vardır:

1. [Çok kiracılı olacak şekilde, uygulama kaydı güncelleştir](#update-registration-to-be-multi-tenant)
2. [/ Common için istekleri göndermek için kodunuzu güncelleştirin uç noktası](#update-your-code-to-send-requests-to-common)
3. [Birden çok veren değerleri işlemek için kodunuzu güncelleştirin](#update-your-code-to-handle-multiple-issuer-values)
4. [Kullanıcı ve yönetici onayını anlama ve uygun kodu değişiklikleri](#understand-user-and-admin-consent)

Her adım ayrıntılı olarak bakalım. Doğrudan atlayabilirsiniz [çok kiracılı örnekleri listesi][AAD-Samples-MT].

## <a name="update-registration-to-be-multi-tenant"></a>Aktualizovat registraci. çok kiracılı olması

Varsayılan olarak, Azure AD web uygulaması/API'si kayıtları tek kiracılı olan. Kaydınızı bularak çok kiracılı yapabileceğiniz **desteklenen hesap türleri** açın **kimlik doğrulaması** uygulama kaydınızı bölmesinde [Azureportalı] [ AZURE-portal] ve ayarlamak **herhangi bir kuruluş dizini hesaplarında**.

Çok kiracılı bir uygulama yapılabilmesi için önce Azure AD uygulama kimliği URI'si uygulamanın genel olarak benzersiz olması gerekir. Uygulama Kimliği URI'si, uygulamanın protokol iletileri içinde tanımlanması için kullanılan yollardan biridir. Tek kiracılı bir uygulamada Uygulama Kimliği URI'sinin kiracı içinde benzersiz olması yeterlidir. Azure AD'nin uygulamayı tüm kiracılar arasında bulabilmesi için çok kiracılı uygulamada bu değerin genel olarak benzersiz olması gerekir. Genel olarak benzersiz olma gereksinimi, Uygulama Kimliği URI'sinin Azure AD kiracısının doğrulanmış etki alanı ile eşleşen bir ana bilgisayar adına sahip olması şartıyla sağlanır.

Varsayılan olarak, Azure Portalı aracılığıyla oluşturulan uygulamalar üzerinde uygulama oluşturma ayarlama genel olarak benzersiz bir uygulama kimliği URI'si sahiptir, ancak bu değeri değiştirebilirsiniz. Örneğin, kiracınızın adını contoso.onmicrosoft.com sonra geçerli ise uygulama kimliği URI'si olması `https://contoso.onmicrosoft.com/myapp`. Kiracınızda doğrulanmış bir etki alanının olsaydı `contoso.com`, geçerli bir uygulama kimliği URI'si de sonra `https://contoso.com/myapp`. Uygulama Kimliği URI'si bu düzene uygun olmadığında uygulamayı çok kiracılı hale getirme işlemi başarısız olur.

> [!NOTE]
> Yerel istemci kayıtları yanı [Microsoft kimlik platformu uygulamalarını](./active-directory-appmodel-v2-overview.md) varsayılan olarak çok kiracılıdır. Bu uygulama kayıtları çok kiracılı yapmak için herhangi bir eylemde bulunmanız gerekmez.

## <a name="update-your-code-to-send-requests-to-common"></a>/ Common için istekleri göndermek için kodunuzu güncelleştirin

Tek kiracılı bir uygulama, kiracının oturum açma uç noktası için oturum açma isteği gönderilir. Uç nokta için contoso.onmicrosoft.com gibi olacaktır: `https://login.microsoftonline.com/contoso.onmicrosoft.com`. Bir kiracının uç noktasına gönderilen istekleri, kiracıdaki uygulamalara kiracıdaki kullanıcılar (veya konuklar) kaydolabilirsiniz.

Çok kiracılı bir uygulama ile bir kiracının uç noktaya istek gönderilemiyor böylece uygulamanın, kullanıcı, kiracısının ne Önden bilmez. Bunun yerine, tüm Azure AD kiracılarında bağlantıları çoğaltır bir uç noktaya istek gönderilir: `https://login.microsoftonline.com/common`

Microsoft kimlik platformu / Common üzerinde bir istek aldığında uç noktası, kullanıcı oturum açtıktan ve, sonuç olarak, hangi Kiracı Kullanıcı geldiği bulur. / Azure AD tarafından desteklenen kimlik doğrulama protokolleri tüm ortak uç nokta çalışır:  Openıd Connect, OAuth 2.0, SAML 2.0 ve WS-Federation.

Uygulama oturum açma yanıtını kullanıcıyı temsil eden bir simge içerir. Belirteci veren değeri uygulamanın hangi Kiracı Kullanıcı dandır söyler. Ne zaman bir yanıt döndürür / Common uç nokta, belirteci veren değeri, kullanıcının kiracısı için karşılık gelir.

> [!IMPORTANT]
> / Ortak uç nokta bir kiracı değil ve bir veren değil, yalnızca çoğaltıcı olur. / Common kullanırken, mantık uygulamanızın belirteçleri doğrulamak için bu hesaba katması için güncelleştirilmesi gerekir.

## <a name="update-your-code-to-handle-multiple-issuer-values"></a>Birden çok veren değerleri işlemek için kodunuzu güncelleştirin

Web uygulamaları ve web API'leri almak ve Microsoft kimlik platformu belirteçleri doğrulayın.

> [!NOTE]
> Yerel istemci uygulama istemek ve Microsoft kimlik platformu belirteçleri alabilmesini olsa da bunlar burada doğrulanır API'leri için gönderilecek bunu yapın. Yerel uygulama belirteçleri doğrulama ve bunları donuk olarak ele almanız gerekir.

Bakalım Uygulama belirteçlerini doğrular nasıl adresindeki Microsoft kimlik platformu alır. Tek kiracılı bir uygulama gibi bir uç nokta değeri normalde alır:

    https://login.microsoftonline.com/contoso.onmicrosoft.com

ve gibi bir meta veri URL'si (Bu durumda, Openıd Connect) oluşturmak için kullanır:

    https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration

belirteçleri doğrulamak için kullanılan kritik iki bilgi parçasını indirmek için: Kiracı anahtarları ve veren değerini imzalama. Her Azure AD kiracısı formun benzersiz bir veren değerine sahiptir:

    https://sts.windows.net/31537af4-6d77-4bb9-a681-d2394888ea26/

burada GUID değeri Kiracı Kiracı Kimliğini yeniden adlandırma uyumlu sürümüdür. Önceki meta veri bağlantısını seçerseniz `contoso.onmicrosoft.com`, bu belgedeki veren değerini görebilirsiniz.

Tek kiracılı bir uygulama bir belirteci doğrular, imzalama anahtarları meta veri belgesi karşı belirtecinin imzası denetler. Bu test meta veri belgesinde bulunan bir belirteci veren değeri eşleştiğinden emin olmak için sağlar.

Çünkü / ortak uç nokta için bir kiracı karşılık gelmiyor ve bir veren, veren değerinden meta verilerini incelemek/genel, gerçek bir değer yerine şablonlu bir URL'ye sahip olduğunda desteklenmez:

    https://sts.windows.net/{tenantid}/

Bu nedenle, çok kiracılı bir uygulama belirteçler veren değerinden meta verileriyle eşleştirerek doğrulayamıyor `issuer` belirteç değeri. Çok kiracılı bir uygulama, hangi veren değerler geçerlidir ve veren değerini Kiracı kimliği bölümünü dayanmayan karar vermek için mantık gerekir. 

Çok kiracılı bir uygulama yalnızca kaydolan servisine belirli kiracısından oturum izin veriyorsa, örneğin, ardından veren değerini teslim almanız gerekir veya `tid` talep değeri belirtecinde söz konusu kiracıyı kendi aboneleri listesinde olduğundan emin olun. Çok kiracılı bir uygulama yalnızca kişiler ile ilgilenen ve kiracının bağlı herhangi bir erişim karar yapmaz, ardından, veren değerini tamamen yoksayabilirsiniz.

İçinde [çok kiracılı örnekleri][AAD-Samples-MT], veren doğrulama oturum açmak herhangi bir Azure AD kiracısı etkinleştirmek için devre dışıdır.

## <a name="understand-user-and-admin-consent"></a>Kullanıcı ve yönetici onayını anlama

Bir kullanıcı bir uygulamanın Azure AD'de oturum açmak, uygulama kullanıcının kiracısında gösterilmelidir. Bu, kuruluş uygulamaya kullanıcılar kendi kiracısında oturum açtığında benzersiz İlkelerimizi gibi işlemler yapmanıza olanak sağlar. Tek kiracılı bir uygulama için bu kayıt basittir; Uygulama kaydetme meydana gelen bir olup [Azure portalında][AZURE-portal].

Çok kiracılı bir uygulama için ilk kayıt için uygulama geliştiricisi tarafından kullanılan Azure AD kiracısında yaşar. Farklı bir kiracıda bir kullanıcı uygulamaya ilk kez oturum açtığında, Azure AD uygulama tarafından istenen izinleri onay ister. Bunlar onay sonra uygulamanın bir gösterimi adı verilen bir *hizmet sorumlusu* kullanıcının kiracısında oluşturulur ve oturum açma devam edebilir. Bir temsilci, dizinde kullanıcının izni uygulama kayıtları da oluşturulur. Uygulamanın uygulama ve ServicePrincipal nesneleri ve birbirleriyle nasıl ilişki kuracağını hakkında daha fazla bilgi için bkz: [uygulama nesneleri ve hizmet sorumlusu nesneleri][AAD-App-SP-Objects].

![Tek katmanlı uygulama için onayı][Consent-Single-Tier]

Bu onay deneyiminde, uygulama tarafından istenen izinleri tarafından etkilenir. Microsoft kimlik platformu iki izinler, yalnızca uygulama ve temsilci türünü destekler.

* Kullanıcı bir alt nesnelerin interneti için oturum açmış bir kullanıcı olarak davranma özelliği yapmak için bir uygulama temsilci atanmış izin verir. Örneğin, bir uygulama oturum açmış olan kullanıcının Takvim okumak için temsilci atanmış izin verebilirsiniz.
* Doğrudan uygulamanın kimliğini bir salt uygulama izni verilir. Örneğin, bir uygulamayı uygulamanın imzalı bağımsız olarak bir kiracıdaki kullanıcıların listesini okuma yalnızca uygulama izni verebilirsiniz.

Diğer bir kiracı yönetici onayı gerektirirken bazı için normal bir kullanıcı tarafından onaylanan izin. 

### <a name="admin-consent"></a>Yönetici onayı

Yalnızca uygulama izinleri, her zaman bir kiracı yönetici onayı gerektirir. Uygulamanızı bir salt uygulama izni isteklerini ve bir kullanıcı uygulamada oturum açmaya çalışırsa, kullanıcı onayı sağlayamadığı belirten bir hata iletisi görüntülenir.

Belirli bir temsilci izinleri de bir kiracı yönetici onayı gerektirir. Örneğin, oturum açmış kullanıcı olarak Azure AD'ye geri yazma özelliğini bir kiracı yönetici onayı gerektirir. Normal bir kullanıcı, yönetici onayı gerektiren temsilci atanmış izin isteyen bir uygulamada oturum çalışırsa, yalnızca uygulama izinleri gibi uygulamanızın bir hata alır. Yönetici onayı yayımlanan kaynak ve kaynak için belgelerinde bulunabilir geliştiricisi tarafından belirlenir olup bir izni gerektirir. İzinleri belgelerine [Azure AD Graph API'si] [ AAD-Graph-Perm-Scopes] ve [Microsoft Graph API] [ MSFT-Graph-permission-scopes] yönetici izinleri gerektiren belirtin onay vermiş olursunuz.

Uygulamanız yönetici onayı gerektiren izinler kullanıyorsa, yönetim eylemi nerede başlatabilirsiniz bir düğme veya bağlantı gibi hareket sahip olması. Bu eylem ayrıca içeren her zamanki OAuth2/Openıd Connect yetkilendirme isteği için uygulamanızın gönderdiği isteği `prompt=admin_consent` sorgu dizesi parametresi. Yönetici onay verdi ve müşteri kiracısında hizmet sorumlusu oluşturulduktan sonra sonraki oturum açma istekleri gerekmeyen `prompt=admin_consent` parametresi. Yönetici istenen izinleri kabul edilebilir olduğuna karar olduğundan, başka hiçbir kullanıcı kiracıda o noktadan ilerideki onayı istenir.

Kiracı Yöneticisi, uygulamalara izin vermesi normal kullanıcılar için devre dışı bırakabilirsiniz. Bu özellik devre dışı bırakılırsa, yönetici onayı her zaman kiracısında kullanılmak üzere uygulama için gerekli değildir. Devre dışı son kullanıcı onayı ile uygulamanızı test etmek istiyorsanız, yapılandırma anahtarı bulabilirsiniz [Azure portalında] [ AZURE-portal] içinde **[kullanıcı ayarları](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/UserSettings/menuId/)** bölümüne **kurumsal uygulamalar**.

`prompt=admin_consent` Parametresi yönetici onayı gerektirmeyen izinleri isteyen uygulamalar tarafından da kullanılabilir. Bu olduğunda kullanılacak bir örnek uygulama nerede Kiracı Yöneticisi "kaydolursa" bir deneyim gerekiyorsa bir saat ve başka kullanıcılar bu noktadan itibaren onayı istenir ' dir.

Bir uygulama, yönetici onayı gerektirir ve bir yönetici olmadan açarsa `prompt=admin_consent` parametresi yönetici başarılı bir şekilde, geçerli uygulamaya onay verdiğinde, gönderilen **yalnızca kendi kullanıcı hesabı için**. Normal kullanıcı hala oturum açın veya uygulama onayı mümkün olmayacaktır. Bu özellik, Kiracı Yöneticisi diğer kullanıcılar erişime izin vermeden önce uygulamanızın keşfedin vermek istiyorsanız yararlıdır.

> [!NOTE]
> Bazı uygulamalar, nerede normal kullanıcıların başlangıçta onay olanağına sahip olursunuz ve daha sonra uygulama yönetici onayı gerektiren yöneticinize başvurarak izinleri içerebilir bir deneyim istersiniz. Bugün Azure AD'de bir v1.0 uygulama kaydı bunu yapmanın bir yolu yoktur; Ancak, Microsoft kimlik Platformu (v2.0) uç noktası kullanarak izin istemek için uygulamalar yerine çalışma zamanında bu senaryoyu olanaklı kılar kayıt zamanında sağlar. Daha fazla bilgi için [Microsoft kimlik platformu uç nokta][AAD-V2-Dev-Guide].

### <a name="consent-and-multi-tier-applications"></a>Onay ve çok katmanlı uygulamalar

Uygulamanızı birden çok katmana sahip olabilir, her Azure AD'de kendi kaydı tarafından temsil. Örneğin, web API'si çağıran bir yerel uygulamayı veya web uygulamasına bir web API'sini çağırır. Her iki durumda, istemci (yerel veya web uygulaması) (web API'si) kaynak çağırmak için izin ister. İstemci bir müşterinin kiracısına tarafından onaylanan başarılı tüm kaynaklar için izinleri isteyen zaten müşteri kiracısında mevcut olmalıdır. Bu koşul karşılanmazsa, Azure AD kaynağı ilk eklenmelidir bir hata döndürür.

#### <a name="multiple-tiers-in-a-single-tenant"></a>Tek bir kiracı birden fazla katmanda

Mantıksal uygulamanızı iki veya daha fazla uygulama kayıtları, örneğin ayrı istemci ve kaynak içeriyorsa bu bir sorun olabilir. Nasıl kaynak müşteri kiracısında oturum ilk alıyorsunuz? Azure AD, bu durumda, tek bir adımda onaylı için istemci ve kaynak etkinleştirerek kapsar. Kullanıcı, hem istemci hem de onay sayfasında kaynak tarafından istenen izinleri toplamının görür. Bu davranışı etkinleştirmek için kaynağın uygulama kaydı, istemci uygulama kimliği olarak içermelidir bir `knownClientApplications` içinde kendi [uygulama bildirimini][AAD-App-Manifest]. Örneğin:

    knownClientApplications": ["94da0930-763f-45c7-8d26-04d5938baab2"]

Bu örnek web API'si çağırma çok katmanlı bir yerel istemci gösterilmiştir [ilişkili içerik](#related-content) bu makalenin sonunda bölüm. Aşağıdaki diyagramda, tek bir kiracıda kayıtlı çok katmanlı bir uygulama için onay genel bir bakış sağlar.

![Çok katmanlı bilinen istemci uygulamaları için onay][Consent-Multi-Tier-Known-Client]

#### <a name="multiple-tiers-in-multiple-tenants"></a>Birden fazla katmanda birden fazla Kiracı

Bir uygulamanın farklı katmanlar farklı kiracıda kayıtlı benzer bir durumda olur. Örneğin, Office 365 Exchange Online API'yi çağıran bir yerel istemci uygulaması oluşturmanın bir durum düşünün. Yerel uygulama ve daha yeni bir müşteri kiracısında çalıştırmak yerel uygulama geliştirmek için Exchange Online hizmet sorumlusu mevcut olması gerekir. Bu durumda, müşteri ve geliştirici Exchange Online için hizmet sorumlusu kiracıları içinde oluşturulacak satın almanız gerekir.

Microsoft dışındaki bir kuruluş tarafından oluşturulan bir API ise, geliştirici API'si müşterilerin kiracıların uygulamaya onay verme müşterileri için bir yol sağlamak üzere gerekir. Üçüncü taraf geliştirici kaydolma uygulamak için bir web istemcisi olarak çalışabilir, API oluşturmak önerilen tasarım içindir. Bunu yapmak için:

1. Çok kiracılı bir uygulama kaydı/kod gereksinimleri API'yi uygulayan emin olmak için önceki bölümlere izleyin.
2. API kapsamları/roller gösterme ek olarak, kayıt içerir emin olun "oturum açın ve kullanıcı profilini okuma" izni (varsayılan olarak sağlanır).
3. Bir oturum-içinde açma/kaydolma sayfasında web istemcisinde uygulayın ve takip [yönetici onayı](#admin-consent) Kılavuzu.
4. Kullanıcının uygulamaya onay sonra kendilerine ait kiracıda oluşturulan hizmet sorumlusu ve onay temsilci bağlantıları ve yerel uygulama için API belirteçleri elde edebilirsiniz.

Aşağıdaki diyagramda, farklı kiracıda kayıtlı çok katmanlı bir uygulama için onay genel bir bakış sağlar.

![Çok katmanlı bir uygulama çok taraflı için onay][Consent-Multi-Tier-Multi-Party]

### <a name="revoking-consent"></a>Onayı iptal etme

Kullanıcıların ve yöneticilerin onay uygulamanıza dilediğiniz zaman iptal edebilirsiniz:

* Kullanıcılar, bunları kaldırarak tek tek uygulamalar için erişimi iptal et kendi [erişim paneli uygulamaları] [ AAD-Access-Panel] listesi.
* Yöneticiler, bunları kaldırarak uygulamalara erişimi iptal et kullanarak [kurumsal uygulamalar](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps) bölümünü [Azure portalında][AZURE-portal].

Yönetici uygulamaya bir kiracıdaki tüm kullanıcılar için bulursa, kullanıcılar tek tek erişimi iptal edemez. Yalnızca yönetici erişimi iptal edebilir ve yalnızca tüm uygulama için.

## <a name="multi-tenant-applications-and-caching-access-tokens"></a>Çok kiracılı uygulamalar ve erişim belirteçlerini önbelleğe alma

Çok kiracılı uygulamaları Azure AD ile korunan API'leri çağırmaya yönelik erişim belirteçleri de alabilir. Active Directory Authentication Library (ADAL) ile çok kiracılı bir uygulama başlangıçta/Common, kullanarak bir kullanıcı için bir belirteç isteği kullanırken sık karşılaşılan hatalardan yanıt, sonra da/Common kullanarak, aynı kullanıcı için bir sonraki belirteç isteği. Yanıt Azure AD'den bir kiracıdan gelen olmayan/ortak, ADAL önbelleğe olarak kiracısından belirteç olduğundan. Kullanıcı için erişim belirteci almak için/Common sonraki çağrı isabetsiz okumaları önbellek girişi ve kullanıcı yeniden oturum açmanız istenir. Önbellek gözden kaçırmamak için kiracının uç noktasına yapılan sonraki çağrılar zaten oturum açmış bir kullanıcı için emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir kullanıcı için herhangi bir Azure AD kiracısında oturum bir uygulamanın nasıl oluşturulacağını öğrendiniz. Çoklu oturum açma (SSO), uygulama ile Azure AD arasında etkinleştirdikten sonra Office 365 gibi Microsoft kaynakları tarafından kullanıma sunulan API'lerine erişmek için uygulamanızı da güncelleştirebilirsiniz. Bu, kullanıcılara kendi profil resminizi ya da bunların sonraki takvim randevusunu gibi bağlamsal bilgiler gösteren, uygulamanızda kişiselleştirilmiş bir deneyim sunmak sağlar. API yapma hakkında daha fazla bilgi için Azure AD'ye çağırır ve Office 365, Exchange, SharePoint, OneDrive, OneNote ve diğer ziyaret gibi hizmetleri [Microsoft Graph API][MSFT-Graph-overview].

## <a name="related-content"></a>İlgili içerik

* [Çok kiracılı uygulama örnekleri][AAD-Samples-MT]
* [Uygulamalar için markalama][AAD-App-Branding]
* [Uygulama nesneleri ve hizmet sorumlusu nesneleri][AAD-App-SP-Objects]
* [Uygulamaları Azure Active Directory ile tümleştirme][AAD-Integrating-Apps]
* [Onay çerçevesine genel bakış][AAD-Consent-Overview]
* [Microsoft Graph API'si izin kapsamları][MSFT-Graph-permission-scopes]
* [Azure AD Graph API'si izin kapsamları][AAD-Graph-Perm-Scopes]

<!--Reference style links IN USE -->
[AAD-Access-Panel]:  https://myapps.microsoft.com
[AAD-App-Branding]:howto-add-branding-in-azure-ad-apps.md
[AAD-App-Manifest]:reference-azure-ad-app-manifest.md
[AAD-App-SP-Objects]:app-objects-and-service-principals.md
[AAD-Auth-Scenarios]:authentication-scenarios.md
[AAD-Consent-Overview]:consent-framework.md
[AAD-Dev-Guide]:azure-ad-developers-guide.md
[AAD-Graph-Overview]: https://azure.microsoft.com/documentation/articles/active-directory-graph-api/
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Integrating-Apps]:quickstart-v1-integrate-apps-with-azure-ad.md
[AAD-Samples-MT]: https://azure.microsoft.com/documentation/samples/?service=active-directory&term=multitenant
[AAD-Why-To-Integrate]: ./active-directory-how-to-integrate.md
[AZURE-portal]: https://portal.azure.com
[MSFT-Graph-overview]: https://developer.microsoft.com/graph/docs/overview/overview
[MSFT-Graph-permission-scopes]: https://developer.microsoft.com/graph/docs/concepts/permissions_reference

<!--Image references-->
[AAD-Sign-In]: ./media/active-directory-devhowto-multi-tenant-overview/sign-in-with-microsoft-light.png
[Consent-Single-Tier]: ./media/howto-convert-app-to-be-multi-tenant/consent-flow-single-tier.svg
[Consent-Multi-Tier-Known-Client]: ./media/howto-convert-app-to-be-multi-tenant/consent-flow-multi-tier-known-clients.svg
[Consent-Multi-Tier-Multi-Party]: ./media/howto-convert-app-to-be-multi-tenant/consent-flow-multi-tier-multi-party.svg

<!--Reference style links -->
[AAD-App-Manifest]:reference-azure-ad-app-manifest.md
[AAD-App-SP-Objects]:app-objects-and-service-principals.md
[AAD-Auth-Scenarios]:authentication-scenarios.md
[AAD-Integrating-Apps]:quickstart-v1-integrate-apps-with-azure-ad.md
[AAD-Dev-Guide]:azure-ad-developers-guide.md
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AAD-Graph-User-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity
[AAD-How-To-Integrate]: ./active-directory-how-to-integrate.md
[AAD-Security-Token-Claims]: ./active-directory-authentication-scenarios/#claims-in-azure-ad-security-tokens
[AAD-Tokens-Claims]:access-tokens.md
[AAD-V2-Dev-Guide]: v2-overview.md
[AZURE-portal]: https://portal.azure.com
[Duyshant-Role-Blog]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
[JWT]: https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32
[O365-Perm-Ref]: https://msdn.microsoft.com/office/office365/howto/application-manifest
[OAuth2-Access-Token-Scopes]: https://tools.ietf.org/html/rfc6749#section-3.3
[OAuth2-AuthZ-Code-Grant-Flow]: https://msdn.microsoft.com/library/azure/dn645542.aspx
[OAuth2-AuthZ-Grant-Types]: https://tools.ietf.org/html/rfc6749#section-1.3 
[OAuth2-Client-Types]: https://tools.ietf.org/html/rfc6749#section-2.1
[OAuth2-Role-Def]: https://tools.ietf.org/html/rfc6749#page-6
[OpenIDConnect]: https://openid.net/specs/openid-connect-core-1_0.html
[OpenIDConnect-ID-Token]: https://openid.net/specs/openid-connect-core-1_0.html#IDToken
