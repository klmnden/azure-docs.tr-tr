---
title: Privileged Identity Management Azure kaynakları için - rollerini etkinleştir | Microsoft Docs
description: PIM rolleri etkinleştirmeyi açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/02/2018
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 3e5456e7a632639cb82d7ba2b2e073938b1798ef
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="privileged-identity-management---resource-roles---activate"></a>Privileged Identity Management - kaynak rolleri - etkinleştirme
Azure kaynakları için rol etkinleştirme, etkinleştirme için gelecekteki bir tarih/saat zamanlayın ve belirli etkinleştirme süresi (yöneticiler tarafından yapılandırılmış) maksimum içinde seçmek uygun Rol üyeleri izin veren yeni bir deneyim sunar. Hakkında bilgi edinin [burada Azure AD rolleri etkinleştirme](../active-directory-privileged-identity-management-how-to-activate-role.md).

## <a name="activate-roles"></a>Rollerini etkinleştir
Gidin My roller bölümünde sol gezinti çubuğunda. "Etkinleştir" içine etkinleştirmek istediğiniz rolü için'ı tıklatın.
![](media/azure-pim-resource-rbac/rbac-roles.png)

İstenen başlangıç tarihini ve saatini rolünü etkinleştirmek için etkinleştirme menüsünden girin. İsteğe bağlı olarak etkinleştirme süresini azaltmak (süre rolü etkin) ve gerekirse; bir gerekçe girin Etkinleştir'i tıklatın.

Başlangıç tarihi ve saati değiştirilmeyen, rol saniye içinde etkinleştirilir. Sitem rollerini sayfasında etkinleştirme başlık iletisi için bir rol sıraya görürsünüz. Bu iletiyi temizlemek için Yenile düğmesini tıklatın.

![](media/azure-pim-resource-rbac/rbac-activate-notification.png)

Etkinleştirme bir gelecek tarih zaman için planlandıysa bekleyen istek sol gezinti menüsünde bekleyen istekler sekmesinde görünür. Rol etkinleştirme artık gerekli olması durumunda, kullanıcı isteği sayfasının sağ tarafında iptal düğmesine tıklayarak iptal edebilirsiniz.

![](media/azure-pim-resource-rbac/rbac-activate-pending.png)


## <a name="just-enough-administration"></a>Tam yetecek kadar yönetim

Kaynak rol atamalarınızı ile tam olarak yeterli yönetim (JEA) en iyi yöntemler kullanarak Azure kaynakları için PIM basittir. Kullanıcı ve Grup üyeleri ile Azure abonelikleri veya kaynak grupları atamalarını kendi mevcut rol ataması azaltılmış bir kapsamda etkinleştirebilirsiniz. 

Arama sayfasından yönetmeniz gereken bağımlı kaynak bulunamıyor.

![](media/azure-pim-resource-rbac/azure-resources-02.png)

My rolleri sol gezinti menüsünde ve etkinleştirmek için uygun rolü seçin. Rolü kaynak grubu yerine abonelik aşağıda gösterildiği gibi atandı beri bildirimi atama türü devralınır.

![](media/azure-pim-resource-rbac/my-roles-02.png)
