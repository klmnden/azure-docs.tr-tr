---
title: Azure AD Connect eşitleme hizmeti gölge öznitelikleri | Microsoft Docs
description: Gölge öznitelikler Azure AD Connect eşitleme hizmetinin nasıl çalıştığı açıklanmaktadır.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: bd1ede2bf8ff642b7be0869e54a6f037b01dd262
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34593427"
---
# <a name="azure-ad-connect-sync-service-shadow-attributes"></a>Azure AD Connect eşitleme hizmeti gölge öznitelikleri
Şirket içi Active Directory oldukları gibi özelliklerin çoğu Azure AD'de aynı şekilde gösterilir. Ancak bazı özel işleme bazı özniteliklere sahip ve Azure AD öznitelik değerinde Azure AD Connect eşitler daha farklı olabilir.

## <a name="introducing-shadow-attributes"></a>Gölge öznitelikleri Tanıtımı
Bazı öznitelikler Azure AD içinde iki sunumu sahiptir. Şirket içi değer ve bir hesaplanan değer depolanır. Bu ek öznitelikler gölge öznitelikleri denir. Bu davranış gördüğünüz iki en yaygın öznitelikleri **userPrincipalName** ve **proxyAddress**. Doğrulanmamış etki alanları temsil eden içinde bu özniteliklerin değerleri olduğunda öznitelik değerleri değişikliği olur. Ancak eşitleme altyapısı Bağlan gölge öznitelik değeri bunu kendi açısından okur, öznitelik Azure AD tarafından onaylanmıştır.

Azure portal veya PowerShell ile kullanarak gölge özniteliklerini göremezsiniz. Ancak kavramı anlamak, belirli senaryolar farklı değerler şirket içi özniteliğine sahip olduğunda ve bulutta gidermenize yardımcı olur.

Davranış daha iyi anlamak için bu örnekten Fabrikam bakın:  
![Etki Alanları](./media/active-directory-aadconnectsyncservice-shadow-attributes/domains.png)  
Kendi şirket içi Active Directory'de birden çok UPN soneki olabilirler, ancak bunlar yalnızca biri doğruladıktan.

### <a name="userprincipalname"></a>userPrincipalName
Bir kullanıcı, aşağıdaki öznitelik değerlerini doğrulanmamış bir etki alanı vardır:

| Öznitelik | Değer |
| --- | --- |
| Şirket içi userPrincipalName | lee.sperry@fabrikam.com |
| Azure AD shadowUserPrincipalName | lee.sperry@fabrikam.com |
| Azure AD userPrincipalName | lee.sperry@fabrikam.onmicrosoft.com |

UserPrincipalName özniteliğinin PowerShell kullanırken gördüğünüz değerdir.

Gerçek şirket içi öznitelik değeri ne zaman fabrikam.com etki alanını doğrulayın, Azure AD içinde kaydedildiği Azure AD userPrincipalName özniteliği shadowUserPrincipalName değeri ile güncelleştirir. Değişiklikleri güncelleştirilmesi için Azure AD Connect bu değerleri için eşitleme gerekmez.

### <a name="proxyaddresses"></a>proxyAddresses
Yalnızca doğrulanmış etki alanları dahil olmak üzere aynı işlemi proxyAddresses, ancak bazı ilave bir mantık daha da oluşur. Doğrulanmış etki alanları için onay yalnızca posta kutusu kullanıcılar için gerçekleşir. Bir posta etkin bir kullanıcıya veya kişi temsil eden bir kullanıcı başka bir Exchange kuruluşunda ve bu nesnelere Proxyaddresses'teki tüm değerleri ekleyebilirsiniz.

Her iki şirket içi posta kutusu bir kullanıcı için ya da Exchange Online'da yalnızca değerleri doğrulanmış etki alanları için görünür. Şu şekilde görünebilir:

| Öznitelik | Değer |
| --- | --- |
| Şirket içi proxyAddresses | SMTP:abbie.spencer@fabrikamonline.com</br>smtp:abbie.spencer@fabrikam.com</br>smtp:abbie@fabrikamonline.com |
| Exchange Online proxyAddresses | SMTP:abbie.spencer@fabrikamonline.com</br>smtp:abbie@fabrikamonline.com</br>SIP:abbie.spencer@fabrikamonline.com |

Bu durumda **smtp:abbie.spencer@fabrikam.com** bu etki alanı doğrulanmadı beri kaldırıldı. Ancak aynı zamanda eklenen Exchange **SIP:abbie.spencer@fabrikamonline.com**. Fabrikam Lync/Skype şirket içi ancak Azure AD kullanmamış ve Exchange Online hazırlamak için.

Bu mantığı proxyAddresses olarak adlandırılır **ProxyCalc**. ProxyCalc her değişikliğe bir kullanıcı olarak çağrılan zaman:

- Kullanıcı, kullanıcı Exchange için lisanslı değil olsa bile Exchange Online içeren bir hizmet planına atanmıştır. Örneğin, kullanıcının Office E3 SKU atanmış, ancak yalnızca SharePoint çevrimiçi atandı. Posta hala şirket içi olsa bile bu geçerlidir.
- Öznitelik msExchRecipientTypeDetails bir değer içeriyor.
- Bir değişiklik proxyAddresses veya userPrincipalName olun.

ProxyCalc bir kullanıcının bir değişiklik işlemek için biraz zaman alabilir ve Azure AD Connect dışarı aktarma işlemine zaman uyumlu değil.

> [!NOTE]
> Bu konudaki belgelenmemiş Gelişmiş senaryolar için bazı ek davranışları ProxyCalc mantığı vardır. Bu konu, davranışlarını anlamak ve tüm İç mantık belge değil sağlanır.

### <a name="quarantined-attribute-values"></a>Karantinaya alınan öznitelik değerleri
Yinelenen öznitelik değerleri olduğunda gölge öznitelikler de kullanılır. Daha fazla bilgi için bkz: [yinelenen öznitelik dayanıklılık](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md).

## <a name="see-also"></a>Ayrıca bkz.
* [Azure AD Connect eşitleme](active-directory-aadconnectsync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md).
