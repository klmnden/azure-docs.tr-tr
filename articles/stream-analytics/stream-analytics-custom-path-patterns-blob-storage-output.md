---
title: Azure Stream Analytics özel çıkış bölümleme blob
description: Bu makalede, özel bir tarih/saat yol desenleri ve blob depolama çıktısını Azure Stream Analytics işleri için özel alan veya öznitelikleri özellikleri açıklar.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 02/07/2019
ms.custom: seodec18
ms.openlocfilehash: e06313cf83768421bedc6c7baddd30c2ef2e4846
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65789414"
---
# <a name="azure-stream-analytics-custom-blob-output-partitioning"></a>Azure Stream Analytics özel çıkış bölümleme blob

Azure Stream Analytics, özel alanlar veya öznitelikler ve özel bir tarih/saat yol desenleri bölümleme özel blob çıktı destekler. 

## <a name="custom-field-or-attributes"></a>Özel alan veya öznitelikleri

Özel alan veya giriş öznitelikleri aşağı yönde veri işleme ve iş akışları hakkında daha fazla denetime çıkış sağlayarak raporlama geliştirin.

### <a name="partition-key-options"></a>Bölüm anahtarı seçenekleri

Bölüm anahtarı veya giriş verileri bölümlemek için kullanılan sütun adı, alfasayısal karakterler ile tire, alt çizgi ve boşluk içerebilir. Diğer adları ile birlikte kullanılmadığı sürece bir bölüm anahtarı olarak iç içe geçmiş alanları kullanmak mümkün değildir. Bölüm anahtarı NVARCHAR(MAX) olması gerekir.

### <a name="example"></a>Örnek

Bir işin burada alınan verileri bir sütun içeren bir dış video oyun hizmetine bağlı Canlı kullanıcı oturumlarını gelen giriş verilerinin sürüyor varsayalım **client_id** oturumları tanımlamak için. Verileri bölümlemek için **client_id**, bölüm belirteci eklemek için Blob yol deseni alanını ayarlayın **{client_id}** bir proje oluştururken blob çıkış özellikleri. Verileri farklı olarak **client_id** değerlerini flow Stream Analytics işi, çıktı verilerini tek bir ayrı klasörlere kaydedilir **client_id** klasörü başına değer.

![İstemci kimliği ile yol deseni](./media/stream-analytics-custom-path-patterns-blob-storage-output/stream-analytics-path-pattern-client-id.png)

Benzer şekilde, burada her algılayıcı b sensör verilerini'algılayıcıları milyonlarca olan iş girişi varsa bir **sensor_id**, yol deseni olacaktır **{sensor_id}** her sensör verilerini farklı klasörlere bölümlemek için.  


REST API, bir JSON çıkışı bölümünü kullanarak bu istek için kullanılan dosya aşağıdaki gibi görünebilir:  

![REST API çıkış](./media/stream-analytics-custom-path-patterns-blob-storage-output/stream-analytics-rest-output.png)

Çalışan, iş başlatıldıktan sonra *istemcileri* kapsayıcı Ara aşağıdaki gibi:  

![İstemciler kapsayıcı](./media/stream-analytics-custom-path-patterns-blob-storage-output/stream-analytics-clients-container.png)

Her bir klasör, burada her blob bir veya daha fazla kayıt içeren birden çok BLOB içerebilir. Yukarıdaki örnekte, bir klasörde aşağıdaki içeriğe sahip "06000000" etiketli bir blobun vardır:

![BLOB içeriği](./media/stream-analytics-custom-path-patterns-blob-storage-output/stream-analytics-blob-contents.png)

Her kayıtta blob sahip olduğuna dikkat edin. bir **client_id** klasörü eşleşen sütun adı çıkış yolunu çıktısında bölümlemek için kullanılan sütun olduğundan **client_id**.

### <a name="limitations"></a>Sınırlamalar

1. Yol deseni blob çıkış özelliğinde yalnızca bir özel bölüm anahtarı izin verilir. Aşağıdaki yol desenleri tümü geçerlidir:

   * cluster1 / {tarih} / {aFieldInMyData}  
   * cluster1 / {time} / {aFieldInMyData}  
   * cluster1 / {aFieldInMyData}  
   * cluster1 / {tarih} / {time} / {aFieldInMyData} 
   
2. Bölüm anahtarları büyük/küçük harfe duyarlı olduğundan bölüm anahtarları "John" ve "john" gibi eşdeğerdir. Ayrıca, ifadeleri, bölüm anahtarı olarak kullanılamaz. Örneğin, **{columnA + columnB}** çalışmıyor.  

3. Bir giriş akışı, kayıtları bir bölüm anahtarı kardinaliteyle altında 8000 içeriyorsa, kayıtları için mevcut blobları eklenir ve yalnızca gerekli olduğunda, yeni BLOB'lar oluşturun. Kardinalite üzerinden ise BLOB mevcut bir garanti yoktur 8000 yazılacağı ve tercihe bağlı sayıda kayıtlar için aynı bölüm anahtarı ile yeni BLOB'ları oluşturulmaz.

## <a name="custom-datetime-path-patterns"></a>Özel DateTime yol desenleri

Özel DateTime yol desenleri, Azure Stream Analytics, Azure HDInsight ve Azure Databricks için aşağı yönde işleme e veri göndermek üzere olanağı sağlayabilir, Hive akışı kuralları ile eşleşen bir çıktı biçimi belirtmenizi sağlar. Özel DateTime yol desenleri, kullanarak kolayca uygulanır `datetime` , biçim belirteci ile birlikte çıkış BLOB yolu ön eki alanındaki anahtar sözcük. Örneğin, `{datetime:yyyy}`.

### <a name="supported-tokens"></a>Desteklenen belirteçler

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

### <a name="extensibility-and-restrictions"></a>Genişletilebilirlik ve kısıtlamaları

Çok belirteçleri kullanabilirsiniz `{datetime:<specifier>}`yolu ön eki karakter sınırı ulaşana kadar yol deseni istediğiniz gibi. Tarih ve saat Açılır kutudan tarafından zaten listelenmiş birleşimleri ötesinde tek bir belirteç içindeki Biçim belirticileri birleştirilemez. 

Bir yol bölümü için `logs/MM/dd`:

|Geçerli deyim   |Geçersiz ifade   |
|----------|-----------|
|`logs/{datetime:MM}/{datetime:dd}`|`logs/{datetime:MM/dd}`|

Yol ön eki aynı biçim tanımlayıcısı birden çok kez kullanabilirsiniz. Belirteci her yinelenmesi gerekir.

### <a name="hive-streaming-conventions"></a>Hive akış kuralları

Blob depolama için özel yol desenleri ile Etiketlenecek klasörleri bekliyor Hive akış kuralı ile kullanılabilir `column=` klasörü adı.

Örneğin, `year={datetime:yyyy}/month={datetime:MM}/day={datetime:dd}/hour={datetime:HH}`.

Özel çıkış tabloları değiştirme ve bölümler Azure Stream Analytics Hive arasında bağlantı noktası verileri el ile ekleyerek ortadan ortadan kaldırır. Bunun yerine, birçok klasörleri kullanılarak otomatik olarak eklenebilir:

```SQL
MSCK REPAIR TABLE while hive.exec.dynamic.partition true
```

### <a name="example"></a>Örnek

Bir depolama hesabı, bir kaynak grubu, bir Stream Analytics işi ve bir giriş kaynağı göre oluşturma [Azure Stream Analytics, Azure portalında](stream-analytics-quick-create-portal.md) Hızlı Başlangıç Kılavuzu. Ayrıca kullanılabilir Hızlı Başlangıç Kılavuzu'nda kullanılan aynı örnek verileri kullanarak [GitHub](https://raw.githubusercontent.com/Azure/azure-stream-analytics/master/Samples/GettingStarted/HelloWorldASA-InputStream.json).

Aşağıdaki yapılandırmaya sahip bir blob çıkış havuzu oluşturun:

![Stream Analytics blob çıkış havuzu oluşturma](./media/stream-analytics-custom-path-patterns-blob-storage-output/stream-analytics-create-output-sink.png)

Tam yol deseni aşağıdaki gibidir:


`year={datetime:yyyy}/month={datetime:MM}/day={datetime:dd}`


Yol deseni temel alınarak bir klasör yapısı, işi başlattığınızda, blob kapsayıcısında oluşturulur. Günlük düzeyi kadar detaya gidebilirsiniz.

![Stream Analytics blob çıktı özel yol deseni](./media/stream-analytics-custom-path-patterns-blob-storage-output/stream-analytics-blob-output-folder-structure.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stream Analytics çıkışları anlama](stream-analytics-define-outputs.md)
