---
title: Azure zaman serisi öngörüleri önizlemesinde bir zaman serisi kimliği seçmek için en iyi yöntemler | Microsoft Docs
description: Azure zaman serisi öngörüleri önizlemesinde bir zaman serisi kimliği seçtiğinizde en iyi yöntemleri anlama.
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 05/08/2019
ms.custom: seodec18
ms.openlocfilehash: 4b2f538831ee9410eaf1a2d272f01fd30a9236e6
ms.sourcegitcommit: 17411cbf03c3fa3602e624e641099196769d718b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2019
ms.locfileid: "65519431"
---
# <a name="best-practices-for-choosing-a-time-series-id"></a>Zaman serisi kimliği seçmeye yönelik en iyi uygulamalar

Bu makale, Azure zaman serisi öngörüleri önizlemesi bölüm anahtarı, zaman serisi kimliği ve bir seçmeye yönelik en iyi uygulamaları kapsar.

## <a name="choose-a-time-series-id"></a>Zaman Serisi Kimliği

Zaman serisi kimliği seçerek bir veritabanı için bir bölüm anahtarı seçme gibi olur. Tasarım zamanında yapılması gereken önemli bir karardır. Farklı bir zaman serisi kimliği kullanmak için bir zaman serisi öngörüleri Önizleme ortamı güncelleştirilemiyor Zaman serisi Kimliğine sahip bir ortam oluşturulduğunda, diğer bir deyişle, ilke değiştirilemeyen bir değişmez özelliğidir.

> [!IMPORTANT]
> Zaman serisi kimliği büyük/küçük harfe ve sabit (ayarlandıktan sonra değiştirilemez).

Uygun zaman serisi kimliği seçerek, aklınızda kritik öneme sahiptir. Zaman serisi kimliği seçtiğinizde, bu en iyi uygulamaları göz önünde bulundurun:

* Çok çeşitli değerleri ve hatta erişim desenleri sahip bir özellik adı seçin. Bir bölüm anahtarı (örneğin, yüzlerce veya binlerce) birçok farklı değerlere sahip en iyi bir uygulamadır. Birçok müşteri için bu sorun, json'da SensorID veya DeviceID gibi olacaktır.
* Zaman serisi kimliği yaprak düğümü düzeyinde benzersiz olmalıdır, [zaman serisi modeli](./time-series-insights-update-tsm.md).
* Zaman serisi kimlik özelliği adı karakter dizesi en fazla 128 karakter olabilir ve zaman serisi kimliği özelliği değerleri en fazla 1024 karakter olabilir.
* Bazı benzersiz zaman serisi kimliği özellik değerlerini eksikse, benzersizlik kısıtlaması katılmak null değerler olarak kabul edilir.

Ayrıca, en fazla seçebileceğiniz *üç* zaman serisi kimliğinizi olarak (3) anahtar özellikleri

  > [!NOTE]
  > *Üç* (3) anahtar özellikleri, dize olması gerekir.

Aşağıdaki senaryolarda, birden fazla anahtar özellik, zaman serisi Kimliğiniz seçerek açıklanmaktadır:  

### <a name="scenario-one"></a>Senaryo bir

* Varlıklar, her bir benzersiz anahtarla eski filolarına var.
* Örneğin, bir fleet özelliği tarafından benzersiz şekilde tanımlanır *DeviceID* ve başka bir benzersiz özelliği olduğu *objectID*. Hiçbiri fleet diğer filonun benzersiz özellik içerir. Bu örnekte, iki anahtar, cihaz kimliği ve objectID, benzersiz anahtarlar olarak seçersiniz.
* Biz null değerleri kabul edin ve olay yükü bir özelliğin bulunması eksikliği sayar olarak bir `null` değeri. Ayrıca her bir olay kaynağı verilerdeki benzersiz bir zaman serisi kimliği sahip olduğu iki farklı olay kaynakları için gönderme verileri işlemek için en uygun yolu budur

### <a name="scenario-two"></a>Senaryo iki

* Birden çok özellik varlıklar aynı filosundan içinde benzersiz olması gerekir. 
* Örneğin, bir akıllı yapı üretici olduğunuz ve her bir odada sensörlerden dağıtma varsayalım. Her bir odada genellikle aynı olduğundan *sensorId*, gibi *sensor1*, *sensor2*, ve *sensor3*.
* Ayrıca, yapı kat ve odası numaraları özelliğinde, siteler arasında çakışan sahip *flrRm*, sahip olduğu değerleri gibi *1a*, *2b*, *3a* ve benzeri.
* Son olarak, bir özelliğe sahip *konumu*, gibi değerler içeren *Redmond*, *Barselona*, ve *Tokyo*. Benzersizlik oluşturmak için şu üç özellik, zaman serisi kimliği anahtarlar olarak belirlemeniz: *sensorId*, *flrRm*, ve *konumu*.

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [veri modelleme](./time-series-insights-update-tsm.md).

* Plan, [Azure Time Series Insights (Önizleme) ortamı](./time-series-insights-update-plan.md).