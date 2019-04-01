---
title: Azure veri Gezgini'nde bir küme oluşturma hatası
description: Bu makalede, Azure veri Gezgini'nde bir küme oluşturmak için sorun giderme adımları açıklanmaktadır.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: b95dabbdecd98902da3bf8217a14f41019c31e82
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58757686"
---
# <a name="troubleshoot-failure-to-create-a-cluster-in-azure-data-explorer"></a>Sorun giderme: Azure veri Gezgini'nde bir küme oluşturma hatası

Azure veri Gezgini'nde küme oluşturma başarısız olası durumunda aşağıdaki adımları izleyin.

1. Yeterli izinlere sahip olun. Bir küme oluşturmak için bir üyesi olmanız *katkıda bulunan* veya *sahibi* rolü için Azure aboneliği. Gerekirse, bunlar için uygun rolü ekleyebilmeniz için abonelik yöneticinize iş durumunda.

1. Doğrulama hataları ilgili küme adını, altında girildiğinden emin olun **küme oluştur** Azure portalında.

1. Denetleme [Azure hizmet durumu Panosu](https://azure.microsoft.com/status/). Azure veri Gezgini'nde kümeyi oluşturmak için burada çalıştığınız bölge durumunu arayın.

    Durum yoksa **iyi** (yeşil onay işareti) durumu iyileştirildikten sonra küme oluşturmayı deneyin.

1. Hala sorununuzun çözümüyle ilgili yardıma ihtiyacınız varsa, Lütfen bir destek isteği açın [Azure portalında](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).