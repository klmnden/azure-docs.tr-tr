---
title: include dosyası
description: include dosyası
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 06/08/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: b8e3d32f0e230673ecbc043597afe8e7b5f25e06
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2018
ms.locfileid: "52742557"
---
## <a name="what-happens-to-my-app-during-deployment"></a>Dağıtım sırasında uygulamama ne?

Resmi olarak desteklenen dağıtım yöntemleri ortak bir şey vardır: dosyalarda değişiklikleri yapmaları `/home/site/wwwroot` uygulamanızın klasör. Bunlar üretimde çalışan aynı dosyalardır. Bu nedenle, dağıtımı kilitli dosyalar nedeniyle başarısız olabilir veya tüm dosyaları aynı anda güncelleştirildiğinden uygulamayı Üretim dağıtımı sırasında öngörülemeyen davranışlara sahip olabilir. Bu sorunlarla karşılaşmamak için birkaç farklı yolu vardır:

- Uygulamanızı durdurma veya dağıtım sırasında uygulamanız için çevrimdışı modunu etkinleştirin. Daha fazla bilgi için [dağıtım sırasında kilitli dosyalarla ilgili](https://github.com/projectkudu/kudu/wiki/Dealing-with-locked-files-during-deployment).
- Dağıtım bir [hazırlama yuvasındaki](../articles/app-service/web-sites-staged-publishing.md) ile [otomatik takas](../articles/app-service/web-sites-staged-publishing.md#configure-auto-swap) etkin. 
- Kullanım [paketi çalıştırmak](https://github.com/Azure/app-service-announcements/issues/84) yerine.
