---
title: Azure Stream Analytics işlerinde hizmet kesintilerini önleme
description: Bu makalede, yükseltme, Stream Analytics işleri dayanıklı hale yönergeler açıklanmaktadır.
services: stream-analytics
author: jseb225
ms.author: jeanb
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/07/2018
ms.custom: seodec18
ms.openlocfilehash: 7375fb2763ad83e049b1ef30a623f164e059a792
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61479465"
---
# <a name="guarantee-stream-analytics-job-reliability-during-service-updates"></a>Stream Analytics işi güvenilirlik garanti sırasında hizmet güncelleştirmeleri

Tam olarak yönetilen bir hizmet olan yeni hizmet işlevlerini ve geliştirmeleri hızlı bir hızda özelliği parçasıdır. Sonuç olarak, Stream Analytics, bir hizmet güncelleştirmesinden tek haftalık (veya daha sık) temelinde dağıtma olabilir. Ne kadarlık testi bitti ne olursa olsun yine de, var olan, çalışan bir iş sunulmasıyla bir hata nedeniyle kesilebilir sokması mümkündür. Kritik akış işleme işlerinizi müşteriler için bu riskleri kaçınılması gerekir. Müşteriler, bu riski azaltmak için kullanabileceğiniz bir Azure'nın mekanizmasıdır **[eşleştirilmiş bölge](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)** modeli. 

## <a name="how-do-azure-paired-regions-address-this-concern"></a>Azure eşleştirilmiş bölgeleri başlıklarla bu nasıl karşılayabileceği?

Stream Analytics, eşleştirilmiş bölgelerin işlerinde ayrı toplu işler üzerinde güncelleştirilir garanti eder. Sonuç olarak olası kesme hataları belirlemek ve düzeltmek için güncelleştirmeleri arasında yeterli bir zaman aralığı yok.

_Orta Hindistan dışında_ (eşleştirilmiş olan bölge, Güney Hindistan, Stream Analytics durum yok), Stream Analytics için bir güncelleştirme dağıtımı, eşleştirilmiş bölgelerin kümesinde aynı anda oluşmaz. Birden çok bölgede dağıtımları **aynı gruptaki** oluşabilir **aynı anda**.

Makale **[kullanılabilirlik ve eşleştirilmiş bölgelerin](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)** bölgeleri halindedir en güncel bilgilere sahip.

Müşteriler, her iki bölge çiftlerine özdeş işleri dağıtmak için önerilir. İzleme özelliklerini iç Stream Analytics yanı sıra müşterilerin de işleri izlemek için önerilir gibi **hem** üretim işleri. Bir kesme kullanılarak bir Stream Analytics Hizmet güncelleştirmesi sonucu belirlenirse, uygun şekilde İlerlet ve herhangi bir aşağı akış tüketiciye sağlıklı iş çıktısı için yük devretme. Yükseltme desteklemek için eşleştirilmiş bölge yeni bir dağıtım tarafından etkilenmesini önlemek ve eşleştirilmiş işleri bütünlüğünü.

## <a name="next-steps"></a>Sonraki adımlar

* [Stream analytics'e giriş](stream-analytics-introduction.md)
* [Stream Analytics ile çalışmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Stream Analytics işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Stream Analytics sorgu dili başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Stream Analytics Yönetimi REST API Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
