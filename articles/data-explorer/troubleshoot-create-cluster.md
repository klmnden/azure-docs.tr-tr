---
title: Azure veri Gezgini'nde bir küme oluşturma hatası
description: Bu makalede, Azure veri Gezgini'nde bir küme oluşturmak için sorun giderme adımları açıklanmaktadır.
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 6009953ece78eefd52fca9f12e3db80a6d2cc3eb
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46953217"
---
# <a name="troubleshoot-failure-to-create-a-cluster-in-azure-data-explorer"></a>Sorunlarını giderme: Azure veri Gezgini'nde bir küme oluşturmak için hata

Azure veri Gezgini'nde küme oluşturma başarısız olası durumunda aşağıdaki adımları izleyin.

1. Yeterli izinlere sahip olun. Bir küme oluşturmak için bir üyesi olmanız *katkıda bulunan* veya *sahibi* rolü için Azure aboneliği. Gerekirse, bunlar için uygun rolü ekleyebilmeniz için abonelik yöneticinize iş durumunda.

1. Doğrulama hataları ilgili küme adını, altında girildiğinden emin olun **küme oluştur** Azure portalında.

1. Denetleme [Azure hizmet durumu Panosu](https://azure.microsoft.com/status/>). Azure veri Gezgini'nde kümeyi oluşturmak için burada çalıştığınız bölge durumunu arayın.

    Durum yoksa **iyi** (yeşil onay işareti) durumu iyileştirildikten sonra küme oluşturmayı deneyin.

1. Hala sorununuzun çözümüyle ilgili yardıma ihtiyacınız varsa, Lütfen bir destek isteği açın [Azure portalında](https://portal.azure.com).