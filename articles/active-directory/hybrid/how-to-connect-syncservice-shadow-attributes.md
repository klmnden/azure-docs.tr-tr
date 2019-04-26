---
title: Azure AD Connect eşitleme hizmeti gölge öznitelikleri | Microsoft Docs
description: Gölge öznitelikler Azure AD Connect eşitleme hizmetinde nasıl çalıştığını açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
origin.date: 07/13/2017
ms.date: 04/09/2019
ms.subservice: hybrid
ms.author: v-junlch
ms.collection: M365-identity-device-management
ms.openlocfilehash: 10a4078f49abbdf431f42c6cde7cf882112e5848
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60384718"
---
# <a name="azure-ad-connect-sync-service-shadow-attributes"></a>Azure AD Connect eşitleme hizmeti gölge öznitelikleri
Şirket içi Active Directory'nizde oldukları gibi çoğu öznitelikler Azure AD'de aynı şekilde temsil edilir. Ancak bazı özel işlem bazı özniteliklere sahip ve öznitelik değeri Azure AD'de, Azure AD Connect eşitler daha farklı olabilir.

## <a name="introducing-shadow-attributes"></a>Gölge öznitelikleri ile tanışın
Bazı öznitelikler Azure AD'de iki temsilleri olabilir. Hem şirket içi değeri hem de bir hesaplanan değeri depolanır. Bu ek öznitelikler gölge öznitelikleri çağrılır. Bu davranış gördüğünüz iki en yaygın özniteliklerinin **userPrincipalName** ve **proxyAddress**. Doğrulanmamış etki alanı temsil eden içinde bu özniteliklerin değerleri olduğunda öznitelik değerleri değişiklik olur. Ancak CONNECT'te eşitleme altyapısı gölge öznitelik değerinde bunu kendi bakış açısına okur, öznitelik Azure AD tarafından onaylanmıştır.

Azure portal veya PowerShell ile kullanarak gölge öznitelikleri göremez. Ancak kavramı anlamak, belirli senaryolar farklı değerler şirket içi özniteliğine sahip olduğunda ve bulutta gidermenize yardımcı olur.

Davranışını daha iyi anlamak için bu örnekte, Fabrikam bakın:  
![Etki Alanları](./media/how-to-connect-syncservice-shadow-attributes/domains.png)  
Kendi şirket içi Active Directory içinde sahip oldukları birden fazla UPN soneki, ancak yalnızca bir doğruladıkları.

### <a name="userprincipalname"></a>userPrincipalName
Bir kullanıcı, aşağıdaki öznitelik değerlerini doğrulanmamış bir etki alanı vardır:

| Öznitelik | Değer |
| --- | --- |
| Şirket içi userPrincipalName | lee.sperry@fabrikam.com |
| Azure AD shadowUserPrincipalName | lee.sperry@fabrikam.com |
| Azure AD userPrincipalName | lee.sperry@fabrikam.partner.onmschina.cn |

UserPrincipalName özniteliği, PowerShell kullanırken görmeyi değerdir.

Gerçek şirket içinde öznitelik değeri, fabrikam.com etki alanını doğrulama, Azure AD'de depolandığı için Azure AD userPrincipalName özniteliği shadowUserPrincipalName değerini güncelleştirir. Güncelleştirilecek Azure AD Connect için bu değerleri değişiklikleri eşitlemek gerekmez.

### <a name="proxyaddresses"></a>proxyAddresses
Yalnızca doğrulanmış etki alanları dahil olmak üzere aynı işlemi proxyAddresses, ancak bazı ilave bir mantık daha da oluşur. Doğrulanmış etki alanları için onay için posta kutusu kullanıcıların yalnızca gerçekleşir. Bir posta etkin bir kullanıcı veya kişi temsil eden bir kullanıcı başka bir Exchange kuruluşunda ve bu nesnelere Proxyaddresses'teki herhangi bir değer ekleyebilirsiniz.

Bir posta kutusu kullanıcısının, şirket içinde veya Exchange Online'da yalnızca doğrulanmış etki alanları için değerler görüntülenir. Şu şekilde görünebilir:

| Öznitelik | Değer |
| --- | --- |
| Şirket içi proxyAddresses | SMTP:abbie.spencer@fabrikamonline.com</br>smtp:abbie.spencer@fabrikam.com</br>smtp:abbie@fabrikamonline.com |
| Exchange Online proxyAddresses | SMTP:abbie.spencer@fabrikamonline.com</br>smtp:abbie@fabrikamonline.com</br>SIP:abbie.spencer@fabrikamonline.com |

Bu durumda **smtp:abbie.spencer\@fabrikam.com** olduğundan bu etki alanı doğrulanmadı kaldırıldı. Ancak aynı zamanda eklenen Exchange **SIP:abbie.spencer\@fabrikamonline.com**. Fabrikam, Lync/Skype Kurumsal şirket içi, ancak Azure AD'ye kullanmamış ve Exchange Online hazırlamak için.

ProxyAddresses için bu mantıksal olarak adlandırılır **ProxyCalc**. ProxyCalc çağrıldığı bir kullanıcının her değişiklik olduğunda:

- Kullanıcı için Exchange kullanıcı lisanslı değil olsa bile, Exchange Online içeren bir hizmet planı atanmış olabilir. Örneğin, kullanıcının Office E3 SKU atanmış, ancak yalnızca SharePoint Online atandı. Posta yine de şirket içi olsa bile bu geçerlidir.
- Öznitelik msExchRecipientTypeDetails bir değere sahip.
- ProxyAddresses veya userPrincipalName bir değişiklik yapın.

ProxyCalc bir kullanıcının bir değişiklik işlemek için biraz zaman alabilir ve Azure AD Connect dışarı aktarma işlemine zaman uyumlu değil.

> [!NOTE]
> Bu konudaki belgelenmemiş Gelişmiş senaryolar için bazı ek davranışları ProxyCalc mantığı vardır. Bu konuda davranışlarını anlamak ve belge tüm iç mantıksal değil için sağlanır.

### <a name="quarantined-attribute-values"></a>Karantinaya alınan öznitelik değerleri
Yinelenen öznitelik değerleri olduğunda gölge öznitelikler de kullanılır. Daha fazla bilgi için [yinelenen öznitelik dayanıklılığı](how-to-connect-syncservice-duplicate-attribute-resiliency.md).

## <a name="see-also"></a>Ayrıca bkz.
* [Azure AD Connect eşitleme](how-to-connect-sync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md).

<!-- Update_Description: wording update -->