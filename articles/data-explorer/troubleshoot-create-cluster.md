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
ms.openlocfilehash: 0edf9ebcde2df7e639666f8fe7472baacdeb8640
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50212191"
---
# <a name="troubleshoot-failure-to-create-a-cluster-in-azure-data-explorer"></a>Sorunlarını giderme: Azure veri Gezgini'nde bir küme oluşturmak için hata

Azure veri Gezgini'nde küme oluşturma başarısız olası durumunda aşağıdaki adımları izleyin.

1. Yeterli izinlere sahip olun. Bir küme oluşturmak için bir üyesi olmanız *katkıda bulunan* veya *sahibi* rolü için Azure aboneliği. Gerekirse, bunlar için uygun rolü ekleyebilmeniz için abonelik yöneticinize iş durumunda.

1. Doğrulama hataları ilgili küme adını, altında girildiğinden emin olun **küme oluştur** Azure portalında.

1. Denetleme [Azure hizmet durumu Panosu](https://azure.microsoft.com/status/>). Azure veri Gezgini'nde kümeyi oluşturmak için burada çalıştığınız bölge durumunu arayın.

    Durum yoksa **iyi** (yeşil onay işareti) durumu iyileştirildikten sonra küme oluşturmayı deneyin.

1. Hala sorununuzun çözümüyle ilgili yardıma ihtiyacınız varsa, Lütfen bir destek isteği açın [Azure portalında](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).