---
title: Birincil yenileme belirteci (PRT) ve Azure AD - Azure Active Directory
description: Rolü nedir ve birincil yenileme belirteci (PRT) Azure Active Directory'de nasıl yönetilir?
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 05/29/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: ravenn
ms.collection: M365-identity-device-management
ms.openlocfilehash: 16e4a5f63ba80b02a967888ad76fedf165a576c8
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66473407"
---
# <a name="what-is-a-primary-refresh-token"></a>Bir birincil yenileme belirteci nedir?

Bir birincil yenileme belirteci (PRT), Windows 10, iOS ve Android cihazlarda Azure AD kimlik doğrulama anahtarı bir yapıdır. Bu, bir JSON Web Token (JWT) bu cihazlar üzerinde kullanılan tüm uygulamalarda çoklu oturum açma (SSO) etkinleştirmek için Microsoft birinci taraf belirteç aracıları için özel olarak verilen olur. Bu makalede, nasıl bir PRT verilen, kullanıldığı ve Windows 10 cihazlarda korumalı hakkında ayrıntılar sağlayacağız.

Bu makalede, zaten farklı bir cihaz durumları Azure AD'de kullanılabilir ve nasıl tek anladığınızı varsayar. oturum açma Windows 10'da çalışır. Azure AD'de cihazları hakkında daha fazla bilgi için bkz [Azure Active Directory'de cihaz yönetimi nedir?](overview.md)

## <a name="key-terminology-and-components"></a>Önemli terimleri ve bileşenleri

Aşağıdaki Windows bileşenleri isteyen ve bir PRT kullanarak önemli bir rol oynar:

* **Bulut kimlik doğrulama sağlayıcısı** (CloudAP): Windows, Windows 10 cihazına oturum açan kullanıcılar doğrulayan oturum açma için CloudAP modern kimlik doğrulamasıdır. CloudAP, kimlik sağlayıcıları için Windows kimlik doğrulamasını etkinleştirmek için bu kimlik sağlayıcısının kimlik bilgilerini kullanarak oluşturabileceğiniz bir eklenti çerçeve sağlar.
* **Web hesabı yöneticisi** (WAM): WAM Windows 10 cihazlarında varsayılan belirteci aracısıdır. WAM eklentisi çerçevesi, kimlik sağlayıcıları oluşturmak ve bağlı olan bu kimlik sağlayıcısını kullanıcıların uygulamalar için SSO'yu etkinleştirmek de sağlar.
* **Azure AD CloudAP eklentisi**: CloudAP framework üzerine inşa edilmiş bir Azure AD belirli eklentisini Windows oturum açma sırasında Azure AD ile kullanıcı kimlik bilgilerini doğrular.
* **Azure AD WAM eklentisi**: WAM framework üzerine inşa edilmiş bir Azure AD belirli eklentisi, Azure AD kimlik doğrulaması kullanan uygulamalar için SSO sağlar.
* **Dsreg**: Belirli bir Azure AD bileşeni tüm cihaz durumları için cihaz kayıt işlemini işleyen Windows 10.
* **Güvenilir Platform Modülü** (TPM): TPM, kullanıcı ve cihaz gizli dizileri donanım tabanlı güvenlik işlevleri sağlayan bir cihaz, yerleşik bir donanım bileşenidir. Daha fazla bilgi makalesinde bulunabilir [Platform Modülü teknoloji genel bakış güvenilen](https://docs.microsoft.com/windows/security/information-protection/tpm/trusted-platform-module-overview).

## <a name="what-does-the-prt-contain"></a>Ne PRT içeriyor mu?

Bir PRT genellikle tüm Azure AD yenileme belirtecinde yer alan talepleri içerir. Ayrıca, PRT dahil bazı cihaza özgü talep vardır. Bunlar aşağıda belirtilmiştir:

* **Cihaz kimliği**: Belirli bir cihazdaki bir kullanıcı için bir PRT verilir. Cihaz kimliği talebi `deviceID` PRT üzerinde kullanıcı tarafından verildiği cihaz belirler. Bu talep, daha sonra PRT elde edilen belirteçleri verilir. Cihaz kimliği talebi, cihaz durumu veya uyumluluk göre koşullu erişim için yetkilendirme belirlemek için kullanılır.
* **Oturum anahtarı**: Şifrelenmiş bir simetrik anahtar, oturum anahtarı olan PRT bir parçası olarak yayımlanan Azure AD kimlik doğrulama hizmeti tarafından oluşturulur. Diğer uygulamalar için belirteçleri elde etmek için bir PRT kullanıldığında oturum anahtarı kanıtını görev yapar.

### <a name="can-i-see-whats-in-a-prt"></a>İçinde bir PRT ne olduğunu görebilirim?

Bir PRT içerikleri, herhangi bir istemci bileşenleri için bilinmemesi Azure AD'den gönderilen opak bir blob ' dir. İçinde bir PRT nedir göremez.

## <a name="how-is-a-prt-issued"></a>Bir PRT nasıl verilir?

Cihaz kaydı, Azure AD'de cihaz tabanlı kimlik doğrulaması için bir önkoşuldur. Bir PRT yalnızca kayıtlı cihazlarda kullanıcılara verilir. Cihaz kaydı ilişkin daha kapsamlı bilgi için bkz [Windows Hello iş ve cihaz kaydı için](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-how-it-works-device-registration). Cihaz kaydı sırasında dsreg bileşen iki şifreleme anahtar çiftleri kümesini oluşturur:

* Cihaz anahtarı (dkpub/dkpriv)
* Anahtar (tkpub/tkpriv) taşıma

Cihaz Azure AD'ye cihaz kayıt işlemi sırasında ortak anahtarları gönderilirken geçerli ve düzgün bir TPM varsa, özel anahtarlar cihazın TPM bağlıdır. Bu anahtarlar PRT istekleri sırasında cihaz durumunu doğrulamak için kullanılır.

Bir Windows 10 cihazda iki senaryoda kullanıcı kimlik doğrulaması sırasında PRT verilen:

* **Azure AD'ye katılmış** veya **hibrit Azure AD'ye katıldı**: Kullanıcı kuruluş kimlik bilgileriyle oturum açtığında bir PRT Windows oturum açma sırasında verilir. Bir PRT tüm Windows 10 desteklenen kimlik bilgileriyle, örneğin, parola ve Windows iş için Hello verilir. Bu senaryoda, Azure AD CloudAP PRT birincil yetkilisini eklentisidir.
* **Azure AD'ye kayıtlı cihaz**: Bir kullanıcı Windows 10 cihazlarına İkincil iş hesabı eklediğinde bir PRT verilir. Kullanıcılar, Windows 10 için iki farklı şekilde bir hesap ekleyebilir-  
   * Aracılığıyla bir hesap ekleme **bu hesabın bu cihaz üzerinde her yerde kullanın** bir uygulamaya (örneğin, Outlook) oturum açtıktan sonra sor
   * Bir hesaptan ekleme **ayarları** > **hesapları** > **işe veya okula erişim** > **Bağlan**

Bu senaryolarda, Azure AD WAM eklentisi PRT birincil yetkilisini olduğu Windows oturum açma, bu Azure AD hesabıyla oluyor değil.

## <a name="what-is-the-lifetime-of-a-prt"></a>Bir PRT ömrünü nedir?

Yayımlandıktan sonra bir PRT 14 gün boyunca geçerlidir ve kullanıcı cihaz etkin olarak kullandığı sürece sürekli olarak yenilenir.  

## <a name="how-is-a-prt-used"></a>Bir PRT nasıl kullanılır?

Bir PRT Windows içinde iki anahtar bileşenler tarafından kullanılır:

* **Azure AD CloudAP eklentisi**: Windows oturum açma sırasında kullanıcı tarafından sağlanan kimlik bilgilerini kullanarak Azure AD'den bir PRT Azure AD CloudAP eklentisi ister. Ayrıca, kullanıcının bir internet bağlantısına erişim yok önbelleğe alınmış oturum açma olanağı PRT önbelleğe alır.
* **Azure AD WAM eklentisi**: Azure AD WAM eklentisi PRT kullanıcılar uygulamalara erişmeyi denediğinde, Windows 10 SSO'yu etkinleştirmek üzere kullanır. Azure AD WAM eklentisi PRT üzerinde WAM belirteç isteklerini kullanan uygulamalar için yenileme ve erişim belirteçlerini istemek için kullanır. Ayrıca SSO tarayıcılarda tarayıcı isteklerine PRT ekleyerek sağlar. Windows 10'daki SSO desteklenen bir tarayıcıdan Microsoft Edge üzerinde (yerel) ve Chrome (aracılığıyla Windows 10 hesapları ya da Office Online uzantısı).

## <a name="how-is-a-prt-renewed"></a>Bir PRT nasıl yenilenir?

Bir PRT içindeki iki farklı yöntem yenilenir:

* **Azure AD CloudAP eklentisi 4 saatte**: CloudAP eklentisi PRT 4 saatte bir Windows oturum açma sırasında yeniler. Kullanıcının bu süre boyunca internet bağlantısı yoksa, cihazın internet'e bağlı sonra CloudAP eklentisi PRT yenilenir.
* **Uygulama belirteci isteklerini sırasında Azure AD WAM eklentisi**: WAM eklentisi, Windows 10 cihazlarında uygulamalar için sessiz belirteci isteklerini etkinleştirerek SSO sağlar. WAM eklentisi, iki farklı şekilde bu belirteç isteği sırasında PRT yenileme yapabilir:
   * İçin bir erişim belirteci WAM uygulama sessizce isteyen, ancak bu uygulama için herhangi bir yenileme belirteci yok. Bu durumda, WAM PRT uygulama için bir belirteç istemek için kullanır ve yeni PRT yanıtta geri alır.
   * Bir uygulama için bir erişim belirteci WAM ister, ancak PRT geçersiz veya Azure AD ek yetkilendirme (örneğin, Azure multi-Factor Authentication) gerektirir. Bu senaryoda, kullanıcının yeniden kimlik doğrulamaya zorlayabilir veya ek doğrulama sağlayan gerektiren bir etkileşimli oturum açma WAM başlatır ve yeni PRT başarılı bir kimlik verilir.

### <a name="key-considerations"></a>Dikkat edilmesi gereken temel konular

* Yalnızca bir PRT verilen ve yerel uygulama kimlik doğrulaması sırasında yenilendi. Bir PRT değil yenilenmesi veya bir tarayıcı oturumu sırasında verilen.
* Azure AD'ye katılmış ve hibrit Azure AD'ye katılmış cihazların, CloudAP eklentisi bir PRT birincil yetkilisini olur. Bir PRT bir WAM tabanlı belirteç isteği sırasında yenilenirse PRT kabul etmeden önce Azure AD ile PRT geçerliliğini doğrular geri CloudAP eklentisi, gönderilir.

## <a name="how-is-the-prt-protected"></a>PRT nasıl korunur?

Bir PRT bağlama, cihaza kullanıcı oturumu açtığı korunur. Azure AD ve Windows 10, aşağıdaki yöntemleri kullanarak PRT korumayı etkinleştir:

* **İlk oturum açma sırasında**: Sırasında ilk oturum açma, cihaz kaydı sırasında şifreli olarak üretilen cihaz anahtarı kullanarak istekleri imzalayarak bir PRT verilir. Geçerli ve çalışır durumda bir TPM'e sahip bir cihazda cihaz anahtarı herhangi bir kötü amaçlı erişimi engelleyen TPM tarafından sağlanır. Karşılık gelen cihaz anahtarı imzası doğrulanamıyor, bir PRT verilmedi.
* **Belirteç isteklerini ve yenileme sırasında**: Bir PRT verildiğinde, Azure AD cihaza bir şifrelenmiş oturum anahtarının de verir. Oluşturulan ve Azure AD'ye cihaz kayıt işleminin bir parçası olarak gönderilen kamu ulaşımını anahtarı (tkpub) ile şifrelenir. Bu oturum anahtarı yalnızca TPM tarafından güvenliği sağlanan özel aktarım anahtarı (tkpriv) tarafından şifresi çözülemez. Oturum anahtarı Azure AD'ye gönderilen tüm istekler için elinde kavram (POP) anahtardır.  Oturum anahtarı, aynı zamanda olarak TPM ile korunduğunu ve diğer bir işletim sistemi bileşeni, erişebilir. Belirteç isteklerini veya PRT yenileme istekleri güvenli bir şekilde TPM aracılığıyla bu oturum anahtarı tarafından imzalanan ve bu nedenle, oynanamaz. Azure AD, karşılık gelen oturum anahtarıyla imzalanmamış tüm istekleri CİHAZDAN geçersiz kılar.

Bu anahtarları TPM ile güvenli hale getirme kötü amaçlı aktörler anahtarları çalabilir veya başka bir yerde bir saldırganın cihaza fiziksel olarak sahip olsa bile TPM erişilemez olduğu gibi PRT yeniden yürütün.  Bu nedenle, TPM büyük ölçüde kullanarak hibrit Azure AD'ye katılmış, Azure AD katıldı güvenliğini artırır ve kimlik hırsızlığına karşı cihazları Azure AD'ye kayıtlı. Performans ve güvenilirlik için TPM 2.0 Windows 10 tüm Azure AD cihaz kaydı senaryoları için önerilen sürümdür.

### <a name="how-are-app-tokens-and-browser-cookies-protected"></a>Uygulama belirteçleri ve tarayıcı tanımlama korumalı nasılsın?

**Uygulama belirteçleri**: Azure AD, bir uygulama belirteci aracılığıyla WAM istediğinde, bir yenileme belirteci ve bir erişim belirteci verir. Ancak WAM yalnızca uygulamaya erişim belirteci verir ve kullanıcının veri koruma uygulama programlama arabirimi (DPAPI) anahtarı ile şifreleyerek önbelleğini yenileme belirteci güvenliğini sağlar. WAM güvenli bir yenileme belirteci isteklerini oturum anahtarıyla imzalayarak daha fazla erişim belirteçlerini vermek için kullanır. DPAPI anahtarı Azure AD'de kendi Azure AD tabanlı simetrik anahtar tarafından güvenlik altına alınır. Azure AD, cihazın kullanıcı profilindeki DPAPI Anahtar ile şifre çözme gerektiğinde, hangi CloudAP eklentisi şifresini çözmek için TPM'nin istekleri oturumu anahtar ile şifrelenmiş DPAPI Anahtar sağlar. Bu işlev, yenileme belirteçleri güvenliğindeki tutarlılık sağlar ve uygulamaları, kendi koruma mekanizmaları uygulama önler.  

**Tarayıcı tanımlama**: Windows 10, Azure AD SSO Internet Explorer ve Microsoft Edge tarayıcı yerel olarak veya Google chrome'da Windows 10 hesapları uzantısı destekler. Güvenlik, yalnızca tanımlama bilgilerini aynı zamanda tanımlama bilgilerini gönderildiği uç noktaları korumak için tasarlandı. Tarayıcı tanımlama bilgileri, aynı şekilde korunur, oturum ve tanımlama bilgilerini korumak için oturum anahtarı yararlanarak bir PRT ise.

Bir kullanıcı, bir tarayıcı etkileşimi başlattığında, tarayıcı (veya uzantısı) COM yerel istemci konak çağırır. Yerel istemci ana sayfa izin verilen etki alanlarından biri olmasını sağlar. Tarayıcı, yerel istemcisi için diğer parametreler, nonce dahil olmak üzere, ancak yerel istemci konak ana bilgisayar doğrulaması garanti konak gönderebilirsiniz. Yerel istemci konak PRT-tanımlama bilgisi oluşturur ve TPM korumalı oturum anahtarıyla imzalar CloudAP eklentisi, gelen istekleri. Tanımlama bilgisi PRT oturum anahtarı ile imzalanmış gibi ile değiştirilmemesi. Bu tanımlama bilgisi PRT istek üstbilgisi alanından kaynaklanan cihaz doğrulamak Azure AD için dahil edilir. Chrome tarayıcı kullanıyorsanız, yalnızca açıkça yerel istemci konağın bildiriminde tanımlanan uzantısı, bu istekler yapmasını rastgele uzantıları engelleme çağırabilirsiniz. Azure AD'ye PRT tanımlama bilgisi doğrulama sonra tarayıcıya bir oturum tanımlama bilgisini verir. Bu oturum tanımlama bilgisini Ayrıca aynı oturum anahtarı ile bir PRT verilen içerir. Sonraki istekleri sırasında oturum anahtarı etkin cihaza bir tanımlama bilgisi bağlama ve başka bir yerden olumsuzlukları önleme doğrulanır.

## <a name="when-does-a-prt-get-an-mfa-claim"></a>Bir PRT MFA talebi ulaştıklarında?

Bir PRT multi factor authentication (MFA) talep belirli senaryolarda alabilirsiniz. Bir MFA tabanlı PRT uygulamalar için belirteçler istemek için kullanıldığında, bu uygulama belirteçleri için MFA talebi aktarılır. Bu işlev, bunu gerektiren her uygulama için MFA testini engelleyerek, kullanıcılar için sorunsuz bir deneyim sunar. Bir PRT MFA talebi aşağıdaki yollarla alabilirsiniz:

* **İş için Windows Hello ile oturum**: Windows iş için Hello parolaları değiştirir, şifreleme anahtarlarını güçlü iki öğeli kimlik doğrulaması sağlamak için kullanır. Windows iş için Hello bir cihazdaki bir kullanıcı için özeldir ve kendisini MFA sağlamayı gerektirir. Oturum Windows iş için Hello bir kullanıcı oturum açtığında, kullanıcının PRT MFA talebi alır. Bu senaryo, akıllı kart kimlik doğrulaması, bir ADFS MFA talep oluşturursa, akıllı kart ile oturum açan kullanıcılar için de geçerlidir.
   * Windows iş için Hello çok faktörlü kimlik doğrulaması olarak kabul edilir, PRT yenilendiğinde, kullanıcılar iş için WIndows Hello ile oturum zaman MFA süresi sürekli olarak genişletecek şekilde MFA talebi güncelleştirilir
* **WAM etkileşimli oturum açma sırasında MFA**: Bir kullanıcının uygulamaya erişmek için mfa'yı yapmanız gerekiyorsa WAM aracılığıyla bir belirteç isteği sırasında bu etkileşim sırasında yenilenen PRT bir MFA talebi ile imprinted.
   * Bu durumda, dizinde belirlenmiş kullanım ömrü MFA süre dayanır için MFA talebi sürekli olarak güncelleştirilmez.
* **Cihaz kaydı sırasında MFA**: Yönetici Azure AD'ye cihaz ayarlarına yapılandırılmış olmadığını [cihazları kaydetmek mfa'yı gerekli](device-management-azure-portal.md#configure-device-settings), kullanıcının kaydı tamamlamak için mfa'yı yapması için gerekli. Bu işlem sırasında bir kullanıcıya verilen PRT elde kaydı sırasında MFA talebi sahiptir. Bu özellik, yalnızca bu cihazda oturum açın diğer kullanıcılara değil, birleştirme işlemi gerçekleştiren kullanıcı için geçerlidir.
   * MFA süresi, dizinde belirlenmiş kullanım ömrü dayalı şekilde benzer WAM etkileşimli oturum açma MFA talebi sürekli olarak güncelleştirilmez.

Windows 10 bölümlenmiş PRTs listesini her kimlik bilgisi tutar. Bu nedenle, her biri için Windows Hello iş, parola veya akıllı kart için bir PRT yoktur. Bu bölümleme değil karma sırasında belirteci isteklerini ve kullanılan kimlik bilgisi dayalı MFA talep yalıtılmış olmasını sağlar.

## <a name="how-is-a-prt-invalidated"></a>Nasıl bir PRT geçersiz kılınır?

Aşağıdaki senaryolarda bir PRT geçersiz:

* **Geçersiz kullanıcı**: Bir kullanıcı silindi ya da devre dışı Azure AD'de kendi PRT geçersiz kılınır ve uygulamalar için belirteçleri elde etmek için kullanılamaz. Silinmiş veya devre dışı bir kullanıcı zaten bir cihaza önce oturum CloudAP geçersiz kendi durumunu uyumlu olana kadar önbelleğe alınmış oturum açma, oturum açabilirsiniz. Kullanıcı geçersiz CloudAP belirledikten sonra sonraki oturum açmalarınız engeller. Geçersiz bir kullanıcı otomatik olarak oturum açma kimlik bilgilerini önbelleğe sahip olmayan yeni cihazlara engellenir.
* **Geçersiz aygıt**: Bir cihaz silindi veya Azure AD'de devre dışı, bu aygıtta alınan PRT geçersiz kılınır ve diğer uygulamalar için belirteçleri elde etmek için kullanılamaz. Bir kullanıcı zaten geçersiz cihazına oturum açtıysa, bunu yapmak devam edebilirsiniz. Ancak, cihazdaki tüm belirteçlerin geçersiz kılınır ve kullanıcının, bu CİHAZDAN herhangi bir kaynakta SSO yok.
* **Parola değiştirme**: Bir kullanıcı parolasını değiştirene sonra önceki parola ile elde edilen PRT Azure AD tarafından geçersiz kılınır. Parola sonuçları yeni PRT alma kullanıcı değiştirin. Bu geçersiz kılma iki farklı şekilde gerçekleşebilir:
   * Windows için yeni bir parola ile oturum açtığında, CloudAP eski PRT atar ve kendi yeni parolayla yeni PRT vermek için Azure AD ister. Kullanıcı bir internet bağlantısı yoksa, yeni parola doğrulanamıyor, Windows kullanıcının eski parolalarını girmeleri gerekebilir.
   * Bir kullanıcı kendi eski parolayla oturum veya parolasını Windows oturum sonra değişti, eski PRT tüm WAM tabanlı belirteç isteklerinin için kullanılır. Bu senaryoda, kullanıcı WAM belirteç isteği sırasında yeniden kimlik doğrulamaya zorlayabilir istenir ve yeni PRT verilir.
* **TPM sorunları**: Bazı durumlarda, bir cihazın TPM falter veya TPM ile korunan anahtarların inaccessibility için önde gelen başarısız olur. Bu durumda, cihaz şifreleme anahtarları elinde kanıtlayın olamaz gibi mevcut bir PRT kullanarak belirteçleri isteyen bir PRT alma veya yetersiz. Sonuç olarak, tüm mevcut PRT Azure AD tarafından geçersiz kılınır. Windows 10, bir hata algıladığında, cihaz yeni şifreleme anahtarlarıyla yeniden kaydolmak için bir kurtarma akış başlatır. Kurtarma, ilk kayıt gibi hibrit Azure Ad join ile kullanıcı girişi olmadan sessizce gerçekleşir. Azure AD'ye katılmış veya Azure AD kayıtlı cihazlar için kurtarma cihazda yönetici ayrıcalıklarına sahip bir kullanıcı tarafından gerçekleştirilmesi gerekir. Bu senaryoda, kullanıcı cihazı başarılı bir şekilde kurtarmak için yol gösteren bir Windows istemi kurtarma akışı başlatılır.

## <a name="detailed-flows"></a>Ayrıntılı akışlar

Aşağıdaki diyagramlarda verme, yenileme ve bir uygulama için bir erişim belirteci istemek için bir PRT kullanarak altyapısal Ayrıntılar gösterilmektedir. Ayrıca, bu adımları ayrıca yukarıda sözü edilen güvenlik mekanizmalarını bu etkileşimleri sırasında nasıl uygulanacağını açıklar.

### <a name="prt-issuance-during-first-sign-in"></a>İlk oturum açma sırasında PRT verme

![İlk oturum açma ayrıntılı akış sırasında PRT verme](./media/concept-primary-refresh-token/prt-initial-sign-in.png)

> [!NOTE]
> Azure AD'de kullanıcı için Windows oturum açabilmeniz için bir PRT vermek için bu exchange zaman uyumlu olur cihazları katıldı. Karma Azure AD'ye katılmış cihazlar, şirket içi Active Directory birincil yetkilisidir. Bu nedenle, zaman uyumsuz olarak PRT verme gerçekleşirken oturum açmak için TGT edinebileceğinizi kadar kullanıcı yalnızca bekliyor. Oturum açma, Azure AD kimlik bilgilerini kullanmıyor gibi bu senaryo Azure AD'ye kayıtlı cihazlar için geçerli değildir.

| Adım | Açıklama |
| :---: | --- |
| A | Kullanıcı oturum açma kullanıcı Arabirimi parolasını girer. LogonUI kimlik bilgilerini bir kimlik doğrulama arabellek CloudAP için dahili olarak geçirir, sırayla LSA geçirir. CloudAP CloudAP eklentisi bu isteği iletir. |
| B | Kullanıcı için kimlik sağlayıcısına tanımlamak için bir bölge bulma isteği CloudAP eklentisi başlatır. Kullanıcının kiracısı bir Federasyon sağlayıcı kurulumunu varsa, Azure AD Federasyon sağlayıcıya ait meta veri değişimi uç noktası (MEX) uç noktasını döndürür. Aksi takdirde, Azure AD, kullanıcının Azure AD ile kimlik doğrulaması yapabilir belirten kullanıcı yönetilse döndürür. |
| C | Kullanıcı yönetiliyorsa, CloudAP nonce Azure AD'den elde edersiniz. Kullanıcı birleştiriliyorsa CloudAP eklentisini kullanıcının kimlik bilgileri ile Federasyon sağlayıcısı SAML belirteci ister. Bunu SAML belirteci aldıktan sonra Azure AD'den nonce ister. |
| D | CloudAP eklentisi isteğini (dkpriv) cihaz anahtarıyla imzalar kullanıcının kimlik bilgilerini ve nonce Aracısı kapsamı ile kimlik doğrulama isteği oluşturur ve Azure AD'ye gönderir. Kullanıcı yerine Federasyon sağlayıcısı tarafından döndürülen SAML belirteci birleştirilmiş bir ortamda CloudAP eklentisi ' kimlik bilgileri. |
| E | Azure AD kullanıcı kimlik bilgilerini nonce doğrular ve cihaz imzası, cihaz kiracıda geçerli olduğunu ve şifrelenmiş PRT sorunlar olduğunu doğrular. PRT yanı sıra Azure AD'ye aktarım anahtarı (tkpub) kullanarak Azure AD tarafından şifrelenmiş oturum anahtarının adlı bir simetrik anahtar, ayrıca verir. Ayrıca, oturum anahtarı içinde PRT katıştırılır. Bu oturum anahtarı ile PRT sonraki istekler için elinde kavram (PoP) anahtar görevi görür. |
| F | CloudAP eklentisi için CloudAP şifrelenmiş PRT ve oturum anahtarı geçirir. CloudAP aktarım anahtarı (tkpriv) kullanarak oturum anahtarının şifresini çözmek için TPM'nin istek ve TPM'nin kendi anahtarını kullanarak yeniden şifrelenemedi. CloudAP önbelleğinde PRT birlikte şifrelenmiş oturum anahtarının depolar. |

### <a name="prt-renewal-in-subsequent-logons"></a>Sonraki oturum açmalarınız içinde PRT yenileme

![Sonraki oturum açmalarınız içinde PRT yenileme](./media/concept-primary-refresh-token/prt-renewal-subsequent-logons.png)

| Adım | Açıklama |
| :---: | --- |
| A | Kullanıcı oturum açma kullanıcı Arabirimi parolasını girer. LogonUI kimlik bilgilerini bir kimlik doğrulama arabellek CloudAP için dahili olarak geçirir, sırayla LSA geçirir. CloudAP CloudAP eklentisi bu isteği iletir. |
| B | Kullanıcı daha önce kullanıcı için oturum açtıysa, önbelleğe alınmış Windows başlatır oturum açın ve kullanıcı oturum açma kimlik bilgilerini doğrular. 4 saatte CloudAP eklentisi PRT yenileme zaman uyumsuz olarak başlatır. |
| C | Kullanıcı için kimlik sağlayıcısına tanımlamak için bir bölge bulma isteği CloudAP eklentisi başlatır. Kullanıcının kiracısı bir Federasyon sağlayıcı kurulumunu varsa, Azure AD Federasyon sağlayıcıya ait meta veri değişimi uç noktası (MEX) uç noktasını döndürür. Aksi takdirde, Azure AD, kullanıcının Azure AD ile kimlik doğrulaması yapabilir belirten kullanıcı yönetilse döndürür. |
| D | Kullanıcı birleştiriliyorsa CloudAP eklentisini kullanıcının kimlik bilgileri ile Federasyon sağlayıcısı SAML belirteci ister. Bunu SAML belirteci aldıktan sonra Azure AD'den nonce ister. Kullanıcı yönetiliyorsa, CloudAP nonce doğrudan Azure AD'den alırsınız. |
| E | CloudAP eklentisi oturum anahtarı ile isteğini imzalar kullanıcının kimlik bilgilerini, nonce ve mevcut PRT ile kimlik doğrulama isteği oluşturur ve Azure AD'ye gönderir. Kullanıcı yerine Federasyon sağlayıcısı tarafından döndürülen SAML belirteci birleştirilmiş bir ortamda CloudAP eklentisi ' kimlik bilgileri. |
| F | Azure AD içinde PRT katıştırılmış oturum anahtarı karşı karşılaştırarak oturum anahtarı imzayı doğrular, nonce doğrular ve cihaz kiracıda geçerli olduğunu ve yeni PRT sorunlar olduğunu doğrular. Önce görüldüğü gibi PRT yeniden taşıma (tkpub) anahtar ile şifrelenmiş oturum anahtarı ile eşlik eder. |
| G | CloudAP eklentisi için CloudAP şifrelenmiş PRT ve oturum anahtarı geçirir. CloudAP aktarım anahtarı (tkpriv) kullanarak oturum anahtarının şifresini çözmek için TPM'nin istekleri ve TPM'nin kendi anahtarını kullanarak yeniden şifrelenemedi. CloudAP önbelleğinde PRT birlikte şifrelenmiş oturum anahtarının depolar. |

### <a name="prt-usage-during-app-token-requests"></a>Uygulama belirteci isteklerini sırasında PRT kullanımı

![Uygulama belirteci isteklerini sırasında PRT kullanımı](./media/concept-primary-refresh-token/prt-usage-app-token-requests.png)

| Adım | Açıklama |
| :---: | --- |
| A | Bir uygulama (örneğin, Outlook, OneNote vb.) WAM için bir belirteç isteğini başlatır. WAM, buna karşılık, belirteç isteğe hizmet vermek için Azure AD WAM eklentisi ister. |
| B | Uygulama için bir yenileme belirteci zaten varsa, Azure AD WAM eklentisi, bir erişim belirteci istemek için kullanır. Kavram kanıtı cihaz bağlama sağlamak için WAM eklentisi istek oturum anahtarıyla imzalar. Azure AD oturum anahtarı doğrular ve bir erişim belirteci ve oturum anahtarı tarafından şifrelenmiş uygulama için yeni bir yenileme belirteci verir. WAM eklentisi, buna karşılık her iki belirteçleri alma WAM eklenti kaynaklanan oturum anahtarı kullanarak şifresini çözmek için TPM'nin istekleri belirteçlerin şifresini çözmek için bulut AP eklentisi ister. Ardından, DPAPI ile yenileme belirtecini yeniden şifreler ve kendi önbellekte depolar WAM eklentisi yalnızca uygulama için erişim belirteci sağlar  |
| C |  Uygulama için bir yenileme belirteci kullanılabilir durumda değilse, Azure AD WAM eklentisi PRT bir erişim belirteci istemek için kullanır. Kanıtını sağlamak için WAM eklentisi PRT oturum anahtarı içeren isteğini imzalar. Azure AD içinde PRT katıştırılmış oturum anahtarı karşı karşılaştırarak oturum anahtarı imzayı doğrular, cihazın geçerli olduğunu ve bir erişim belirteci ve uygulama için bir yenileme belirteci verir doğrular. Ayrıca, Azure AD (yenileme döngüsünde bağlı olarak), yeni bir PRT verebilir tümünün oturum anahtarı ile şifrelenir. |
| D | WAM eklentisi, buna karşılık her iki belirteçleri alma WAM eklenti kaynaklanan oturum anahtarı kullanarak şifresini çözmek için TPM'nin istekleri belirteçlerin şifresini çözmek için bulut AP eklentisi ister. Ardından, DPAPI ile yenileme belirtecini yeniden şifreler ve kendi önbellekte depolar WAM eklentisi yalnızca uygulama için erişim belirteci sağlar. WAM eklentisi, bu uygulama için ileriye dönük yenileme belirtecini kullanır. WAM eklentisi Ayrıca geri yeni PRT doğrular, kendi önbellekte güncelleştirmeden önce Azure AD ile PRT bulut AP eklentisi sağlar. Bundan sonra yeni PRT bulut AP eklentisi kullanır. |
| E | Çağıran uygulamanın sağladığı sırayla WAM, yeni verilen erişim belirteci WAM sağlar|

### <a name="browser-sso-using-prt"></a>Tarayıcı SSO PRT kullanma

![Tarayıcı SSO PRT kullanma](./media/concept-primary-refresh-token/browser-sso-using-prt.png)

| Adım | Açıklama |
| :---: | --- |
| A | Kullanıcı Windows için bir PRT almak için kimlik bilgilerini kaydeder. Kullanıcı tarayıcının açtığında, tarayıcı (veya uzantısı) URL'leri kayıt defterinden yükler. |
| B | Bir Azure AD oturum açma URL'si bir kullanıcı oturum açtığında, kayıt defterinden alınan fiyatlarla URL'sini bir tarayıcı veya uzantı doğrular. Eşleşirlerse, tarayıcının yerel istemci konak bir belirteç almak için çağırır. |
| C | Yerel istemci konak URL'leri Microsoft kimlik sağlayıcıları (Microsoft hesabı veya Azure AD) ait, URL'deki gönderilen nonce ayıklar ve CloudAP eklentisi PRT tanımlama bilgisi almak için çağrıda olduğunu doğrular. |
| D | CloudAP eklentisi PRT tanımlama bilgisini oluşturmak, TPM bağlı oturum anahtarı ile oturum açın ve yerel istemci konağa geri gönderin. Tanımlama bilgisi oturum anahtarı ile imzalanmış gibi ile değiştirilmemesi. |
| E | Yerel istemci konak bu PRT tanımlama bilgisi x-ms-RefreshTokenCredential ve istek belirteçleri Azure AD'den adlı istek üst bilgisi bir parçası olarak dahil edilir tarayıcıya döndürür. |
| F | Azure AD PRT tanımlama bilgisi oturum anahtarı imzayı doğrular, nonce doğrular, cihaz kiracıda geçerli olduğunu ve web sayfası için bir kimlik belirteci ve tarayıcı için bir şifrelenmiş oturum tanımlama bilgisi sorunlar olduğunu doğrular. |

## <a name="next-steps"></a>Sonraki adımlar

PRT ile ilgili sorunları giderme hakkında daha fazla bilgi için bkz [sorun giderme hibrit Azure Active Directory'ye katılmış Windows 10 ve Windows Server 2016 cihazları](troubleshoot-hybrid-join-windows-current.md).
