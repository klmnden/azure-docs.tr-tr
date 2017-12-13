---
title: "Herhangi bir Azure AD kullanıcı oturum bir uygulama oluşturma"
description: "Adım adım bir uygulama oluşturmaya yönelik yönergeler olarak da bilinen bir çok kiracılı uygulama tüm Azure Active Directory kiracısındaki bir kullanıcı olarak oturum açabilir."
services: active-directory
documentationcenter: 
author: bryanla
manager: mtillman
editor: 
ms.assetid: 35af95cb-ced3-46ad-b01d-5d2f6fd064a3
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/26/2017
ms.author: bryanla
ms.custom: aaddev
ms.openlocfilehash: f6d8d2c07c2860059c4e9deb75d0bc4a876e057b
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-sign-in-any-azure-active-directory-ad-user-using-the-multi-tenant-application-pattern"></a>Çok kiracılı uygulama desenini kullanarak Azure Active Directory (AD) içinde herhangi bir kullanıcının imzalama
Çoğu kuruluş için bir hizmet uygulaması olarak bir yazılım teklifi sunuyorsanız, uygulamanızın oturum açmalardan tüm Azure AD kiracısı kabul edecek şekilde yapılandırabilirsiniz.  Azure AD içinde bu yapılandırma, uygulama çok kiracılı özelleştirme adı verilir.  Tüm Azure AD Kiracı kullanıcılar hesaplarına uygulamanızla birlikte kullanmak için onaylıyorsunuz sonra uygulamanıza oturum açabilir.  

Kendi hesabı sistem veya diğer oturum açma diğer bulut sağlayıcılardan türünü destekler, var olan bir uygulamanız varsa, ekleme Azure AD oturum açma dikey basit bir işlemdir. Yalnızca uygulamanızı kaydetme, oturum açma kodu OAuth2, Openıd Connect veya SAML aracılığıyla ekleyin ve "Sign In ile Microsoft" düğmesi, uygulamanızda yerleştirin. Uygulamanızı markalama hakkında daha fazla bilgi için aşağıdaki düğmeye tıklayın.

[![Düğmesini oturum][AAD-Sign-In]][AAD-App-Branding]

Bu makale, zaten Azure AD için tek bir kiracı uygulama oluşturmaya tanıdık varsayar.  Siz değilseniz, head kadar geri [Geliştirici Kılavuzu giriş sayfası] [ AAD-Dev-Guide] ve bizim hızlı başlangıçlar birini deneyin!

Uygulamanızı Azure AD çoklu kiracı uygulamaya dönüştürmek için dört basit adım vardır:

1. Çoklu kiracı uygulama kaydınızı güncelleştir
2. / Common için istekleri göndermek için kodunuzu güncelleştirin uç noktası 
3. Birden çok veren değerleri işlemek için kodunuzu güncelleştirin
4. Kullanıcı ve yönetici onayı anlamak ve uygun kodu değişiklikleri yapın

Her adım ayrıntılı bakalım. Doğrudan atlayabilirsiniz [bu liste çok kiracılı örnekleri][AAD-Samples-MT].

## <a name="update-registration-to-be-multi-tenant"></a>Çoklu kiracı güncelleştirme kaydı
Varsayılan olarak, tek bir kiracı web app/API kayıtlar Azure AD'de etkilenir.  Kaydınızı, uygulama kaydı Özellikler sayfasında "Çok kiralanan" anahtar bularak çok kiracılı yapabileceğiniz [Azure portal] [ AZURE-portal] ve "Evet" ayarı

Ayrıca unutmayın, çok kiracılı bir uygulama yapılabilmesi için önce Azure AD genel olarak benzersiz olması için uygulamanın uygulama kimliği URI'si gerektirir. Uygulama Kimliği URI'si uygulama protokolü iletilerinde tanımlanan yollardan biridir.  Tek kiracılı uygulama için Kiracı içinde benzersiz olması uygulama kimliği URI'si için yeterli olur.  Azure AD uygulama tüm kiracılar bulabilmek için çok kiracılı uygulama için genel olarak benzersiz olmalıdır.  Genel benzersizlik Azure AD kiracısı doğrulanmış bir etki alanı ile eşleşen bir ana bilgisayar adı uygulama kimliği URI'si gerektirerek zorlanır.  Örneğin, contoso.onmicrosoft.com sonra geçerli bir kiracı adı ise, uygulama kimliği URI'si olması `https://contoso.onmicrosoft.com/myapp`.  Kiracı doğrulanmış bir etki alanı varsa `contoso.com`, geçerli bir uygulama kimliği URI'sini de sonra `https://contoso.com/myapp`.  Çok kiracılı başarısız olarak bir uygulama ayarı, uygulama kimliği URI'si bu deseni izlemenizi değil

Yerel istemci kayıtları varsayılan olarak çok kiracılı.  Yerel yapmak için herhangi bir eylemde bulunmanız gerekmez istemci uygulama kayıt çok kiracılı.

## <a name="update-your-code-to-send-requests-to-common"></a>/ Common için istekleri göndermek için kodunuzu güncelleştirin
Tek bir kiracı uygulamada oturum açma istekleri kiracının oturum açma uç noktasına gönderilir. Örneğin, contoso.onmicrosoft.com için uç nokta olacaktır:

    https://login.microsoftonline.com/contoso.onmicrosoft.com

Bir kiracının uç noktasına gönderilen istekleri, Kiracı uygulamalara Kiracı, kullanıcılar (veya konuklar) kaydolabilirsiniz.  Çok kiracılı uygulama ile uygulamanın kullanıcı arasındadır hangi Kiracı Önden bilmiyor kiracının uç noktasına istek gönderemez.  Bunun yerine, tüm Azure AD kiracılar arasında multiplexes bir uç nokta için istekleri gönderilir:

    https://login.microsoftonline.com/common

Ne zaman Azure AD alır / Common ağda bir istek uç noktası, kullanıcı oturum açtığında ve sonuç olarak, Kiracı Kullanıcı olduğunu bulur.  / Ortak uç nokta çalışır tüm Azure AD tarafından desteklenen kimlik doğrulama protokolleri: Openıd Connect, OAuth 2.0, SAML 2.0 ve WS-Federasyon.

Uygulama oturum açma yanıtı kullanıcıyı temsil eden bir belirteci içeriyor.  Belirteç veren değerinde bir uygulama kullanıcının arasındadır hangi Kiracı söyler.  Bir yanıt / Common döndüğünde uç noktası, Belirteç Verenin değerinde kullanıcının Kiracı karşılık gelir.  / Common dikkate almak önemlidir uç noktası bir kiracı değil ve bir veren değil, yalnızca çoğaltıcı değildir.  / Common kullanırken belirteçleri doğrulamak için uygulamanızın mantık bu dikkate güncelleştirilmesi gerekiyor. 

Daha önce belirtildiği gibi çok kiracılı uygulamalara yönergeleri markalama Azure AD uygulaması aşağıdaki kullanıcılar için tutarlı bir oturum açma deneyimi de sağlamalıdır. Uygulamanızı markalama hakkında daha fazla bilgi için aşağıdaki düğmeye tıklayın.

[![Düğmesini oturum][AAD-Sign-In]][AAD-App-Branding]

/ Common kullanımını bir göz atalım uç noktası ve kod uygulamanızda daha ayrıntılı bilgi.

## <a name="update-your-code-to-handle-multiple-issuer-values"></a>Birden çok veren değerleri işlemek için kodunuzu güncelleştirin
Web uygulamaları ve web API'leri alır ve Azure AD'den belirteçleri doğrulamak.  

> [!NOTE]
> Yerel istemci uygulamaları istemek ve Azure AD'den belirteçleri almak olsa da, bunlar burada doğrulandığı API'leri için göndermek için bunu.  Yerel uygulama belirteçleri doğrulamaz ve bunları donuk olarak ele almanız gerekir.
> 
> 

Bakalım Uygulama belirteçleri nasıl doğrular Azure AD'den alır.  Tek kiracılı uygulama normalde bir uç nokta değeri gibi alır:

    https://login.microsoftonline.com/contoso.onmicrosoft.com

ve gibi meta veri URL'sini (Bu durumda, Openıd Connect) oluşturmak için kullanın:

    https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration

belirteçleri doğrulamak için kullanılan iki kritik bilgi parçalarını yüklemek için: Kiracı anahtarları ve veren değeriyle imzalama.  Her Azure AD kiracısı formun benzersiz veren değerine sahip:

    https://sts.windows.net/31537af4-6d77-4bb9-a681-d2394888ea26/

burada GUID değeri Kiracı Kiracı kimliği yeniden adlandırma uyumlu sürümüdür.  Önceki meta veri bağlantısını tıklatırsanız `contoso.onmicrosoft.com`, bu belgeyi verenin değerinde görebilirsiniz.

Tek kiracılı uygulama bir belirteci doğrular, meta veri belgesi İmzalama anahtarları karşı belirtecinin imzası denetler. Bu test meta veri belgesinde bulunamadı bir belirteci veren değeriyle eşleştiğinden emin olmak için sağlar.

/ Common itibaren uç noktası için bir kiracı karşılık gelmiyor ve bir veren için meta verilerde veren değerini inceleyin / genel, gerçek değer yerine şablonlu bir URL'ye sahip değil:

    https://sts.windows.net/{tenantid}/

Bu nedenle, çok kiracılı uygulama belirteçleri meta verileriyle veren değerinde eşleştirerek doğrulayamıyor `issuer` belirtecin değeri.  Çok kiracılı uygulama mantığını hangi veren değerler geçerlidir ve hangi olan karar değil gerekir, Kiracı'veren değeriyle kimliği bölümünü göre.  

Çok kiracılı uygulama yalnızca kendi hizmet için kaydolup, belirli kiracıdan oturum açma izin veriyorsa, örneğin, sonra veren değeri kullanıma gerekir veya `tid` talep değeri belirteçte o Kiracı kendi aboneleri listesinde olduğundan emin olun.  Çok kiracılı uygulama yalnızca kişiler ile ilgilenir ve kiracılar dayanarak hiçbir erişim kararları değil, ardından veren değeriyle tamamen yok sayabilirsiniz.

Çok kiracılı örneklerinde içinde [ilgili içerik](#related-content) veren doğrulama bu makalenin sonunda bölüm oturum açmak tüm Azure AD kiracısı etkinleştirmek için devre dışıdır.

Artık çok kiracılı uygulamalara imzalama kullanıcılar için kullanıcı deneyimi bakalım.

## <a name="understanding-user-and-admin-consent"></a>Anlama kullanıcı ve yönetici onayı
Bir kullanıcı bir uygulamaya Azure AD'de oturum açmak kullanıcının kiracısında uygulama gösterilmelidir.  Bu uygulama için kendi Kiracı kullanıcılar oturum açtığında benzersiz ilkeleri uygulamak gibi şeyler için kuruluş sağlar.  Tek kiracılı uygulama için bu kayıt kolaydır; uygulamada kaydederken oluşan bir olan [Azure portal][AZURE-portal].

Çok kiracılı uygulama için uygulama ilk kaydı geliştirici tarafından kullanılan Azure AD kiracısında yaşar.  Farklı bir kiracısındaki bir kullanıcı uygulama ilk kez oturum açtığında, Azure AD uygulama tarafından istenen izinler için izin ister.  Bunlar onayı sonra uygulama gösterimini adlı bir *hizmet sorumlusu* kullanıcının Kiracı içinde oluşturulur ve oturum açma işlemi devam edebilir. Bir temsilci ayrıca kullanıcının izni uygulamaya kayıtları dizinde oluşturulur. Bkz: [uygulama ve hizmet sorumlusu nesneleri] [ AAD-App-SP-Objects] uygulamanın uygulama ve ServicePrincipal nesneleri ve birbirleriyle nasıl ilişkili olduğunu hakkında ayrıntılı bilgi için.

![Tek katmanlı uygulama onayı][Consent-Single-Tier] 

Bu onayı deneyimi uygulama tarafından istendiğinde izinleri etkilenir.  Azure AD, yalnızca uygulama ve temsilci izinleri iki tür destekler:

* Bir alt nesnelerin interneti için oturum açmış olan kullanıcının kullanıcı görevi görür olanağı yapmak için bir uygulama bir temsilci izni verir.  Örneğin, bir uygulama temsilcisi oturum açmış olan kullanıcının Takvim okuma izni verebilirsiniz.
* Yalnızca uygulama izni doğrudan uygulama kimliği verilir.  Örneğin, bir uygulama kullanıcıların uygulamaya imzalanmış bakılmaksızın bir kiracıdaki listesini okuma yalnızca uygulama izni verebilirsiniz.

Diğer bir kiracı yöneticisinin iznini gerektirirken bazı izinler için normal bir kullanıcı tarafından rıza. 

### <a name="admin-consent"></a>Yönetici onayı
Yalnızca uygulama izinleri her zaman bir kiracı yönetici izni gerektirir.  Uygulamanızı bir yalnızca uygulama izni ister ve uygulamada oturum açmak bir kullanıcı çalışırsa, kullanıcı onayı mümkün değil bildiren bir hata iletisi görüntülenir.

Belirli izinlere temsilci, ayrıca bir kiracı yönetici izni gerektirir.  Örneğin, oturum açmış olan kullanıcının olarak Azure AD ile geri yazma özelliğini bir kiracı yönetici izni gerektirir.  Yalnızca uygulama izinleri gibi sıradan bir kullanıcı yönetici izni gerektiren bir temsilci izni isteklerini bir uygulamaya oturum açmak çalışırsa, uygulamanızın bir hata alır.  İzin gerektirip gerektirmediğini yönetici onayı kaynak yayımlanan geliştirici tarafından belirlenir ve kaynak belgelerinde bulunabilir.  Azure AD grafik API'si ve Microsoft Graph API için izinleri açıklayan konulara bağlantılar [ilgili içerik](#related-content) bu makalenin.

Uygulamanızı yönetici izni gerektiren izinleri kullanıyorsa, yönetici eylemi burada başlatabilirsiniz düğmesini veya bağlantısını gibi hareket olması gerekir.  Uygulamanızı gönderir normal bir OAuth2/Openıd Connect yetkilendirme isteği bu eylemi değildir ancak de içeren istek `prompt=admin_consent` sorgu dizesi parametresi.  Yönetici seçtiği ve hizmet sorumlusu müşterinin Kiracı oluşturulduktan sonra sonraki oturum açma istekleri gerekmeyen `prompt=admin_consent` parametresi. Yönetici istenen izinleri kabul edilebilir olduğuna karar olduğundan, hiçbir diğer kullanıcılar Kiracı o noktadan izin istenir.

`prompt=admin_consent` Parametresi da yönetici onayı gerektirmeyen izinleri gerektiren uygulamalar tarafından kullanılabilir. Applicator nerede Kiracı Yönetici "kaydolduğunda" bir zaman ve başka kullanıcılara izin o noktadan itibaren üzerinde sorulur bir deneyim gerektirdiğinde bu yapılır.

Bir uygulama yönetici izni gerektirir ve bir yönetici oturum açtığında, ancak `prompt=admin_consent` parametresi gönderilmez, yönetici başarıyla uygulamaya izin **yalnızca kullanıcı hesapları için**.  Normal kullanıcıların olan hala değil oturum açın ve uygulamaya onayı.  Bu özellik, Kiracı yönetici diğer kullanıcılar erişime izin vermeden önce uygulamanızın keşfedin vermek istiyorsanız yararlıdır.

Kiracı Yöneticisi uygulamalara onayı normal kullanıcılara devre dışı bırakabilirsiniz.  Bu özelliği devre dışıysa, yönetici onayı her zaman uygulamanın kiracısında kurulması gereklidir.  Devre dışı normal kullanıcı izin ile uygulamanızı test etmek istiyorsanız, yapılandırma bölümünü yapılandırma anahtarı Azure AD kiracısı'nda bulabilirsiniz [Azure portal][AZURE-portal].

> [!NOTE]
> Bazı uygulamalar, nerede normal kullanıcıların başlangıçta onayı sağlayabilir ve daha sonra uygulama yönetici izni gerektiren yönetici ve istek izinleri içerebileceği bir deneyim istiyor.  Bugün Azure AD içinde tek bir uygulama kaydı ile bunun için hiçbir yolu yoktur.  Yaklaşan Azure AD Resource Manager dağıtım modeli uç uygulamalarının çalışma zamanında izinleri istemek için bu senaryo sağlayan kayıt zaman yerine sağlar.  Daha fazla bilgi için bkz: [Azure AD uygulama modeli Resource Manager dağıtım modeli Geliştirici Kılavuzu][AAD-V2-Dev-Guide].
> 
> 

### <a name="consent-and-multi-tier-applications"></a>İzin ve çok katmanlı uygulamalar
Uygulamanız birden çok katman olabilir, her kendi kayıt tarafından Azure AD'de temsil.  Örneğin, bir web API çağrılarının yerel bir uygulama veya bir web uygulaması, web API çağırır.  Her iki durumda, istemci (yerel uygulama veya web uygulaması) (web API) kaynak çağrısına izin ister.  İstemcinin bir müşterinin Kiracı başarıyla rıza için tüm kaynaklar için izinleri isteyen Müşteri'nin kiracısında önceden var olmalıdır.  Bu koşul değil, Azure AD kaynağı ilk eklenmelidir bir hata döndürür.

**Tek bir kiracı içinde birden çok katmanları**

Mantıksal uygulamanızı iki veya daha fazla uygulama kayıtları, örneğin ayrı istemci ve kaynak oluşuyorsa bu bir sorun olabilir.  Nasıl kaynak müşteri Kiracı ilk alıyorsunuz?  Azure AD bu durumda, istemci ve kaynak tek bir adımda rıza etkinleştirerek ele alınmaktadır. Kullanıcı, hem istemci hem de kaynak onay sayfasında tarafından talep edilen izinler Toplamı toplam görür.  Bu davranışı etkinleştirmek için kaynağın uygulama kaydı istemcinin uygulama kimliği olarak içermelidir bir `knownClientApplications` kendi uygulama bildiriminde.  Örneğin:

    knownClientApplications": ["94da0930-763f-45c7-8d26-04d5938baab2"]

Bu özellik aracılığıyla kaynağa güncelleştirilebilir [uygulama bildirimi][AAD-App-Manifest]. Bu web API'si örneğinin arayan bir çok katmanlı Yerel İstemcisi'nde gösterilen [ilgili içerik](#related-content) bu makalenin sonunda bölüm. Aşağıdaki diyagramda, tek bir kiracı kayıtlı çok katmanlı bir uygulama için izin genel bir bakış sağlar:

![Çok katmanlı bilinen istemci uygulamaları için onayı][Consent-Multi-Tier-Known-Client] 

**Birden çok Kiracı içinde birden çok katmanları**

Bir uygulamanın farklı katmanlara farklı kiracılar kayıtlı benzer bir durumda olur.  Örneğin, Office 365 Exchange Online API çağrılarının yerel istemci uygulaması oluşturmanın bir durum düşünün.  Uygulama ve daha sonraki bir müşterinin Kiracı içinde çalıştırmak yerel uygulama için yerel geliştirmek için Exchange Online hizmet sorumlusu mevcut olması gerekir.  Bu durumda, müşteri ve geliştirici Exchange Online kiracıları içinde oluşturulacak asıl hizmeti satın almanız gerekir.  

Microsoft dışındaki bir kuruluş tarafından oluşturulan bir API söz konusu olduğunda, müşterilerin kiracılar uygulamasına kabul müşterileri için bir yol sağlamak üzere API Geliştirici gerekir. Kaydolma uygulamak için bir web istemcisi olarak çalışabilir, API oluşturmak 3 taraf geliştirici için önerilen tasarımdır bakın:

1. Çok kiracılı uygulama kaydı/kodu gereksinimleri API'yi uygulayan emin olmak için önceki bölümlerde izleyin
2. API kapsamları/rolleri gösterme ek olarak, kayıt içerdiğinden emin olun "oturum açmak ve kullanıcı profilini okuma" (varsayılan olarak sağlanır) Azure AD izni
3. Aşağıdaki web istemcisinde bir oturum-açma/kaydolma sayfası uygulamak [yönetici onayı](#admin-consent) Kılavuzu daha önce ele alınan 
4. Kullanıcı uygulamaya izin sonra hizmet asıl ve onay temsilci bağlantılar kendi Kiracı içinde oluşturulur ve yerel uygulama için API belirteç alabilir

Aşağıdaki diyagramda, farklı kiracılar kayıtlı çok katmanlı bir uygulama için izin genel bir bakış sağlar:

![Çok katmanlı çok kişili uygulama onayı][Consent-Multi-Tier-Multi-Party] 

### <a name="revoking-consent"></a>İzni iptal etme
Kullanıcıların ve yöneticilerin izin uygulamanıza herhangi bir zamanda iptal edebilirsiniz:

* Kullanıcıların erişimi iptal et tek tek uygulamalar için bunları kaldırarak kendi [erişim paneli uygulamaları] [ AAD-Access-Panel] listesi.
* Yöneticiler, Azure AD Yönetimi bölümünü kullanarak Azure AD'den kaldırarak uygulamalara erişimi iptal etme [Azure portal][AZURE-portal].

Bir uygulamaya bir kiracıdaki tüm kullanıcılar için bir yönetici izin verirse, kullanıcılar tek tek erişim iptal edilemez.  Yönetici erişimi iptal edebilirsiniz yalnızca ve yalnızca tüm uygulama için.

### <a name="consent-and-protocol-support"></a>İzin ve protokol desteği
Onay, Openıd Connect, OAuth aracılığıyla Azure AD'de desteklenir WS-Federation ve SAML protokoller.  SAML ve WS-Federasyon protokollerini desteklemeyen `prompt=admin_consent` yönetici izni yalnızca OAuth ve Openıd Connect aracılığıyla mümkün olması için parametre.

## <a name="multi-tenant-applications-and-caching-access-tokens"></a>Çok Kiracılı uygulamalara ve erişim belirteçleri önbelleğe alma
Çok kiracılı uygulamalara da Azure AD tarafından korunan API'leri çağırmak için erişim belirteçleri elde edebilirsiniz.  Active Directory Authentication Library (ADAL) çok kiracılı uygulama ile kullanırken yaygın görülen bir hata başlangıçta/Common kullanarak bir kullanıcı için bir belirteç isteği, bir yanıt almak için sonra da/Common kullanarak, aynı kullanıcı için bir sonraki belirteç isteği.  Azure AD yanıttan bir kiracıdan gelen değil/yaygın ADAL olarak kiracısından belirteç önbelleğe beri. Kullanıcı için erişim belirteci almak için/Common sonraki çağrı önbellek girişi isabetsiz okuma ve kullanıcı yeniden oturum açmanız istenir.  Önbellek eksik önlemek için zaten oturum açmış olan kullanıcı için sonraki çağrılar kiracının uç noktasına yapıldığından emin olun.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Azure Active Directory kiracısındaki bir kullanıcı olarak oturum bir uygulama oluşturmak nasıl öğrendiniz. Çoklu oturum açma uygulama ve Azure Active Directory arasında etkinleştirdikten sonra Office 365 gibi Microsoft kaynakları tarafından sunulan API'lere erişim uygulamanıza da güncelleştirebilirsiniz. Bu nedenle örneğin kullanıcılara kendi profil resmi veya kendi sonraki takvim randevu gibi bağlamsal bilgi gösteren kişiselleştirilmiş bir deneyim, uygulamanızda sunabilir. Exchange, SharePoint, OneDrive, OneNote, Planlayıcısı, Excel ve daha fazlası gibi Azure Active Directory ve Office 365 hizmetlerine API çağrıları yapma hakkında daha fazla bilgi edinmek için şurayı ziyaret edin: [Microsoft Graph API][MSFT-Graph-overview].


## <a name="related-content"></a>İlgili içerik
* [Çok kiracılı uygulama örnekleri][AAD-Samples-MT]
* [Uygulamalar için markalama talimatları][AAD-App-Branding]
* [Azure AD Geliştirici Kılavuzu][AAD-Dev-Guide]
* [Uygulama nesneleri ve hizmet sorumlusu nesneleri][AAD-App-SP-Objects]
* [Uygulamaları Azure Active Directory ile tümleştirme][AAD-Integrating-Apps]
* [Onay Framework'e Genel Bakış][AAD-Consent-Overview]
* [Microsoft Graph API'si izin kapsamları][MSFT-Graph-permision-scopes]
* [Azure AD grafik API'si izin kapsamları][AAD-Graph-Perm-Scopes]

Geri bildirim sağlamak ve iyileştirmek ve içeriği şekil yardımcı olmak için aşağıdaki açıklamaları bölümü kullanın.

<!--Reference style links IN USE -->
[AAD-Access-Panel]:  https://myapps.microsoft.com
[AAD-App-Branding]: ./active-directory-branding-guidelines.md
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Consent-Overview]: ./active-directory-integrating-applications.md#overview-of-the-consent-framework
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Overview]: https://azure.microsoft.com/en-us/documentation/articles/active-directory-graph-api/
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Samples-MT]: https://azure.microsoft.com/documentation/samples/?service=active-directory&term=multitenant
[AAD-Why-To-Integrate]: ./active-directory-how-to-integrate.md
[AZURE-portal]: https://portal.azure.com
[MSFT-Graph-overview]: https://graph.microsoft.io/en-us/docs/overview/overview
[MSFT-Graph-permision-scopes]: https://graph.microsoft.io/en-us/docs/authorization/permission_scopes

<!--Image references-->
[AAD-Sign-In]: ./media/active-directory-devhowto-multi-tenant-overview/sign-in-with-microsoft-light.png
[Consent-Single-Tier]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-single-tier.png
[Consent-Multi-Tier-Known-Client]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-multi-tier-known-clients.png
[Consent-Multi-Tier-Multi-Party]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-multi-tier-multi-party.png

<!--Reference style links -->
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AAD-Graph-User-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity
[AAD-How-To-Integrate]: ./active-directory-how-to-integrate.md
[AAD-Security-Token-Claims]: ./active-directory-authentication-scenarios/#claims-in-azure-ad-security-tokens
[AAD-Tokens-Claims]: ./active-directory-token-and-claims.md
[AAD-V2-Dev-Guide]: ../active-directory-appmodel-v2-overview.md
[AZURE-portal]: https://portal.azure.com
[Duyshant-Role-Blog]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
[JWT]: https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32
[O365-Perm-Ref]: https://msdn.microsoft.com/en-us/office/office365/howto/application-manifest
[OAuth2-Access-Token-Scopes]: https://tools.ietf.org/html/rfc6749#section-3.3
[OAuth2-AuthZ-Code-Grant-Flow]: https://msdn.microsoft.com/library/azure/dn645542.aspx
[OAuth2-AuthZ-Grant-Types]: https://tools.ietf.org/html/rfc6749#section-1.3 
[OAuth2-Client-Types]: https://tools.ietf.org/html/rfc6749#section-2.1
[OAuth2-Role-Def]: https://tools.ietf.org/html/rfc6749#page-6
[OpenIDConnect]: http://openid.net/specs/openid-connect-core-1_0.html
[OpenIDConnect-ID-Token]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken














