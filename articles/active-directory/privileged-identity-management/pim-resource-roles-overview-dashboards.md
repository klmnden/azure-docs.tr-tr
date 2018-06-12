---
title: 'Genel Bakış: bir erişim gözden geçirme Privileged Identity Management için Azure kaynaklarını gerçekleştirmek | Microsoft Docs'
description: Bu belge, bir erişim gözden geçirme için Azure kaynaklarını PIM içinde gerçekleştirmek açıklar.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.component: protection
ms.date: 03/30/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 2f96f9c70d6a12aeddd51602bccd7f03f89ec7d1
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35260877"
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


