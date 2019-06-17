---
title: Acil Durum erişimi yönetici hesaplarını - Azure Active Directory yönetme | Microsoft Docs
description: Bu makalede, Azure Active Directory (Azure AD) kiracınızın dışında yanlışlıkla kilitlenen önlemeye yardımcı olmak için Acil Durum erişim hesapları kullanmayı açıklar.
services: active-directory
author: markwahl-msft
ms.author: curtand
ms.date: 03/19/2019
ms.topic: conceptual
ms.service: active-directory
ms.subservice: users-groups-roles
ms.workload: identity
ms.custom: it-pro
ms.reviewer: markwahl-msft
ms.collection: M365-identity-device-management
ms.openlocfilehash: 42de060d81539030ef1970e01e753383662e924f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67083904"
---
# <a name="manage-emergency-access-accounts-in-azure-ad"></a>Azure AD'de Acil Durum erişim hesapları yönetme

Farkında olmadan oturum açın veya var olan tek bir kullanıcının hesabı yönetici olarak etkinleştirmek için Azure Active Directory (Azure AD) kiracınızın dışında kilitli önlemesini önemlidir. İki veya daha fazla oluşturarak yönetici erişimi yanlışlıkla eksikliği etkisini en aza indirebileceğiniz *Acil Durum erişim hesapları* kiracınızdaki.

Acil Durum erişim hesapları son derece ayrıcalıklı olan ve belirli kişilere atanmaz. Acil Durum erişim hesapları, Acil Durum ya da 'Acil Durum' senaryoları normal yönetim hesaplarını burada kullanılamaz sınırlıdır. Kuruluşlar, yalnızca kesinlikle gerekli olduğunda kez Acil Durum hesabına ait kullanım kısıtlama bir hedef sürdürmeniz gerekir.

Bu makalede, Azure AD'de Acil Durum erişim hesaplarını yönetmek için yönergeler sağlar.

## <a name="when-would-you-use-an-emergency-access-account"></a>Acil Durum erişim hesabı kullandığınızda?

Bir kuruluş, aşağıdaki durumlarda bir Acil Durum erişim hesabı kullanmanız gerekebilir:

- Kullanıcı hesaplarını Federal ve Federasyon bir hücre ağ kesintisi veya kimlik sağlayıcısı kesinti nedeniyle şu anda kullanılamıyor. Örneğin, ortamınızda kimlik sağlayıcısı konağı aşağıya indi, kullanıcıları Azure AD kimlik sağlayıcısına yönlendirir, oturum açamıyor olabilir.
- Yöneticiler Azure multi-Factor Authentication kaydedilen ve tek tek tüm cihazlardan kullanılamıyor veya hizmet kullanılamıyor. Kullanıcıların multi-Factor Authentication'ın bir rolü etkinleştirmek için tamamlayamıyor olabilir. Örneğin, bir hücre ağ kesintisi bunları yanıtlama telefon görüşmeleri veya kısa mesaj, kayıtlı cihazlarını yalnızca iki kimlik doğrulama mekanizması almasını engelliyor.
- En son genel yönetici erişimi olan bir kişi kuruluştan ayrılan. Azure AD'ye son genel yönetici hesabını silinmesini engeller, ancak hesap siliniyor gelen veya şirket devre dışı engellemez. Her iki durumda, kuruluş hesabı kurtarmanız mümkün hale getirebilirsiniz.
- Acil, doğal afetler gibi öngörülemeyen durumlarda aşamasında bir cep telefonu veya diğer ağlara kullanılamıyor olabilir. 

## <a name="create-two-cloud-based-emergency-access-accounts"></a>İki bulut tabanlı bir Acil Durum erişim hesapları oluşturma

İki veya daha fazla Acil Durum erişim hesapları oluşturun. Bu hesapları kullanan yalnızca bulut hesapları olmalıdır \*. onmicrosoft.com etki alanı olan ve olmayan Federasyon veya bir şirket içi ortamdan eşitlenen.

Bu hesaplar yapılandırırken, aşağıdaki gereksinimler karşılanmalıdır:

- Acil Durum erişim hesapları kuruluştaki herhangi bir kullanıcı ile ilişkilendirilmemelidir. Hesaplarınız bir çalışan tarafından sağlanan cep telefonları ile bağlı değil, bu seyahat çalışanları veya diğer çalışan özel kimlik bilgileri ile donanım belirteçleri emin olun. Bu önlem, kimlik bilgisi gerektiğinde tek bir çalışan ulaşılamaz olduğu örnekleri kapsar. Tüm kayıtlı cihazları olan Azure AD ile iletişim kuran birden fazla yol bilinen ve güvenli bir konumda kalmasını sağlamak önemlidir.
- Bir Acil Durum erişim hesabı için kullanılan kimlik doğrulama mekanizması diğer Acil Durum erişim hesapları dahil olmak üzere, diğer yönetim hesapları tarafından kullanılan farklı olmalıdır.  Örneğin, normal yönetici oturum açma şirket içi MFA ise, Azure mfa'yı farklı bir mekanizma olacaktır.  Azure MFA, birincil yönetici hesaplarınız için kimlik doğrulaması parçası ise, ancak daha sonra farklı bir yaklaşım gibi bir üçüncü taraf MFA sağlayıcısı ile koşullu erişim kullanarak bu göz önünde bulundurun.
- Cihaz kimlik bilgisi gereken değil sona veya kullanımını alınamadığından otomatik temizleme kapsamında olabilir.  
- Genel yönetici rolü atama kalıcı, Acil Durum erişim hesapları için yapmanız. 


### <a name="exclude-at-least-one-account-from-phone-based-multi-factor-authentication"></a>En az bir hesap multi-Factor authentication'dan telefon tabanlı Dışla

Güvenliği aşılmış bir paroladan kaynaklanan bir saldırı riskini azaltmak için Azure AD, tüm bireysel kullanıcılar için çok faktörlü kimlik doğrulaması gerektiren önerir. Bu grubun Yöneticiler ve diğerlerinin tümü (örneğin, finansal görevlileri), gizliliği tehlikeye giren hesap önemli bir etkisi yoktur içerir.

Ancak, Acil Durum erişim hesapları en az biri, diğer olmayan Acil Durum hesapları olarak aynı çok faktörlü kimlik doğrulaması mekanizması olmamalıdır. Bu, üçüncü taraf multi-Factor authentication çözümleri içerir. Gerektirmek için koşullu erişim ilkesi varsa [her yönetici için multi-Factor authentication](../authentication/howto-mfa-userstates.md) Azure AD'de ve diğer bağlı yazılım hizmet (SaaS) uygulamaları, Acil Durum erişim hesapları bu dışarıda gereksinim ve bunun yerine, farklı bir mekanizma yapılandırın. Ayrıca, bir kullanıcı başına çok faktörlü kimlik doğrulama İlkesi hesaplar sahip olmadığından emin olmanız gerekir.

### <a name="exclude-at-least-one-account-from-conditional-access-policies"></a>En az bir hesabı koşullu erişim ilkelerinin dışında tutun

Acil sırasında olası bir sorunu düzeltmek için erişimi engellemek için bir ilke istemezsiniz. Öğeden tüm koşullu erişim ilkeleri en az bir Acil Durum erişim hesabı'nın hariç tutulması gerekir. Etkinleştirdiyseniz bir [temel ilke](../conditional-access/baseline-protection.md), Acil Durum erişim hesapları dışlamanız gerekir.

## <a name="additional-guidance-for-hybrid-customers"></a>Karma müşteriler için ek yönergeler

Azure AD ile federasyona eklemek için AD etki alanı Hizmetleri ve ADFS kullanan kuruluşlar veya benzer bir kimlik sağlayıcısı için ek bir seçenek olup, MFA talebi, kimlik sağlayıcısı tarafından sağlanan bir Acil Durum erişim hesabı yapılandırmak için.  Örneğin, Acil Durum erişim hesabı gibi bir akıllı kart üzerinde depolanan bir sertifika ve anahtar çifti tarafından yedeklenen.  AD, kullanıcının kimliği doğrulandığında, AD FS için Azure AD kullanıcı MFA gereksinimlerinin karşılandığından emin gösteren bir talep sağlayabilirsiniz.  Federasyon kurulamıyor durumunda bile bu yaklaşımda, kuruluşlar yine de bulut tabanlı bir Acil Durum erişim hesapları olması gerekir. 

## <a name="store-devices-and-credentials-in-a-safe-location"></a>Cihazlar ve kimlik bilgilerini güvenli bir konuma Store

Kuruluşlar, Acil Durum erişim hesapları için kimlik bilgilerini yalnızca bunları kullanma yetkisi olan kişiler için güvenli ve bilinen kalmasını sağlamak gerekir. Bazı müşteriler bir akıllı kart ve parolaların diğerleri kullanın. Genellikle bir Acil Durum erişim hesabı için bir parola iki veya üç parçalara ayrılmış, üzerinde kağıda ayrı parçaları yazılan ve güvenli, ayrı konumlardaki güvenli, ateşe dayanıklı kasalar depolanır.

Parola kullanıyorsanız, hesaplarının parola süresinin sona ermediğinden güçlü parolalar sahip olduğundan emin olun. İdeal olarak, parolalar, uzun ve rastgele oluşturulan en az 16 karakter arasında olmalıdır.


## <a name="monitor-sign-in-and-audit-logs"></a>Oturum izleme ve Denetim günlükleri

İzleyici [Azure AD oturum açma ve denetim günlüklerini](../reports-monitoring/concept-sign-ins.md) oturum açma işlemleri için ve Acil Durum erişim hesapları etkinliğinden denetleyebilirsiniz. Normalde, bu hesaplar, oturum açarken olması değil ve onları kullanmanız açısından anormal ve güvenlik araştırma gerektiren büyük olasılıkla, bu nedenle, değişiklikler yapmak değil.

## <a name="validate-accounts-at-regular-intervals"></a>Düzenli aralıklarla hesapları doğrula

Acil Durum erişim hesapları kullanmak ve Acil Durum erişim hesapları doğrulamak için personel eğitmek için düzenli aralıklarla aşağıdaki en düşük adımları uygulayın:

- Personel güvenlik izleme hesabı onay etkinliği devam ediyor farkında olduğundan emin olun.
- Acil Durum sonu cam işlemi bu hesapları kullanma belgelenmiş ve güncel olduğundan emin olun.
- Yöneticiler ve kimin bir Acil Durum sırasında şu adımları gerçekleştirmeniz gerekebilir güvenlik sorumluları işlemi eğitilir emin olun.
- Hesap kimlik bilgilerini, özellikle, Acil Durum erişim hesapları, parolaları güncelleştirin ve ardından Acil Durum erişim hesapları oturum açma ve yönetimsel görevleri gerçekleştirmek doğrulayın.
- Kullanıcıların multi-Factor Authentication ya da tek tek her kullanıcının cihazına veya kişisel ayrıntılar için Self Servis parola sıfırlama (SSPR) kaydolmadınız emin olun. 
- Hesaplar çok faktörlü kimlik doğrulaması için oturum açma veya rol etkinleştirme sırasında kullanım için bir cihaz için kayıtlı bir Acil Durum sırasında kullanmak için gereken tüm yöneticiler için cihaz erişilebildiğinden emin olun. Ayrıca cihaz ortak bir hata modu paylaşmayın en az iki ağ yolları ile iletişim kurabildiğini doğrulayın. Örneğin, cihaz özelliği'nin kablosuz ağ hem hücresi sağlayıcısını Ağ üzerinden İnternet'e iletişim kurabilir.

Bu adımlar, düzenli aralıklarla ve önemli değişiklikler için gerçekleştirilmelidir:

- En az 90 günde bir
- BT ekibi, işi değişiklik, bir ayrılma ya da yeni bir işe alma gibi yeni bir değişiklik olduğunda
- Azure AD aboneliklerini kuruluştaki zaman değiştirildi

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD'de karma ve bulut dağıtımları için ayrıcalıklı erişim güvenliğini sağlama](directory-admin-roles-secure.md)
- [Azure AD kullanarak kullanıcı ekleme](../fundamentals/add-users-azure-active-directory.md) ve [yeni kullanıcı genel Yönetici rolüne atayın.](../fundamentals/active-directory-users-assign-role-azure-portal.md)
- [Azure AD Premium'a kaydolun](../fundamentals/active-directory-get-started-premium.md), zaten kaydolmuş olmasanız
- [Bir kullanıcı için iki aşamalı doğrulama gerektirme](../authentication/howto-mfa-userstates.md)
- [Office 365'te genel Yöneticiler için ek koruma olanaklarını yapılandırma](https://docs.microsoft.com/office365/enterprise/protect-your-global-administrator-accounts), Office 365 kullanıyorsanız
- [Genel yönetici / erişim değerlendirmesi başlatma](../privileged-identity-management/pim-how-to-start-security-review.md) ve [varolan genel yöneticileri daha özel yönetici rolleri için geçiş](directory-assign-admin-roles.md)
