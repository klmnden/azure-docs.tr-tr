---
title: Azure AD'de Acil Durum erişimi yönetici hesaplarını yönetin | Microsoft Docs
description: Bu makalede, kuruluşların mevcut Azure Active Directory ortamında ayrıcalıklı erişimi kısıtlamasına yardımcı olmak için Acil Durum erişim hesapları kullanmayı açıklar.
services: active-directory
author: markwahl-msft
ms.author: billmath
ms.date: 12/13/2017
ms.topic: article-type-from-white-list
ms.service: active-directory
ms.workload: identity
ms.custom: it-pro
ms.reviewer: markwahl-msft
ms.openlocfilehash: 185f6730babe077332be9f054ba338ff48295eca
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37450271"
---
# <a name="manage-emergency-access-administrative-accounts-in-azure-ad"></a>Azure AD'de Acil Durum erişimi yönetici hesaplarını yönetme 

Çoğu günlük etkinlikler için *genel yönetici* hakları, kullanıcılarınızın gerekmiyor. Sahip olması gereken daha yüksek izinler gerektiren bir görevi gerçekleştirdikleri yanlışlıkla çünkü kullanıcı rolü için kalıcı olarak atanmamalıdır. Genel yönetici olarak görev yapacak kullanıcıları ihtiyacınız olmadığında, Azure Active Directory (Azure AD) Privileged Identity Management (PIM), kendi hesabı veya başka bir yönetici hesabı kullanarak rol atamasını etkinleştirmelisiniz.

Kullanıcıların yönetici erişim haklarını kendileri için ayırdığınız yanı sıra, oturum ne var olan tek bir kullanıcının hesabı olarak etkinleştirmek için Azure AD kiracınızın Yönetim dışı yanlışlıkla kilitlenen önlemek ihtiyacınız bir Yönetici. İki veya daha fazla depolayarak yönetici erişimi yanlışlıkla eksikliği etkisini en aza indirebileceğiniz *Acil Durum erişim hesapları* kiracınızdaki.

Acil Durum erişim hesapları, kuruluşların mevcut Azure Active Directory ortamında ayrıcalıklı erişimi kısıtlamasına yardımcı olabilir. Bu tür hesaplar son derece ayrıcalıklı olan ve belirli kişilere atanmaz. Acil Durum erişim hesapları, Acil Durum ya da 'Acil Durum' senaryoları, normal yönetim hesapları kullanılamaz olduğu durumlarda sınırlıdır. Kuruluşlar, bir hedef Acil Durum hesabının kullanım sırasında gereklidir o zaman yalnızca kısıtlama sürdürmeniz gerekir.

Bir kuruluş, aşağıdaki durumlarda bir Acil Durum erişim hesabı kullanmanız gerekebilir:

 - Kullanıcı hesaplarını Federal ve Federasyon bir hücre ağ kesintisi veya kimlik sağlayıcısı kesinti nedeniyle şu anda kullanılamıyor. Örneğin, ortamınızda kimlik sağlayıcısı konağı aşağıya indi, kullanıcıları Azure AD kimlik sağlayıcısına yönlendirir, oturum açamıyor olabilir. 
 - Yöneticiler Azure multi-Factor Authentication kaydedilen ve tek tek tüm cihazlardan kullanılamaz. Kullanıcıların multi-Factor Authentication'ın bir rolü etkinleştirmek için tamamlayamıyor olabilir. Örneğin, bir hücre ağ kesintisi bunları yanıtlama telefon görüşmeleri veya kısa mesaj, kayıtlı cihazlarını yalnızca iki kimlik doğrulama mekanizması almasını engelliyor. 
 - En son genel yönetici erişimine sahip bir kişi kuruluştan ayrılan. Azure AD'ye son engeller *genel yönetici* silinmesini gelen hesabı, ancak engellemez silinmesini öğesinden hesap veya şirket devre dışı. Her iki durumda, kuruluş hesabı kurtarmanız mümkün hale getirebilirsiniz.

## <a name="initial-configuration"></a>İlk Yapılandırma

İki veya daha fazla Acil Durum erişim hesapları oluşturun. Bunlar kullanan yalnızca bulut hesapları olmalıdır \*. onmicrosoft.com etki alanı olan ve olmayan Federasyon veya bir şirket içi ortamdan eşitlenen. 

Hesapları kuruluştaki herhangi bir kullanıcı ile ilişkilendirilmemelidir. Bu hesaplar için kimlik bilgilerini yalnızca bunları kullanma yetkisi olan kişiler için güvenli ve bilinen kalmasını sağlamak kuruluşların gerekir. 

> [!NOTE]
> Genellikle bir hesap parolası bir Acil Durum erişim hesabı için iki veya üç parçalara ayrılmış, kağıda ayrı parçaları üzerinde yazılan ve güvenli, ayrı konumlardaki güvenli, ateşe dayanıklı kasalar depolanır. 
>
> Acil Durum erişim hesapları, tüm çalışan tarafından sağlanan cep telefonları ile bağlı değil, bu seyahat çalışanları veya diğer çalışan özel kimlik bilgileri ile donanım belirteçleri emin olun. Bu önlem, kimlik bilgisi gerektiğinde tek bir çalışan ulaşılamaz olduğu örnekleri kapsar. 

### <a name="initial-configuration-with-permanent-assignments"></a>İlk yapılandırma ile kalıcı atamaları

Bir seçenek olan kullanıcılar üyeleri kalıcı hale getirmek üzere *genel yönetici* rol. Bu seçenek, Azure AD Premium P2 abonelikleri olmayan kuruluşlar için uygun olacaktır.

Güvenliği aşılmış bir paroladan kaynaklanan bir saldırı riskini azaltmak için Azure AD, tüm bireysel kullanıcılar için çok faktörlü kimlik doğrulaması gerektiren önerir. Bu grup yöneticileri ve diğer tüm (örneğin, finansal görevlileri), gizliliği tehlikeye giren hesap önemli bir etkisi yoktur içermelidir. 

Bununla birlikte, paylaşılan cihazlar, kuruluşunuzun sahip değilse, multi-Factor Authentication için bu Acil Durum erişim hesapları mümkün olmayabilir. Gerektirmek için koşullu erişim ilkesi yapılandırıyorsanız [her yöneticinin multi-Factor Authentication kaydını](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-user-states) Azure AD'de ve diğer bağlı bir hizmet (SaaS) uygulamaları olarak yazılım, ilke yapılandırma gerekebilir Acil Durum erişim hesapları bu gereksinimden hariç tutmak için özel durumlar.

### <a name="initial-configuration-with-approvals"></a>İlk yapılandırma ile onaylar

Kullanıcılarınız için uygun ve etkinleştirmek için onaylayan yapılandırmak için başka bir seçenektir *genel yönetici* rol. Bu seçenek, kuruluşunuzun Azure AD Premium P2 aboneliği gerektirir. Ayrıca, birden çok kişiye ve ağ ortamı arasında paylaşılan kullanım için uygun olan bir çok faktörlü kimlik doğrulaması seçeneği de gerekir. Bu gereksinimler olduğu etkinleştirme *genel yönetici* rolü, kullanıcıların daha önce çok faktörlü kimlik doğrulaması gerçekleştirdikten gerektirir. Daha fazla bilgi için [Azure AD Privileged Identity Management içinde çok faktörlü kimlik doğrulaması isteme](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-how-to-require-mfa).

Kişisel cihazlar için Acil Durum erişim hesapları ile ilişkili bir multi-Factor Authentication kullanarak önermiyoruz. Gerçek acil durumlarda, kişisel cihaz olan bir multi-Factor Authentication kayıtlı bir cihaz erişmesi kişiye olmayabilir. 

Ayrıca, tehdit Manzarası göz önünde bulundurun. Örneğin, Acil Durum doğal afetler gibi öngörülemeyen bir durumda, bu sırada bir cep telefonu veya diğer ağlara kullanılamayabilir ortaya çıkabilir. Tüm kayıtlı cihazları olan Azure AD ile iletişim kuran birden fazla yol bilinen ve güvenli bir konumda kalmasını sağlamak önemlidir.

## <a name="ongoing-monitoring"></a>Devam eden izleme

İzleyici [Azure AD oturum açma ve denetim günlüklerini](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins) oturum açma işlemleri için ve Acil Durum erişim hesapları etkinliğinden denetleyebilirsiniz. Normalde bu hesapların açarken olması değil ve onları kullanmanız açısından anormal ve güvenlik araştırma gerektiren büyük olasılıkla, bu nedenle, değişiklikler yapmak değil.

## <a name="account-check-validation-must-occur-at-regular-intervals"></a>Hesap denetimini doğrulama düzenli aralıklarla gerçekleşmelidir.

Hesabı doğrulamak için en azından aşağıdaki adımları gerçekleştirin:
- Her 90 günde.
- BT ekibi, işi değişiklik, bir ayrılma ya da yeni bir işe alma gibi yeni bir değişiklik olduğunda.
- Azure AD aboneliklerini kuruluştaki zaman değişti.

Acil Durum erişim hesapları kullanmak için personel eğitmek için aşağıdakileri yapın:

* Personel güvenlik izleme hesabı onay etkinliği devam ediyor farkında olduğundan emin olun.
* Bulut kullanıcı hesapları oturum açın ve kendi rollerini etkinleştirebilmelerini ve Acil sırasında şu adımları gerçekleştirmeniz gerekebilir kullanıcılar üzerindeki işlem eğitilir doğrulayın.
* Multi-Factor Authentication ya da tek tek her kullanıcının cihazına veya kişisel ayrıntılar için Self Servis parola sıfırlama (SSPR) isteneceği değil emin olun. 
* Hesapları için multi-Factor Authentication için rol etkinleştirme sırasında kullanım için bir cihaz kaydedilirse cihazın Acil sırasında kullanmak için gereken tüm yöneticiler için erişilebilir olduğundan emin olun. Ayrıca, ortak bir hata modu paylaşmayın en az iki mekanizmalar aracılığıyla cihazın kayıtlı olup olmadığını doğrulayın. Örneğin, cihaz özelliği'nin kablosuz ağ hem hücresi sağlayıcısını Ağ üzerinden İnternet'e iletişim kurabilir.
* Hesap kimlik bilgilerini güncelleştirin.

## <a name="next-steps"></a>Sonraki adımlar
- [Bulut tabanlı kullanıcı ekleme](../fundamentals/add-users-azure-active-directory.md) ve [yeni kullanıcı genel Yönetici rolüne atayın](../fundamentals/active-directory-users-assign-role-azure-portal.md).
- [Azure Active Directory Premium'a kaydolma](../fundamentals/active-directory-get-started-premium.md), zaten kaydolmuş olmasanız.
- [Yönetici olarak atanan bireysel kullanıcılar için Azure multi-Factor Authentication iste](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-user-states).
- [Office 365'te genel Yöneticiler için ek koruma olanaklarını yapılandırma](https://support.office.com/article/Protect-your-Office-365-global-administrator-accounts-6b4ded77-ac8d-42ed-8606-c014fd947560), Office 365 kullanıyorsanız.
- [Genel Yöneticiler, erişim gözden geçirmesi gerçekleştirme](../active-directory-privileged-identity-management-how-to-start-security-review.md) ve [varolan genel yöneticileri daha özel yönetici rolleri için geçiş](directory-assign-admin-roles.md).


