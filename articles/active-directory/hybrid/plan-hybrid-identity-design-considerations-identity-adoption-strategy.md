---
title: Karma kimlik tasarımı - benimseme stratejisi Azure | Microsoft Docs
description: Koşullu erişim denetimi ile Azure Active Directory kullanıcı kimlik doğrulaması yapılırken ve uygulamaya erişime izin vermeden önce çekme belirli koşullara bakar. Bu koşullar sağlandığında, kullanıcı kimlik doğrulaması ve uygulamaya erişim izni.
documentationcenter: ''
services: active-directory
author: billmath
manager: daveba
editor: ''
ms.assetid: b92fa5a9-c04c-4692-b495-ff64d023792c
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/29/2019
ms.subservice: hybrid
ms.author: billmath
ms.custom: seohack1
ms.collection: M365-identity-device-management
ms.openlocfilehash: 39495e11e42853bf3cf9481475d970667c56223f
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64919097"
---
# <a name="define-a-hybrid-identity-adoption-strategy"></a>Karma kimlik benimseme stratejinizi tanımlayın
Bu görevde, karma kimlik çözümü içinde bahsedilen iş gereksinimlerini karşılamak için karma kimlik benimseme stratejinizi tanımlayın:

* [İş gereksinimlerini belirleme](plan-hybrid-identity-design-considerations-business-needs.md)
* [Dizin eşitleme gereksinimlerini belirleme](plan-hybrid-identity-design-considerations-directory-sync-requirements.md)
* [Çok faktörlü kimlik doğrulama gereksinimlerini belirleme](plan-hybrid-identity-design-considerations-multifactor-auth-requirements.md)

## <a name="define-business-needs-strategy"></a>İş gereksinimlerini stratejisini tanımlayın
Kuruluşların iş belirleme ilk görev adres olması gerekir.  Bu çok geniş olabilir ve dikkatli olmazsanız kapsam yayılmasını ortaya çıkabilir.  Başlangıçta, basit tutmak ancak barındırmak ve değişiklik gelecekte kolaylaştırmak için bir tasarım planı her zaman yükseltmeyi unutmayın.  Basit bir tasarım veya son derece karmaşık bir olmasından bağımsız olarak, Azure Active Directory, Office 365 ve Microsoft Online Services bulut destekleyen Microsoft Identity platformdur kullanan uygulamalar.

## <a name="define-an-integration-strategy"></a>Bir tümleştirme stratejisini tanımlayın
Microsoft bulut kimlikleri, eşitlenen kimlikler ve Federasyon kimlikleri olan üç ana tümleştirme senaryolarına sahiptir.  Bu tümleştirme stratejiler birini benimsenmesi planlamanız gerekir.  Seçtiğiniz strateji değişebilir ve en uygun maliyetli nedir ve kararların bir seçme içinde bulunabilir, ne tür bir kullanıcı deneyimi sunmak, mevcut bir altyapınız var istiyorsunuz.  

![Tümleştirme senaryoları](./media/plan-hybrid-identity-design-considerations/integration-scenarios.png)

Yukarıdaki şekilde tanımlanan tüm senaryolar şunlardır:

* **Bulut kimlikleri**: Bunlar yalnızca bulutta bulunan hesaplardır.  Özellikle Azure AD dizininizi Azure AD'ye söz konusu olduğunda, bulundukları.
* **Eşitlenen**: Bunlar, mevcut şirket içi kimlikleri ve bulut.  Azure AD Connect kullanarak, bu kullanıcılar da oluşturulur veya var olan Azure AD hesapları ile birleştirilmiş.  Kullanıcının parola karmasını parola karması olarak adlandırılan bulutta şirket içi ortamdan eşitlenir.  Bir uyarı bir kullanıcı şirket içi ortamda devre dışıysa, uygulamanın Azure AD'de gösterilecek hesap durumu üç saate kadar sürebilir kullanarak eşitlenmiş andır.  Bu nedeniyle eşitleme zaman aralığıdır.
* **Federasyon**: Bu kimlikleri mevcut hem şirket içi ve bulut.  Azure AD Connect kullanarak, bu kullanıcılar da oluşturulur veya var olan Azure AD hesapları ile birleştirilmiş.  

> [!NOTE]
> Eşitleme seçenekleri hakkında daha fazla bilgi için okuma [şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md).
> 
> 

Aşağıdaki tabloda, avantajları ve dezavantajları aşağıdaki stratejilerden birini belirlemede yardımcı olur:

| Stratejisi | Yararları | Dezavantajları |
| --- | --- | --- |
| **Bulut kimlikleri** |Küçük Kuruluşum için yönetilmesi daha kolay. <br> Şirket içi yüklemek için bir şey yok. Ek donanım gereklidir<br>Kullanıcının şirketten ayrılması durumunda kolayca devre dışı |Kullanıcıların buluttaki iş yüklerinizi erişirken oturum açmanız gerekir <br> Parolaları olabilir ya da Bulut ve şirket içi kimlikler için aynı olamaz |
| **Eşitlendi** |Şirket içinde hem şirket içi hem de bulut parola kimlik doğrulaması dizinleri <br>Küçük, Orta veya büyük kuruluşlar için yönetilmesi daha kolay <br>Kullanıcılar bazı kaynaklar için çoklu oturum açma (SSO) olabilir <br> Eşitleme için tercih edilen Microsoft yöntemi <br> Yönetilmesi daha kolay |Bazı müşterilerin belirli şirketin Polis nedeniyle bulut ile kendi dizinlerinizin eşitlenmesi isteksiz olabilir |
| **Federasyon** |Kullanıcılar, çoklu oturum açma (SSO) olabilir <br>Bir kullanıcı sonlandırılır veya bırakır, hesap hemen devre dışı ve erişimi iptal<br> Gelişmiş ile elde edemiyor senaryolarını destekler eşitlendi |Daha fazla adım ayarlamak ve yapılandırmak için <br> Daha yüksek bakım <br> Ek donanım STS altyapısı gerektirebilir. <br> Federasyon sunucusu yüklemek için ek donanım gerektirebilir. AD FS kullanılırsa, ek yazılım gereklidir <br> SSO için kapsamlı Kurulum gerektirir <br> Federasyon sunucusu kapalıysa bir hata noktası kritik, kullanıcıların kimliğini doğrulamak mümkün olmayacaktır |

### <a name="client-experience"></a>Müşteri deneyimi
Kullandığınız stratejisi, kullanıcı oturum açma deneyimini gösterecektir.  Aşağıdaki tablolarda, hangi kullanıcı olarak oturum açma deneyimlerini beklemelisiniz hakkında bilgi sağlar.  Tüm Federal Kimlik sağlayıcıları tüm senaryolarda SSO'yu destekler.

**Etki alanı ile birleşik ve özel ağ uygulamaları**:

|  | Eşitlenen bir kimlik | Federal Kimlik |
| --- | --- | --- |
| Web tarayıcıları |Form tabanlı kimlik doğrulaması |Çoklu oturum bazen kuruluş kimliği sağlamanız gereken açma |
| Outlook |Kimlik bilgisi istemi |Kimlik bilgisi istemi |
| Skype Kurumsal (Lync) |Kimlik bilgisi istemi |Çoklu oturum açma Lync için Exchange için kimlik bilgileri istenir. |
| OneDrive İş |Kimlik bilgisi istemi |Çoklu oturum açma |
| Office Pro Plus aboneliği |Kimlik bilgisi istemi |Çoklu oturum açma |

**Dış veya güvenilmeyen kaynaklardan**:

|  | Eşitlenen bir kimlik | Federal Kimlik |
| --- | --- | --- |
| Web tarayıcıları |Form tabanlı kimlik doğrulaması |Form tabanlı kimlik doğrulaması |
| Outlook, Skype Kurumsal (Lync), OneDrive iş, Office Aboneliği |Kimlik bilgisi istemi |Kimlik bilgisi istemi |
| Exchange ActiveSync |Kimlik bilgisi istemi |Çoklu oturum açma Lync için Exchange için kimlik bilgileri istenir. |
| Mobil uygulamalar |Kimlik bilgisi istemi |Kimlik bilgisi istemi |

Görev 1 bir Azure AD ile Federasyon sağlamak için kullanmak üzere bir üçüncü taraf IDP veya olan giderek olduğunu belirlediyseniz, aşağıdaki desteklenen yeteneklerini bilmeniz gerekir:

* SP-Lite profil için uyumlu olan herhangi bir SAML 2.0 sağlayıcı Azure AD kimlik destekleyebilir ve ilişkili uygulamalar
* OWA, SPO vb. için kimlik doğrulamayı kolaylaştıran pasif kimlik doğrulamasını destekler.
* Exchange Online istemciler, SAML 2.0 Gelişmiş istemci profili (ECP aracılığıyla) desteklenebilir

Ayrıca hangi özelliklerin kullanılabilir olmayacak nın farkında olmanız gerekir:

* WS-Güven/Federasyon desteği olmadan, tüm etkin istemciler Kes
  * Hiçbir Lync istemcisini, OneDrive istemcisi, Office aboneliği, Office 2016'dan önce Office Mobile anlamına
* Saf SAML 2.0 Idp'yi desteklemesi geçiş Office pasif kimlik doğrulama sağlar, ancak desteğini istemci tarafından istemci olarak olmaya devam edecektir

> [!NOTE]
> En güncel listesini makaleyi okuyun için [Azure AD Federasyonu uyumluluk listesi](how-to-connect-fed-compatibility.md).
> 
> 

## <a name="define-synchronization-strategy"></a>Eşitleme stratejisini tanımlayın
Bu görev ile eşitlemek için kullanılan araçları tanımlamak kuruluşun verileri Bulut ve hangi şirket topolojisi kullanmalısınız.  Çoğu kuruluş Active Directory'yi kullanmak için yukarıdaki soruları ele almak için Azure AD Connect kullanarak bilgi biraz ayrıntılı olarak sağlanır.  Active Directory olmayan ortamlar için bu strateji planlamanıza yardımcı olacak FIM 2010 R2 veya MIM 2016'yı kullanma hakkında bilgi yok.  Ancak, LDAP dizinleri, bu nedenle, zaman çizelgesinde bağlı olarak Azure AD Connect'ın gelecek sürümlerini destekler, bu bilgiler yardımcı olmak üzere mümkün olabilir.

### <a name="synchronization-tools"></a>Eşitleme araçları
Yıllar içinde birden çok eşitleme araçları vardı ve çeşitli senaryolar için kullanılan.  Şu anda Azure AD Connect aracı tüm desteklenen senaryolar için tercih ettiğiniz Git ' dir.  AAD eşitleme ve DirSync yine de geçici olarak ve hatta artık ortamınızda mevcut olabilir. 

> [!NOTE]
> En son her aracı için desteklenen özellikler hakkında bilgi edinin [dizin tümleştirme araçları karşılaştırması](plan-hybrid-identity-design-considerations-tools-comparison.md) makalesi.  
> 
> 

### <a name="supported-topologies"></a>Desteklenen topolojiler
Bir eşitleme stratejiyi tanımlarken, kullanılan topoloji belirlenmesi gerekir. Adımda belirlenen bilgilere bağlı olarak 2 hangi topolojiyi kullanmak için uygun olduğunu belirleyebilirsiniz. Tek orman, tek Azure AD topoloji yaygın olarak kullanılır ve tek bir Active Directory ormanı ve tek bir Azure AD örneğinde oluşur.  Bu senaryolardan Çoğunluk kullanılacak geçiyor ve beklenen topolojisi aşağıdaki çizimde gösterildiği gibi Azure AD Connect Express yüklemesi kullanarak andır.

![Desteklenen topolojiler](./media/plan-hybrid-identity-design-considerations/single-forest.png) tek orman senaryo olduğu birden çok ormanı daha küçük ve büyük ölçekli kuruluşlar için yaygın Şekil 5'te gösterildiği gibi.

> [!NOTE]
> Eşitleme şirket içi ve Azure AD Connect ile Azure AD topolojiler hakkında daha fazla bilgi için makaleyi okuyun [Azure AD Connect için topolojiler](plan-connect-topologies.md).
> 
> 

![çok ormanlı topolojisi](./media/plan-hybrid-identity-design-considerations/multi-forest.png) 

Çok ormanlı senaryosu

Bu durum, çok ormanlı tek ise Azure AD topoloji, aşağıdaki öğeleri doğruysa sayılacağı:

* Kullanıcılar tüm ormanlarda yalnızca 1 kimlik sahip – bu benzersiz şekilde tanımlayan kullanıcılar bölümünde daha ayrıntılı olarak açıklar.
* Kullanıcı kimliklerini bulunduğu ormana kimliğini doğrular.
* UPN ve kaynak bağlantısı (sabit kimlik) bu ormandan gelir
* Tüm ormanlar Azure AD Connect tarafından erişilebilir: Bu etki alanına katılmış ve bu bunu kolaylaştırır, bir DMZ'ye yerleştirilebilir gerekmez anlamına gelir.
* Kullanıcılar tek bir posta kutusuna sahip
* Bir kullanıcının posta kutusuna barındıran ormanda Exchange Genel adres listesi (GAL) içinde görünür olan öznitelikler için en iyi veri kalitesi vardır
* Kullanıcı posta kutusu varsa, daha sonra herhangi bir orman bu değerleri katkıda bulunmak için kullanılabilir
* Bağlı bir posta kutusu varsa, ardından da başka bir hesap var. oturum açmak için kullanılan farklı bir ormanda.

> [!NOTE]
> Hem şirket içindeki ve buluttaki mevcut nesneleri "benzersiz bir tanımlayıcıya bağlıdır". Dizin eşitleme bağlamında bu benzersiz tanımlayıcı SourceAnchor adlandırılır. Çoklu oturum açma bağlamında, bu Immutableıd adlandırılır. [İçin Azure AD Connect tasarım kavramları](plan-connect-design-concepts.md#sourceanchor) SourceAnchor kullanımına ilişkin daha fazla konuları için.
> 
> 

Yukarıdaki doğru değildir ve birden fazla etkin hesap ya da birden fazla posta kutusu varsa Azure AD Connect, bir tane seçin ve diğer yoksay.  Posta kutularını ancak başka bir hesap bağladıysanız bu hesapları Azure AD'ye aktarılmaz ve bu kullanıcı herhangi bir gruba üye olmamasını.  Bu, nasıl DirSync ile daha önce olduğu ve bu çok ormanlı senaryolar kasıtlı daha iyi destek, öğesinden farklıdır. Çok ormanlı senaryo aşağıdaki çizimde gösterilmektedir.

![Birden çok Azure AD kiracıları](./media/plan-hybrid-identity-design-considerations/multiforest-multipleAzureAD.png) 

**Çok ormanlı birden çok Azure AD senaryosu**

Bir kuruluş için Azure AD'de yalnızca tek bir dizin sağlamak için önerilir, ancak bu desteklenen bir Azure AD Connect eşitleme sunucusu ile Azure AD dizini arasında 1:1 ilişki tutulur.  Her Azure AD örneği için bir Azure AD Connect yüklemesi gerekir.  Ayrıca, Azure AD, tasarımı gereği yalıtılır ve kullanıcıların bir Azure AD örneğinde kullanıcıların başka bir örnek görmek mümkün olmayacaktır.

Bu, olası ve aşağıdaki çizimde gösterildiği gibi birden çok Azure AD dizini için şirket içi Active Directory örneğini bir bağlanmak için desteklenen olur:

![tek orman filtreleme](./media/plan-hybrid-identity-design-considerations/single-forest-flitering.png) 

**Tek ormanlı filtreleme senaryosu**

Bunu yapmak için aşağıdakilerin doğru olması gerekir:

* Azure AD Connect eşitleme sunucusu birbirini dışlayan nesne sahip oldukları her şekilde filtrelemek için yapılandırılması gerekir.  Bu özel etki alanı ya da OU her sunucuya kapsamı tarafından yapılır.
* Bir DNS etki alanı yalnızca tek bir kayıtlı Azure AD dizini böylece UPN kullanıcıların şirket içi AD ayrı ad alanları kullanmanız gerekir
* Bir Azure AD örneğinde kullanıcı yalnızca kendi örneği kullanıcıları görmek mümkün olacaktır.  Bunlar diğer örneklerin kullanıcıları görmek mümkün olmayacaktır
* Bir Azure AD dizini ile şirket içi Exchange karma etkinleştirebilirsiniz yalnızca AD
* Karşılıklı olmama geri yazma için de geçerlidir.  Bu, bu tek şirket içi yapılandırma varsayar olduğundan bu topoloji ile desteklenmeyen bazı geri yazma özellikleri sağlar.  Buna aşağıdakiler dahildir:
  * Grup geri yazma özelliğiyle varsayılan yapılandırma
  * Cihaz geri yazma

Aşağıdakiler desteklenmez ve bir uygulama seçilmelidir değil:

* Birbirini dışlayan nesne kümesini eşitlemek için yapılandırılmış olsa bile aynı Azure AD dizinine bağlanan birden çok Azure AD Connect eşitleme sunucusu için desteklenmiyor
* Aynı kullanıcıya birden çok Azure AD dizini eşitlemek için desteklenmiyor. 
* Ayrıca, bir yapılandırma başka bir Azure AD dizini kişiler olarak görünmesi için bir Azure AD kullanıcıların yapmalarına değişikliği yapmak için desteklenmiyor. 
* Bağlanmak için birden çok Azure AD dizini için Azure AD Connect eşitleme değiştirmek için desteklenmiyor.
* Azure AD dizin yalıtılmış tasarım gereği ' dir. Dizinler arasında ortak ve birleşik bir GAL oluşturma girişimi, başka bir Azure AD dizininden verileri okumak için Azure AD Connect eşitleme yapılandırmasını değiştirmek için desteklenmiyor. Kişilere olarak kullanıcıları dışarı aktarmak için de desteklenmeyen başka bir Azure AD Connect eşitleme kullanarak AD şirket.

> [!NOTE]
> Kuruluşunuz, ağınızdaki bilgisayarların Internet'e bağlanmasını kısıtlıyorsa, uç noktaları (FQDN, IPv4 ve IPv6 adres aralıklarını) Bu makalede listelenmektedir dahil listeler ve Internet Explorer Güvenilen siteler bölgesine istemcisinin, Gidene izin ver Bilgisayarlarınızı emin olmak için bilgisayarlara başarıyla Office 365 kullanabilirsiniz. Daha fazla bilgi için okuma [Office 365 URL'leri ve IP adresi aralıkları](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US).
> 
> 

## <a name="define-multi-factor-authentication-strategy"></a>Çok faktörlü kimlik doğrulaması stratejisini tanımlayın
Bu görev ile kullanmak için çok faktörlü kimlik doğrulaması stratejisini tanımlayacaksınız.  Azure multi-Factor Authentication iki farklı sürümde sunulur.  Bir bulut tabanlı ve diğer şirket içi Azure MFA sunucusu kullanan tabanlı.  Değerlendirmeye göre yukarıda stratejiniz için doğru olanı çözümüdür belirleyebilirsiniz.  En iyi tasarım seçeneği, şirketinizin güvenlik gereksinimleri karşıladığı belirlemek için aşağıdaki tabloyu kullanın:

Çok faktörlü tasarım seçenekleri:

| Güvenli hale getirmek için varlık | Bulutta MFA | Şirket içi MFA |
| --- | --- | --- |
| Microsoft uygulamaları |evet |evet |
| Uygulama galerisinde SaaS uygulamaları |evet |evet |
| Azure AD Uygulaması Proxy üzerinden yayımlanan IIS uygulamaları |evet |evet |
| Azure AD uygulaması Proxy üzerinden yayımlanmayan IIS uygulamaları |hayır |evet |
| VPN, RDG olarak uzaktan erişim |hayır |evet |

Stratejiniz için bir çözüm üzerinde kapatılmış, ancak yine de, kullanıcılarınızın bulunduğu yere üzerinde Yukarıdaki değerlendirme kullanmanız gerekir.  Bu çözümü değiştirmek neden olabilir.  Bu belirlemede yardımcı olması için aşağıdaki tabloyu kullanın:

| Kullanıcı konumu | Tercih edilen tasarım seçeneği |
| --- | --- |
| Azure Active Directory |Multi-FactorAuthentication bulutta |
| AD FS ile federasyon kullanana Azure AD ve şirket içi AD |Her ikisi de |
| Azure AD ve şirket içi Azure AD kullanarak AD Connect parola eşitleme yok |Her ikisi de |
| Azure AD ve parola eşitleme ile Azure AD Connect kullanarak şirket içi |Her ikisi de |
| Şirket içi AD |Multi-Factor Authentication Sunucusu |

> [!NOTE]
> Ayrıca, seçtiğiniz çok faktörlü kimlik doğrulaması tasarım seçeneği tasarımınız için gerekli olan özellikleri desteklediğini de emin olmalısınız.  Daha fazla bilgi için okuma [multi-Factor güvenlik çözümünü seçtiğiniz](../authentication/concept-mfa-whichversion.md#what-am-i-trying-to-secure).
> 
> 

## <a name="multi-factor-auth-provider"></a>Multi-Factor Auth sağlayıcısı
Çok faktörlü kimlik doğrulaması, Azure Active Directory kiracısı olan küresel Yöneticiler için varsayılan olarak kullanılabilir. Tüm kullanıcılarınızın çok faktörlü kimlik doğrulaması genişletmek ve/veya genel Yöneticiler için Yönetim Portalı, özel karşılamalar ve raporları gibi özellikler avantajı mümkün olmasını istediğiniz istiyorsanız, ancak daha sonra satın alma ve yapılandırmanız gerekir Çok faktörlü kimlik doğrulama sağlayıcısı.

> [!NOTE]
> Ayrıca, seçtiğiniz çok faktörlü kimlik doğrulaması tasarım seçeneği tasarımınız için gerekli olan özellikleri desteklediğini de emin olmalısınız. 
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
[Veri koruma gereksinimlerini belirleme](plan-hybrid-identity-design-considerations-dataprotection-requirements.md)

## <a name="see-also"></a>Ayrıca bkz.
[Tasarım konularına genel bakış](plan-hybrid-identity-design-considerations-overview.md)

