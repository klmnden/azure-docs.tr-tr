---
title: Karma kimlik tasarımı - benimseme stratejinizi Azure | Microsoft Docs
description: Koşullu erişim denetimi ile Azure Active Directory kullanıcı doğrulanırken ve uygulamaya erişimine izin vermeden önce çekme belirli koşullar denetler. Bu koşullar sağlandığında, kullanıcı kimlik doğrulaması ve uygulamaya erişim izni.
documentationcenter: ''
services: active-directory
author: billmath
manager: mtillman
editor: ''
ms.assetid: b92fa5a9-c04c-4692-b495-ff64d023792c
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.custom: seohack1
ms.openlocfilehash: 290c41e62080edcd9a2fad1b5045bac4328cc4cd
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="define-a-hybrid-identity-adoption-strategy"></a>Bir karma kimlik benimseme stratejinizi tanımlayın
Bu görevde, ele alınan iş gereksinimlerini karşılamak karma kimlik çözümünü karma kimlik benimseme stratejinizi tanımlayın:

* [İş gereksinimlerini belirleme](active-directory-hybrid-identity-design-considerations-business-needs.md)
* [Dizin eşitleme gereksinimlerini belirleyin](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)
* [Çok faktörlü kimlik doğrulama gereksinimlerini belirleyin](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)

## <a name="define-business-needs-strategy"></a>İş gereksinimlerini stratejisini tanımlayın
Kuruluşlar iş belirleme ilk görev adres olması gerekir.  Bu çok geniş olabilir ve kapsam yayılmasını dikkatli değilse ortaya çıkabilir.  Başlangıçta, basit tutmak ancak her zaman uygun hale getirmek ve değişiklik gelecekte kolaylaştırmak için bir tasarımı planlayın unutmayın.  Basit bir tasarım veya çok karmaşık bir olup bağımsız olarak, Azure Active Directory Office 365, Microsoft Online Services ve bulut destekleyen Microsoft Identity platformudur kullanan uygulamalar.

## <a name="define-an-integration-strategy"></a>Bir tümleştirme stratejisini tanımlayın
Microsoft, bulut kimlikleri, eşitlenen kimlikler ve Federasyon kimlikleri olan üç ana tümleştirme senaryolarına sahiptir.  Bu tümleştirme stratejiler birini benimsenmesi planlamanız gerekir.  Farklı olabilir ve bir seçerken kararları içerebilir, ne tür bir kullanıcı deneyimi sağlamak için bazı varolan altyapısının yerinde'zaten var ve en düşük maliyetli nedir istediğiniz seçtiğiniz stratejisi.  

![](./media/hybrid-id-design-considerations/integration-scenarios.png)

Yukarıdaki şekilde tanımlanan tüm senaryolar şunlardır:

* **Bulut kimlikleri**: Bunlar yalnızca bulutta mevcut hesaplardır.  Azure AD söz konusu olduğunda, özellikle Azure AD dizininizi bulunması.
* **Eşitlenen**: şirket içi mevcut kimlikleri bunlar içinde ve bulutta.  Azure AD Connect'i kullanarak, bu kullanıcılar oluşturulan ya da var olan Azure AD hesapları ile birleştirilmiş.  Kullanıcının parola karmasını şirket içi ortamından bir parola karması olarak adlandırılan bulutta eşitlenir.  Bir uyarı bir kullanıcı şirket içi ortamda devre dışıysa, bu Azure AD içinde gösterilmesi bu hesabı durumu üç saat sürebilir kullanılarak eşitlenene durumdur.  Bu nedeniyle eşitleme zaman aralığıdır.
* **Federasyon**: Bu kimlikleri hem şirket içinde bulunur ve bulutta.  Azure AD Connect'i kullanarak, bu kullanıcılar oluşturulan ya da var olan Azure AD hesapları ile birleştirilmiş.  

> [!NOTE]
> Eşitleme seçenekleri hakkında daha fazla bilgi için okuma [şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](connect/active-directory-aadconnect.md).
> 
> 

Aşağıdaki tabloda, avantajları ve dezavantajları aşağıdaki stratejilerden birini belirlemede yardımcı olur:

| Stratejisi | Avantajları | Olumsuz yönleri |
| --- | --- | --- |
| **Bulut kimlikleri** |Küçük bir kuruluş için yönetilmesi daha kolay. <br> Hiçbir şey gerekli üzerinde-şirket içi-No ek donanım yüklemek için<br>Kullanıcının şirketten ayrılması durumunda kolayca devre dışı |Kullanıcıların iş yüklerini bulutta erişirken oturum açmak gerekir <br> Parolalar olabilir veya Bulut ve şirket içi kimlikleri için aynı olamaz |
| **Eşitlendi** |Şirket içi parola kimlik doğrulaması şirket içi ve bulut dizinleri <br>Küçük, Orta veya büyük kuruluşlar için yönetilmesi daha kolay <br>Kullanıcıların bazı kaynaklar için çoklu oturum açma (SSO) olabilir <br> Eşitleme için tercih edilen Microsoft yöntemi <br> Yönetilmesi daha kolay |Bazı müşterilerin belirli şirketin Polis son bulut dizinlerine eşitlemek etme konusunda isteksiz olabilirsiniz |
| **Federasyon** |Kullanıcılar, çoklu oturum açma (SSO) olabilir <br>Bir kullanıcı sonlandırılır veya bırakır, hesap hemen devre dışı ve erişimi iptal<br> Gelişmiş ile elde edemiyor senaryoları destekler eşitlendi |Daha fazla adımları ayarlama ve yapılandırma <br> Daha yüksek bakım <br> STS altyapısı için ek donanım gerektirebilir <br> Federasyon sunucusunu yüklemek için ek donanım gerektirebilir. AD FS kullanılırsa, ek yazılım gereklidir <br> SSO için kapsamlı Kurulum gerektirir <br> Federasyon sunucusu kapalı olduğunda hata noktası kritik, kullanıcıların kimliğini doğrulamak doğrulayamazsınız. |

### <a name="client-experience"></a>Müşteri deneyimi
Kullandığınız stratejisi oturum açma kullanıcı deneyimini benimsendiği belirler.  Aşağıdaki tablolarda, hangi kullanıcıların olması için oturum açma deneyimlerini beklemelisiniz hakkında bilgi sağlar.  Tüm Federasyon kimlik sağlayıcıları tüm senaryolarda SSO'yu destekler.

**Etki alanı ile birleşik ve özel ağ uygulamaları**:

|  | Eşitlenen kimliği | Federal Kimlik |
| --- | --- | --- |
| Web tarayıcıları |Form tabanlı kimlik doğrulama |Çoklu oturum açma bazen kuruluş kimliği sağlamak için gerekli özelliğini, |
| Outlook |Kimlik bilgisi istemi |Kimlik bilgisi istemi |
| Skype Kurumsal (Lync) |Kimlik bilgisi istemi |Çoklu oturum açma Lync için kimlik bilgileri değişimi için istenir. |
| SkyDrive Pro |Kimlik bilgisi istemi |Çoklu oturum açma |
| Office aboneliği artı Pro |Kimlik bilgisi istemi |Çoklu oturum açma |

**Dış veya güvenilmeyen kaynaklardan**:

|  | Eşitlenen kimliği | Federal Kimlik |
| --- | --- | --- |
| Web tarayıcıları |Form tabanlı kimlik doğrulama |Form tabanlı kimlik doğrulama |
| Outlook, Skype Kurumsal (Lync) Skydrive Pro, Office Aboneliği |Kimlik bilgisi istemi |Kimlik bilgisi istemi |
| Exchange ActiveSync |Kimlik bilgisi istemi |Çoklu oturum açma Lync için kimlik bilgileri değişimi için istenir. |
| Mobil uygulamalar |Kimlik bilgisi istemi |Kimlik bilgisi istemi |

Görev 1 bir Azure AD ile Federasyon sağlamak için kullanmak üzere bir üçüncü taraf IDP veya olan giderek olduğunu belirlediyseniz, aşağıdaki desteklenen yeteneklerini bilmeniz gerekir:

* SP Lite profilini uyumlu herhangi bir SAML 2.0 sağlayıcısına Azure ad kimlik doğrulama destekleyebilir ve ilişkili uygulamalar
* OWA, SPO vb. için kimlik doğrulamayı kolaylaştıran pasif kimlik doğrulamasını destekler.
* Exchange Online istemcileri SAML 2.0 Gelişmiş istemci profili (ECP aracılığıyla) desteklenebilir

Ayrıca hangi özellikleri kullanılamaz olarak haberdar olmanız gerekir:

* WS-Güven/Federasyon desteği olmadan, tüm etkin istemciler bölün
  * Hiçbir Lync istemcisi, OneDrive istemcisi, Office aboneliği, Office 2016 önce Office Mobile anlamına
* Office geçiş pasif kimlik doğrulama için bunları saf SAML 2.0 IdPs desteklemek verir, ancak bir istemci istemci temelinde desteği devam edebilir

> [!NOTE]
> Makaleyi en güncel listesi okumak için https://aka.ms/ssoproviders.
> 
> 

## <a name="define-synchronization-strategy"></a>Eşitleme stratejisini tanımlayın
Eşitlemek için kullanılan araçlar tanımlamak bu görevde kuruluşun verileri bulutta ve ne için şirket içi topolojisi kullanması gerekir.  Çoğu kuruluş Active Directory'yi kullanmak için yukarıdaki soruları için Azure AD Connect'i kullanarak bilgi biraz ayrıntılı olarak sağlanır.  Active Directory olmayan ortamlar için bu stratejiyi planlamanıza yardımcı olması için FIM 2010 R2 veya MIM 2016 kullanma hakkında bilgi yoktur.  Ancak, Azure AD Connect gelecek sürümlerde LDAP dizinleri, bu nedenle, zaman çizelgesinde bağlı olarak destekler, bu bilgiler yardımcı olmak mümkün olabilir.

### <a name="synchronization-tools"></a>Eşitleme araçları
Yıllar içinde birkaç eşitleme araçları sahip ve mevcut olan çeşitli senaryoları için kullanılır.  Şu anda Azure AD Connect aracıyla ilgili tüm desteklenen senaryolar için Git değil.  AAD eşitleme ve DirSync yine de geçici ve hatta şimdi ortamınızda mevcut olabilir. 

> [!NOTE]
> En son her aracı için desteklenen özellikler hakkında bilgi okuma [dizin tümleştirme araçları karşılaştırması](active-directory-hybrid-identity-design-considerations-tools-comparison.md) makalesi.  
> 
> 

### <a name="supported-topologies"></a>Desteklenen topolojiler
Bir eşitleme stratejinizi tanımlarken kullanılır topoloji belirlenmesi gerekir. Adımda belirlendi bilgileri bağlı olarak, 2 hangi topolojisi kullanmak için uygun olduğunu belirleyebilirsiniz. Tek tek orman Azure AD topoloji yaygın olarak kullanılır ve tek bir Active Directory ormanı ve Azure AD tek bir örneğini oluşur.  Bu Çoğunluk senaryolarda kullanılacak giderek ve beklenen topolojisi aşağıdaki çizimde gösterildiği gibi Azure AD Connect Express yüklemesi kullanırken.

![](./media/hybrid-id-design-considerations/single-forest.png) Tek orman senaryo Şekil 5'te gösterildiği gibi birden çok orman bile küçük ve büyük kuruluşlar için yaygın bir sorundur.

> [!NOTE]
> Eşitleme şirket içi ve Azure AD Connect ile Azure AD topolojileri hakkında daha fazla bilgi için makaleyi okuyun [Azure AD Connect için topolojiler](connect/active-directory-aadconnect-topologies.md).
> 
> 

![](./media/hybrid-id-design-considerations/multi-forest.png) 

Çoklu orman senaryosu

Aşağıdaki öğeler doğruysa, bu durum sonra çok-forest-tek Azure AD topoloji değerlendirilmesi gerekiyorsa:

* Tüm ormanlarda yalnızca 1 kimlik kullanıcınız – aşağıdaki benzersiz şekilde tanımlayan kullanıcılar bölümünde bu daha ayrıntılı olarak açıklanmaktadır.
* Kullanıcı kimliklerini bulunduğu ormana kimliğini doğrular.
* UPN ve kaynak bağlantısı (değişmez kimliği) bu ormandan gelir
* Tüm ormanlarda Azure AD Connect tarafından erişilebilir – bu etki alanına katılmış ve bu bu kolaylaştırır, çevre ağınızda yerleştirilebilir olması gerekmez anlamına gelir.
* Kullanıcıların yalnızca bir posta kutusuna sahip
* Bir kullanıcının posta barındıran ormanda Exchange Genel adres listesi (GAL) içinde görünür öznitelikler için en iyi veri kalitesini vardır
* Kullanıcıya hiçbir posta kutusu varsa, daha sonra herhangi bir orman bu değerleri katkıda bulunmak için kullanılabilir
* Bağlı bir posta kutusu varsa, ardından var. Ayrıca başka bir hesapla oturum açmak için kullandığınız farklı bir ormanda

> [!NOTE]
> Her iki şirket içi ve bulut mevcut nesneleri "benzersiz bir tanımlayıcıya bağlı". Dizin eşitleme bağlamında bu benzersiz tanımlayıcı SourceAnchor adlandırılır. Çoklu oturum açma bağlamında bu İmmutableıd adlandırılır. [İçin Azure AD Connect tasarım kavramları](connect/active-directory-aadconnect-design-concepts.md#sourceanchor) SourceAnchor kullanılmasıyla ilgili daha fazla hususlara için.
> 
> 

Yukarıdaki doğru değildir ve birden fazla etkin hesabı ya da birden fazla posta kutusu varsa, Azure AD Connect birini seçin ve diğer yoksay.  Posta kutularını ancak başka bir hesap bağladıysanız, bu hesapları Azure AD'ye aktarılmaz ve kullanıcının herhangi bir grup üyesi olmaz.  Bu nasıl, DirSync ile geçmişte oluştu ve bu çok ormanlı senaryolar için daha iyi kasıtlı desteğidir farklıdır. Çoklu orman senaryosu aşağıdaki çizimde gösterilmektedir.

![](./media/hybrid-id-design-considerations/multiforest-multipleAzureAD.png) 

**Çoklu orman birden çok Azure AD senaryosu**

Bir kuruluş için Azure AD içinde yalnızca tek bir dizin olması önerilir ancak, desteklenen bir Azure AD Connect eşitleme sunucusu ve Azure AD dizini arasında 1:1 ilişki tutulur.  Azure AD her örneği için Azure AD Connect yüklemesi gerekir.  Ayrıca, Azure AD tasarım gereği yalıtılır ve kullanıcıların Azure ad bir örneği başka bir örneği kullanıcılar görmeye olmaz.

Olası ve Active Directory bir şirket içi örneğini aşağıdaki şekilde gösterildiği gibi birden çok Azure AD dizinlerinden bağlanmak için desteklenen şöyledir:

![](./media/hybrid-id-design-considerations/single-forest-flitering.png) 

**Tek orman filtreleme senaryosu**

Bunu aşağıdakileri yapmak için true olması gerekir:

* Azure AD Connect eşitleme sunucusu birbirini dışlayan bir nesneler kümesini sahip oldukları her şekilde filtreleme için yapılandırılmış olması gerekir.  Bu örneğin, belirli bir etki alanı ya da OU'yu her sunucuya kapsamı tarafından yapılır.
* Bir DNS etki alanı yalnızca tek bir kaydedilebilir Azure AD dizini böylece UPN'ler kullanıcıların şirket içi AD ayrı ad alanları kullanmanız gerekir
* Bir Azure AD örneğini kullanıcılar yalnızca kendi örneği kullanıcılardan görmeye olacaktır.  Diğer durumlarda kullanıcıları görmek seçebilecekler değil
* Azure AD dizinlerinden biri şirket içi Exchange karma etkinleştirebilirsiniz yalnızca AD
* Karşılıklı exclusivity geri yazma için de geçerlidir.  Bu, bu tek şirket içi yapılandırma varsayın beri bu topolojisi ile desteklenmeyen bazı geri yazma özellikleri sağlar.  Buna aşağıdakiler dahildir:
  * Grup geri yazma varsayılan yapılandırmaya sahip
  * Cihaz geri yazma

Aşağıdakiler desteklenmez ve bir uygulama seçilmelidir değil:

* Birbirini dışlayan nesne kümesini eşitlemek için yapılandırılmış olsa bile aynı Azure AD dizinine bağlanma birden çok Azure AD Connect eşitleme sunucusu için desteklenmiyor
* Aynı kullanıcıya birden çok Azure AD dizinlerinden eşitlemek için desteklenmiyor. 
* Ayrıca, bir yapılandırma başka bir Azure AD dizini kişiler olarak görünmesi için bir Azure AD kullanıcıların açmak için değişikliği yapmak için desteklenmiyor. 
* Birden çok Azure AD dizinlerinden bağlanmak için Azure AD Connect eşitleme değiştirmek için desteklenmiyor.
* Azure AD dizinleri yalıtılmış tasarım gereği ' dir. Dizinler arasındaki ortak ve birleştirilmiş bir GAL oluşturma girişimi başka bir Azure AD dizini olan verileri okumak için Azure AD Connect eşitleme yapılandırmasını değiştirmek için desteklenmiyor. Kullanıcıları, kişileri dışarı aktarmak için de desteklenmeyen başka bir Azure AD Connect eşitleme kullanarak AD şirket içi.

> [!NOTE]
> Kuruluşunuzun Internet'e bağlanma ağınızdaki bilgisayarlara kısıtladığında, bu makalede (FQDN'ler, IPv4 ve IPv6 adres aralıklarını) uç noktaları listeler de içermelidir, giden listeler ve Internet Explorer Güvenilen siteler bölgesine istemci bilgisayarlarınızı başarıyla Office 365 kullandığınızdan emin olun bilgisayarların izin verin. Daha fazla bilgi için okuma [Office 365 URL'leri ve IP adresi aralıkları](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US).
> 
> 

## <a name="define-multi-factor-authentication-strategy"></a>Çok faktörlü kimlik doğrulaması stratejisini tanımlayın
Bu görevde kullanmak için çok faktörlü kimlik doğrulaması stratejisini tanımlayacaksınız.  Azure multi-Factor Authentication iki farklı sürümü bulunur.  Bir bulut tabanlı ve diğer şirket içi tabanlı Azure MFA sunucusu kullanarak.  Değerlendirmeye dayanarak, stratejinizi için doğru bir çözümdür belirleyebilirsiniz.  Tasarım seçeneği, şirketinizin güvenlik gereksinimi karşılayan belirlemek için aşağıdaki tabloyu kullanın:

Çok faktörlü tasarım seçenekleri:

| Güvenli hale getirmek için varlık | Bulutta MFA | Şirket içi MFA |
| --- | --- | --- |
| Microsoft uygulamaları |evet |evet |
| Uygulama galerisinde SaaS uygulamaları |evet |evet |
| Azure AD Uygulaması Proxy üzerinden yayımlanan IIS uygulamaları |evet |evet |
| Azure AD uygulaması Proxy üzerinden yayımlanmayan IIS uygulamaları |hayır |evet |
| Uzaktan erişim VPN, RDG olarak |hayır |evet |

Stratejinizi için bir çözüm üzerinde kapatılan, ancak hala kullanıcılarınızın bulunduğu üzerinde üstten değerlendirme kullanmanız gerekir.  Bu çözüm değiştirmek neden olabilir.  Bu belirleme yardımcı olması için aşağıdaki tabloyu kullanın:

| Kullanıcı konumu | Tercih edilen tasarım seçeneği |
| --- | --- |
| Azure Active Directory |Multi-FactorAuthentication bulutta |
| AD FS ile federasyon kullanana Azure AD ve şirket içi AD |Her İkisi |
| Azure AD ve şirket içi AD kullanarak Azure AD Connect parola eşitleme yok |Her İkisi |
| Azure AD ve parola eşitleme ile Azure AD Connect kullanarak şirket içi |Her İkisi |
| Şirket içi AD |Multi-Factor Authentication Sunucusu |

> [!NOTE]
> Ayrıca, seçtiğiniz çok faktörlü kimlik doğrulaması tasarım seçeneği tasarımınız için gerekli olan özellikleri desteklediğini de emin olmalısınız.  Daha fazla bilgi için okuma [multi-Factor güvenlik çözümünü seçtiğiniz](authentication/concept-mfa-whichversion.md#what-am-i-trying-to-secure).
> 
> 

## <a name="multi-factor-auth-provider"></a>Multi-Factor Auth sağlayıcısı
Çok faktörlü kimlik doğrulaması varsayılan olarak, bir Azure Active Directory kiracısı olan küresel yöneticiler tarafından kullanılabilir. Ancak, çok faktörlü kimlik doğrulaması tüm kullanıcılarınızın genişletmek ve/veya, genel Yöneticiler için Yönetim Portalı, özel Karşılama ve raporlar gibi avantajı özellikleri yararlanamazsınız istediğiniz isterseniz, daha sonra satın ve çok faktörlü kimlik doğrulama sağlayıcısını yapılandırmanız gerekir.

> [!NOTE]
> Ayrıca, seçtiğiniz çok faktörlü kimlik doğrulaması tasarım seçeneği tasarımınız için gerekli olan özellikleri desteklediğini de emin olmalısınız. 
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
[Veri koruma gereksinimlerini belirleme](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md)

## <a name="see-also"></a>Ayrıca bkz.
[Tasarım konularına genel bakış](active-directory-hybrid-identity-design-considerations-overview.md)

