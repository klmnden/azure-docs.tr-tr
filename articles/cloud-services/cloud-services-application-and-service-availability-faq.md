---
title: Uygulama ve hizmet kullanılabilirliği sorunlarını için Microsoft Azure bulut Hizmetleri ile ilgili SSS | Microsoft Docs
description: Bu makalede, Microsoft Azure bulut Hizmetleri için uygulama ve hizmet kullanılabilirliği hakkında sık sorulan soruları listelenmektedir.
services: cloud-services
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
tags: top-support-issue
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: fb4b5dde63d8c7c75419d3202d9848cd6fde8b8a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60337553"
---
# <a name="application-and-service-availability-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Uygulama ve hizmet kullanılabilirliği sorunlarını Azure bulut Hizmetleri için: Sık sorulan sorular (SSS)

Bu makale, uygulama ve hizmet kullanılabilirliği sorunlarını için hakkında sık sorulan sorular içerir [Microsoft Azure bulut Hizmetleri](https://azure.microsoft.com/services/cloud-services). Ayrıca başvurabilirsiniz [bulut Hizmetleri VM boyutu sayfası](cloud-services-sizes-specs.md) boyutu bilgi.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="my-role-got-recycled-was-there-any-update-rolled-out-for-my-cloud-service"></a>Rolüm geri alındı. Bulut hizmetimde piyasaya sunuluyor herhangi bir güncelleştirme vardı?
Kabaca ayda bir kez Microsoft yeni bir konuk işletim sistemi sürümü Windows Azure PaaS Vm'leri için serbest bırakır. Konuk işletim sistemi gibi işlem hattında yalnızca bir güncelleme var. Bir yayın birçok diğer faktörler tarafından etkilenebilir. Ayrıca, Azure, yüz binlerce makineler üzerinde çalışır. Bu nedenle, tam tarih ve saat rollerinizi ne zaman yeniden başlatılır tahmin etmek mümkün değildir. Konuk işletim sistemi güncelleştirme RSS akışına sahip olduğumuz en son bilgilerle güncelleştiriyoruz ancak, bildirilen süresi yaklaşık bir değer olmasını dikkate almanız gerekir. Biz kullanan müşteriler için sorunlu olduğunu ve sınır ya da tam olarak zaman yeniden başlatmalar bir plan üzerinde çalışıyoruz.

En yeni konuk işletim sistemi güncelleştirmeleri hakkında tüm ayrıntılar için bkz. [Azure konuk işletim sistemi sürümleri ve SDK uyumluluk matrisi](cloud-services-guestos-update-matrix.md).

Yeniden başlatma ve teknik ayrıntılarını Konuk ve konak işletim sistemi güncelleştirmeleri işaretçileri yararlı bilgiler için MSDN gönderisine bakın [rol örneği yeniden nedeniyle işletim sistemi yükseltmelerini](https://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx).

## <a name="why-does-the-first-request-to-my-cloud-service-after-the-service-has-been-idle-for-some-time-take-longer-than-usual"></a>Bulut hizmetimde hizmetini bir süredir boşta kaldıktan sonra ilk isteği neden normalden uzun sürüyor?
Web sunucusu, ilk isteği aldığında, önce kod yeniden derler ve ardından isteği işler. İşte bu nedenle ilk istek diğerlerine göre daha uzun sürer. Varsayılan olarak, uygulama havuzu kullanıcı etkinlik durumlarda kapatıldı. Uygulama havuzu da varsayılan olarak 1,740 dakikada (29 saat) geri.

Internet Information Services (IIS) uygulama havuzları uygulamaya yol açabilecek kararsız durumları önlemek için düzenli olarak dönüştürülebilir, askıda, kilitlenme veya bellek sızıntıları.

Aşağıdaki belgeler, anlamak ve bu sorunu gidermek yardımcı olur:
* [Yavaş ilk yük IIS için düzeltme](https://stackoverflow.com/questions/13386471/fixing-slow-initial-load-for-iis)
* [Uygulama havuzu geri dönüşümü çok yavaş sonra IIS 7.5 web uygulaması ilk istek](https://stackoverflow.com/questions/13917205/iis-7-5-web-application-first-request-after-app-pool-recycle-very-slow)

IIS varsayılan davranışını değiştirmek istiyorsanız, Web rolü örnekleri için el ile değişiklikleri uygularsanız, değişiklikleri sonunda kaybolacağı için başlangıç görevleri kullanmanız gerekir.

Daha fazla bilgi için [nasıl yapılandırılacağı ve bir bulut hizmeti için başlangıç görevleri çalıştırma](cloud-services-startup-tasks.md).
