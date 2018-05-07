---
title: 'Genel Bakış: bir erişim gözden geçirme Privileged Identity Management için Azure kaynaklarını gerçekleştirmek | Microsoft Docs'
description: Bu belge, bir erişim gözden geçirme için Azure kaynaklarını PIM içinde gerçekleştirmek açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: mwahl
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/30/2018
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 61be9873cac462c096599680a6e071e104f3a54c
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2018
---
# <a name="use-a-resource-dashboard-to-perform-an-access-review"></a>Bir erişim gözden geçirme gerçekleştirmek için bir kaynak Panosu kullanın

Bir kaynak Pano, bir erişim gözden geçirme için Azure kaynaklarını ayrıcalıklı Kimlik Yönetimi (PIM) gerçekleştirmek için kullanabilirsiniz. Yönetici görünümünü Pano üç birincil bileşenden oluşur:

- Kaynak rol etkinleştirme grafik gösterimi.
- Atamaya göre rol atamalarını dağıtımını görüntülemek iki grafik yazın.
- Yeni rol atamaları için ilgili bir veri alanı.

![Grafikler ve gösteren ekran görüntüsü yönetici görünümünü Panosu](media/azure-pim-resource-rbac/rbac-overview-top.png)

![Veri listeleri gösteren ekran görüntüsü yönetici görünümünü Panosu](media/azure-pim-resource-rbac/role-settings.png)

Kaynak rol etkinleştirme grafik gösterimi son yedi gün kapsar. Bu veriler seçilen kaynağa kapsamlıdır ve etkinleştirmeler en sık kullanılan rolleri (sahibi, katkıda bulunan, kullanıcı erişimi Yöneticisi) ve birleşik tüm rolleri görüntüler.

Etkinleştirmeleri grafik Sağda iki grafikler, kullanıcılar ve gruplar için atama türü tarafından rol atamalarını dağıtımını görüntüler. Grafiğin bir dilim seçerek bir (veya tersi), değeri değiştirebilirsiniz.

Grafikler, son 30 gün ve toplam atamalara (Azalan) göre sıralanmış rollerin listesini üzerinde yeni rol atamaları olan kullanıcılar ve gruplar sayısı bakın.


