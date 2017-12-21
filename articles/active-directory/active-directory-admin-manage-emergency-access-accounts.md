---
title: "Azure AD'de Acil Durum erişimi yönetim hesaplarını yönetme | Microsoft Docs"
description: "Acil Durum erişimi hesapları mevcut Azure Active Directory ortamında ayrıcalıklı erişimi kısıtlama hızlandırmasına yardımcı olmak için nasıl kullanılacağını açıklar."
services: active-directory
keywords: "Verme ekleyin veya SEO uzmanınıza danışmanlık olmadan anahtar sözcükleri düzenleyin."
author: markwahl-msft
ms.author: billmath
ms.date: 12/13/2017
ms.topic: article-type-from-white-list
ms.service: active-directory
ms.workload: identity
ms.custom: it-pro
ms.reviewer: markwahl-msft
ms.openlocfilehash: 75ea8e445c890e7af5ab14f4fc4c53cfb1af4568
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="managing-emergency-access-administrative-accounts-in-azure-ad"></a>Azure AD'de Acil Durum erişimi yönetici hesaplarını yönetme 

Çoğu günlük etkinlikleri için genel yönetici rolü hakları gerekli değildir.  Kullanıcı yanlışlıkla gerekenden daha yüksek izinleri olan bir task gerçekleştirebilir gibi kullanıcı rolü için kalıcı olarak atanmamalıdır. Bir kullanıcı genel yönetici olarak hareket gerektiğinde, Azure AD PIM, ya da kendi hesabını veya alternatif bir yönetici hesabı kullanarak rol ataması etkinleştirmeniz.

Yönetimsel erişim haklarını kendileri için ayırdığınız kullanıcılar ek olarak, bunlar burada bunlar yanlışlıkla kendi Azure AD kiracısı imzalamak için kendi sorunları nedeniyle Yönetim dışı kilitlenmiş bir durum içine almazsanız emin olmak hazırlamak için müşteriler gerekir. içinde veya bir yönetici olarak var olan tek bir kullanıcının hesabını etkinleştirebilir.  Kendi Kiracı iki veya daha fazla depolama aracılığıyla yönetimsel erişim yanlışlıkla eksikliği etkisini en aza indirebileceğiniz *Acil erişim hesapları*.

Acil erişim hesapları, kuruluşların mevcut Azure Active Directory ortamında ayrıcalıklı erişimi kısıtlama yardımcı olur. Bu hesapları yüksek ayrıcalıklı ve belirli kişilere atanmaz. Acil Durum erişimi hesaplarını burada normal yönetim hesapları kullanılamaz 'Acil Durum' senaryoları için Acil Durum sınırlıdır.  Kuruluşlar, denetleme ve Acil Durum hesabın kullanımı için gereklidir o zaman yalnızca ile AIM emin olmalısınız.

İçinde bir kuruluş yapmanız gerekebilen senaryoları bir Acil Durum erişimi hesabını kullan:

 - Kullanıcı hesaplarını federe ve Federasyon nedeni bir ağ kesintisi veya kesinti kimlik sağlayıcı kullanılamaz.  Müşteri'nin ortamda kimlik sağlayıcısı ana bilgisayarı koptu, örneğin, daha sonra bir kullanıcı Azure AD, kimlik sağlayıcısı yönlendirildiklerinde oturum açabilir olmayabilir. 
 - Yöneticileri MFA ile kaydedilen ve tüm tek tek cihazları kullanılamaz.  Kullanıcılar bir rol örneği için bir hücre ağ kesintisi telefon aramaları yanıt veya metin iletileri almasına yapamamasına kullanıcıların engel olabilir ve yalnızca mekanizması edildi etkinleştirmek için tam çok faktörlü kimlik doğrulaması için erişilemiyor mümkündür, bunlar kendi cihaz için kayıtlı. 
 - Genel yönetici erişimi olan son kişiye kuruluş kalmadı.  Azure AD silinmesini son genel yönetici hesabı engeller, ancak silinen gelen hesabı veya devre dışı şirket içi, kuruluşun bu hesabı kurtarmak mümkün olduğu bir durumda baştaki engellemez.

## <a name="initial-configuration"></a>İlk Yapılandırma

İki veya daha fazla Acil erişim hesapları oluşturun.  Bunlar kullanarak yalnızca bulut hesapları olmalıdır *. değil federe veya bir şirket içi ortamından eşitlenen onmicrosoft.com etki alanı.  

Bunlar kuruluştaki herhangi bir kullanıcı için ilişkili olmamalıdır.  Kuruluşlar, bu hesaplar için kimlik bilgilerini güvenli ve bilinen bunları kullanmaya yetkili kişiler tutulduğundan emin olmak gerekir. 

Not: Genellikle, bir Acil erişim hesabı için hesap parolası iki veya üç bölümlere ayrılmış kağıt ayrı parçalarını yazılmış ve güvenli ayrı konumlardaki güvenli, ateşe dayanıklı kasalar depolanır. Bu ayrı ayrı çalışan aynı anda ulaşılamaz olması durumunda Acil Durum erişimi hesaplarınızı tüm çalışan sağlanan cep telefonları ile bağlı değil, tek tek çalışanlar veya diğer çalışan özgü kimlik bilgileri ile bu seyahat donanım belirteçleri emin olun kimlik bilgisi gereklidir. 

### <a name="initial-configuration-with-permanent-assignments"></a>İlk yapılandırma ile kalıcı atamaları

Bu kullanıcıların kalıcı genel Yönetici rolüne üye olarak atamak bir seçenektir.  Bu, Azure AD Premium P2 abonelikleri olmayan kuruluşlar için uygun olacaktır.

Azure AD (örneğin finansal görevlileri) Yöneticiler ve kendi hesabı önemli bir etkisi olması gereken tüm diğer kullanıcılar da dahil olmak üzere, tek tek tüm kullanıcılar için çok faktörlü kimlik doğrulaması (MFA) tehlikeye gerektiren önerir. Bu, güvenliği aşılmış bir parola nedeniyle bir saldırı riskini azaltır. Kuruluşunuz paylaşılan cihazlar yoksa ancak, ardından MFA bu Acil Durum erişimi hesaplar için mümkün olmayabilir.  Bir koşullu erişim ilkesi gerektirecek şekilde yapılandırıyorsanız [her Yönetim için MFA kayıt](https://docs.microsoft.com/en-us/azure/multi-factor-authentication/multi-factor-authentication-get-started-user-states) Azure AD için ve diğer SaaS uygulamaları bağlı, Acil Durum erişimi hesapları öğesinden dışarıda bırakmak için ilke Dışlamalar yapılandırmanız gerekebilir gereksinimi.

### <a name="initial-configuration-with-approvals"></a>İlk yapılandırma ile onaylar

Bu kullanıcılar uygun olarak ve bunları onaylayanlar genel yönetici rolünü etkinleştirmek için yapılandırma başka bir seçenektir.  Bu kuruluşun birden çok kişiler ve ağ ortamı arasında paylaşılan kullanmak için uygun bir MFA seçeneği yanı sıra, Azure AD Premium P2 abonelikleri sahip olması gerekir.  Etkinleştirme genel Yönetici rolüne kullanıcıya MFA daha önce gerçekleştirdiğiniz gerektirdiğinden budur.  Hakkında daha fazla bilgi edinin [Azure AD Privileged Identity Management MFA nasıl gerektirilir](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-privileged-identity-management-how-to-require-mfa).

Kişisel cihazlar ile ilişkili MFA kullanarak önerilmez Acil erişim hesapları için gerek duyduğu bir gerçek acil bir kişiye içinde kişisel cihaz bulunduran bir MFA kayıtlı bir cihaza erişimi olmayabileceğinden.  Ayrıca nedeniyle öngörülemeyen durumda cep telefonu veya diğer ağlara doğal afet Acil Durum sırasında kullanılabilir durumda değilse tehdit göz önünde bulundurun.  Bu nedenle, tüm kayıtlı cihazlarda Azure AD ile iletişim birden çok yol var bilinen ve güvenli bir konumda kalmasını sağlamak önemlidir.

## <a name="ongoing-monitoring"></a>Devam eden izleme

İzleyici [Azure AD oturum açma ve Denetim günlükleri](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-activity-sign-ins) hiçbir oturum açma işlemleri için ve denetim Acil erişim hesapları etkinliğinden.  Normalde bu hesapların oturum olması ve bunları kullanımını anormal ve güvenlik incelenmesi büyük olasılıkla olacak şekilde, değişiklik değil.

## <a name="account-check-validation-must-occur-at-regular-intervals"></a>Hesap onay doğrulama düzenli aralıklarla olmalıdır

En az aşağıdaki adımları gerçekleştirin:
 - Her 90 günde
 - BT personeli – yeni bir değişiklik olduğunda bir iş, ayrılma veya değiştirme yeni işe alma
 - Kuruluşunuzdaki Azure AD aboneliklerini değiştiği

Personel Acil Durum erişimi hesaplarını kullanacak şekilde nasıl eğitilmiş sağlamak için:

1.  Güvenlik İzleme personel hesap onay etkinliği ediyor farkında olduğundan emin olun.
2.  Bulut kullanıcı hesapları oturum açabilir ve kendi rolleri etkinleştirmek ve Acil sırasında şu adımları gerçekleştirmeniz gereken kullanıcılar için işlemi üzerinde eğitilmiş olup doğrulayın.
3.  Bunlar MFA veya SSPR herhangi tek bir kullanıcının aygıt veya kişisel ayrıntılar kaydedilmeyen emin olun.  
4. Hesapları MFA için rol etkinleştirme sırasında kullanmak için bir aygıt için kayıtlı cihaz Acil sırasında kullanmanız gerekebilir tüm yöneticiler için erişilebilir olduğundan emin olun.  Aynı zamanda aygıt yaygın bir başarısızlık modu paylaşmaz en az iki mekanizma kayıtlı olduğunu doğrulayın (örneğin, aygıt hem tesis'in kablosuz ağ yoluyla yanı sıra bir hücre sağlayıcısı ağ üzerinden internet iletişim).
5.  Kimlik bilgilerini güncelleştirin.

## <a name="next-steps"></a>Sonraki adımlar
- [Bulut tabanlı kullanıcı ekleme](add-users-azure-active-directory.md) ve [yeni kullanıcı için genel yönetici rolü atama](active-directory-users-assign-role-azure-portal.md)
- [Azure Active Directory Premium için kaydolun](active-directory-get-started-premium.md), henüz yapmadıysanız
- [Yönetici olarak atanan bireysel kullanıcılar için Azure MFA gerekir](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-user-states)
- [Office 365'te genel Yöneticiler için ek koruma yapılandırma](https://support.office.com/article/Protect-your-Office-365-global-administrator-accounts-6b4ded77-ac8d-42ed-8606-c014fd947560), Office 365 kullanıyorsanız
- [Bir erişim gözden geçirme ve genel yöneticilerin gerçekleştirmek](active-directory-privileged-identity-management-how-to-start-security-review.md) ve [daha özel yönetici rolleri için var olan genel Yöneticiler geçiş](active-directory-assign-admin-roles-azure-portal.md)

