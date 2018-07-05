---
title: "Genel Bakış: Privileged Identity Management'ta erişim gözden geçirmesi, Azure kaynakları için gerçekleştirin. | Microsoft Docs"
description: Bu belge, Azure kaynakları için PIM'de erişim gözden geçirmesi gerçekleştirmeyi açıklar.
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
ms.component: protection
ms.date: 03/30/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 9e9053c62f2ead3b6ae7d4ca3c6c38fd1063b8da
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37441521"
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


