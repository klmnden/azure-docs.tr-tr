---
title: Azure erişim denetimi Hizmeti'nden geçiş | Microsoft Docs
description: Uygulamaları ve Hizmetleri Azure Access Control Service (ACS) öğesinden taşıma seçenekleri hakkında bilgi edinin.
services: active-directory
documentationcenter: dev-center-name
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/03/2018
ms.author: celested
ms.reviewer: jlu, annaba, hirsin
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5f9fd062d445fb738842667cab0c24332c0e4cc8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60301082"
---
# <a name="how-to-migrate-from-the-azure-access-control-service"></a>Nasıl yapılır: Azure Access Control Service'ten geçiş yapma

Microsoft Azure Access Control Service (ACS), Azure Active Directory (Azure AD), bir hizmet 7 Kasım 2018'de kullanımdan kaldırılacaktır. Şu anda erişim denetimi kullanın, uygulamalar ve hizmetler için bir farklı kimlik doğrulama mekanizması tarafından daha sonra tam olarak geçirilmelidir. Bu makalede, erişim denetimi kullanımınız kullanımdan planladığınız geçerli müşteri önerileri açıklanmaktadır. Access Control şu anda kullanmazsanız, herhangi bir eylemde bulunmanız gerekmez.

## <a name="overview"></a>Genel Bakış

Erişim denetimi kimliğini doğrulamak ve web uygulamaları ve hizmetlerine erişim için kullanıcılara yetki vermek için kolay bir şekilde bir bulut kimlik hizmetidir. Kimlik doğrulaması ve yetkilendirme kodunuz dışında faktörlenebilen için birçok özellik sağlar. Erişim denetimi, öncelikle geliştiricilere ve mimarlara Microsoft .NET istemcileri, ASP.NET web uygulamaları ve Windows Communication Foundation (WCF) web hizmetleri tarafından kullanılır.

Erişim denetimi için kullanım örnekleri, üç ana kategoriye ayrılabilir:

- Azure Service Bus ve Dynamics CRM gibi belirli Microsoft bulut Hizmetleri, kimlik doğrulaması. İstemci uygulamaları, çeşitli eylemleri gerçekleştirmek için bu hizmetler için kimlik doğrulaması için erişim denetimi ile belirteçleri alın.
- Web uygulamaları, hem özel hem de önceden paketlenmiş (SharePoint gibi) kimlik doğrulaması ekleme. Erişim denetimi "pasif" kimlik doğrulaması kullanarak web uygulamaları oturum açma Microsoft hesabı (önceki adıyla Live ID) ve Google, Facebook, Yahoo, Azure AD hesapları destekleyebilir ve Active Directory Federasyon Hizmetleri (AD FS).
- Access Control tarafından verilen belirteçlere ile özel web hizmetleri güvenli hale getirme. "Active" kimlik doğrulaması'nı kullanarak web Hizmetleri, erişim denetimi ile kimlik doğrulamasını yalnızca bilinen istemciler için erişim sağlarlar sağlayabilirsiniz.

Bunların her biri kullanım örneklerinize ve stratejileri açıklanan, önerilen geçiş sağlanmaktadır.

> [!WARNING]
> Çoğu durumda, var olan uygulamalar ve hizmetler için yeni teknolojileri geçirmek için önemli kod değişikliği gerekir. Planlama hemen başlamanızı öneririz ve olası kesintileri veya kapalı kalma süresini önlemek için geçişiniz yürütülüyor.

Erişim denetimi aşağıdaki bileşenlere sahiptir:

- Kimlik doğrulama isteklerini alır ve buna karşılık olarak güvenlik belirteçleri bir güvenli belirteç Hizmeti'ne (STS).
- Oluşturduğunuz yerdir, Klasik Azure portalı, Sil ve etkinleştirme ve erişim denetimi ad alanları devre dışı.
- Özelleştirme ve erişim denetimi ad alanları yapılandırabileceğiniz bir ayrı erişim denetimi Yönetim Portalı.
- Portalları işlevlerini otomatik hale getirmek için kullanabileceğiniz bir yönetim hizmeti.
- Erişim denetimi sorunlarını belirteçleri çıkış biçimini değiştirmek için karmaşık mantığı tanımlamak için kullanabileceğiniz bir belirteç dönüştürme kuralı altyapısı.

Bu bileşenler kullanmak için bir veya daha fazla erişim denetimi ad alanı oluşturmanız gerekir. A *ad alanı* uygulamaları ve Hizmetleri ile iletişim kuran bir erişim denetimi adanmış örneğidir. Diğer tüm erişim denetimi müşterilerden yalıtılmış bir ad alanıdır. Access Control diğer müşterilerin kendi isim uzaylarını kullanın. Adanmış bir URL şuna benzer bir erişim denetimi ad alanı vardır:

```HTTP
https://<mynamespace>.accesscontrol.windows.net
```

Yönetim işlemleri ve STS ile tüm iletişim bu URL'de gerçekleştirilir. Farklı amaçlar için farklı yollar kullanırsınız. Uygulamalara veya hizmetlere erişim denetimi kullanıp kullanmadığını belirlemek için izleme https:// tüm trafik için&lt;ad alanı&gt;. accesscontrol.windows.net. Bu URL için tüm trafik, Access Control tarafından işlenir ve sona erecek şekilde gerekiyor. 

Tüm trafik için bunun özel durumu olan `https://accounts.accesscontrol.windows.net`. Bu URL'yi trafiği farklı bir hizmet tarafından zaten işlenmiş ve **değil** erişim denetimi kullanımdan kaldırma tarafından etkilenen. 

Erişim denetimi hakkında daha fazla bilgi için bkz. [erişim denetimi hizmeti (arşivlenmiş) 2.0](https://msdn.microsoft.com/library/hh147631.aspx).

## <a name="find-out-which-of-your-apps-will-be-impacted"></a>Hangi uygulamalarınızın etkilenecek kullanıma Bul

ACS emeklilik tarafından hangi uygulamalarınızın etkilenecek kullanıma bulmak için bu bölümdeki adımları izleyin.

### <a name="download-and-install-acs-powershell"></a>ACS PowerShell'i indirip yükleyin

1. Git PowerShell Galerisi'nden ve indirme [Acs.Namespaces](https://www.powershellgallery.com/packages/Acs.Namespaces/1.0.2).
1. Çalıştırarak modülünü yükleme

    ```powershell
    Install-Module -Name Acs.Namespaces
    ```

1. Çalıştırarak tüm olası komutların bir listesini alın

    ```powershell
    Get-Command -Module Acs.Namespaces
    ```

    Belirli bir komutla ilgili Yardım almak için şunu çalıştırın:

    ```
     Get-Help [Command-Name] -Full
    ```
    
    Burada `[Command-Name]` ACS komut adıdır.

### <a name="list-your-acs-namespaces"></a>ACS ad alanları listesi

1. ACS kullanarak bağlan **Connect AcsAccount** cmdlet'i.
  
    Çalıştırmanız gerekebilir `Set-ExecutionPolicy -ExecutionPolicy Bypass` komutları yürütmeden önce ve komutları yürütmek için bu Aboneliklerdeki yönetici olabilir.

1. Kullanarak mevcut Azure aboneliklerinizi listesinde **Get-AcsSubscription** cmdlet'i.
1. ACS kullanarak ad alanları listesi **Get-AcsNamespace** cmdlet'i.

### <a name="check-which-applications-will-be-impacted"></a>Hangi uygulamaların etkilenecek denetleyin

1. Önceki adımdan gelen ad alanı kullanın ve Git `https://<namespace>.accesscontrol.windows.net`

    Örneğin, Git ad alanlarından biri contoso-test varsa, `https://contoso-test.accesscontrol.windows.net`

1. Altında **güven ilişkileri**seçin **bağlı olan taraf uygulamaları** ACS emeklilik tarafından etkilenen uygulamaları listesini görmek için.
1. Sahip olduğunuz tüm diğer ACS namespace (s) için 1 ve 2. adımları tekrarlayın.

## <a name="retirement-schedule"></a>Kullanımdan kaldırma zamanlaması

Kasım 2017'den itibaren tamamen desteklenen ve işletimsel tüm erişim denetimi bileşenleri şunlardır. Olan tek kısıtlama, [Klasik Azure portalı ile yeni erişim denetimi ad alanları oluşturulamıyor](https://azure.microsoft.com/blog/acs-access-control-service-namespace-creation-restriction/).

Erişim denetimi bileşenleri kullanımdan zamanlamasını şu şekildedir:

- **Kasım 2017**:  Klasik Azure portalında Azure AD Yöneticisi deneyimi [kullanımdan kaldırıldığında](https://blogs.technet.microsoft.com/enterprisemobility/2017/09/18/marching-into-the-future-of-the-azure-ad-admin-experience-retiring-the-azure-classic-portal/). Bu noktada, erişim denetimi ad alanı yönetimi yeni ve ayrılmış bir URL'de kullanılabilir: `https://manage.windowsazure.com?restoreClassic=true`. İsterseniz bu URl, var olan ad alanları görüntülemek, etkinleştirmek ve ad alanları devre dışı bırakma ve ad alanları, silmek için kullanın.
- **2 Nisan 2018**: Klasik Azure portalında tamamen kullanımdan erişim denetimi ad alanı yönetimi artık herhangi bir URL kullanılabilir anlamına gelir. Bu noktada, devre dışı bırakmak veya etkinleştirmek, silemez veya erişim denetimi ad alanlarınıza listeleme. Erişim denetimi Yönetim Portalı ve tam olarak işlevsel konumunda bulunan ancak olacaktır `https://\<namespace\>.accesscontrol.windows.net`. Erişim denetimi tüm diğer bileşenleri normal şekilde çalışmaya devam eder.
- **7 Kasım 2018'den**: Tüm erişim denetimi bileşenleri kalıcı olarak kapatıldı. Bu, erişim denetimi Yönetim Portalı, yönetim hizmeti, STS'ye ve belirteç dönüştürme kuralı altyapısı içerir. Bu noktada, erişim denetimi için gönderilen tüm istekler (konumundaki \<ad alanı\>. accesscontrol.windows.net) başarısız. Var olan tüm uygulamaları ve Hizmetleri için diğer teknolojiler de bu süreden önce geçirdiğiniz.

> [!NOTE]
> Bir ilke bir süre için bir belirteç istediniz olmayan ad alanlarını devre dışı bırakır. Erken Eylül 2018'den itibaren bu süre şu anda 14 günlük etkin olmama süresi, ancak bu, gelecek haftalarda yapılmadığında 7 gün için kısaltılacak. Şu anda devre dışı bırakılmış bir erişim denetimi ad alanları varsa [ACS PowerShell'i indirip yükleyin](#download-and-install-acs-powershell) namespace (s) yeniden etkinleştirebilirsiniz.

## <a name="migration-strategies"></a>Geçiş stratejileri

Aşağıdaki bölümlerde, diğer Microsoft teknolojileri için erişim denetiminden geçirmek için üst düzey önerileri açıklanmaktadır.

### <a name="clients-of-microsoft-cloud-services"></a>İstemcileri Microsoft bulut Hizmetleri

Access Control tarafından artık verilen belirteçleri kabul eden her bir Microsoft bulut hizmeti, en az bir alternatif kimlik doğrulama biçimi destekler. Her hizmet için doğru kimlik doğrulama mekanizması değişir. Resmi rehberlik için her bir hizmet için belirli belgelere başvurmak öneririz. Kolaylık olması için her bir belge kümesi verilmiştir:

| Hizmet | Rehber |
| ------- | -------- |
| Azure Service Bus | [Paylaşılan erişim imzaları geçirme](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-migrate-acs-sas) |
| Azure Service Bus geçişi | [Paylaşılan erişim imzaları geçirme](https://docs.microsoft.com/azure/service-bus-relay/relay-migrate-acs-sas) |
| Azure yönetilen önbellek | [Azure önbelleği için Redis için geçirme](https://docs.microsoft.com/azure/azure-cache-for-redis/cache-faq#which-azure-cache-offering-is-right-for-me) |
| Azure DataMarket | [Bilişsel hizmetler API'lerine geçiş](https://docs.microsoft.com/azure/machine-learning/studio/datamarket-deprecation) |
| BizTalk Services | [Azure App Service'in Logic Apps özelliği için geçirme](https://docs.microsoft.com/azure/machine-learning/studio/datamarket-deprecation) |
| Azure Media Services | [Azure AD kimlik doğrulamaya geçiş](https://azure.microsoft.com/blog/azure-media-service-aad-auth-and-acs-deprecation/) |
| Azure Backup | [Azure Backup Aracısı'nı yükseltme](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq) |

<!-- Dynamics CRM: Migrate to new SDK, Dynamics team handling privately -->
<!-- Azure RemoteApp deprecated in favor of Citrix: https://www.zdnet.com/article/microsoft-to-drop-azure-remoteapp-in-favor-of-citrix-remoting-technologies/ -->
<!-- Exchange push notifications are moving, customers don't need to move -->
<!-- Retail federation services are moving, customers don't need to move -->
<!-- Azure StorSimple: TODO -->
<!-- Azure SiteRecovery: TODO -->

### <a name="sharepoint-customers"></a>SharePoint müşteriler

SharePoint 2013, 2016 ve SharePoint Online müşterilerine uzun ACS kimlik doğrulaması amacıyla kullanılan bulut, şirket içi ve karma senaryoları. Bazıları taşınmaz sırasında bazı SharePoint özelliklerini ve kullanım örnekleri tarafından ACS devre dışı bırakılması, etkilenecek. En popüler SharePoint bazıları bu Dengeleme ACS özellik için Geçiş Kılavuzu aşağıdaki tabloda özetlenmiştir:

| Özellik | Rehber |
| ------- | -------- |
| Azure ad kullanıcıların kimliklerinin doğrulanması | Daha önce Azure AD kimlik doğrulaması için SharePoint'e gereken SAML 1.1 belirteçleri desteklemiyor ve ACS SharePoint ile Azure AD belirteç biçimlerini uyumlu olarak yapılan bir aracı olarak kullanıldı. Artık, [SharePoint şirket içi uygulama Azure AD uygulama galerisinde SharePoint kullanarak doğrudan Azure AD'ye bağlama](https://docs.microsoft.com/azure/active-directory/saas-apps/sharepoint-on-premises-tutorial). |
| [Uygulama kimlik doğrulaması ve SharePoint şirket içi sunucudan sunucuya kimlik doğrulaması](https://technet.microsoft.com/library/jj219571(v=office.16).aspx) | ACS emeklilik tarafından etkilenen değil; Gerekli değişiklik yok. | 
| [SharePoint eklentileri için (sağlayıcı tarafından barındırılan ve barındırılan SharePoint) yetkilendirme düşük güven](https://docs.microsoft.com/sharepoint/dev/sp-add-ins/three-authorization-systems-for-sharepoint-add-ins) | ACS emeklilik tarafından etkilenen değil; Gerekli değişiklik yok. |
| [SharePoint bulut hibrit arama](https://blogs.msdn.microsoft.com/spses/2015/09/15/cloud-hybrid-search-service-application/) | ACS emeklilik tarafından etkilenen değil; Gerekli değişiklik yok. |

### <a name="web-applications-that-use-passive-authentication"></a>Pasif kimlik doğrulama kullanan web uygulamaları

Kullanıcı kimlik doğrulaması için erişim denetimi kullanan web uygulamaları için web uygulaması geliştiricileri ve mimarları için aşağıdaki özellikleri ve yetenekleri erişim denetimi sağlar:

- Derin tümleştirme Windows Identity Foundation (WIF).
- Google, Facebook, Yahoo, Azure Active Directory ve AD FS hesapları ve Microsoft hesapları ile Federasyon.
- Aşağıdaki kimlik doğrulama protokolleri için destek: OAuth 2.0 taslak 13, WS-Güven ve Web Hizmetleri Federasyonu (WS-Federation).
- Aşağıdaki belirteci biçimleri için destek: JSON Web Token (JWT), SAML 1.1, SAML 2.0 ve basit Web belirteci (SWT).
- WIF tümleşik bir giriş bölgesi bulma deneyimini, kullanıcıların oturum açarken kullandığınız hesap türünü seçin olanak tanır. Bu deneyim web uygulaması tarafından barındırılıyor ve tam olarak özelleştirilebilir.
- Belirteç Zengin özelleştirme erişim denetiminden web uygulaması tarafından alınan Talep veren dönüştürme de dahil olmak üzere:
    - Kimlik sağlayıcıları gelen talepleri geçirir.
    - Ek özel talep ekleniyor.
    - Belirli koşullar altında talepleri basit if-then mantığını.

Ne yazık ki, tüm bu eşdeğer özellikler sunan bir hizmet yoktur. Hangi özellikleri erişim denetimi, gereksinim ve ardından kullanma arasında seçim değerlendirmelidir [Azure Active Directory](https://azure.microsoft.com/develop/identity/signin/), [Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) (Azure AD B2C) veya başka bir bulut kimlik doğrulaması hizmeti.

#### <a name="migrate-to-azure-active-directory"></a>Azure Active Directory'ye geçirme

Dikkate alınması gereken bir yolu, uygulamalarınız ve hizmetleriniz doğrudan Azure AD ile tümleştirme. Azure AD, bulut tabanlı kimlik sağlayıcısı için Microsoft iş veya Okul hesapları. Azure AD, Office 365, Azure ve çok daha fazlası için kimlik sağlayıcıdır. Benzer sağlar federe kimlik doğrulama özellikleri için erişim denetimi, ancak tüm erişim denetimi özellikleri desteklemiyor. 

Birincil Federasyon Facebook, Google ve Yahoo gibi sosyal kimlik sağlayıcıları ile örnektir. Kullanıcılarınızın bu tür bir kimlik bilgileri ile oturum açarsanız, Azure AD, çözüm değildir. 

Azure AD erişim denetimi olarak tam olarak aynı kimlik doğrulama protokolleri mutlaka desteklemiyor. Örneğin, erişim denetimi hem de Azure AD OAuth desteklese de, her uygulama arasındaki farklar vardır. Farklı uygulamaları bir geçişin parçası olarak kod değiştirmenizi gerektirir.

Ancak, Azure AD Access Control müşterilere çeşitli olası avantajları sunar. Yerel olarak destekleyen Microsoft iş veya Okul erişim denetimi müşteriler tarafından yaygın olarak kullanılan, bulutta barındırılan hesapları. 

Azure AD kiracısı ayrıca bir veya birden fazla örneğini AD FS aracılığıyla şirket içi Active Directory Federasyon. Bu şekilde, uygulamanız kullanıcıların bulut tabanlı kimlik doğrulaması yapabilir ve kullanıcılar şirket içinde barındırılan. Ayrıca, WIF kullanarak bir web uygulamasıyla tümleştirmek oldukça basit hale getirir WS-Federasyon protokolünü destekler.

Aşağıdaki tabloda, Azure AD'de kullanılabilen bu özellikler web uygulamalarıyla ilgili erişim denetimi özelliklerinin karşılaştırır. 

Yüksek bir düzeyde *Azure Active Directory büyük olasılıkla en iyi seçenek için geçişiniz kullanıcıların izin verirseniz, yalnızca kendi Microsoft ile iş veya Okul hesaplarını*.

| Özellik | Erişim denetimi desteği | Azure AD desteği |
| ---------- | ----------- | ---------------- |
| **Hesap türü** | | |
| Microsoft iş veya Okul hesapları | Desteklenen | Desteklenen |
| Windows Server Active Directory ve AD FS hesaplarından |-Azure AD kiracısı ile Federasyon aracılığıyla desteklenir <br />-AD FS ile doğrudan Federasyon aracılığıyla desteklenir | Yalnızca Azure AD kiracısı ile Federasyon aracılığıyla desteklenir | 
| Diğer kuruluş Kimlik Yönetimi sistemlerinde hesaplarından |-Olası aracılığıyla Azure AD kiracısı ile Federasyon <br />-Doğrudan Federasyon desteklenir | Olası aracılığıyla Azure AD kiracısı ile Federasyon |
| Microsoft hesapları kişisel kullanım için | Desteklenen | Azure AD v2.0 OAuth protokolünün aracılığıyla, ancak başka bir protokol üzerinden değil desteklenir | 
| Facebook, Google, Yahoo hesabı | Desteklenen | Vermemektedir desteklenmiyor |
| **Protokoller ve SDK uyumluluk** | | |
| WIF | Desteklenen | Desteklenir, ancak sınırlı yönergeleri şurada bulabilirsiniz: |
| WS-Federation | Desteklenen | Desteklenen |
| OAuth 2.0 | Taslak 13 desteği | RFC 6749, çoğu modern belirtimi için destek |
| WS-Güven | Desteklenen | Desteklenmiyor |
| **Belirteç biçimleri** | | |
| JWT | Beta sürümünde desteklenir | Desteklenen |
| SAML 1.1 | Desteklenen | Önizleme |
| SAML 2.0 | Desteklenen | Desteklenen |
| SWT | Desteklenen | Desteklenmiyor |
| **Özelleştirmeleri** | | |
| Özelleştirilebilir giriş bölgesi bulma/hesap-çekme kullanıcı Arabirimi | Uygulamalara dahil edilebilir indirilebilir kod | Desteklenmiyor |
| Özel belirteç imzalama sertifikalarını karşıya yükleme | Desteklenen | Desteklenen |
| Belirteçlere talep özelleştirme |-Kimlik sağlayıcılardan gelen giriş talepleri geçirir<br />-Erişim belirteci kimlik sağlayıcısından gelen talep olarak alma<br />-Çıkış talep giriş talepleri değerlerine dayalı sorunu<br />-Sabit değerler ile çıkış talep verme |-Federe kimlik sağlayıcılardan gelen talep geçişine olamaz<br />-Erişim kimlik sağlayıcısından gelen talep olarak belirteci alınamıyor<br />-Çıkış talep giriş talepleri değerlerine dayalı veremez<br />-Sabit değerler ile çıkış talebi<br />-Azure AD'ye eşitlenmiş kullanıcılar özelliklerine göre çıkış talepler verebilir |
| **Otomasyon** | | |
| Yapılandırma ve yönetim görevlerini otomatikleştirin | Erişim denetimi yönetim hizmeti desteklenir | Microsoft Graph ve Azure AD Graph API desteklenen |

Azure AD uygulamaları ve Hizmetleri için en iyi geçiş yolu olduğuna karar verirseniz, uygulamanızı Azure AD ile tümleştirmek için iki şekilde haberdar olmanız gerekir.

Azure AD ile tümleştirmek için WS-Federation veya WIF kullanmak için aşağıdaki yaklaşımı açıklanan önerilir [Federasyon çoklu oturum açma galeri dışı bir uygulama yapılandırma](https://docs.microsoft.com/azure/active-directory/application-config-sso-how-to-configure-federated-sso-non-gallery). Makalede, Azure AD SAML tabanlı çoklu oturum açma için yapılandırmaya yönelik başvuruyor, ancak WS-Federasyon yapılandırmak için de çalışır. Bu yaklaşım, bir Azure AD Premium lisansı gerektirir. Bu yaklaşımın iki avantajları vardır:

- Azure AD belirteç özelleştirme tam esnekliğe sahip olursunuz. Azure AD Access Control tarafından verilen taleplere eşleşecek şekilde tarafından verilen talepleri özelleştirebilirsiniz. Bu özellikle, kullanıcı kimliği veya adı tanımlayıcısı talebi içerir. Teknolojileri değiştirdikten sonra kullanıcılarınız için tutarlı kullanıcı tanımlayıcılarını almaya devam etmek için kullanıcı kimliklerini Azure AD'ye eşleşme tarafından bu erişim denetimi tarafından verilen verilen emin olun.
- Sizin denetlediğiniz bir yaşam süresi ve uygulamanıza özgü bir belirteç imzalama sertifikası yapılandırabilirsiniz.

> [!NOTE]
> Bu yaklaşım, bir Azure AD Premium lisansı gerektirir. Bir erişim denetimi müşterisi olduğunuz ve çoklu oturum açma için bir uygulama ayarlamak için bir premium Lisansı gerektiren varsa, bizimle iletişime geçin. Geliştirici lisansı kullanabilmeniz için konusunda seve olacaktır.

Alternatif bir yaklaşım izlemektir [Bu kod örneği](https://github.com/Azure-Samples/active-directory-dotnet-webapp-wsfederation), WS-Federation ayarlamak için biraz daha farklı yönergeler sağlar. Bu kod örneği, WIF, ancak bunun yerine, ASP.NET 4.5 OWIN ara yazılımı kullanmaz. Ancak, uygulama kaydı yönergelerini WIF kullanarak uygulamalar için geçerlidir ve Azure AD Premium lisansı gerekmez. 

Bu yaklaşım tercih ederseniz anlamanız gerekir [Azure AD'de imzalama anahtarı geçiş işlemi](https://docs.microsoft.com/azure/active-directory/develop/active-directory-signing-key-rollover). Bu yaklaşım, imzalama anahtarı sorunu belirteçleri genel Azure AD kullanır. Varsayılan olarak, WIF İmzalama anahtarları otomatik olarak yenilenmez. Azure AD genel, imzalama anahtarları döndürdüğünde, WIF uygulamanız değişiklikleri kabul etmek için hazırlanması gerekir. Daha fazla bilgi için [Azure AD'de imzalama anahtarı geçiş işlemi hakkında önemli bilgiler](https://msdn.microsoft.com/library/azure/dn641920.aspx).

Openıd Connect veya OAuth protokolleri üzerinden Azure ad'yle tümleştirebilirsiniz varsa bunu öneririz. Kapsamlı belgeler ve Azure AD web uygulamanızı kullanılabilir tümleştirin konusunda rehberlik sunuyoruz bizim [Azure AD Geliştirici Kılavuzu](https://aka.ms/aaddev).

#### <a name="migrate-to-azure-active-directory-b2c"></a>Azure Active Directory B2C'ye geçirme

Dikkate alınması gereken diğer geçiş yolu, Azure AD B2C ' dir. Azure AD B2C, erişim denetimi gibi bir bulut hizmeti için kendi kimlik doğrulaması ve kimlik yönetimi mantığı dış geliştiricilerin olanak veren bir bulut kimlik hizmetidir. Bu kadar milyonlarca etkin kullanıcıların yaşayabileceği tüketiciye yönelik uygulamalar için tasarlanmış bir Ücretli (olan ücretsiz ve premium Katmanlar) hizmetidir.

Erişim denetimi gibi Azure AD B2C'in en cazip özellikler çok sayıda farklı hesap türlerinin desteklediğini biridir. Azure AD B2C ile kullanıcılar, Microsoft hesabı ya da Facebook, Google, LinkedIn, GitHub veya Yahoo hesapları ve daha fazlasını kullanarak kaydolabilirsiniz. Azure AD B2C, uygulamanız için özel olarak "Yerel hesaplar," veya kullanıcı adı ve kullanıcıların parolaları da destekler. Azure AD B2C, ayrıca oturum açma akışlarınızda özelleştirmek için kullanabileceğiniz Zengin genişletilebilirlik sağlar. 

Ancak, Azure AD B2C kimlik doğrulama protokolleri kapsamını desteklemiyor ve bu erişim denetimini müşteriler gerektirebilir belirteci biçimlendirir. Kullanıcı hakkında ek bilgi için belirteçleri ve sorgu Microsoft Kimlik sağlayıcısı'ndan almak için Azure AD B2C'yi kullanamazsınız veya tersi durumda.

Aşağıdaki tabloda, Azure AD B2C'de kullanılabilir olanlara web uygulamalarıyla ilgili erişim denetimi özelliklerinin karşılaştırır. Yüksek bir düzeyde *Azure AD B2C büyük olasılıkla doğru seçim için geçişiniz uygulamanız tüketiciye ise ya da çok sayıda farklı hesap türlerinin destekler.*

| Özellik | Erişim denetimi desteği | Azure AD B2C desteği |
| ---------- | ----------- | ---------------- |
| **Hesap türü** | | |
| Microsoft iş veya Okul hesapları | Desteklenen | Özel ilkeler desteklenir  |
| Windows Server Active Directory ve AD FS hesaplarından | AD FS ile doğrudan Federasyon aracılığıyla desteklenir | Özel ilkeler kullanarak SAML Federasyon desteklenir. |
| Diğer kuruluş Kimlik Yönetimi sistemlerinde hesaplarından | WS-Federation aracılığıyla doğrudan Federasyon aracılığıyla desteklenir | Özel ilkeler kullanarak SAML Federasyon desteklenir. |
| Microsoft hesapları kişisel kullanım için | Desteklenen | Desteklenen | 
| Facebook, Google, Yahoo hesabı | Desteklenen | Facebook ve Google, Yahoo Openıd Connect Federasyon özel ilkeler kullanarak desteklenen desteklen |
| **Protokoller ve SDK uyumluluk** | | |
| Windows Identity Foundation (WIF) | Desteklenen | Desteklenmiyor |
| WS-Federation | Desteklenen | Desteklenmiyor |
| OAuth 2.0 | Taslak 13 desteği | RFC 6749, çoğu modern belirtimi için destek |
| WS-Güven | Desteklenen | Desteklenmiyor |
| **Belirteç biçimleri** | | |
| JWT | Beta sürümünde desteklenir | Desteklenen |
| SAML 1.1 | Desteklenen | Desteklenmiyor |
| SAML 2.0 | Desteklenen | Desteklenmiyor |
| SWT | Desteklenen | Desteklenmiyor |
| **Özelleştirmeleri** | | |
| Özelleştirilebilir giriş bölgesi bulma/hesap-çekme kullanıcı Arabirimi | Uygulamalara dahil edilebilir indirilebilir kod | Özel CSS aracılığıyla tamamen özelleştirilebilir kullanıcı Arabirimi |
| Özel belirteç imzalama sertifikalarını karşıya yükleme | Desteklenen | Özel İmzalama anahtarları özel ilkeleri desteklenen, olmayan sertifikalar |
| Belirteçlere talep özelleştirme |-Kimlik sağlayıcılardan gelen giriş talepleri geçirir<br />-Erişim belirteci kimlik sağlayıcısından gelen talep olarak alma<br />-Çıkış talep giriş talepleri değerlerine dayalı sorunu<br />-Sabit değerler ile çıkış talep verme |-Talep kimlik sağlayıcılardan gelen geçebileceği; Bazı talep için gereken özel ilkeler<br />-Erişim kimlik sağlayıcısından gelen talep olarak belirteci alınamıyor<br />-Çıkış talep giriş talepleri özel ilkeler aracılığıyla değerlere göre verebilir<br />-Çıkış talep sabit değerler özel ilkeler aracılığıyla ile verebilir |
| **Otomasyon** | | |
| Yapılandırma ve yönetim görevlerini otomatikleştirin | Erişim denetimi yönetim hizmeti desteklenir |-Azure AD Graph API'si ile verilen kullanıcılar oluşturma<br />-B2C kiracıları, uygulamaları veya ilkeleri programlı olarak oluşturulamıyor |

Azure AD B2C uygulamaları ve Hizmetleri için en iyi geçiş yolu olduğuna karar verirseniz, aşağıdaki kaynaklarla başlayın:

- [Azure AD B2C belgeleri](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview)
- [Azure AD B2C özel ilkeler](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview-custom)
- [Azure AD B2C fiyatlandırması](https://azure.microsoft.com/pricing/details/active-directory-b2c/)

#### <a name="migrate-to-ping-identity-or-auth0"></a>Ping Identity veya Auth0 geçirme

Bazı durumlarda, Azure AD ile Azure AD bulabilirsiniz B2C erişim denetimi, web uygulamalarınızda büyük kod değişikliği yapmadan değiştirmek yeterli değildir. Bazı genel örnekleri içerebilir:

- Google veya Facebook gibi sosyal kimlik sağlayıcıları ile oturum açma için WIF veya WS-Federasyon kullanan web uygulamaları.
- Bir Kurumsal kimlik sağlayıcısı için doğrudan Federasyon gerçekleştirmek WS-Federation protokolü üzerinden web uygulamaları.
- Access Control tarafından verilen belirteçlere talep olarak sosyal kimlik sağlayıcısı (örneğin, Google veya Facebook gibi) tarafından verilen erişim belirteci gerektiren uygulamalar web.
- Azure AD veya Azure AD B2C üretemeyebilir karmaşık belirteç dönüştürme kuralları ile Web uygulamaları.
- Birçok farklı kimlik sağlayıcıları için Federasyon merkezi olarak yönetmek için ACS kullanan çok kiracılı web uygulamaları

Bu gibi durumlarda, web uygulamanızın başka bir bulut kimlik doğrulaması hizmetine geçirmeyi düşünün isteyebilirsiniz. Aşağıdaki seçenekleri keşfetmeye öneririz. Aşağıdaki seçeneklerin her biri benzer erişim denetimi özellikleri sağlar:

|     |     | 
| --- | --- |
| ![Auth0](./media/active-directory-acs-migration/rsz_auth0.png) | [Auth0](https://auth0.com/acs) oluşturduğu bir esnek bir bulut kimlik hizmetidir [müşteriler erişim denetimi için üst düzey geçiş kılavuzunu](https://auth0.com/acs)ve ACS yapan neredeyse her bir özelliği destekler. |
| ![Ping](./media/active-directory-acs-migration/rsz_ping.png) | [Ping Identity](https://www.pingidentity.com) ACS'ye benzer iki çözümler sunar. PingOne ACS olarak aynı özelliklerin çoğunu destekleyen bir bulut kimlik hizmetidir ve PingFederate bir şirket içi kimlik daha fazla esneklik sunan bir ürün üzerinde benzer. Başvurmak [Ping'ın ACS emeklilik Kılavuzu](https://www.pingidentity.com/en/company/blog/2017/11/20/migrating_from_microsoft_acs_to_ping_identity.html) bu ürünleri kullanma hakkında daha fazla bilgi. |

Bizim AIM Auth0 Ping Identity ile çalışırken, tüm erişim denetimi müşterilerin erişim denetiminden taşımak için gerekli çalışma miktarını azaltan bir geçiş yolu, uygulamalar ve hizmetler için sahip olduğundan emin olmaktır.

<!--

## SharePoint 2010, 2013, 2016

TODO: Azure AD only, use Azure AD SAML 1.1 tokens, when we bring it back online.
Other IDPs: use Auth0? https://auth0.com/docs/integrations/sharepoint.

-->

### <a name="web-services-that-use-active-authentication"></a>Etkin kimlik doğrulama kullanan web Hizmetleri

Access Control tarafından verilen belirteçleri ile güvenliği sağlanan web hizmetleri için erişim denetimi, aşağıdaki özellikleri ve yetenekleri sunar:

- Bir veya daha fazla kaydetme olanağı *hizmet kimliklerini* erişim denetimi ad alanınızdaki. Hizmet kimlikleri, belirteçler istemek için kullanılabilir.
- OAuth SARMALAYIN ve OAuth 2.0 taslak 13 belirteçleri, aşağıdaki türde kimlik bilgilerini kullanarak istemeye ilişkin protokollerini destekler:
    - Hizmet kimliği için oluşturulan basit parolalar
    - Simetrik anahtar veya X509 kullanarak SWT imzalı bir sertifika
    - Güvenilen kimlik sağlayıcı (genellikle, AD FS örneği) tarafından verilen SAML belirteci
- Aşağıdaki belirteci biçimleri için destek: JWT, SAML 1.1, SAML 2.0 ve SWT.
- Basit bir belirteç dönüştürme kuralları.

Hizmet kimliği erişim denetimi, genellikle sunucudan sunucuya kimlik doğrulaması uygulamak için kullanılır. 

#### <a name="migrate-to-azure-active-directory"></a>Azure Active Directory'ye geçirme

Bu tür bir kimlik doğrulama akışı için tavsiyemiz geçirmek için olan [Azure Active Directory](https://azure.microsoft.com/develop/identity/signin/). Azure AD, bulut tabanlı kimlik sağlayıcısı için Microsoft iş veya Okul hesapları. Azure AD, Office 365, Azure ve çok daha fazlası için kimlik sağlayıcıdır. 

Ayrıca, Azure AD OAuth istemci kimlik bilgileri verme Azure AD uygulaması kullanarak sunucudan sunucuya kimlik doğrulaması için kullanabilirsiniz. Aşağıdaki tabloda, sunucudan sunucuya kimlik doğrulaması ile Azure AD'de kullanılabilir olanlara erişim denetimi özellikleri karşılaştırılmaktadır.

| Özellik | Erişim denetimi desteği | Azure AD desteği |
| ---------- | ----------- | ---------------- |
| Bir web hizmetine kaydetme | Bir bağlı olan tarafa erişim denetimi Yönetim Portalı'nda oluşturma | Azure portalında bir Azure AD web uygulaması oluşturma |
| Bir istemci kaydetme | Erişim denetimi Yönetim Portalı'nda hizmet kimliği oluşturma | Azure portalını kullanarak başka bir Azure AD web uygulaması oluşturma |
| Kullanılan protokolü |-OAuth kaydırma Protokolü<br />-OAuth 2.0 taslak 13 istemci kimlik bilgileri verme | OAuth 2.0 istemci kimlik bilgileri verme |
| İstemci kimlik doğrulaması yöntemleri |-Basit parola<br />-İmzalı SWT<br />-Federe kimlik sağlayıcısından gelen SAML belirteci |-Basit parola<br />-İmzalı JWT |
| Belirteç biçimleri |- JWT<br />- SAML 1.1<br />- SAML 2.0<br />-SWT<br /> | Yalnızca JWT |
| Belirteç dönüştürme |-Özel talep ekleme<br />-İf-then basit talep verme mantığı | Özel talep ekleme | 
| Yapılandırma ve yönetim görevlerini otomatikleştirin | Erişim denetimi yönetim hizmeti desteklenir | Microsoft Graph ve Azure AD Graph API desteklenen |

Uygulama sunucudan sunucuya senaryolar hakkında yönergeler için aşağıdaki kaynaklara bakın:

- Hizmetten hizmete bölümünü [Azure AD Geliştirici Kılavuzu](https://aka.ms/aaddev)
- [Basit parolalar istemci kimlik bilgilerini kullanarak arka plan kod örneği](https://github.com/Azure-Samples/active-directory-dotnet-daemon)
- [Sertifika istemci kimlik bilgilerini kullanarak arka plan kod örneği](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential)

#### <a name="migrate-to-ping-identity-or-auth0"></a>Ping Identity veya Auth0 geçirme

Bazı durumlarda, Azure AD istemci kimlik bilgileri ve OAuth uygulama erişim denetimi mimarinizdeki ana kod değişikliği yapmadan değiştirmek yeterli değildir vermenizi bulabilirsiniz. Bazı genel örnekleri içerebilir:

- Sunucudan sunucuya kimlik doğrulaması belirteci kullanarak Jwt'ler dışında biçimlendirir.
- Bir dış kimlik sağlayıcısı tarafından sağlanan bir giriş belirteci kullanarak sunucudan sunucuya kimlik doğrulaması.
- Sunucudan sunucuya kimlik doğrulaması belirteci dönüştürme kurallarıyla Azure AD yeniden oluşturulamıyor.

Bu durumlarda, başka bir bulut kimlik doğrulama hizmeti, web uygulamanızı geçişi göz önünde bulundurabilirsiniz. Aşağıdaki seçenekleri keşfetmeye öneririz. Aşağıdaki seçeneklerin her biri benzer erişim denetimi özellikleri sağlar:

|     |     | 
| --- | --- |
| ![Auth0](./media/active-directory-acs-migration/rsz_auth0.png) | [Auth0](https://auth0.com/acs) oluşturduğu bir esnek bir bulut kimlik hizmetidir [müşteriler erişim denetimi için üst düzey geçiş kılavuzunu](https://auth0.com/acs)ve ACS yapan neredeyse her bir özelliği destekler. |
| ![Ping](./media/active-directory-acs-migration/rsz_ping.png) | [Ping Identity](https://www.pingidentity.com) ACS'ye benzer iki çözümler sunar. PingOne ACS olarak aynı özelliklerin çoğunu destekleyen bir bulut kimlik hizmetidir ve PingFederate bir şirket içi kimlik daha fazla esneklik sunan bir ürün üzerinde benzer. Başvurmak [Ping'ın ACS emeklilik Kılavuzu](https://www.pingidentity.com/en/company/blog/2017/11/20/migrating_from_microsoft_acs_to_ping_identity.html) bu ürünleri kullanma hakkında daha fazla bilgi. |

Bizim AIM Auth0 Ping Identity ile çalışırken, tüm erişim denetimi müşterilerin erişim denetiminden taşımak için gerekli çalışma miktarını azaltan bir geçiş yolu, uygulamalar ve hizmetler için sahip olduğundan emin olmaktır.

#### <a name="passthrough-authentication"></a>Geçişli kimlik doğrulamasını

Rastgele belirteç dönüştürme ile geçişli kimlik doğrulaması için ACS için eşdeğer Microsoft teknoloji yoktur. Müşterilerinizin gerekenler olan Auth0 en yakın yaklaşık çözüm sağlayan bir olabilir.

## <a name="questions-concerns-and-feedback"></a>Soru ve endişeleriniz geri bildirim

Bu makaleyi okuduktan sonra çok sayıda erişim denetimi müşterilere açık bir geçiş yolu bulmaz biliyoruz. Bazı Yardım veya doğru planı belirlemede rehberlik gerekebilir. Lütfen geçiş senaryoları ve sorular tartışmak istiyorsanız, bu sayfada bir yorum yazın.
