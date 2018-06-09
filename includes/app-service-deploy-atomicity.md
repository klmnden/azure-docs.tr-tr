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
ms.openlocfilehash: ac2cf4d688b1bdc54ed2d7341f0e195d3b2fe42d
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35236482"
---
## <a name="what-happens-to-my-app-during-deployment"></a>Dağıtım sırasında uygulamama neler?

Tüm resmi olarak desteklenen dağıtım yöntemleri tek şey ortaktır: Bunlar dosyalarında değişiklik `/site/home/wwwroot` klasörü, uygulamanızın. Bunlar, üretim ortamında çalışması aynı dosyalardır. Bu nedenle, dağıtım kilitli dosyalar nedeniyle başarısız olabilir veya tüm dosyaları aynı anda güncelleştirilmediğinden üretim uygulama dağıtımı sırasında beklenmeyen davranışlara olabilir. Bu sorunları önlemek için birkaç farklı yolu vardır:

- Uygulamanızı durdurma veya çevrimdışı modda, uygulamanız için dağıtım sırasında etkinleştirin. Daha fazla bilgi için bkz: [dağıtımı sırasında kilitli dosyalarla ilgili](https://github.com/projectkudu/kudu/wiki/Dealing-with-locked-files-during-deployment).
- Dağıtma bir [hazırlık yuvasındaki](../articles/app-service/web-sites-staged-publishing.md) ile [otomatik takas](../articles/app-service/web-sites-staged-publishing.md#configure-auto-swap) etkin. 
- Kullanım [çalışma alanından Zip](https://github.com/Azure/app-service-announcements/issues/84) yerine.
