---
title: Limitler, kotalar ve Azure scheduler'da eşikleri
description: Limitler, kotalar, varsayılan değerleri hakkında bilgi edinin ve Azure Zamanlayıcı için eşikler azaltma
services: scheduler
ms.service: scheduler
author: derek1ee
ms.author: deli
ms.reviewer: klam
ms.assetid: 88f4a3e9-6dbd-4943-8543-f0649d423061
ms.topic: article
ms.date: 08/18/2016
ms.openlocfilehash: 0c1e704a3bdec239c87d879ae1ef95e6e76d27fc
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46966935"
---
# <a name="limits-quotas-and-throttle-thresholds-in-azure-scheduler"></a>Limitler, kotalar ve azaltma eşikleri Azure scheduler'da

> [!IMPORTANT]
> [Azure Logic Apps](../logic-apps/logic-apps-overview.md) devre dışı bırakılıyor Azure Scheduler değiştiriyor. İşleri zamanlamak için [Azure Logic Apps'i deneyin](../scheduler/migrate-from-scheduler-to-logic-apps.md). 

## <a name="limits-quotas-and-thresholds"></a>Limitler, kotalar ve eşikleri

[!INCLUDE [scheduler-limits-table](../../includes/scheduler-limits-table.md)]

## <a name="x-ms-request-id-header"></a>x-ms-request-id üst bilgisi

Zamanlayıcı hizmetine karşı yapılan her isteği adlı bir yanıt üstbilgisi döndürür **x-ms-request-id**. Bu üst bilgi isteği benzersiz olarak tanımlayan genel olmayan bir değer içerir. Bu nedenle, bir istek tutarlı olarak başarısız olur ve istek düzgün şekilde biçimlendirilmiş Onaylandı, hata Microsoft'a sağlayarak bildirebilir **x-ms-request-id** yanıt üst bilgisi değeri ve bu ayrıntılar dahil olmak üzere: 

* **X-ms-request-id** değeri
* İsteğin ne zaman yapıldığını gereken yaklaşık süreyi 
* Azure aboneliği, iş koleksiyonu ve iş tanımlayıcıları 
* İstek Denenen işlem türü

## <a name="see-also"></a>Ayrıca bkz.

* [Azure Scheduler nedir?](scheduler-intro.md)
* [Azure Scheduler kavramları, terminolojisi ve varlık hiyerarşisi](scheduler-concepts-terms.md)
