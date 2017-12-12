---
title: "Azure Active Directory PoC Playbook uygulaması | Microsoft Docs"
description: "Keşfetmek ve hızlı bir şekilde kimlik ve erişim yönetimi senaryoları uygulayan"
services: active-directory
keywords: "Azure active directory, playbook, kavram kanıtı, PT"
documentationcenter: 
author: dstefanMSFT
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2017
ms.author: dstefan
ms.openlocfilehash: e26dfe4aaa374f5587038a0de66c0bd8703c9a41
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-proof-of-concept-playbook-implementation"></a>Azure Active Directory Playbook kavram kanıtı: uygulama

## <a name="foundation---syncing-ad-to-azure-ad"></a>Foundation - AD'den Azure AD'ye eşitleniyor 

Karma kimlik zaten bir şirket içi dizin olan Kurumsal müşteriler çoğunu temelidir. Burada amaç kasıtlı olarak daha az zaman burada gerçek kimlik ve erişim senaryoları değerini göstermek mümkün olduğunca olarak harcadığınız olmaktır. 

| Senaryo | Yapı taşları| 
| --- | --- |  
| [Şirket içi kimliğinizi buluta genişletme](#extending-your-on-premises-identity-to-the-cloud) | [Dizin eşitleme - parola karması eşitlemesi](active-directory-playbook-building-blocks.md#directory-synchronization---password-hash-sync-phs---new-installation) <br/>**Not**: DirSync/ADSync veya Azure AD Connect'in önceki sürümlerinde zaten varsa, bu adım isteğe bağlıdır. Bu kılavuzdaki bazı senaryolar, Azure AD Connect daha yeni sürümü gerektirebilir.  <br/>[Markalama](active-directory-playbook-building-blocks.md#branding) | 
| [Grupları kullanarak Azure AD lisans atama](#assigning-azure-ad-licenses-using-groups) | [Lisans grup tabanlı](active-directory-playbook-building-blocks.md#group-based-licensing) |


### <a name="extending-your-on-premises-identity-to-the-cloud"></a>Şirket içi kimliğinizi buluta genişletme 

1. Bob contoso'da Active Directory yöneticisidir. Hüseyin, bir kullanıcı kümesi için bir hizmet olarak kimlik etkinleştirme gereksinimi alır. Azure AD Connect Sihirbazı, bulutta hedef kullanıcıların kimliklerini yürütme sonrasında. 
2. Bob Susie, Azure Active Directory'ye erişim paneline erişmek ve aynen doğrulanabilir onaylamak için hedef kullanıcılardan birine sorar. Susie markalı oturum açma sayfası ve gelecekteki uygulama erişimini etkinleştirmek için hazır olan bir boş erişim paneli görür.

### <a name="assigning-azure-ad-licenses-using-groups"></a>Grupları kullanarak Azure AD lisans atama 

1. Bob, Azure AD genel yönetici olan ve Azure ad ilk dağıtımının bir parçası olarak belirli bir kullanıcı kümesi için Azure AD lisansları ayırmak istiyor.
2. Bob pilot kullanıcılar için bir grup oluşturur. 
3. Bob lisans grubuna atar.
4. Bir bilgi çalışanı Susie kendi iş işlevlerinin bir parçası olarak güvenlik grubuna eklenir
5. Bir süre sonra Susie Azure AD premium lisansı erişebilir. Bu daha POC senaryoları daha sonra olanak tanır.

## <a name="theme---lots-of-apps-one-identity"></a>Tema - çok sayıda uygulamalar, bir kimlik

| Senaryo | Yapı taşları| 
| --- | --- |  
| [SaaS uygulamaları - Federasyon SSO tümleştirme](#integrate-saas-applications---federated-sso) | [SaaS Federasyon SSO yapılandırma](active-directory-playbook-building-blocks.md#saas-federated-sso-configuration) <br/>[Grupları - temsilci sahipliği](active-directory-playbook-building-blocks.md#groups---delegated-ownership) |
| [SaaS uygulamaları - parola SSO tümleştirme](#integrate-saas-applications---password-sso) | [SaaS parola SSO yapılandırma](active-directory-playbook-building-blocks.md#saas-password-sso-configuration) |
| [SSO ve kimlik yaşam döngüsü olayları](#sso-and-identity-lifecycle-events) | [SaaS ve kimlik yaşam döngüsü](active-directory-playbook-building-blocks.md#saas-and-identity-lifecycle) |
| [Paylaşılan hesaplarına güvenli erişim](#secure-access-to-shared-accounts) | [SaaS paylaşılan hesaplarını yapılandırma](active-directory-playbook-building-blocks.md#saas-shared-accounts-configuration) |
| [Şirket içi uygulamalara güvenli uzaktan erişim](#secure-remote-access-to-on-premises-applications) | [Uygulama Proxy Yapılandırması](active-directory-playbook-building-blocks.md#app-proxy-configuration) |
| [LDAP kimlikleri Azure ad eşitleme](#synchronize-ldap-identities-to-azure-ad) |  [Genel LDAP Bağlayıcısı yapılandırması](active-directory-playbook-building-blocks.md#generic-ldap-connector-configuration) |

### <a name="integrate-saas-applications---federated-sso"></a>SaaS uygulamaları - Federasyon SSO tümleştirme 

1. Bob, Azure AD genel yönetici olan ve kendi ServiceNow örneği erişimi etkinleştirmek için pazarlama departmanından bir isteği alır. Bob adım adım öğretici Azure AD belgelerinde bulur ve onu izleyen ve uygulama kullanıcılara atama Kevin, pazarlama ekibinin head atar. 
2. Kevin ServiceNow yetkilendirmeler sahibi olarak oturum açması ve uygulamaya Susie atar. Susie'nın profili ServiceNow içinde otomatik olarak oluşturulmuş Kevin ayrıca bildirimler
3. Susie pazarlama departmanındaki bir bilgi çalışanı ' dir. Kendisi için azure AD oturum açtığında ve aynen atanan tüm SaaS uygulamaları myapps Portalı'nda bulur. Buradan, aynen sorunsuz bir şekilde ServiceNow erişim alır.
4. Pazarlama departmanı denetim ServiceNow kimin istiyor. Bob bir etkinlik raporu indirir ve e-posta Kevin ile paylaşır.  

### <a name="sso-and-identity-lifecycle-events"></a>SSO ve kimlik yaşam döngüsü olayları

1. Bir Mazeret Susie alır ve geçici devre dışı şirket ilkesi tarafından şirket içi AD hesabıdır. Susie artık Azure AD ile oturum açamıyorsanız ve ServiceNow etkinleştirilemez. 
2. Susie yanal hareket satış pazarlama yapar. Kevin kendi erişim ServiceNow kaldırır. Azure ad myapps Susie günlüğe kaydeder ve artık ServiceNow döşeme görür. 10 dakika sonra Susie hesap ServiceNow Yönetim Konsolu'ndan devre dışı bırakıldı Kevin onaylar.

### <a name="integrate-saas-applications---password-sso"></a>SaaS uygulamaları - parola SSO tümleştirme

1. Bob Atlassian HipChat erişimi yapılandırır. Parola SSO tümleştirme ve Susie erişim HipChat sahip
2. Susie myapps portalında oturum ve aynen indirmeleri Azure AD IE tarayıcı uzantısı indirmek için bir bağlantı görür
3. Tıklatıldığında, bunları kendi HipChat kullanıcı adı ve parola kimlik bilgileri sorulur. Bu tek seferlik bir işlemdir ve tamamladıktan sonra aynen HipChat erişimi
4. Daha sonra birkaç gün Susie myapps portal açar ve HipChat yeniden tıklar. Bu sefer orada aynen sorunsuz erişim alır
5. Uygulama kimin denetlemek üzere Kevin, HipChat uygulama sahibi istemektedir. Bob denetim raporu indirir ve e-posta Kevin ile paylaşır. 

### <a name="secure-access-to-shared-accounts"></a>Paylaşılan hesaplarına güvenli erişim 

1. Bob, satış ekibinin üyeleri için paylaşılan Twitter tanıtıcı güvenli hale getirmek için görevli. Kendisi Twitter SSO uygulaması ekler ve satış ekibi güvenlik grubuna atar. Kendisine paylaşılan hesap kimlik bilgilerini verilen ve kendisine sistemde sağlar. 
2. Twitter kimlik bilgileri paylaşımı artık farkında birden çok kişi nedeniyle güvenilir değil. Bob Twitter parola otomatik geçişi sağlar.
3. Susie, satış ekibinin bir üyesi myapps portalında oturum ve Azure AD IE tarayıcı uzantısı indirmek için bir bağlantı görür. Aynen yükler.
4. Tıklatıldığında aynen almak doğrudan Twitter erişim. Aynen parola bilmez.
5. ARNOLD da satış ekibi bir parçasıdır. 3-4 arası adımları Susie olarak aynı deneyimi sahip
6. Satış departmanı denetim Twitter kimin istiyor. Bob bir etkinlik raporu indirir ve e-posta Kevin ile paylaşır. 

### <a name="secure-remote-access-to-on-premises-applications"></a>Şirket içi uygulamalara güvenli uzaktan erişim

1. Bob, Azure AD genel yönetici, çalışanların uzaktan çalışırken giderleri uygulama gibi birkaç yararlı şirket kaynaklarına erişmesini sağlamak için çok sayıda isteği aldı. Müşterinizle izleyen [uygulama proxy'si belgelerine](active-directory-application-proxy-enable.md) bir bağlayıcı yükleyip giderleri uygulama proxy'si uygulama yayımlamak için. 
2. Bob dış giderleri uygulama URL'si Susie ile uzaktan erişim gereken çalışanlar birini paylaşır. Aynen bağlantıyı erişir ve AAD karşı kimlik doğrulamasından sonra aynen giderleri uygulama erişebilir ve üretken olmaya devam uzaktan oluştu. 
3. Bob, aynı işlemi kullanarak ve gerektiğinde kullanıcılara erişim verip ek şirket içi uygulamaları yayımlamak sonra devam eder. Müşterinizle koşullu erişim ve kendisine, ek güvenlik sağlamak için yayımlar daha hassas uygulamalar için çok faktörlü kimlik doğrulaması ekler.

### <a name="synchronize-ldap-identities-to-azure-ad"></a>LDAP kimlikleri Azure ad eşitleme

1. Bob'ın şirket karmaşık kimlik altyapısı sahiptir. Kullanıcıların çoğunun Windows Server Active Directory etki alanı Hizmetleri (EKLER içinde) korunur. Bazıları, ik sistemi içindeki Active Directory Basit Dizin Hizmetleri (ADLDS) tarafından yönetilir.
2. Bob (aynı zamanda bu EKLER mevcut olmayan) tüm kullanıcıların SaaS uygulamaları için erişimi etkinleştirme ile görevli.
3. Bob, Azure AD CONNECT'te ADLDS gelen veri çekmek için genel LDAP Bağlayıcısı yapılandırır.
4. LDAP kullanıcılar meta veri deposu içine ve Azure ad doldurulur Bob eşitleme kuralları oluşturur, bu nedenle
5. Her SaaS uygulamasını kullanarak LDAP kullanıcının eriştiği olan Susie kimlik eşitlendi



> [!IMPORTANT] 
> Bu, FIM/MIM aşina gerektiren gelişmiş bir yapılandırmadır. Üretimde Biz bu yapılandırma hakkında sorular öneri kullandıysanız geçtikleri [Premier Destek](https://support.microsoft.com/premier).



## <a name="theme---increase-your-security"></a>Tema - artış güvenlik 

| Senaryo | Yapı taşları| 
| --- | --- |  
| [Güvenli yönetici hesabına erişimi](#secure-administrator-account-access) | [Telefon görüşmesi ile Azure MFA](active-directory-playbook-building-blocks.md#azure-multi-factor-authentication-with-phone-calls) |
| [Uygulamalar için güvenli erişim](#secure-access-to-applications) | [SaaS uygulamaları için koşullu erişim](active-directory-playbook-building-blocks.md#mfa-conditional-access-for-saas-applications) |
| [Yalnızca zaman içinde yönetim etkinleştir](#enable-just-in-time-jit-administration) | [Privileged Identity Management](active-directory-playbook-building-blocks.md#privileged-identity-management-pim) |
| [Risk üzerinde temel kimlikleri koru](#protect-identities-based-on-risk) | [Risk olaylarını keşfetme](active-directory-playbook-building-blocks.md#discovering-risk-events) <br/>[Oturum açma riski ilkelerini dağıtma](active-directory-playbook-building-blocks.md#deploying-sign-in-risk-policies) |
| [Sertifika tabanlı kimlik doğrulaması kullanarak parolaları kimlik doğrulaması](#authenticate-without-passwords-using-certificate-based-authentication) | [Sertifika tabanlı kimlik doğrulamasını yapılandırma](active-directory-playbook-building-blocks.md#configuring-certificate-based-authentication)

### <a name="secure-administrator-account-access"></a>Güvenli yönetici hesabına erişimi

1. Bob, Azure AD genel yönetici haklarına sahip. Müşterinizle Stuart hizmetinin ortak yönetici olarak belirledi. 
2. Bob Fatih'ın hesabı her zaman güvenlik tutumunu geliştirmek MFA gerektirecek şekilde yapılandırır
3. Stuart Azure portalı ve kullanıcı oturum açma devam etmek için kendi telefon numarasını kaydetmesi gereken bildirimler için oturum açtığında
4. Stuart sonraki oturum açmayı artık çok faktörlü kimlik doğrulamasıyla korunur ve kendisine artık kendi kimliğini doğrulamak için bir telefon araması alır.

### <a name="secure-access-to-applications"></a>Uygulamalara güvenli erişim

1. Kevin ServiceNow iş sahibi. Şirket, kurumsal ağ dışından erişirken MFA ile oturum açmak için bu kullanıcıların artık istemektedir.
2. Bob, bizim Azure AD genel yönetici MFA için dış erişim sağlamak için ServiceNow uygulaması için bir koşullu erişim ilkesi ekler.
3. Susie, bizim bilgi çalışanı uygulamaları portalımı günlüğe kaydeder ve ServiceNow döşeme tıklar. Aynen yüküyle şimdi MFA ile.

### <a name="enable-just-in-time-jit-administration"></a>Zamanında (JIT) yönetiminde sadece etkinleştir

1. Bob ve Fatih Azure AD genel yönetici yaşıyor. Yönetim rollerini JIT erişimi etkinleştirmek ve ayrıcalıklı rolleri kullanımını kayıtlarını saklamak için isterler.
2. Bob, Azure AD kiracısında PIM sağlar ve Güvenlik Yöneticisi haline gelir. Hatay kendisi ve Fatih'ın genel yönetici rolü üyeliği kalıcı için uygun çevirir.
3. Şimdi, Bob ve Fatih rolleri Azure Portalı aracılığıyla Azure AD yapılandırması için herhangi bir değişiklik yapmadan önce etkinleştirme gerektirir. 

### <a name="protect-identities-based-on-risk"></a>Risk üzerinde temel kimlikleri koru 

1. Susie, tor tarayıcıdan oturum açmayı bir bilgi çalışanı çalışır. 
2. Bob, Azure AD Identity protection Panosu denetler ve anonim bir IP adresinden Susie'nın oturum açma görür. Güvenlik ekibine tür erişimleri kullanıcılar MFA ile sınama istiyor
3. Bob için Orta veya yüksek risk olaylarını MFA sınama Azure AD Identity Protection ilkesini etkinleştirir
4. Zaman gider ve Susie Tor tarayıcıdan yeniden bağlanır. Bu süre, aynen MFA testini görürsünüz

### <a name="authenticate-without-passwords-using-certificate-based-authentication"></a>Sertifika tabanlı kimlik doğrulaması kullanarak parolaları kimlik doğrulaması

1. Bob için parola kullanımını bir kimlik doğrulama faktörü olarak uygulamalarını engelliyor finansal kuruluştan, genel yönetici içindir.
2. Bob etkinleştirir ve ADFS ve Azure AD sertifika kimlik doğrulamasını zorunlu kılar
3. Uygulama erişimi sırasında Susie sertifika kullanarak kimlik doğrulaması istenir.

## <a name="theme---scale-with-self-service"></a>Tema - Self Servis ölçekli

| Senaryo | Yapı taşları| 
| --- | --- |  
| [Self Servis parola sıfırlama](#self-service-password-reset) | [Self Servis parola sıfırlama](active-directory-playbook-building-blocks.md#self-service-password-reset) |
| [Self Servis uygulamalara erişim](#self-service-access-to-applications) | [Self Servis uygulamalara erişim](active-directory-playbook-building-blocks.md#self-service-access-to-application-management) |

### <a name="self-service-password-reset"></a>Self Servis parola sıfırlama 

1. Bob, Azure AD genel yönetici olan ve bir kullanıcılar alt kümesine Susie dahil olmak üzere, Self Servis parola yönetimi sağlar. 
2. Myapps portal ve bakın gelecekte parola için kendi güvenlik bilgileri kaydetmek için bir ileti Susie günlüklerinde olayları sıfırlayın.
3. Sar birkaç gün Susie kendi parolayı unutması ve Azure AD Portalı aracılığıyla sıfırlar

### <a name="self-service-access-to-applications"></a>Self Servis uygulamalara erişim 

1. Kevin ServiceNow iş sahibi. "İçin aynı anda eklemek yerine isteğe bağlı, kaydolmak için" kullanıcıların istediği
2. Bob, bizim Azure AD genel yönetici self servis isteklerini etkinleştirmek için ServiceNow uygulama değiştirir
3. Susie, bizim bilgi çalışanı uygulamaları portalımı günlüğe kaydeder ve "daha fazla uygulama Ekle" düğmesine tıklar ve önerilen uygulamalar biri olarak ServiceNow bakın. Ardından kendisinin my uygulamaları portalına gider ve ServiceNow bakın.

[!INCLUDE [active-directory-playbook-toc](../../includes/active-directory-playbook-steps.md)]