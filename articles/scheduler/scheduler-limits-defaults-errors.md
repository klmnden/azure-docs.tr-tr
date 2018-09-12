---
title: Scheduler sınırları ve Varsayılanları
description: Scheduler sınırları ve Varsayılanları
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: ''
ms.assetid: 88f4a3e9-6dbd-4943-8543-f0649d423061
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/18/2016
ms.author: deli
ms.openlocfilehash: 4be0695402b66fdb2a027bfbada0e0b26e02d36f
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2018
ms.locfileid: "44378729"
---
# <a name="scheduler-limits-and-defaults"></a>Scheduler sınırları ve Varsayılanları
## <a name="scheduler-quotas-limits-defaults-and-throttles"></a>Zamanlayıcı kotaları, sınırları, Varsayılanları ve kısıtlamalar
[!INCLUDE [scheduler-limits-table](../../includes/scheduler-limits-table.md)]

## <a name="the-x-ms-request-id-header"></a>X-ms-request-id üst bilgisi
Zamanlayıcı hizmetine karşı yapılan her isteği adlı bir yanıt üstbilgisi döndürür **x-ms-request-id**. Bu üst bilgi isteği benzersiz olarak tanımlayan genel olmayan bir değer içerir.

Bir isteği tutarlı olarak başarısız oluyor ve istek düzgün şeklide doğruladıktan, sorunu Microsoft'a bildirmek için bu değeri kullanabilir. Raporu, x-ms-request-id, isteği gereken yaklaşık süreyi değerini, abonelik, iş koleksiyonu ve/veya iş ve istek Denenen işlem türü tanımlayıcısını içerir.

## <a name="see-also"></a>Ayrıca Bkz.
 [Scheduler nedir?](scheduler-intro.md)

 [Azure Scheduler kavramları, terminolojisi ve varlık hiyerarşisi](scheduler-concepts-terms.md)

 [Azure portalında Scheduler’ı kullanmaya başlama](scheduler-get-started-portal.md)

 [Azure Scheduler’da planlar ve faturalama](scheduler-plans-billing.md)

 [Azure Scheduler REST API başvurusu](https://msdn.microsoft.com/library/mt629143)

 [Azure Scheduler PowerShell cmdlet’leri başvurusu](scheduler-powershell-reference.md)

 [Yüksek Azure Scheduler kullanılabilirliği ve güvenilirliği](scheduler-high-availability-reliability.md)

 [Azure Scheduler giden bağlantı kimlik doğrulaması](scheduler-outbound-authentication.md)

