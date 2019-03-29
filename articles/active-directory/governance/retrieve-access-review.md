---
title: Grupları veya uygulamalarda erişim gözden geçirmeleri - Azure Active Directory erişim gözden geçirmesi sonuçlarını alma | Microsoft Docs
description: Grup üyelerini veya Azure Active Directory erişim gözden geçirmeleri uygulama erişimi için erişim gözden geçirmesi sonuçlarını alma hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 06/21/2018
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: eae4bafb3eefcee2785c784030d7be8dde66988e
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2019
ms.locfileid: "58578722"
---
# <a name="retrieve-access-review-results-for-groups-or-applications-in-azure-ad-access-reviews"></a>Erişim gözden geçirmesi almak sonuçları grupları veya Azure AD uygulamaları için erişim gözden geçirmeleri

Yöneticiler, bir uygulamaya atanmış grup üyeleri veya kullanıcılar için [bir erişim gözden geçirmesi oluşturmak](create-access-review.md) üzere Azure Active Directory’yi (Azure AD) kullanabilir.  İçinde bir kullanıcı **genel yönetici**, **Kullanıcı Yöneticisi**, **Güvenlik Yöneticisi** veya **güvenlik okuyucusu** rolü aynı zamanda erişim gözden geçirmesi sonuçlarını okuyun.  Kullanıcılar bu rollerden birine atamak için ayrıcalıklı Rol Yöneticisi Azure AD PIM bir kullanıcı rolü etkinleştirmek uygun hale getirmek için kullanabilirsiniz veya kalıcı olarak genel yönetici olabilir [bir kullanıcı rolüne atamak](../fundamentals/active-directory-users-assign-role-azure-portal.md).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="locating-an-access-review"></a>Erişim gözden geçirmesini bulma

Erişim gözden geçirmesini hangi programın içerdiğini biliyorsanız erişim gözden geçirmeleri sayfasına gidin, **Programlar**’ı seçin ve erişim gözden geçirmesi denetimini içeren programı seçin.  Ardından **Denetimler**’i ve erişim gözden geçirmesi denetimini seçin. Programda çok sayıda denetim varsa, belirli bir türdeki denetimleri filtreleyebilir ve durumlarına göre sıralayabilirsiniz. Ayrıca, erişim gözden geçirmesi denetiminin adına ya da onu oluşturan sahibin görünen adına göre arama yapabilirsiniz. 

Erişim gözden geçirmesini içeren programı bilmiyorsanız, erişim gözden geçirmesi sayfasına gidin ve **Denetimler**’i seçin.  Belirli bir türdeki denetimleri filtreleyebilir ve durumlarına göre sıralayabilirsiniz ve erişim gözden geçirmesi denetiminin adına ya da onu oluşturan sahibin görünen adına göre arama yapabilirsiniz. 

## <a name="retrieving-the-results-for-a-one-time-access-review"></a>Tek seferlik erişim gözden geçirmesi için sonuçları alma

Gözden geçirme yineleme türü bir kez ise, etkin bir erişim gözden geçirmesinin ilerleme durumu ve tamamlanmış erişim gözden geçirmesinin sonuçları, **Sonuçlar** bölümünden alınabilir.  Erişimi gözden geçirilen bir kullanıcının görünen adını veya kullanıcı asıl adını yazarak, o kullanıcının erişimini görüntüleyebilirsiniz.  Tamamlanmış bir erişim gözden geçirmesinin tüm sonuçlarını almak için **İndir** düğmesine tıklayın.

## <a name="retrieving-the-results-for-multiple-instances-of-a-recurring-access-review"></a>Yinelenen bir erişim gözden geçirmesinin birden fazla örneğine ait sonuçları alma

Yinelenen bir etkin erişim gözden geçirmesini görüntülemek için **Sonuçlar** bölümüne tıklayın.  Erişimi gözden geçirilmekte olan bir kullanıcının görünen adını ya da kullanıcı asıl adını yazabilirsiniz.

Yinelenen bir erişim gözden geçirmesinin tamamlanmış bir örneğine ait sonuçları görüntülemek için **Gözden geçirme geçmişi**’ni seçin, ardından örneğin başlangıç ve bitiş tarihine göre tamamlanmış erişim gözden geçirmesi örneklerinin listesinden belirli bir örneği seçin.   Bu örneğin sonuçları, **Sonuçlar** bölümünden alınabilir.  Erişimi gözden geçirilen bir kullanıcının görünen adını veya kullanıcı asıl adını yazarak, o kullanıcının erişimini görüntüleyebilirsiniz.  Yinelenen bir erişim gözden geçirmesinin tamamlanmış örneğine ait tüm sonuçları almak için **İndir** düğmesine tıklayın.


## <a name="removing-users-from-an-access-review"></a>Bir erişim gözden geçirmesinden kullanıcı kaldırma

Varsayılan olarak, silinmiş bir kullanıcı Azure AD’de 30 gün boyunca silinmiş olarak kalır ve bu süre boyunca gerekirse bir yönetici tarafından geri alınabilir.  30 gün sonra bu kullanıcı kalıcı olarak silinir.  Ayrıca, bir Genel Yönetici bu süreye ulaşılmadan önce Azure Active Directory portalını kullanarak [kısa süre önce silinmiş bir kullanıcıyı kalıcı olarak silebilir](../fundamentals/active-directory-users-restore.md).  Bir kullanıcı kalıcı olarak silindikten sonra, bu kullanıcıya ilişkin sonraki veriler etkin erişim gözden geçirmelerinden kaldırılır.  Silinmiş kullanıcılara ilişkin denetim bilgileri, denetim günlüğünde kalır.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD erişim gözden geçirmeleriyle kullanıcı erişimini yönetme](manage-user-access-with-access-reviews.md)
- [Azure AD erişim gözden geçirmeleriyle konuk erişimini yönetme](manage-guest-access-with-access-reviews.md)
- [Azure AD erişim gözden geçirmeleri için programları ve denetimleri yönetme](manage-programs-controls.md)
- [Grupları ve uygulamaları, erişim gözden geçirmesi oluştur](create-access-review.md)
- [Bir Azure AD yönetici rolündeki kullanıcılar için erişim gözden geçirmesi oluşturma](../privileged-identity-management/pim-how-to-start-security-review.md)


