---
title: Azure Veri Gezgini küme oluşturulurken bir hata sorunlarını giderme
description: Bu makalede, Azure veri Gezgini'nde bir küme oluşturmak için sorun giderme adımları açıklanmaktadır.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 9e6b3f53f07ac86d6b648a8562be4ef45879c37e
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59044129"
---
# <a name="troubleshoot-failed-cluster-creation-of-azure-data-explorer"></a>Sorun giderme: Azure veri Gezgini'nin başarısız küme oluşturma

Azure veri Gezgini'nde küme oluşturma başarısız olası durumunda aşağıdaki adımları izleyin.

1. Yeterli izinlere sahip olun. Bir küme oluşturmak için bir üyesi olmanız *katkıda bulunan* veya *sahibi* rolü için Azure aboneliği. Gerekirse, bunlar için uygun rolü ekleyebilmeniz için abonelik yöneticinize iş durumunda.

1. Doğrulama hataları ilgili küme adını, altında girildiğinden emin olun **küme oluştur** Azure portalında.

1. Denetleme [Azure hizmet durumu Panosu](https://azure.microsoft.com/status/). Azure veri Gezgini'nde kümeyi oluşturmak için burada çalıştığınız bölge durumunu arayın.

    Durum yoksa **iyi** (yeşil onay işareti) durumu iyileştirildikten sonra küme oluşturmayı deneyin.

1. Hala sorununuzun çözümüyle ilgili yardıma ihtiyacınız varsa, Lütfen bir destek isteği açın [Azure portalında](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).
