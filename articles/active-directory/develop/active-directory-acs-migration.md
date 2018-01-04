---
title: "Azure erişim denetimi Hizmeti'nden geçirme | Microsoft Docs"
description: "Uygulamaları ve Hizmetleri Azure erişim denetimi Hizmeti'nden taşıma seçenekleri"
services: active-directory
documentationcenter: dev-center-name
author: dstrockis
manager: mtillman
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/14/2017
ms.author: dastrock
<<<<<<< HEAD
ms.openlocfilehash: e32cac7feda929a63c4a80fc0078b221117eb2b5
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: HT
=======
ms.openlocfilehash: f3de9016fe29a51ab2c7fb9e93fcd33af0f0e871
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="migrate-from-the-azure-access-control-service"></a>Azure erişim denetimi Hizmeti'nden geçirme

Azure erişim denetimi, Azure Active Directory (Azure AD), bir hizmet Kasım 2018 kullanımdan kaldırılacaktır. Uygulamalar ve şu anda erişim denetimi kullanan hizmetler için farklı kimlik doğrulama mekanizması ardından tarafından tam olarak geçirilmelidir. Erişim denetimi kullanımınız alanı onaylanamadı planlarken bu makalede geçerli müşteriler için öneriler açıklanmaktadır. Erişim denetimi şu anda kullanmıyorsanız, herhangi bir eylemde bulunmanız gerekmez.


## <a name="overview"></a>Genel Bakış

Erişim denetimi kimliğini doğrulamak ve web uygulamaları ve hizmetlerine erişim için Kullanıcıları yetkilendirmek için kolay bir yol sunan bir bulut kimlik doğrulama hizmetidir. Kimlik doğrulama ve yetkilendirme kodunuzu dışında oluşturmak için birçok özellik sağlar. Erişim denetimi, öncelikle geliştiriciler ve Microsoft .NET istemcileri, ASP.NET web uygulamaları ve Windows Communication Foundation (WCF) web hizmetlerini mimarları tarafından kullanılır.

Erişim denetimi için kullanım örnekleri üç ana kategoriye ayrılabilir:

- Azure Service Bus ve Dynamics CRM gibi belirli Microsoft bulut hizmetlerine, kimlik doğrulaması. İstemci uygulamaları belirteçleri çeşitli eylemleri gerçekleştirmek için bu hizmetler için kimlik doğrulaması için erişim denetimi ile alın.
- Web uygulamaları, hem özel hem de (örneğin, SharePoint) paketlenmiş ekleme kimlik doğrulaması. Erişim denetimi "pasif" kimlik doğrulaması kullanarak web uygulamaları oturum açma bir Microsoft hesabı (önceki adıyla Live ID) ve Google, Facebook, Yahoo, Azure AD hesapları ile destekleyebilir ve Active Directory Federasyon Hizmetleri (AD FS).
- Erişim denetimi tarafından yayınlanan belirteçleri ile özel web hizmetleri güvenli hale getirme. "Active" kimlik doğrulaması kullanarak, yalnızca kimliği doğrulanmış bilinen istemcilere erişim erişim denetimi ile izin web hizmetleri emin olabilirsiniz.

Bunların her birini kullanın ve stratejileri açıklanan, önerilen geçiş durumları aşağıdaki bölümlerde. 

> [!WARNING]
> Çoğu durumda, var olan uygulamalar ve hizmetler için daha yeni teknolojileri geçirmek için önemli kod değişikliği gerekir. Planlama hemen başlamanızı öneriyoruz ve kapalı kalma süresi veya olası tüm kesintileri önlemek için geçiş yürütme.

Erişim denetimi aşağıdaki bileşenlere sahiptir:

- Kimlik doğrulama isteklerini alır ve iade olarak güvenlik belirteçleri bir güvenli belirteç hizmeti (STS).
- Burada oluşturduğunuz, Klasik Azure portalı, silmek ve etkinleştirmek ve erişim denetimi ad alanları devre dışı.
- Burada özelleştirmek ve erişim denetimi ad yapılandırma bir ayrı erişim denetimi Yönetim Portalı.
- Portal işlevleri otomatikleştirmek için kullanabileceğiniz bir yönetim hizmeti.
- Erişim denetimi sorunları belirteçleri çıkış biçimini denetlemek için karmaşık mantık tanımlamak için kullanabileceğiniz bir belirteç dönüştürme kuralı alt yapısı.

Bu bileşenleri kullanmak için bir veya daha fazla erişim denetimi ad alanı oluşturmanız gerekir. A *ad alanı* uygulamaları ve Hizmetleri ile iletişim kuran bir erişim denetimi, adanmış bir örneğidir. Diğer tüm erişim denetimi müşterilerden yalıtılmış bir ad alanıdır. Diğer erişim denetimi müşteriler kendi ad alanları kullanın. Erişim denetimi ad alanı şuna benzer bir ayrılmış URL sahiptir:

```HTTP
https://<mynamespace>.accesscontrol.windows.net
```

STS ve yönetim işlemleri tüm iletişim bu URL'de yapılır. Farklı amaçlar için farklı yollar kullanın. Uygulamalara veya hizmetlere erişim denetimi kullanıp kullanmayacağınızı belirlemek için izleme https:// tüm trafik için\<ad alanı\>. accesscontrol.windows.net. Bu URL için tüm trafik, erişim denetimi tarafından işlenir ve kullanımdan kaldırılacak gerekiyor. 

Bunun özel durumu https://accounts.accesscontrol.windows.net için tüm trafiğidir. Bu URL için trafiği zaten farklı bir hizmet tarafından işlenir ve erişim denetimi kullanımdan tarafından etkilenmez. 

Ayrıca Klasik Azure portalı ve sahip olduğunuz Aboneliklerde herhangi bir erişim denetimi ad alanları için onay oturum. Erişim denetimi ad listelenir **erişim denetimi ad** sekmesinde, altında **Active Directory** hizmet.

Erişim denetimi hakkında daha fazla bilgi için bkz: [erişim denetimi hizmeti (arşivlenmiş) 2.0](https://msdn.microsoft.com/library/hh147631.aspx).

## <a name="retirement-schedule"></a>Devre dışı bırakma zamanlama

Kasım 2017'ten itibaren tüm erişim denetimi tam olarak desteklenen ve işletimsel bileşenleridir. Yalnızca kısıtlama olan, [Klasik Azure Portalı aracılığıyla yeni erişim denetimi ad oluşturulamıyor](https://azure.microsoft.com/blog/acs-access-control-service-namespace-creation-restriction/).

Erişim denetimi bileşenleri onaysız kılınmadan zamanlamasını şöyledir:

- **Kasım 2017**: Azure AD yönetim deneyimi Azure Klasik Portalı'nda [devre dışı bırakılmış](https://blogs.technet.microsoft.com/enterprisemobility/2017/09/18/marching-into-the-future-of-the-azure-ad-admin-experience-retiring-the-azure-classic-portal/). Bu noktada, ad alanı yönetim erişim denetimi için yeni, özel bir URL kullanılabilir: http://manage.windowsazure.com?restoreClassic=true. İsterseniz bu URl, var olan ad alanları görüntülemek, etkinleştirin ve ad alanlarını devre dışı bırakma ve ad alanları, silmek için kullanın.
- **Nisan 2018**: erişim denetimi ad alanı yönetimidir artık adanmış http://manage.windowsazure.com?restoreClassic=true URL'de kullanılabilir. Bu noktada, devre dışı bırakmak veya etkinleştirmek, silemez veya erişim denetimi ad alanları numaralandırır. Ancak, erişim denetimi Yönetim Portalı'nı tam olarak işlevsel ve https:// konumunda bulunan olacaktır\<ad alanı\>. accesscontrol.windows.net. Tüm bileşenlerle erişim denetimi normal şekilde çalışmaya devam eder.
- **Kasım 2018**: tüm erişim denetimi bileşenleri kalıcı olarak kapatıldı. Bu, erişim denetimi Yönetim Portalı, yönetim hizmeti, STS ve belirteç dönüştürme kuralı altyapısı içerir. Bu noktada, erişim denetimi için gönderilen tüm istekler (konumunda bulunan \<ad alanı\>. accesscontrol.windows.net) başarısız. Var olan tüm uygulamaları ve Hizmetleri için başka teknolojiler de bu süreden önce geçirmiş olmanız.


## <a name="migration-strategies"></a>Geçiş stratejileri

Aşağıdaki bölümlerde, diğer Microsoft teknolojileri için erişim denetiminden geçirmek için üst düzey öneriler açıklanmaktadır.

### <a name="clients-of-microsoft-cloud-services"></a>İstemciler, Microsoft bulut Hizmetleri

Erişim denetimi tarafından şimdi verilen belirteçleri kabul eder her Microsoft bulut hizmetine en az bir alternatif kimlik doğrulaması biçimini destekler. Her hizmet için doğru kimlik doğrulama mekanizması değişir. Her hizmet için resmi kılavuzu için belirli belgelerine başvurun öneririz. Kolaylık olması için her bir belge kümesi burada sağlanır:

| Hizmet | Rehber |
| ------- | -------- |
| Azure Service Bus | [Paylaşılan erişim imzaları geçirme](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-migrate-acs-sas) |
| Azure Service Bus geçişi | [Paylaşılan erişim imzaları geçirme](https://docs.microsoft.com/azure/service-bus-relay/relay-migrate-acs-sas) |
| Azure yönetilen önbellek | [Azure Redis Önbelleğine Geçiş](https://docs.microsoft.com/azure/redis-cache/cache-faq#which-azure-cache-offering-is-right-for-me) |
| Azure DataMarket | [Bilişsel hizmetler API'leri geçirme](https://docs.microsoft.com/azure/machine-learning/studio/datamarket-deprecation) |
| BizTalk Services | [Azure App Service Logic Apps özelliğini geçirme](https://docs.microsoft.com/azure/machine-learning/studio/datamarket-deprecation) |
| Azure Media Services | [Azure AD kimlik doğrulaması için geçirme](https://azure.microsoft.com/blog/azure-media-service-aad-auth-and-acs-deprecation/) |
| Azure Backup | [Azure Backup Aracısı yükseltme](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq) |

<!-- Dynamics CRM: Migrate to new SDK, Dynamics team handling privately -->
<!-- Azure RemoteApp deprecated in favor of Citrix: http://www.zdnet.com/article/microsoft-to-drop-azure-remoteapp-in-favor-of-citrix-remoting-technologies/ -->
<!-- Exchange push notifications are moving, customers don't need to move -->
<!-- Retail federation services are moving, customers don't need to move -->
<!-- Azure StorSimple: TODO -->
<!-- Azure SiteRecovery: TODO -->


### <a name="web-applications-that-use-passive-authentication"></a>Pasif kimlik doğrulama kullanan web uygulamaları

Kullanıcı kimlik doğrulaması için erişim denetimi kullanan web uygulamaları için web uygulaması geliştiricileri ve mimarlar için aşağıdaki özellikler ve yetenekler erişim denetimi sağlar:

- Derin tümleştirme Windows Identity Foundation (WIF).
- Google, Facebook, Yahoo, Azure Active Directory ve AD FS ve Microsoft hesaplarının ile Federasyon.
- Aşağıdaki kimlik doğrulama protokolleri için destek: OAuth 2.0 taslak 13, WS-Trust ve Web Hizmetleri Federasyonu (WS-Federation).
- Aşağıdaki belirteci biçimleri için destek: JSON Web Token (JWT), SAML 1.1, SAML 2.0 ve basit Web Token (SWT).
- WIF tümleşik bir giriş bölgesi bulma deneyimini, kullanıcıların oturum açmak için kullandığınız hesap türü seç olanak tanır. Bu deneyim, web uygulaması tarafından barındırılan ve tam olarak özelleştirilebilir.
- Simge Zengin özelleştirme erişim denetimi, web uygulamasından tarafından alınan Talep veren dönüştürme dahil olmak üzere:
    - Kimlik sağlayıcılardan gelen taleplere geçirir.
    - Ek özel talep ekleniyor.
    - Belirli koşullar altında talepleri vermek basit IF then mantığı.

Ne yazık ki, tüm bu eşdeğer yetenekleri sağlayan bir hizmet yok. Hangi özellikleri erişim denetimi, gerekir ve kullanımı arasında seçim değerlendirmelidir [Azure Active Directory](https://azure.microsoft.com/develop/identity/signin/), [Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) (Azure AD B2C) veya başka bir bulut kimlik doğrulaması hizmet.

#### <a name="migrate-to-azure-active-directory"></a>Azure Active Directory'ye geçirme

Dikkate alınması gereken bir yolu, uygulamaları ve Hizmetleri doğrudan Azure AD ile tümleştirme. Azure AD bulut tabanlı kimlik sağlayıcısı olduğu için Microsoft iş veya Okul hesapları. Azure AD kimlik sağlayıcısı için Office 365, Azure ve daha fazlasını ' dir. Benzer sağlar federe kimlik doğrulama özellikleri için erişim denetimi, ancak tüm erişim denetimi özellikleri desteklemiyor. 

Birincil Federasyon Facebook, Google ve Yahoo gibi sosyal kimlik sağlayıcısı ile bir örnektir. Kullanıcılarınız bu tür kimlik bilgileriyle oturum açarsanız Azure AD için çözüm değil. 

Azure AD erişim denetimi olarak tam olarak aynı kimlik doğrulama protokolleri mutlaka desteklemiyor. Örneğin, erişim denetimi ve Azure AD OAuth desteklese de, her uygulama arasındaki farklar vardır. Farklı uygulamaları, geçiş işleminin parçası kod değiştirmenizi gerektirir.

Ancak, Azure AD erişim denetimi müşterilere çeşitli olası avantajları sunar. Onu yerel olarak destekleyen Microsoft iş veya Okul erişim denetimi müşteriler tarafından yaygın olarak kullanılan bulutta barındırılan hesapları. 

Azure AD kiracısı ayrıca şirket içi Active Directory AD FS ile bir veya daha fazla örneğini federe. Bu şekilde, uygulamanızın bulut tabanlı kullanıcıları doğrulayabilir ve kullanıcılar şirket içi barındırılır. Ayrıca, WIF kullanarak bir web uygulaması ile tümleştirmek görece basit hale getirir WS-Federasyon protokolünü destekler.

Aşağıdaki tabloda, Azure AD'de kullanılabilen bu özellikler web uygulamalarıyla ilgili erişim denetimi özellikleri karşılaştırılmaktadır. 

Yüksek bir düzeyde *Azure Active Directory büyük olasılıkla en iyi seçenek, geçiş için kullanıcıların oturum izin verirseniz, yalnızca kendi Microsoft ile iş veya Okul hesapları*.

| Özellik | Erişim denetimi desteği | Azure AD desteği |
| ---------- | ----------- | ---------------- |
| **Hesap türü** | | |
| Microsoft iş veya Okul hesapları | Destekleniyor | Destekleniyor |
| Windows Server Active Directory ve AD FS hesaplarından |-Azure AD kiracısı ile Federasyon aracılığıyla desteklenir <br />-AD FS ile doğrudan Federasyon aracılığıyla desteklenir | Azure AD kiracısı ile Federasyon ile yalnızca desteklenen | 
| Diğer Kurumsal kimlik yönetimi sistemlerinden hesapları |-Azure AD kiracısı ile Federasyon aracılığıyla olası <br />-Doğrudan Federasyon desteklenen | Azure AD kiracısı ile Federasyon aracılığıyla olası |
| Kişisel kullanım için Microsoft hesapları | Destekleniyor | Azure AD v2.0 OAuth protokolü aracılığıyla, ancak başka bir protokol üzerinden değil desteklenen | 
| Facebook, Google, Yahoo hesapları | Destekleniyor | Yoktur desteklenmiyor |
| **Protokoller ve SDK uyumluluk** | | |
| WIF | Destekleniyor | Desteklenen, ancak sınırlı yönergeler mevcuttur |
| WS-Federasyon | Destekleniyor | Destekleniyor |
| OAuth 2.0 | Taslak 13 desteği | RFC 6749, çoğu modern belirtimi için destek |
| WS-Trust | Destekleniyor | Desteklenmiyor |
| **Belirteç biçimleri** | | |
| JWT | Beta'de desteklenen | Destekleniyor |
| SAML 1.1 | Destekleniyor | Önizleme |
| SAML 2.0 | Destekleniyor | Destekleniyor |
| SWT | Destekleniyor | Desteklenmiyor |
| **Özelleştirmeleri** | | |
| Özelleştirilebilir giriş bölgesi bulma/hesap-çekme kullanıcı Arabirimi | Uygulamalarda birleştirilebilir indirilebilir kodu | Desteklenmiyor |
| Özel belirteç imzalama sertifikalarını karşıya yükleme | Destekleniyor | Destekleniyor |
| Belirteçleri Taleplerde özelleştirme |-Kimlik sağlayıcılardan gelen giriş talepleri geçirir<br />-Erişim belirteci kimlik sağlayıcısı'ndan talep olarak alma<br />-Çıkış talep giriş talepleri değerlerine göre vermek<br />-Sorun çıkış talep ile sabit değerleri |-Federe kimlik sağlayıcılardan gelen taleplere aracılığıyla geçiremezsiniz<br />-Erişim kimlik sağlayıcısından bir talep belirteci alınamıyor<br />-Çıkış talep giriş talepleri değerlerine göre veremez<br />-Çıktı talepler ile sabit değerleri verebilir<br />-Azure AD ile eşitlenen kullanıcılar özelliklerini göre çıkış talep verebilir |
| **Otomasyon** | | |
| Yapılandırma ve yönetim görevlerini otomatik hale getirme | Erişim denetimi yönetim hizmeti desteklenir | Desteklenen Microsoft Graph ve Azure AD grafik API'si |

Azure AD uygulamaları ve Hizmetleri için en iyi geçiş yolu olduğuna karar verirseniz, uygulamanızı Azure AD ile tümleştirmek için iki yol haberdar olmanız gerekir.

WS-Federasyon veya WIF Azure AD ile tümleştirmek üzere kullanmak için aşağıdaki yaklaşımı açıklanan öneririz [yapılandırma Federasyon tek oturum açma için Galeri olmayan uygulama](https://docs.microsoft.com/azure/active-directory/application-config-sso-how-to-configure-federated-sso-non-gallery). Makale, Azure AD SAML tabanlı çoklu oturum açma için yapılandırmaya yönelik başvuruyor, ancak WS-Federasyon yapılandırmak için de çalışır. Bu yaklaşımı izleyerek bir Azure AD Premium lisansı gerektirir. Bu yaklaşımın iki avantajı vardır:

- Azure AD belirteci özelleştirme tam esnekliğini alın. Erişim denetimi tarafından verilen talep eşleştirmek için Azure AD tarafından verilen talep özelleştirebilirsiniz. Bu özellikle kullanıcı kimliği veya adı tanımlayıcısı talebi içerir. Teknolojileri değiştirdikten sonra kullanıcılarınızın tutarlı bir kullanıcı tanımlayıcıları almaya devam etmek için kullanıcı kimlikleri ile Azure AD Eşle erişim denetimi tarafından verilenler verilen emin olun.
- Uygulamanıza ve denetim bir yaşam süresi ile belirli bir belirteç imzalama sertifikası yapılandırabilirsiniz.

<!--

Possible nameIdentifiers from Access Control (via AAD or AD FS):
- AD FS - Whatever AD FS is configured to send (email, UPN, employeeID, what have you)
- Default from AAD using App Registrations, or Custom Apps before ClaimsIssuance policy: subject/persistent ID
- Default from AAD using Custom apps nowadays: UPN
- Kusto can't tell us distribution, it's redacted

-->

> [!NOTE]
> Bu yaklaşım, bir Azure AD Premium lisansı gerektirir. Bir erişim denetimi müşterisiyseniz ve çoklu oturum açma bir uygulama için ayarlamak için premium lisansı gerektirir, bizimle iletişime geçin. Geliştirici lisansı kullanmanıza olanak sağlamak mutluluk olacaktır.

Alternatif bir yaklaşım izlemektir [Bu kod örneği](https://github.com/Azure-Samples/active-directory-dotnet-webapp-wsfederation), WS-Federasyon biraz farklı yönergeleri sağlar. Bu kod örneği, WIF, ancak bunun yerine, ASP.NET 4.5 OWIN ara yazılımı kullanmaz. Ancak, uygulama kayıt yönergelerini WIF kullanan uygulamalar için geçerlidir ve bir Azure AD Premium lisansı gerektirmez. 

Bu yaklaşım seçerseniz, anlamanız gerekir [anahtar geçişi Azure AD'de imzalama](https://docs.microsoft.com/azure/active-directory/develop/active-directory-signing-key-rollover). Bu yaklaşım imzalama anahtarı sorunu belirteçleri için genel Azure AD kullanır. Varsayılan olarak, WIF İmzalama anahtarları otomatik olarak yenilemez. Azure AD genel kendi İmzalama anahtarları döndürdüğünde WIF uygulamanızı değişiklikleri kabul etmek için hazırlanması gerekir.

Openıd Connect veya OAuth protokolleri aracılığıyla Azure AD ile tümleştirme, bunun yapılması önerilir. Kapsamlı belgeler ve uygulamanıza web bulunan Azure AD tümleştirme hakkında yönergeler sahibiz bizim [Azure AD Geliştirici Kılavuzu](http://aka.ms/aaddev).

<!-- TODO: If customers ask about authZ, let's put a blurb on role claims here -->

#### <a name="migrate-to-azure-active-directory-b2c"></a>Azure Active Directory B2C'ye geçirme

Dikkate alınması gereken diğer geçiş Azure AD B2C yoludur. Azure AD B2C, geliştiricilerin kendi kimlik doğrulama ve kimlik yönetimi mantığı bir bulut hizmeti için dış kaynaklara erişim denetimi gibi sağlayan bir bulut kimlik doğrulama hizmetidir. Etkin kullanıcılar en çok bir milyonlarca olabilir tüketiciye yönelik uygulamalar için tasarlanmış bir Ücretli (içeren ücretsiz ve premium Katmanlar) hizmetidir.

Erişim denetimi gibi Azure AD B2C'in en çekici özellikleri farklı türlerde hesaplarını destekleyen biridir. Azure AD B2C ile kullanıcıların Microsoft hesaplarını veya Facebook, Google, LinkedIn, GitHub veya Yahoo hesapları ve daha fazlasını kullanarak kullanıcıların imzalayabilirsiniz. Azure AD B2C, özellikle uygulamanız için "Yerel hesaplar," veya kullanıcı adı ve kullanıcı oluşturma parolaları de destekler. Azure AD B2C'de, oturum açma akışları özelleştirmek için kullanabileceğiniz Zengin genişletilebilirlik sağlar. 

Ancak, Azure AD B2C kimlik doğrulama protokolleri derecesini desteklemez ve müşteriler gerektirebilir bu erişim denetimi belirteci biçimlendirir. Kullanıcı hakkında ek bilgi için sorgu ve belirteç kimlik sağlayıcısından Microsoft almak için Azure AD B2C kullanamazsınız veya tersi durumda.

Aşağıdaki tabloda Azure AD B2C'de kullanılabilir olan web uygulamalarıyla ilgili erişim denetimi özellikleri karşılaştırılmaktadır. Yüksek bir düzeyde *Azure AD B2C büyük olasılıkla, geçiş için doğru seçim uygulamanız karşılıklı tüketici ise ya da farklı türlerde hesaplarını destekler.*

| Özellik | Erişim denetimi desteği | Azure AD B2C desteği |
| ---------- | ----------- | ---------------- |
| **Hesap türü** | | |
| Microsoft iş veya Okul hesapları | Destekleniyor | Özel ilkeler desteklenir  |
| Windows Server Active Directory ve AD FS hesaplarından | AD FS ile doğrudan Federasyon aracılığıyla desteklenir | SAML Federasyon özel ilkeler kullanılarak desteklenen |
| Diğer Kurumsal kimlik yönetimi sistemlerinden hesapları | WS-Federasyon aracılığıyla doğrudan Federasyon aracılığıyla desteklenir | SAML Federasyon özel ilkeler kullanılarak desteklenen |
| Kişisel kullanım için Microsoft hesapları | Destekleniyor | Destekleniyor | 
| Facebook, Google, Yahoo hesapları | Destekleniyor | Facebook ve Google, Yahoo Openıd Connect Federasyon özel ilkeler kullanılarak desteklenen desteklen |
| **Protokoller ve SDK uyumluluk** | | |
| Windows Identity Foundation (WIF) | Destekleniyor | Desteklenmiyor |
| WS-Federasyon | Destekleniyor | Desteklenmiyor |
| OAuth 2.0 | Taslak 13 desteği | RFC 6749, çoğu modern belirtimi için destek |
| WS-Trust | Destekleniyor | Desteklenmiyor |
| **Belirteç biçimleri** | | |
| JWT | Beta'de desteklenen | Destekleniyor |
| SAML 1.1 | Destekleniyor | Desteklenmiyor |
| SAML 2.0 | Destekleniyor | Desteklenmiyor |
| SWT | Destekleniyor | Desteklenmiyor |
| **Özelleştirmeleri** | | |
| Özelleştirilebilir giriş bölgesi bulma/hesap-çekme kullanıcı Arabirimi | Uygulamalarda birleştirilebilir indirilebilir kodu | Özel CSS aracılığıyla tam olarak özelleştirilebilir kullanıcı Arabirimi |
| Özel belirteç imzalama sertifikalarını karşıya yükleme | Destekleniyor | Özel İmzalama anahtarları özel ilkeler desteklenen, sertifikaları değil |
| Belirteçleri Taleplerde özelleştirme |-Kimlik sağlayıcılardan gelen giriş talepleri geçirir<br />-Erişim belirteci kimlik sağlayıcısı'ndan talep olarak alma<br />-Çıkış talep giriş talepleri değerlerine göre vermek<br />-Sorun çıkış talep ile sabit değerleri |-Kimlik sağlayıcılardan gelen taleplere geçebileceği; Bazı talepler için gereken özel ilkeler<br />-Erişim kimlik sağlayıcısından bir talep belirteci alınamıyor<br />-Çıkış talep giriş talepleri özel ilkeler aracılığıyla değerlerine dayalı verebilir<br />-Çıktı talepler özel ilkeler aracılığıyla sabit değerleri ile verebilir |
| **Otomasyon** | | |
| Yapılandırma ve yönetim görevlerini otomatik hale getirme | Erişim denetimi yönetim hizmeti desteklenir |-Azure AD Graph API üzerinden izin verilen kullanıcı oluşturma<br />-B2C Kiracı, uygulamaları veya ilkeleri program aracılığıyla oluşturulamıyor |

Azure AD B2C uygulamaları ve Hizmetleri için en iyi geçiş yolu olduğuna karar verirseniz, aşağıdaki kaynaklarla çalışmaya başlayın:

- [Azure AD B2C belgeleri](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview)
- [Azure AD B2C özel ilkeler](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview-custom)
- [Azure AD B2C fiyatlandırma](https://azure.microsoft.com/pricing/details/active-directory-b2c/)


#### <a name="migrate-to-ping-identity-or-auth0"></a>Ping kimliği veya Auth0 geçirme

Bazı durumlarda, Azure AD ve Azure AD bulabilirsiniz B2C ana kod değişiklik yapmadan erişim denetimi web uygulamalarınızda değiştirmek yeterli değil. Sık karşılaşılan örnekleri içerebilir:

- Google veya Facebook gibi sosyal kimlik sağlayıcıları ile oturum açma için WIF veya WS-Federasyon kullanan web uygulamaları.
- Bir kuruluş için doğrudan Federasyon gerçekleştirmek web uygulamaları WS-Federasyon protokolü üzerinden sağlayıcı tanımlar.
- Erişim denetimi tarafından yayınlanan belirteçleri, bir talep olarak sosyal kimlik sağlayıcısı (örneğin, Google veya Facebook) tarafından verilen erişim belirteci gerektiren uygulamalar web.
- Azure AD veya Azure AD B2C oluşturamadığı karmaşık belirteci dönüştürme kuralları ile Web uygulamaları.
- Federasyon birçok farklı kimlik sağlayıcısı için merkezi olarak yönetmek için ACS kullanan çok kiracılı web uygulamaları

Bu durumlarda, web uygulamanıza başka bir bulut kimlik doğrulama hizmeti geçirmeyi düşünün isteyebilirsiniz. Aşağıdaki seçenekler keşfetme öneririz. Aşağıdaki seçeneklerin her biri için erişim denetimi benzer özellikleri sunar:



|     |     | 
| --- | --- |
| ![Auth0](./media/active-directory-acs-migration/rsz_auth0.png) | [Auth0](https://auth0.com/acs) oluşturduğu bir esnek bulut kimlik hizmetidir [erişim denetimi müşterileri için üst düzey Geçiş Kılavuzu](https://auth0.com/acs)ve ACS mu neredeyse her özelliğini destekler. |
| ![Ping](./media/active-directory-acs-migration/rsz_ping.png) | [Ping kimlik](https://www.pingidentity.com) ACS benzer iki çözümler sunar. PingOne birçok ACS aynı özellikleri destekleyen bir bulut kimlik hizmetidir ve PingFederate daha fazla esneklik sunar benzer bir şirket içi kimlik ürünlerinden biridir. Başvurmak [Ping'ın ACS devre dışı bırakma Kılavuzu](https://www.pingidentity.com/company/blog/2017/11/20/migrating_from_microsoft_acs_to_ping_identity.html) bu ürünleri kullanma hakkında daha fazla ayrıntı için.  |

Ping kimlik ve Auth0 ile çalışırken bizim AIM tüm erişim denetimi müşteriler erişim denetiminden taşımak için gereken iş miktarını bir geçiş yolu uygulamaları ve Hizmetleri sahip olduğunuzdan emin olmaktır.

<!--

## Sharepoint 2010, 2013, 2016

TODO: Azure AD only, use Azure AD SAML 1.1 tokens, when we bring it back online.
Other IDPs: use Auth0? https://auth0.com/docs/integrations/sharepoint.

-->

### <a name="web-services-that-use-active-authentication"></a>Etkin kimlik doğrulaması kullanan web Hizmetleri

Erişim denetimi tarafından yayınlanan belirteçleri ile güvenliği sağlanan web hizmetleri için erişim denetimi aşağıdaki özellikleri ve yetenekleri sunar:

- Bir veya daha fazla kayıt yeteneği *hizmet kimliklerini* , erişim denetimi ad. Hizmet kimlikleri belirteçler istemek için kullanılır.
- OAuth kaydırma ve OAuth 2.0 taslak 13 belirteçleri, kimlik bilgilerini aşağıdaki türlerini kullanarak isteyen protokollerini destekler:
    - Hizmet kimliği için oluşturulan basit bir parola
    - Bir simetrik anahtar veya X509 kullanarak SWT imzalı sertifika
    - Güvenilen kimlik sağlayıcısı (genellikle bir AD FS örneği) tarafından verilen bir SAML belirteci
- Aşağıdaki belirteci biçimleri için destek: JWT, SAML 1.1, SAML 2.0 ve SWT.
- Basit belirteci dönüştürme kuralları.

Erişim denetimi hizmeti kimliklerini tipik sunucu sunucu kimlik doğrulaması uygulamak için kullanılır.  

#### <a name="migrate-to-azure-active-directory"></a>Azure Active Directory'ye geçirme

Bu tür bir kimlik doğrulama akışı için Bizim önerimiz geçirmek için olan [Azure Active Directory](https://azure.microsoft.com/develop/identity/signin/). Azure AD bulut tabanlı kimlik sağlayıcısı olduğu için Microsoft iş veya Okul hesapları. Azure AD kimlik sağlayıcısı için Office 365, Azure ve daha fazlasını ' dir. 

OAuth istemci kimlik bilgilerini verme Azure AD uygulaması kullanarak Azure AD sunucusuna sunucu kimlik doğrulaması için de kullanabilirsiniz. Aşağıdaki tabloda, sunucudan sunucuya kimlik doğrulaması ile Azure AD içinde kullanılabilir olan erişim denetimi özelliklerinde karşılaştırır.

| Özellik | Erişim denetimi desteği | Azure AD desteği |
| ---------- | ----------- | ---------------- |
| Bir web hizmetine kaydetme | Erişim denetimi Yönetim Portalı'nda bir bağlı olan taraf oluşturma | Azure portalında Azure AD web uygulaması oluşturma |
| Bir istemci kaydetme | Erişim denetimi Yönetim Portalı'nda bir hizmet kimliği oluşturma | Azure portalında başka bir Azure AD web uygulaması oluşturma |
| Kullanılan protokolü |-OAuth kaydırma Protokolü<br />-OAuth 2.0 taslak 13 istemci kimlik bilgileri verin | OAuth 2.0 istemci kimlik bilgileri verin |
| İstemci kimlik doğrulaması yöntemleri |-Basit parola<br />-İmzalı SWT<br />-Bir Federasyon kimlik sağlayıcısından SAML belirteci |-Basit parola<br />-İmzalı JWT |
| Belirteç biçimleri |-JWT<br />-SAML 1.1<br />-SAML 2.0<br />-SWT<br /> | JWT yalnızca |
| Belirteç dönüştürme |-Özel talep ekleme<br />-Basit IF then talep verme mantığı | Özel talep ekleme | 
| Yapılandırma ve yönetim görevlerini otomatik hale getirme | Erişim denetimi yönetim hizmeti desteklenir | Desteklenen Microsoft Graph ve Azure AD grafik API'si |

Uygulama sunucusu sunucusu senaryoları hakkında yönergeler için aşağıdaki kaynaklara bakın:

- Hizmetten hizmete bölümünü [Azure AD Geliştirici Kılavuzu](https://aka.ms/aaddev)
- [Basit parola istemci kimlik bilgilerini kullanarak arka plan kod örneği](https://github.com/Azure-Samples/active-directory-dotnet-daemon)
- [Sertifika istemci kimlik bilgilerini kullanarak arka plan kod örneği](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential)

#### <a name="migrate-to-ping-identity-or-auth0"></a>Ping kimliği veya Auth0 geçirme

Bazı durumlarda, Azure AD istemci kimlik ve OAuth uygulama Mimarinizi ana kod değişiklikleri olmadan erişim denetimi değiştirmek yeterli değil vermek bulabilirsiniz. Sık karşılaşılan örnekleri içerebilir:

- Sunucu-sunucu kimlik doğrulama belirteci kullanarak Jwt'ler dışında biçimlendirir.
- Bir dış kimlik sağlayıcısı tarafından sağlanan bir giriş belirteci kullanarak sunucu-sunucu kimlik doğrulaması.
- Azure AD oluşturamadığı belirteci dönüştürme kuralları ile sunucu-sunucu kimlik doğrulaması.

Bu durumlarda, web uygulamanıza başka bir bulut kimlik doğrulama hizmeti geçirme düşünebilirsiniz. Aşağıdaki seçenekler keşfetme öneririz. Aşağıdaki seçeneklerin her biri için erişim denetimi benzer özellikleri sunar:

|     |     | 
| --- | --- |
| ![Auth0](./media/active-directory-acs-migration/rsz_auth0.png) | [Auth0](https://auth0.com/acs) oluşturduğu bir esnek bulut kimlik hizmetidir [erişim denetimi müşterileri için üst düzey Geçiş Kılavuzu](https://auth0.com/acs)ve ACS mu neredeyse her özelliğini destekler. |
| ![Ping](./media/active-directory-acs-migration/rsz_ping.png) | [Ping kimlik](https://www.pingidentity.com) ACS benzer iki çözümler sunar. PingOne birçok ACS aynı özellikleri destekleyen bir bulut kimlik hizmetidir ve PingFederate daha fazla esneklik sunar benzer bir şirket içi kimlik ürünlerinden biridir. Başvurmak [Ping'ın ACS devre dışı bırakma Kılavuzu](https://www.pingidentity.com/company/blog/2017/11/20/migrating_from_microsoft_acs_to_ping_identity.html) bu ürünleri kullanma hakkında daha fazla ayrıntı için.  |

Ping kimlik ve Auth0 ile çalışırken bizim AIM tüm erişim denetimi müşteriler erişim denetiminden taşımak için gereken iş miktarını bir geçiş yolu uygulamaları ve Hizmetleri sahip olduğunuzdan emin olmaktır.

## <a name="questions-concerns-and-feedback"></a>Sorular, sorunları ve geri bildirim

Bu makaleyi okuduktan sonra çok sayıda erişim denetimi müşteriler açık bir geçiş yolu bulamaz anlayın. Bazı Yardım veya doğru planın belirlemede rehberlik gerekebilir. Geçiş senaryoları ve soruları ele istiyorsanız, lütfen bu sayfada bir yorum bırakın.
