---
title: En iyi uygulamalar bir zaman serisi kimliği Azure zaman serisi Öngörülerinde seçerken | Microsoft Docs
description: Zaman serisi kimliği Azure zaman serisi Öngörülerinde seçerken en iyi yöntemleri anlama
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 11/30/2018
ms.openlocfilehash: a7fd1ff4a0fe96724af0c43019afe53666ab81d7
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52856856"
---
# <a name="best-practices-when-choosing-a-time-series-id"></a>Zaman serisi kimliği seçerken en iyi uygulamalar

Bu belgede Azure Time Series Insights (Önizleme) bölüm anahtarı kapsar **zaman serisi kimliği**ve en iyi yöntemleri seçin.

## <a name="choose-a-time-series-id"></a>Zaman serisi kimliği seçin

Seçimi **zaman serisi kimliği** bir veritabanı için bir bölüm anahtarı seçilmesinin gibi. Bu nedenle, tasarım zamanında yapılan önemli bir karardır. Farklı bir kullanmak için mevcut TSI (Önizleme) ortamında güncelleştirilemiyor **zaman serisi kimliği**. Diğer bir deyişle, bir ortam ile oluşturulduktan sonra bir **zaman serisi kimliği**, ilke, sabit bir özellik olduğundan değiştirilemez.

> [!IMPORTANT]
> **Zaman serisi kimliği** büyük/küçük harfe ve sabit (kez ayarlandıktan sonra değiştirilemez).

Aklınızda uygun seçerek **zaman serisi kimliği** kritik öneme sahiptir. Aşağıdaki özellikler seçerken göz önünde bulundurun bir **zaman serisi kimliği**:

> [!div class="checklist"]
> * Çok çeşitli değerleri ve hatta erişim desenleri sahip bir özellik adı seçin. Bir bölüm anahtarı (örneğin, yüzlerce veya binlerce) birçok farklı değerlere sahip en iyi bir uygulamadır. Birçok müşteri için bu gibi bir şey olacaktır **cihaz kimliği**, **algılayıcı kimliği**vb.  
> * Yaprak düğümü düzeyinde benzersiz olmalıdır, [zaman serisi modeli](./time-series-insights-update-tsm.md).
> * A **zaman serisi kimliği** özellik adı karakter dizesi, en çok 128 karakter olabilir ve **zaman serisi kimliği** özellik değerleri, en fazla 1024 karakter olabilir.
> * Bazı benzersiz **zaman serisi kimliği** özellik değerleri eksik, benzersizlik kısıtlaması katılmak null değerler olarak kabul edilir.

Ayrıca, en fazla seçebileceğiniz **üç** (3) anahtar özellik olarak, **zaman serisi kimliği**.

  > [!NOTE]
  > **Üç** (3) anahtar özellikleri, dize olması gerekir.

Birden fazla anahtar özellik olarak seçerek, aşağıdaki senaryoları açıklar, **zaman serisi kimliği**:  

* Senaryo 1:

  * Her benzersiz anahtarlara sahip varlıklar eski filolarına var. 
  * Örneğin, bir fleet özelliği tarafından benzersiz şekilde tanımlanır **DeviceID** ve başka bir benzersiz özelliği olduğu **objectID**.  Her iki filolarına diğer filonun benzersiz özelliği mevcut değil. Bu örnekte, iki anahtar seçersiniz **DeviceID**, ve **objectID** benzersiz anahtarlar olarak. 
  * Biz null değerleri kabul edin ve olay yükü bir özelliğin bulunması eksikliği olarak sayılır bir `null` değeri.  Bu ayrıca veri gönderimi burada her olay kaynağındaki verileriyle farklı bir, iki farklı olay kaynakları için uygun şekilde olacaktır benzersiz **zaman serisi kimliği**.

* Senaryo 2:

  * Benzersizlik varlıklar aynı filosundan göstermek için birden çok özellik gerektirir. Örneğin, bir akıllı yapı üreticisi olan ve her bir odada sensörlerden dağıtma varsayalım. Her bir odada genellikle aynı olduğundan **sensorId**de dahil olmak üzere **sensor1**, **sensor2**, **sensor3**ve benzeri.
  * Ayrıca, katı ve odası numaraları özelliğinde, siteler arasında çakışan sahip **flrRm** gibi değerler içeren `1a`, `2b`, `3a`ve benzeri.
  * Son olarak, bir özelliğe sahip **konumu** gibi değerlere bulundupğu `Redmond`, `Barcelona`, `Tokyo`ve benzeri. Benzersizlik oluşturmak için bu özellikleri olarak üç belirlemeniz, **zaman serisi kimliği** anahtarları – **sensorId**, **flrRm**, ve **konumu**.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [Azure Time Series Insights (Önizleme) zaman serisi modelleri](./time-series-insights-update-tsm.md).

Plan, [Azure Time Series Insights (Önizleme) ortamı](./time-series-insights-update-plan.md).