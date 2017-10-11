---
title: "Azure Active Directory Geliştirici sözlüğü | Microsoft Docs"
description: "Yaygın olarak kullanılan Azure Active Directory Geliştirici kavramları ve Özellikler koşulları listesi."
services: active-directory
documentationcenter: 
author: bryanla
manager: mbaldwin
editor: 
ms.assetid: 551512df-46fb-4219-a14b-9c9fc23998ba
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/19/2017
ms.author: bryanla
ms.custom: aaddev
ms.openlocfilehash: d7bc694b05ed1eb3915ba913afdb3cc39e048ca7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="azure-active-directory-developer-glossary"></a>Azure Active Directory Geliştirici sözlüğü
Bu makalede, Azure AD için uygulama geliştirme öğrenmeye olduğunda faydalıdır çekirdek Azure Active Directory (AD) Geliştirici kavramları bazıları için tanımları içerir.

## <a name="access-token"></a>erişim belirteci
Bir tür [güvenlik belirteci](#security-token) tarafından verilen bir [yetkilendirme sunucusu](#authorization-server)ve tarafından kullanılan bir [istemci uygulaması](#client-application) erişmek için bir [kaynak sunucusukorumalı](#resource-server). Genellikle biçiminde bir [JSON Web Token (JWT)][JWT], istemci tarafından verilen yetkilendirme belirteci içerir [kaynak sahibi](#resource-owner), istenen erişim düzeyi için. Belirteç uygulanabilir tüm içerir [talep](#claim) konu, belirli bir kaynağa erişilirken bir kimlik bilgisi formu olarak kullanmak istemci uygulaması etkinleştiriliyor. Bu aynı zamanda istemci kimlik bilgilerini kullanıma sunmak kaynak sahibine gereksinimini ortadan kaldırır.

Erişim belirteçleri "Kullanıcı + uygulama" veya "Uygulama yalnızca", bağlı olarak temsil ettiği kimlik bilgileri olarak da adlandırılır. Örneğin, bir istemci uygulaması kullandığında:

* ["Yetkilendirme kodu" yetkilendirme verme](#authorization-grant), kaynağa erişim yetkisi istemciye yetki aktarımına kaynak sahibi olarak ilk son kullanıcının kimliğini doğrular. İstemci, daha sonra bir erişim belirteci edinirken'kimliğini doğrular. İstemci uygulama ve uygulama yetkilendirilmiş hem kullanıcı hem de temsil eden olarak belirtecin bazen için "Kullanıcı + uygulama" token daha belirgin olarak başvurulabilir.
* ["İstemci kimlik bilgileri" yetkilendirme verme](#authorization-grant), belirteç bazen "Yalnızca uygulama" belirteci olarak başvurulabilen şekilde kaynak sahibinin kimlik doğrulama/yetkilendirme çalışan tek kimlik doğrulaması, istemci sağlar.

Bkz: [Azure AD belirteç başvurusu] [ AAD-Tokens-Claims] daha fazla ayrıntı için.

## <a name="application-manifest"></a>Uygulama bildirimi
Tarafından sağlanan bir özellik [Azure portal][AZURE-portal], uygulamanın kimlik yapılandırması, kendi ilişkili güncelleştirme mekanizması olarak kullanılan bir JSON gösterimini üreten [ Uygulama] [ AAD-Graph-App-Entity] ve [ServicePrincipal] [ AAD-Graph-Sp-Entity] varlıklar. Bkz: [Azure Active Directory Uygulama bildirimini anlama] [ AAD-App-Manifest] daha fazla ayrıntı için.

## <a name="application-object"></a>Uygulama nesnesi
Ne zaman, kaydetme/güncelleştirme bir uygulamada [Azure portal][AZURE-portal], portal/uygulama nesnesi ve karşılık gelen bir güncelleştirme [hizmet sorumlusu nesnesi](#service-principal-object) Bu Kiracı için. Uygulama nesnesi *tanımlar* uygulama kimlik yapılandırması (üzerinden erişim bulunduğu genel olarak tüm kiracılar), kendi karşılık gelen hizmet asıl nesneleri olan bir şablonu sağlama kullanıcının  *türetilmiş* yerel olarak çalışma zamanında (içinde belirli bir kiracı) kullanmak için.

Bkz: [uygulama ve hizmet sorumlusu nesneleri] [ AAD-App-SP-Objects] daha fazla bilgi için.

## <a name="application-registration"></a>Uygulama kaydı
İle tümleştirmek ve Azure ad kimlik ve erişim yönetimi işlevleri temsilci seçmek için bir uygulama izin vermek üzere bir Azure AD ile kayıtlı [Kiracı](#tenant). Azure AD ile uygulamanızı kaydederken, Azure AD ile tümleştirmek ve özellikleri gibi kullandığınız izin vererek, uygulamanız için bir kimlik yapılandırması sağlanmaktadır:

* Güçlü bir yönetim, çoklu oturum açma Azure AD Kimlik Yönetimi'ni kullanarak özelliğini ve [Openıd Connect] [ OpenIDConnect] protokol uygulanması
* Aracılı erişimi [korunan kaynakları](#resource-server) tarafından [istemci uygulamaları](#client-application), Azure AD OAuth 2.0 aracılığıyla [yetkilendirme sunucusu](#authorization-server) uygulama
* [Onay framework](#consent) kaynak sahibi kimlik bağlı olarak korunan kaynakları'na istemci erişimini yönetme.

Bkz: [uygulamaları Azure Active Directory ile tümleştirme] [ AAD-Integrating-Apps] daha fazla ayrıntı için.

## <a name="authentication"></a>Kimlik doğrulaması
Kimlik ve erişim denetimi için kullanılacak bir güvenlik sorumlusu oluşturulması için temel sağlayan bir taraf yasal kimlik bilgileri, zor işlemi. Sırasında bir [OAuth2 yetkilendirme verme](#authorization-grant) kimlik doğrulaması taraf rolü ya da, örneğin, doldurma [kaynak sahibi](#resource-owner) veya [istemci uygulaması](#client-application)bağlı olarak kullanılan verin.

## <a name="authorization"></a>Yetkilendirme
Bir şey yapmak için bir kimliği doğrulanmış güvenlik sorumlusu izin verme eylemi. Azure AD programlama modelinde iki birincil kullanım örnekleri şunlardır:

* Sırasında bir [OAuth2 yetkilendirme verme](#authorization-grant) akış: zaman [kaynak sahibi](#resource-owner) yetkisi verir [istemci uygulaması](#client-application), kaynağa erişmesine izin vermeden Sahibin kaynaklar.
* İstemci tarafından kaynak erişimi sırasında: tarafından uygulanan [kaynak sunucusu](#resource-server)kullanarak [talep](#claim) değerleri sunmak [erişim belirteci](#access-token) erişim denetimi kararlar almak için Bunlar üzerine temel.

## <a name="authorization-code"></a>Yetkilendirme kodu
Bir kısa süreli "token" için sağlanan bir [istemci uygulaması](#client-application) tarafından [yetkilendirme uç noktası](#authorization-endpoint), dört OAuth2 birini "yetkilendirme kodu" akışının parçası olarak [yetkilendirmeverir](#authorization-grant). Kimlik doğrulaması yanıt istemci uygulamasına döndürülen kodu bir [kaynak sahibi](#resource-owner), kaynak sahibi belirten istenen kaynaklara erişmek için yetki temsilci. Akış bir parçası olarak, kodu daha sonra için kullanılan bir [erişim belirteci](#access-token).

## <a name="authorization-endpoint"></a>Yetkilendirme uç noktası
Tarafından uygulanan uç noktalardan biri [yetkilendirme sunucusu](#authorization-server)ile etkileşim kurmak için kullanılan [kaynak sahibi](#resource-owner) sağlamak amacıyla bir [yetkilendirme verme](#authorization-grant) bir OAuth2 sırasında Yetkilendirme akışı verin. Kullanılan yetkilendirme grant akışa bağlı olarak sağlanan gerçek verin, dahil olmak üzere değişebilir bir [yetkilendirme kodu](#authorization-code) veya [güvenlik belirteci](#security-token).

OAuth2 belirtimi 's bkz [yetki türleri] [ OAuth2-AuthZ-Grant-Types] ve [yetkilendirme uç noktası] [ OAuth2-AuthZ-Endpoint] bölümler ve [Openıdconnect belirtimi] [ OpenIDConnect-AuthZ-Endpoint] daha fazla ayrıntı için.

## <a name="authorization-grant"></a>Yetkilendirme verme
Kimlik bilgisi temsil eden bir [kaynak sahibinin](#resource-owner) [yetkilendirme](#authorization) izni verilir, korunan kaynaklara erişim için bir [istemci uygulaması](#client-application). Bir istemci uygulaması birini kullanabilirsiniz [dört verme OAuth2 yetkilendirme çerçevesi tarafından tanımlanan türlerini] [ OAuth2-AuthZ-Grant-Types] istemci türü/gereksinimlerine bağlı olarak bir grant elde edilir: "yetkilendirme kodu verme", "istemci kimlik bilgileri vermenizi","örtük grant"ve"kaynak sahibi parolası kimlik bilgileri verin". İstemciye döndürülen kimlik bilgileri olan bir [erişim belirteci](#access-token), veya bir [yetkilendirme kodu](#authorization-code) (daha sonra bir erişim belirteci için değiştirilen), yetkilendirme verme kullanılan türüne bağlı olarak.

## <a name="authorization-server"></a>Yetkilendirme sunucusu
Tarafından tanımlanan [OAuth2 yetkilendirme Framework][OAuth2-Role-Def], sunucunun erişimi vermekten sorumlu belirteçler için [istemci](#client-application) başarıylakimlikdoğrulandıktansonra[kaynak sahibi](#resource-owner) ve kendi yetkilendirme alma. A [istemci uygulaması](#client-application) çalışma zamanında yetkilendirme sunucusu ile etkileşime giren kendi [yetkilendirme](#authorization-endpoint) ve [belirteci](#token-endpoint) uç noktaları, OAuth2 uygun olarak tanımlanan [yetkilendirme verir](#authorization-grant).

Örneğin yetkilendirme sunucusu rolü için Azure AD uygulamaları ve Microsoft hizmet API'leri, Azure AD uygulama tümleştirmesi söz konusu olduğunda, Azure AD uygulayan [Microsoft Graph API][Microsoft-Graph].

## <a name="claim"></a>Talep
A [güvenlik belirteci](#security-token) bir varlık onaylar sağlayan talepleri içerir (gibi bir [istemci uygulaması](#client-application) veya [kaynak sahibi](#resource-owner)) (örneğin, başkabirvarlıkiçin[kaynak sunucusu](#resource-server)). Talep belirteci konu hakkında bilgiler geçiş ad/değer çiftleri olan (örneğin, tarafından doğrulandı güvenlik sorumlusu [yetkilendirme sunucusu](#authorization-server)). Verilen belirteçte talep belirteci, konu, uygulama yapılandırması, vb. kimliğini doğrulamak için kullanılan kimlik bilgileri türünü türünü de dahil olmak üzere çeşitli değişkenler bağlıdır.

Bkz: [Azure AD belirteç başvurusu] [ AAD-Tokens-Claims] daha fazla ayrıntı için.

## <a name="client-application"></a>istemci uygulaması
Tarafından tanımlanan [OAuth2 yetkilendirme Framework][OAuth2-Role-Def], adına kaynak istekleri yapan bir uygulamada korunan [kaynak sahibi](#resource-owner). "İstemci" terimi (örneğin, olup uygulama bir sunucu, bir masaüstü veya diğer cihazları yürütür) herhangi belirli donanım uygulama özelliklerine göstermez.  

Bir istemci uygulaması ister [yetkilendirme](#authorization) 'na katılmak için bir kaynak sahibinden bir [OAuth2 yetkilendirme verme](#authorization-grant) akış ve API/veri kaynak sahibinin adınıza erişebilir. OAuth2 yetkilendirme Framework [iki istemci türlerini tanımlayan][OAuth2-Client-Types], "gizli" ve "Genel", istemcinin kimlik bilgilerini gizliliğini korumak becerisini göre. Uygulamaları uygulayabilirsiniz bir [web istemcisi (gizli)](#web-client) bir web sunucusunda çalışan bir [yerel istemci (Genel)](#native-client) bir aygıtta yüklü veya [kullanıcı aracısı-tabanlı istemci (Genel)](#user-agent-based-client)bir cihazın tarayıcısında çalıştırır.

## <a name="consent"></a>Onay
İşlemi bir [kaynak sahibi](#resource-owner) yetkisi verme bir [istemci uygulaması](#client-application), özel korumalı kaynaklarınıza erişmek için [izinleri](#permissions), adına Kaynak sahibi. İstemci tarafından istenilen izinlerine bağlı olarak, bir yönetici veya kullanıcı sırasıyla kuruluşun/tek verilerine erişmesine izin vermek için izin istenir. Unutmayın, içinde bir [çok kiracılı](#multi-tenant-application) senaryosu, uygulamanın [hizmet sorumlusu](#service-principal-object) consenting kullanıcı Kiracı da kaydedilir.

## <a name="id-token"></a>ID simgesi
Bir [Openıd Connect] [ OpenIDConnect-ID-Token] [güvenlik belirteci](#security-token) tarafından sağlanan bir [yetkilendirme sunucunun](#authorization-server) [yetkilendirme uç noktası](#authorization-endpoint), içeren [talep](#claim) bir son kullanıcı kimlik doğrulaması için ilgili [kaynak sahibi](#resource-owner). Gibi bir erişim belirteci, ayrıca kimlik belirteçlerini temsil dijital olarak imzalanmış olarak [JSON Web Token (JWT)][JWT]. Farklı bir erişim belirteci olarak yine de bir kimliği belirtecin talep kaynağına erişimle ilgili amaçlar için kullanılmaz ve özellikle erişim denetimi.

Bkz: [Azure AD belirteç başvurusu] [ AAD-Tokens-Claims] daha fazla ayrıntı için.

## <a name="multi-tenant-application"></a>çok kiracılı uygulama
Oturum açma sağlayan uygulama sınıfının ve [onayı](#consent) tüm Azure AD içinde sağlanan kullanıcılar tarafından [Kiracı](#tenant), kiracılar, istemci kayıtlı bir dışında dahil olmak üzere. [Yerel istemci](#native-client) uygulamalar varsayılan olarak, çok kiracılı ancak [web istemcisi](#web-client) ve [web kaynak/API](#resource-server) uygulamalara sahip tek veya birden çok Kiracı arasında seçme özelliği. Bunun aksine, tek-Kiracı olarak kayıtlı bir web uygulaması yalnızca izin oturum açma işlemleri aynı Kiracı biri olarak sağlanan kullanıcı hesaplarından uygulama, kayıtlı.

Bkz: [çok kiracılı uygulama desenini kullanarak Azure AD alanındaki herhangi bir kullanıcı oturum nasıl] [ AAD-Multi-Tenant-Overview] daha fazla ayrıntı için.

## <a name="native-client"></a>yerel istemci
Bir tür [istemci uygulaması](#client-application) yüklü olan yerel bir aygıtta. Tüm kod, bir cihazda yürütülür olduğundan, "Genel" istemci kimlik bilgileri özel olarak/ilkemiz depolamak için sorunu nedeniyle olarak kabul edilir. Bkz: [OAuth2 istemci türleri ve profiller] [ OAuth2-Client-Types] daha fazla ayrıntı için.

## <a name="permissions"></a>İzinleri
A [istemci uygulaması](#client-application) erişim kazanır bir [kaynak sunucusu](#resource-server) izin istekleri bildirme tarafından. İki tür mevcuttur:

* "Belirtin izinleri atanmış" [kapsam tabanlı](#scopes) imzalı bileşeninden temsilci yetkilendirme kullanarak erişim [kaynak sahibi](#resource-owner), kaynak çalışma zamanında sunulan ["scp" Talep](#claim) istemciye ait [erişim belirteci](#access-token).
* Belirtin "Uygulama" izinleri [rol tabanlı](#roles) istemci uygulamanın kimlik bilgileri/kimliğini kullanarak erişmek için kaynak çalışma zamanında sunulan ["rol" talep](#claim) istemciye ait erişim belirteci.

Bunlar sırasında yüzey [onayı](#consent) işlemi, yöneticiye veya kaynak sahibi olarak kiracısındaki kaynaklarına istemci erişimi verme/reddetme fırsatı verir.

İzin istekleri yapılandırılmış olan "Uygulamaları" / "Ayarlar" sekmesini [Azure portal][AZURE-portal], "Gerekli istenen"İzinlere temsilci"ve"uygulama seçerek izinler altında" İzinler"(ikinci genel yönetici rolü üyeliği gerekir). Çünkü bir [ortak istemci](#client-application) kimlik bilgileri, güvenli bir şekilde korunamaz izinlere temsilci, yalnızca isteyebilir sırada bir [gizli istemci](#client-application) yetkilendirilen ve uygulama isteği olanağı vardır izinler. İstemcinin [uygulama nesnesi](#application-object) bildirilen izinler depolar, [requiredResourceAccess özelliği][AAD-Graph-App-Entity].

## <a name="resource-owner"></a>Kaynak sahibi
Tarafından tanımlanan [OAuth2 yetkilendirme Framework][OAuth2-Role-Def], korunan bir kaynağa erişim verilirken yeteneğine sahip bir varlık. Kaynak sahibi bir kişi olduğunda, son kullanıcı olarak bilinir. Örneğin, bir [istemci uygulaması](#client-application) bir kullanıcının posta üzerinden erişmek istiyor [Microsoft Graph API][Microsoft-Graph], kaynak sahibi izni gerektirir posta kutusu.

## <a name="resource-server"></a>Kaynak sunucusu
Tarafından tanımlanan [OAuth2 yetkilendirme Framework][OAuth2-Role-Def], ana bilgisayar kaynakları, kabul etme ve yanıtlama özellikli korunan bir sunucu kaynak istekleri tarafından korunan [istemci uygulamaları](#client-application) , mevcut bir [erişim belirteci](#access-token). Olarak da bilinen bir korumalı kaynak sunucuda veya kaynak uygulama.

Kaynak sunucuda API'lerini gösterir ve korumalı kaynaklarına erişimini zorunlu kılar [kapsamları](#scopes) ve [rolleri](#roles), OAuth 2.0 yetkilendirme Framework kullanarak. Örnek, Azure AD Kiracı verilere erişim sağlayan Azure AD Graph API ve posta ve takvim gibi verilere erişim sağlayan Office 365 API'leri verilebilir. Bunların her ikisi de aynı zamanda aracılığıyla erişilebilir [Microsoft Graph API][Microsoft-Graph].  

Yalnızca bir istemci uygulaması gibi kaynak uygulamanın kimlik yapılandırması üzerinden kurulur [kayıt](#application-registration) bir Azure AD kiracısında uygulama ve hizmet sorumlusu nesnesi sağlar. Azure AD grafik API'si gibi bazı Microsoft tarafından sağlanan API'leri sağlama işlemi sırasında tüm kiracılar kullanılabilir hale hizmet asıl adı önceden kaydettiniz.

## <a name="roles"></a>roles
Gibi [kapsamları](#scopes), rollerini sağlamak için bir yol bir [kaynak sunucusu](#resource-server) korunan kaynaklara erişimi yönetmek üzere. İki tür vardır: bir "kullanıcı" rolü "uygulama" rolü için aynı uygulayan sırada kaynağa erişim gerektiren kullanıcıları/grupları için rol tabanlı erişim denetimi uygulayan [istemci uygulamaları](#client-application) erişimi gerektirir.

Rolleri, kaynak tanımlı dizeleri (örneğin "gider onaylayıcı", "Salt okunur", "Directory.ReadWrite.All"), içinde yönetilen [Azure portal] [ AZURE-portal] kaynağın aracılığıyla [uygulama bildirim](#application-manifest)ve kaynağın depolanan [appRoles özelliği][AAD-Graph-Sp-Entity]. Azure portalı da kullanıcılar "kullanıcı" rolü atayın ve istemci yapılandırmak için kullanılan [uygulama izinleri](#permissions) "uygulama" rolü erişmek için.

Azure AD grafik API'si tarafından kullanıma sunulan uygulama rolleri hakkında ayrıntılı bilgi için bkz: [grafik API'si izin kapsamları][AAD-Graph-Perm-Scopes]. Adım adım uygulama örnek için bkz: [rol tabanlı erişim denetimi kullanarak Azure AD bulut uygulamalarında][Duyshant-Role-Blog].

## <a name="scopes"></a>Kapsamları
Gibi [rolleri](#roles), kapsamları sağlamak için bir yol bir [kaynak sunucusu](#resource-server) korunan kaynaklara erişimi yönetmek üzere. Kapsamlar uygulamak için kullanılan [kapsam tabanlı] [ OAuth2-Access-Token-Scopes] için erişim denetimi, bir [istemci uygulaması](#client-application) , verildiği atanmış erişim için kaynak sahibi tarafından.

Kapsamları dizelerdir yönetilen kaynak tanımlı (örneğin "Mail.Read", "Directory.ReadWrite.All"), [Azure portal] [ AZURE-portal] kaynağın aracılığıyla [uygulama bildirimi](#application-manifest)ve kaynağın depolanan [oauth2Permissions özelliği][AAD-Graph-Sp-Entity]. Azure portal ayrıca istemci uygulamasını yapılandırmak için kullanılan [izinleri atanmış](#permissions) kapsam erişmek için.

En iyi yöntem adlandırma kuralı, "resource.operation.constraint" biçimini kullanın etmektir. Azure AD grafik API'si tarafından kullanıma sunulan kapsamları hakkında ayrıntılı bilgi için bkz: [grafik API'si izin kapsamları][AAD-Graph-Perm-Scopes]. Office 365 Hizmetleri tarafından sunulan kapsamlar için bkz: [Office 365 API izinleri başvurusu][O365-Perm-Ref].

## <a name="security-token"></a>Güvenlik belirteci
OAuth2 belirteci veya SAML 2.0 onaylama gibi talepleri içeren imzalı bir belge. Bir OAuth2 için [yetkilendirme verme](#authorization-grant), bir [erişim belirteci](#access-token) (OAuth2) ve bir [kimliği belirteci](http://openid.net/specs/openid-connect-core-1_0.html#IDToken) güvenlik belirteçleri, her ikisi de olarak uygulanır türleridir bir [JSON Web Token (JWT)][JWT].

## <a name="service-principal-object"></a>Hizmet sorumlusu nesnesi
Ne zaman, kaydetme/güncelleştirme bir uygulamada [Azure portal][AZURE-portal], portal/hem güncelleştirme bir [uygulama nesnesi](#application-object) ve karşılık gelen bir hizmet sorumlusu nesnesi Bu Kiracı için. Uygulama nesnesi *tanımlar* (arasında ilişkili uygulama verildiğini erişim genel olarak tüm kiracılar), uygulamanın kimlik yapılandırması ve şablondan, karşılık gelen hizmet sorumlusu nesneleri olan *türetilmiş* yerel olarak çalışma zamanında (içinde belirli bir kiracı) kullanmak için.

Bkz: [uygulama ve hizmet sorumlusu nesneleri] [ AAD-App-SP-Objects] daha fazla bilgi için.

## <a name="sign-in"></a>oturum açma
İşlemi bir [istemci uygulaması](#client-application) son kullanıcı kimlik doğrulamayı başlatan ve yakalama ilgili alınırken amacıyla durumu, bir [güvenlik belirteci](#security-token) ve bu durumda uygulama oturumuna kapsamı. Kullanıcı profili bilgileri gibi yapılarını içerebilir ve bu bilgileri belirteci talepleri türetilmiş durumu.

Bir uygulama oturum açma işlevini genellikle çoklu oturum açma (SSO) uygulamak için kullanılır. Bu aynı zamanda "kaydolma" bir işlev tarafından bir uygulamaya (bağlı ilk oturum açma) erişim kazanmak bir son kullanıcı için giriş noktası olarak öncesinde. Kaydolma işlevi toplayın ve kullanıcıya özgü ek durum kalıcı hale getirmek için kullanılır ve gerektirebilir [kullanıcı izni](#consent).

## <a name="sign-out"></a>oturum kapatma
Kullanıcı durumunu ayrılırken bir son kullanıcı ilişkili beklemediğiniz kimlik doğrulama işlemini [istemci uygulaması](#client-application) oturumu sırasında [oturum açma](#sign-in)

## <a name="tenant"></a>Kiracı
Azure AD dizini örneğini Azure AD kiracısı adlandırılır. Özellikler de dahil olmak üzere, çeşitli sağlar:

* Tümleşik uygulamalar için bir kayıt defteri hizmeti
* kimlik doğrulama kullanıcı hesapları ve kayıtlı uygulamalar
* OAuth2 ve SAML, dahil çeşitli protokoller desteklemek için gereken REST uç noktalarını da dahil olmak üzere [yetkilendirme uç noktası](#authorization-endpoint), [belirteç uç noktası](#token-endpoint) ve "Genel" uç nokta tarafından kullanılan [ çok kiracılı uygulamalara](#multi-tenant-application).

Bir kiracı, bir Azure AD ile de ilişkilidir veya abonelik için kimlik ve erişim yönetimi özellikleri sağlayarak bu aboneliğin sağlama sırasında Office 365 aboneliği. Bkz: [Azure Active Directory kiracısı alma] [ AAD-How-To-Tenant] çeşitli yollar hakkında ayrıntılar için erişim için bir kiracı elde edebilirsiniz. Bkz: [Azure aboneliklerinin Azure Active Directory ile ilişkili] [ AAD-How-Subscriptions-Assoc] abonelikleri ve Azure AD kiracısı arasındaki ilişki hakkında ayrıntılı bilgi için.

## <a name="token-endpoint"></a>belirteç uç noktası
Tarafından uygulanan uç noktalardan biri [yetkilendirme sunucusu](#authorization-server) destek OAuth2 [yetkilendirme verir](#authorization-grant). Grant bağlı olarak, bu almak için kullanılabilir bir [erişim belirteci](#access-token) (ve ilgili "Yenile" belirteç) için bir [istemci](#client-application), veya [kimliği belirteci](#ID-token) ile kullanıldığında [Openıd Bağlantı] [ OpenIDConnect] protokolü.

## <a name="user-agent-based-client"></a>İstemci kullanıcı aracısı tabanlı
Bir tür [istemci uygulaması](#client-application) , kodu bir web sunucusundan indirir ve bir kullanıcı aracısı içinde (örneğin, bir web tarayıcısı), bir tek sayfa uygulama (SPA) gibi yürütür. Tüm kod, bir cihazda yürütülür olduğundan, "Genel" istemci kimlik bilgileri özel olarak/ilkemiz depolamak için sorunu nedeniyle olarak kabul edilir. Bkz: [OAuth2 istemci türleri ve profiller] [ OAuth2-Client-Types] daha fazla ayrıntı için.

## <a name="user-principal"></a>Kullanıcı asıl adı
Benzer şekilde bir hizmet sorumlusu nesnesi uygulama örneğini temsil etmek için kullanılır, kullanıcı asıl nesne başka bir kullanıcı temsil eden güvenlik sorumlusu türüdür. Azure AD grafik [kullanıcı varlığı] [ AAD-Graph-User-Entity] kullanıcı ile ilgili özellikler adı ve Soyadı, vb. kullanıcı asıl adı, dizin rolü üyeliği de dahil olmak üzere bir kullanıcı nesnesi için şema tanımlar. Bu, kullanıcı asıl çalışma zamanında oluşturmak Azure AD kullanıcı kimlik yapılandırmasını sağlar. Kullanıcı asıl adı çoklu oturum açma için kimliği doğrulanmış bir kullanıcı göstermek için kullanılan kaydı [onayı](#consent) temsilci, erişim denetimi kararlarını, vb. yapma.

## <a name="web-client"></a>Web istemcisi
Bir tür [istemci uygulaması](#client-application) , kimlik bilgileri sunucuda güvenli bir şekilde depolayarak "gizli" istemci olarak çalışması için tüm kodu bir web sunucusunda ve mümkün yürütür. Bkz: [OAuth2 istemci türleri ve profiller] [ OAuth2-Client-Types] daha fazla ayrıntı için.

## <a name="next-steps"></a>Sonraki adımlar
[Azure AD Geliştirici Kılavuzu] [ AAD-Dev-Guide] tüm Azure AD geliştirme için kullanılacak portalıdır ilgili konular, genel bir bakış da dahil olmak üzere [uygulama tümleştirmesi] [ AAD-How-To-Integrate] ve temelleri [Azure AD kimlik doğrulama ve desteklenen kimlik doğrulama senaryoları][AAD-Auth-Scenarios].

Lütfen geri bildirim sağlamak ve iyileştirmek ve yeni tanımları istekleri dahil olmak üzere veya var olanları güncelleştirme bizim içerik şekil yardımcı olmak için aşağıdaki Açıklamalar bölümüne kullanın!

<!--Image references-->

<!--Reference style links -->
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AAD-Graph-User-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity
[AAD-How-Subscriptions-Assoc]: ../active-directory-how-subscriptions-associated-directory.md
[AAD-How-To-Integrate]: ./active-directory-how-to-integrate.md
[AAD-How-To-Tenant]: active-directory-howto-tenant.md
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Multi-Tenant-Overview]: active-directory-devhowto-multi-tenant-overview.md
[AAD-Security-Token-Claims]: ./active-directory-authentication-scenarios/#claims-in-azure-ad-security-tokens
[AAD-Tokens-Claims]: ./active-directory-token-and-claims.md
[AZURE-portal]: https://portal.azure.com
[Duyshant-Role-Blog]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
[JWT]: https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32
[Microsoft-Graph]: https://graph.microsoft.io
[O365-Perm-Ref]: https://msdn.microsoft.com/en-us/office/office365/howto/application-manifest
[OAuth2-Access-Token-Scopes]: https://tools.ietf.org/html/rfc6749#section-3.3
[OAuth2-AuthZ-Endpoint]: https://tools.ietf.org/html/rfc6749#section-3.1
[OAuth2-AuthZ-Grant-Types]: https://tools.ietf.org/html/rfc6749#section-1.3
[OAuth2-Client-Types]: https://tools.ietf.org/html/rfc6749#section-2.1
[OAuth2-Role-Def]: https://tools.ietf.org/html/rfc6749#page-6
[OpenIDConnect]: http://openid.net/specs/openid-connect-core-1_0.html
[OpenIDConnect-AuthZ-Endpoint]: http://openid.net/specs/openid-connect-core-1_0.html#AuthorizationEndpoint
[OpenIDConnect-ID-Token]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken
