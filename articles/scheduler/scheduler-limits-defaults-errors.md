---
title: Limitler, kotalar ve Azure scheduler'da eşikleri
description: Limitler, kotalar, varsayılan değerleri hakkında bilgi edinin ve Azure Zamanlayıcı için eşikler azaltma
services: scheduler
ms.service: scheduler
author: WenJason
ms.author: v-jay
ms.reviewer: klam
ms.assetid: 88f4a3e9-6dbd-4943-8543-f0649d423061
ms.topic: article
origin.date: 08/18/2016
ms.date: 11/12/2018
ms.openlocfilehash: 478afb20f3dabec74d66d00bec325408ce6604fc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60531271"
---
# <a name="limits-quotas-and-throttle-thresholds-in-azure-scheduler"></a>Limitler, kotalar ve azaltma eşikleri Azure scheduler'da

> [!IMPORTANT]
> Kullanımdan kaldırılan Azure Scheduler uygulamasının yerini [Azure Logic Apps](../logic-apps/logic-apps-overview.md) alacaktır. İş zamanlamak için [Azure Logic Apps'ı deneyebilirsiniz](../scheduler/migrate-from-scheduler-to-logic-apps.md). 

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
