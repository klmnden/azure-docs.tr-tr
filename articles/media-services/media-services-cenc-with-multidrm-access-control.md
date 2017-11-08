---
title: "Çoklu DRM ve erişim denetimi ile CENC: bir başvuru tasarım ve uygulama Azure ve Azure Media Services | Microsoft Docs"
description: "Hakkında bilgi almak için Microsoft® kesintisiz akış istemci bağlantı noktası oluşturma Seti lisans."
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
ms.openlocfilehash: e4a53d053a4c792f54e215c19a8f0c4064815839
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2017
---
# <a name="cenc-with-multi-drm-and-access-control-a-reference-design-and-implementation-on-azure-and-azure-media-services"></a>Çoklu DRM ve Access Control ile CENC: Azure ve Azure Media Services Tasarım ve Uygulama Başvurusu
 
## <a name="introduction"></a>Giriş
İyi bilinen tasarımı ve DRM alt sistemi için bir OTT yapı karmaşık bir görev olduğunu veya çevrimiçi çözüm akış. Ve bu bölümü özelleştirilmiş DRM hizmet sağlayıcıları için dış kaynak sağlamak işleçleri/çevrimiçi video sağlayıcıları için ortak bir uygulama. Bu belgede bir başvuru tasarım ve uygulama uçtan uca DRM alt sisteminin OTT veya çevrimiçi akış çözüm sunmak için hedefidir.

Bu belgenin hedeflenen okuyucular DRM Subsystem OTT veya çevrimiçi akış/çok-screen çözümleri ya da tüm okuyucular DRM alt sisteminde ilgilenen çalışma mühendisleri ' dir. Okuyucular DRM teknolojileri PlayReady, Widevine, FairPlay veya Adobe erişim gibi piyasadaki en az biri alışık olduğunuz varsayılır.

DRM tarafından biz de çoklu DRM ile CENC (ortak şifreleme) içerir. Bir ana eğilimi çevrimiçi akış ve OTT endüstri CENC çok-native DRM çeşitli istemci platformlarında ile vardiya çoklu DRM ve kendi İstemci SDK çeşitli istemci platformları için kullanarak, önceki eğilim da olduğu kullanmaktır. CENC çok-native DRM ile kullanırken, PlayReady ve Widevine başına şifrelenmiş [ortak şifreleme (ISO/IEC 23001-7 CENC)](http://www.iso.org/iso/home/store/catalogue_ics/catalogue_detail_ics.htm?csnumber=65271/) belirtimi.

Çoklu DRM ile CENC avantajları aşağıdaki gibidir:

1. Şifreleme tek şifreleme işleme kendi yerel DRMs ile farklı platformları hedefleme kullanıldığından maliyeti azaltır;
2. Yalnızca tek bir kopyasını şifrelenmiş varlıklar gerekli olmadığından şifrelenmiş varlıklarını yönetme maliyetini azaltır;
3. DRM istemci yerel DRM istemcisi yerel platformda genellikle boş olduğundan maliyet lisansı ortadan kaldırır.

Microsoft, kısa çizgi ve CENC etkin bir promoter bazı önemli endüstri oynatıcıları birlikte olmuştur. Microsoft Azure Media Services, tire ve CENC desteği sağlama. Son Duyurular için lütfen Mingfei'nın Bloglara bakın: [tanışın Google Widevine lisans teslim hizmetleri Azure Media Services](https://azure.microsoft.com/blog/announcing-general-availability-of-google-widevine-license-services/), ve [Azure Media Services ekler çoklu DRM akış teslim etmek için Google Widevine paketlemeyi](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/).  

### <a name="overview-of-this-article"></a>Bu makalede genel bakış
Bu makalede amacı aşağıdakileri içerir:

1. Çoklu DRM ile CENC kullanarak DRM alt sisteminin başvuru tasarımı sağlar;
2. Bir başvuru uygulaması, Microsoft Azure/Azure Media Services platformda sağlar;
3. Bazı tasarım ve uygulama konuları anlatılmaktadır.

Makalesinde "çoklu DRM" aşağıdakileri içerir:

1. Microsoft PlayReady
2. Google Widevine
3. Apple FairPlay 

Aşağıdaki tabloda, yerel platform/yerel uygulama ve her DRM tarafından desteklenen Gözatıcıların özetler.

| **İstemci Platformu** | **Yerel DRM desteği** | **Tarayıcı/uygulaması** | **Akış biçimlerine** |
| --- | --- | --- | --- |
| **Akıllı TV, işleci STB, OTT STB** |PlayReady öncelikle, ve/veya Widevine ve/veya diğer |Linux, Opera, WebKit, diğer |Çeşitli biçimleri |
| **Windows 10 cihazları (Windows PC, Windows tabletler, Windows Phone, Xbox)** |PlayReady |MS kenar/IE11/EME<br/><br/><br/>UWP |TİRE (HLS için PlayReady desteklenmez)<br/><br/>DASH, kesintisiz akış (HLS için PlayReady desteklenmez) |
| **Android cihazlar (telefon, Tablet, TV)** |Widevine |Chrome/EME |DASH, HLS |
| **iOS (iPhone, iPad), OS X istemcileri ve Apple TV** |FairPlay |Safari 8 +/ EME |HLS |


Dağıtım için her DRM geçerli durumunu göz önünde bulundurularak, bir hizmet genellikle en iyi şekilde uç noktalar tüm türleri adres emin olmak için 2 veya 3 DRMs uygulamak isteyeceksiniz.

Hizmet mantığı karmaşıklığına ve belirli bir düzeyde kullanıcı deneyimi çeşitli istemcilere ulaşmak için istemci tarafında karmaşıklık arasında kolaylığını yoktur.

Seçim yapmak için bu bilgileri göz önünde bulundurun:

* PlayReady ve neredeyse her platformda yazılım SDK aracılığıyla kullanılabilen bazı Android cihazlarda, her Windows aygıt yerel olarak uygulanan
* Widevine her Android cihazı, Chrome ve diğer bazı cihazların yerel olarak uygulanır
* FairPlay yalnızca iOS ve Mac OS istemciler veya iTunes aracılığıyla kullanılabilir.

Bu nedenle, tipik bir çoklu DRM olacaktır:

* Seçenek 1: PlayReady ve Widevine
* Seçenek 2: PlayReady, Widevine ve FairPlay

## <a name="a-reference-design"></a>Bir başvuru tasarımı
Bu bölümde, biz uygulamak için kullanılan teknolojiler bağımsızdır bir başvuru tasarımı sunacaktır.

DRM alt sistemi, aşağıdaki bileşenleri içerebilir:

1. Anahtar Yönetimi
2. DRM paketleme
3. DRM lisansı verme
4. Yetkilendirme denetimi
5. Kimlik doğrulama/yetkilendirme
6. Player
7. Kaynak/CDN

Aşağıdaki diyagramda DRM alt sistemi bileşenleri arasındaki üst düzey etkileşimi gösterilmektedir.

![DRM alt sistemi ile CENC](./media/media-services-cenc-with-multidrm-access-control/media-services-generic-drm-subsystem-with-cenc.png)

Tasarımda üç temel "katmanları" vardır:

1. Harici olarak gösterilmeyen arka ofis Katmanı ('Siyah);
2. Tüm genel kullanıma yönelik uç noktalar içeren "DMZ" katman (mavi);
3. CDN ve trafik oyuncularla ortak Internet üzerinden içeren ortak Internet katman (mavi).

DRM koruma denetlemek için bir içerik yönetim aracı olmalıdır, ne olursa olsun statik veya dinamik şifreleme olduğundan. DRM şifreleme için girişleri içermelidir:

1. MBR video içeriği;
2. İçerik anahtarı;
3. Lisans edinme URL'leri.

Kayıttan yürütme süre boyunca, üst düzey akış sunar:

1. Kullanıcının kimliği doğrulanır;
2. Yetkilendirme belirtecini kullanıcıdan oluşturulur;
3. DRM korumalı içeriği (bildirim) aygıta yüklenir;
4. Player anahtarı kimliği ve yetkilendirme birlikte lisans sunucularına lisans edinme isteği gönderen belirteci.

Sonraki bölümüne, anahtar yönetimi tasarımı ile ilgili birkaç sözcük taşımadan önce.

| **ContentKey – varlık** | **Senaryo** |
| --- | --- |
| 1 – 1 |En basit durumda. En iyi denetim sağlar. Ancak bu genelde yüksek lisans teslimat maliyeti sonuçlanır. En az bir lisans isteği korunan her varlık için gereklidir. |
| 1 – çok |Aynı içerik anahtarı için birden çok varlığı kullanabilirsiniz. Örneğin, tüm varlıklar için bir mantıksal grubunda bir tarzını veya bir alt kümesini Tarz (veya film Gene) gibi tek bir içerik anahtarı kullanabilirsiniz. |
| Çok – 1 |Birden çok içerik anahtarı her varlık için gereklidir. <br/><br/>Örneğin, MPEG-DASH için çoklu DRM ile dinamik CENC koruma ve HLS için dinamik AES-128 şifrelemesini uygulamanız gerekiyorsa, iki farklı içerik anahtarları, her biri kendi ContentKeyType gerekir. (Dinamik CENC koruma için kullanılan içerik anahtarı için ContentKeyType.CommonEncryption olmalıdır, dinamik AES-128 şifreleme için kullanılan içerik anahtarı sırasında kullanılan, ContentKeyType.EnvelopeEncryption kullanılmalıdır.)<br/><br/>Başka bir örnekte, tire içeriğinin, içerik anahtarı video akışına ve ses akışı korumak için başka bir içerik anahtarı korumak için kullanılan teorik CENC koruma. |
| Çok –-- çok |Yukarıdaki iki senaryo birleşimi: içerik anahtarı bir dizi her biri birden çok varlıkları aynı varlık "grubu" için kullanılır. |

Dikkate alınması gereken başka bir önemli kalıcı ve kalıcı olmayan lisans kullanımını faktördür.

Bu noktalar neden önemlidir?

Lisans teslimat için genel bulut kullanırsanız, lisans teslimat maliyet doğrudan etkisi sahiptirler. Şimdi aşağıdaki iki farklı tasarım durumlarda göstermek için göz önünde bulundurun:

1. Aylık abonelik: kalıcı lisans ve 1-çok içerik anahtarı varlık eşleme kullanın. Örneğin Tüm çocuklar filmler için tek bir içerik anahtarı şifreleme için kullanın. Bu durumda:

    Toplam tüm çocuklar filmler/aygıt için istenen # lisans = 1
2. Aylık abonelik: kalıcı olmayan lisans ve içerik anahtarı ve varlık arasında 1-1 eşleme kullanın. Bu durumda:

    Toplam tüm çocuklar filmler/aygıt için istenen # lisans [# filmler izlenen] = [# oturumları] x

Kolayca görebileceğiniz gibi iki farklı tasarımları bu nedenle çok farklı lisans isteği düzenleri lisans teslimat hizmeti Azure Media Services gibi genel bulut tarafından sağlanıyorsa maliyet lisans teslimat neden.

## <a name="mapping-design-to-technology-for-implementation"></a>Uygulama için teknoloji tasarım eşleme
Ardından, biz bizim genel tasarım Microsoft Azure/Azure Media Services platformunda teknolojileri için her Yapı bloğu kullanmak için hangi teknoloji belirterek eşleyin.

Aşağıdaki tablo eşleme gösterir:

| **Yapı Taşı** | **Teknoloji** |
| --- | --- |
| **Player** |[Azure Media Player](https://azure.microsoft.com/services/media-services/media-player/) |
| **Kimlik sağlayıcıyı (IDP)** |Azure Active Directory |
| **Güvenli belirteç hizmeti (STS)** |Azure Active Directory |
| **DRM koruma iş akışı** |Dinamik koruma Azure medya Hizmetleri |
| **DRM lisans teslimat** |1. Azure Media Services lisans teslimat (PlayReady, Widevine, FairPlay) veya <br/>2. Axinom lisans sunucusu <br/>3. Özel PlayReady lisans sunucusu |
| **Kaynak** |Akış uç Azure medya Hizmetleri |
| **Anahtar Yönetimi** |Başvuru uygulaması için gerekli değil |
| **İçerik Yönetimi** |Bir C# konsol uygulaması |

Diğer bir deyişle, kimlik sağlayıcısı (IDP) ve güvenli belirteç hizmeti (STS) Azure AD olacaktır. Player için kullanacağız [Azure Media Player API](http://amp.azure.net/libs/amp/latest/docs/). Azure Media Services ve Azure Media Player çoklu DRM ile DASH ve CENC destekler.

Aşağıdaki diyagramda, genel yapısı ve yukarıdaki teknolojisi eşleme ile akışını göstermektedir.

![AMS üzerinde CENC](./media/media-services-cenc-with-multidrm-access-control/media-services-cenc-subsystem-on-AMS-platform.png)

Dinamik CENC şifrelemesi ayarlamak için aşağıdaki girişleri içerik yönetimi aracını kullanır:

1. Açık içerik;
2. Anahtarından içerik anahtarı oluşturma/Yönetim;
3. Lisans edinme URL'leri;
4. Azure AD'den bilgi listesi.

İçerik Yönetimi aracın çıktısının olacaktır:

1. JWT belirteci ve DRM lisans belirtimleri lisans teslimat nasıl doğrular üzerinde belirtimi içeren ContentKeyAuthorizationPolicy;
2. Biçim, DRM koruma ve lisans edinme URL'leri akış üzerinde belirtimleri içeren AssetDeliveryPolicy.

Çalışma zamanı sırasında olarak akışıdır altında:

1. Kullanıcı kimlik doğrulaması sırasında bir JWT belirteci oluşturulur;
2. JWT belirtecinde yer alan talepler "grupları" talep "EntitledUserGroup" grubu nesne Kimliğini içeren biridir. Bu talep "yetkilendirme onay" geçirmek için kullanılır.
3. Bir CENC Player yüklemeleri istemci bildirimi, korumalı içeriği ve "aşağıdaki görür":

   1. anahtar kimliği,
   2. içeriğin CENC korumalı olduğundan,
   3. Lisans edinme URL'leri.
4. Player desteklenen tarayıcı/DRM üzerinde temel lisans edinme isteğinde bulunur. Lisans edinme isteği kimliği anahtarı ve JWT belirteci da gönderilir. Lisans teslimat hizmeti JWT belirteci ve gerekli lisans vermeden önce yer alan talepler doğrular.

## <a name="implementation"></a>Uygulama
### <a name="implementation-procedures"></a>Uygulama yordamları
Uygulaması aşağıdaki adımları içerir:

1. Test kıymetlerin hazırlayın: kodlamak/paketi bir test video Çoklu bit hızlı parçalanmış MP4 Azure Media Services. Bu varlık DRM korumalı değil. DRM korumasını daha sonra dinamik Koruması tarafından yapılır.
2. Anahtar kimliği ve içerik oluşturma (isteğe bağlı olarak gelen anahtar seed) anahtarı. Biz yalnızca tek bir dizi ile çalışıyorsanız olmadığından bu konudaki Hedefimiz için anahtar yönetimi sistemi gerekli değildir anahtar kimliği ve içerik anahtarı birkaç test varlıklar;
3. AMS API test varlık için çoklu DRM lisans teslimat hizmetlerini yapılandırmak için kullanın. Şirketiniz veya Azure Media Services lisans Hizmetleri'nde yerine şirketinizin satıcıları tarafından özel lisans sunucuları kullanıyorsanız, bu adımı atlayın ve lisans teslimat yapılandırma adımda lisans edinme URL'leri belirtin. AMS API bazı yapılandırmalar, yetkilendirme ilkesi kısıtlama, lisans yanıt şablonları farklı DRM lisans Hizmetleri, vb. gibi ayrıntılı belirtmek için gereklidir. Şu anda Azure portalında henüz gerekli UI bu yapılandırma için sağlamaz. API düzeyi bilgileri bulmak ve örnek koduna Jale Kornich'ın belgedeki: [kullanarak PlayReady ve/veya Widevine dinamik ortak şifreleme](media-services-protect-with-drm.md).
4. AMS API test varlık için varlık teslim ilkesini yapılandırmak için kullanın. API düzeyi bilgileri bulmak ve örnek koduna Jale Kornich'ın belgedeki: [kullanarak PlayReady ve/veya Widevine dinamik ortak şifreleme](media-services-protect-with-drm.md).
5. Oluşturma ve Azure Active Directory kiracısı Azure'da yapılandırın;
6. Birkaç kullanıcı hesapları ve grupları, Azure Active Directory kiracınızda oluşturun: en az oluşturmanız gerekir "EntitledUser" Grup ve bu gruba bir kullanıcı ekleyebilirsiniz. Bu gruptaki kullanıcılar lisans edinme yetkilendirme denetimi başarılı ve bu gruptaki kullanıcılar kimlik doğrulama denetimi geçirmek başarısız olur ve herhangi bir lisans edinmeye mümkün olmaz. Bu "EntitledUser" grubunun bir üyesi olması gereken "grupları" talep Azure AD tarafından verilen JWT belirteci olur. Çoklu DRM lisans teslimat hizmetlerini adım yapılandırmada bu talep gereksinim belirtilmelidir.
7. Video oynatıcı barındıran bir ASP.NET MVC uygulaması oluşturursunuz. Bu ASP.NET uygulamasını kullanıcı kimlik doğrulamasıyla Azure Active Directory Kiracı karşı korunur. Uygun talep kullanıcı kimlik doğrulamasından sonra elde erişim belirteçleri eklenecektir. Openıd Connect API, bu adım için önerilir. Aşağıdaki NuGet paketleri yüklemeniz gerekir:
   * Install-Package Microsoft.Azure.ActiveDirectory.GraphClient
   * Install-Package Microsoft.Owin.Security.OpenIdConnect
   * Install-Package Microsoft.Owin.Security.Cookies
   * Install-Package Microsoft.Owin.Host.SystemWeb
   * Install-Package Microsoft.IdentityModel.Clients.activedirectory tarafından
8. Kullanarak bir oynatıcı oluşturmak [Azure Media Player API](http://amp.azure.net/libs/amp/latest/docs/). [Azure Media Player'ın ProtectionInfo API](http://amp.azure.net/libs/amp/latest/docs/) farklı DRM platformda kullanmak için hangi DRM teknolojisi belirtmenize olanak tanır.
9. Testi matris:

| **DRM** | **Tarayıcı** | **Yetkili kullanıcı için sonuç** | **Sonuç beklemediğiniz yetkili bir kullanıcı için** |
| --- | --- | --- | --- |
| **PlayReady** |MS kenar veya Windows 10 IE11 |Başarılı |Başarısız |
| **Widevine** |Windows 10 Chrome |Başarılı |Başarısız |
| **FairPlay** |TBD | | |

Azure Media Services ekibine George Trifonov Azure Active Directory için bir ASP.NET MVC oynatıcı uygulaması ayarlama hakkında ayrıntılı adımlar sağlayan bir blog yazma: [tümleştirmek Azure Media Services OWIN MVC uygulama Azure Active Directory ile temel ve JWT talepleri temelinde içerik anahtar teslim kısıtlamak](http://gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

George da yazılmış bir blog üzerinde [JWT belirteci kimlik doğrulaması Azure Media Services ve dinamik şifreleme](http://gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/).  

Azure Active Directory hakkında daha fazla bilgi için:

* Geliştirici bilgiler bulabilirsiniz [Azure Active Directory Geliştirici Kılavuzu](../active-directory/active-directory-developers-guide.md).
* Yönetici bilgileri bulabilirsiniz [yönetmek bilgisayarınızı Azure AD dizini](../active-directory/active-directory-administer.md).

### <a name="some-gotchas-in-implementation"></a>Uygulamasındaki bazı FRİKİKLERİNDEN
Uygulamasında bazı "FRİKİKLERİNDEN" vardır. Neyse "FRİKİKLERİNDEN" aşağıdaki listesini sorunla çalışması durumunda sorun giderme yardımcı olabilir.

1. **Veren** URL ile bitmelidir **"/"**.  

    **Hedef kitle** player uygulama istemci kimliği olmalı ve ayrıca eklemelisiniz **"/"** veren URL sonunda.

        <add key="ida:audience" value="[Application Client ID GUID]" />
        <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/" />

    İçinde [JWT Decoder](http://jwt.calebb.net/), görmeniz gerekir **aud** ve **ISS** JWT belirteci olarak aşağıda içinde:

    ![1. sorunu](./media/media-services-cenc-with-multidrm-access-control/media-services-1st-gotcha.png)
2. Aad'de uygulama (uygulamasının yapılandırma sekmesinde) için izinleri ekleyin. Bu, her uygulama için (yerel ve dağıtılan sürümler) gereklidir.

    ![2 sorunu](./media/media-services-cenc-with-multidrm-access-control/media-services-perms-to-other-apps.png)
3. Dinamik CENC koruma kurulumuna içinde doğru veren kullanın:

        <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/"/>

    Aşağıdakiler çalışmaz:

        <add key="ida:issuer" value="https://willzhanad.onmicrosoft.com/" />

    GUID AAD Kiracı kimliğidir. GUID uç noktaları açılan penceresinde Azure Portalı'nda bulunabilir.
4. GRANT grup üyeliği ayrıcalıkları talepleri. AAD uygulama bildirim dosyasında emin olun, şunları sunuyoruz

    "groupMembershipClaims": "Tümü", (varsayılan değeri null)
5. Uygun TokenType kısıtlama gereksinimleri oluştururken ayarlama.

        objTokenRestrictionTemplate.TokenType = TokenType.JWT;

    Destek JWT (AAD) yanı sıra SWT (ACS) eklendiğinden beri TokenType TokenType.JWT varsayılandır. SWT/ACS kullanırsanız TokenType.SWT için ayarlamanız gerekir.

## <a name="additional-topics-for-implementation"></a>Uygulama için ek konular
Sonraki biz uss bazı ek konular tasarım ve uygulama disk.

### <a name="http-or-https"></a>HTTP veya HTTPS?
Oluşturduğumuz ASP.NET MVC oynatıcı uygulaması aşağıdaki desteklemesi gerekir:

1. Kullanıcı kimlik doğrulaması, HTTPS altında alınması gereken Azure AD ile;
2. İstemci, HTTPS altında alınması gereken Azure AD arasında JWT belirteci exchange;
3. Lisans teslimat Azure Media Services tarafından sağlanıyorsa HTTPS altında olması için gerekli olan istemci tarafından DRM lisans edinme. Elbette, PlayReady ürün paketini HTTPS lisans teslimat için zorunlu kılabilir değil. PlayReady lisans sunucunuz Azure Media Services dışında ise, HTTP veya HTTPS kullanılabilir.

Bu nedenle, ASP.NET oynatıcı uygulaması en iyi uygulama olarak HTTPS kullanır. Bu, Azure Media Player sayfasında HTTPS altında anlamına gelir. Ancak, akış için biz HTTP tercih ederseniz, bu nedenle biz karışık içerik sorunu dikkate almanız gerekir.

1. Tarayıcı karışık içeriğe izin vermiyor. Ancak eklentileri kesintisiz için Silverlight ve OSMF eklentisi ve çizgi gibi izin verin. Karışık içerik güvenlik sorunu - müşteri verilerin risk neden kötü amaçlı JS ekleme yeteneği tehdit nedeni budur.  Tarayıcılar bu varsayılan olarak engelliyor ve o ana kadarki bunu çözmek için yalnızca tüm etki alanları (bakılmaksızın https veya http) izin vermek için sunucu (kaynak) tarafında yoludur. Bu bir ya da fikirdir olmayabilir.
2. Karışık içerik kaçınmanız gerekir: her ikisi de HTTP kullanabilir veya her ikisi de HTTPS kullanır. Karışık içerik yürütürken silverlightSS teknik bir karışık içerik uyarı temizlenmesini gerektirir. flashSS teknik karışık içerik uyarmadan karışık içerik işler.
3. Akış uç noktanızı Ağustos 2014 önce oluşturulduysa, HTTPS desteklemez. Bu durumda, lütfen oluşturun ve yeni bir akış uç noktası için HTTPS kullanın.

Başvuru uygulamasında DRM korumalı içeriği için uygulama ve akış HTTTPS altında olacaktır. HTTP veya HTTPS kullanmak üzere özgürlük alacak şekilde açık içeriği için player kimlik doğrulaması veya lisans gerekli değildir.

### <a name="azure-active-directory-signing-key-rollover"></a>Anahtar geçişi imzalama Azure Active Directory
Bu, uygulamanızın dikkate önemli bir noktadır. Uygulamanızda bu katmazsanız tamamlanmış sistem sonunda tamamen en çok 6 hafta içinde çalışmayı durdurur.

Azure AD, kendisi ve Azure AD kullanarak uygulamalar arasında güven sağlamak için endüstri standardını kullanır. Özellikle, Azure AD genel ve özel bir anahtar çiftinden oluşur bir imzalama anahtarı kullanır. Azure AD kullanıcı hakkındaki bilgileri içeren bir güvenlik belirteci oluşturduğunda, bu belirteç uygulama geri gönderilmeden önce özel anahtarını kullanarak Azure AD tarafından imzalanır. Belirtecin geçerli ve Azure AD'den gerçekte kaynaklı olduğunu doğrulamak için uygulama kiracının Federasyon meta veri belgesi içinde yer alan Azure AD tarafından sunulan ortak anahtarı kullanılarak belirtecinin imzası doğrulamanız gerekir. Bu ortak anahtarı – ve kendisinden türetilen imzalama anahtarı – Azure AD'de tüm kiracılar için kullanılanla aynıdır.

Azure AD anahtar geçişi hakkında ayrıntılı bilgiler belgesinde bulunabilir: [imzalama anahtarı Rollover Azure AD'de hakkında önemli bilgiler](../active-directory/active-directory-signing-key-rollover.md).

Arasında [genel-özel anahtar çifti](https://login.microsoftonline.com/common/discovery/keys/),

* Özel anahtarı tarafından Azure Active Directory JWT belirteci üretmek için kullanılır;
* Ortak anahtar DRM lisans teslimat hizmetlerini AMS içinde gibi bir uygulama tarafından JWT belirteci doğrulamak için kullanılır;

Güvenlik amaç için Azure Active Directory bu sertifikayı düzenli aralıklarla (6 haftada) döndürür. Güvenlik ihlallerini durumunda, anahtar geçişi dilediğiniz zaman ortaya çıkabilir. Bu nedenle, AMS lisans teslim hizmetleri Azure AD anahtar çifti döndürür, aksi takdirde AMS belirteci kimlik doğrulaması başarısız olur ve hiçbir lisans verilecek olarak kullanılan ortak anahtar güncelleştirmeniz gerekir.

Bu, DRM lisans teslimat hizmetlerini yapılandırırken TokenRestrictionTemplate.OpenIdConnectDiscoveryDocument ayarlayarak sağlanır.

JWT belirteci akışı gibidir altında:

1. Azure AD kimliği doğrulanmış bir kullanıcı için geçerli özel anahtarı olan JWT belirteci vermek;
2. Bir oyuncu çoklu DRM korumalı içeriği ile CENC gördüğünde, ilk Azure AD tarafından verilen JWT belirteci bulur.
3. Player AMS lisans teslim hizmetleri JWT belirteci ile lisans edinme isteği gönderir;
4. AMS lisans teslim hizmetleri, Azure AD'den geçerli/geçerli ortak anahtar lisansları vermeden önce JWT belirteci doğrulamak için kullanır.

DRM lisans teslimat hizmetlerini Azure AD'den geçerli/geçerli ortak anahtar için her zaman denetleniyor. Azure AD tarafından sunulan ortak anahtar Azure AD tarafından verilen JWT belirteci doğrulamak için kullanılan anahtar olacaktır.

Ne anahtar geçişi sonra AAD JWT belirteci oluşturur ama önce JWT belirteci oyuncu tarafından Itanium tabanlı sistemler için DRM lisans teslimat hizmetlerini AMS içinde doğrulama için gönderilen olur?

Herhangi bir anda bir anahtar alınması çünkü her zaman birden fazla geçerli ortak anahtar Federasyon meta veri belgesi mevcut değil. Azure Media Services lisans teslimat belgede belirtilen anahtarlara kullanabileceğiniz bir anahtar en kısa sürede geri alınması olduğundan, başka bir değişimi olması ve benzeri.

### <a name="where-is-the-access-token"></a>Erişim belirteci olarak nerede?
Bir web uygulaması altında bir API uygulamasını nasıl çağıran en baktığınızda [uygulama kimliği OAuth 2.0 istemci kimlik bilgilerini verme ile](../active-directory/develop/active-directory-authentication-scenarios.md#web-application-to-web-api), kimlik doğrulaması akışı gibidir altında:

1. Bir kullanıcı Azure ad ile web uygulamasında oturum (bkz [Web uygulamasına Web tarayıcısı](../active-directory/develop/active-directory-authentication-scenarios.md#web-browser-to-web-application).
2. Azure AD yetkilendirme son noktası kullanıcı aracısını geri bir yetkilendirme koduyla istemci uygulaması yönlendirir. Kullanıcı Aracısı, istemci uygulamanın yeniden yönlendirme URI'si yetkilendirme kodu döndürür.
3. Web uygulaması, böylece web API kimlik doğrulaması ve istenen kaynak almak bir erişim belirteci alması gerekiyor. Kimlik bilgisi, istemci kimliği ve web API uygulama kimliği URI'sini sağlayan Azure AD belirteç uç noktası için bir istek kolaylaştırır. Kullanıcının seçtiği kanıtlamak için yetkilendirme kodu gösterir.
4. Azure AD uygulama kimliğini doğrular ve web API'sini çağırmak için kullanılan bir JWT erişim belirteci döndürür.
5. HTTPS üzerinden web uygulaması "Bearer" tayin JWT dizesiyle web API isteğinin yetkilendirme üst bilgi eklemek için döndürülen JWT erişim belirtecini kullanır. Web API JWT belirteci doğrular ve doğrulama başarılı olursa, istenen kaynak döndürür.

Bu "uygulama kimliği" Akış web API'si web uygulaması için kullanıcı kimliklerinin güvenir. Bu nedenle, bu deseni güvenilir alt sistem adı verilir. [Diyagram, bu sayfada](https://docs.microsoft.com/azure/active-directory/active-directory-protocols-oauth-code) nasıl yetki kodu izin akışı works açıklar.

Lisans edinme belirteç kısıtlamasına'de, biz aynı güvenilir alt sistem deseni takip. Ve Azure Media Services lisans teslimat hizmeti API kaynağını, bir web uygulaması erişmesi "kaynak" arka uç web. Bu nedenle erişim belirteci nerede?

Aslında, biz erişim belirteci Azure AD'den alma. Kullanıcı başarıyla kimlik doğrulamasından sonra yetkilendirme kodu döndürülür. Yetkilendirme kodu sonra istemci kimliği ve uygulama anahtarı ile birlikte için erişim belirtecini gönderip almak için kullanılır. Ve erişim belirteci "işaretçi" gösteren uygulama erişme veya Azure Media Services lisans teslimat hizmeti temsil eden için.

Kaydedin ve aşağıdaki adımları izleyerek Azure AD içinde "işaretçi" uygulama yapılandırmak ihtiyacımız var:

1. Azure AD kiracısında

   * oturum açma URL'si ile bir uygulama (kaynak) ekleyin:

   https://[resource_name].azurewebsites.NET/ ve

   * Uygulama Kimliği URL'si:

   https://[aad_tenant_name].onmicrosoft.com/[resource_name];
2. Kaynak uygulama için yeni bir anahtar ekleyin;
3. Uygulama bildirim dosyasını güncelleştirin, böylece groupMembershipClaims özelliği şu değere sahip: "groupMembershipClaims": "Tümü",  
4. Azure AD uygulama player web uygulaması'na işaret eden, bölümündeki "diğer uygulamalara izinler", yukarıdaki 1. adımda eklenen kaynak uygulama ekleyin. "İzinleri atanmış altında", "Erişim [resource_name]" onay işaretine denetleyin. Bu kaynak uygulama erişmek için erişim belirteçleri oluşturmak için web uygulama izni verir. Visual Studio ve Azure web uygulaması ile geliştiriyorsanız, bu web uygulamasını hem yerel hem de dağıtılan sürümü için yapmanız gerekir.

Bu nedenle Azure AD tarafından verilen JWT belirteci gerçekten bu "işaretçi" kaynağa erişmek için erişim belirteci olur.

### <a name="what-about-live-streaming"></a>Canlı ne akış?
Yukarıdaki içinde bizim tartışma isteğe bağlı varlıklar üzerinde odaklanan. Ne hakkında canlı akış?

İyi haber, tam olarak aynı tasarım ve uygulama Canlı Azure Media Services'de akış "VOD varlık" olarak bir programla ilişkili varlığı düşünerek korumak için kullanabileceğiniz ' dir.

Özellikle, iyi bilinen yapmak için canlı akış Azure Media Services ile bir kanal sonra kanalı altında bir program oluşturmak ihtiyacınız olduğunu. Program oluşturmak için program için Canlı arşiv yer alacağı bir varlığı oluşturmanız gerekir. CENC ile canlı içerik çoklu DRM koruma sağlamak için tüm yapmanız gereken, aynı kurulum / varlık için program başlamadan önce bunu "VOD varlık" boşmuş gibi işleme uygulamaktır.

### <a name="what-about-license-servers-outside-of-azure-media-services"></a>Lisans sunucuları Azure Media Services dışında nasıldır?
Genellikle, müşteriler kendi veri merkezi ya da barındırılan lisans sunucu grubundaki DRM hizmet sağlayıcıları tarafından yatırım yapmış kullanıcılar. Neyse ki, Azure medya Hizmetleri içerik koruma, karma modda çalışmaya sağlar: içeriği barındırılan ve Azure Media Services dışında sunucuları tarafından DRM lisansları teslim ederken dinamik olarak Azure Media Services ile korumalı. Bu durumda, değişiklikleri aşağıdaki hususlara vardır:

1. Güvenli belirteç hizmeti, kabul edilebilir olan ve lisans sunucusu grubu tarafından doğrulanan belirteçleri gerekiyor. Örneğin, Axinom tarafından sağlanan Widevine lisans sunucuları "yetkilendirme ileti" içeren belirli bir JWT belirteci gerektirir. Bu nedenle, bu tür JWT belirteci vermek için bir STS'ye olması gerekir. Yazarları gibi bir uygulama tamamlamış ve Ayrıntılar aşağıdaki belgede bulabileceğiniz [Azure Belge Merkezi'nde](https://azure.microsoft.com/documentation/): [kullanarak Azure Media Services Widevine lisansları teslim için Axinom](media-services-axinom-integration.md).
2. Artık Azure Media Services lisans teslimat hizmetinin (ContentKeyAuthorizationPolicy) yapılandırmanız gerekir. Lisans edinme URL'leri (PlayReady, Widevine ve FairPlay) sağlamak için yapmanız gerekenler olan çoklu DRM ile CENC ayarı AssetDeliveryPolicy yapılandırırken.

### <a name="what-if-i-want-to-use-a-custom-sts"></a>Ne özel bir STS kullan istiyor?
Bir müşteri JWT belirteçleri sağlamak için özel bir STS (güvenli belirteç hizmeti) kullanmayı tercih edebilirsiniz birkaç nedeni olabilir. Bunlardan bazıları şunlardır:

1. Müşteri tarafından kullanılan kimlik sağlayıcısı (IDP) STS desteklemez. Bu durumda özel bir STS bir seçenek olabilir.
2. Müşteri, daha esnek ya da daha sıkı STS faturalama sisteminize müşterinin aboneye tümleştirme kontrol gerekebilir. Örneğin, bir MVPD işleci birden çok OTT abone paketleri premium, temel gibi spor, vb. sunabilir. İşleci, yalnızca doğru paket içeriğini yapıldığında böylece kullanılabilir talep belirteci bir abonenin paketi ile eşleşecek şekilde isteyebilirsiniz. Bu durumda, özel bir STS gerekli esneklik ve denetim sağlar.

İki değişiklik özel bir STS'ye kullanılırken yapılması gerekir:

1. Bir varlık için lisans teslimat hizmeti yapılandırırken, doğrulama için Azure Active Directory'den geçerli anahtar yerine özel STS (daha fazla ayrıntı aşağıdadır) tarafından kullanılan güvenlik anahtarı belirtmeniz gerekir.
2. JTW belirteç oluşturulduğunda geçerli X509 özel anahtarı yerine belirtilen bir güvenlik anahtar Azure Active Directory'de sertifika.

Güvenlik anahtarları iki tür vardır:

1. Simetrik anahtar: aynı anahtar oluşturma hem bir JWT belirteci; doğrulamak için kullanılır
2. Asimetrik anahtar: sertifika şifreleme/JWT belirteci ve belirteci doğrulamak için ortak anahtar oluşturmak için özel anahtar olarak kullanılan bir X509 bir genel-özel anahtar çifti.

#### <a name="tech-note"></a>Teknik Not
.NET Framework'te kullanıyorsanız / C# geliştirme platformu olarak X509 asimetrik güvenlik anahtarı için kullanılan sertifika anahtar uzunluğu en az 2048 olması gerekir. Bu, .NET Framework'teki System.IdentityModel.Tokens.X509AsymmetricSecurityKey sınıfının bir gereksinimdir. Aksi takdirde, şu özel durum oluşturulur:

IDX10630: 'imzalama System.IdentityModel.Tokens.X509AsymmetricSecurityKey' '2048' bitten küçük olamaz.

## <a name="the-completed-system-and-test"></a>Tamamlanmış Sistem ve test
Okuyucular davranışı "resmi" temel bir oturum açma hesabı almadan önce böylece birkaç senaryolar üzerinden tamamlanmış uçtan uca sistemde anlatılmaktadır.

Player web uygulama ve onun oturum açma bulunabilir [burada](https://openidconnectweb.azurewebsites.net/).

Gerekenler "tümleşik olmayan" senaryo ise: ya da korumasız veya DRM ile korunan Azure Media Services, ancak belirteç kimlik doğrulaması olmadan barındırılan video varlıklar (aktaranın için bir lisans verme istediği), bu oturum açma bilgileri olmadan (video akışı HTTP üzerinden ise HTTP geçerek) test edebilirsiniz.

Gerekenler uçtan uca tümleşik senaryo ise: video varlıklar belirteci kimlik doğrulaması ve Azure AD tarafından oluşturulan JWT belirteci ile Azure Media Services, dinamik DRM koruma altında oturum açmak gerekir.

### <a name="user-login"></a>Kullanıcı oturum açma
Uçtan uca tümleşik DRM sistemini test etmek için "oluşturulan veya eklenen bir hesabınız" olması gerekir.

Hangi hesabı?

Artık Azure başlangıçta yalnızca Microsoft hesabı kullanıcıları tarafından erişime izin verse de her iki sistemle kullanıcıları tarafından erişime izin verir. Bu, tüm Azure özelliklerinin kimlik doğrulaması için Azure AD'ye güvenmesi sağlanarak, Azure AD'nin kuruluş kullanıcılarının kimliğini doğrulaması sağlanarak ve Azure AD'nin tüketici kullanıcıların kimliğini doğrulamak için Microsoft hesabı tüketici kimliği sistemine güvendiği bir federasyon ilişkisi oluşturularak gerçekleştirilmiştir. Sonuç olarak Azure AD, "yerel" Azure AD hesaplarının yanı sıra, "konuk" Microsoft hesapları için kimlik doğrulaması yapabilmektedir.

Azure AD Microsoft hesabı (MSA) etki alanı güvenleri olduğundan, aşağıdaki etki alanlarının tümünden herhangi hesapları özel Azure ad Kiracı ve oturum açma hesabı kullanmak ekleyebilirsiniz:

| **Etki alanı adı** | **Etki alanı** |
| --- | --- |
| **Özel Azure AD Kiracı etki alanı** |somename.onmicrosoft.com |
| **Şirket etki alanı** |Microsoft.com |
| **Microsoft hesabı (MSA) etki alanı** |Outlook.com, live.com, hotmail.com |

Oluşturulan veya sizin için eklenen hesabınız yazarların başvurabilirsiniz.

Aşağıda, farklı bir etki alanı hesapları tarafından kullanılan farklı bir oturum açma sayfalarını ekran görüntüleri verilmiştir.

**Özel Azure AD Kiracı etki alanı hesabı**: özel özelleştirilmiş oturum açma sayfasının bu durumda, gördüğünüz Azure AD Kiracı etki alanı.

![Özel Azure AD Kiracı etki alanı hesabı](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain1.png)

**Microsoft etki alanı hesabı akıllı kart ile**: Microsoft tarafından şirket özelleştirilmiş oturum açma sayfasına bakın bu durumda BT iki faktörlü kimlik doğrulamasıyla.

![Özel Azure AD Kiracı etki alanı hesabı](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain2.png)

**Microsoft hesabı (MSA)**: Bu durumda, Tüketiciler için Microsoft Account oturum açma sayfasına bakın.

![Özel Azure AD Kiracı etki alanı hesabı](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain3.png)

### <a name="using-encrypted-media-extensions-for-playready"></a>Şifrelenmiş Media uzantıları için PlayReady kullanma
PlayReady desteği, Windows 8.1 ve yukarı IE 11 ve Windows 10, Microsoft Edge tarayıcısının gibi modern tarayıcının şifrelenmiş medya Uzantıları (EME) ile PlayReady EME için temel alınan DRM olacaktır.

![PlayReady için EME kullanma](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-playready1.png)

PlayReady koruması bir ekran yakalama korumalı videonun yapmasını engeller due için koyu player alanı olabilir.

Aşağıdaki ekran player eklenti ve MSE/EME destek gösterir.

![PlayReady için EME kullanma](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-playready2.png)

Microsoft Edge ve Windows 10 IE 11 EME sağlar, çağırma [PlayReady SL3000](https://www.microsoft.com/playready/features/EnhancedContentProtection.aspx/) destekleyen Windows 10 cihazlarda. PlayReady SL3000 geliştirilmiş premium içerik akışı kilidini açar (4K, HDR, vb.) ve yeni içerik teslim modellerinden (Gelişmiş içerik için erken pencere).

Odak Windows cihazlarda: PlayReady yalnızca DRM (PlayReady SL3000) Windows cihazlarda kullanılabilir donanım içinde değil. Bir akış hizmeti PlayReady EME ya da bir UWP uygulamasını kullanın ve başka bir DRM daha PlayReady SL3000 kullanarak daha yüksek bir video kalitesini sunar. Genellikle, 2K içerik Chrome veya Firefox ve 4 K içeriği Microsoft Edge/IE11 veya bir UWP uygulaması (bağlı olarak hizmet ayarlarını ve uygulama) aynı aygıtta aracılığıyla aracılığıyla akar.

#### <a name="using-eme-for-widevine"></a>Widevine için EME kullanma
Modern bir tarayıcı, Chrome 41 + Windows 10, Windows 8.1, Mac OSX Yosemite ve Chrome gibi EME/Widevine destek Android 4.4.4 ile Google Widevine DRM EME arkasında açıktır.

![Widevine için EME kullanma](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-widevine1.png)

Widevine bir ekran yakalama korumalı videonun yapmasını engellemeyeceğini dikkat edin.

![Widevine için EME kullanma](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-widevine2.png)

### <a name="not-entitled-users"></a>Değil yetkili kullanıcılar
Bir kullanıcı "Başlıklı kullanıcıları" grubunun bir üyesi değilse, kullanıcının "yetkilendirme denetimi" başarılı olmaz ve çoklu DRM lisans hizmeti aşağıda gösterildiği gibi istenen lisansı vermek reddeder. Ayrıntılı açıklama "Lisans başarısız oldu", tasarlandığı gibi olduğu.

![Beklemediğiniz yetkili kullanıcılar](./media/media-services-cenc-with-multidrm-access-control/media-services-unentitledusers.png)

### <a name="running-custom-secure-token-service"></a>Özel güvenli belirteç hizmeti çalışıyor
Özel güvenli belirteç hizmeti (STS) çalıştıran senaryo için JWT belirteci simetrik ya da asimetrik anahtar kullanılarak özel STS tarafından verilir.

Simetrik anahtar (Chrome kullanarak) kullanarak, durum:

![Özel STS çalıştırma](./media/media-services-cenc-with-multidrm-access-control/media-services-running-sts1.png)

Bir X509 asimetrik anahtarla kullanma durumu (Microsoft modern tarayıcısı kullanılarak) sertifika.

![Özel STS çalıştırma](./media/media-services-cenc-with-multidrm-access-control/media-services-running-sts2.png)

Yukarıdaki durumların her ikisinde kullanıcı kimlik doğrulaması aynı – Azure AD ile kalır. Tek fark, JWT belirteçleri Azure AD yerine özel STS tarafından verilen olmasıdır. Elbette, dinamik CENC koruma yapılandırırken, JWT belirteci türünü lisans teslimat hizmeti kısıtlama belirtir simetrik ya da asimetrik anahtar.

## <a name="summary"></a>Özet
Bu belgede, biz CENC belirteci kimlik doğrulaması aracılığıyla çok native DRM ve erişim denetimi ile açıklanan: tasarımını ve Azure, Azure Media Services ve Azure Media Player kullanarak kendi uygulama.

* Bir başvuru tasarımı DRM/CENC alt sistemi tüm gerekli bileşenleri içeren sunulur;
* Bir başvuru uygulaması Azure, Azure Media Services ve Azure Media Player.
* Tasarım ve uygulama doğrudan ilgili bazı konular da ele alınmıştır.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
 
