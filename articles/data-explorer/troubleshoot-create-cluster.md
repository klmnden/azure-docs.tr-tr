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
ms.openlocfilehash: 9b8bfe2a4b9b7a8432f14fb53b3e7a4cae49a3b4
ms.sourcegitcommit: f331186a967d21c302a128299f60402e89035a8d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58189980"
---
# <a name="troubleshoot-failure-to-create-a-cluster-in-azure-data-explorer"></a>Sorun giderme: Azure veri Gezgini'nde bir küme oluşturma hatası

Azure veri Gezgini'nde küme oluşturma başarısız olası durumunda aşağıdaki adımları izleyin.

1. Yeterli izinlere sahip olun. Bir küme oluşturmak için bir üyesi olmanız *katkıda bulunan* veya *sahibi* rolü için Azure aboneliği. Gerekirse, bunlar için uygun rolü ekleyebilmeniz için abonelik yöneticinize iş durumunda.

1. Doğrulama hataları ilgili küme adını, altında girildiğinden emin olun **küme oluştur** Azure portalında.

1. Denetleme [Azure hizmet durumu Panosu](https://azure.microsoft.com/status/). Azure veri Gezgini'nde kümeyi oluşturmak için burada çalıştığınız bölge durumunu arayın.

    Durum yoksa **iyi** (yeşil onay işareti) durumu iyileştirildikten sonra küme oluşturmayı deneyin.

1. Hala sorununuzun çözümüyle ilgili yardıma ihtiyacınız varsa, Lütfen bir destek isteği açın [Azure portalında](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).