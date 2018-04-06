---
title: Azure AD'de Acil Durum erişimi yönetici hesaplarını yönetme | Microsoft Docs
description: Bu makalede, kuruluşların mevcut Azure Active Directory ortamında ayrıcalıklı erişimi kısıtlama yardımcı olmak için Acil erişim hesapları kullanmayı açıklar.
services: active-directory
keywords: SEO uzmanınıza danışmadan anahtar sözcük eklemeyin veya anahtar sözcükleri düzenlemeyin.
author: markwahl-msft
ms.author: billmath
ms.date: 12/13/2017
ms.topic: article-type-from-white-list
ms.service: active-directory
ms.workload: identity
ms.custom: it-pro
ms.reviewer: markwahl-msft
ms.openlocfilehash: 1545fb9a89794a74efbb855c4480040973c3308e
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="manage-emergency-access-administrative-accounts-in-azure-ad"></a>Azure AD'de Acil Durum erişimi yönetici hesaplarını yönetin 

Çoğu günlük etkinlikler için *genel yönetici* hakları kullanıcılarınız tarafından gerekmiyor. Sahip olması gereken daha yüksek izinleri gerektiren bir görevi gerçekleştirdikleri yanlışlıkla çünkü kullanıcı rolü için kalıcı olarak atanmamalıdır. Genel yönetici olarak davranmak üzere ihtiyacınız yoksa, Azure Active Directory (Azure AD) Privileged Identity Management (PIM), kendi hesabını ya da başka bir yönetici hesabı kullanarak rol ataması etkinleştirmelisiniz.

Oturum ne var olan tek bir kullanıcının hesabı olarak etkinleştirmek için Azure AD kiracınıza Yönetim dışı yanlışlıkla kilitleniyor önlemek gerek kullanıcıların yönetimsel erişim haklarını kendileri için ayırdığınız ek olarak, bir Yönetici. İki veya daha fazla depolayarak yönetimsel erişim yanlışlıkla eksikliği etkisini en aza indirebileceğiniz *Acil erişim hesapları* kiracınızda.

Acil erişim hesapları, kuruluşların mevcut Azure Active Directory ortamında ayrıcalıklı erişimi kısıtlama yardımcı olabilir. Bu tür hesaplara yüksek ayrıcalıklı ve belirli kişilere atanmaz. Acil erişim hesapları, Acil Durum ya da 'Acil Durum' senaryoları, normal yönetim hesapları kullanılamaz olduğu durumlarda sınırlıdır. Kuruluşlar Acil Durum hesabın kullanım sırasında gereklidir o zaman yalnızca kısıtlama bir hedef bulundurmanız gerekir.

Bir kuruluş, aşağıdaki durumlarda bir Acil Durum erişimi hesabı kullanmanız gerekebilir:

 - Kullanıcı hesaplarını federe ve Federasyon bir hücre ağ kesintisi veya bir kimlik sağlayıcısı kesintisi nedeniyle şu anda kullanılamıyor. Örneğin, kimlik sağlayıcısı ana bilgisayarı, ortamınızdaki koptu, kullanıcıların Azure AD, kimlik sağlayıcısı yönlendirildiklerinde oturum açmak mümkün olmayabilir. 
 - Yöneticiler Azure multi-Factor Authentication ile kaydedilen ve tek tek tüm cihazları kullanılamaz. Kullanıcılar bir rolü etkinleştirmek için çok faktörlü kimlik doğrulamasını tamamlamak mümkün olmayabilir. Örneğin, bir hücre ağ kesintisi onları telefon aramaları yanıtlama veya metin iletileri, kayıtlı cihazlarını yalnızca iki kimlik doğrulama mekanizması almasını engelliyor. 
 - En son genel yönetici erişimi olan kişi kuruluş ayrıldı. Azure AD engeller son *genel yönetici* siliniyor gelen hesap, ancak engellemez siliniyor gelen hesabı ya da devre dışı şirket. Her iki durum kuruluş hesabı kurtarmanız mümkün hale getirebilirsiniz.

## <a name="initial-configuration"></a>İlk Yapılandırma

İki veya daha fazla Acil erişim hesapları oluşturun. Bunlar kullanmak yalnızca bulut hesapları olmalıdır \*. onmicrosoft.com etki alanı ve, değil federe veya bir şirket içi ortamından eşitlenir. 

Hesaplar kuruluştaki herhangi bir kullanıcı ile ilişkili olmamalıdır. Kuruluşların bu hesaplar için kimlik bilgileri yalnızca bunları kullanmaya yetkili kişiler için güvenli ve bilinen tutulmasını gerekir. 

> [!NOTE]
> Genellikle bir hesap parolası Acil erişim hesabı için iki veya üç bölümlere ayrılmış, kağıt ayrı parçalarını yazılmış ve güvenli, ayrı konumlardaki güvenli, ateşe dayanıklı kasalar depolanır. 
>
> Acil Durum erişimi hesaplarınızı tüm çalışan sağlanan cep telefonları ile bağlı değil, tek tek çalışanlar veya diğer çalışan özgü kimlik bilgileri ile bu seyahat donanım belirteçleri emin olun. Bu önlem kimlik bilgisi gerektiğinde tek bir çalışan ulaşılamaz olduğu örnekleri ele alınmaktadır. 

### <a name="initial-configuration-with-permanent-assignments"></a>İlk yapılandırma ile kalıcı atamaları

Bir seçenek olan kullanıcıların kalıcı üyesi olmak için *genel yönetici* rol. Bu seçenek, Azure AD Premium P2 aboneliğiniz yok kuruluşlar için uygun olabilir.

Güvenliği aşılmış bir paroladan kaynaklanan bir saldırı riskini azaltmak için tüm tek tek kullanıcılar için çok faktörlü kimlik doğrulaması gerektiren Azure AD önerir. Bu grup, Yöneticiler ve diğerlerinin tümü (örneğin, finansal görevlileri) güvenliği aşılmış hesabı önemli bir etkisi yoktur eklemeniz gerekir. 

Ancak, kuruluşunuz paylaşılan cihazlar yoksa, çok faktörlü kimlik doğrulaması için bu Acil erişim hesapları mümkün olmayabilir. Bir koşullu erişim ilkesi gerektirecek şekilde yapılandırıyorsanız [her Yönetim için çok faktörlü kimlik doğrulaması kayıt](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-user-states) Azure AD için ve diğer bağlı yazılım hizmet (SaaS) uygulamaları olarak ihtiyacınız olabilecek ilkesini yapılandırmak Bu gereksinimden Acil erişim hesapları dışlamak için dışarıda bırakılacak.

### <a name="initial-configuration-with-approvals"></a>İlk yapılandırma ile onaylar

Kullanıcılarınız için uygun ve etkinleştirmek için onaylayanlar yapılandırmak için başka bir seçenektir *genel yönetici* rol. Bu seçenek, kuruluşunuzun Azure AD Premium P2 aboneliklere sahip olması gerekir. Ayrıca, birden çok kişiler ve ağ ortamı arasında paylaşılan kullanım için uygun bir çok faktörlü kimlik doğrulaması seçeneği de gerekir. Bu gereksinimleri olduklarından etkinleştirmesi *genel yönetici* rolü daha önce çok faktörlü kimlik doğrulaması gerçekleştirmiş kullanıcıların gerektirir. Daha fazla bilgi için bkz: [Azure AD Privileged Identity Management çok faktörlü kimlik doğrulaması zorunlu kılma](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-how-to-require-mfa).

Acil erişim hesapları için kişisel aygıtları ile ilişkili multi-Factor Authentication kullanarak önermiyoruz. Gerçek acil durumlarda kişisel cihaz sahip bir çok faktörlü kimlik doğrulaması kayıtlı bir cihaz erişmesi gereken kişi olmayabilir. 

Ayrıca, tehdit göz önünde bulundurun. Örneğin, Acil Durum doğal afet gibi öngörülemeyen bir koşul, hangi sırasında cep telefonu veya diğer ağlara kullanılamayabilir ortaya çıkabilir. Tüm kayıtlı cihazlarda Azure AD ile iletişim birden çok yol var bilinen ve güvenli bir konumda kalmasını sağlamak önemlidir.

## <a name="ongoing-monitoring"></a>Devam eden izleme

İzleyici [Azure AD oturum açma ve denetim günlüklerini](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins) hiçbir oturum açma işlemleri için ve Denetim erişim Acil Durum hesapları etkinliğinden. Normalde bu hesapların oturum olması ve bunları kullanımını anormal ve güvenlik incelenmesi büyük olasılıkla olacak şekilde, değişiklik değil.

## <a name="account-check-validation-must-occur-at-regular-intervals"></a>Hesap denetimini doğrulama düzenli aralıklarla olmalıdır

Hesabı doğrulamak için en azından aşağıdaki adımları gerçekleştirin:
- Her 90 günde.
- Bir iş değişikliği, bir ayrılma veya yeni bir işe alma gibi BT personeli, son zamanlarda bir değişiklik olduğunda.
- Kuruluşunuzdaki Azure AD aboneliklerini ne zaman değiştirilmiştir.

Acil Durum erişimi hesaplarını kullanma personel eğitmek için aşağıdakileri yapın:

* Personel Güvenliği İzleme hesabı onay etkinliği ediyor farkında olduğundan emin olun.
* Bulut kullanıcı hesapları oturum açabilir ve kendi rolleri etkinleştirin ve işlemi sırasında Acil adımları gerçekleştirmeniz gerekebilir kullanıcılar eğitilmiş olup doğrulayın.
* Bunlar çok faktörlü kimlik doğrulaması veya Self Servis parola sıfırlama (SSPR) herhangi tek bir kullanıcının aygıt veya kişisel ayrıntılar kaydolmadınız emin olun. 
* Hesapları rol etkinleştirme sırasında kullanılacak bir aygıtı için çok faktörlü kimlik doğrulama için kaydedilen cihaz Acil sırasında kullanmak için gerekebilecek tüm yöneticiler için erişilebilir durumda olduğundan emin olun. Ayrıca cihaz yaygın bir başarısızlık modu paylaşmaz en az iki mekanizma kayıtlı olduğunu doğrulayın. Örneğin, aygıt tesis'in kablosuz ağ ve bir hücre sağlayıcısı ağ üzerinden internet iletişim kurabilir.
* Hesap kimlik bilgilerini güncelleştirin.

## <a name="next-steps"></a>Sonraki adımlar
- [Bulut tabanlı kullanıcı ekleme](add-users-azure-active-directory.md) ve [yeni kullanıcı genel Yönetici rolüne atamak](active-directory-users-assign-role-azure-portal.md).
- [Azure Active Directory Premium için kaydolun](active-directory-get-started-premium.md), zaten kaydolmuş olmasanız.
- [Yönetici olarak atanan tek tek kullanıcılar Azure multi-Factor Authentication iste](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-user-states).
- [Office 365'te genel Yöneticiler için ek koruma yapılandırma](https://support.office.com/article/Protect-your-Office-365-global-administrator-accounts-6b4ded77-ac8d-42ed-8606-c014fd947560), Office 365 kullanıyorsanız.
- [Bir erişim gözden geçirme ve genel yöneticilerin gerçekleştirmek](active-directory-privileged-identity-management-how-to-start-security-review.md) ve [daha özel yönetici rolleri için var olan genel Yöneticiler geçiş](active-directory-assign-admin-roles-azure-portal.md).


