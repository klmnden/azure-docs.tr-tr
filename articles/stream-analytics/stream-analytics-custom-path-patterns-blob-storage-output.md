---
title: Azure Stream Analytics için özel bir tarih/saat yol desenleri blob depolama çıkışı (Önizleme)
description: ''
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: da29c6bd8ddc1e2f62a78fb683df5e1784141722
ms.sourcegitcommit: f31bfb398430ed7d66a85c7ca1f1cc9943656678
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/28/2018
ms.locfileid: "47452573"
---
# <a name="custom-datetime-path-patterns-for-azure-stream-analytics-blob-storage-output-preview"></a>Azure Stream Analytics için özel bir tarih/saat yol desenleri blob depolama çıkışı (Önizleme)

Azure Stream Analytics, blob depolama çıkış dosya yolunda özel tarih ve saat biçim tanımlayıcılarının destekler. Özel DateTime yol desenleri, Azure Stream Analytics, Azure HDInsight ve Azure Databricks için aşağı yönde işleme e veri göndermek üzere olanağı sağlayabilir, Hive akışı kuralları ile eşleşen bir çıktı biçimi belirtmenizi sağlar. Özel DateTime yol desenleri, kullanarak kolayca uygulanır `datetime` , biçim belirteci ile birlikte çıkış BLOB yolu ön eki alanındaki anahtar sözcük. Örneğin, `{datetime:yyyy}`.

Bu bağlantı için kullanmak [Azure portalı](https://portal.azure.com/?Microsoft_Azure_StreamAnalytics_bloboutputcustomdatetimeformats=true) özel DateTime yol desenleri için blob depolama Çıktı Önizleme sağlayan bir özellik bayrağını değiştirilecek. Bu özellik, ana Portalı'nda kısa süre içinde etkinleştirilecektir.

## <a name="supported-tokens"></a>Desteklenen belirteçler

Aşağıdaki biçim belirticisi belirteçler, tek başına veya birlikte özel DateTime biçim elde etmek için kullanılabilir:

|Biçim belirleyicisi   |Açıklama   |Sonuçları örnek zaman 2018-01-02T10:06:08|
|----------|-----------|------------|
|{datetime:yyyy}|Dört basamaklı bir sayı olarak yıl|2018|
|{datetime:MM}|01 ile 12 ay|01|
|{datetime:M}|1 ile 12 ay|1|
|{datetime:dd}|01 ile 31 gün|02|
|{datetime:d}|1 ile 12 gün|2|
|{datetime:HH}|Saat 00 ile 23 arasında 24 saatlik biçim kullanılarak|10|
|{datetime:mm}|Dakika 00 ile 24 arasındaki sürümleri|06|
|{datetime:m}|0 dakika ile 24 arasındaki sürümleri|6|
|{datetime:ss}|00 60 saniye|08|

Özel DateTime desenleri kullanmak istemiyorsanız, {date} ekleyin ve/veya {yolu yerleşik DateTime biçimlerinden bir açılan listesi oluşturmak için ön eki için belirteci zaman}.

![Stream Analytics eski tarih/saat biçimleri](./media/stream-analytics-custom-path-patterns-blob-storage-output/stream-analytics-old-date-time-formats.png)

## <a name="extensibility-and-restrictions"></a>Genişletilebilirlik ve kısıtlamaları

Çok belirteçleri kullanabilirsiniz `{datetime:<specifier>}`yolu ön eki karakter sınırı ulaşana kadar yol deseni istediğiniz gibi. Tarih ve saat Açılır kutudan tarafından zaten listelenmiş birleşimleri ötesinde tek bir belirteç içindeki Biçim belirticileri birleştirilemez. 

Bir yol bölümü için `logs/MM/dd`:

|Geçerli deyim   |Geçersiz ifade   |
|----------|-----------|
|`logs/{datetime:MM}/{datetime:dd}`|`logs/{datetime:MM/dd}`|

Yol ön eki aynı biçim tanımlayıcısı birden çok kez kullanabilirsiniz. Belirteci her yinelenmesi gerekir.

## <a name="hive-streaming-conventions"></a>Hive akış kuralları

Blob depolama için özel yol desenleri ile Etiketlenecek klasörleri bekliyor Hive akış kuralı ile kullanılabilir `column=` klasörü adı.

Örneğin, `year={datetime:yyyy}/month={datetime:MM}/day={datetime:dd}/hour={datetime:HH}`.

Özel çıkış tabloları değiştirme ve bölümler Azure Stream Analytics Hive arasında bağlantı noktası verileri el ile ekleyerek ortadan ortadan kaldırır. Bunun yerine, birçok klasörleri kullanılarak otomatik olarak eklenebilir:

```
MSCK REPAIR TABLE while hive.exec.dynamic.partition true
```

### <a name="example"></a>Örnek

Bir depolama hesabı, bir kaynak grubu, bir Stream Analytics işi ve bir giriş kaynağı göre oluşturma [Azure Stream Analytics, Azure portalında](stream-analytics-quick-create-portal.md) Hızlı Başlangıç Kılavuzu. Ayrıca kullanılabilir Hızlı Başlangıç Kılavuzu'nda kullanılan aynı örnek verileri kullanarak [GitHub](https://raw.githubusercontent.com/Azure/azure-stream-analytics/master/Samples/GettingStarted/HelloWorldASA-InputStream.json).

Aşağıdaki yapılandırmaya sahip bir blob çıkış havuzu oluşturun:

![Stream Analytics blob çıkış havuzu oluşturma](./media/stream-analytics-custom-path-patterns-blob-storage-output/stream-analytics-create-output-sink.png)

Tam yol deseni aşağıdaki gibidir:

```
year={datetime:yyyy}/month={datetime:MM}/day={datetime:dd}
```

Yol deseni temel alınarak bir klasör yapısı, işi başlattığınızda, blob kapsayıcısında oluşturulur. Günlük düzeyi kadar detaya gidebilirsiniz.

![Stream Analytics blob çıktı özel yol deseni](./media/stream-analytics-custom-path-patterns-blob-storage-output/stream-analytics-blob-output-folder-structure.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stream Analytics çıkışları anlama](stream-analytics-define-outputs.md)
