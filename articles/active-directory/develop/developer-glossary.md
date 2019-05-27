---
title: Microsoft kimlik platformu Geliştirici sözlüğü | Azure
description: Yaygın olarak kullanılan Microsoft kimlik platformu Geliştirici kavramları ve özelliklerine yönelik terimleri içeren bir liste.
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: 551512df-46fb-4219-a14b-9c9fc23998ba
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/13/2019
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: jmprieur, saeeda, jesakowi, nacanuma
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6c989b690e9537dcaaf3710996474a1b8b99826b
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65962743"
---
# <a name="microsoft-identity-platform-developer-glossary"></a>Microsoft kimlik platformu Geliştirici sözlüğü

Bu makalede, Microsoft kimlik platformu kullanarak uygulama geliştirmeyi öğrenme olduğunda yararlıdır Geliştirici kavramları ve terminolojisi, bazıları için tanımları içerir.

## <a name="access-token"></a>erişim belirteci

Bir tür [güvenlik belirteci](#security-token) tarafından verilen bir [yetkilendirme sunucusu](#authorization-server)ve tarafından kullanılan bir [istemci uygulaması](#client-application) erişmek için bir [kaynak sunucusukorumalı](#resource-server). Genellikle biçiminde bir [JSON Web Token (JWT)][JWT], istemci tarafından verilen yetkilendirme belirteci şekillendiren [kaynak sahibi](#resource-owner), istenen erişim düzeyi için. Belirteç yürürlükteki tüm içeren [talep](#claim) konu, belirli bir kaynağa erişirken kimlik bilgisinin form olarak kullanmak için istemci uygulaması etkinleştirme. Bu da kaynak sahibi istemciye kimlik bilgilerini kullanıma sunmak ihtiyacını ortadan kaldırır.

Erişim belirteçleri "Kullanıcı + uygulama" veya "Yalnızca uygulama", temsil ettiği kimlik bilgilerini bağlı olarak da adlandırılır. Örneğin, bir istemci uygulaması kullandığında:

* ["Yetkilendirme kodu" yetkilendirme verme](#authorization-grant), son kullanıcının kaynağa erişim yetkisi istemciye yetki aktarımına kaynak sahibi olarak ilk kimliğini doğrular. İstemci, daha sonra bir erişim belirteci alınırken'kimliğini doğrular. İstemci uygulama ve uygulamanın yetkilendirilmiş hem kullanıcı hem de temsil ettiğinden belirteç bazen için özellikle bir "Kullanıcı + uygulama" belirteci adlandırılır.
* ["İstemci kimlik bilgileri" yetkilendirme verme](#authorization-grant), istemciye tek kimlik doğrulaması belirteci bazen "Yalnızca uygulama" belirteci olarak başvurulabilen şekilde kaynak sahibinin kimlik doğrulama/yetkilendirme çalışmasını sağlar.

Bkz: [Microsoft kimlik platformu belirteç başvurusu] [ AAD-Tokens-Claims] daha fazla ayrıntı için.

## <a name="application-id-client-id"></a>Uygulama Kimliği (istemci kimliği)

Belirli bir uygulamayı ve ilişkili yapılandırmaları tanımlayan bir uygulama kaydı için benzersiz tanımlayıcı Azure AD'ye sorunları. Bu uygulama Kimliğini ([istemci kimliği](https://tools.ietf.org/html/rfc6749#page-15)) kimlik doğrulaması gerçekleştirme ve istekleri olduğunda kullanılan kimlik doğrulama kitaplıkları için sağlanan geliştirme zamanında. Uygulama Kimliği (istemci kimliği) bir gizli dizi değil.

## <a name="application-manifest"></a>Uygulama bildirimi

Bir özellik tarafından sağlanan [Azure portalında][AZURE-portal], uygulamanın kimlik yapılandırması, ilişkili güncelleştirmeye yönelik bir mekanizma olarak kullanılan bir JSON temsili üretir [ Uygulama] [ AAD-Graph-App-Entity] ve [ServicePrincipal] [ AAD-Graph-Sp-Entity] varlıklar. Bkz: [Azure Active Directory Uygulama bildirimini anlama] [ AAD-App-Manifest] daha fazla ayrıntı için.

## <a name="application-object"></a>Uygulama nesnesi

Ne zaman, kaydolun/güncelleştirme uygulamada [Azure portalında][AZURE-portal], portal/uygulama nesnesini ve karşılık gelen bir güncelleştirme [hizmet sorumlusu nesnesi](#service-principal-object) Bu Kiracı için. Uygulama nesnesi *tanımlar* uygulama (arasında erişim, sahip olduğu genel olarak tüm kiracılar), kimlik yapılandırması, karşılık gelen hizmet sorumlusu nesneleri olan bir şablonu sağlayan kullanıcının  *türetilmiş* için yerel çalışma zamanı (içinde belirli bir kiracıda) kullanın.

Daha fazla bilgi için [uygulama ve hizmet sorumlusu nesneleri][AAD-App-SP-Objects].

## <a name="application-registration"></a>Uygulama kaydı

Bir uygulamayı tümleştirin ve Azure ad kimlik ve erişim yönetimi işlevleri temsilci izin vermek üzere bir Azure AD'ye kayıtlı [Kiracı](#tenant). Uygulamanızı Azure AD'ye kaydetme, uygulamanız için bir kimlik yapılandırması gibi özellikleri kullanın ve Azure AD ile tümleştirme izin veren sağlanmaktadır:

* Güçlü bir yönetim, tek Azure AD Identity Management kullanarak oturum açmayı ve [Openıd Connect] [ OpenIDConnect] protokol uygulaması
* Aracılı erişimi [korunan kaynakları](#resource-server) tarafından [istemci uygulamaları](#client-application), OAuth 2.0 aracılığıyla [yetkilendirme sunucusu](#authorization-server)
* [Onay çerçevesine](#consent) istemci kaynak sahibi kimlik tabanlı korumalı kaynaklara erişimi yönetme.

Bkz: [uygulamaları Azure Active Directory ile tümleştirme] [ AAD-Integrating-Apps] daha fazla ayrıntı için.

## <a name="authentication"></a>kimlik doğrulaması

Zorlu kimlik ve erişim denetimi için kullanılacak bir güvenlik sorumlusu oluşturma için temel sağlayan bir taraf meşru kimlik bilgileri işlemi. Sırasında bir [OAuth2 yetkilendirme verme](#authorization-grant) kimlik doğrulaması taraf ya da rolü gibi doldurma [kaynak sahibi](#resource-owner) veya [istemci uygulaması](#client-application)bağlı olarak kullanılan verin.

## <a name="authorization"></a>Yetkilendirme

Bir şey yapmak için bir kimliği doğrulanmış güvenlik sorumlusu izin verme işlemi. Azure AD programlama modelinde iki temel kullanım örneği vardır:

* Sırasında bir [OAuth2 yetkilendirme verme](#authorization-grant) akış: zaman [kaynak sahibi](#resource-owner) yetkisi veren [istemci uygulaması](#client-application), kaynağa erişmek izin vermeden Sahibin kaynaklar.
* İstemci tarafından kaynak erişim sırasında: tarafından uygulanan [kaynak sunucusu](#resource-server)kullanarak [talep](#claim) değerleri sunmak [erişim belirteci](#access-token) erişim denetimi kararları vermek için Bunlar üzerine temel.

## <a name="authorization-code"></a>Yetkilendirme kodu

Bir kısa süreli "token" için sağlanan bir [istemci uygulaması](#client-application) tarafından [yetkilendirme uç noktası](#authorization-endpoint), dört OAuth2 biri "yetkilendirme kodu" akışının parçası olarak [yetkilendirmeverir](#authorization-grant). Kod kimlik doğrulaması yanıtı istemci uygulamasına döndürülen bir [kaynak sahibi](#resource-owner), kaynak sahibi belirten yetkilendirme istenen kaynaklara erişmek için temsilci. Akışın bir parçası olarak, kodu daha sonra için kullanıldıktan bir [erişim belirteci](#access-token).

## <a name="authorization-endpoint"></a>Yetkilendirme uç noktası

Uygulayan uç noktalardan biri [yetkilendirme sunucusu](#authorization-server)ile etkileşim kurmak için kullanılan [kaynak sahibi](#resource-owner) sağlamak amacıyla bir [yetkilendirme verme](#authorization-grant) bir OAuth2 sırasında Yetkilendirme verme akışı. Kullanılan yetkilendirme verme akışı bağlı olarak sağlanan gerçek verin, dahil olmak üzere değişebilir bir [yetkilendirme kodu](#authorization-code) veya [güvenlik belirteci](#security-token).

OAuth2 belirtimi bkz [yetkilendirme verme türleri] [ OAuth2-AuthZ-Grant-Types] ve [yetkilendirme uç noktası] [ OAuth2-AuthZ-Endpoint] bölümler ve [Openıdconnect belirtimi] [ OpenIDConnect-AuthZ-Endpoint] daha fazla ayrıntı için.

## <a name="authorization-grant"></a>Yetkilendirme verme

Bir kimlik bilgisi temsil eden [kaynak sahibinin](#resource-owner) [yetkilendirme](#authorization) verilen, korunan kaynaklara erişim için bir [istemci uygulaması](#client-application). Bir istemci uygulaması kullanabilirsiniz [dört OAuth2 yetkilendirme Framework tarafından tanımlanan türleri vermek] [ OAuth2-AuthZ-Grant-Types] istemci türü/gereksinimlerine bağlı olarak bir izni almak için: "yetkilendirme kodu verme", "istemci kimlik bilgileri verme","örtük verme"ve"kaynak sahibi parola kimlik bilgileri verme". İstemciye döndürülen kimlik bilgileri geçerli bir [erişim belirteci](#access-token), veya bir [yetkilendirme kodu](#authorization-code) (daha sonra değiş tokuş için bir erişim belirteci), kullanılan yetkilendirme verme türüne bağlıdır.

## <a name="authorization-server"></a>Yetkilendirme sunucusu

Tarafından tanımlandığı gibi [OAuth2 yetkilendirme Framework][OAuth2-Role-Def], sunucu erişimi vermekten sorumlu belirteçler için [istemci](#client-application) başarıylakimlikdoğrulandıktansonra[kaynak sahibi](#resource-owner) ve kendi yetkilendirme alma. A [istemci uygulaması](#client-application) yetkilendirme sunucusu zamanında etkileşimde kendi [yetkilendirme](#authorization-endpoint) ve [belirteci](#token-endpoint) uç noktaları, OAuth2 uygun olarak tanımlanan [yetkilendirme vermeleri](#authorization-grant).

Örneğin yetkilendirme sunucusu rolü Azure AD uygulamaları ve API'leri, Microsoft hizmeti için Microsoft kimlik platformu uygulaması tümleştirme söz konusu olduğunda, Microsoft kimlik platformu uygulayan [Microsoft Graph API'lerini] [Microsoft-Graph].

## <a name="claim"></a>talep

A [güvenlik belirteci](#security-token) onaylar sağlayan bir varlık talepleri içerir (gibi bir [istemci uygulaması](#client-application) veya [kaynak sahibi](#resource-owner)) başka bir varlığa (örneğin, [kaynak sunucusu](#resource-server)). Talep belirteci konu hakkında bilgiler geçiş ad/değer çiftleri olan (örneğin, tarafından doğrulanmış güvenlik sorumlusu [yetkilendirme sunucusu](#authorization-server)). Verilen belirteçte talep belirtecinin, konu, uygulama yapılandırması, vb. kimliğini doğrulamak için kullanılan kimlik bilgisi türünü de dahil olmak üzere çeşitli değişkenler üzerinde bağımlıdır.

Bkz: [Microsoft kimlik platformu belirteç başvurusu] [ AAD-Tokens-Claims] daha fazla ayrıntı için.

## <a name="client-application"></a>İstemci uygulaması

Tarafından tanımlandığı gibi [OAuth2 yetkilendirme Framework][OAuth2-Role-Def], yapan bir uygulamada korunan kaynak isteklerini adına [kaynak sahibi](#resource-owner). "İstemci" terimini (örneği için olup olmadığını uygulama bir sunucu, masaüstü veya diğer cihazlar üzerinde çalıştırılır) belirli donanım uygulama özelliklerinin göstermez.

Bir istemci uygulamanın istekleri [yetkilendirme](#authorization) katılmak için bir kaynak sahibinden bir [OAuth2 yetkilendirme verme](#authorization-grant) akış ve kaynak sahibinin adınıza API'leri/verilerine erişebilir. OAuth2 yetkilendirme Framework [iki türde istemci tanımlar][OAuth2-Client-Types], "gizli" ve "Genel", istemcinin, kimlik bilgilerinin gizliliğini yeteneği göre. Uygulamaları uygulayabilirsiniz bir [web istemcisi (gizli)](#web-client) bir web sunucusunda çalışan bir [yerel istemci (Genel)](#native-client) bir cihazda yüklü veya [istemci kullanıcı aracısı tabanlı (Genel)](#user-agent-based-client)bir cihazın tarayıcısında çalışır.

## <a name="consent"></a>Onayı

İşlemi bir [kaynak sahibi](#resource-owner) yetkisi verme bir [istemci uygulaması](#client-application)özel altındaki korumalı kaynaklara erişmek için [izinleri](#permissions), adına Kaynak sahibi. İstemci tarafından istenen izinlere bağlı olarak, bir yönetici veya kullanıcı sırasıyla kuruluş/tek verilerine erişmesine izin vermek için onay istenir. Unutmayın, içinde bir [çok kiracılı](#multi-tenant-application) senaryosu, uygulamanın [hizmet sorumlusu](#service-principal-object) consenting kullanıcının kiracıda da kaydedilir.

Bkz: [onay çerçevesine](consent-framework.md) daha fazla bilgi için.

## <a name="id-token"></a>Kimlik belirteci

Bir [Openıd Connect] [ OpenIDConnect-ID-Token] [güvenlik belirteci](#security-token) tarafından sağlanan bir [yetkilendirme sunucusunun](#authorization-server) [yetkilendirme uç noktası](#authorization-endpoint), içeren [talep](#claim) son kullanıcı kimlik doğrulaması için tıklarsınız [kaynak sahibi](#resource-owner). Bir erişim belirteci gibi ayrıca kimlik belirteçlerini temsil dijital olarak imzalanmış olarak [JSON Web Token (JWT)][JWT]. Bir erişim belirteci aksine, bir kimlik belirtecinin talep kaynağına erişimle ilgili amacıyla kullanılmaz ve özellikle erişim denetimi.

Bkz: [Microsoft kimlik platformu belirteç başvurusu] [ AAD-Tokens-Claims] daha fazla ayrıntı için.

## <a name="microsoft-identity-platform"></a>Microsoft kimlik platformu

Microsoft kimlik platformu, Azure Active Directory (Azure AD) kimlik hizmeti ve geliştirici platformunun geliştirilmesiyle ortaya çıkmıştır. Bu platform geliştiricilerin tüm Microsoft kimlikleriyle oturum açan ve Microsoft Graph veya diğer Microsoft API'leri ya da geliştiricilerin derlemiş olduğu API'lere çağrı göndermek için gerekli belirteçleri alan uygulamalar derlemesini sağlar. Bir kimlik doğrulama hizmeti, kitaplıkları, uygulama kaydı ve yapılandırması, tam Geliştirici belgeleri, kod örnekleri ve diğer Geliştirici içeriği oluşan tam özellikli bir platform var. Microsoft Identity Platform OAuth 2.0 ve OpenID Connect gibi sektör standardı protokolleri destekler. Bkz: [hakkında Microsoft kimlik platformu](about-microsoft-identity-platform.md) daha fazla ayrıntı için.

## <a name="multi-tenant-application"></a>çok kiracılı uygulama

Oturum açma sağlayan uygulama sınıfının ve [onay](#consent) herhangi bir Azure AD'de sağlanan kullanıcılar tarafından [Kiracı](#tenant), farklı istemci kayıtlı olduğu kiracıları dahil. [Yerel istemci](#native-client) varsayılan olarak, çok kiracılı uygulamalar ise [web istemcisi](#web-client) ve [web kaynak/API](#resource-server) uygulamalar arasında tek veya çok kiracılı seçebilme sahiptir. Aksine, tek kiracılı, kayıtlı bir web uygulaması yalnızca izin oturum açma kullanıcı hesaplarından biri olarak aynı kiracıda sağlanan Uygulama yeri kaydedilir.

Bkz: [çok kiracılı uygulama desenini kullanarak istediğiniz Azure AD kullanıcısı ile oturum açma] [ AAD-Multi-Tenant-Overview] daha fazla ayrıntı için.

## <a name="native-client"></a>Yerel istemci

Bir tür [istemci uygulaması](#client-application) yüklü olan yerel olarak bir cihazda. Bir cihazda tüm kod yürütülür olduğundan, kimlik bilgileri özel olarak/ilkemiz depolamak becerisinin nedeniyle "Genel" bir istemci olarak kabul edilir. Bkz: [OAuth2 istemci türleri ve profiller] [ OAuth2-Client-Types] daha fazla ayrıntı için.

## <a name="permissions"></a>izinler

A [istemci uygulaması](#client-application) erişim kazanır bir [kaynak sunucusu](#resource-server) tarafından izin isteklerini bildirme. İki türü kullanılabilir:

* "Yönetici temsilcisi izinlerini belirtin" [kapsam tabanlı](#scopes) imzalı bileşeninden Temsilcili yetkilendirme kullanarak erişim [kaynak sahibi](#resource-owner), kaynağı çalışma zamanı için sunulan ["scp" Talep](#claim) istemciye ait [erişim belirteci](#access-token).
* Belirttiğiniz "Uygulama" izinleri [rol tabanlı](#roles) istemci uygulamanın kimlik bilgileri/kimliğine erişim, kaynağı çalışma zamanı için sunulan ["rolleri" talep](#claim) istemciye ait erişim belirteci.

Bunlar sırasında yüzey [onay](#consent) işlemi, yöneticiye veya kaynak sahibi kiracısındaki kaynaklarına istemci erişimi verme/reddetme fırsatı verir.

İzin istekleri yapılandırılmış "Uygulamalar" / "Ayarlar" sekmesinde bulunan [Azure portalında][AZURE-portal], "Gerekli"Temsilcili izinler"ve"uygulama istenen seçerek izinleri altında", İzinler"(ikincisi genel Yönetici rolüne üyelik gerektirir). Çünkü bir [genel istemci](#client-application) kimlik bilgilerini güvenli bir şekilde koruyamadığını yalnızca temsilci izinleri isteyebilir sırada bir [gizli istemci](#client-application) yetkilendirilen ve uygulama yeteneği vardır izinler. İstemcinin [uygulama nesnesi](#application-object) bildirilen izinler depolar, [requiredResourceAccess özelliği][AAD-Graph-App-Entity].

## <a name="resource-owner"></a>Kaynak sahibi

Tarafından tanımlandığı gibi [OAuth2 yetkilendirme Framework][OAuth2-Role-Def], korunan bir kaynağa erişim izni verme yeteneğine sahip bir varlık. Kaynak sahibi bir kişinin olduğunda, bir son kullanıcı olarak adlandırılır. Örneğin, bir [istemci uygulaması](#client-application) aracılığıyla bir kullanıcının posta erişmek isteyen [Microsoft Graph API][Microsoft-Graph], kaynak sahibinin izni gerektirir posta kutusu.

## <a name="resource-server"></a>Kaynak sunucusu

Tarafından tanımlandığı gibi [OAuth2 yetkilendirme Framework][OAuth2-Role-Def], ana bilgisayar kaynakları, kabul etme ve yanıtlama özellikli korumalı bir sunucusu korunan kaynak isteklerini tarafından [istemci uygulamaları](#client-application) bulunandan bir [erişim belirteci](#access-token). Olarak da bilinen bir korumalı kaynak sunucuda veya kaynağı uygulama.

Kaynak sunucuda API'lerini kullanıma sunar ve kendi korumalı kaynaklara erişimini zorunlu kılar [kapsamları](#scopes) ve [rolleri](#roles), OAuth 2.0 yetkilendirme Framework kullanarak. Azure AD kiracısına ilişkin veriler erişim sağlayan bir Azure AD Graph API'si ve e-posta ve takvim gibi veri erişim sağlayan Office 365 API'lerini verilebilir. Bunların her ikisi de aynı zamanda aracılığıyla erişilebilir [Microsoft Graph API][Microsoft-Graph].

Yalnızca bir istemci uygulaması gibi kaynak uygulamanın kimlik yapılandırması aracılığıyla kurulur [kayıt](#application-registration) bir Azure AD kiracısında uygulama ve hizmet sorumlusu nesnesi sağlar. Bazı Microsoft tarafından sağlanan API'leri, Azure AD Graph API'si gibi tüm kiracılar sağlama sırasında sunulan hizmet sorumluları önceden kayıtlı.

## <a name="roles"></a>rol

Gibi [kapsamları](#scopes), roller, bir yol sağlar bir [kaynak sunucusu](#resource-server) kendi korumalı kaynaklara erişimi yönetmek için. İki tür vardır: bir "kullanıcı" rolü "uygulama" rolü için aynı uygular kaynak erişim gerektiren kullanıcılar/gruplar için rol tabanlı erişim denetimi uygular [istemci uygulamaları](#client-application) erişimi gerektirir.

Rolleridir kaynak tanımlı dizeleri (örneğin "Harcama onaylayan", "Salt okunur", "Directory.ReadWrite.All"), yönetilen olarak [Azure portalında] [ AZURE-portal] kaynağın aracılığıyla [uygulama bildirim](#application-manifest)ve kaynağın depolanan [appRoles özelliği][AAD-Graph-Sp-Entity]. Azure portalında da kullanıcıları "kullanıcı" rollere atamak ve istemciyi yapılandırmak için kullanılan [uygulama izinleri](#permissions) "uygulama" rolü erişmek için.

Azure AD'nin Graph API'si tarafından kullanıma sunulan uygulama rolleri hakkında ayrıntılı bilgi için bkz: [Graph API'si izin kapsamları][AAD-Graph-Perm-Scopes]. Bir adım adım uygulama örneği için bkz [RBAC ve Azure portalını kullanarak erişimini yönetme][AAD-RBAC].

## <a name="scopes"></a>scopes

Gibi [rolleri](#roles), kapsam için bir yol sağlar bir [kaynak sunucusu](#resource-server) kendi korumalı kaynaklara erişimi yönetmek için. Kapsamlar uygulamak için kullanılan [kapsam tabanlı] [ OAuth2-Access-Token-Scopes] için erişim denetimi, bir [istemci uygulaması](#client-application) , verildiği Temsilcili erişim için kaynağın sahibi tarafından.

Kapsamları misiniz yönetilen kaynak tanımlı dizeleri (örneğin "Mail.Read", "Directory.ReadWrite.All") [Azure portalında] [ AZURE-portal] kaynağın aracılığıyla [uygulama bildirimini](#application-manifest)ve kaynağın depolanan [oauth2Permissions özelliği][AAD-Graph-Sp-Entity]. Azure portalında da istemci uygulamasını yapılandırmak için kullanılan [temsilci izinleri](#permissions) kapsam erişmek için.

En iyi yöntem adlandırma kuralı, gelir "resource.operation.constraint" biçimini kullanın. Azure AD'nin Graph API'si tarafından kullanıma sunulan kapsamları hakkında ayrıntılı bilgi için bkz: [Graph API'si izin kapsamları][AAD-Graph-Perm-Scopes]. Office 365 Hizmetleri tarafından kullanıma sunulan kapsamlar için bkz: [Office 365 API izinleri başvurusu][O365-Perm-Ref].

## <a name="security-token"></a>Güvenlik belirteci

Talepler, bir OAuth2 belirteç veya SAML 2.0 onaylama gibi içeren, imzalanmış bir belge. Bir OAuth2 için [yetkilendirme verme](#authorization-grant)e [erişim belirteci](#access-token) (OAuth2) ve bir [kimlik belirteci](https://openid.net/specs/openid-connect-core-1_0.html#IDToken) ikisi de olarak uygulanır, güvenlik belirteçleri türleri bir [JSON Web Token (JWT)][JWT].

## <a name="service-principal-object"></a>Hizmet sorumlusu nesnesi

Ne zaman, kaydolun/güncelleştirme uygulamada [Azure portalında][AZURE-portal], portal/hem de güncelleştirme bir [uygulama nesnesi](#application-object) ve karşılık gelen hizmet sorumlusu nesnesi Bu Kiracı için. Uygulama nesnesi *tanımlar* (burada ilişkili uygulama verilmiş erişim genel olarak tüm kiracılar) arasında uygulamanın kimlik yapılandırması ve şablondan, karşılık gelen hizmet sorumlusu nesne olan *türetilmiş* için yerel çalışma zamanı (içinde belirli bir kiracıda) kullanın.

Daha fazla bilgi için [uygulama ve hizmet sorumlusu nesneleri][AAD-App-SP-Objects].

## <a name="sign-in"></a>oturum açma

İşlemi bir [istemci uygulaması](#client-application) alınırken amacıyla durumu, son kullanıcı kimlik doğrulamayı başlatan ve yakalama ilgili bir [güvenlik belirteci](#security-token) ve bu durumda uygulama oturuma kapsamını belirleme. Kullanıcı profili bilgileri gibi yapılar içerebilir ve bu bilgileri belirteç taleplerinden türetilmiş durumu.

Bir uygulamanın oturum açma işlevi genellikle çoklu oturum açma (SSO) uygulamak için kullanılır. Bunu ayrıca bir "kayıt" işlevi (ilk oturum açma sonrası) bir uygulamaya erişim kazanmak bir son kullanıcı için giriş noktası olarak bulunabilen. Kaydolma işlevi toplamak ve kullanıcıya özgü ek durumu kalıcı hale getirmek için kullanılır ve gerektirebilir [kullanıcı onayı](#consent).

## <a name="sign-out"></a>oturumu kapatma

Kullanıcı durumunu ayırma bir son kullanıcı unauthenticating işlemi ile ilişkili [istemci uygulaması](#client-application) oturumu sırasında [oturum açma](#sign-in)

## <a name="tenant"></a>tek

Bir Azure AD directory örneğini Azure AD kiracısı adlandırılır. Bu da dahil olmak üzere çeşitli özellikler sağlar:

* Tümleşik uygulamalar için bir kayıt defteri hizmeti
* kimlik doğrulama kullanıcı hesapları ile kayıtlı uygulamalar
* OAuth2 ve SAML, dahil çeşitli protokoller desteklemek için gereken bir REST uç noktalarını da dahil olmak üzere [yetkilendirme uç noktası](#authorization-endpoint), [belirteç uç noktası](#token-endpoint) ve "Genel" uç nokta tarafından kullanılan [ çok kiracılı uygulamaları](#multi-tenant-application).

Azure ve Office 365 abonelikleri ile oluşturulan/ilişkili kayıt, sağlama kimlik ve erişim yönetimi özellikleri için abonelik sırasında Azure AD kiracılarıyla. Azure aboneliği yöneticileri de ek oluşturabilirsiniz Azure portal aracılığıyla Azure AD kiracılarıyla. Bkz: [bir Azure Active Directory kiracısı edinme] [ AAD-How-To-Tenant] çeşitli yollar hakkında daha fazla ayrıntı için bir kiracı için erişim alabilirsiniz. Bkz: [Azure aboneliklerinin Azure Active Directory ile ilişkisi] [ AAD-How-Subscriptions-Assoc] abonelikleri ve Azure AD kiracısı arasındaki ilişki hakkında ayrıntılı bilgi için.

## <a name="token-endpoint"></a>belirteç uç noktası

Uygulayan uç noktalardan biri [yetkilendirme sunucusu](#authorization-server) destek OAuth2 [yetkilendirme vermeleri](#authorization-grant). Grant bağlı olarak, bu almak için kullanılabilir bir [erişim belirteci](#access-token) (ve ilgili "Yenile" belirteci) için bir [istemci](#client-application), veya [kimlik belirteci](#id-token) ile kullanıldığında [Openıd Connect] [ OpenIDConnect] protokolü.

## <a name="user-agent-based-client"></a>İstemci kullanıcı aracısı tabanlı

Bir tür [istemci uygulaması](#client-application) kodu bir web sunucusundan indirir ve tek sayfalı uygulama (SPA) gibi bir kullanıcı aracısı içinde (örneğin, bir web tarayıcısı) yürütür. Bir cihazda tüm kod yürütülür olduğundan, kimlik bilgileri özel olarak/ilkemiz depolamak becerisinin nedeniyle "Genel" bir istemci olarak kabul edilir. Daha fazla bilgi için [OAuth2 istemci türleri ve profiller][OAuth2-Client-Types].

## <a name="user-principal"></a>Kullanıcı asıl adı

Benzer şekilde, bir uygulama örneği temsil eden bir hizmet sorumlusu nesnesi kullanılır, kullanıcı asıl nesne başka bir kullanıcı temsil eden güvenlik sorumlusu türüdür. Azure AD Graph [kullanıcı varlığı] [ AAD-Graph-User-Entity] adı ve Soyadı, kullanıcı asıl adı, dizin rolü üyeliği vb. gibi kullanıcı ile ilgili özellikleri dahil olmak üzere bir kullanıcı nesnesi için bir şema tanımlar. Bu, çalışma zamanında bir kullanıcı asıl adı oluşturmak Azure AD için kullanıcı kimlik yapılandırması sağlar. Çoklu oturum açma için kimliği doğrulanmış bir kullanıcıyı temsil etmek için kullanılan kullanıcı asıl adı kaydı [onay](#consent) temsilci, erişim denetimi kararları vb. yapma.

## <a name="web-client"></a>Web istemcisi

Bir tür [istemci uygulaması](#client-application) sunucuda kimlik bilgilerini güvenli bir şekilde depolanarak "gizli" bir istemci çalışması için tüm kodu bir web sunucusundaki ve mümkün yürütür. Daha fazla bilgi için [OAuth2 istemci türleri ve profiller][OAuth2-Client-Types].

## <a name="next-steps"></a>Sonraki adımlar

[Microsoft kimlik platformu Geliştirici Kılavuzu] [ AAD-Dev-Guide] genel bir bakış da dahil olmak üzere tüm Microsoft kimlik platformu geliştirme ile ilgili konular için kullanılacak giriş sayfasıdır [uygulama Tümleştirme] [ AAD-How-To-Integrate] ve temelleri [Microsoft kimlik platformu doğrulama ve desteklenen kimlik doğrulama senaryoları][AAD-Auth-Scenarios]. Kod örnekleri ve öğreticiler çalışmaya hızlıca almak nasıl da bulabilirsiniz [GitHub](https://github.com/azure-samples?utf8=%E2%9C%93&q=active%20directory&type=&language=).

Geri bildirim sağlamak ve geliştirmek ve istekleri yeni tanımları dahil olmak üzere veya var olanları güncelleştirme bu içeriği biçimlendirmek için yardımcı olmak için aşağıdaki Açıklamalar bölümüne kullanın!

<!--Image references-->

<!--Reference style links -->
[AAD-App-Manifest]:reference-app-manifest.md
[AAD-App-SP-Objects]:app-objects-and-service-principals.md
[AAD-Auth-Scenarios]:authentication-scenarios.md
[AAD-Dev-Guide]:azure-ad-developers-guide.md
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AAD-Graph-User-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity
[AAD-How-Subscriptions-Assoc]:../fundamentals/active-directory-how-subscriptions-associated-directory.md
[AAD-How-To-Integrate]: ./active-directory-how-to-integrate.md
[AAD-How-To-Tenant]:quickstart-create-new-tenant.md
[AAD-Integrating-Apps]:quickstart-v1-integrate-apps-with-azure-ad.md
[AAD-Multi-Tenant-Overview]:howto-convert-app-to-be-multi-tenant.md
[AAD-Security-Token-Claims]: ./active-directory-authentication-scenarios/#claims-in-azure-ad-security-tokens
[AAD-Tokens-Claims]:access-tokens.md
[AZURE-portal]: https://portal.azure.com
[AAD-RBAC]: ../../role-based-access-control/role-assignments-portal.md
[JWT]: https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32
[Microsoft-Graph]: https://developer.microsoft.com/graph
[O365-Perm-Ref]: https://msdn.microsoft.com/office/office365/howto/application-manifest
[OAuth2-Access-Token-Scopes]: https://tools.ietf.org/html/rfc6749#section-3.3
[OAuth2-AuthZ-Endpoint]: https://tools.ietf.org/html/rfc6749#section-3.1
[OAuth2-AuthZ-Grant-Types]: https://tools.ietf.org/html/rfc6749#section-1.3
[OAuth2-Client-Types]: https://tools.ietf.org/html/rfc6749#section-2.1
[OAuth2-Role-Def]: https://tools.ietf.org/html/rfc6749#page-6
[OpenIDConnect]: https://openid.net/specs/openid-connect-core-1_0.html
[OpenIDConnect-AuthZ-Endpoint]: https://openid.net/specs/openid-connect-core-1_0.html#AuthorizationEndpoint
[OpenIDConnect-ID-Token]: https://openid.net/specs/openid-connect-core-1_0.html#IDToken
