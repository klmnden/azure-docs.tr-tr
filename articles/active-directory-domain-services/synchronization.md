---
title: 'Azure Active Directory etki alanı Hizmetleri: Yönetilen etki alanlarında eşitleme | Microsoft Docs'
description: Azure Active Directory Domain Services yönetilen etki alanındaki eşitleme anlama
services: active-directory-ds
documentationcenter: ''
author: MikeStephens-MS
manager: daveba
editor: curtand
ms.assetid: 57cbf436-fc1d-4bab-b991-7d25b6e987ef
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/22/2019
ms.author: mstephen
ms.openlocfilehash: 295a991e610e76971413a2abdba1e2fcc5f9eba6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66246697"
---
# <a name="synchronization-in-an-azure-ad-domain-services-managed-domain"></a>Azure AD Domain Services yönetilen etki alanı eşitleme
Aşağıdaki diyagram, eşitleme Azure AD Domain Services yönetilen etki alanlarını nasıl çalıştığını gösterir.

![Azure AD Etki Alanı Hizmetleri'nde eşitleme](./media/active-directory-domain-services-design-guide/sync-topology.png)

## <a name="synchronization-from-your-on-premises-directory-to-your-azure-ad-tenant"></a>Azure AD kiracınız için şirket içi dizininizden eşitleme
Azure AD Connect eşitleme grup üyelikleri kullanıcı hesaplarını eşitlemek için kullanılır ve Azure AD kiracınız için kimlik bilgisi karma hale getirir. Kullanıcı öznitelikleri gibi UPN'nin hesaplar ve şirket içi SID (güvenlik tanımlayıcısı) eşitlenir. Azure AD Domain Services'ı kullanırsanız, NTLM ve Kerberos kimlik doğrulaması için gereken eski bir kimlik bilgisi karmalarının Azure AD kiracınıza de eşitlenir.

Geri yazma yapılandırırsanız, Azure AD dizininizde gerçekleşen değişiklikler geri şirket içi Active Directory'nize eşitlenir. Örneğin, Azure AD Self Servis parola Yönetimi'ni kullanarak parolanızı değiştirirseniz, şirket içinde değiştirilen parolayı güncelleştirilir AD etki alanı.

> [!NOTE]
> Düzeltmeleri tüm bilinen hataların olduğundan emin olmak için her zaman Azure AD Connect'in en son sürümünü kullanın.
>
>

## <a name="synchronization-from-your-azure-ad-tenant-to-your-managed-domain"></a>Azure AD kiracınızdan yönetilen Etki Alanınızla eşitleme
Kullanıcı hesapları, Grup üyeliklerinin ve kimlik bilgisi karmalarının Azure AD kiracınızdan Azure AD Domain Services yönetilen Etki Alanınızla eşitlenir. Bu eşitleme işlemi otomatik olarak gerçekleşir. Yapılandırma, izleme veya bu eşitleme işlemi, yönetimi gerekmez. İlk eşitleme birkaç gün Azure AD dizininizdeki nesneleri sayısına bağlı olarak birkaç saat sürebilir. İlk eşitleme tamamlandıktan sonra yönetilen etki alanınızda güncelleştirilecek Azure AD'de yapılan değişiklikler için yaklaşık 20-30 dakika sürer. Bu eşitleme aralığı parola değişiklikleri uygular veya için öznitelikler Azure AD'de yapılan değişiklikler.

Eşitleme işlemi, aynı zamanda bir-way/doğası gereği tek yönlü. Yönetilen etki alanınıza büyük ölçüde oluşturduğunuz özel OU'lar dışında salt okunur. Bu nedenle, kullanıcı öznitelikleri, kullanıcı parolalarını veya yönetilen etki alanı içinde grup üyeliği değişiklik yapamazsınız. Sonuç olarak, yönetilen etki alanınızdan değişikliklerin Azure AD kiracınız için geri ters eşitleme yoktur.

## <a name="synchronization-from-a-multi-forest-on-premises-environment"></a>Çok ormanlı şirket içi ortamdan eşitleme
Çoğu kuruluş birden fazla hesap ormanına oluşan bir oldukça karmaşık şirket içi kimlik altyapınızı vardır. Azure AD Connect eşitleme kullanıcıları, grupları ve kimlik bilgisi karmalarının Azure AD kiracınıza Çoklu orman ortamlarından destekler.

Buna karşılık, Azure AD kiracınıza kadar bir daha basit ve düz ad alanıdır. Kullanıcının Azure AD tarafından güvenliği sağlanan uygulamalar güvenilir bir şekilde erişmesini sağlamak için kullanıcı hesapları farklı ormanlardaki genelinde UPN çakışmalarını çözme. Azure AD kiracınıza benzeyen, Azure AD Domain Services yönetilen etki alanı ayılarının kapatın. Yönetilen etki alanınızda düz bir OU yapısı görürsünüz. Tüm kullanıcı hesapları ve grupları farklı şirket içi etki alanları veya ormanlar eşitlenmekte olan rağmen 'AADDC Users' kapsayıcısı içinde depolanır. Hiyerarşik bir OU yapılandırılmış şirket içi yapı. Yönetilen etki alanınıza hala basit düz bir OU yapısına sahiptir.

## <a name="exclusions---what-isnt-synchronized-to-your-managed-domain"></a>Dışlamalar - ne yönetilen Etki Alanınızla eşitlenmemiş
Aşağıdaki nesneler veya öznitelikleri, Azure AD kiracınız veya yönetilen etki alanınıza eşitlenmedi:

* **Hariç tutulan öznitelikleri:** Azure AD Connect kullanarak şirket içi etki alanınızdan Azure AD kiracınız ile eşitlenmesini belirli öznitelikleri dışlamak isteyebilirsiniz. Hariç tutulan bu öznitelikler, yönetilen etki alanında kullanılamaz.
* **Grup ilkeleri:** Şirket içi etki alanınızda yapılandırılan Grup ilkeleri, yönetilen Etki Alanınızla eşitlenmez.
* **Sysvol paylaşımı:** Benzer şekilde, yönetilen etki alanınıza şirket içi etki alanınızdaki Sysvol paylaşımının içeriğini eşitlenmez.
* **Bilgisayar nesneleri için:** Bilgisayar nesneleri, şirket içi etki alanına katılmış bilgisayarlar için yönetilen Etki Alanınızla eşitlenmez. Bu bilgisayarlar değil, yönetilen etki alanınız ile güven ilişkisi olan ve şirket içi etki alanınıza yalnızca ait. Yönetilen etki alanında yalnızca, açıkça etki alanının yönetilen etki alanına katılmış bilgisayarları için bilgisayar nesneleri bulun.
* **Kullanıcılar ve gruplar için SID Geçmişi öznitelikleri:** Birincil kullanıcı ve birincil grup SID şirket etki alanınızdan yönetilen Etki Alanınızla eşitlenir. Ancak, kullanıcılar ve gruplar için mevcut SIDHistory öznitelikleri, yönetilen etki alanınıza şirket içi etki alanınızdan eşitlenmez.
* **Kuruluş birimi (OU) yapıları:** Tanımlanan şirket içi etki alanınızdaki kuruluş birimlerini, yönetilen Etki Alanınızla eşitleme yapmayın. Yönetilen etki alanınızda iki yerleşik OU'lar vardır. Varsayılan olarak, yönetilen etki alanınıza düz bir OU yapısı vardır. Ancak tercih edebilirsiniz [yönetilen etki alanınızda özel bir OU oluşturun](create-ou.md).

## <a name="how-specific-attributes-are-synchronized-to-your-managed-domain"></a>Yönetilen etki alanınızla eşitlenen belirli öznitelikler nasıl
Aşağıdaki tabloda, bazı ortak öznitelikleri listeler ve yönetilen Etki Alanınızla nasıl eşitleneceğini açıklar.

| Yönetilen etki alanınızda özniteliği | source | Notlar |
|:--- |:--- |:--- |
| UPN |Azure AD kiracınızda kullanıcının UPN özniteliği |Azure AD kiracınızdan UPN özniteliğini, yönetilen Etki Alanınızla olarak eşitlenir. Bu nedenle, yönetilen Etki Alanınızla oturum açmak için en güvenilir yol UPN'nizi kullanıyor. |
| SAMAccountName |Kullanıcının mailNickname Azure AD kiracınızda özniteliği veya otomatik olarak oluşturulan |SAMAccountName özniteliğinin Azure AD kiracınızda mailNickname özniteliğinden kaynaklanıyor. Birden çok kullanıcı hesapları aynı mailNickname özniteliğine sahipse, SAMAccountName otomatik olarak üretilir. Kullanıcının mailNickname veya UPN önek 20 karakterden uzun ise, SAMAccountName SAMAccountName özniteliklerde 20 karakter sınırını karşılamak için otomatik oluşturulmuş olur. |
| Parolalar |Azure AD kiracınızdan kullanıcı parolası |(Ek kimlik bilgileri olarak da bilinir) NTLM veya Kerberos kimlik doğrulaması için gereken kimlik bilgisi karmalarını Azure AD kiracınızdan eşitlenir. Azure AD kiracınıza eşitlenmiş bir kiracı ise, bu kimlik bilgilerini şirket etki alanınızdan elde edilir. |
| Birincil kullanıcı/Grup SID |Otomatik olarak oluşturulan |Birincil SID kullanıcı/grup hesapları için yönetilen etki alanınız otomatik olarak üretilir. Bu öznitelik, birincil kullanıcı/Grup SID'si şirket içi nesnesinin eşleşmiyor AD etki alanı. Bu uyuşmazlık, yönetilen etki alanı farklı bir SID ad alanı, şirket içi etki sahip olmasıdır. |
| Kullanıcılar ve gruplar için SID Geçmişi |Şirket içi birincil kullanıcı ve Grup SID'si |Yönetilen etki alanınız içindeki kullanıcılar ve gruplar için SID Geçmişi özniteliği, ilgili birincil kullanıcı veya grup SID şirket içi etki alanınızda eşleşecek şekilde ayarlanır. Bu özellik, yeniden ACL kaynaklarına gerekmediğinden lift-and-shift ile taşıma şirket içi uygulamaların yönetilen etki alanında kolaylaştırmak yardımcı olur. |

> [!NOTE]
> **UPN biçimini kullanarak yönetilen etki alanına oturum açın:** SAMAccountName özniteliğini, yönetilen etki alanınıza bazı kullanıcı hesapları için otomatik olarak oluşturulmuş olabilir. Birden çok kullanıcı aynı mailNickname özniteliğine sahip veya kullanıcıların aşırı uzun UPN ön ekleri varsa, bu kullanıcılar için SAMAccountName otomatik olarak oluşturulmuş olabilir. Bu nedenle, SAMAccountName biçimi (örneğin, ' CONTOSO100\joeuser') her zaman etki alanında oturum açmak için güvenilir bir yol değil. Kullanıcıların otomatik olarak oluşturulan SAMAccountName kendi UPN önekten farklı olabilir. UPN biçimini kullanın (örneğin, 'joeuser@contoso100.com') için yönetilen etki alanında güvenilir bir şekilde oturum açmak için.

### <a name="attribute-mapping-for-user-accounts"></a>Kullanıcı hesapları için öznitelik eşlemesi
Aşağıdaki tabloda, Azure AD kiracınızda nesneleri yönetilen etki alanınıza karşılık gelen özniteliklerle eşitlenen bir kullanıcı için nasıl özel öznitelikler gösterilmektedir.

| Azure AD kiracınıza kullanıcı özniteliği | Yönetilen etki alanınıza kullanıcı özniteliği |
|:--- |:--- |
| accountEnabled |userAccountControl (ayarlar veya bit ACCOUNT_DISABLED temizler) |
| city |m |
| Ülke |Ortak |
| Bölüm |Bölüm |
| displayName |displayName |
| facsimileTelephoneNumber |facsimileTelephoneNumber |
| givenName |givenName |
| İş Unvanı |başlık |
| posta |posta |
| mailNickname |msDS-AzureADMailNickname |
| mailNickname |SAMAccountName (bazen otomatik olarak oluşturulmuş olabilir) |
| Mobil |Mobil |
| Nesne Kimliği |msDS-AzureADObjectId |
| onPremiseSecurityIdentifier |SID Geçmişi |
| passwordPolicies |userAccountControl (ayarlar veya bit DONT_EXPIRE_PASSWORD temizler) |
| physicalDeliveryOfficeName |physicalDeliveryOfficeName |
| posta kodu |posta kodu |
| preferredLanguage |preferredLanguage |
| state |St |
| streetAddress |streetAddress |
| Soyadı |sn |
| telephoneNumber |telephoneNumber |
| userPrincipalName |userPrincipalName |

### <a name="attribute-mapping-for-groups"></a>Gruplar için öznitelik eşlemesi
Aşağıdaki tabloda, Azure AD kiracınızda nesneler, yönetilen etki alanınıza karşılık gelen özniteliklerle eşitlenir grubu için nasıl özel öznitelikler gösterilmektedir.

| Azure AD kiracınızda grubu özniteliği | Yönetilen etki alanınıza grubu özniteliği |
|:--- |:--- |
| displayName |displayName |
| displayName |SAMAccountName (bazen otomatik olarak oluşturulmuş olabilir) |
| posta |posta |
| mailNickname |msDS-AzureADMailNickname |
| Nesne Kimliği |msDS-AzureADObjectId |
| onPremiseSecurityIdentifier |SID Geçmişi |
| securityEnabled |groupType |

## <a name="password-hash-synchronization-and-security-considerations"></a>Parola Karması eşitleme ve güvenlik konuları
Azure AD Etki Alanı Hizmetleri'ni etkinleştirdiğinizde, Azure AD dizininizi oluşturur ve parola karmalarının NTLM ve Kerberos uyumlu biçimlerde depolar. 

Mevcut bulut kullanıcı hesapları için Azure AD düz metin parolalarını hiçbir zaman depolar bu yana bu karmaları otomatik olarak oluşturulamaz. Bu nedenle Microsoft gerektirir [parolalarını sıfırlama/değiştirme için bulut kullanıcıları](active-directory-ds-getting-started-password-sync.md) sırada oluşturulan ve Azure AD'de depolanan kendi parola karmaları için. Parola karmalarının Azure AD Domain Services'ı etkinleştirdikten sonra Azure AD'de oluşturulan bulut kullanıcı hesabı için oluşturulur ve NTLM ve Kerberos uyumlu biçimlerinde depolanır. 

Kullanıcı hesaplarını gelen eşitlenen için şirket içi Azure AD Connect Sync kullanarak AD yapmanız [NTLM ve Kerberos uyumlu biçimde parola karmaları eşitlemek için Azure AD Connect yapılandırma](active-directory-ds-getting-started-password-sync-synced-tenant.md).

NTLM ve Kerberos uyumlu parola karmaları, Azure AD'de her zaman şifrelenmiş olarak depolanır. Bu karmalar, yalnızca Azure AD Domain Services sahip şifre çözme anahtarları erişim şekilde şifrelenir. Başka bir hizmet veya bileşen Azure AD'de şifre çözme anahtarları erişimi vardır. Şifreleme anahtarları benzersiz başına Azure AD kiracınız var. Azure AD Domain Services yönetilen etki alanınız için etki alanı denetleyicilerinde parola karmalarının eşitler. Bu parola karmaları depolanır ve bu etki alanı denetleyicileri Windows Server AD etki alanı denetleyicilerinde güvenli parolaları nasıl depolandığını ve benzer güvenli. Bu yönetilen etki alanı denetleyicileri için diskler, bekleme sırasında şifrelenir.

## <a name="objects-that-are-not-synchronized-to-your-azure-ad-tenant-from-your-managed-domain"></a>Azure AD kiracınız için yönetilen etki alanınızdan eşitlenmez nesneleri
Bu makalenin önceki bölümde açıklandığı gibi yönetilen etki alanınızdan Azure AD kiracınız için yeniden eşitleme yoktur. Tercih edebilirsiniz [özel kuruluş birimi (OU) oluşturun](create-ou.md) yönetilen etki alanınızda. Ayrıca, diğer OU'ları, kullanıcıları, grupları veya bu özel OU içinde hizmet hesapları oluşturabilirsiniz. Hiçbir özel OU içinde oluşturulan nesneler, Azure AD kiracınıza eşitlenir. Bu nesneler, yalnızca yönetilen etki alanınız içinde kullanmak için kullanılabilir. Bu nedenle, bu nesneler, Azure AD PowerShell cmdlet'leri, Azure AD Graph API'si veya Azure AD Yönetimi kullanıcı Arabirimi kullanarak görünür değildir.

## <a name="related-content"></a>İlgili İçerik
* [Özellikler - Azure AD etki alanı Hizmetleri](active-directory-ds-features.md)
* [Dağıtım senaryoları - Azure AD Domain Services](scenarios.md)
* [Azure AD Domain Services ilgili ağ konuları](network-considerations.md)
* [Azure AD Domain Services ile çalışmaya başlama](create-instance.md)
