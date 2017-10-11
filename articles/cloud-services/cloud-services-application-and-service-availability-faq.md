---
title: "Uygulama ve hizmet kullanılabilirliği sorunlarını için Microsoft Azure bulut Hizmetleri ile ilgili SSS | Microsoft Docs"
description: "Bu makalede, Microsoft Azure bulut Hizmetleri için uygulama ve hizmet kullanılabilirliği hakkında sık sorulan sorular listelenmektedir."
services: cloud-services
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: top-support-issue
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/10/2017
ms.author: genli
ms.openlocfilehash: a893a6d009dd018ad440964419e0a5a1963084b4
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="application-and-service-availability-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Uygulama ve hizmet kullanılabilirliği sorunlarını Azure Cloud Services: sık sorulan sorular (SSS)

Bu makale, uygulama ve hizmet kullanılabilirliği sorunlarını hakkında sık sorulan sorular içermektedir [Microsoft Azure bulut Hizmetleri](https://azure.microsoft.com/services/cloud-services). Ayrıca başvurabilirsiniz [bulut Hizmetleri VM boyutu sayfa](cloud-services-sizes-specs.md) boyutu bilgi.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="my-role-got-recycled-was-there-any-update-rolled-out-for-my-cloud-service"></a>My rol geri aldı. My bulut hizmeti için toplu herhangi bir güncelleştirme oluştu?
Kabaca ayda bir kez Microsoft yeni bir konuk işletim sistemi sürümü Windows Azure PaaS VM'ler için serbest bırakır. Konuk işletim sistemi yalnızca bir tür ardışık düzeninde güncelleştirmesidir. Bir yayın birçok diğer faktörler tarafından etkilenebilir. Ayrıca, Azure yüz binlerce makineler üzerinde çalışır. Bu nedenle, tam tarihi ve saati rollerinizi ne zaman yeniden tahmin etmek mümkün değildir. Konuk işletim sistemi güncelleştirme RSS akışı sahibiz en son bilgilerle güncelleştiriyoruz ancak yaklaşık bir değer süreyi bildirilen göz önünde bulundurmanız gerekir. Biz bu müşteriler için sorunlu olduğunu farkında olduğundan ve sınırı veya tam olarak zaman yeniden başlatma için bir plan üzerinde çalışma.

Yeni konuk işletim sistemi güncelleştirmeleri hakkında tam Ayrıntılar için bkz: [Azure konuk işletim sistemi sürümleri ve SDK uyumluluk matrisi](cloud-services-guestos-update-matrix.md).

Yeniden başlatılır ve Konuk ve ana bilgisayar işletim sistemi güncelleştirmelerinin teknik ayrıntılar işaretçiler hakkında yararlı bilgi için MSDN blog gönderisine bakın [rol örneği yeniden son işletim sistemi yükseltmeleri için](http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx).

## <a name="why-does-the-first-request-to-my-cloud-service-after-the-service-has-been-idle-for-some-time-take-longer-than-usual"></a>Hizmet için biraz zaman boşta kaldıktan sonra bulut Hizmetim ilk isteği neden normalden daha uzun sürer?
Web sunucusuna yapılan ilk istek aldığında, ilk kod yeniden derler ve isteği işler. İşte bu nedenle ilk istek diğerlerinden daha uzun sürer. Varsayılan olarak, uygulama havuzu kullanıcı etkinlik durumlarda kapatıldı. Uygulama havuzu da varsayılan olarak 1,740 dakikada (29 saat) geri.

Internet Information Services (IIS) uygulama havuzları uygulamaya açabilir kararsız durumları önlemek için düzenli aralıklarla dönüştürülebilir, askıda, kilitlenmesine veya bellek sızıntıları.

Aşağıdaki belgeler anlamanıza ve bu sorunu azaltılmasına yardımcı olur:
* [IIS için yavaş ilk yük çözme](http://stackoverflow.com/questions/13386471/fixing-slow-initial-load-for-iis)
* [Uygulama havuzu geri dönüşümü çok yavaş sonra IIS 7.5 web uygulaması ilk istek](http://stackoverflow.com/questions/13917205/iis-7-5-web-application-first-request-after-app-pool-recycle-very-slow)

IIS varsayılan davranışını değiştirmek istiyorsanız, Web rolü örneklerine el ile değişiklik uygularsanız, değişiklikleri sonunda kaybolacağı için başlangıç görevleri kullanmanız gerekecektir.

Daha fazla bilgi için bkz: [nasıl yapılandırılacağı ve bir bulut hizmeti için başlangıç görevleri çalıştırmak](cloud-services-startup-tasks.md).
