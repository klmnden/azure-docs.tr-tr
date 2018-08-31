---
title: Erişim gözden geçirmesi - Azure gerçekleştirmek için bir kaynak Panosu'nu kullanın | Microsoft Docs
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
ms.component: pim
ms.date: 03/30/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 20172cf7413397aedc4b3c32d0f1419531a2588a
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2018
ms.locfileid: "43188506"
---
# <a name="use-a-resource-dashboard-to-perform-an-access-review"></a>Erişim gözden geçirmesi gerçekleştirmek için bir kaynak Panosu'nu kullanın

Erişim gözden geçirmesi Azure kaynakları için Privileged Identity Management (PIM) gerçekleştirmek için bir kaynak panoyu kullanabilirsiniz. Yönetici görünümü Pano üç birincil bileşenden oluşur:

- Kaynak rol etkinleştirmeleri grafik gösterimi.
- Rol atamaları atamaya göre dağılımını gösteren iki grafik türü.
- Yeni rol atamaları için ilgili bir veri alanı.

![Yönetici görünümü Pano graflar ve grafikler gösteren ekran görüntüsü](media/azure-pim-resource-rbac/rbac-overview-top.png)

![Yönetici görünümü panosunun ekran görüntüsü, gösteren veriler listelenir.](media/azure-pim-resource-rbac/role-settings.png)

Kaynak rol etkinleştirmeleri grafik gösterimi son yedi gün kapsar. Bu veriler, seçili kaynak için kapsamlı ve en sık kullanılan rolleri (sahibi, katkıda bulunan, kullanıcı erişimi Yöneticisi) ve birleşik tüm roller için etkinleştirme görüntüler.

Etkinleştirmeleri graf sağında, iki grafik hem kullanıcılar hem de grupları atama türü tarafından rol atamalarını dağılımını görüntüleyin. Değerin bir yüzde değeri (veya tersi), bir dilimi grafiğin seçerek değiştirebilirsiniz.

Grafikleri, son 30 gün ve roller (Azalan) toplam atamalara göre sıralanmış listesini üzerinde kullanıcılar ve gruplar ile yeni rol atamaları sayısı bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynak rolleri için erişim gözden geçirmesi PIM'de Başlat](pim-resource-roles-start-access-review.md) 
