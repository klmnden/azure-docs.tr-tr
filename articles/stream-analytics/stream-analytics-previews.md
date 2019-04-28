---
title: Azure Stream Analytics Önizleme özellikleri
description: Bu makalede, şu anda Önizleme aşamasında olan Azure Stream Analytics özellikleri listeler.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 02/05/2019
ms.openlocfilehash: 08430f3eee858cdb6c9a7fbdfe11bd4c00ef148d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61485684"
---
# <a name="azure-stream-analytics-preview-features"></a>Azure Stream Analytics Önizleme özellikleri

Bu makalede, Azure Stream Analytics için şu anda önizlemede tüm özellikler özetlenmektedir. Önizleme özellikleri, bir üretim ortamında kullanmadan önerilmez.

## <a name="public-previews"></a>Genel Önizleme

Aşağıdaki özellikler, genel Önizleme aşamasındadır. Bu özelliklerin bugün yararlanabilirsiniz, ancak bunları üretim ortamında kullanmayın.

### <a name="anomaly-detection"></a>Anomali Algılama

Azure Stream Analytics yeni makine öğrenimi modelleri için destek tanıtır *depo* ve *Düşüşler* yönlü, yavaş yavaş olumlu ve olumsuz eğilimleri saptama yanı sıra algılama. Daha fazla bilgi için ziyaret [Azure Stream analytics'te Anomali algılama](stream-analytics-machine-learning-anomaly-detection.md).

### <a name="sql-database-reference-data"></a>SQL veritabanı başvuru verileri

Azure Stream Analytics, Azure SQL veritabanı kaynağı başvuru verileri için giriş olarak destekler. SQL veritabanı, Stream Analytics işiniz için Visual Studio ve Azure portalında Stream Analytics araçları ile başvuru veri kullanabilirsiniz. Daha fazla bilgi için ziyaret [kullanımı başvuru verilerini bir SQL veritabanı için Azure Stream Analytics işi](sql-reference-data.md).

### <a name="integration-with-azure-machine-learning"></a>Azure Machine Learning ile tümleştirme

Machine Learning (ML) işlevleri olan Stream Analytics işlerini ölçeklendirme yapabilir. ML işlevleri, Stream Analytics işinde nasıl kullanabileceğiniz hakkında daha fazla bilgi edinmek için [Stream Analytics işinizi Azure Machine Learning işlevleriyle ölçeklendirme](stream-analytics-scale-with-machine-learning-functions.md). Gerçek dünya senaryosuyla kullanıma [Azure Stream Analytics ve Azure Machine Learning kullanarak yaklaşım analizi gerçekleştirme](stream-analytics-machine-learning-integration-tutorial.md).

### <a name="javascript-user-defined-aggregate"></a>JavaScript kullanıcı tanımlı toplam

Azure Stream Analytics, karmaşık bir durum bilgisi olan iş mantığı uygulamanız sağlayan JavaScript dilinde yazılmış kullanıcı tanımlı toplamlarda (UDA) destekler. Uda'lar gelen kullanmayı öğrenin [Azure Stream Analytics JavaScript kullanıcı tanımlı toplamlarda](stream-analytics-javascript-user-defined-aggregates.md) belgeleri. 

### <a name="live-data-testing-in-visual-studio"></a>Canlı veri Visual Studio'da testi

Azure Stream Analytics için Visual Studio Araçları, olay hub'ı veya IOT hub gibi bulut kaynakları Canlı Etkinlik Akışlarını sorguları test etmenize olanak tanıyan yerel test özelliği geliştirin. Bilgi edinmek için nasıl [Test canlı verileri yerel olarak Visual Studio için Azure Stream Analytics araçları kullanarak](stream-analytics-live-data-local-testing.md).

### <a name="net-user-defined-functions-on-iot-edge"></a>IOT Edge üzerinde .NET kullanıcı tanımlı işlevler

.NET standart kullanıcı tanımlı işlevleri ile akış işlem hattınızın parçası olarak .NET Standard kod çalıştırabilir. Basit C# sınıfları oluşturmak veya tam proje ve kitaplıkları Al. Visual Studio'da tam yazma ve hata ayıklama deneyimini desteklenir. Daha fazla bilgi için ziyaret [.NET Standard geliştirme kullanıcı tanımlı işlevleri için Azure Stream Analytics Edge işleri](stream-analytics-edge-csharp-udf-methods.md).

## <a name="private-previews"></a>Özel önizleme

Aşağıdaki özellikler için özel Önizleme aşamasındadır.

### <a name="c-custom-deserializer-for-azure-stream-analytics-on-iot-edge"></a>C# özel seri durumdan çıkarıcının IOT Edge üzerinde Azure Stream Analytics için

Geliştiriciler, Azure Stream Analytics tarafından alınan olayları seri durumdan çıkarmak için C# dilinde özel deserializers artık uygulayabilirsiniz. Parquet, Protobuf, XML veya herhangi bir ikili biçimi seri durumdan çıkarılabiliyorsa biçimleri örnekleridir.

### <a name="visual-studio-code-for-azure-stream-analytics"></a>Visual Studio Code için Azure Stream Analytics

Azure Stream Analytics işleri, Visual Studio Code'da yazılabilir. İçin özel Önizleme özellikleri araçlarına erişimi başvurun *ASAToolsfeedback\@microsoft.com*.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stream analytics'te sekiz yeni özellikler](https://azure.microsoft.com/blog/eight-new-features-in-azure-stream-analytics/)

* [Dört yeni özellikler Azure Stream Analytics'te kullanıma sunuldu](https://azure.microsoft.com/blog/4-new-features-now-available-in-azure-stream-analytics/)
