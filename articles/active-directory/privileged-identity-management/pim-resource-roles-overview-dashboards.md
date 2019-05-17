---
title: Kaynak Pano PIM - Azure Active Directory erişim gözden geçirmesi gerçekleştirme | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM) erişim gözden geçirmesi gerçekleştirmek için bir kaynak Pano kullanmayı açıklar.
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
ms.subservice: pim
ms.date: 03/30/2018
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: e5404d1b85821458aedef64b72ae635ea49aa1ff
ms.sourcegitcommit: 6ea7f0a6e9add35547c77eef26f34d2504796565
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65602480"
---
# <a name="use-a-resource-dashboard-to-perform-an-access-review-in-pim"></a>PIM'de erişim gözden geçirmesi gerçekleştirmek için bir kaynak Panosu'nu kullanın

Kaynak Pano, Azure Active Directory (Azure AD) Privileged Identity Management (PIM) erişim gözden geçirmesi gerçekleştirmek için kullanabilirsiniz. Yönetici görünümü Pano üç birincil bileşenden oluşur:

- Kaynak rol etkinleştirmeleri grafik gösterimi.
- Rol atamaları atamaya göre dağılımını gösteren iki grafik türü.
- Yeni rol atamaları için ilgili bir veri alanı.

![Yönetici görünümü Pano graflar ve grafikler gösteren ekran görüntüsü](media/pim-resource-roles-overview-dashboards/rbac-overview-top.png)

![Yönetici görünümü panosunun ekran görüntüsü, gösteren veriler listelenir.](media/pim-resource-roles-overview-dashboards/role-settings.png)

Kaynak rol etkinleştirmeleri grafik gösterimi son yedi gün kapsar. Bu veriler, seçili kaynak için kapsamlı ve en sık kullanılan rolleri (sahibi, katkıda bulunan, kullanıcı erişimi Yöneticisi) ve birleşik tüm roller için etkinleştirme görüntüler.

Etkinleştirmeleri graf sağında, iki grafik hem kullanıcılar hem de grupları atama türü tarafından rol atamalarını dağılımını görüntüleyin. Değerin bir yüzde değeri (veya tersi), bir dilimi grafiğin seçerek değiştirebilirsiniz.

Grafikleri, son 30 gün ve roller (Azalan) toplam atamalara göre sıralanmış listesini üzerinde kullanıcılar ve gruplar ile yeni rol atamaları sayısı bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [PIM hizmetinde Azure kaynak rolleri için erişim gözden geçirmesi başlatma](pim-resource-roles-start-access-review.md) 
