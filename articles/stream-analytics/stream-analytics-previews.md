---
title: Azure Stream Analytics Önizleme özellikleri
description: Bu makalede, şu anda Önizleme aşamasında olan Azure Stream Analytics özellikleri listeler.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/07/2019
ms.openlocfilehash: f3838bf6b9f7daa24c0cdb5b0c5a08d41d164530
ms.sourcegitcommit: 179918af242d52664d3274370c6fdaec6c783eb6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/13/2019
ms.locfileid: "65561147"
---
# <a name="azure-stream-analytics-preview-features"></a>Azure Stream Analytics Önizleme özellikleri

Bu makalede, Azure Stream Analytics için şu anda önizlemede tüm özellikler özetlenmektedir. Önizleme özellikleri, bir üretim ortamında kullanmadan önerilmez.

## <a name="public-previews"></a>Genel Önizleme

Aşağıdaki özellikler, genel Önizleme aşamasındadır. Bu özelliklerin bugün yararlanabilirsiniz, ancak bunları üretim ortamında kullanmayın.

### <a name="visual-studio-code-for-azure-stream-analytics-released-may-2019"></a>Visual Studio Code için Azure Stream Analytics (Mayıs 2019'da yayımlanan)

Azure Stream Analytics işleri, Visual Studio Code'da yazılabilir. Bkz. bizim [VS Code ile çalışmaya başlama Öğreticisi](https://docs.microsoft.com/azure/stream-analytics/quick-create-vs-code).

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

## <a name="other-previews"></a>Diğer önizlemeleri

Aşağıdaki özellikler de önizleme mevcuttur.

### <a name="c-custom-deserializer-for-azure-stream-analytics-on-iot-edge-and-cloud-announced-may-2019"></a>C#IOT Edge ve (Announced Mayıs 2019) bulut üzerinde Azure Stream Analytics için özel seri durumdan çıkarıcının

Geliştiriciler, özel deserializers uygulayabilirsiniz C# Azure Stream Analytics tarafından alınan olayları seri durumdan çıkarılacak. Parquet, Protobuf, XML veya herhangi bir ikili biçimi seri durumdan çıkarılabiliyorsa biçimleri örnekleridir. Bu önizleme için kaydolun [burada](https://aka.ms/asapreview1).

### <a name="parquet-output-announced-may-2019"></a>Parquet çıkış (Mayıs 2019'da duyurulan)
Parquet verimli büyük veri işleme etkinleştirme sütunlu bir biçimidir. Verileri Parquet biçiminde bir veri gölü'nde çıktısı tarafından power büyük ölçekte ETL akış için Azure Stream Analytics yararlanın ve toplu işlem çalıştırabilir, makine öğrenimi algoritmalarını eğitmek veya geçmiş verileriniz üzerinde etkileşimli sorguları çalıştırma. Bu önizleme için kaydolun [burada](https://aka.ms/asapreview1).

### <a name="one-click-integration-with-event-hubs-announced-may-2019"></a>Event hubs'ı (Announced Mayıs 2019) ile tek tıklamayla tümleştirme 
Bu tümleştirme sayesinde, artık gelen verileri görselleştirmek ve olay hub'ı portaldan tek bir tıklamayla bir Stream Analytics sorgu yazmaya başlayın mümkün olacaktır. Sorgunuzu hazır hale geldikten sonra yalnızca birkaç tıklamayla productize ve gerçek zamanlı öngörüleri almak başlatmak mümkün olacaktır. Bu, zaman ve maliyet gerçek zamanlı analiz çözümleri geliştirmek için önemli ölçüde azaltır. Bu önizleme için kaydolun [burada](https://aka.ms/asapreview1).

### <a name="support-for-azure-stack-announced-may-2019"></a>Azure Stack (Mayıs 2019'da duyurulan) için destek
Azure IOT Edge çalışma zamanı bu özellik, yerel girişleri için yerel destek gibi özel Azure Stack özelliklerini yararlanır ve Azure Stack (örneğin, Event Hubs, IOT Hub, Blob Depolama) üzerinde çalışan çıkartır. Bu yeni tümleştirme, verilerinizin nerede oluşturulduğunu ve gecikme süresini azaltmayı ınsights en üst düzeye yakın çözümleyebilirsiniz karma mimariler oluşturmanızı sağlar.
Bu önizleme için kaydolun [burada](https://aka.ms/asapreview1).

