---
title: Azure kimlik ve erişim en iyi güvenlik yöntemleri | Microsoft Docs
description: Bu makalede bir dizi kimlik yönetimi için en iyi yöntemler ve erişim denetimi Azure özellikleri kullanılarak.
services: security
documentationcenter: na
author: barclayn
manager: barbkess
editor: TomSh
ms.assetid: 07d8e8a8-47e8-447c-9c06-3a88d2713bc1
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/17/2018
ms.author: barclayn
ms.openlocfilehash: f872c61ad0597d2307cd244668fdfc258f7a45cb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60611247"
---
# <a name="azure-identity-management-and-access-control-security-best-practices"></a>Azure kimlik yönetimi ve erişim en iyi güvenlik denetimi

Birçok yeni bir sınır katman bu role geleneksel ağ merkezli perspektifinden ele güvenlik, kimlik göz önünde bulundurun. Bu birincil pivot evrimi güvenlik dikkat ve yatırımlarınızı ağ duvarlar hale giderek porous ve bu çevre savunması kez olarak etkin olamaz olgu gelen için önce açılımı olan [KCG ](https://aka.ms/byodcg) cihazlar ve bulut uygulamaları.

Bu makalede, Azure kimlik yönetimi ve erişim denetimi en iyi güvenlik uygulamaları koleksiyonu ele alır. Bu en iyi uygulamaları ile deneyimimizi türetilmiştir [Azure AD'ye](../active-directory/fundamentals/active-directory-whatis.md) ve müşteri deneyimleri bulunun.

En iyi her uygulama için biz açıklar:

* En iyi nedir
* Bu en iyi etkinleştirmek istediğiniz neden
* En iyi etkinleştirme başarısız olursa ne sonuç olabilir
* En iyi olası alternatifler
* Nasıl en iyi etkinleştirmek bilgi edinebilirsiniz

Bu makale, bu Azure kimlik yönetimi ve erişim denetimi güvenliği zaman oldukları gibi en iyi yöntemler makalesi fikir birliğine varılmış fikrim ve Azure platformu özellikleri ve özellik kümeleri dayalı olarak yazılmıştır. Fikirlerini ve teknolojileri zamanla değişir ve bu makalede, bu değişiklikleri yansıtacak şekilde düzenli olarak güncelleştirilir.

Bu makalede ele alınan azure kimlik yönetimi ve erişim denetimi güvenlik en iyi uygulamalar şunlardır:

* Kimlik birincil güvenlik çevresi olarak değerlendir
* Kimlik yönetimini merkezden gerçekleştirin
* Çoklu oturum açmayı etkinleştir
* Koşullu erişimi etkinleştirmek
* Parola yönetimini etkinleştirme
* Kullanıcılar için çok faktörlü doğrulama zorla
* Rol tabanlı erişim denetimi kullanma
* Ayrıcalıklı hesapların açığa çıkarılması
* Kaynakların yer aldığı denetim konumları

## <a name="treat-identity-as-the-primary-security-perimeter"></a>Kimlik birincil güvenlik çevresi olarak değerlendir

Birçok kimlik birincil güvenlik çevresi olarak göz önünde bulundurun. Bu, gelen ağ güvenlik odaklı geleneksel bir kaydırmadır. Ağ duvarlar tutmak alma daha porous ve bu çevre savunması açılımı önce olduğu gibi etkin olamaz [KCG](https://aka.ms/byodcg) cihazlar ve bulut uygulamaları.
[Azure Active Directory (Azure AD)](../active-directory/active-directory-whatis.md) Azure kimlik ve erişim yönetimi çözümüdür. Azure AD bir çok kiracılı, bulut tabanlı dizin ve Kimlik Yönetimi Microsoft hizmetidir. Bu, temel Dizin Hizmetleri, uygulama erişim yönetimi ve kimlik korumasını tek bir çözüm birleştirir.

Aşağıdaki bölümlerde, Azure AD kullanarak kimlik ve erişim güvenliği için en iyi yöntemler listelenmiştir.

## <a name="centralize-identity-management"></a>Kimlik yönetimini merkezden gerçekleştirin

İçinde bir [karma kimlik](https://resources.office.com/ww-landing-M365E-EMS-IDAM-Hybrid-Identity-WhitePaper.html?) senaryo, şirket içi tümleştirme ve bulut dizinleri öneririz. Tümleştirme hesaplarını bir hesap oluşturulduğu bağımsız olarak, tek tek konumdan yönetmek üzere BT ekibinize sağlar. Tümleştirme, ayrıca, kullanıcılarınızın hem bulut hem de şirket içi kaynaklara erişmek için bir ortak kimlik sağlayarak daha üretken olmanıza yardımcı olur.


**En iyi yöntem**: Şirket içi dizinlerinizi Azure AD ile tümleştirme.  
**Ayrıntı**: Kullanım [Azure AD Connect](../active-directory/connect/active-directory-aadconnect.md) şirket içi dizininizi bulut dizininizle eşitlenecek.

**En iyi yöntem**: Parola Karması eşitlemeyi kapatın.  
**Ayrıntı**: Parola Karması eşitleme, karma bir bulut tabanlı Azure şirket içi Active Directory örneğinden kullanıcı parola karmalarının eşitlemek için kullanılan bir özelliktir AD örneği.

Active Directory Federasyon Hizmetleri (AD FS) veya diğer kimlik sağlayıcıları ile Federasyon kullanmaya karar olsa bile, şirket içi sunucularınızı geçici olarak kullanılamıyor veya başarısız durumda isteğe bağlı olarak parola karması eşitleme yedek olarak ayarlayabilirsiniz. Bu hizmete kendi şirket içi Active Directory örneğine oturum açmak için kullandıkları aynı parolayı kullanarak oturum açmalarını sağlar. Ayrıca bu parola karmalarının tehlikeye için bir kullanıcı kendi aynı e-posta adresi ve parola Azure AD'ye bağlı diğer sistemlerde kullanıldıysa bilinen parola ile karşılaştırarak tehlikeye atılmış kimlik bilgilerine algılamak kimlik koruması sağlar.

Daha fazla bilgi için [Azure AD Connect eşitlemesi ile parola karması eşitlemeyi uygulama](../active-directory/connect/active-directory-aadconnectsync-implement-password-hash-synchronization.md).

Bulut kimliklerini ile kendi şirket içi kimlik tümleştirme olmayan kuruluşlar hesapları yönetiminde daha fazla ek yükü olabilir. Bu ek hatalar ve güvenlik ihlallerini olasılığını artırır.

## <a name="enable-single-sign-on"></a>Çoklu oturum açmayı etkinleştir

Bir mobil öncelikli ve bulut öncelikli dünyada, kullanıcılarınız ne zaman ve yerde üretken olabilmeleri çoklu oturum açma (SSO) cihazları, uygulamaları ve Hizmetleri için her yerden etkinleştirmek istiyorsunuz. Yönetmek için birden çok kimlik çözümleri varsa, bu değil yalnızca yönetimsel bir sorun haline BT aynı zamanda birden fazla parolayı hatırlamak zorunda kullanıcılar için.

Tüm uygulamalar ve kaynaklar için aynı kimlik çözümü kullanarak, SSO elde edebilirsiniz. Ve kullanıcılarınızın oturum açmak ve bunlar, kaynakların şirket içinde olup olmadığını veya bulutta gereken kaynaklara erişmek için aynı kimlik bilgileri kümesi kullanabilirsiniz.

**En iyi yöntem**: SSO'yu etkinleştirin.  
**Ayrıntı**: Azure AD [şirket içi Active Directory genişletir](../active-directory/connect/active-directory-aadconnect.md) buluta. Kullanıcılar, kendi etki alanına katılmış cihazlar, şirket kaynaklarına ve tüm web ve işlerini halletmek için ihtiyaç duydukları SaaS uygulamaları için birincil iş veya Okul hesabı kullanabilir. Kullanıcılar birden çok kullanıcı adları ve parolalar kümesini hatırlamak zorunda kalmaz ve kendi uygulama erişimini otomatik olarak sağlanan (veya sağlaması) kuruluş grup üyeliklerini ve çalışan olarak durumlarına göre. Galeri uygulamaları veya geliştirip [Azure AD Application Proxy](../active-directory/active-directory-application-proxy-get-started.md) ile yayımladığınız şirket içi uygulamalar için bu erişimi siz denetleyebilirsiniz.

SSO kullanıcıların erişim sağlamak için kullanın, [SaaS uygulamalarına](../active-directory/active-directory-appssoaccess-whatis.md) Azure AD'de kendi iş veya Okul hesaplarına bağlı. Bu yalnızca Microsoft SaaS uygulamaları, ancak diğer uygulamalar için de geçerli olduğu gibi [Google Apps](../active-directory/active-directory-saas-google-apps-tutorial.md) ve [Salesforce](../active-directory/active-directory-saas-salesforce-tutorial.md). Uygulamanızı Azure AD'ye olarak kullanmak üzere yapılandırabileceğiniz bir [SAML tabanlı kimlik](../active-directory/fundamentals-identity.md) sağlayıcısı. Güvenlik denetimi gibi Azure AD, Azure AD üzerinden erişim verilmiş sürece uygulamada oturum açmasına izin veren bir belirteç kesmez. Kullanıcılar bir üyesi olduğunu, doğrudan veya bir gruba erişim verebilirsiniz.

Kendi kullanıcıları ve uygulamaları için SSO kurmak için ortak bir kimlik oluşturmayın kuruluşların daha kullanıcıların sahip olduğu birden çok parola senaryoları için kullanıma sunulur. Bu senaryolar, kullanıcıların parolaları yeniden kullanma veya zayıf parola kullanma olasılığını artırın.

## <a name="turn-on-conditional-access"></a>Koşullu erişimi etkinleştirmek

Kullanıcıların çeşitli cihazlar ve uygulamalar her yerden kullanarak, kuruluşunuzun kaynaklarına erişim sağlayabilir. BT yöneticisi, bu cihazlar için güvenlik ve uyumluluk standartlarınızı karşılayan emin olmak istersiniz. Kimin bir kaynağa erişmek için yalnızca odaklanarak artık yeterli değildir.

Güvenlik ve üretkenlik dengelemek için bir erişim denetimi karar yapabilmeniz için önce bir kaynak nasıl erişildiği düşünmeniz gerekir. Azure AD'nin koşullu erişim özelliği sayesinde bu gereksinimi karşılayabilirsiniz. Koşullu erişimle koşullara göre bulut uygulamalarınıza erişen için otomatik erişim denetimi kararları yapabilirsiniz.

**En iyi yöntem**: Yönetme ve şirket kaynaklarına erişimi denetleyebilirsiniz.  
**Ayrıntı**: Azure AD'yi yapılandırma [koşullu erişim](../active-directory/active-directory-conditional-access-azure-portal.md) grubu, konum ve SaaS uygulamaları ve Azure AD bağlı uygulamalar için uygulama hassaslığı göre.

## <a name="enable-password-management"></a>Parola yönetimini etkinleştirme

Birden fazla Kiracı varsa veya kullanıcılara etkinleştirmek istediğiniz [kullanıcıların kendi parolalarını sıfırlamasına](../active-directory/active-directory-passwords-update-your-own-password.md), kötüye kullanımı önlemek için uygun güvenlik ilkelerini kullanmak önemlidir.

**En iyi yöntem**: Kullanıcılarınız için Self Servis parola sıfırlama (SSPR) ayarlayın.  
**Ayrıntı**: Azure AD'yi kullanın [Self Servis parola sıfırlama](../active-directory-b2c/active-directory-b2c-reference-sspr.md) özelliği.

**En iyi yöntem**: İzleyici nasıl veya SSPR gerçekten kullanılıyorsa.  
**Ayrıntı**: Azure AD kullanarak kaydettirmekte olduğunuz kullanıcıları izlemek [parola sıfırlama kayıt Etkinlik Raporu](../active-directory/active-directory-passwords-get-insights.md). Azure AD'nin sağladığı raporlama özelliği, önceden oluşturulmuş raporları kullanarak soruları yanıtlamanıza yardımcı olur. Uygun şekilde lisansınız varsa, özel sorgular oluşturabilirsiniz.

## <a name="enforce-multi-factor-verification-for-users"></a>Kullanıcılar için çok faktörlü doğrulama zorla

İki aşamalı doğrulama tüm kullanıcılarınız için ihtiyacınız olan öneririz. Bu Yöneticiler ve başkaları (örneğin, finansal görevlileri), hesap tehlikede olursa önemli bir etkisi olabilir kuruluşunuzda içerir.

İki aşamalı doğrulama gerektirme birden çok seçenek vardır. Sizin için en iyi seçenek, hedeflerinizi, kullanmakta olduğunuz Azure AD sürümü ve, bir lisanslama programı bağlıdır. Bkz: [bir kullanıcı için iki aşamalı doğrulama gerektirme](../active-directory/authentication/howto-mfa-userstates.md) sizin için en iyi seçeneği belirlemek için. Bkz: [Azure AD'ye](https://azure.microsoft.com/pricing/details/active-directory/) ve [Azure multi-Factor Authentication](https://azure.microsoft.com/pricing/details/multi-factor-authentication/) sayfaları lisansları hakkında daha fazla bilgi için fiyatlandırma ve fiyatlandırma.

Seçeneklerinizi ve Avantajlarınızı iki aşamalı doğrulamayı etkinleştirmek için aşağıda verilmiştir:

**Seçenek 1**: [Kullanıcı durumunu değiştirerek çok faktörlü kimlik doğrulamasını etkinleştirme](../active-directory/authentication/howto-mfa-userstates.md).   
**Avantajı**: Bu, iki aşamalı doğrulama gerektirme geleneksel yöntemidir. Her ikisi de çalıştığını [Azure multi-Factor Authentication Bulut ve Azure multi-Factor Authentication sunucusu](../active-directory/authentication/concept-mfa-whichversion.md). Bu yöntemi kullanarak oturum açtıklarında her açtığınızda iki aşamalı doğrulamayı gerçekleştirmek üzere kullanıcıların gerektirir ve koşullu erişim ilkeleri geçersiz kılar.

**Seçenek 2**: [Multi-Factor Authentication'ı ile koşullu erişim ilkesini etkinleştir](../active-directory/authentication/howto-mfa-getstarted.md).
**Avantajı**: Bu seçeneği kullanarak belirli koşullar altında iki aşamalı doğrulama iste sağlar [koşullu erişim](../active-directory/active-directory-conditional-access-azure-portal.md). Belirli koşullar, kullanıcı oturum açma farklı konumlar, güvenilmeyen cihazlar ya da riskli uygulamalar olabilir. İki aşamalı doğrulamayı gerekli olduğu belirli koşullar tanımlayarak sabit bir önlemeye kullanıcı deneyimi olabilir, kullanıcılarınız için isteyen kaçınmanızı sağlar.

Kullanıcılarınız için iki aşamalı doğrulamayı etkinleştirmek için en esnek yolu budur. Koşullu erişim ilkesini etkinleştirme yalnızca bulutta Azure multi-Factor Authentication için geçerlidir ve Azure AD premium özelliğidir. Bu yöntemde hakkında daha fazla bilgi bulabilirsiniz [Azure multi-Factor Authentication'ı bulut tabanlı dağıtım](../active-directory/authentication/howto-mfa-getstarted.md).

**Seçenek 3**: Multi-Factor Authentication ile koşullu erişim ilkeleri kullanıcı ve oturum açma riski değerlendirilerek etkinleştirmek [Azure AD kimlik koruması](../active-directory/authentication/tutorial-risk-based-sspr-mfa.md).   
**Avantajı**: Bu seçeneği sağlar:

- Kuruluşunuzun kimliklerini etkileyen olası güvenlik açıklarını algılama.
- Otomatik yanıtları, kuruluşunuzun kimliklerini ilgili algılanan kuşkulu eylemleri için yapılandırın.
- Şüpheli olayları araştırmanıza ve bunları gidermek için uygun eylemi gerçekleştirin.

Kullanıcı ve oturum açma riski tüm bulut uygulamalarınız için iki aşamalı doğrulamayı gerekli olup olmadığını belirlemek için Azure AD kimlik koruması risk değerlendirmesine göre bu yöntemi kullanır. Bu yöntem, Azure Active Directory P2 lisansı gerektirir. Bu yöntemde hakkında daha fazla bilgi bulabilirsiniz [Azure Active Directory kimlik koruması](../active-directory/identity-protection/overview.md).

> [!Note]
> Seçenek 1, kullanıcı durumunu değiştirerek çok faktörlü kimlik doğrulamasını etkinleştirme, koşullu erişim ilkeleri geçersiz kılar. Seçenek 2 ve 3 koşullu erişim ilkeleri kullandığınızdan, 1. seçenek kendileriyle kullanamazsınız.

Kimlik koruması, iki aşamalı doğrulama gibi ek katmanları eklemeyen kuruluşları için kimlik bilgisi hırsızlığı saldırısına daha açıktır. Bir kimlik bilgisi hırsızlığı saldırısına veri güvenliğinin aşılmasına yol açabilir.

## <a name="use-role-based-access-control-rbac"></a>Rol tabanlı erişim denetimi (RBAC) kullanın

Erişimi kısıtlama temel alarak [bilmeniz gereken](https://en.wikipedia.org/wiki/Need_to_know) ve [en az ayrıcalık](https://en.wikipedia.org/wiki/Principle_of_least_privilege) güvenlik ilkeleri, veri erişimi için güvenlik ilkelerini zorlamak istediğinizde kuruluşlar için zorunlu. Kullanabileceğiniz [rol tabanlı erişim denetimi (RBAC)](../role-based-access-control/overview.md) kullanıcılara, gruplara ve uygulamalara belirli bir kapsamda izin vermek istiyorsanız. Rol atamasının kapsamı, bir abonelik, kaynak grubu veya tek bir kaynak olabilir.

Kullanabileceğiniz [yerleşik RBAC](../role-based-access-control/built-in-roles.md) ayrıcalıkları kullanıcılara atamak için azure'da rolleri. Veri erişim denetimi, RBAC gibi özellikleri kullanarak zorlama kuruluşların kullanıcıları için gerekenden daha fazla ayrıcalık vermiş. Belirli sahip olmamalıdır (örneğin, yüksek iş etkisi) veri türleri için kullanıcı erişimine izin vererek bu veri güvenliğinin aşılmasına yol açabilir.

## <a name="lower-exposure-of-privileged-accounts"></a>Ayrıcalıklı hesapların açığa çıkarılması

Güvenliğini sağlama ayrıcalıklı erişim iş varlıkların korunması için bir kritik ilk adımdır. Bilgi veya kaynakları güvenli hale getirmek için erişimi olan kişi sayısını en aza indirmek erişim sağlama kötü niyetli bir kullanıcı veya yetkili bir kullanıcı yanlışlıkla hassas kaynak etkileyen olasılığını azaltır.

Ayrıcalıklı hesaplar BT sistemlerini yöneten hesaplarıdır. Siber saldırganlar bir kuruluşun veri ve sistemlere erişim elde etmek için bu hesapları hedefler. Ayrıcalıklı erişim güvenliğini sağlamak için hesaplar ve kötü niyetli bir kullanıcının maruz kalma riskini sistemlerden yalıtmak.

Geliştirme ve siber saldırganlara karşı ayrıcalıklı erişimin güvenliğini sağlamak için bir yol haritasının izlenmesini öneririz. Microsoft Azure, Office 365 ve diğer bulut Hizmetleri, kimliklerin yönetimi ve yönetilen veya Azure AD'de bildirilen erişim güvenliğini sağlamak için ayrıntılı bir yol haritası oluşturma hakkında daha fazla bilgi için gözden [ayrıcalıklı erişimin güvenliğini sağlama karma ve bulut dağıtımları için Azure AD](../active-directory/users-groups-roles/directory-admin-roles-secure.md).

Aşağıdaki bulunan en iyi yöntemler özetlenmiştir [ayrıcalıklı erişimin güvenliğini sağlama ve karma bulut dağıtımları için Azure AD'de](../active-directory/users-groups-roles/directory-admin-roles-secure.md):

**En iyi yöntem**: Yönetebilir, denetleyebilir ve ayrıcalıklı hesaplara erişim izleyin.   
**Ayrıntı**: Açma [Azure AD Privileged Identity Management](../active-directory/privileged-identity-management/active-directory-securing-privileged-access.md). İçin Privileged Identity Management'ı etkinleştirdikten sonra bildirim alacaksınız. e-posta iletileri için ayrıcalıklı erişim rol değişiklikleri. Dizininizdeki yüksek ayrıcalıklı rollere ek kullanıcılar eklendiğinde bu bildirimleri erken uyarı sağlamak.

**En iyi yöntem**: Belirleyin ve yüksek ayrıcalıklı rolleri hesaplarının kategorilere ayırın.   
**Ayrıntı**: Üzerinde Azure AD Privileged Identity Management'ı etkinleştirdikten sonra genel yönetici, ayrıcalıklı Rol Yöneticisi ve diğer yüksek ayrıcalıklı rolleri olan kullanıcıların görüntüleyin. Artık gerekli olmayan tüm hesapları bu rolleri kaldırın ve yönetici rollerine atandığı geri kalan hesapları kategorilere ayırın:

- Tek tek yönetici kullanıcılara atanan ve yönetim dışı amaçlar için (örneğin, kişisel e-posta) kullanılabilir
- Tek tek yönetici kullanıcılara atanan ve yalnızca yönetim amacıyla belirtilen
- Birden çok kullanıcı arasında paylaşılan
- Acil Durum erişim senaryoları için
- Otomatik betikler için
- Dış kullanıcılar için

**En iyi yöntem**: "Tam zamanında" uygulamak daha fazla ayrıcalıklı hesapların kullanımını içine görünürlüğünüzü artırın ve ayrıcalıkların tehditlere maruz kalabileceği süreyi azaltmak için (JIT) erişim.   
**Ayrıntı**: Azure AD Privileged Identity Management sağlar:

- Yalnızca kendi ayrıcalıklarını JIT alma kullanıcıların sınırlayın.
- Roller için kısaltılmış bir süre ayrıcalıkları otomatik olarak iptal edilir güvenle atayın.

**En iyi yöntem**: En az iki Acil Durum erişim hesapları tanımlayın.   
**Ayrıntı**: Acil Durum erişim hesapları, kuruluşların var olan bir Azure Active Directory ortamında ayrıcalıklı erişimi kısıtlamasına yardımcı olur. Bu hesaplar, son derece ayrıcalıklı olan ve belirli kişilere atanmaz. Acil Durum erişim hesapları, normal yönetim hesaplarını burada kullanılamaz senaryoları için sınırlıdır. Kuruluşlar, yalnızca gerekli süreyi Acil Durum hesabının kullanımını sınırlamanız gerekir.

Atanan veya genel yönetici rolü için uygun olan hesaplarını değerlendirin. Kullanarak herhangi bir salt bulut hesapları görmüyorsanız, `*.onmicrosoft.com` (hedeflenen) Acil Durum erişim için etki alanı oluşturun. Daha fazla bilgi için bkz: Azure AD'de Acil Durum erişimi yönetici hesaplarını yönetme.

**En iyi yöntem**: Çok faktörlü kimlik doğrulamasını etkinleştirmek ve diğer tüm yüksek ayrıcalıklı tek kullanıcı Federasyon olmayan yönetici hesapları kaydedin.  
**Ayrıntı**: Oturum açma bir veya daha fazla Azure AD yönetim rolleri kalıcı olarak atanan tüm bireysel kullanıcılar için Azure multi-Factor Authentication iste: Genel yönetici, ayrıcalıklı Rol Yöneticisi, Exchange Online yönetici ve SharePoint Online Yönetici. Etkinleştirmek için kılavuzu kullanın [yönetici hesaplarınız için multi-Factor Authentication](../active-directory/authentication/howto-mfa-userstates.md) ve tüm kullanıcılarla olduğundan emin olun [kayıtlı](https://aka.ms/mfasetup).

**En iyi yöntem**: En sık kullanılan Saldırıya uğrayan tekniklerini azaltma için adımları uygulayın.  
**Ayrıntı**: [İş veya Okul hesapları için geçiş için gereken yönetim rolleri, Microsoft hesapları tanımlayın](../active-directory/users-groups-roles/directory-admin-roles-secure.md#identify-microsoft-accounts-in-administrative-roles-that-need-to-be-switched-to-work-or-school-accounts)  

[Ayrı kullanıcı hesaplarını ve posta iletme genel yönetici hesapları için emin olun.](../active-directory/users-groups-roles/directory-admin-roles-secure.md)  

[Yönetim hesapları, parolaları en son değiştirilen olun](../active-directory/users-groups-roles/directory-admin-roles-secure.md#ensure-the-passwords-of-administrative-accounts-have-recently-changed)  

[Parola Karması eşitleme üzerinde Aç](../active-directory/users-groups-roles/directory-admin-roles-secure.md#turn-on-password-hash-synchronization)  

[İfşa edilen kullanıcı yanı sıra tüm ayrıcalıklı rollerdeki kullanıcılar için multi-Factor Authentication iste](../active-directory/users-groups-roles/directory-admin-roles-secure.md#require-multi-factor-authentication-mfa-for-users-in-all-privileged-roles-as-well-as-exposed-users)  

[Office 365 güvenli puanı (Office 365 kullanıyorsanız) edinmek](../active-directory/users-groups-roles/directory-admin-roles-secure.md#obtain-your-office-365-secure-score-if-using-office-365)  

[(Office 365 kullanılıyorsa) Office 365 güvenlik ve uyumluluk Kılavuzu gözden geçirin](../active-directory/users-groups-roles/directory-admin-roles-secure.md#review-the-office-365-security-and-compliance-guidance-if-using-office-365)  

[Office 365 Etkinlik izleme (Office 365 kullanıyorsanız) yapılandırma](../active-directory/users-groups-roles/directory-admin-roles-secure.md#configure-office-365-activity-monitoring-if-using-office-365)  

[Olay/Acil Durum yanıt planı sahipleri oluştur](../active-directory/users-groups-roles/directory-admin-roles-secure.md#establish-incidentemergency-response-plan-owners)  

[Şirket içinde ayrıcalıklı yönetim hesaplarının güvenliğini sağlama](../active-directory/users-groups-roles/directory-admin-roles-secure.md#turn-on-password-hash-synchronization)

Ayrıcalıklı erişim güvenliğini sağlamazsanız, çok sayıda kullanıcınız yüksek ayrıcalıklı rolleri ve saldırılara karşı daha savunmasızdır bulabilirsiniz. Siber saldırganlar, genellikle hedef yönetici hesapları ve ayrıcalıklı erişimin diğer öğelerini kimlik bilgisi hırsızlığı kullanarak hassas verileri ve sistemleri erişim kazanmak için de dahil olmak üzere kötü amaçlı aktörler.

## <a name="control-locations-where-resources-are-created"></a>Kaynakların oluşturulabileceği denetim konumları

Bulut operatörleri, kuruluşunuzun kaynakları yönetmek için gerekli olan kuralları kesilmesini önlenirken görevleri gerçekleştirmek etkinleştirmek çok önemlidir. Kaynakların oluşturulabileceği konumlarını denetlemek isterseniz kuruluşların Bu konumlar sabit kodlamanız gerekir.

Kullanabileceğiniz [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) , tanımları, özellikle verilmeyen kaynakları ve eylemleri açıklayan güvenlik ilkeleri oluşturmak için. Bu ilke tanımları abonelik, kaynak grubunu veya ayrı bir kaynak gibi istenilen kapsamda atarsınız.

> [!NOTE]
> Güvenlik ilkeleri RBAC ile aynı değildir. Bunlar aslında RBAC bu kaynakları oluşturmak için Kullanıcıları yetkilendirmek için kullanır.
>
>

Kaynakları nasıl oluşturulduğunu denetlediğiniz olmayan kuruluşlar, kullanıcılara ihtiyaç duydukları çok daha fazla kaynak oluşturarak hizmet sırasında kötüye kullandığı daha açıktır. Kaynak oluşturma işlemini sağlamlaştırma, çok kiracılı bir senaryo güvenliğini sağlamak için önemli bir adımdır.

## <a name="actively-monitor-for-suspicious-activities"></a>Şüpheli etkinlikler için etkin bir şekilde izleyin

Sistem izleme etkin bir kimliği hızla şüpheli davranışı saptamanıza ve araştırılması için bir uyarı tetikler. Aşağıdaki tabloda, kuruluşların kimliklerini izlemesine yardımcı olacak iki Azure AD özellikleri listelenmektedir:

**En iyi yöntem**: Belirlemek üzere bir yöntem vardır:

- Oturum açmak çalışır [izlenen olmadan](../active-directory/active-directory-reporting-sign-ins-from-unknown-sources.md).
- [Deneme yanılma](../active-directory/active-directory-reporting-sign-ins-after-multiple-failures.md) saldırılarına karşı belirli bir hesabı.
- Birden çok konumdan oturum açmak çalışır.
- Oturum açma işlemleri [virüs bulaşmış cihazlar](../active-directory/active-directory-reporting-sign-ins-from-possibly-infected-devices.md).
- Şüpheli IP adresleri.

**Ayrıntı**: Azure AD Premium kullanma [anomali raporları](../active-directory/active-directory-view-access-usage-reports.md). Bu raporlar günlük olarak veya isteğe bağlı olarak (genellikle, bir olay yanıtı senaryosu) çalıştırmak, BT yöneticileri için yerinde süreçleri ve yordamları sahip.

**En iyi yöntem**: Riskleri size bildirir ve risk düzeyine (yüksek, Orta veya düşük) iş gereksinimlerinizi ayarlayabilirsiniz etkin bir izleme sistemi vardır.   
**Ayrıntı**: Kullanım [Azure AD kimlik koruması](../active-directory/active-directory-identityprotection.md), hangi geçerli bayrakları risklerini kendi Panoda ve e-posta ile günlük özet bildirimleri gönderir. Kuruluşunuzun kimliklerini korumaya yardımcı olmak için belirtilen risk düzeyi üst sınırına ulaşıldığında, otomatik olarak algılanan sorunlara yanıt risk tabanlı ilkeler yapılandırabilirsiniz.

Etkin kimlik sistemlerinin izleme kuruluşların, kullanıcı kimlik bilgilerini ele yaşama risk altındadır. Kuşkulu etkinlikler bu kimlik bilgileri aracılığıyla yer alıyorlar bilgi sahibi olmadan, kuruluşların bu tür bir tehdit etkisini olamaz.

## <a name="next-step"></a>Sonraki adım

Bkz: [Azure güvenlik en iyi uygulamaları ve desenleri](security-best-practices-and-patterns.md) kullanmak üzere daha fazla güvenlik için en iyi yöntemler, tasarlama, dağıtma ve Azure'ı kullanarak bulut çözümlerinizi yönetme.
