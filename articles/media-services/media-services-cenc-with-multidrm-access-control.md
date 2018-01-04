---
title: "Çoklu DRM ve erişim denetimi ile CENC: bir başvuru tasarım ve uygulama Azure ve Azure Media Services | Microsoft Docs"
description: "Microsoft kesintisiz akış istemci bağlantı noktası oluşturma Seti Lisansı hakkında bilgi edinin."
services: media-services
documentationcenter: 
author: willzhan
manager: cfowler
editor: 
ms.assetid: 7814739b-cea9-4b9b-8370-538702e5c615
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: willzhan;kilroyh;yanmf;juliako
ms.openlocfilehash: cac1513d805e01f2c9633b3b318ad6808027904e
ms.sourcegitcommit: 6f33adc568931edf91bfa96abbccf3719aa32041
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/22/2017
---
# <a name="cenc-with-multi-drm-and-access-control-a-reference-design-and-implementation-on-azure-and-azure-media-services"></a>Çoklu DRM ve erişim denetimi ile CENC: bir başvuru tasarım ve uygulama Azure ve Azure Media Services
 
## <a name="introduction"></a>Giriş
Tasarlama ve dijital hak yönetimi (DRM) alt sistemi bir üzerinden--top (OTT) için derleme veya çevrimiçi çözüm akış karmaşık bir görevdir. İşleçler/çevrimiçi video sağlayıcıları, genellikle özel DRM hizmet sağlayıcıları için bu görev dış. Bu belgede bir başvuru tasarım ve uygulama bir uçtan uca DRM alt sisteminin bir OTT veya çevrimiçi akış çözüm sunmak için hedefidir.

Bu belge için hedeflenen okuyucular DRM alt sistemleri OTT veya çevrimiçi akış/multiscreen çözümleri ya da DRM alt ilgilenen okuyucular çalışma mühendisleri ' dir. Okuyucular DRM teknolojileri PlayReady, Widevine, FairPlay veya Adobe erişim gibi piyasadaki en az biri alışık olduğunuz varsayılır.

Bu DRM tartışmada biz de çoklu DRM ile ortak şifreleme (CENC) içerir. Çevrimiçi akış ve OTT endüstri önemli bir eğilimi çeşitli istemci platformlarında çok yerel DRM ile CENC kullanmaktır. Bu eğilimin bir kaydırma çoklu DRM ve kendi İstemci SDK çeşitli istemci platformları için kullanılan önceki bir ' dir. Birden çok yerel DRM ile CENC kullandığınızda, PlayReady ve Widevine başına şifrelenmiş [ortak şifreleme (ISO/IEC 23001-7 CENC)](http://www.iso.org/iso/home/store/catalogue_ics/catalogue_detail_ics.htm?csnumber=65271/) belirtimi.

Bu BT çoklu DRM ile CENC avantajları şunlardır:

* Tek şifreleme işlemi yerel DRMs farklı platformlara hedeflemek için kullanılan şifreleme maliyeti azaltır.
* Yalnızca tek bir kopyasını şifrelenmiş varlıklar gerektiğinden şifrelenmiş varlıklarını yönetme maliyetini azaltır.
* DRM istemci yerel DRM istemcisi yerel platformda genellikle boş olduğu için maliyet lisansı ortadan kaldırır.

Microsoft, tire ve CENC etkin bir promoter bazı önemli endüstri oynatıcıları birlikte verir. Azure Media Services, tire ve CENC desteği sağlar. Son Duyurular için aşağıdaki Bloglara bakın:

*  [Azure Media Services’ta Google Widevine lisans teslim hizmetleri ile tanışın](https://azure.microsoft.com/blog/announcing-general-availability-of-google-widevine-license-services/)
* [Google Widevine paketlemeyi Çoklu DRM akış teslim etmek üzere Azure Media Services ekler](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/)  

### <a name="overview-of-this-article"></a>Bu makalede genel bakış
Bu makalede amaçlarını üzeresiniz:

* Çoklu DRM ile CENC kullanan bir DRM alt başvuru tasarımını sağlar.
* Bir başvuru uygulaması Azure/Media Services platformda sağlar.
* Bazı tasarım ve uygulama konularını ele alınmıştır.

Makalesinde "çoklu DRM" terimi aşağıdaki ürünleri kapsar:

* Microsoft PlayReady
* Google Widevine
* Apple FairPlay 

Aşağıdaki tabloda her DRM tarafından desteklenen tarayıcılar ve yerel platform/yerel uygulama özetler.

| **İstemci Platformu** | **Yerel DRM desteği** | **Tarayıcı/uygulaması** | **Akış biçimlerine** |
| --- | --- | --- | --- |
| **Akıllı TV, işleci STB, OTT STB** |PlayReady öncelikle, ve/veya Widevine ve/veya diğer |Linux, Opera, WebKit, diğer |Çeşitli biçimleri |
| **Windows 10 cihazları (Windows PC, Windows tabletler, Windows Phone, Xbox)** |PlayReady |MS kenar/IE11/EME<br/><br/><br/>Evrensel Windows platformu |TİRE (HLS için PlayReady desteklenmiyor)<br/><br/>DASH, kesintisiz akış (HLS için PlayReady desteklenmiyor) |
| **Android cihazlar (telefon, tablet, TV)** |Widevine |Chrome/EME |DASH, HLS |
| **iOS (iPhone, iPad), OS X istemcileri ve Apple TV** |FairPlay |Safari 8 +/ EME |HLS |


Dağıtım için her DRM geçerli durumunu göz önünde bulundurularak, bir hizmet genellikle en iyi şekilde uç noktalar tüm türleri adres emin olmak için iki veya üç DRMs uygulamak istiyor.

Hizmet mantığı karmaşıklığına ve belirli bir düzeyde kullanıcı deneyimi çeşitli istemcilere ulaşmak için istemci tarafında karmaşıklık arasında kolaylığını yoktur.

Seçim yapmak için göz önünde bulundurun:

* PlayReady ve neredeyse her platformda yazılım SDK aracılığıyla kullanılabilen bazı Android cihazlarda, her Windows aygıt yerel olarak uygulanır.
* Widevine her Android cihazı, Chrome ve diğer bazı cihazların yerel olarak uygulanır.
* FairPlay yalnızca iOS ve Mac OS istemciler veya iTunes aracılığıyla kullanılabilir.

Tipik bir çoklu DRM için iki seçenek vardır:

* PlayReady ve Widevine
* PlayReady ve Widevine FairPlay

## <a name="a-reference-design"></a>Bir başvuru tasarımı
Bu bölümde uygulamak için kullanılan teknolojiler bağımsızdır bir başvuru tasarımı gösterir.

DRM alt sistemi aşağıdaki bileşenleri içerir:

* Anahtar Yönetimi
* DRM paketleme
* DRM lisansı verme
* Yetkilendirme denetimi
* Kimlik doğrulama/yetkilendirme
* Oynatıcı
* Kaynak/içerik teslim ağı (CDN)

Aşağıdaki diyagramda DRM alt sistemi bileşenleri arasındaki üst düzey etkileşimi gösterilmektedir:

![DRM alt sistemi ile CENC](./media/media-services-cenc-with-multidrm-access-control/media-services-generic-drm-subsystem-with-cenc.png)

Tasarım üç temel katmanı vardır:

* Arka ofis katmanındaki (siyah) harici olarak gösterilmez.
* DMZ katman (mavi) ortak yüz tüm uç noktaları içerir.
* Ortak bir internet Katmanı (mavi), ortak Internet üzerinden trafik oyuncularla ve CDN içerir.

Ayrıca olmamalıdır denetim statik veya dinamik şifreleme olmasından bağımsız olarak DRM koruması için bir içerik yönetim aracı. DRM şifreleme için girdiler şunlardır:

* MBR video içeriği
* İçerik anahtarı
* Lisans edinme URL'leri

Kayıttan yürütme süresinde üst düzey akış şöyledir:

* Kullanıcının kimliği doğrulanır.
* Bir yetkilendirme belirteci kullanıcı için oluşturulur.
* DRM korumalı içeriği (bildirim) aygıta yüklenir.
* Lisans edinme isteği bir anahtarı ile birlikte lisans sunucularına player gönderir kimliği ve bir yetki belirteci.

Aşağıdaki bölümde, anahtar yönetimi tasarımını anlatılmaktadır.

| **ContentKey varlık** | **Senaryo** |
| --- | --- |
| 1-1 |En basit durumda. En iyi denetim sağlar. Ancak, bu düzenleme genellikle en yüksek lisans teslimat maliyeti sonuçlanır. En az bir lisans isteği korunan her varlık için gereklidir. |
| 1-çok |Aynı içerik anahtarı için birden çok varlığı kullanabilirsiniz. Örneğin, tüm varlıklar için bir mantıksal grup içindeki, bir tarzını veya bir alt kümesini bir tarzını (veya film gene) gibi tek bir içerik anahtarı kullanabilirsiniz. |
| -1 |Birden çok içerik anahtarı her varlık için gereklidir. <br/><br/>Örneğin, MPEG-DASH için çoklu DRM ile dinamik CENC koruma ve HLS için dinamik AES-128 şifrelemesini uygulamanız gerekiyorsa, iki ayrı içerik anahtarı gerekir. Her içerik anahtarı kendi ContentKeyType gerekir. (Dinamik CENC koruma için kullanılan içerik anahtarı için ContentKeyType.CommonEncryption kullanın. Dinamik AES-128 şifreleme için kullanılan içerik anahtarı için ContentKeyType.EnvelopeEncryption kullanın.)<br/><br/>Başka bir örnek olarak, teorik, tire içeriğin CENC koruma, bir içerik anahtarı video akışına ve ses akışı korumak için başka bir içerik anahtarı korumak için kullanabilirsiniz. |
| Çok- |Önceki iki senaryo birleşimi. İçerik anahtarı bir dizi her aynı varlık grubunda birden çok varlıklar için kullanılır. |

Dikkate alınması gereken başka bir önemli kalıcı ve kalıcı olmayan lisans kullanımını faktördür.

Bu noktalar neden önemlidir?

Lisans teslimat için bir genel bulut kullanırsanız, kalıcı ve kalıcı olmayan lisansları lisans teslimat maliyetine doğrudan bir etkiye sahiptir. Aşağıdaki iki farklı tasarım göstermeye durumlarına:

* Aylık abonelik: kalıcı lisans ve 1-çok içerik anahtarı varlık eşleme kullanın. Örneğin, tüm çocuk filmler için tek bir içerik anahtarı şifreleme için kullanırız. Bu durumda:

    Tüm çocuklar filmler/aygıt için istenen lisans toplam sayısı = 1

* Aylık abonelik: kalıcı olmayan bir lisans ve içerik anahtarı ve varlık arasında 1-1 eşleme kullanın. Bu durumda:

    Tüm çocuklar filmler/aygıt için istenen lisans toplam sayısı = [izlenen filmler sayısı] x [oturum sayısı]

İki farklı tasarımları çok farklı lisans isteği düzenleri neden. Farklı lisans teslimat lisans teslimat hizmeti Media Services gibi genel bulut tarafından sağlanıyorsa maliyet farklı desenleri sonuçlanır.

## <a name="map-design-to-technology-for-implementation"></a>Uygulama için teknoloji tasarım Eşle
Ardından, genel tasarım Azure/Media Services platform teknolojilerini her Yapı bloğu için kullanmak üzere hangi teknoloji belirterek eşleştirilir.

Aşağıdaki tablo eşleme gösterir.

| **Yapı Taşı** | **Teknoloji** |
| --- | --- |
| **Player** |[Azure Media Player](https://azure.microsoft.com/services/media-services/media-player/) |
| **Kimlik sağlayıcıyı (IDP)** |Azure Active Directory (Azure AD) |
| **Güvenlik belirteci hizmeti (STS)** |Azure AD |
| **DRM koruma iş akışı** |Media Services dinamik koruma |
| **DRM lisansı teslimi** |* Media Services lisans teslimat (PlayReady, Widevine, FairPlay) <br/>* Axinom lisans sunucusu <br/>* Özel PlayReady lisans sunucusu |
| **Kaynak** |Media Services akış uç noktası |
| **Anahtar yönetimi** |Başvuru uygulaması için gerekli değil |
| **İçerik yönetimi** |Bir C# konsol uygulaması |

Diğer bir deyişle, IDP ve STS Azure AD ile kullanılır. [Azure Media Player API](http://amp.azure.net/libs/amp/latest/docs/) player için kullanılır. Media Services ve Media Player çoklu DRM ile DASH ve CENC destekler.

Aşağıdaki diyagramda, genel yapısı ve önceki teknolojisi eşleme ile akışı gösterilmektedir:

![Media Services CENC](./media/media-services-cenc-with-multidrm-access-control/media-services-cenc-subsystem-on-AMS-platform.png)

Dinamik CENC şifrelemesi ayarlamak için aşağıdaki girişleri içerik yönetimi aracını kullanır:

* Açık içerik
* Anahtarından içerik anahtarı oluşturma/yönetimi
* Lisans edinme URL'leri
* Azure AD'den bilgi listesi

İçerik Yönetimi Aracı çıktısı şöyledir:

* ContentKeyAuthorizationPolicy DRM lisans belirtimleri ve lisans teslimat bir JSON Web Token (JWT) nasıl doğrular üzerinde belirtimi içeriyor.
* AssetDeliveryPolicy biçimi, DRM koruma ve lisans edinme URL'leri akış üzerindeki özellikleri içerir.

Çalışma zamanı sırasında akışı şöyledir:

* Kullanıcı kimlik doğrulaması sırasında bir JWT oluşturulur.
* JWT alan talepler grup nesnesi kimliği EntitledUserGroup içeren gruplar talep biridir. Bu talep yetkilendirme onay geçirmek için kullanılır.
* Player istemci bildirimi CENC korunan içeriği indirir ve aşağıdakileri tanımlar:
   * Anahtarı kimliği.
   * İçeriği korumalı CENC olur.
   * Lisans edinme URL'leri.
* Player desteklenen tarayıcı/DRM üzerinde temel lisans edinme isteğinde bulunur. Kimliği ve JWT de gönderilen lisans edinme istekte, anahtar. Lisans teslimat hizmeti JWT ve gerekli lisans vermeden önce yer alan talepler doğrular.

## <a name="implementation"></a>Uygulama
### <a name="implementation-procedures"></a>Uygulama yordamları
Uygulaması aşağıdaki adımları içerir:

1. Test varlıklar hazırlayın. Media Services Çoklu bit hızlı parçalanmış MP4 test videoya kodlamak/paketi. Bu varlık *değil* DRM korumalı. DRM korumasını daha sonra dinamik Koruması tarafından gerçekleştirilir.

2. Bir anahtar oluşturma kimliği ve bir içerik anahtarı (isteğe bağlı olarak anahtar çekirdek). Bu örnekte, anahtar yönetimi sistemi olduğundan gerekli değil yalnızca tek bir anahtar kimliği ve içerik anahtarı test varlıklar birkaç için gerekli.

3. Media Services API test varlık için çoklu DRM lisans teslimat hizmetlerini yapılandırmak için kullanın. Şirketiniz veya şirketinizin satıcılar Media Services lisans Hizmetleri'nde yerine özel lisans sunucuları kullanıyorsanız bu adımı atlayabilirsiniz. Lisans teslimat yapılandırdığınızda, lisans edinme URL'leri adımda belirtebilirsiniz. Media Services API yetkilendirme ilkesi kısıtlama ve farklı DRM lisans Hizmetleri için lisans yanıt şablonlar gibi bazı ayrıntılı yapılandırmaları belirtmek için gereklidir. Şu anda Azure portalı, bu yapılandırma için gerekli kullanıcı Arabirimi sağlamaz. API düzeyi bilgi ve örnek kod için bkz: [kullanım PlayReady ve/veya Widevine dinamik ortak şifreleme](media-services-protect-with-playready-widevine.md).

4. Media Services API test varlık için varlık teslim ilkesini yapılandırmak için kullanın. API düzeyi bilgi ve örnek kod için bkz: [kullanım PlayReady ve/veya Widevine dinamik ortak şifreleme](media-services-protect-with-playready-widevine.md).

5. Oluşturun ve Azure AD kiracısı Azure içinde yapılandırın.

6. Birkaç kullanıcı hesapları ve grupları, Azure AD kiracınızda oluşturun. En az bir "Kullanıcı hakkı" grubu oluşturun ve bu gruba bir kullanıcı ekleyebilirsiniz. Bu gruptaki kullanıcılar lisans edinme yetkilendirme denetimi başarılı. Bu gruptaki kullanıcılar kimlik doğrulama denetimi geçirmek başarısız ve bir lisans alınamıyor. Azure AD tarafından verilen JWT gerekli gruplara talepten bu "Kullanıcı hakkı" grubu gerekliliktir. Çoklu DRM lisans teslimat hizmetlerini yapılandırdığınızda bir adımda bu talep gereksinim belirtin.

7. Video oynatıcı barındırmak için bir ASP.NET MVC uygulaması oluşturursunuz. Bu ASP.NET uygulamasını kullanıcı kimlik doğrulamasıyla Azure AD kiracısı karşı korunur. Uygun talep kullanıcı kimlik doğrulamasından sonra elde erişim belirteçleri dahil edilmiştir. Bu adım için Openıd Connect API öneririz. Aşağıdaki NuGet paketlerini yükleyin:

   * Install-Package Microsoft.Azure.ActiveDirectory.GraphClient
   * Install-Package Microsoft.Owin.Security.OpenIdConnect
   * Install-Package Microsoft.Owin.Security.Cookies
   * Install-Package Microsoft.Owin.Host.SystemWeb
   * Install-Package Microsoft.IdentityModel.Clients.activedirectory tarafından

8. Kullanarak bir oynatıcı oluşturma [Azure Media Player API](http://amp.azure.net/libs/amp/latest/docs/). Kullanmak [Azure Media Player ProtectionInfo API](http://amp.azure.net/libs/amp/latest/docs/) hangi DRM teknolojinin farklı DRM platformlarda kullanılacağını belirtmek için.

9. Aşağıdaki tabloda testi matris gösterir.

    | **DRM** | **Tarayıcı** | **Yetkili kullanıcı için sonuç** | **Sonuç unentitled kullanıcı için** |
    | --- | --- | --- | --- |
    | **PlayReady** |Microsoft Edge veya Internet Explorer 11 Windows 10 |Başarılı |Başarısız |
    | **Widevine** |Windows 10 Chrome |Başarılı |Başarısız |
    | **FairPlay** |TBD | | |

Bir ASP.NET MVC oynatıcı uygulaması için Azure AD ayarlama hakkında daha fazla bilgi için bkz: [Azure Media Services OWIN MVC tabanlı bir uygulamanın Azure Active Directory ile tümleştirme ve JWT talepleri temelinde içerik anahtar teslim kısıtlamak](http://gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

Daha fazla bilgi için bkz: [JWT belirteci kimlik doğrulaması Azure Media Services ve dinamik şifreleme](http://gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/).  

Azure AD hakkında daha fazla bilgi için:

* Geliştirici bilgiler bulabilirsiniz [Azure Active Directory Geliştirici Kılavuzu](../active-directory/active-directory-developers-guide.md).
* Yönetici bilgileri bulabilirsiniz [Azure AD Kiracı dizinini yönetme](../active-directory/active-directory-administer.md).

### <a name="some-issues-in-implementation"></a>Uygulamasındaki bazı sorunlar
Uygulama sorunları ile ilgili Yardım için aşağıdaki sorun giderme bilgileri kullanın.

* URL ile bitmelidir veren "/". Hedef kitleyi player uygulama istemci kimliği olmalıdır Ayrıca, Ekle "/" veren URL sonunda.

        <add key="ida:audience" value="[Application Client ID GUID]" />
        <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/" />

    İçinde [JWT Decoder](http://jwt.calebb.net/), gördüğünüz **aud** ve **ISS**JWT gösterildiği gibi:

    ![JWT](./media/media-services-cenc-with-multidrm-access-control/media-services-1st-gotcha.png)

* Azure AD uygulama izinleri eklemek **yapılandırma** uygulama sekmesinde. Her uygulama için hem yerel hem de dağıtılan sürümleri izinleri gereklidir.

    ![İzinler](./media/media-services-cenc-with-multidrm-access-control/media-services-perms-to-other-apps.png)

* Dinamik CENC korumayı ayarlamadan doğru veren kullanın.

        <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/"/>

    Aşağıdakiler çalışmaz:

        <add key="ida:issuer" value="https://willzhanad.onmicrosoft.com/" />

    GUID Azure AD Kiracı kimliğidir. GUID bulunabilir **uç noktaları** Azure portalında açılır menü.

* GRANT grup üyeliği ayrıcalıkları talepleri. Azure AD uygulama bildirim dosyasında aşağıdaki olduğundan emin olun: 

    "groupMembershipClaims": "Tümü" (varsayılan değeri null)

* Kısıtlama gereksinimleri oluşturduğunuzda uygun TokenType ayarlayın.

        objTokenRestrictionTemplate.TokenType = TokenType.JWT;

    JWT (Azure AD) yanı sıra SWT (ACS) için destek eklemek için TokenType TokenType.JWT varsayılandır. SWT/ACS kullanırsanız, belirteç TokenType.SWT için ayarlamanız gerekir.

## <a name="additional-topics-for-implementation"></a>Uygulama için ek konular
Bu bölümde, tasarım ve uygulama bazı ek konular ele alınmıştır.

### <a name="http-or-https"></a>HTTP veya HTTPS?
ASP.NET MVC oynatıcı uygulaması aşağıdaki desteklemesi gerekir:

* HTTPS altında olan Azure AD üzerinden kullanıcı kimlik doğrulaması.
* JWT exchange istemci HTTPS altında olan Azure AD arasında.
* Lisans teslimat Media Services tarafından sağlanıyorsa, HTTPS altında olmalıdır istemci tarafından DRM lisans edinme. PlayReady ürün paketini HTTPS lisans teslimat için zorunlu kılabilir değil. PlayReady lisans sunucunuz Media Services dışında ise, HTTP veya HTTPS kullanabilirsiniz.

Media Player sayfasında HTTPS altında olacak şekilde ASP.NET oynatıcı uygulaması en iyi uygulama, HTTPS kullanır. Ancak, karışık içerik sorunu göz önünde bulundurmanız gereken şekilde HTTP Akış için tercih edilir.

* Tarayıcı karışık içeriğe izin vermiyor. Ancak eklentileri gibi Silverlight ve eklenti için OSMF kesintisiz ve tire sağlar. Karışık içerik, müşteri verileri risk neden kötü amaçlı JavaScript ekleme yeteneği tehdit nedeniyle güvenlik konusudur. Tarayıcılar bu özellik varsayılan olarak engelliyor. Bunu çözmek için tek sunucu (kaynak) tarafında tüm etki alanları (bakılmaksızın, HTTPS veya HTTP) vererek yoludur. Bu bir ya da fikirdir olmayabilir.
* Karışık içerik kaçının. Oynatıcı uygulaması ve Media Player, HTTP veya HTTPS kullanmalıdır. Karışık içerik yürütürken silverlightSS teknik bir karışık içerik uyarı temizlenmesini gerektirir. FlashSS teknik bir karışık içerik uyarı olmadan karışık içerik işler.
* Akış uç noktanızı Ağustos 2014 önce oluşturulduysa, HTTPS'yi destekleyecek olmaz. Bu durumda, oluşturun ve yeni bir akış uç noktası için HTTPS kullanın.

DRM korumalı içeriği, hem uygulama hem de HTTPS altında akış olan başvuru uygulamasında. HTTP veya HTTPS kullanılabilmesi için açık içerik, kimlik doğrulaması veya bir lisans player gerekmez.

### <a name="azure-active-directory-signing-key-rollover"></a>Anahtar geçişi imzalama Azure Active Directory
Anahtar geçişi imzalama, uygulamanızda dikkate almak için önemli bir noktadır. Yoksayarsanız, tamamlanmış sistem sonunda tamamen altı hafta içinde en çok çalışmayı durdurur.

Azure AD endüstri standartları kendisi ve Azure AD kullanan uygulamalar arasında güven sağlamak için kullanır. Özellikle, Azure AD genel ve özel bir anahtar çiftinden oluşur bir imzalama anahtarı kullanır. Azure AD kullanıcı hakkındaki bilgileri içeren bir güvenlik belirteci oluşturduğunda, uygulama geri gönderilmeden önce Azure AD tarafından özel bir anahtarla imzalanmıştır. Belirtecin geçerli ve Azure AD'den kaynaklı olduğunu doğrulamak için uygulama belirtecinin imzası doğrulamanız gerekir. Uygulama kiracının Federasyon meta veri belgesi içinde yer alan Azure AD tarafından sunulan ortak anahtarı kullanır. Bu ortak anahtar ve kendisinden, türetilen, imzalama anahtarı ile aynı Azure AD içinde tüm kiracılar için kullanılan olur.

Azure AD anahtar geçişi hakkında daha fazla bilgi için bkz: [anahtar geçişi Azure AD'de imzalama hakkında önemli bilgiler](../active-directory/active-directory-signing-key-rollover.md).

Arasında [genel-özel anahtar çifti](https://login.microsoftonline.com/common/discovery/keys/):

* Özel anahtar, bir JWT oluşturmak için Azure AD tarafından kullanılır.
* Ortak anahtar, JWT doğrulamak için DRM lisans teslimat hizmetlerini Media Services gibi bir uygulama tarafından kullanılır.

Güvenlik nedeniyle, Azure AD sertifika düzenli aralıklarla (Altı haftada) döndürür. Güvenlik ihlallerini söz konusu olduğunda, anahtar geçişi dilediğiniz zaman ortaya çıkabilir. Bu nedenle, Media Services lisans teslimat hizmetlerini Azure AD anahtar çifti döndüğü olarak kullanılan ortak anahtar güncelleştirmeniz gerekir. Aksi takdirde, Media Services belirteci kimlik doğrulaması başarısız olur ve hiçbir lisans verilir.

Bu hizmeti kurmak için DRM lisans teslimat hizmetlerini yapılandırdığınızda TokenRestrictionTemplate.OpenIdConnectDiscoveryDocument ayarlayın.

JWT akış şöyledir:

* Azure AD, kimliği doğrulanmış bir kullanıcı için geçerli özel anahtarı olan JWT verir.
* Bir oyuncu çoklu DRM korumalı içeriği ile CENC gördüğünde önce Azure AD tarafından verilen JWT bulur.
* Player Media Services lisans teslimat hizmetlerini JWT sahip bir lisans edinme isteğini gönderir.
* Media Services lisans teslimat hizmetlerini Azure AD'den geçerli/geçerli ortak anahtar lisansları vermeden önce JWT doğrulamak için kullanın.

DRM lisans teslimat hizmetlerini her zaman geçerli/geçerli ortak anahtar Azure AD'den denetleyin. Azure AD tarafından sunulan ortak anahtarı, Azure AD tarafından verilen JWT doğrulamak için kullanılan anahtardır.

Ne anahtar geçişi, bir JWT Azure AD oluşturduktan sonra ancak JWT oyuncu tarafından DRM lisans teslimat hizmetlerini doğrulama için Media Services gönderilmeden önce olur?

Birden fazla geçerli ortak anahtar, her zaman bir anahtar herhangi bir anda alınabilir için Federasyon meta veri belgesi kullanılamıyor. Media Services lisans teslimat belgede belirtilen anahtarları birini kullanabilirsiniz. Bir anahtar en kısa sürede geri alınması çünkü başka değişimi olması ve benzeri.

### <a name="where-is-the-access-token"></a>Erişim belirteci nerede?
Bir web uygulaması altında bir API uygulamasını nasıl çağıran en baktığınızda [uygulama kimliği OAuth 2.0 istemci kimlik bilgileri sağlama ile](../active-directory/develop/active-directory-authentication-scenarios.md#web-application-to-web-api), kimlik doğrulaması akışı aşağıdaki gibidir:

* Bir web uygulamasında Azure ad oturum açtığında. Daha fazla bilgi için bkz: [web uygulamasına Web tarayıcısı](../active-directory/develop/active-directory-authentication-scenarios.md#web-browser-to-web-application).
* Azure AD yetkilendirme son noktası kullanıcı aracısını geri bir yetkilendirme koduyla istemci uygulaması yönlendirir. Kullanıcı Aracısı, istemci uygulamanın yeniden yönlendirme URI'si yetkilendirme kodu döndürür.
* Web uygulaması, böylece web API kimlik doğrulaması ve istenen kaynak almak bir erişim belirteci alması gerekiyor. Azure AD belirteç uç noktası istekte bulunur ve kimlik bilgileri, istemci kimliği ve web API uygulama kimliği URI'sini sağlar. Kullanıcı rıza kanıtlamak için yetkilendirme kodu gösterir.
* Azure AD uygulama kimliğini doğrular ve web API'sini çağırmak için kullanılan bir JWT erişim belirteci döndürür.
* HTTPS üzerinden web uygulaması "Bearer" tayin JWT dizesiyle web API isteği "Yetkilendirme" üst bilgisi eklemek için döndürülen JWT erişim belirtecini kullanır. Web API sonra JWT doğrular. Doğrulama başarılı olursa, istenen kaynak döndürür.

Bu uygulama kimlik Akış web API'si web uygulaması için kullanıcı kimliklerinin güvenir. Bu nedenle, bu deseni güvenilir alt sistem adı verilir. [Yetkilendirme Akış Diyagramı](https://docs.microsoft.com/azure/active-directory/active-directory-protocols-oauth-code) nasıl grant kod yetkilendirme akışı açıklanır çalışır.

Lisans edinme belirteci kısıtlaması ile aynı güvenilir alt sistem deseni izler. Media Services lisans teslimat hizmetinin web API kaynak ya da "bir web uygulaması erişmesi arka uç kaynak" dir. Bu nedenle erişim belirteci nerede?

Erişim belirteci, Azure AD'den elde edilir. Kullanıcı başarıyla kimlik doğrulamasından sonra bir kimlik doğrulama kodu döndürülür. Yetkilendirme kodu sonra istemci kimliği ve uygulama anahtarı ile birlikte için erişim belirtecini gönderip almak için kullanılır. Erişim belirteci işaret veya Media Services lisans teslimat hizmeti temsil eden bir "işaretçi" uygulamaya erişmek için kullanılır.

Kaydolun ve Azure AD içinde işaretçi uygulamayı yapılandırmak için aşağıdaki adımları uygulayın:

1. Azure AD kiracısı'nda:

   * Oturum açma URL'si https://[resource_name].azurewebsites.net/ bir uygulamayla (kaynak) ekleyin. 
   * URL https://[aad_tenant_name].onmicrosoft.com/[resource_name uygulama Kimliğiyle Ekle].

2. Kaynak uygulama için yeni bir anahtar ekleyin.

3. Uygulama bildirim dosyasını güncelleştirin ve böylece groupMembershipClaims özelliği "groupMembershipClaims" değerine sahip: "Tümü".

4. Player web uygulamasında bölüm işaret Azure AD uygulama **diğer uygulamalara izinler**, 1. adımda eklenen kaynak uygulama ekleyin. Altında **izin temsilci**seçin **erişim [resource_name]**. Bu seçenek kaynak uygulama erişim erişim belirteçleri oluşturmak için web uygulama izni verir. Visual Studio ve Azure web uygulaması geliştirme hem yerel ve dağıtılan web uygulamasının sürümü için bunu.

Azure AD tarafından verilen JWT işaretçi kaynağa erişmek için kullanılan erişim belirteci ' dir.

### <a name="what-about-live-streaming"></a>Ne hakkında canlı akış?
Önceki tartışma isteğe bağlı varlıklar üzerinde odaklanmıştır. Ne hakkında canlı akış?

Canlı Media Services akış VOD varlık olarak bir programla ilişkili varlığı düşünerek korumak için tam olarak aynı tasarımını ve uygulamasını kullanabilirsiniz.

Özellikle, canlı akış medya Hizmetleri için bir kanal oluşturmak ve ardından bir programı kanal altında oluşturmak gerekir. Program oluşturmak için program için Canlı arşiv içeren bir varlığı oluşturmanız gerekir. Dinamik içerik çoklu DRM koruması ile CENC sağlamak için program başlamadan önce VOD varlık kabul edildiğinde varlık için aynı kurulum/işleme uygulama.

### <a name="what-about-license-servers-outside-media-services"></a>Media Services dışındaki lisans sunucularına nasıldır?
Genellikle, müşteriler kendi veri merkezi ya da DRM hizmet sağlayıcıları tarafından barındırılan bir lisans sunucu grubunda yatırım. Medya Hizmetleri içerik koruma ile karma modda çalışabilir. İçeriği barındırılan ve Media Services dışında sunucuları tarafından DRM lisansları teslim ederken Media Services, dinamik olarak korumalı. Bu durumda, aşağıdaki değişiklikleri göz önünde bulundurun:

* STS, kabul edilebilir ve lisans sunucusu grubu tarafından doğrulanan belirteçleri gerekiyor. Örneğin, bir yetkilendirme iletisini içeren belirli bir JWT Axinom tarafından sağlanan Widevine lisans sunucuları gerektirir. Bu nedenle, bu tür bir JWT'nin vermek için bir STS'ye olması gerekir. Bu tür bir uygulama hakkında daha fazla bilgi için Git [Azure Belge Merkezi'nde](https://azure.microsoft.com/documentation/) ve [kullanmak için Azure Media Services Widevine lisansları teslim için Axinom](media-services-axinom-integration.md).
* Artık Media Services lisans teslimat hizmetinin (ContentKeyAuthorizationPolicy) yapılandırmanız gerekir. Lisans edinme URL'leri (PlayReady, Widevine ve FairPlay) sağlamanız gereken çoklu DRM ile CENC ayarlamak için AssetDeliveryPolicy yapılandırdığınızda.

### <a name="what-if-i-want-to-use-a-custom-sts"></a>Ne özel bir STS kullan istiyor?
Bir müşteri Jwt'ler sağlamak için özel bir STS kullanmayı seçebilirsiniz. Nedenler şunlardır:

* Müşteri tarafından kullanılan IDP STS desteklemiyor. Bu durumda, özel bir STS bir seçenek olabilir.
* Müşteri faturalama sisteminize Müşteri'nin aboneye STS tümleştirmek için daha esnek ya da daha sıkı denetim gerekebilir. Örneğin, bir MVPD işleci birden çok premium, basic, gibi OTT abone paketler ve Spor sunabilir. İşleci, böylece yalnızca belirli bir paket içeriğini kullanılabilir hale getirilir talep belirteci bir abonenin paketi ile eşleşecek şekilde isteyebilirsiniz. Bu durumda, özel bir STS gerekli esneklik ve denetim sağlar.

Özel bir STS kullandığınızda, iki değişiklik yapılması gerekir:

* Bir varlık için lisans teslimat hizmeti yapılandırdığınızda, Azure AD'den geçerli anahtar yerine özel STS tarafından doğrulama için kullanılan güvenlik anahtarı belirtmeniz gerekir. (Daha fazla ayrıntı izleyin.) 
* JTW belirteç oluşturulduğunda geçerli X509 özel anahtarı yerine belirtilen bir güvenlik anahtar Azure AD'de sertifika.

Güvenlik anahtarları iki tür vardır:

* Simetrik anahtar: aynı anahtar oluşturmak için ve bir JWT doğrulamak için kullanılır.
* Asimetrik anahtar: bir genel-özel anahtar çifti bir X509 JWT şifrelemek/oluşturmak için bir özel anahtar ile belirteci doğrulamak için ortak anahtar sertifikası kullanılır.

> [!NOTE]
> .NET Framework'te kullanıyorsanız / C# geliştirme platformu olarak X509 asimetrik güvenlik anahtarı için kullanılan sertifika anahtar uzunluğu en az 2048 olması gerekir. Bu, .NET Framework'teki System.IdentityModel.Tokens.X509AsymmetricSecurityKey sınıfının bir gereksinimdir. Aksi takdirde, aşağıdaki özel durum oluşur:

> IDX10630: 'imzalama System.IdentityModel.Tokens.X509AsymmetricSecurityKey' '2048' bitten küçük olamaz.

## <a name="the-completed-system-and-test"></a>Tamamlanmış Sistem ve test
Bir oturum açma hesabı ulaşmadan, temel bir resim davranışının böylece bu bölümde, aşağıdaki senaryolarda tamamlanmış uçtan uca sistemde açıklanmaktadır:

* Tümleşik olmayan bir senaryo ihtiyacınız varsa:

    * Media Services ile olan barındırılan video varlıklar için ya da korumasız veya DRM ile korunan ancak (aktaranın istenen için bir lisans verme) belirteci kimlik doğrulaması olmadan, bu oturum açma olmadan test edebilirsiniz. Video akışı HTTP üzerinden ise HTTP anahtarı.

* Uçtan uca tümleşik senaryo ihtiyacınız varsa:

    * Belirteç kimlik doğrulaması ve Azure AD tarafından oluşturulan JWT ile Media Services, dinamik DRM koruma altında video varlıklar için oturum açmanız.

Player web uygulaması ve oturum açma için bkz: [bu Web sitesi](https://openidconnectweb.azurewebsites.net/).

### <a name="user-sign-in"></a>Kullanıcı oturumu açma
Uçtan uca tümleşik DRM sistemini test etmek için oluşturulan veya eklenen bir hesabınızın olması gerekir.

Hangi hesabı?

Azure başlangıçta yalnızca Microsoft hesabı kullanıcıları tarafından erişime izin verse de erişim artık her iki sistemle kullanıcıları tarafından verilir. Tüm Azure özelliklerinin artık kimlik doğrulaması için Azure AD güven ve Azure AD kuruluş kullanıcılarının kimliğini doğrular. Burada Azure AD tüketici kullanıcıların kimliğini doğrulamak için Microsoft hesabı tüketici kimliği sistemine güvendiği bir Federasyon ilişkisi oluşturuldu. Sonuç olarak, Azure AD Konuk Microsoft hesapları da yerel olarak Azure AD hesapların doğrulayabilir.

Azure AD Microsoft hesap etki alanı güvenleri olduğundan, aşağıdaki etki alanlarının tümünden herhangi hesapları özel Azure ad Kiracı ve hesabın oturum açmak için kullandığınız ekleyebilirsiniz:

| **Etki alanı adı** | **Etki alanı** |
| --- | --- |
| **Özel Azure AD Kiracı etki alanı** |somename.onmicrosoft.com |
| **Şirket etki alanı** |Microsoft.com |
| **Microsoft hesabı etki alanı** |Outlook.com, live.com, hotmail.com |

Oluşturulan veya sizin için eklenen hesabınız yazarların başvurabilirsiniz.

Aşağıdaki ekran görüntüleri farklı etki alanı hesapları tarafından kullanılan farklı oturum açma sayfalarını göster:

**Özel Azure AD Kiracı etki alanı hesabı**: özelleştirilmiş oturum açma sayfası özel Azure ad Kiracı etki alanı.

![Özel Azure AD Kiracı etki alanı hesabı](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain1.png)

**Microsoft etki alanı hesabı akıllı kart ile**: Microsoft tarafından şirket özelleştirilmiş oturum açma sayfası iki faktörlü kimlik doğrulaması ile BT.

![Özel Azure AD Kiracı etki alanı hesabı](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain2.png)

**Microsoft hesabı**: tüketicileri için Microsoft hesabı oturum açma sayfası.

![Özel Azure AD Kiracı etki alanı hesabı](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain3.png)

### <a name="use-encrypted-media-extensions-for-playready"></a>Şifrelenmiş medya uzantıları için PlayReady kullanma
Internet Explorer 11 Windows 8.1 veya üzeri ve Windows 10, Microsoft Edge tarayıcısının gibi PlayReady desteği için modern bir tarayıcı şifrelenmiş medya Uzantıları (EME) ile PlayReady EME için temel alınan DRM açıktır.

![PlayReady EME kullanılmak](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-playready1.png)

PlayReady koruma bir ekran yakalama korumalı videonun yapmasını engelleyeceğinden koyu player alanıdır.

Aşağıdaki ekran görüntüsü player eklentilerini ve Microsoft Güvenlik temelleri (MSE) gösterir / EME destek:

![Player için eklentiler PlayReady](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-playready2.png)

Microsoft Edge ve Internet Explorer 11 Windows 10 EME verir [PlayReady SL3000](https://www.microsoft.com/playready/features/EnhancedContentProtection.aspx/) destekleyen Windows 10 cihazlarda çağrılacak. PlayReady SL3000 kilidini açar (4K, HDR) geliştirilmiş premium içeriğinin ve yeni akış içerik teslim modellerinden (için geliştirilmiş içerik).

Windows cihazlarda odaklanmak için PlayReady (PlayReady SL3000) Windows cihazlarda kullanılabilir donanım yalnızca DRM ' dir. Bir akış hizmeti PlayReady EME veya bir evrensel Windows Platform uygulaması aracılığıyla kullanın ve başka bir DRM daha PlayReady SL3000 kullanarak, daha yüksek bir video kalitesini sağlar. Genellikle, Chrome veya Firefox aracılığıyla 2K akışları için yukarı içeriği ve içerik varan Microsoft Edge/Internet Explorer 11 veya aynı cihaza bir evrensel Windows platformu uygulamasının aracılığıyla 4K akar. Hizmet ayarları ve uygulama bağlıdır.

#### <a name="use-eme-for-widevine"></a>Widevine EME kullanılmak
Modern bir tarayıcı, Chrome 41 + Windows 10, Windows 8.1, Mac OSX Yosemite ve Chrome gibi EME/Widevine destek Android 4.4.4 ile Google Widevine DRM EME arkasında açıktır.

![Widevine EME kullanılmak](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-widevine1.png)

Widevine bir ekran yakalama korumalı videonun yapmasını engellemez.

![Player eklentileri Widevine](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-widevine2.png)

### <a name="unentitled-users"></a>Unentitled kullanıcılar
Bir kullanıcı "Başlıklı kullanıcıları" grubunun bir üyesi değilse, kullanıcının yetkilendirme onay geçmiyor. Çoklu DRM lisans hizmeti, ardından gösterildiği gibi istenen lisansı vermek reddediyor. Ayrıntılı açıklama "Lisans başarısız oldu," tasarlandığı gibi olduğu.

![Unentitled kullanıcılar](./media/media-services-cenc-with-multidrm-access-control/media-services-unentitledusers.png)

### <a name="run-a-custom-security-token-service"></a>Özel güvenlik belirteci hizmeti çalıştırın
Özel bir STS çalıştırırsanız, JWT bir simetrik ya da asimetrik anahtar kullanarak özel STS tarafından verilir.

Aşağıdaki ekran görüntüsü (Chrome kullanarak) bir simetrik anahtar kullanan bir senaryo gösterilmektedir:

![Simetrik anahtar ile özel STS](./media/media-services-cenc-with-multidrm-access-control/media-services-running-sts1.png)

Aşağıdaki ekran görüntüsünde bir X509 aracılığıyla bir asimetrik anahtar kullanan bir senaryo gösterilmektedir (Microsoft modern bir tarayıcı kullanarak) sertifika:

![Asimetrik anahtar ile özel STS](./media/media-services-cenc-with-multidrm-access-control/media-services-running-sts2.png)

Hem de önceki durumlarda, kullanıcı kimlik doğrulaması aynı kalır. Bu, Azure AD üzerinden gerçekleşir. Jwt'ler Azure AD yerine özel STS tarafından verilen yalnızca farktır. Dinamik CENC koruma yapılandırdığınızda, lisans teslimat hizmeti kısıtlama JWT, bir simetrik ya da asimetrik anahtar türünü belirtir.

## <a name="summary"></a>Özet
Bu belgede Azure, Media Services ve Media Player kullanarak belirteci kimlik doğrulaması, tasarımını ve kendi uygulama aracılığıyla birden çok yerel DRM ve erişim denetimi ile CENC açıklanmıştır.

* Bir başvuru tasarımı DRM/CENC alt sistemi tüm gerekli bileşenleri içeren sunulmadı.
* Bir başvuru uygulaması Azure, Media Services ve Media Player sunulmadı.
* Tasarım ve uygulama doğrudan ilgili bazı konular da ele alınan.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
 
