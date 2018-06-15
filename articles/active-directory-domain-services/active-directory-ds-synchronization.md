---
title: 'Azure Active Directory etki alanı Hizmetleri: Eşitleme yönetilen etki alanlarında | Microsoft Docs'
description: Bir Azure Active Directory etki alanı Hizmetleri yönetilen etki alanında eşitleme anlama
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 57cbf436-fc1d-4bab-b991-7d25b6e987ef
ms.service: active-directory
ms.component: domains
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/30/2018
ms.author: maheshu
ms.openlocfilehash: 2449c8178f726eacad089debeae6cf1db56cc67a
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34698450"
---
# <a name="synchronization-in-an-azure-ad-domain-services-managed-domain"></a>Azure AD etki alanı Hizmetleri yönetilen etki alanında eşitleme
Aşağıdaki diyagramda, eşitleme Azure AD Etki Alanı Hizmetleri'nde yönetilen etki alanlarını nasıl çalıştığı gösterilmektedir.

![Azure AD Etki Alanı Hizmetleri'nde eşitleme](./media/active-directory-domain-services-design-guide/sync-topology.png)

## <a name="synchronization-from-your-on-premises-directory-to-your-azure-ad-tenant"></a>Azure AD kiracınız için şirket içi dizininizden eşitleme
Azure AD Connect eşitleme kullanılan kullanıcı hesaplarını eşitlemek için Grup üyeliklerinin ve kimlik bilgileri Azure AD kiracınıza karma hale getirir. Kullanıcı öznitelikleri gibi UPN hesapları ve şirket içi SID (güvenlik tanımlayıcısı) eşitlenir. Azure AD etki alanı hizmetlerini kullanıyorsanız, NTLM ve Kerberos kimlik doğrulaması için gerekli eski kimlik bilgisi karmalarını ayrıca Azure AD kiracınıza eşitlenir.

Sonradan yazma yapılandırırsanız, Azure AD dizininizi gerçekleşen değişiklikler geri, şirket içi Active Directory'ye eşitlenir. Örneğin, Azure AD Self Servis parola Yönetimi'ni kullanarak parolanızı değiştirirseniz, şirket içinde değiştirilen parola güncelleştirilir AD etki alanı.

> [!NOTE]
> Düzeltmeleri tüm bilinen hatalar için sahip emin olmak için her zaman Azure AD Connect'in en son sürümünü kullanın.
>
>

## <a name="synchronization-from-your-azure-ad-tenant-to-your-managed-domain"></a>Azure AD kiracınıza eşitlemeden yönetilen etki alanınız için
Kullanıcı hesapları, grup üyelikleri ve kimlik bilgisi karmalarını Azure AD kiracınız, Azure AD etki alanı Hizmetleri yönetilen etki alanı eşitlenir. Bu eşitleme işlemi otomatik olarak yapılır. Yapılandırmak, izlemek veya bu eşitleme işlemi yönetmek gerekmez. İlk eşitleme için birkaç gün, Azure AD directory içindeki nesneleri sayısına bağlı olarak birkaç saat sürebilir. İlk eşitleme tamamlandıktan sonra Azure AD'de yönetilen etki alanınızda güncelleştirilmesi yapılan değişiklikleri yaklaşık 20-30 dakika sürer. Bu eşitleme aralığı parola değişiklikleri için geçerli veya Azure AD içinde yapılan özniteliklerini değiştirir.

Eşitleme işlemi aynı zamanda bir-way/doğası gereği tek yönlüdür. Yönetilen etki alanınızı büyük ölçüde oluşturduğunuz özel OU'lar dışında salt okunurdur. Bu nedenle, kullanıcı öznitelikleri, kullanıcı parolalarını veya grup üyeliklerini yönetilen etki alanı içinde değişiklik yapamazsınız. Sonuç olarak, yönetilen etki alanınızdan değişiklikleri geri Azure AD kiracınız için ters eşitleme yoktur.

## <a name="synchronization-from-a-multi-forest-on-premises-environment"></a>Bir Çoklu orman şirket içi ortamından eşitleme
Çoğu kuruluş birden fazla hesap ormanına oluşan oldukça karmaşık şirket içi kimlik altyapınızı sahiptir. Azure AD Connect eşitleme kullanıcıları, grupları ve Azure AD kiracınız için kimlik bilgisi karmalarını Çoklu orman ortamlarından destekler.

Buna karşılık, Azure AD kiracınıza kadar bir daha basit ve düz ad alanıdır. Kullanıcıların Azure AD tarafından güvenliği sağlanan uygulamalar güvenilir bir şekilde erişmesini etkinleştirmek için farklı ormanlardaki kullanıcı hesapları arasında UPN çakışmaları çözümleyin. Azure AD etki alanı Hizmetleri yönetilen etki alanı bears Azure AD kiracınıza benzerlik kapatın. Yönetilen etki alanınızda düz bir OU yapısı bakın. Tüm kullanıcı hesapları ve grupları farklı şirket içi etki alanları veya ormanlar eşitlenmekte olan rağmen 'AADDC kullanıcılar' kapsayıcı içinde depolanır. Hiyerarşik bir OU yapılandırılmış yapı içi. Yönetilen etki alanınızı hala basit düz bir OU yapıya sahip.

## <a name="exclusions---what-isnt-synchronized-to-your-managed-domain"></a>Dışlamalar - ne yönetilen etki alanınıza eşitlenmemiş
Aşağıdaki nesne veya öznitelik Azure AD kiracınıza veya yönetilen etki alanınız için eşitlenmedi:

* **Öznitelikler dışarıda:** belirli öznitelikler Azure AD kiracınız için Azure AD Connect'i kullanarak, şirket içi etki alanınızdan eşitlemeden dışlamak tercih edebilirsiniz. Dışlanan özniteliklerden, yönetilen etki alanında kullanılabilir değil.
* **Grup ilkeleri:** şirket içi etki alanınızda yapılandırılan Grup İlkesi, yönetilen etki alanınıza eşitlenmez.
* **Sysvol paylaşımının:** benzer şekilde, Sysvol paylaşımı, şirket içi etki alanınızda içeriğini yönetilen etki alanınıza eşitlenmez.
* **Bilgisayar nesnelerini:** bilgisayar nesneleri, şirket içi etki alanına katılmış bilgisayarlar için yönetilen etki alanınıza eşitlenir. Bu bilgisayarlar değil, yönetilen etki alanınız ile güven ilişkisine sahip ve şirket içi etki alanınıza yalnızca ait. Yönetilen etki alanında yalnızca siz açıkça etki alanı yönetilen etki alanına katılmış bilgisayarlar için bilgisayar nesneleri bulun.
* **Kullanıcılar ve gruplar için SIDHistory öznitelikler:** birincil kullanıcısı ve birincil grup SID şirket etki alanınızdan yönetilen etki alanınıza eşitlenir. Ancak, kullanıcılar ve gruplar için varolan SIDHistory öznitelikleri, yönetilen etki alanınıza, şirket içi etki alanınızdan eşitlenmez.
* **Kuruluş birimi (OU) yapıları:** şirket içi etki alanınızda tanımlanan kuruluş birimleri, yönetilen etki alanınız için eşitleme değil. Yönetilen etki alanınızda iki yerleşik OU'lar vardır. Varsayılan olarak, düz bir OU yapısı, yönetilen etki alanınız vardır. Ancak tercih edebilirsiniz [yönetilen etki alanınızda özel bir OU oluşturmanız](active-directory-ds-admin-guide-create-ou.md).

## <a name="how-specific-attributes-are-synchronized-to-your-managed-domain"></a>Özel öznitelikler yönetilen etki alanınıza eşitlenir nasıl
Aşağıdaki tabloda bazı ortak öznitelikleri listeler ve yönetilen etki alanınıza nasıl eşitleneceğini açıklar.

| Yönetilen etki alanınızda özniteliği | Kaynak | Notlar |
|:--- |:--- |:--- |
| UPN |Azure AD kiracınızda kullanıcının UPN özniteliği |Azure AD kiracınıza UPN özniteliği, yönetilen etki alanınıza olarak eşitlenir. Bu nedenle, yönetilen etki alanınıza oturum açmak için en güvenilir yolu, UPN kullanıyor. |
| SAMAccountName |Kullanıcının mailNickname Azure AD kiracınızda özniteliği ya da otomatik olarak oluşturulan |Azure AD kiracınızda mailNickname özniteliğinden SAMAccountName özniteliği kaynaklıdır. Birden çok kullanıcı hesapları aynı mailNickname özniteliği varsa, SAMAccountName otomatik olarak üretilir. Kullanıcının mailNickname veya UPN öneki 20 karakterden uzunsa SAMAccountName otomatik SAMAccountName öznitelikleri 20 karakter sınırını karşılamak için olarak üretilir. |
| Parolalar |Azure AD kiracınıza kullanıcının parolasından |(Ek kimlik bilgileri olarak da bilinir) NTLM veya Kerberos kimlik doğrulaması için gerekli kimlik bilgisi karmalarını Azure AD kiracınıza eşitlenir. Azure AD kiracınıza eşitlenmiş Kiracı ise, bu kimlik bilgileri, şirket içi etki alanından elde edilir. |
| Birincil kullanıcı/Grup SID |Otomatik olarak oluşturulan |Kullanıcı/grup hesapları için birincil SID, yönetilen etki alanında otomatik olarak üretilir. Bu öznitelik birincil kullanıcı/Grup SID şirket içi nesnesinin eşleşmiyor AD etki alanı. Yönetilen etki alanı farklı bir SID ad alanı, şirket içi etki olduğundan bu eşleşmemesidir. |
| Kullanıcılar ve gruplar için SID Geçmişi |Şirket içi birincil kullanıcı ve Grup SID |Kullanıcıları ve grupları, yönetilen etki alanınızdaki SIDHistory özniteliğini karşılık gelen birincil kullanıcı veya grubun SID şirket içi etki alanınızdaki eşleşecek şekilde ayarlanır. Bu özellik, yeniden ACL kaynaklara gerekmediği yükseltme-ve-shift yönetilen etki alanı için şirket içi uygulamaların kolaylaştırmak yardımcı olur. |

> [!NOTE]
> **UPN biçimini kullanarak yönetilen etki alanında oturum açın:** SAMAccountName özniteliği, yönetilen etki alanınızdaki bazı kullanıcı hesapları için otomatik olarak oluşturulan olabilir. Birden çok kullanıcı aynı mailNickname özniteliği veya aşırı uzun UPN önekleri kullanıcınız varsa, bu kullanıcılar için SAMAccountName otomatik olarak oluşturulan olabilir. Bu nedenle, SAMAccountName biçimi (örneğin, ' CONTOSO100\joeuser') her zaman etki alanında oturum açmak için güvenilir bir yolu değil. Kullanıcıların otomatik olarak oluşturulan SAMAccountName kendi UPN önekten farklı olabilir. UPN biçimini kullanın (örneğin, 'joeuser@contoso100.com') yönetilen etki alanı için güvenilir bir şekilde oturum açmak için.
>
>

### <a name="attribute-mapping-for-user-accounts"></a>Kullanıcı hesapları için özellik eşlemesi
Aşağıdaki tabloda Azure AD kiracınıza nesneleri yönetilen etki alanınızdaki karşılık gelen öznitelikleri eşitlenir kullanıcı için nasıl özel öznitelikler gösterilmektedir.

| Azure AD kiracınızda kullanıcı özniteliği | Yönetilen etki alanınızdaki kullanıcı özniteliği |
|:--- |:--- |
| accountEnabled |userAccountControl (ayarlar veya bit ACCOUNT_DISABLED temizler) |
| city |m |
| Ülke |Ortak |
| bölüm |bölüm |
| displayName |displayName |
| facsimileTelephoneNumber |facsimileTelephoneNumber |
| givenName |givenName |
| İş Unvanı |başlık |
| posta |posta |
| mailNickname |msDS-AzureADMailNickname |
| mailNickname |SAMAccountName (bazen otomatik olarak oluşturulan olabilir) |
| Mobil |Mobil |
| objectID |msDS-AzureADObjectId |
| onPremiseSecurityIdentifier |SID Geçmişi |
| passwordPolicies |userAccountControl (ayarlar veya bit DONT_EXPIRE_PASSWORD temizler) |
| physicalDeliveryOfficeName |physicalDeliveryOfficeName |
| posta kodu |posta kodu |
| preferredLanguage |preferredLanguage |
| durum |St |
| StreetAddress |StreetAddress |
| Soyadı |sn |
| telephoneNumber |telephoneNumber |
| userPrincipalName |userPrincipalName |

### <a name="attribute-mapping-for-groups"></a>Gruplar için özellik eşlemesi
Aşağıdaki tabloda Azure AD kiracınıza nesneleri yönetilen etki alanınızdaki karşılık gelen öznitelikleri eşitlenir grubu için belirli öznitelikleri gösterir.

| Azure AD kiracınıza Grup özniteliği | Yönetilen etki alanınızdaki Grup özniteliği |
|:--- |:--- |
| displayName |displayName |
| displayName |SAMAccountName (bazen otomatik olarak oluşturulan olabilir) |
| posta |posta |
| mailNickname |msDS-AzureADMailNickname |
| objectID |msDS-AzureADObjectId |
| onPremiseSecurityIdentifier |SID Geçmişi |
| securityEnabled |groupType |

## <a name="password-hash-synchronization-and-security-considerations"></a>Parola karma eşitlemesi ve güvenlik konuları
Azure AD Etki Alanı Hizmetleri'ni etkinleştirdiğinizde, Azure AD dizininizi oluşturur ve parola karmaları NTLM ve Kerberos uyumlu biçimlerinden birinde depolar. 

Mevcut bulut kullanıcı hesapları için Azure AD hiçbir zaman düz metin parolalarını depolar beri bu karmalar otomatik olarak oluşturulamaz. Bu nedenle, Microsoft gerektirir [parolalarını sıfırlama/Değiştir bulut-kullanıcıları](active-directory-ds-getting-started-password-sync.md) oluşturulan ve Azure AD'de depolanan kullanıcıların parola karmalarını sırayla. Azure AD Etki Alanı Hizmetleri'ni etkinleştirdikten sonra Azure AD'de oluşturulan bulut kullanıcı hesabı için parola karmaları oluşturulan ve NTLM ve Kerberos uyumlu biçimlerde depolanır. 

Gelen eşitlenen kullanıcı hesapları için şirket içi AD kullanarak Azure AD Connect eşitleme yapmanız [parola karmaları NTLM ve Kerberos uyumlu biçimlerde eşitlemek için Azure AD Connect yapılandırma](active-directory-ds-getting-started-password-sync-synced-tenant.md).

NTLM ve Kerberos uyumlu parola karmaları, her zaman Azure AD içinde şifrelenmiş bir biçimde depolanır. Bu karmalar, yalnızca Azure AD etki alanı Hizmetleri sahip şifre çözme anahtarlarını erişimi şekilde şifrelenir. Başka bir hizmet veya Azure AD bileşeninde şifre çözme anahtarlarını erişebilir. Şifreleme anahtarları benzersiz başına-Azure AD Kiracı ' dir. Azure AD etki alanı Hizmetleri yönetilen etki alanınız için etki alanı denetleyicilerinde parola karmaları eşitler. Bu parola karmaları depolanır ve parolaları nasıl depolandığını ve Windows Server AD etki alanı denetleyicilerinde güvenli benzer bu etki alanı denetleyicilerindeki güvenli. Bu yönetilen etki alanı denetleyicileri için diskleri bekleyen şifrelenir.

## <a name="objects-that-are-not-synchronized-to-your-azure-ad-tenant-from-your-managed-domain"></a>Azure AD kiracınız, yönetilen etki alanınızdan eşitlenmez nesneleri
Bu makalenin önceki bölümde açıklandığı gibi yönetilen etki alanınızdan eşitleme Azure AD kiracınıza dön yoktur. Tercih edebilirsiniz [özel kuruluş birimi (OU) oluşturun](active-directory-ds-admin-guide-create-ou.md) yönetilen etki alanınızdaki. Ayrıca, diğer OU'lar, kullanıcıların, grupların veya bu özel OU içinde hizmet hesapları oluşturabilirsiniz. İçinde özel OU'lar oluşturulan nesneler hiçbiri, Azure AD kiracınıza eşitlenir. Bu nesneler yalnızca yönetilen etki alanınızın içinde kullanılabilir. Bu nedenle, bu nesnelerin Azure AD PowerShell cmdlet'leri, Azure AD grafik API'si veya Azure AD yönetim kullanıcı Arabirimi kullanarak görünür değildir.

## <a name="related-content"></a>İlgili İçerik
* [Özellikler - Azure AD etki alanı Hizmetleri](active-directory-ds-features.md)
* [Dağıtım senaryoları - Azure AD etki alanı Hizmetleri](active-directory-ds-scenarios.md)
* [Azure AD etki alanı Hizmetleri için ağ konuları](active-directory-ds-networking.md)
* [Azure AD etki alanı Hizmetleri ile çalışmaya başlama](active-directory-ds-getting-started.md)
