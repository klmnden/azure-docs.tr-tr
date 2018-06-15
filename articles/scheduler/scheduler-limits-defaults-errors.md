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
ms.openlocfilehash: db6b1c196cb468f41c7a7ce34758de346b522abb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23867818"
---
# <a name="scheduler-limits-and-defaults"></a>Scheduler sınırları ve Varsayılanları
## <a name="scheduler-quotas-limits-defaults-and-throttles"></a>Zamanlayıcı kotaları, sınırları, Varsayılanları ve kısıtlamaları
[!INCLUDE [scheduler-limits-table](../../includes/scheduler-limits-table.md)]

## <a name="the-x-ms-request-id-header"></a>X-ms-request-id üstbilgisi
Adlı bir yanıt üstbilgisi karşı Zamanlayıcı hizmeti yapılan her isteği döndürür**x-ms-request-id**. Bu başlığı istek benzersiz olarak tanımlayan genel olmayan bir değer içerir.

Bir istek sürekli olarak başarısız oluyor ve istek düzgün şeklide doğruladıktan, Microsoft'a hata raporu için bu değeri kullanabilirsiniz. Raporunuzda, x-ms-request-id, isteği yapıldı yaklaşık zaman değeri, abonelik, iş koleksiyonu ve/veya iş ve istek çalıştı işlemi türünü tanımlayıcısını içerir.

## <a name="see-also"></a>Ayrıca Bkz.
 [Scheduler nedir?](scheduler-intro.md)

 [Azure Scheduler kavramları, terminolojisi ve varlık hiyerarşisi](scheduler-concepts-terms.md)

 [Azure portalında Scheduler’ı kullanmaya başlama](scheduler-get-started-portal.md)

 [Azure Scheduler’da planlar ve faturalama](scheduler-plans-billing.md)

 [Azure Scheduler REST API başvurusu](https://msdn.microsoft.com/library/mt629143)

 [Azure Scheduler PowerShell cmdlet’leri başvurusu](scheduler-powershell-reference.md)

 [Yüksek Azure Scheduler kullanılabilirliği ve güvenilirliği](scheduler-high-availability-reliability.md)

 [Azure Scheduler giden bağlantı kimlik doğrulaması](scheduler-outbound-authentication.md)

