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
ms.date: 05/03/2019
ms.author: barclayn
ms.openlocfilehash: 2a669f5b46db4d5de7d1d6863b94e6c117667aee
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65153232"
---
# <a name="azure-identity-management-and-access-control-security-best-practices"></a>Azure kimlik yönetimi ve erişim en iyi güvenlik denetimi
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
* Bağlı kiracılarını yönetme
* Çoklu oturum açmayı etkinleştir
* Koşullu erişimi etkinleştirmek
* Parola yönetimini etkinleştirme
* Kullanıcılar için çok faktörlü doğrulama zorla
* Rol tabanlı erişim denetimi kullanma
* Ayrıcalıklı hesapların açığa çıkarılması
* Kaynakların yer aldığı denetim konumları
* Depolama kimlik doğrulaması için Azure AD'yi kullanın

## <a name="treat-identity-as-the-primary-security-perimeter"></a>Kimlik birincil güvenlik çevresi olarak değerlendir

Birçok kimlik birincil güvenlik çevresi olarak göz önünde bulundurun. Bu, gelen ağ güvenlik odaklı geleneksel bir kaydırmadır. Ağ duvarlar tutmak alma daha porous ve bu çevre savunması açılımı önce olduğu gibi etkin olamaz [KCG](https://aka.ms/byodcg) cihazlar ve bulut uygulamaları.

[Azure Active Directory (Azure AD)](../active-directory/active-directory-whatis.md) Azure kimlik ve erişim yönetimi çözümüdür. Azure AD bir çok kiracılı, bulut tabanlı dizin ve Kimlik Yönetimi Microsoft hizmetidir. Bu, temel Dizin Hizmetleri, uygulama erişim yönetimi ve kimlik korumasını tek bir çözüm birleştirir.

Aşağıdaki bölümlerde, Azure AD kullanarak kimlik ve erişim güvenliği için en iyi yöntemler listelenmiştir.

## <a name="centralize-identity-management"></a>Kimlik yönetimini merkezden gerçekleştirin

İçinde bir [karma kimlik](https://resources.office.com/ww-landing-M365E-EMS-IDAM-Hybrid-Identity-WhitePaper.html?) senaryo, şirket içi tümleştirme ve bulut dizinleri öneririz. Tümleştirme hesaplarını bir hesap oluşturulduğu bağımsız olarak bir konumdan yönetmek üzere BT ekibinize sağlar. Tümleştirme, ayrıca, kullanıcılarınızın hem bulut hem de şirket içi kaynaklara erişmek için bir ortak kimlik sağlayarak daha üretken olmanıza yardımcı olur.

**En iyi yöntem**: Tek bir'kurmak Azure AD örneği. Tutarlılık ve tek bir yetkili kaynaklardan netliği artırabilir ve insan hatalarına ve yapılandırma karmaşıklığı güvenlik risklerini azaltın.
**Ayrıntı**: Tek bir Azure AD atamak şirket ve kuruluş hesapları için yetkili kaynak olarak dizin.

**En iyi yöntem**: Şirket içi dizinlerinizi Azure AD ile tümleştirme.  
**Ayrıntı**: Kullanım [Azure AD Connect](../active-directory/connect/active-directory-aadconnect.md) şirket içi dizininizi bulut dizininizle eşitlenecek.

> [!Note]
> Vardır [Azure AD Connect performansını etkileyen faktörler](../active-directory/hybrid/plan-connect-performance-factors.md). Azure AD Connect engelleyen güvenlik ve üretkenlik sistemlerden yeterli performansa sahip olmayan tutmak için yeterli kapasiteye sahip olun. Büyük veya karmaşık kuruluşların (100. 000'den fazla nesneleri sağlama kuruluşlar) izlemelidir [önerileri](../active-directory/hybrid/whatis-hybrid-identity.md) kendi Azure AD Connect uygulamasını en iyi duruma getirme.

**En iyi yöntem**: Var olan Active Directory Örneğinizde yüksek ayrıcalıklara sahip hesaplarını Azure AD'ye eşitleme.
**Ayrıntı**: Varsayılan değişmez [Azure AD Connect yapılandırmasının](../active-directory/hybrid/how-to-connect-sync-configure-filtering.md) bu hesapları filtreler. Bu yapılandırma, saldırganlar bulut hizmetinden (önemli bir olay oluşturabilir) şirket içi varlıkları için özetleme riskini azaltır.

**En iyi yöntem**: Parola Karması eşitlemeyi kapatın.  
**Ayrıntı**: Parola Karması eşitleme, bulut tabanlı bir Azure şirket içi Active Directory örneğinden kullanıcı parola karmalarını eşitleyin için kullanılan bir özelliktir AD örneği. Bu eşitleme önceki saldırılara karşı durumdayken sızdırılan kimlik bilgilerine karşı korunmasına yardımcı olur.

Active Directory Federasyon Hizmetleri (AD FS) veya diğer kimlik sağlayıcıları ile Federasyon kullanmaya karar olsa bile, şirket içi sunucularınızı geçici olarak kullanılamıyor veya başarısız durumda isteğe bağlı olarak parola karması eşitleme yedek olarak ayarlayabilirsiniz. Bu eşitleme hizmeti için kendi şirket içi Active Directory örneğine oturum açmak için kullandıkları aynı parolayı kullanarak oturum açmalarını sağlar. Ayrıca, bir kullanıcı aynı e-posta adresi ve parola Azure AD'ye bağlı olmayan diğer sistemlerde kullanıldıysa tehlikeye, bilinen parola ile eşitlenmiş parola karmalarının karşılaştırarak tehlikeye atılmış kimlik bilgilerine algılamak kimlik koruması sağlar.

Daha fazla bilgi için [Azure AD Connect eşitlemesi ile parola karması eşitlemeyi uygulama](../active-directory/connect/active-directory-aadconnectsync-implement-password-hash-synchronization.md).

**En iyi yöntem**: Yeni uygulama geliştirme için Azure AD kimlik doğrulaması için kullanın.
**Ayrıntı**: Kimlik doğrulamasını desteklemek için doğru özellikleri kullanın:

  - Çalışanlar için Azure AD
  - [Azure AD B2B](https://docs.microsoft.com/azure/active-directory/b2b/) Konuk kullanıcılar ve dış iş ortakları için
  - [Azure AD B2C](https://docs.microsoft.com/azure/active-directory-b2c/) müşteriler kaydolması kontrol etmek için oturum açın ve uygulamalarınızı kullandıklarında profillerini yönetme

Bulut kimliklerini ile kendi şirket içi kimlik tümleştirme olmayan kuruluşlar hesapları yönetiminde daha fazla ek yükü olabilir. Bu ek hatalar ve güvenlik ihlallerini olasılığını artırır.

> [!Note]
> Hangi dizin hesapları kalır ve yönetici iş istasyonunda kullanılan yeni bulut Hizmetleri tarafından yönetilen veya mevcut olup işler kritik seçmeniz gerekebilir. Mevcut yönetim ve işlemler sağlama kimlik kullanarak bazı riskleri azaltabilir, ancak bir saldırganın bir şirket içi hesap ödün ve buluta özetleme riskini de oluşturabilirsiniz. Farklı roller (örneğin, iş birimi yöneticileri ve BT yöneticileri) için farklı bir strateji kullanmak isteyebilirsiniz. İki seçeneğiniz vardır. İlk seçenek, şirket içi Active Directory örneğinizle eşitlenmedi Azure AD hesapları oluşturmaktır. Yönetme ve düzeltme eki Intune kullanarak Azure ad yönetim iş istasyonunuzu katılın. İkinci seçenek, şirket içi Active Directory örneğinizle eşitleyerek mevcut yönetici hesaplarını kullanmaktır. Var olan iş istasyonları, yönetim ve güvenlik için Active Directory etki alanında kullanın.

## <a name="manage-connected-tenants"></a>Bağlı kiracılarını yönetme
Kuruluşunuzun güvenlik riskini değerlendirmek için ve ilkeleri, kuruluşunuzun Yasal gereksinimlere ve ardından olup olmadığını belirlemek için görünürlük gerekir. Güvenlik kuruluşunuzun görünürlük tüm Abonelikleri, üretim ortamına ve ağa bağlı olduğundan emin olmanız gerekir (aracılığıyla [Azure ExpressRoute](../expressroute/expressroute-introduction.md) veya [siteden siteye VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)). A [genel yönetici/şirket Yöneticisi](../active-directory/users-groups-roles/directory-assign-admin-roles.md#company-administrator) Azure AD'de erişimlerinin yükseltebilir [kullanıcı erişimi Yöneticisi](../role-based-access-control/built-in-roles.md#user-access-administrator) rolü ve tüm abonelikleri ve ortamınıza bağlı yönetilen grupları görüntüleyin.

Bkz: [tüm Azure aboneliklerini ve Yönetim gruplarını yönetmek için erişimini yükseltme](../role-based-access-control/elevate-access-global-admin.md) ortamınıza bağlı ve güvenlik grubunuz tüm abonelikleri veya yönetim gruplarıyla görebilmelerini sağlamak için. Riskleri değerlendirdikten sonra bu yükseltilmiş erişim kaldırmanız gerekir.

## <a name="enable-single-sign-on"></a>Çoklu oturum açmayı etkinleştir

Bir mobil öncelikli ve bulut öncelikli dünyada, kullanıcılarınız ne zaman ve yerde üretken olabilmeleri çoklu oturum açma (SSO) cihazları, uygulamaları ve Hizmetleri için her yerden etkinleştirmek istiyorsunuz. Yönetmek için birden çok kimlik çözümleri varsa, bu değil yalnızca yönetimsel bir sorun haline BT aynı zamanda birden fazla parolayı hatırlamak zorunda kullanıcılar için.

Tüm uygulamalar ve kaynaklar için aynı kimlik çözümü kullanarak, SSO elde edebilirsiniz. Ve kullanıcılarınızın oturum açmak ve bunlar, kaynakların şirket içinde olup olmadığını veya bulutta gereken kaynaklara erişmek için aynı kimlik bilgileri kümesi kullanabilirsiniz.

**En iyi yöntem**: SSO'yu etkinleştirin.  
**Ayrıntı**: Azure AD [şirket içi Active Directory genişletir](../active-directory/connect/active-directory-aadconnect.md) buluta. Kullanıcılar, kendi etki alanına katılmış cihazlar, şirket kaynaklarına ve tüm web ve işlerini halletmek için ihtiyaç duydukları SaaS uygulamaları için birincil iş veya Okul hesabı kullanabilir. Kullanıcılar birden çok kullanıcı adları ve parolalar kümesini hatırlamak zorunda kalmaz ve kendi uygulama erişimini otomatik olarak sağlanan (veya sağlaması) kuruluş grup üyeliklerini ve çalışan olarak durumlarına göre. Galeri uygulamaları veya geliştirip [Azure AD Application Proxy](../active-directory/active-directory-application-proxy-get-started.md) ile yayımladığınız şirket içi uygulamalar için bu erişimi siz denetleyebilirsiniz.

SSO kullanıcıların erişim sağlamak için kullanın, [SaaS uygulamalarına](../active-directory/active-directory-appssoaccess-whatis.md) Azure AD'de kendi iş veya Okul hesaplarına bağlı. Bu yalnızca Microsoft SaaS uygulamaları, ancak diğer uygulamalar için de geçerli olduğu gibi [Google Apps](../active-directory/active-directory-saas-google-apps-tutorial.md) ve [Salesforce](../active-directory/active-directory-saas-salesforce-tutorial.md). Uygulamanızı Azure AD'ye olarak kullanmak üzere yapılandırabileceğiniz bir [SAML tabanlı kimlik](../active-directory/fundamentals-identity.md) sağlayıcısı. Güvenlik denetimi gibi Azure AD, Azure AD üzerinden erişim verilmiş sürece uygulamada oturum açmasına izin veren bir belirteç kesmez. Kullanıcılar bir üyesi olduğunu, doğrudan veya bir gruba erişim verebilirsiniz.

Kendi kullanıcıları ve uygulamaları için SSO kurmak için ortak bir kimlik oluşturmayın kuruluşların daha kullanıcıların sahip olduğu birden çok parola senaryoları için kullanıma sunulur. Bu senaryolar, kullanıcıların parolaları yeniden kullanma veya zayıf parola kullanma olasılığını artırın.

## <a name="turn-on-conditional-access"></a>Koşullu erişimi etkinleştirmek

Kullanıcıların çeşitli cihazlar ve uygulamalar her yerden kullanarak, kuruluşunuzun kaynaklarına erişim sağlayabilir. BT yöneticisi, bu cihazlar için güvenlik ve uyumluluk standartlarınızı karşılayan emin olmanız gerekir. Kimin bir kaynağa erişmek için yalnızca odaklanarak artık yeterli değildir.

Güvenlik ve üretkenlik dengelemek için erişim denetimi hakkında bir karar yapabilmeniz için önce bir kaynak nasıl erişildiği düşünmeniz gerekir. Azure AD'nin koşullu erişim özelliği sayesinde bu gereksinimi karşılayabilirsiniz. Koşullu erişim ile bulut uygulamalarınıza erişen koşulları dayalı otomatik erişim denetimi kararları yapabilirsiniz.

**En iyi yöntem**: Yönetme ve şirket kaynaklarına erişimi denetleyebilirsiniz.  
**Ayrıntı**: Azure AD'yi yapılandırma [koşullu erişim](../active-directory/active-directory-conditional-access-azure-portal.md) grubu, konum ve SaaS uygulamaları ve Azure AD bağlı uygulamalar için uygulama hassaslığı göre.

**En iyi yöntem**: Eski bir kimlik doğrulama protokollerini engellemek.
**Ayrıntı**: Saldırganlar özellikle parola ilaç saldırıları için her gün daha eski protokolleri zayıflıklar yararlanma. Eski protokollerini engellemek için koşullu erişimi yapılandırın. Video bkz [Azure AD: İlgilenmenin](https://www.youtube.com/watch?v=wGk0J4z90GI) daha fazla bilgi için.

## <a name="enable-password-management"></a>Parola yönetimini etkinleştirme

Birden fazla Kiracı varsa veya kullanıcılara etkinleştirmek istediğiniz [kullanıcıların kendi parolalarını sıfırlamasına](../active-directory/active-directory-passwords-update-your-own-password.md), kötüye kullanımı önlemek için uygun güvenlik ilkelerini kullanmak önemlidir.

**En iyi yöntem**: Kullanıcılarınız için Self Servis parola sıfırlama (SSPR) ayarlayın.  
**Ayrıntı**: Azure AD'yi kullanın [Self Servis parola sıfırlama](../active-directory-b2c/active-directory-b2c-reference-sspr.md) özelliği.

**En iyi yöntem**: İzleyici nasıl veya SSPR gerçekten kullanılıyorsa.  
**Ayrıntı**: Azure AD kullanarak kaydettirmekte olduğunuz kullanıcıları izlemek [parola sıfırlama kayıt Etkinlik Raporu](../active-directory/active-directory-passwords-get-insights.md). Azure AD'nin sağladığı raporlama özelliği, önceden oluşturulmuş raporları kullanarak soruları yanıtlamanıza yardımcı olur. Uygun şekilde lisansınız varsa, özel sorgular oluşturabilirsiniz.

**En iyi yöntem**: Bulut tabanlı parola ilkelerini, şirket içi altyapınızı genişletin.
**Ayrıntı**: Parola ilkeleri, kuruluşunuzdaki bulut tabanlı parola değişiklikleri için yaptığınız gibi şirket içi parola değişiklikleri için aynı denetimleri uygulayarak geliştirin. Yükleme [Azure AD parola koruması](../active-directory/authentication/concept-password-ban-bad.md) Windows Server Active Directory aracıları var olan altyapınızla yasaklı parola listelerini genişletmek şirket için. Kullanıcıları ve yöneticileri değiştirme ayarlayın ya da parolalarını şirket içi yalnızca bulut kullanıcı olarak aynı parola ilkesine uyması istenir.

## <a name="enforce-multi-factor-verification-for-users"></a>Kullanıcılar için çok faktörlü doğrulama zorla

İki aşamalı doğrulama tüm kullanıcılarınız için ihtiyacınız olan öneririz. Bu Yöneticiler ve başkaları (örneğin, finansal görevlileri), hesap tehlikede olursa önemli bir etkisi olabilir kuruluşunuzda içerir.

İki aşamalı doğrulama gerektirme birden çok seçenek vardır. Sizin için en iyi seçenek, hedeflerinizi, kullanmakta olduğunuz Azure AD sürümü ve, bir lisanslama programı bağlıdır. Bkz: [bir kullanıcı için iki aşamalı doğrulama gerektirme](../active-directory/authentication/howto-mfa-userstates.md) sizin için en iyi seçeneği belirlemek için. Bkz: [Azure AD'ye](https://azure.microsoft.com/pricing/details/active-directory/) ve [Azure multi-Factor Authentication](https://azure.microsoft.com/pricing/details/multi-factor-authentication/) sayfaları lisansları hakkında daha fazla bilgi için fiyatlandırma ve fiyatlandırma.

Seçeneklerinizi ve Avantajlarınızı iki aşamalı doğrulamayı etkinleştirmek için aşağıda verilmiştir:

**Seçenek 1**: [Kullanıcı durumunu değiştirerek çok faktörlü kimlik doğrulamasını etkinleştirme](../active-directory/authentication/howto-mfa-userstates.md).   
**Avantajı**: Bu, iki aşamalı doğrulama gerektirme geleneksel yöntemidir. Her ikisi de çalıştığını [Azure multi-Factor Authentication Bulut ve Azure multi-Factor Authentication sunucusu](../active-directory/authentication/concept-mfa-whichversion.md). Bu yöntemi kullanarak oturum açtıklarında her açtığınızda iki aşamalı doğrulamayı gerçekleştirmek üzere kullanıcıların gerektirir ve koşullu erişim ilkeleri geçersiz kılar.

Çok faktörlü kimlik doğrulaması etkinleştirilmiş olması gereken yere belirlemek için bkz: [Kuruluşum için hangi sürümün Azure MFA'ın hangisi?](../active-directory/authentication/concept-mfa-whichversion.md).

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

## <a name="use-role-based-access-control"></a>Rol tabanlı erişim denetimi kullanma
Bulut kaynakları için erişim yönetimi bulut kullanan tüm kuruluşlar için önemlidir. [Rol tabanlı erişim denetimi (RBAC)](../role-based-access-control/overview.md) hangi alanlarda erişime sahip Azure kaynaklarına kimlerin erişebileceğini ve bu kaynaklarla ne yapabileceklerini yönetmenize yardımcı olur.

Grupları veya bireysel rolleri Azure belirli işlevler için sorumlu belirleme, İnsan için yol açabilecek karışıklık ve güvenlik risklerini oluşturduğunuz Otomasyon hataları önlemeye yardımcı olur. Erişimi kısıtlama temel alarak [bilmeniz gereken](https://en.wikipedia.org/wiki/Need_to_know) ve [en az ayrıcalık](https://en.wikipedia.org/wiki/Principle_of_least_privilege) güvenlik ilkeleri, veri erişimi için güvenlik ilkelerini zorlamak istediğinizde kuruluşlar için zorunlu.

Azure kaynaklarınızı değerlendirmek ve riskleri düzeltmek için görünürlük, güvenlik ekibinizle gerekir. Güvenlik ekibine, operasyonel sorumlulukları varsa, bunlar işlerini yapmak için ek izinler gerekir.

Kullanabileceğiniz [RBAC](../role-based-access-control/overview.md) kullanıcılara, gruplara ve uygulamalara belirli bir kapsamda izin vermek istiyorsanız. Rol atamasının kapsamı, bir abonelik, kaynak grubu veya tek bir kaynak olabilir.

**En iyi yöntem**: Ekibiniz içinde görevleri ayırmak ve işlerini yapmak için ihtiyaçları olan kullanıcılara sadece erişim miktarını verebilirsiniz. Herkes vermek yerine Azure abonelik veya kaynak, Kısıtlanmamış izinler belirli eylemleri yalnızca belirli bir kapsamda sağlar.
**Ayrıntı**: Kullanım [yerleşik RBAC rolleri](../role-based-access-control/built-in-roles.md) ayrıcalıkları kullanıcılara atamak için Azure.

> [!Note]
> Özel izinler, çünkü gereksiz bir karmaşıklık ve Karışıklığı önlemek için bir şey bozucu ödemeden düzeltmesi zor olan bir "eski" yapılandırması ile biriktirme, oluşturun. Kaynağa özel izinler kaçının. Bunun yerine, Kurumsal Çapta izinleri ve abonelikleri içinde izinler için kaynak gruplar için Yönetim grupları kullanın. Kullanıcı özel izinleri kaçının. Bunun yerine, erişim, Azure AD'de gruplara atayın.

**En iyi yöntem**: Güvenliği ekiplerinin değerlendirmenize ve riski Azure kaynaklarını görmek için Azure sorumlulukları erişim verin.
**Ayrıntı**: Güvenliği ekiplerinin RBAC izni [güvenlik okuyucusu](../role-based-access-control/built-in-roles.md#security-reader) rol. Kök yönetim grubu ya da kesim yönetim grubu sorumlulukları kapsamını bağlı olarak kullanabilirsiniz:

- **Kök yönetim grubu** tüm kurumsal kaynaklar için etmeden sorumlu takımlar için
- **Segment yönetim grubu** (genellikle nedeniyle yasal veya diğer kuruluş sınırları) sınırlı kapsamı olan takımlar için

**En iyi yöntem**: Doğrudan operasyonel sorumluluklara sahiptir güvenlik ekibi için uygun izinleri verin.
**Ayrıntı**: Uygun rol ataması için RBAC yerleşik rolleri gözden geçirin. Yerleşik roller, kuruluşunuzun özel ihtiyaçlarını karşılamıyorsa, oluşturabileceğiniz [Azure kaynakları için özel roller](../role-based-access-control/custom-roles.md). Yerleşik rolleri ile kullanıcılara, gruplara veya abonelik, kaynak grubu ve kaynak kapsamları hizmet sorumluları için özel roller atayabilirsiniz gibi.

**En iyi uygulamalar**: İhtiyaç duyduğunuz güvenlik rolleri için erişim verme Azure Güvenlik Merkezi. Güvenlik Merkezi hızlı bir şekilde tanımlamak ve riskleri düzeltmek güvenliği ekiplerinin sağlar.
**Ayrıntı**: Bu ihtiyaçları olan güvenlik takımlar için RBAC ekleme [Güvenlik Yöneticisi](../role-based-access-control/built-in-roles.md#security-admin) , güvenlik ilkeleri, güvenlik durumlarını görüntülemek, güvenlik ilkeleri, uyarıları görüntüleme ve öneriler düzenlemek, görüntüleyip uyarıları ve öneriler Kapat rol. Kök yönetim grubu ya da kesim yönetim grubu sorumlulukları kapsamını bağlı olarak kullanarak bunu yapabilirsiniz.

Veri erişim denetimi, RBAC kullanıcılar için gerekenden daha fazla ayrıcalık vermek gibi özelliklerini kullanarak zorlamaz kuruluşlar. Bu veri güvenliğinin aşılması için kullanıcıların vererek sahip olmamalıdır (örneğin, yüksek iş etkisi) veri türleri erişmek için açabilir.

## <a name="lower-exposure-of-privileged-accounts"></a>Ayrıcalıklı hesapların açığa çıkarılması

Güvenliğini sağlama ayrıcalıklı erişim iş varlıkların korunması için bir kritik ilk adımdır. Bilgi veya kaynakları güvenli hale getirmek için erişimi olan kişi sayısını en aza indirmek erişim sağlama kötü niyetli bir kullanıcı veya yetkili bir kullanıcı yanlışlıkla hassas kaynak etkileyen olasılığını azaltır.

Ayrıcalıklı hesaplar BT sistemlerini yöneten hesaplarıdır. Siber saldırganlar bir kuruluşun veri ve sistemlere erişim elde etmek için bu hesapları hedefler. Ayrıcalıklı erişim güvenliğini sağlamak için hesaplar ve kötü niyetli bir kullanıcının maruz kalma riskini sistemlerden yalıtmak.

Geliştirme ve siber saldırganlara karşı ayrıcalıklı erişimin güvenliğini sağlamak için bir yol haritasının izlenmesini öneririz. Microsoft Azure, Office 365 ve diğer bulut Hizmetleri, kimliklerin yönetimi ve yönetilen veya Azure AD'de bildirilen erişim güvenliğini sağlamak için ayrıntılı bir yol haritası oluşturma hakkında daha fazla bilgi için gözden [ayrıcalıklı erişimin güvenliğini sağlama karma ve bulut dağıtımları için Azure AD](../active-directory/users-groups-roles/directory-admin-roles-secure.md).

Aşağıdaki bulunan en iyi yöntemler özetlenmiştir [ayrıcalıklı erişimin güvenliğini sağlama ve karma bulut dağıtımları için Azure AD'de](../active-directory/users-groups-roles/directory-admin-roles-secure.md):

**En iyi yöntem**: Yönetebilir, denetleyebilir ve ayrıcalıklı hesaplara erişim izleyin.   
**Ayrıntı**: Açma [Azure AD Privileged Identity Management](../active-directory/privileged-identity-management/active-directory-securing-privileged-access.md). İçin Privileged Identity Management'ı etkinleştirdikten sonra bildirim alacaksınız. e-posta iletileri için ayrıcalıklı erişim rol değişiklikleri. Dizininizdeki yüksek ayrıcalıklı rollere ek kullanıcılar eklendiğinde bu bildirimleri erken uyarı sağlamak.

**En iyi yöntem**: Tüm kritik yönetici hesapları yönetilen olun Azure AD hesapları.
**Ayrıntı**: Kritik yönetici rolleri (örneğin, hotmail.com, live.com ve outlook.com gibi Microsoft hesapları) herhangi bir tüketici hesabı kaldırın.

**En iyi yöntem**: Tüm kritik yönetim rolleri, kimlik avı ve yönetici ayrıcalıkları aşmaya diğer saldırıları önlemek için yönetim görevleri için ayrı bir hesap sahip olun.
**Ayrıntı**: Atanmış yönetim görevlerini gerçekleştirmek için gerekli ayrıcalıklara sahip ayrı bir yönetim hesabı oluşturun. Microsoft Office 365 e-posta veya rastgele Web'e gözatma gibi günlük üretkenlik araçları için bu yönetim hesaplarına kullanımını engelleyin.

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

Atanan veya genel yönetici rolü için uygun olan hesaplarını değerlendirin. Kullanarak herhangi bir salt bulut hesapları görmüyorsanız, `*.onmicrosoft.com` (hedeflenen) Acil Durum erişim için etki alanı oluşturun. Daha fazla bilgi için [Azure AD'de Acil Durum erişimi yönetici hesaplarını yönetme](../active-directory/users-groups-roles/directory-emergency-access.md).

**En iyi yöntem**: Bir Acil bir yerde "Acil Durum" bir işlem var.
**Ayrıntı**: Bağlantısındaki [ayrıcalıklı erişimin güvenliğini sağlama ve karma bulut dağıtımları için Azure AD'de](../active-directory/users-groups-roles/directory-admin-roles-secure.md).

**En iyi yöntem**: Çok faktörlü kimlik doğrulaması gerektiren veya parola olmadan tüm kritik yönetici hesabı (tercih edilir) gerektirir.
**Ayrıntı**: Kullanım [Microsoft Authenticator uygulamasını](../active-directory/authentication/howto-authentication-phone-sign-in.md) herhangi bir Azure AD hesabı için parola kullanmadan oturum açmak için. Gibi [Windows iş için Hello](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification), Microsoft Authenticator, biyometrik kimlik doğrulaması veya PIN kullanır ve bir cihaza bağlı bir kullanıcı kimlik bilgisi etkinleştirmek için anahtar tabanlı kimlik doğrulaması kullanır.

Oturum açma bir veya daha fazla Azure AD yönetim rolleri kalıcı olarak atanan tüm bireysel kullanıcılar için Azure multi-Factor Authentication iste: Genel yönetici, ayrıcalıklı Rol Yöneticisi, Exchange Online yönetici ve SharePoint Online yönetici. Etkinleştirme [yönetici hesaplarınız için multi-Factor Authentication](../active-directory/authentication/howto-mfa-userstates.md) ve yönetici hesabı kullanıcılarını kaydolduğundan emin olun.

**En iyi yöntem**: Kritik yönetici hesapları için üretim görevler (örneğin, göz atma ve e-posta) burada izin verilmeyen bir yönetici iş istasyonunda vardır. Bu yönetici hesaplarınızdan göz atma ve e-posta ve önemli bir olayı riskini önemli ölçüde daha düşük saldırgan vektörlerinden korumak.
**Ayrıntı**: Bir yönetici iş istasyonunda kullanın. İş İstasyonu güvenlik düzeyini seçin:

- Yüksek oranda güvenli verimlilik cihazları gözatma için Gelişmiş Güvenlik ve diğer üretkenlik görevleri sağlar.
- [Ayrıcalıklı erişim iş istasyonları (Paw'lar)](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations) internet saldırıları ve tehdit vektörlerini hassas görevler için korunan ayrılmış bir işletim sistemi sağlar.

**En iyi yöntem**: Çalışanlar, kuruluşunuzun ayrıldığında yönetici hesapları sağlamasını kaldırın.
**Ayrıntı**: Bir işlem devre dışı bırakır veya çalışanlar, kuruluşunuzun ayrıldığında yönetici hesapları silen bir yerde sahip.

**En iyi yöntem**: Düzenli olarak yönetici hesapları, geçerli saldırı teknikleri kullanarak test edin.
**Ayrıntı**: Office 365 saldırı simülatörü veya üçüncü taraf sunan gerçekçi saldırı senaryoları, kuruluşunuzda çalıştırmak kullanın. Bu, gerçek bir saldırı gerçekleşmeden önce güvenlik açığı bulunan kullanıcılar bulmanıza yardımcı olabilir.

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

## <a name="use-azure-ad-for-storage-authentication"></a>Depolama kimlik doğrulaması için Azure AD'yi kullanın
[Azure depolama](../storage/common/storage-auth-aad.md) Blob Depolama ve kuyruk depolama için kimlik doğrulaması ve Azure AD ile yetkilendirme destekler. Azure AD kimlik doğrulaması ile Azure rol tabanlı erişim denetimi, kullanıcılara, gruplara veya tek bir blob kapsayıcısı veya sıra kapsamı aşağı uygulamalar için belirli izinler vermek için kullanabilirsiniz.

Kullanmanızı öneririz [depolama erişimi kimlik doğrulaması için Azure AD](https://azure.microsoft.com/blog/azure-storage-support-for-azure-ad-based-access-control-now-generally-available/).

## <a name="next-step"></a>Sonraki adım

Bkz: [Azure güvenlik en iyi uygulamaları ve desenleri](security-best-practices-and-patterns.md) kullanmak üzere daha fazla güvenlik için en iyi yöntemler, tasarlama, dağıtma ve Azure'ı kullanarak bulut çözümlerinizi yönetme.
