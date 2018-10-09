---
title: Azure Stream Analytics Önizleme özellikleri
description: Bu makalede, şu anda Önizleme aşamasında olan Azure Stream Analytics özellikleri listeler.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 10/05/2018
ms.openlocfilehash: 124e936b619e3078c71094156bf91a437a28492b
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2018
ms.locfileid: "48862791"
---
# <a name="azure-stream-analytics-preview-features"></a>Azure Stream Analytics Önizleme özellikleri

Bu makalede, Azure Stream Analytics için şu anda önizlemede tüm özellikler özetlenmektedir. Önizleme özellikleri, bir üretim ortamında kullanmadan önerilmez.

## <a name="public-previews"></a>Genel Önizleme

Aşağıdaki özellikler, genel Önizleme aşamasındadır. Bu özelliklerin bugün yararlanabilirsiniz, ancak bunları üretim ortamında kullanmayın.

### <a name="azure-stream-analytics-on-iot-edge"></a>IoT Edge üzerinde Azure Stream Analytics

IOT Edge üzerinde Azure Stream Analytics, geliştiricilerin neredeyse gerçek zamanlı analizler IOT Edge cihazlarında dağıtma olanak tanır. Daha fazla bilgi için [IOT Edge üzerinde Azure Stream Analytics](stream-analytics-edge.md) belgeleri.

### <a name="integration-with-azure-machine-learning"></a>Azure Machine Learning ile tümleştirme

Machine Learning (ML) işlevleri olan Stream Analytics işlerini ölçeklendirme yapabilir. ML işlevleri, Stream Analytics işinde nasıl kullanabileceğiniz hakkında daha fazla bilgi edinmek için [Stream Analytics işinizi Azure Machine Learning işlevleriyle ölçeklendirme](stream-analytics-scale-with-machine-learning-functions.md). Gerçek dünya senaryosuyla kullanıma [Azure Stream Analytics ve Azure Machine Learning kullanarak yaklaşım analizi gerçekleştirme](stream-analytics-machine-learning-integration-tutorial.md).

### <a name="session-windows"></a>Windows oturumu

Stream Analytics, geliştiricilerin çok az bir çabayla Yazar karmaşık akış işleme işlerini Pencereleme işlevleri için yerel desteğe sahiptir. [Oturum windows](https://msdn.microsoft.com/azure/stream-analytics/reference/session-window-azure-stream-analytics) Grup benzer zamanlarda gelen olayları süreler filtreleme bulunduğu veri yok. Pencereleme işlevleri hakkında daha fazla bilgi edinmek için [Pencereleme işlevleri Stream Analytics'e giriş](stream-analytics-window-functions.md).

### <a name="blob-output-partitioning-by-custom-time"></a>Blob çıktı özel zamanına göre bölümleme

Azure Stream Analytics, Blob depolama alanına özel zaman özniteliklerine dayalı çıkış sağlayabilir. Daha fazla bilgi için ziyaret [özel DateTime yol desenleri Azure Stream Analytics için blob depolama çıkışı](stream-analytics-custom-path-patterns-blob-storage-output.md).

### <a name="javascript-user-defined-aggregate"></a>JavaScript kullanıcı tanımlı toplam

Azure Stream Analytics, karmaşık bir durum bilgisi olan iş mantığı uygulamanız sağlayan JavaScript dilinde yazılmış kullanıcı tanımlı toplamlarda (UDA) destekler. Uda'lar gelen kullanmayı öğrenin [Azure Stream Analytics JavaScript kullanıcı tanımlı toplamlarda](stream-analytics-javascript-user-defined-aggregates.md) belgeleri. 

### <a name="live-data-testing-in-visual-studio"></a>Canlı veri Visual Studio'da testi

Azure Stream Analytics için Visual Studio Araçları, olay hub'ı veya IOT hub gibi bulut kaynakları Canlı Etkinlik Akışlarını sorguları test etmenize olanak tanıyan yerel test özelliği geliştirin. Bilgi edinmek için nasıl [Test canlı verileri yerel olarak Visual Studio için Azure Stream Analytics araçları kullanarak](stream-analytics-live-data-local-testing.md).

### <a name="net-user-defined-functions-on-iot-edge"></a>IOT Edge üzerinde .NET kullanıcı tanımlı işlevler

.NET standart kullanıcı tanımlı işlevleri ile akış işlem hattınızın parçası olarak .NET Standard kod çalıştırabilir. Basit C# sınıfları oluşturmak veya tam proje ve kitaplıkları Al. Visual Studio'da tam yazma ve hata ayıklama deneyimini desteklenir. Daha fazla bilgi için ziyaret [.NET Standard geliştirme kullanıcı tanımlı işlevleri için Azure Stream Analytics Edge işleri](stream-analytics-edge-csharp-udf-methods.md).

## <a name="private-previews"></a>Özel önizleme

Aşağıdaki özellikler için özel Önizleme aşamasındadır. Bu önizlemeler erişmek için Azure Stream Analytics özel Önizleme ziyaret [kaydolun](https://aka.ms/ASApreview1) sayfası.

### <a name="anomaly-detection"></a>Anomali Algılama

Azure Stream Analytics yeni makine öğrenimi modelleri için destek tanıtır *depo* ve *Düşüşler* yönlü, yavaş yavaş olumlu ve olumsuz eğilimleri saptama yanı sıra algılama.

### <a name="c-custom-deserializer-for-azure-stream-analytics-on-iot-edge"></a>C# özel seri durumdan çıkarıcının IOT Edge üzerinde Azure Stream Analytics için

Geliştiriciler, Azure Stream Analytics tarafından alınan olayları seri durumdan çıkarmak için C# dilinde özel deserializers artık uygulayabilirsiniz. Parquet, Protobuf, XML veya herhangi bir ikili biçimi seri durumdan çıkarılabiliyorsa biçimleri örnekleridir.

### <a name="blob-output-partitioning-by-custom-attribute"></a>Blob çıktı özel özniteliği tarafından bölümleme

Artık, sorgunuzun herhangi bir sütuna göre Blob Depolama için Azure Stream Analytics çıkış bölümlemek mümkündür.

### <a name="managed-service-identity-msi-authentication-to-azure-data-lake-storage"></a>Azure Data Lake Storage için Yönetilen hizmet kimliği (MSI) kimlik doğrulaması

Şimdi, işleri programlı olarak oluşturmanıza olanak tanıyan gerçek zamanlı işlem hatlarınızı MSI tabanlı kimlik doğrulaması için Azure Data Lake depolama Gen1 yazarken ile çalışır hale getirebilirsiniz. Daha fazla bilgi için ziyaret [kullanım yönetilen kimlikleri için Azure Stream Analytics işleri kimlik doğrulaması için Azure Data Lake depolama Gen1 çıkış](stream-analytics-managed-identities-adls.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stream analytics'te sekiz yeni özellikler](https://azure.microsoft.com/blog/eight-new-features-in-azure-stream-analytics/)

* [Azure Stream Analytics'te sunuldu 4 yeni özellikler](https://azure.microsoft.com/blog/4-new-features-now-available-in-azure-stream-analytics/)
