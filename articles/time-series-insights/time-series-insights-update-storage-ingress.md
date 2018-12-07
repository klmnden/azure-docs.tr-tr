---
title: Veri depolama ve Azure Time Series Insights (Önizleme) içinde giriş | Microsoft Docs
description: Veri depolama ve Azure Time Series Insights (Önizleme) içinde giriş anlama
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/05/2018
ms.openlocfilehash: e2440f6aa32710730e8b015bef1e7c583f7063e2
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "53001856"
---
# <a name="data-storage-and-ingress-in-the-azure-time-series-insights-preview"></a>Veri depolama ve Azure Time Series Insights (Önizleme) içinde giriş

Bu makalede, Azure zaman serisi öngörüleri (TSI) önizleme verileri depolama ve giriş değişiklikleri açıklar. Dosya biçimi, temel alınan depolama yapısı kapsar ve **zaman serisi kimliği** özelliği. Ayrıca, temel alınan giriş işlemi, aktarım hızı ve sınırlamalar açıklanır.

## <a name="data-storage"></a>Veri depolama

Bir Azure TSI önizlemesi oluşturulurken (**PAYG SKU**) ortamında, iki kaynak oluşturmakta olduğunuz:

* Bir Azure TSI ortamı.
* Verilerin depolanacağı bir Azure depolama genel amaçlı V1 hesabı.

(Önizleme) TSI Parquet dosya türü ile Azure Blob Depolama kullanır. Azure TSI, dizin oluşturma ve Azure depolama hesabındaki verileri bölümleme BLOB'ları oluşturma da dahil olmak üzere tüm veri işlemlerini yönetir. Bu BLOB'ları kullanarak bir Azure depolama hesabı oluşturulur.

Diğer Azure depolama blobu gibi okuma ve yazma farklı tümleştirme senaryolarını desteklemek için Azure TSI oluşturulan BLOB'ları için.

> [!TIP]
> Okuma veya çok sık, bloblar için yazma TSI performans olumsuz etkilenebilir.

Azure Blob Depolama'ya genel bakış için okuma [depolama blobları giriş](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction).

Parquet dosya türü hakkında daha fazla bilgi için bkz. [desteklenen dosya türleri Azure Depolama'da](https://docs.microsoft.com/azure/data-factory/supported-file-formats-and-compression-codecs#Parquet-format).

## <a name="parquet-file-format"></a>Parquet dosyası biçimi

Parquet sütun odaklı veri, dosya biçimi için tasarlanmıştır:

* Birlikte çalışabilirlik
* Alan verimliliğini
* Sorgu verimliliği artar

Azure TSI Parquet verimli veri sıkıştırma sağlar ve karmaşık veri toplu işleme için performans ile kodlama düzenleriyle Gelişmiş seçtiniz.

Parquet dosyası biçimi hakkında daha iyi bir anlayış kazanmak için okuma [resmi Parquet belge](https://parquet.apache.org/documentation/latest/).

## <a name="event-structure-in-parquet"></a>Parquet içindeki olay yapısı

Aşağıdaki biçimlerde Azure TSI tarafından oluşturulan BLOB'ları iki kopya depolanır:

1. İlk olarak, bir ilk kopyalama geliş saati tarafından ayrılmış olmalıdır:

    * `V=1/PT=Time/Y=<YYYY>/M=<MM>/<YYYYMMDDHHMMSSfff>_<TSI_INTERNAL_SUFFIX>.parquet`
    * Bölümlenmiş geliş saati ile BLOB'ları için BLOB oluşturma zamanı.

1. Dinamik gruplandırarak repartitioned kopyalama, ikinci bölümlenmesi **zaman serisi kimliği**:

    * `V=1/PT=TsId/Y=<YYYY>/M=<MM>/<YYYYMMDDHHMMSSfff>_<TSI_INTERNAL_SUFFIX>.parquet`
    * En kısa olay zaman damgası tarafından bölümlenmiş BLOB'ları için BLOB **zaman serisi kimliği**.

> [!NOTE]
> * `<YYYY>` 4 basamaklı bir yıl gösterimi eşlenir.
> * `<MM>` 2-basamaklı ay gösterimi eşlenir.
> * `<YYYYMMDDHHMMSSfff>` bir zaman damgası biçimi 4 basamaklı bir yıl ile eşlenir (`YYYY`), 2-basamaklı ay (`MM`), 2 basamaklı gün (`DD`), 2 basamaklı saat (`HH`), 2 basamaklı dakika (`MM`), 2-basamaklı saniye (`SS`) ve 3 haneli milisaniye (`fff`).

Azure TSI olayları Parquet dosya içerikleri için şu şekilde eşlenir:

* Her olay için tek bir satır eşler.
* Yerleşik **zaman damgası** bir olay zaman damgası içeren sütun. **Zaman damgası** özelliği null hiçbir zaman.  Varsayılan **olay kaynağı sıraya saati** varsa **zaman damgası** özelliği, olay kaynağı belirtilmedi.  **Zaman damgası** UTC biçiminde olan.  
* Sütunlar için diğer tüm özellikler eşlemesi ile sona erecek `_string` (dize) `_bool` (boolean) `_datetime` (TarihSaat), ve `_double` özellik türüne bağlı olarak (çift).
* Dosya biçimi ilk sürümü olan ve olarak diyoruz **V = 1**.  Bu özellik gelişse bile adı buna göre artırılır için **V = 2**, **V = 3**ve benzeri.

## <a name="partitions"></a>Bölümler

Her Azure TSI (Önizleme) ortama sahip olmanız gerekir bir **zaman serisi kimliği** özelliği ve **zaman damgası** benzersiz şekilde tanımlamak özelliği. **Zaman serisi kimliği** verileriniz için mantıksal bir bölümü olarak görev yapar ve veriler fiziksel bölümler arasında dağıtmak için doğal bir sınır ile Azure TSI (Önizleme) ortamı sağlar. Fiziksel bölüm yönetimi, bir Azure depolama hesabında Azure TSI (Önizleme) tarafından yönetilir.

Azure TSI, bırakarak ve bölümleri yeniden depolama kullanımı ve sorgu performansını iyileştirmek için dinamik bölümlemeyi kullanır. Azure için birden çok veri sahip tek bir fiziksel bölüm önlemek için dinamik bölümleme algoritması çalışır TSI (Önizleme) ayrı, mantıksal bölümler. Diğer bir deyişle, bölümleme algoritması tüm verileri tek bir belirli saklar **zaman serisi kimliği** diğer aralıklı olmadan Parquet dosya özel olarak mevcut **zaman serisi kimlikleri**. Olayları tek bir özgün sıralamasını korumak dinamik bölümleme algoritması da çalışır **zaman serisi kimliği**.

Giriş zamanında tarafından veri başlangıçta bölümlenen **zaman damgası** için belirtilen zaman aralığı içinde mantıksal, tek bir bölüm birden çok fiziksel bölümler arasında yayılıyor olabilir. Tek bir fiziksel bölüm çoğu veya tüm mantıksal bölümler de içerebilir.  BLOB boyutu sınırlamaları nedeniyle, bile en uygun bölümleme tek bir mantıksal bölüm birden çok fiziksel bölüme kaplayabilir.

> [!NOTE]
> **Zaman damgası** değerdir ileti **sıraya zaman** varsayılan olarak yapılandırılan olay kaynağınızdaki.  

Geçmiş verileri veya toplu iletileri yüklüyorsanız belirlemeniz gerekir **zaman damgası** uygun eşleşen veri özelliği **zaman damgası** ile verilerinizi depolamak istediğiniz değer.  **Zaman damgası** özelliği duyarlıdır. Daha fazla bilgi için okuma [zaman serisi modeli makale](./time-series-insights-update-tsm.md).

## <a name="physical-partitions"></a>Fiziksel bölümler

Bir fiziksel bölüm, Azure Depolama'da depolanan bir blok blobudur. Anında iletme oranına bağlı olarak BLOB'ları yaklaşık 20-50 MB boyutunda olmasını bekliyoruz ancak blobları gerçek boyutuna göre değişir. Bu beklentiler nedeniyle TSI takım 20 MB'lık boyut sorgu performansını iyileştirmek için seçildi. Bu, dosya boyutu ve hızı veri girişine göre zaman içinde değişebilir.

> [!NOTE]
> * Bloblar, 20 MB boyutlandırılır.
> * Azure BLOB'ları bazen daha iyi performans için olarak bırakılan ve yeniden yeniden bölümlenebildiği.
> * Ayrıca aynı TSI verileri birden çok bloblar bulunabilir unutmayın.

## <a name="logical-partitions"></a>Mantıksal bölümler

Bir mantıksal bölüm, bir tek bölüm anahtarı değeri ile ilişkili tüm verileri depolayan bir fiziksel bölüm içindeki bir bölümdür. TSI (Önizleme), mantıksal olarak iki özelliğe göre her bir blob bölüm:

1. **Zaman serisi kimliği** -olay akışını ve modeli içindeki tüm TSI veriler için bölüm anahtarı.
1. **Zaman damgası** - ilk girişine göre.

Bu iki özelliklerine bağlı, yüksek performanslı sorgular Azure TSI (Önizleme) sağlar. Bu iki özellik de TSI verileri hızlı bir şekilde sunmaya yönelik en etkili yöntem sağlar.

Uygun bir seçmeniz önemlidir **zaman serisi kimliği**, sabit bir özelliği olarak.  Bkz: [seçerek zaman serisi kimlikleri](./time-series-insights-update-how-to-id.md) daha fazla bilgi için.

## <a name="your-azure-storage-account"></a>Azure depolama hesabınız

### <a name="storage"></a>Depolama

Bir TSI oluşturduğunuzda **PAYG** ortamı, iki kaynak – TSI ortamı ve verilerin depolanacağı bir Azure depolama genel amaçlı V1 hesabı oluşturun. Azure depolama genel amaçlı V1, birlikte çalışabilirlik, fiyat ve Performans nedeniyle varsayılan hale getirmek seçtik.  

Azure TSI, Azure depolama hesabınızdaki her bir olay en fazla iki kopyasını yayımlar. İlk kopyayı diğer hizmetleri kullanarak performantly sorgu emin olmak için her zaman korunur. Bu nedenle, Spark, Hadoop ve tanıdık araçlardan kolayca genelinde kullanılabilir **zaman serisi kimlikleri** ham Parquet dosya olduğundan bu altyapılar temel destek dosyaları ada göre filtre uygula. Blobları yıl ve aya göre gruplandırma, özel bir iş için belirli bir zaman aralığı içinde blob listesi toplamak kullanışlıdır.  

Ayrıca, TSI Azure TSI API'leri için en iyi duruma getirme Parquet dosyalarını yeniden bölümlenip ve en son repartitioned dosya da kaydedilir.

Genel Önizleme sırasında veriler Azure depolama hesabınızdaki süresiz olarak depolanır.

### <a name="writing-and-editing-time-series-insights-blobs"></a>Yazma ve Time Series Insights BLOB'ları düzenleme

Sorgu performansı ve veri kullanılabilirliğini sağlamak için düzenleyemeyeceği ya TSI tarafından oluşturulan tüm blobları silin.

### <a name="accessing-and-exporting-data-from-time-series-insights-preview"></a>Erişim ve zaman serisi Öngörülerinden (Önizleme) verileri dışarı aktarma

Diğer hizmetler ile birlikte kullanmak için (Önizleme) Azure TSI Gezgini içinde depolanan verilere isteyebilirsiniz. Örneğin, Azure Machine Learning Studio'yu kullanarak makine öğrenimi için Power BI veya bir not defteri uygulama Jupyter not defterleri, vb. raporlama için verilerinizi kullanmak isteyebilirsiniz.

Verilerinize erişmek için üç genel yol vardır:

* (Önizleme) Azure TSI Gezgini.
* Azure TSI (Önizleme) API'ları.
* Doğrudan bir Azure depolama hesabından.

### <a name="from-the-time-series-insights-preview-explorer"></a>Time Series Insights (Önizleme) Gezgini'nden

Verileri (Önizleme) TSI Gezgininde CSV dosyası olarak dışarı aktarabilirsiniz. Daha fazla bilgi edinin [(Önizleme) TSI Gezgini](./time-series-insights-update-explorer.md).

### <a name="from-the-time-series-insights-preview-apis"></a>Zaman serisi öngörüleri (Önizleme) API'lerinden

API uç noktası adresinden ulaşılabilir `/getRecorded`. Bu API hakkında daha fazla bilgi okuyun [zaman serisi sorgu](./time-series-insights-update-tsq.md).

### <a name="from-an-azure-storage-account"></a>Bir Azure depolama hesabından

1. Ne olursa olsun hesabına TSI verilerinize erişmek için kullanacağınız verilen okuma erişimi olması gerekir. Azure Blob depolamaya okuma erişimi verme hakkında daha fazla bilgi edinmek için [depolama kaynaklara erişimi yönetme](https://docs.microsoft.com/azure/storage/blobs/storage-manage-access-to-resources).

1. Azure Blob depolama alanından verileri okumak için doğrudan yollar hakkında daha fazla bilgi için okuma [için ve Azure Depolama'dan veri taşıma](https://docs.microsoft.com/azure/storage/common/storage-moving-data?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

1. Verileri bir Azure depolama hesabından dışarı aktarma:

    * İlk verileri dışarı aktarmak için karşılanması gereken gereksinimleri, hesabınızın olduğundan emin olun. Okuma [depolama içeri ve dışarı aktarma gereksinimleri](https://docs.microsoft.com/azure/storage/common/storage-import-export-requirements) daha fazla bilgi için.
    * Verileri Azure depolama hesabınızı sık ziyaret ederek dışarı aktarmak için diğer yollar hakkında bilgi edinin [depolama almak ve bloblarından veri dışarı aktarma](https://docs.microsoft.com/azure/storage/common/storage-import-export-data-from-blobs)

### <a name="data-deletion"></a>Veri silme

(Önizleme) Azure TSI TSI güncelleştirme içinde BLOB'ları hakkında meta veriler tutar beri blobları silmeyin.

## <a name="ingress"></a>Giriş

### <a name="azure-time-series-insights-ingress-policies"></a>Azure zaman serisi görüşleri giriş ilkeleri

(Önizleme) Azure TSI, aynı olay kaynakları ve şu anda dosya türlerini destekler.

Desteklenen olay kaynakları şunlardır:

- Azure IoT Hub
- Azure Event Hubs
  
  > [!NOTE]
  > Azure olay hub'ı örnekleri Kafka destekler.

Desteklenen dosya türleri şunlardır:

* JSON: desteklenen JSON şekilleri üzerinde size daha fazla işlemek için bkz: [şekli JSON nasıl](./time-series-insights-send-events.md#json) belgeleri.

### <a name="data-availability"></a>Veri kullanılabilirliği

Genel önizlemede olan Azure TSI (Önizleme), bir blob boyutu iyileştirme strateji kullanarak verilerin dizinini oluşturur. Veriler (hangi veri miktarını ve hangi hız uygun yayımlanacak göre) dizinlendikten sonra sorgu kullanılabilir anlamına gelir.

> [!IMPORTANT]
> * GA TSI veri 60 saniye içinde bunu bir olay kaynağı ulaşmaktan kullandıracağınız.  
> * Önizleme sırasında veri kullanıma sunulmadan önce daha uzun bir süre görmek bekliyoruz.  
>   * Önemli bir gecikme yaşıyorsanız, lütfen bizimle iletişime geçin.

### <a name="scale"></a>Ölçek

Bir ortam başına en fazla 6 MB/sn ilk giriş ölçeğini Azure TSI (Önizleme) destekler. Gelişmiş ölçeklendirme desteği devam ediyor. Belgeler, bu geliştirmeler yansıtacak şekilde güncelleştirilir.

## <a name="next-steps"></a>Sonraki adımlar

Okuma [Azure TSI (Önizleme) depolama ve giriş](./time-series-insights-update-storage-ingress.md).

Yeni hakkında okuyun [veri modelleme](./time-series-insights-update-tsm.md).

<!-- Images -->
[1]: media/v2-update-storage-ingress/storage-architecture.png
[2]: media/v2-update-storage-ingress/parquet-files.png
[3]: media/v2-update-storage-ingress/blob-storage.png
