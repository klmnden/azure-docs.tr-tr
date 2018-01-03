---
title: "Azure erişim denetimi Hizmeti'nden (ACS) geçirme"
description: "Uygulamalar ve hizmetler Azure erişim denetimi hizmeti dışına taşımak için Seçenekler | Microsoft Azure"
services: active-directory
documentationcenter: dev-center-name
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/14/2017
ms.author: dastrock
ms.openlocfilehash: e32cac7feda929a63c4a80fc0078b221117eb2b5
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/18/2017
---
# <a name="migrating-from-azure-access-control-service-acs"></a>Azure erişim denetimi Hizmeti'nden (ACS) geçirme

Microsoft Azure Active Directory erişim denetimi (erişim denetimi hizmeti veya ACS olarak da bilinir) Kasım 2018 devre dışı bırakıldı.  Uygulamalar ve hizmetler şu anda ACS kullanarak farklı kimlik doğrulama mekanizması bu tarihten önce tam olarak geçirmek gerekir. ACS kullanımını alanı onaylanamadı planlarken bu belgede geçerli müşteriler için öneriler açıklanmaktadır. Şu anda ACS kullanmıyorsanız, herhangi bir eylemde bulunmanız gerekmez.


## <a name="brief-acs-overview"></a>Kısa ACS genel bakış

ACS web uygulamalarınıza erişmek için kimlik doğrulaması ve Kullanıcıları yetkilendirmek kolay bir yol sağlayan bir bulut kimlik doğrulama hizmeti ve Hizmetleri kimlik doğrulaması ve kodunuzu dışında oluşturmak için yetkilendirme özelliklerinin çoğu verirken ' dir. Öncelikle geliştiriciler & mimarları .NET istemcileri, ASP.NET web uygulamaları & WCF web hizmetleri tarafından kullanılır.

ACS kullanım durumları üç ana kategoriye ayrılabilir:

- Azure Service Bus, Dynamics CRM ve diğerleri gibi belirli Microsoft bulut hizmetlerine, kimlik doğrulaması. İstemci uygulamaları, çeşitli eylemleri gerçekleştirmek için bu hizmetleri kimliğini doğrulamak için kullanılan ACS gelen belirteçleri edinebilir.
- Web uygulamaları, hem yerleşik özel çeşitli & (Sharepoint gibi) önceden paketlenmiş çeşitli kimlik doğrulama ekleme. ACS "pasif" kimlik doğrulaması kullanarak, Google, Facebook, Yahoo, Microsoft hesabı (Live kimliği), Azure Active Directory ve ADFS hesaplarıyla oturum açma web uygulamaları destekleyebilir.
- ACS tarafından yayınlanan belirteçleri ile özel yerleşik web hizmetleri güvenli hale getirme. "Active" kimlik doğrulaması kullanarak, web hizmetleri bunlar yalnızca erişim ACS ile doğrulanmıştır bilinen istemcilerden izin verdiğinden emin olun.

Bunların her biri kullanım örnekleri ve bunların önerilen geçiş stratejilerini aşağıdaki bölümlerde ele alınmıştır. Durumlarda büyük çoğunluğu var olan uygulamalar ve hizmetler için daha yeni teknolojileri geçirmek için önemli kod değişikliği gerekir. Başlamanız önerilir planlama & hemen kesintileri ya da kapalı kalma süresi herhangi olasılığını önlemek için geçiş yürütülüyor.

> [!WARNING]
> Çoğu durumda, var olan uygulamalar ve hizmetler için daha yeni teknolojileri geçirmek için önemli kod değişikliği gerekir. Başlamanız önerilir planlama & hemen kesintileri ya da kapalı kalma süresi herhangi olasılığını önlemek için geçiş yürütülüyor.

Mimari, ACS tamamen aşağıdaki bileşenlerden oluşur:

- Kimlik doğrulama isteklerini alır ve iade olarak güvenlik belirteçleri bir güvenli belirteç Hizmeti'ne (STS).
- Oluşturma, silme ve etkinleştirme/ACS ad alanları devre dışı bırakmak için kullanılan Klasik Azure portalı.
- Özelleştirme ve ACS ad alanı davranışını yapılandırmak için kullanılan bir ayrı ACS yönetim portalı.
- Portallar işlevlerini otomatik hale getirmek için kullanılan bir yönetim hizmeti.
- ACS tarafından yayınlanan belirteçleri çıktı biçimi düzenleme için karmaşık mantık tanımlamak için kullanılabilecek bir belirteç dönüştürme kuralı altyapısı.

Bu bileşenleri kullanmak için bir veya daha fazla ACS oluşturma **ad alanları**. Bir ad alanı, uygulamalar ve hizmetler ile iletişim kurmak ACS adanmış örneğidir; kendi ad alanlarını kullanan tüm diğer ACS müşterilerden, ayrı tutulur. Bir ad alanındaki ACS gibi bir ayrılmış URL sahiptir:

```HTTP
https://mynamespace.accesscontrol.windows.net
```

STS ve yönetim işlemleri tüm iletişim bu URL'de farklı amaçlar için farklı yollar ile gerçekleştirilir. Uygulamalar veya hizmetler ACS kullanırsanız belirlemek için izleme tüm trafiği için `https://{namespace}.accesscontrol.windows.net`.  Bu URL için tüm trafik, ACS tarafından işlenir ve kullanımdan kaldırılacak gerekiyor trafiğidir.  Tüm trafik tek istisnası `https://accounts.accesscontrol.windows.net` -trafiği için bu URL zaten farklı bir hizmet tarafından işlenir ve ACS kullanımdan tarafından etkilenmez.  Klasik Azure portalı ve sahip olduğunuz abonelikleri tüm ACS ad alanları için onay oturum açmanın emin olmalısınız.  ACS ad alanları listelenir **Active Directory** altında hizmet **erişim denetimi ad** sekmesi.

ACS hakkında daha fazla bilgi için bkz: [bu arşivlenmiş MSDN'de belgelerin](https://msdn.microsoft.com/library/hh147631.aspx).

## <a name="retirement-schedule"></a>Devre dışı bırakma zamanlama

Kasım 2017'ten itibaren tüm ACS tam olarak desteklenen & işletimsel bileşenleridir. Yalnızca kısıtlama olan [yeni ACS ad alanları Klasik Azure portalı üzerinden oluşturulamıyor](https://azure.microsoft.com/blog/acs-access-control-service-namespace-creation-restriction/).

Bu bileşenlerin kullanımdan kaldırma için zaman çizelgesi bu zamanlamayı aşağıdaki gibidir:

- **Kasım 2017**: Azure AD yönetim deneyimi Klasik Azure Portalı'nda [devre dışı bırakılmış](https://blogs.technet.microsoft.com/enterprisemobility/2017/09/18/marching-into-the-future-of-the-azure-ad-admin-experience-retiring-the-azure-classic-portal/). ACS yeni, şu anda kullanılabilir olacağı için bu noktada, ad alanı yönetim ayrılmış URL: `http://manage.windowsazure.com?restoreClassic=true`. Bu, var olan ad alanları görüntülemek için bunları etkinleştirmek/devre sağlar ve tamamen isterseniz, bunları silin içindir.
- **Nisan 2018**: ACS ad alanı yönetim olduğu artık bu adanmış URL'de kullanılabilir. Bu anda, devre dışı bırak/etkinleştir, silmek veya ACS alanlarınızın numaralandırmak mümkün değildir. ACS yönetim Portalı'nı ancak, tam olarak işlevsel ve konumunda bulunan `https://{namespace}.accesscontrol.windows.net`. Tüm bileşenlerle ACS normal olarak da çalışmaya devam eder.
- **Kasım 2018**: tüm ACS bileşenlerine kapatma kalıcı olarak. Bu ACS yönetim portalı, yönetim hizmeti, STS ve belirteç dönüştürme kuralı altyapısı içerir. Bu noktada, ACS gönderilen tüm istekler (konumunda bulunan `{namespace}.accesscontrol.windows.net`) başarısız. Var olan tüm uygulamalar ve hizmetler için diğer teknolojileri de bu süre önce geçirmiş olmanız.


## <a name="migration-strategies"></a>Geçiş stratejileri

Aşağıdaki bölümlerde, diğer Microsoft teknolojileri için ACS dışına geçirmek için yüksek düzey öneriler açıklanmaktadır.

### <a name="clients-of-microsoft-cloud-services"></a>İstemciler, Microsoft bulut Hizmetleri

Şimdi ACS tarafından yayınlanan belirteçleri kabul Microsoft bulut hizmetlerinin her biri en az bir alternatif kimlik doğrulaması biçimini destekler. Her hizmet resmi kılavuzu için belirli belgeleri başvurma öneririz, böylece her hizmet için doğru kimlik doğrulama mekanizması değişir. Kolaylık olması için her bir belge kümesi burada sağlanır:

| Hizmet | Rehber |
| ------- | -------- |
| Azure Service Bus | [Paylaşılan erişim imzaları geçirme](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-migrate-acs-sas) |
| Azure Geçişi | [Paylaşılan erişim imzaları geçirme](https://docs.microsoft.com/azure/service-bus-relay/relay-migrate-acs-sas) |
| Azure Cache | [Azure Redis Önbelleğine Geçiş](https://docs.microsoft.com/azure/redis-cache/cache-faq#which-azure-cache-offering-is-right-for-me) |
| Azure veri Pazar | [Bilişsel hizmetler API'leri geçirme](https://docs.microsoft.com/azure/machine-learning/studio/datamarket-deprecation) |
| BizTalk Services | [Azure mantıksal uygulamaları geçirme](https://docs.microsoft.com/azure/machine-learning/studio/datamarket-deprecation) |
| Azure Media Services | [Azure AD kimlik doğrulaması için geçirme](https://azure.microsoft.com/blog/azure-media-service-aad-auth-and-acs-deprecation/) |
| Azure Backup | [Azure Yedekleme aracısı yükseltme](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq) |

<!-- Dynamics CRM: Migrate to new SDK, Dynamics team handling privately -->
<!-- Azure RemoteApp deprecated in favor of Citrix: http://www.zdnet.com/article/microsoft-to-drop-azure-remoteapp-in-favor-of-citrix-remoting-technologies/ -->
<!-- Exchange push notifications are moving, customers don't need to move -->
<!-- Retail federation services are moving, customers don't need to move -->
<!-- Azure StorSimple: TODO -->
<!-- Azure SiteRecovery: TODO -->


### <a name="web-applications-using-passive-authentication"></a>Pasif kimlik doğrulama kullanarak web uygulamaları

Geliştiriciler & mimarları web uygulamaları için aşağıdaki özellikler ve yetenekler ACS ACS için kullanıcı kimlik doğrulaması kullanarak web uygulamaları için sağlanan:

- Derin tümleştirme Windows Identity Foundation (WIF).
- Google, Facebook, Yahoo, Microsoft hesabı (Live kimliği), Azure Active Directory ve ADFS ile Federasyon.
- Aşağıdaki kimlik doğrulama protokolleri için destek: OAuth 2.0 taslak 13, Ws-Trust ve Ws-Federasyon.
- Aşağıdaki belirteci biçimleri için destek: JSON web token (JWT), SAML 1.1, SAML 2.0 ve basit web token (SWT).
- Kullanıcıların oturum açmak için kullandığınız hesabın türünü seçmek izin WIF, tümleştirilmiş bir giriş bölgesi bulma deneyimini. Bu deneyim ve tam olarak özelleştirilebilir web uygulaması tarafından barındırılan.
- ACS, web uygulamasından tarafından alınan talep Zengin özelleştirme izin dönüştürme belirteci dahil olmak üzere:
    - kimlik sağlayıcılardan gelen taleplere aracılığıyla geçirme
    - Ek özel talep ekleme
    - belirli koşullar altında talepleri vermek basit IF then mantığı

Ne yazık ki, değil tüm bu eşdeğer yetenekleri sağlayan bir hizmet yok.  ACS hangi özelliklerini, gerekir ve ardından kullanma arasında değerlendirmelidir [ **Azure Active Directory** ](https://azure.microsoft.com/develop/identity/signin/) (Azure AD), [ **Azure AD B2C** ](https://azure.microsoft.com/services/active-directory-b2c/), veya başka bir bulut kimlik doğrulama hizmeti.

#### <a name="migrating-to-azure-active-directory"></a>Azure Active Directory'ye geçirme

Dikkate alınması gereken bir yolu, uygulamalar ve hizmetler doğrudan Azure Active Directory ile tümleştirme. Azure AD bulut tabanlı kimlik sağlayıcısı olduğu için Microsoft iş ve Okul hesapları - Office 365, Azure ve daha fazlasını kimlik sağlayıcıdır. Benzer sağlar federe kimlik doğrulama özellikleri için ACS, ancak tüm ACS özellikleri desteklemiyor. Birincil Federasyon Facebook, Google ve Yahoo gibi sosyal kimlik sağlayıcısı ile bir örnektir. Kullanıcılarınız bu tür kimlik bilgileriyle oturum açarsanız Azure AD sizin için değil. Bu ayrıca gerekmez destek ACS - iki ACS sırasında olarak kesin aynı kimlik doğrulama protokolleri ve Azure AD desteği OAuth yok, örneğin, kod geçişin bir parçası değiştirmek ihtiyaç duyduğunuz her uygulaması arasındaki farklar vardır.

Azure AD ancak ACS müşterilere çeşitli olası avantajları sağlar. Yerel olarak destekleyen Microsoft İş & Okul ACS müşteriler tarafından yaygın olarak kullanılan bulutta barındırılan hesapları.  Azure AD kiracısı de ADFS hem bulut tabanlı kullanıcıları hem de şirket içinde barındırılan kullanıcıların kimliğini doğrulamak, uygulamanın verme aracılığıyla şirket içi Active Directory bir veya daha fazla örneğini federe.  Ayrıca, Windows Identity Foundation (WIF) kullanan bir web uygulaması ile tümleştirmek görece basit hale getirir Ws-Federasyon protokolünü destekler.

Aşağıdaki tabloda, Azure AD'de ACS kullanılabilenlerle web uygulamalarıyla ilgili özellikleri karşılaştırılmaktadır. Yüksek bir düzeyde **Azure Active Directory büyük olasılıkla, geçiş için uygun seçeneği yalnızca kullanıcıların oturum açma ile Microsoft işlerine & Okul hesapları izin verirseniz**.

| Özellik | ACS desteği | Azure AD desteği |
| ---------- | ----------- | ---------------- |
| **Hesap türü** | | |
| Microsoft iş ve Okul hesapları | Destekleniyor | Destekleniyor |
| Windows Server Active Directory & ADFS hesaplarından | Azure AD kiracısı ile Federasyon aracılığıyla desteklenir <br /> ADFS ile doğrudan Federasyon aracılığıyla desteklenir | Azure AD kiracısı ile Federasyon ile yalnızca desteklenen | 
| Diğer Kurumsal kimlik yönetimi sistemlerinden hesapları | Azure AD kiracısı ile Federasyon aracılığıyla olası <br />Doğrudan Federasyon desteklenen | Azure AD kiracısı ile Federasyon aracılığıyla olası |
| Microsoft hesapları kişisel kullanım (Windows Live ID) | Destekleniyor | Azure AD v2.0 OAuth protokolü aracılığıyla, ancak başka bir protokol üzerinden değil desteklenir. | 
| Facebook, Google, Yahoo hesapları | Destekleniyor | Desteklenmeyen yoktur. |
| **Protokoller ve SDK uyumluluk** | | |
| Windows Identity Foundation (WIF) | Destekleniyor | , Kullanılabilir sınırlı yönergeleri desteklenir. |
| WS-Federasyon | Destekleniyor | Destekleniyor |
| OAuth 2.0 | Taslak 13 desteği | RFC 6749, çoğu modern belirtimi desteği. |
| WS-Trust | Destekleniyor | Desteklenmiyor |
| **Belirteç biçimleri** | | |
| JSON Web belirteçleri (Jwt'ler) | Beta'de desteklenen | Destekleniyor |
| SAML 1.1 belirteçleri | Destekleniyor | Destekleniyor |
| SAML 2.0 belirteçleri | Destekleniyor | Destekleniyor |
| Basit Web belirteçleri (SWT) | Destekleniyor | Desteklenmiyor |
| **Özelleştirmeleri** | | |
| Özelleştirilebilir giriş bölgesi bulma/hesabı UI çekme | Uygulamalarda birleştirilebilir indirilebilir kodu | Desteklenmiyor |
| Özel belirteç imzalama sertifikaları karşıya yükle | Destekleniyor | Destekleniyor |
| Belirteçleri Taleplerde özelleştirme | Geçiş kimlik sağlayıcılardan gelen taleplere giriş<br />Erişim kimlik sağlayıcısından bir talep belirteci alma<br />Çıkış talep giriş talepleri değerlerine göre<br />Sabit değerleri içeren sorun çıktı talepleri | Geçiş taleplerin Federal Kimlik sağlayıcılardan yapılamıyor<br />Erişim belirteci kimlik sağlayıcısı'ndan talep olarak alınamıyor<br />Çıkış talep giriş talepleri değerlerine göre veremez<br />Çıktı talepler ile sabit değerleri verebilir<br />Kullanıcıların Azure AD ile synched özelliklerine göre çıkış talep verebilir |
| **Otomasyon** | | |
| Yapılandırma/yönetim görevlerini otomatik hale getirme | ACS yönetim hizmeti desteklenir | Microsoft Graph & Azure AD grafik API'si üzerinden desteklenir. |

Azure AD uygun yolu İleri uygulamalar ve hizmetler için olduğuna karar verirseniz, uygulamanızı Azure AD ile tümleştirmek için iki yol haberdar olmanız gerekir.

Azure AD ile tümleştirmek için WS-Federasyon/WIF kullanmak için önerilir aşağıdaki [bu makalede açıklanan yaklaşım](https://docs.microsoft.com/azure/active-directory/application-config-sso-how-to-configure-federated-sso-non-gallery). Makaleyi SAML tabanlı çoklu oturum açma için Azure AD, ancak Ws-Federasyon de yapılandırma works yapılandırmaya başvuruyor. Bu yaklaşımı izleyerek bir Azure AD Premium lisansı gerektirir, ancak iki avantajları vardır:

- Azure AD belirteci özelleştirme tam esnekliğini alın. Bu, özellikle kullanıcı kimliği veya adı tanımlayıcısı talebi dahil olmak üzere ACS tarafından verilen eşleştiğinden Azure AD tarafından verilen talepler özelleştirmenize olanak sağlar. Kullanıcı kimliklerini teknolojileri bile değiştirdikten sonra kullanıcılarınızın tutarlı bir kullanıcı tanımlayıcıları almaya devam Azure AD eşleşme Bu ACS tarafından verilen tarafından verilen emin olmak gerekir.
- Bir belirteç imzalama sertifikası, uygulamaya özgü yapılandırabilirsiniz denetim, yaşam süresi.

<!--

Possible nameIdentifiers from ACS (via AAD or ADFS):
- ADFS - Whatever ADFS is configured to send (email, UPN, employeeID, what have you)
- Default from AAD using App Registrations, or Custom Apps before ClaimsIssuance policy: subject/persistent ID
- Default from AAD using Custom apps nowadays: UPN
- Kusto can't tell us distribution, it's redacted

-->

> [!NOTE]
> Bu yaklaşım ile devam eden bir Azure AD Premium lisansı gerektirir. Bir ACS müşterisiyseniz ve çoklu oturum açma bir uygulama için ayarlamak için premium lisansı gerektirir, bize ulaşın ve biz kullanımınız için geliştirici lisansı sağlamak mutluluk.

Alternatif bir yaklaşım izlemektir [Bu kod örneği](https://github.com/Azure-Samples/active-directory-dotnet-webapp-wsfederation), Ws-Federasyon ayarlama konusunda biraz farklı yönergeler sağlayan. Bu kod örneği, WIF, ancak bunun yerine, ASP.NET 4.5 OWIN ara yazılımı kullanmaz. Ancak, uygulama kayıt yönergelerini WIF kullanan uygulamalar için geçerlidir ve bir Azure AD Premium lisansı gerektirmez.  Bu yaklaşım, anlamak için heneed seçerseniz [anahtar geçişi Azure AD'de imzalama](https://docs.microsoft.com/azure/active-directory/develop/active-directory-signing-key-rollover). Bu yaklaşım imzalama anahtarı sorunu belirteçleri için genel Azure AD kullanır. Varsayılan olarak, WIF İmzalama anahtarları otomatik olarak yenilemez. Azure AD genel kendi İmzalama anahtarları döndürdüğünde WIF uygulamanızı değişiklikleri kabul etmek için hazırlanması gerekir.

Openıd Connect veya OAuth protokolleri aracılığıyla Azure AD ile tümleştirebilir varsa, bunun yapılması önerilir.  Uygulamanıza web bulunan Azure AD tümleştirme yönelik Rehber ve kapsamlı belgeler sahibiz bizim [Azure AD Geliştirici Kılavuzu](http://aka.ms/aaddev).

<!-- TODO: If customers ask about authZ, let's put a blurb on role claims here -->

#### <a name="migrating-to-azure-ad-b2c"></a>Azure AD B2C'ye geçirme

Dikkate alınması gereken diğer geçiş Azure AD B2C yoludur.  B2C bulut kimlik doğrulaması olduğundan, hizmet ACS, benzer bir bulut hizmeti için kendi kimlik doğrulama ve kimlik yönetimi mantığı dış yayınlamasına izin verir.  Etkin kullanıcılar en çok bir milyonlarca içeren uygulamalar karşılıklı tüketici için tasarlanmış bir Ücretli hizmetiyle (ücretsiz ve premium katmanları) değil.

ACS, B2C en çekici özellikler, farklı türlerde hesaplarını destekleyen olan gibi. B2C ile oturum açma kendi Facebook, Google, Microsoft, LinkedIn, GitHub, Yahoo hesapları olan kullanıcılar ve daha fazlası. B2C ayrıca özellikle uygulamanız için "Yerel hesaplar", veya kullanıcı adı ve kullanıcı oluşturma parolaları destekler. Azure AD B2C'de, oturum açma akışları özelleştirmek için kullanabileceğiniz Zengin genişletilebilirlik sağlar. Bu ancak, kimlik doğrulama protokolleri & ACS müşteriler gerektirebilir belirteci biçimleri derecesini desteklemez. Belirteçleri Al & Sorgu kimlik sağlayıcısından Microsoft kullanıcı hakkında ek bilgi için ayrıca kullanılamaz veya aksi takdirde.

Aşağıdaki tabloda Azure AD B2C'de kullanılabilir olan web uygulamalarıyla ilgili ACS özelliklerini karşılaştırır. Yüksek bir düzeyde **Azure AD B2C büyük olasılıkla, geçiş için doğru seçim uygulamanız karşılıklı tüketici ise ya da farklı türlerde hesaplarını destekler.**

| Özellik | ACS desteği | Azure AD B2C desteği |
| ---------- | ----------- | ---------------- |
| **Hesap türü** | | |
| Microsoft iş ve Okul hesapları | Destekleniyor | Özel ilkeler desteklenir  |
| Windows Server Active Directory & ADFS hesaplarından | ADFS ile doğrudan Federasyon aracılığıyla desteklenir | Desteklenen SAML Federasyon özel ilkelerini kullanma |
| Diğer Kurumsal kimlik yönetimi sistemlerinden hesapları | Ws-Federasyon doğrudan Federasyon desteklenen | Desteklenen SAML Federasyon özel ilkelerini kullanma |
| Microsoft hesapları kişisel kullanım (Windows Live ID) | Destekleniyor | Destekleniyor | 
| Facebook, Google, Yahoo hesapları | Destekleniyor | Facebook & Google, Yahoo Openıd Connect Federasyon özel ilkelerini kullanma desteklenen desteklen |
| **Protokoller ve SDK uyumluluk** | | |
| Windows Identity Foundation (WIF) | Destekleniyor | Desteklenmiyor. |
| WS-Federasyon | Destekleniyor | Desteklenmiyor. |
| OAuth 2.0 | Taslak 13 desteği | RFC 6749, çoğu modern belirtimi desteği. |
| WS-Trust | Destekleniyor | Desteklenmiyor. |
| **Belirteç biçimleri** | | |
| JSON Web belirteçleri (Jwt'ler) | Beta'de desteklenen | Destekleniyor |
| SAML 1.1 belirteçleri | Destekleniyor | Desteklenmiyor |
| SAML 2.0 belirteçleri | Destekleniyor | Desteklenmiyor |
| Basit Web belirteçleri (SWT) | Destekleniyor | Desteklenmiyor |
| **Özelleştirmeleri** | | |
| Özelleştirilebilir giriş bölgesi bulma/hesabı UI çekme | Uygulamalarda birleştirilebilir indirilebilir kodu | Tam olarak özelleştirilebilir kullanıcı Arabirimi aracılığıyla özel CSS. |
| Özel belirteç imzalama sertifikaları karşıya yükle | Destekleniyor | Özel ilkeler desteklenen özel İmzalama anahtarları, sertifikaları değil. |
| Belirteçleri Taleplerde özelleştirme | Geçiş kimlik sağlayıcılardan gelen taleplere giriş<br />Erişim kimlik sağlayıcısından bir talep belirteci alma<br />Çıkış talep giriş talepleri değerlerine göre<br />Sabit değerleri içeren sorun çıktı talepleri | Kimlik sağlayıcılardan gelen geçiş talep edebilir. Özel ilkeler bazı talepler için gereklidir.<br />Erişim belirteci kimlik sağlayıcısı'ndan talep olarak alınamıyor<br />Çıkış talep giriş talepleri özel ilkeler aracılığıyla değerlerine dayalı verebilir<br />Özel ilkeler aracılığıyla sabit değerleri ile çıkış talepler verebilir |
| **Otomasyon** | | |
| Yapılandırma/yönetim görevlerini otomatik hale getirme | ACS yönetim hizmeti desteklenir | Azure AD Graph API üzerinden verilen kullanıcılar oluşturma. B2C Kiracı, uygulamaları veya ilkeleri programlı olarak oluşturulamıyor. |

Azure AD B2C uygun yolu İleri uygulamalar ve hizmetler için olduğuna karar verirseniz, aşağıdaki kaynaklar ile başlamanız gerekir:

- [Azure AD B2C belgeleri](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview)
- [Azure AD B2C özel ilkeler](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview-custom)
- [Azure AD B2C fiyatlandırma](https://azure.microsoft.com/pricing/details/active-directory-b2c/)


#### <a name="other-migration-options"></a>Diğer geçiş seçenekleri

Varsa hiçbiri Azure AD veya Azure AD B2C, web uygulamanızın ihtiyaçlarını karşılayan, lütfen bize ulaşın ve en iyi yolu ileri tanımlamak yardımcı olabiliriz.

<!--

#### Migrating to Ping Identity or Auth0

In some cases, you may find that neither Azure AD nor Azure AD B2C is sufficient to replace ACS in your web applications without making major code changes.  Some common examples might include:

- web applications using WIF and/or WS-Federation for sign-in with social identity providers such as Google or Facebook.
- web applications performing direct federation to an enterprise IDP over the Ws-Federation protocol.
- web applications that require the access token issued by a social identity provider (such as Google or Facebook) as a claim in the tokens issued by ACS.
- web applications with complex token transformation rules that Azure AD or Azure AD B2C cannot reproduce.

In these cases, you may want to consider migrating your web application to another cloud authentication service. We recommend exploring the following options, as each provides capabilities similar to ACS:

- [Auth0](https://auth0.com/) has created [high-level migration guidance for customers of ACS](https://auth0.com/blog/windows-azure-acs-alternative-replacement/), and provides a feature-by-feature comparison of ACS vs. Auth0.
- Enterprise customers should consider [Ping Identity](https://www.pingidentity.com) as well. Reach out to us and we can connect you with a representative from Ping who is prepared to help identify potential solutions.

Our aim in working with Ping Identity & Auth0 is to ensure that all ACS customers have a path forward for their apps & services that minimizes the amount of work required to move off of ACS.

-->

<!--

## Sharepoint 2010, 2013, 2016

TODO: AAD only, use AAD SAML 1.1 tokens, when we bring it back online.
Other IDPs: use Auth0? https://auth0.com/docs/integrations/sharepoint.

-->

### <a name="web-services-using-active-authentication"></a>Etkin kimlik doğrulaması kullanan web Hizmetleri

ACS tarafından yayınlanan belirteçleri ile güvenliği sağlanan web hizmetleri için aşağıdaki özellikler ve yetenekler ACS sağlanmış:

- Bir veya daha fazla kayıt yeteneği **hizmet kimliklerini** belirteçler istemek için kullanılan, ACS ad.
- OAuth Kaydır & OAuth 2.0 taslak 13 belirteçleri, kimlik bilgilerini aşağıdaki türlerini kullanarak isteyen protokollerini destekler:
    - Hizmet kimliği için oluşturulan basit bir parola
    - Bir simetrik anahtar veya X509 kullanarak SWT imzalı sertifika
    - Güvenilen kimlik sağlayıcısı (genellikle bir ADFS örneği) tarafından verilen bir SAML belirteci
- Aşağıdaki belirteci biçimleri için destek: JSON web token (JWT), SAML 1.1, SAML 2.0 ve basit web token (SWT).
- Basit belirteci dönüştürme kuralları

ACS hizmet kimlikleri genellikle sunucudan sunucuya (S2S) gibi kimlik doğrulama gerçekleştirmek için kullanılır.  

#### <a name="migrating-to-azure-active-directory"></a>Azure Active Directory'ye geçirme

Bu tür bir kimlik doğrulama akışı için Bizim önerimiz geçirmek için olan [ **Azure Active Directory** ](https://azure.microsoft.com/develop/identity/signin/) (Azure AD). Azure AD bulut tabanlı kimlik sağlayıcısı olduğu için Microsoft iş ve Okul hesapları - Office 365, Azure ve daha fazlasını kimlik sağlayıcıdır. Ancak, OAuth istemci kimlik bilgilerini verme Azure AD uygulaması kullanarak sunucu kimlik doğrulaması için de kullanılabilir.  Aşağıdaki tabloda Azure AD ile kullanılabilen sunucu kimlik doğrulaması ACS özelliklerinde karşılaştırır.

| Özellik | ACS desteği | Azure AD desteği |
| ---------- | ----------- | ---------------- |
| Bir web hizmetine kaydetme | Bir bağlı olan taraf ACS yönetim Portalı'nda oluşturun. | Azure portalında Azure AD web uygulaması oluşturun. |
| Bir istemci kaydetme | Bir hizmet kimliği ACS yönetim Portalı'nda oluşturun. | Azure portalında başka bir Azure AD web uygulaması oluşturun. |
| Kullanılan protokolü | OAuth SARMALAMAK Protokolü<br />OAuth 2.0 taslak 13 istemci kimlik bilgileri verin | OAuth 2.0 istemci kimlik bilgileri verin |
| İstemci kimlik doğrulaması yöntemleri | Basit parola<br />İmzalı SWT<br />Federasyon IDP SAML belirtecinden | Basit parola<br />İmzalı JWT |
| Belirteç biçimleri | JWT<br />SAML 1.1<br />SAML 2.0<br />SWT<br /> | JWT yalnızca |
| Belirteç dönüştürme | Özel talep ekleme<br />Basit IF then talep verme mantığı | Özel talep ekleme | 
| Yapılandırma/yönetim görevlerini otomatik hale getirme | ACS yönetim hizmeti desteklenir | Microsoft Graph & Azure AD grafik API'si üzerinden desteklenir. |

Uygulama sunucusu sunucusu senaryoları hakkında yönergeler için aşağıdaki kaynaklara bakın:

- [Azure AD Geliştirici Kılavuzu'nun hizmet bölümüne](https://aka.ms/aaddev).
- [Basit parola istemci kimlik bilgilerini kullanarak arka plan kod örneği](https://github.com/Azure-Samples/active-directory-dotnet-daemon)
- [Sertifika istemci kimlik bilgilerini kullanarak arka plan kod örneği](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential)

#### <a name="other-migration-options"></a>Diğer geçiş seçenekleri

Azure AD web hizmetiniz gereksinimlerini karşılamıyorsa, Lütfen bir yorum yazın ve belirli durumunuz için en iyi plan tanımlamaya yardımcı olabiliriz.

<!--

#### Migrating to Ping Identity or Auth0

In some cases, you may find that neither Azure AD's client credentials OAuth grant implementation is not sufficient to replace ACS in your architecture without major code changes.  Some common examples might include:

- server-to-server authentication using token formats other than JWTs.
- server-to-server authentication using an input token provided by an external identity provider.
- server-to-server authentication with token transformation rules that Azure AD cannot reproduce.

In these cases, you may want to consider migrating your web application to another cloud authentication service. We recommend exploring the following options, as each provides capabilities similar to ACS:

- [Auth0](https://auth0.com/) has created [high-level migration guidance for customers of ACS](https://auth0.com/blog/windows-azure-acs-alternative-replacement/), and provides a feature-by-feature comparison of ACS vs. Auth0.
- Enterprise customers should consider [Ping Identity](https://www.pingidentity.com) as well. Reach out to us and we can connect you with a representative from Ping who is prepared to help identify potential solutions.

Our aim in working with Ping Identity & Auth0 is to ensure that all ACS customers have a path forward for their apps & services that minimizes the amount of work required to move off of ACS.

-->

## <a name="questions-concerns--feedback"></a>Sorular, sorunları ve geri bildirim

Biz, çok sayıda ACS müşteriler açık bir geçiş yolu bu makaleyi okuduktan sonra bulamaz ve bazı Yardım veya doğru planın belirlemede rehberlik gerekebilir anlayın. Geçiş senaryolarını ve soruları ele istiyorsanız, bu sayfada bir yorum bırakın.
