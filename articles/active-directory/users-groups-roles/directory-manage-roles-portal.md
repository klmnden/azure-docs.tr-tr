---
title: Görüntüleyin ve rolü izinleri Azure Active Directory'de yönetici atama | Microsoft Docs
description: Artık bkz ve portalında bir Azure AD Yönetici rolü üyeleri yönetebilir. Kişiler için sık rol atamalarını yönetmek.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 09/25/2018
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.openlocfilehash: cb2e5286eb8e910b555e221242a735f00dff4778
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47182833"
---
# <a name="view-and-assign-administrator-roles-in-azure-active-directory"></a>Görüntüleme ve Azure Active Directory'de yönetici rolleri atama

Artık bkz ve Azure Active Directory portalındaki yönetici rollerinin tüm üyeleri Yönet. Sık rol atamalarını yönetmek, büyük olasılıkla bu deneyim tercih eder. Ve "denetle Bu rolleri gerçekten ne?" merak şimdiye kadar Azure AD yönetici rollerinin her biri için izinleri ayrıntılı bir listesini görebilirsiniz.

Kendi izinlerinizi de görmek daha kolaydır. Tıklayın **rolünüz** kullanıcı sayfanıza hızlı erişim için tüm etkin atanan rollerin listesini alın. Rol ayrıntılı açıklamasını açmak için her satırın sağ tıklayın.

![Azure AD portalında rollerinin listesi](./media/directory-manage-roles-portal/role-list.png)

Role atanan kullanıcılar görüntülemek bir rol için satırı seçin. Seçebileceğiniz **PIM yönetme** ek yönetim özellikleri için. Ayrıcalıklı rol yöneticileri "Kalıcı" olarak değiştirebilirsiniz (her zaman etkin rolü) atamaları "Uygun" (rolündeki yalnızca yükseltilmiş olduğunda). PIM yoksa, yine de seçebilirsiniz **PIM yönetme** deneme sürümüne kaydolmak için. Privileged Identity Management gerektiren bir [Azure AD Premium P2 lisansı planı](../privileged-identity-management/subscription-requirements.md).

![bir yönetici rolü üyeleri listesi](./media/directory-manage-roles-portal/member-list.png)

Genel yönetici veya bir ayrıcalıklı Rol Yöneticisi, kolayca ekleme veya üyeleri kaldırın, listeyi filtreleyin veya rol atanmış kullanıcıların etkin görmek için bir üyesini seçin.

## <a name="view-role-permissions"></a>Rol izinleri görüntüle

Bir rolün üyeleri görüntülenirken seçin **açıklama** rol ataması tarafından verilen izinler tam listesini görmek için. Sayfa size yol Dizin rolleri yönetme ile ilgili belgelere bağlantılar içerir.

![Yönetici rolü için izinleri listesi](./media/directory-manage-roles-portal/role-description.png)


## <a name="next-steps"></a>Sonraki adımlar

* Bizimle paylaşmayı rahatça [Azure AD yönetim rolleri Forumu](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032).
* Rolleri ve Yönetici rolü atama hakkında daha fazla bilgi için bkz. [yönetici rolleri atama](directory-assign-admin-roles.md).
* Varsayılan kullanıcı izinleri için bkz. bir [varsayılan Konuk ve üye kullanıcı izinlerini karşılaştırma](../fundamentals/users-default-permissions.md).
