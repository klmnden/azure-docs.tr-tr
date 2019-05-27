---
title: Veri depolama ve Azure zaman serisi öngörüleri önizlemesinde giriş | Microsoft Docs
description: Veri depolama ve Azure zaman serisi öngörüleri önizlemesinde giriş anlama.
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 05/20/2019
ms.custom: seodec18
ms.openlocfilehash: cebe22dddf9ef382c4eceb799e05cbaab30aedaa
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65951096"
---
# <a name="data-storage-and-ingress-in-azure-time-series-insights-preview"></a>Veri depolama ve Azure zaman serisi öngörüleri önizlemesinde giriş

Bu makalede, Azure zaman serisi öngörüleri Önizleme verileri depolama ve giriş değişiklikleri açıklar. Temel alınan depolama yapısı, dosya biçimi ve zaman serisi ID özelliği kapsar. Makalede ayrıca temel alınan giriş işlemi, aktarım hızı ve sınırlamalar açıklanır.

## <a name="data-storage"></a>Veri depolama

Zaman serisi öngörüleri Önizleme Kullandıkça Öde SKU ortam oluşturduğunuzda, iki kaynak oluşturmakta olduğunuz:

* Zaman serisi görüşleri ortamına.
* Verilerin depolanacağı bir Azure depolama genel amaçlı V1 hesabı.

Zaman serisi öngörüleri Önizleme Parquet dosya türü ile Azure Blob Depolama kullanır. Time Series Insights, dizin oluşturma ve Azure depolama hesabındaki verileri bölümleme BLOB'ları oluşturma da dahil olmak üzere tüm veri işlemlerini yönetir. Bu BLOB'ları, bir Azure depolama hesabı kullanarak oluşturun.

Diğer Azure depolama blobları gibi Time Series Insights oluşturulan BLOB'ları, okuma ve yazma için bunları çeşitli tümleştirme senaryolarını desteklemek üzere olanak tanır.

> [!TIP]
> Okuma veya çok sık, bloblar için yazma zaman serisi görüşleri performansını olumsuz yönde etkilenebilir.

Azure Blob depolamaya genel bakış için bkz. [depolama blobları giriş](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction).

Parquet dosya türü hakkında daha fazla bilgi için bkz. [desteklenen dosya türleri Azure Depolama'da](https://docs.microsoft.com/azure/data-factory/supported-file-formats-and-compression-codecs#Parquet-format).

## <a name="parquet-file-format"></a>Parquet dosyası biçimi

Parquet için tasarlanmış bir sütun odaklı veri dosyası biçimi şöyledir:

* Birlikte çalışabilirlik
* Alan verimliliğini
* Sorgu verimliliği artar

Time Series Insights düzenleri, kodlama ile toplu karmaşık veri işleyebilir performans Gelişmiş ve verimli veri sıkıştırma sağladığından Parquet seçtiniz.

Daha iyi Parquet dosya biçimine anlamak için bkz: [Parquet belgeleri](https://parquet.apache.org/documentation/latest/).

### <a name="event-structure-in-parquet"></a>Parquet içindeki olay yapısı

Time Series Insights, oluşturur ve blobları kopyalarını aşağıdaki iki biçimlerde depolar:

1. İlk, ilk kopyalama geliş saati bölümlenmiş:

    * `V=1/PT=Time/Y=<YYYY>/M=<MM>/<YYYYMMDDHHMMSSfff>_<TSI_INTERNAL_SUFFIX>.parquet`
    * Bölümlenmiş geliş saati ile BLOB'ları için BLOB oluşturma zamanı.

1. Zaman serisi kimliği bir dinamik gruplandırma bölümlenmiş ikinci, yeniden bölümlenebildiği Kopyala:

    * `V=1/PT=TsId/Y=<YYYY>/M=<MM>/<YYYYMMDDHHMMSSfff>_<TSI_INTERNAL_SUFFIX>.parquet`
    * Zaman serisi kimliği ile bölümlenmiş BLOB'ları için BLOB en kısa olay zaman damgası

> [!NOTE]
> * `<YYYY>` 4 basamaklı bir yıl gösterimi eşlenir.
> * `<MM>` 2-basamaklı ay gösterimi eşlenir.
> * `<YYYYMMDDHHMMSSfff>` bir zaman damgası biçimi 4 basamaklı bir yıl ile eşlenir (`YYYY`), 2-basamaklı ay (`MM`), 2 basamaklı gün (`DD`), 2 basamaklı saat (`HH`), 2 basamaklı dakika (`MM`), 2-basamaklı saniye (`SS`) ve 3 haneli milisaniye (`fff`).

Zaman serisi görüşleri olay Parquet dosya içerikleri için şu şekilde eşlenir:

* Her olay için tek bir satır eşler.
* Yerleşik **zaman damgası** bir olay zaman damgası içeren sütun. Zaman damgası özelliği, hiçbir zaman null olur. Varsayılan **olay kaynağı sıraya saati** zaman damgası özellik belirtilmezse, olay kaynağı. UTC zaman damgası var. 
* Sütunlarıyla eşlenen diğer tüm özellikler ile bitemez `_string` (dize) `_bool` (Boolean) `_datetime` (TarihSaat), ve `_double` (çift), özellik türüne bağlı olarak.
* Eşleme düzeni olarak dosya biçimi'nün ilk sürümü için olan **V = 1**. Bu özellik geliştikçe adı için artırılır **V = 2**, **V = 3**ve benzeri.

## <a name="partitions"></a>Bölmeler

Her zaman serisi öngörüleri Önizleme ortamı olmalıdır bir **zaman serisi kimliği** özelliği ve **zaman damgası** benzersiz olarak tanımlayan özellik. Zaman serisi Kimliğinizi verileriniz için mantıksal bir bölümü olarak görev yapar ve zaman serisi öngörüleri Önizleme ortamı, verileri fiziksel bölümler arasında dağıtmak için doğal bir sınır sağlar. Fiziksel bölüm yönetimi, bir Azure depolama hesabında zaman serisi öngörüleri Preview tarafından yönetilir.

Time Series Insights, bırakarak ve bölümleri yeniden oluşturarak depolamayı ve sorgu performansını iyileştirmek için dinamik bölümlemeyi kullanır. Zaman serisi öngörüleri dinamik bölümleme algoritması çalıştığında birden çok, verileri tek bir fiziksel bölüm önlemek için Önizleme ayrı, mantıksal bölümler. Diğer bir deyişle, bölümleme algoritması tüm verileri bir tek zaman serisi kimliği Parquet dosyalarını yalnızca mevcut diğer zaman serisi kimliklerle aralıklı olmadan belirli saklar. Dinamik bölümleme algoritma ayrıca tek bir zaman serisi kimliği içindeki olayların özgün sırasını korumaya çalışır

Başlangıçta, böylece bir tek, mantıksal bölüm belirtilen zaman aralığı içinde birden çok fiziksel bölümler arasında yayılabilir Giriş zaman zaman damgası tarafından veri bölümlenen. Tek bir fiziksel bölüm de çoğu veya tüm mantıksal bölümler içeriyor olabilir. En uygun bölümleme, hatta ile blob boyutu sınırlamaları nedeniyle tek bir mantıksal bölüm birden çok fiziksel bölüme kaplayabilir.

> [!NOTE]
> Varsayılan olarak, ileti zaman damgası değerdir *sıraya zaman* yapılandırılan olay kaynağınızdaki.

Geçmiş verileri veya toplu iletileri karşıya yüklemekte, verilerinize uygun zaman damgası için eşleşen bir zaman damgası özelliği ile depolamak istediğiniz değeri atayın. Zaman damgası özelliği, büyük/küçük harf duyarlıdır. Daha fazla bilgi için [zaman serisi modeli](./time-series-insights-update-tsm.md).

### <a name="physical-partitions"></a>Fiziksel bölümler

Bir fiziksel bölüm depolama hesabınızda depolanan bir blok blobudur. Anında iletme oranına boyutuna bağlı olduğundan blobları gerçek boyutu değişebilir. Ancak, BLOB'ları yaklaşık 50 MB 20 MB boyutunda olmasını bekliyoruz. Sorgu performansını iyileştirmek için boyut 20 MB'ı seçin zaman serisi görüşleri takımın bu beklentisi gerektiriyordu. Bu boyut, dosya boyutu ve veri giriş hız bağlı olarak, zaman içinde değişebilir.

> [!NOTE]
> * Bloblar, 20 MB boyutlandırılır.
> * Azure BLOB'ları bazen daha iyi performans için olarak bırakılır ve yeniden oluşturulduğunda yeniden bölümlenebildiği.
> * Ayrıca, aynı zaman serisi öngörüleri verilerini iki veya daha fazla bloblar bulunabilir.

### <a name="logical-partitions"></a>Mantıksal bölümler

Bir mantıksal bölüm, bir tek bölüm anahtarı değeri ile ilişkili tüm verileri depolayan bir fiziksel bölüm içindeki bir bölümdür. Zaman serisi öngörüleri Önizleme iki özelliğe göre her bir blob mantıksal bölümler:

* **Zaman serisi kimliği**: Tüm zaman serisi öngörüleri verilerini olay akışını ve model için bölüm anahtarı.
* **Zaman damgası**: İlk giriş üzerinde bağlı süre.

Bu iki özelliklerine bağlı, yüksek performanslı sorgular zaman serisi öngörüleri Önizleme sağlar. Bu iki özellik, zaman serisi öngörüleri verilerini hızla sunmak için en etkili yöntem de sağlar.

Sabit bir özelliği olduğundan, uygun bir zaman serisi kimliği seçmeniz önemlidir. Daha fazla bilgi için [seçin zaman serisi kimlikleri](./time-series-insights-update-how-to-id.md).

## <a name="azure-storage"></a>Azure Storage

### <a name="your-storage-account"></a>Depolama hesabınız

Time Series Insights Kullandıkça Öde ortam oluşturduğunuzda, iki kaynak oluşturma: zaman serisi görüşleri ortamına ve verilerin depolanacağı bir Azure depolama genel amaçlı V1 hesabı. Birlikte çalışabilirlik, fiyat ve Performans nedeniyle varsayılan kaynak Azure depolama genel amaçlı V1 yapmak seçtik. 

Time Series Insights, Azure depolama hesabınızdaki her bir olay en fazla iki kopyasını yayımlar. Başlangıç kopyası, her zaman hızla diğer hizmetleri kullanarak sorgulayabilmesi korunur. Bu altyapılar temel dosya adı filtreleme desteklediğinden kolayca Spark, Hadoop ve diğer tanıdık araçlar arasında zaman serisi kimlikleri ham Parquet dosyalarını kullanabilirsiniz. Blobları yıl ve aya göre gruplandırma, belirli bir zaman aralığı için bir özel iş içindeki blobları listeleme için kullanışlı bir yoldur. 

Ayrıca, zaman serisi görüşleri, zaman serisi öngörüleri API'leri için en iyi duruma getirme Parquet dosyalarını yeniden bölümlendirir. En son repartitioned dosya da kaydedilir.

Genel Önizleme sırasında veriler Azure depolama hesabınızdaki süresiz olarak depolanır.

### <a name="writing-and-editing-time-series-insights-blobs"></a>Yazma ve Time Series Insights BLOB'ları düzenleme

Sorgu performansı ve veri kullanılabilirliğini sağlamak için yoksa düzenleyin veya zaman serisi görüşleri tarafından oluşturulan tüm blobları silin.

### <a name="accessing-and-exporting-data-from-time-series-insights-preview"></a>Erişim ve zaman serisi öngörüleri Önizlemesi'nden verileri dışarı aktarma

Diğer hizmetler ile birlikte kullanmak için zaman serisi öngörüleri Önizleme Gezgini içinde depolanan verilere erişmek isteyebilirsiniz. Örneğin, Azure Machine Learning Studio'yu kullanarak makine öğrenimi için veya bir not defteri uygulaması Jupyter not defterleri ile kullanmak için rapor verilerinizi Power BI'da kullanmak isteyebilirsiniz.

Verilerinizi üç genel yollarla erişebilirsiniz:

* Zaman serisi öngörüleri Önizleme Gezgini'nden: zaman serisi öngörüleri Önizleme Gezgini'nden bir CSV dosyası olarak verileri dışarı aktarabilirsiniz. Daha fazla bilgi için [zaman serisi öngörüleri Önizleme Gezgini](./time-series-insights-update-explorer.md).
* Time Series Insights Önizleme API'leri öğesinden: API uç noktası adresinden ulaşılabilir `/getRecorded`. Bu API hakkında daha fazla bilgi için bkz: [zaman serisi sorgu](./time-series-insights-update-tsq.md).
* Doğrudan bir Azure depolama hesabından (aşağıda).

#### <a name="from-an-azure-storage-account"></a>Bir Azure depolama hesabından

* Zaman serisi görüşleri verilerinize erişmek için kullandığınız hangi hesabı okuma erişimine ihtiyacı vardır. Daha fazla bilgi için [depolama hesabı kaynaklarına erişimi yönetme](https://docs.microsoft.com/azure/storage/blobs/storage-manage-access-to-resources).
* Azure Blob depolama alanından verileri okumak için doğrudan yöntemleri hakkında daha fazla bilgi için bkz. [veri taşımak ve depolama hesabınızdan](https://docs.microsoft.com/azure/storage/common/storage-moving-data?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).
* Bir Azure depolama hesabındaki verileri dışarı aktarmak için:
    * İlk hesabınızı verileri dışarı aktarma için gereksinimleri karşıladığından emin olun. Daha fazla bilgi için [depolama içeri ve dışarı aktarma gereksinimleri](https://docs.microsoft.com/azure/storage/common/storage-import-export-requirements).
    * Azure depolama hesabınızdan verileri dışarı aktarma için kullanabileceğiniz diğer yöntemler hakkında bilgi edinmek için bkz. [BLOB'ları içeri ve dışarı aktarma verileri](https://docs.microsoft.com/azure/storage/common/storage-import-export-data-from-blobs).

### <a name="data-deletion"></a>Veri silme

Blobları silmeyin. Yalnızca bunlar yararlı denetimi ve bir kayıt verilerinizi korumak için zaman serisi öngörüleri Önizleme her blob içinde blob meta verilerini tutar.

## <a name="time-series-insights-data-ingress"></a>Zaman serisi görüşleri veri girişi

### <a name="ingress-policies"></a>Giriş ilkeleri

Zaman serisi öngörüleri Önizleme Time Series Insights şu anda destekler aynı olay kaynakları ve dosya türlerini destekler.

Desteklenen olay kaynakları şunlardır:

- Azure IoT Hub
- Azure Event Hubs
  
  > [!NOTE]
  > Azure olay hub'ı örnekleri Kafka destekler.

Desteklenen dosya türleri şunlardır:

* JSON: Biz işleyebilir desteklenen JSON şekilleri hakkında daha fazla bilgi için bkz: [şekli JSON nasıl](./time-series-insights-send-events.md#json).

### <a name="data-availability"></a>Veri kullanılabilirliği

Zaman serisi öngörüleri Önizleme, bir blob boyutu iyileştirme stratejisi kullanarak verilerin dizinini oluşturur. Bu, ne kadar veri ve hangi hız uygun yayımlanacak dayalı olarak dizinlendikten sonra sorgulamak veri kullanılabilir.

> [!IMPORTANT]
> * Time Series Insights genel kullanıma (GA) sürümü veri bir olay kaynağı ulaşmaktan sonra 60 saniye içinde kullandıracağınız. 
> * Önizleme sırasında veri kullanıma sunulmadan önce daha uzun bir süre bekler.
> * Önemli bir gecikme karşılaşırsanız bizimle emin olun.

### <a name="scale"></a>Ölçek

Zaman serisi öngörüleri Önizleme, ortam başına en fazla 1 Mega (bayt/sn Mbps) saniyede bir ilk giriş ölçeği destekler. Gelişmiş ölçeklendirme desteği devam ediyor. Bu geliştirmeler yansıtacak şekilde belgelerimize güncelleştirmeyi planlıyoruz.

## <a name="next-steps"></a>Sonraki adımlar

- Okuma [Azure Time Series Insights, depolama ve giriş Önizleme](./time-series-insights-update-storage-ingress.md).

- Yeni hakkında okuyun [veri modelleme](./time-series-insights-update-tsm.md).

<!-- Images -->
[1]: media/v2-update-storage-ingress/storage-architecture.png
[2]: media/v2-update-storage-ingress/parquet-files.png
[3]: media/v2-update-storage-ingress/blob-storage.png
