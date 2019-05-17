---
title: Azure Stream Analytics tanılama günlüğü veri hataları
description: Bu makalede, farklı giriş ve çıkış veri Azure Stream Analytics kullanırken oluşabilecek bir hataları açıklanır.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/09/2019
ms.openlocfilehash: b00eb12092838746f4bfe16f00eac55df9224b09
ms.sourcegitcommit: 6ea7f0a6e9add35547c77eef26f34d2504796565
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65607236"
---
# <a name="azure-stream-analytics-data-errors"></a>Azure Stream Analytics veri hataları

Bir Azure Stream Analytics işi tarafından işlenen verileri bir uyuşmazlık olduğunda Stream Analytics için tanılama günlüklerini veri hata olayı gönderir. Stream Analytics, ayrıntılı bilgi ve örnek olay verileri hatalar oluştuğunda, tanılama günlüklerini yazar. Bu bilgilerin özetini de bazı hatalar için portal bildirimlerini aracılığıyla sağlanır.

Bu makalede, farklı hata türleri, nedenleri ve girdi ve çıktı verilerini hataları Tanılama Günlüğü ayrıntılarını özetlenmektedir.

## <a name="diagnostic-log-schema"></a>Tanılama günlüğü şeması

Bkz: [tanılama günlükleri'ni kullanarak Azure Stream Analytics sorunlarını giderme](stream-analytics-job-diagnostic-logs.md#diagnostics-logs-schema) tanılama günlükleri için bir şema görmek için. Aşağıdaki JSON için bir örnek değer **özellikleri** veri hatası için bir tanılama günlüğü alanı.

```json
{
    "Source": "InputTelemetryData",
    "Type": "DataError",
    "DataErrorType": "InputDeserializerError.InvalidData",
    "BriefMessage": "Json input stream should either be an array of objects or line separated objects. Found token type: Integer",
    "Message": "Input Message Id: https:\\/\\/exampleBlob.blob.core.windows.net\\/inputfolder\\/csv.txt Error: Json input stream should either be an array of objects or line separated objects. Found token type: Integer",
    "ExampleEvents": "[\"1,2\\\\u000d\\\\u000a3,4\\\\u000d\\\\u000a5,6\"]",
    "FromTimestamp": "2019-03-22T22:34:18.5664937Z",
    "ToTimestamp": "2019-03-22T22:34:18.5965248Z",
    "EventCount": 1
}
```

## <a name="input-data-errors"></a>Giriş veri hataları

### <a name="inputdeserializererrorinvalidcompressiontype"></a>InputDeserializerError.InvalidCompressionType

* Neden: Seçili giriş sıkıştırma veri türüyle eşleşmiyor.
* Sağlanan portal bildirimi: Evet
* Tanılama günlüğü düzeyi: Uyarı
* Günlük Ayrıntıları
   * İleti tanımlayıcısı girin. Olay hub'ı PartitionID uzaklığı ve sıra numarası tanımlayıcısıdır.

**hata iletisi**

```json
"BriefMessage": "Unable to decompress events from resource 'https:\\/\\/exampleBlob.blob.core.windows.net\\/inputfolder\\/csv.txt'. Please ensure compression setting fits the data being processed."
```

### <a name="inputdeserializererrorinvalidheader"></a>InputDeserializerError.InvalidHeader

* Neden: Giriş veri üst bilgisi geçersiz. Örneğin, bir CSV yinelenen adları olan sütun içeriyor.
* Sağlanan portal bildirimi: Evet
* Tanılama günlüğü düzeyi: Uyarı
* Günlük Ayrıntıları
   * İleti tanımlayıcısı girin. 
   * Gerçek yükü kaç kilobayt kadar.

**hata iletisi**

```json
"BriefMessage": "Invalid CSV Header for resource 'https:\\/\\/exampleBlob.blob.core.windows.net\\/inputfolder\\/csv.txt'. Please make sure there are no duplicate field names."
```

### <a name="inputdeserializererrormissingcolumns"></a>InputDeserializerError.MissingColumns

* Neden: CREATE TABLE veya TIMESTAMP BY ile tanımlanan giriş sütunları yok.
* Sağlanan portal bildirimi: Evet
* Tanılama günlüğü düzeyi: Uyarı
* Günlük Ayrıntıları
   * İleti tanımlayıcısı girin. 
   * Eksik olan sütunların adları. 
   * Gerçek yükü birkaç kilobayt kadar.

**Hata iletileri**

```json
"BriefMessage": "Could not deserialize the input event(s) from resource 'https:\\/\\/exampleBlob.blob.core.windows.net\\/inputfolder\\/csv.txt' as Csv. Some possible reasons: 1) Malformed events 2) Input source configured with incorrect serialization format" 
```

```json
"Message": "Missing fields specified in query or in create table. Fields expected:ColumnA Fields found:ColumnB"
```

### <a name="inputdeserializererrortypeconversionerror"></a>InputDeserializerError.TypeConversionError

* Neden: Giriş CREATE TABLE deyiminde belirtilen türe dönüştürülemedi.
* Sağlanan portal bildirimi: Evet
* Tanılama günlüğü düzeyi: Uyarı
* Günlük Ayrıntıları
   * İleti tanımlayıcısı girin. 
   * Sütun ve beklenen tür adı.

**Hata iletileri**

```json
"BriefMessage": "Could not deserialize the input event(s) from resource '''https:\\/\\/exampleBlob.blob.core.windows.net\\/inputfolder\\/csv.txt ' as Csv. Some possible reasons: 1) Malformed events 2) Input source configured with incorrect serialization format" 
```

```json
"Message": "Unable to convert column: dateColumn to expected type."
```

### <a name="inputdeserializererrorinvaliddata"></a>InputDeserializerError.InvalidData

* Neden: Giriş verileri doğru biçimde değil. Örneğin, giriş geçerli bir JSON değil.
* Sağlanan portal bildirimi: Evet
* Tanılama günlüğü düzeyi: Uyarı
* Günlük Ayrıntıları
   * İleti tanımlayıcısı girin. 
   * Gerçek yükü kaç kilobayt kadar.

**Hata iletileri**

```json
"BriefMessage": "Json input stream should either be an array of objects or line separated objects. Found token type: String"
```

```json
"Message": "Json input stream should either be an array of objects or line separated objects. Found token type: String"
```

### <a name="invalidinputtimestamp"></a>InvalidInputTimeStamp

* Neden: TIMESTAMP BY ifadesinin değerini datetime türüne dönüştürülemez.
* Sağlanan portal bildirimi: Evet
* Tanılama günlüğü düzeyi: Uyarı
* Günlük Ayrıntıları
   * İleti tanımlayıcısı girin. 
   * Hata iletisi. 
   * Gerçek yükü kaç kilobayt kadar.

**hata iletisi**

```json
"BriefMessage": "Unable to get timestamp for resource 'https:\\/\\/exampleBlob.blob.core.windows.net\\/inputfolder\\/csv.txt ' due to error 'Cannot convert string to datetime'"
```

### <a name="invalidinputtimestampkey"></a>InvalidInputTimeStampKey

* Neden: TIMESTAMP BY OVER timestampColumn değeri null.
* Sağlanan portal bildirimi: Evet
* Tanılama günlüğü düzeyi: Uyarı
* Günlük Ayrıntıları
   * Kaç kilobayt kadar gerçek yükü.

**hata iletisi**

```json
"BriefMessage": "Unable to get value of TIMESTAMP BY OVER COLUMN"
```

### <a name="lateinputevent"></a>LateInputEvent

* Neden: Uygulama zamanı geliş saati arasındaki farkı geç varış toleransı penceresi büyüktür.
* Sağlanan portal bildirimi: Hayır
* Tanılama günlüğü düzeyi: Bilgi
* Günlük Ayrıntıları
   * Uygulama süresi ve geliş saati. 
   * Gerçek yükü kaç kilobayt kadar.

**hata iletisi**

```json
"BriefMessage": "Input event with application timestamp '2019-01-01' and arrival time '2019-01-02' was sent later than configured tolerance."
```

### <a name="earlyinputevent"></a>EarlyInputEvent

* Neden: 5 dakikadan fazla uygulama süresi ve geliş saati arasındaki farktır.
* Sağlanan portal bildirimi: Hayır
* Tanılama günlüğü düzeyi: Bilgi
* Günlük Ayrıntıları
   * Uygulama süresi ve geliş saati. 
   * Gerçek yükü kaç kilobayt kadar.

**hata iletisi**

```json
"BriefMessage": "Input event arrival time '2019-01-01' is earlier than input event application timestamp '2019-01-02' by more than 5 minutes."
```

### <a name="outoforderevent"></a>OutOfOrderEvent

* Neden: Olay sıralama dışı tolerans penceresi göre tanımlanan sıralamaya kabul edilir.
* Sağlanan portal bildirimi: Hayır
* Tanılama günlüğü düzeyi: Bilgi
* Günlük Ayrıntıları
   * Gerçek yükü kaç kilobayt kadar.

**hata iletisi**

```json
"Message": "Out of order event(s) received."
```

## <a name="output-data-errors"></a>Çıkış veri hataları

### <a name="outputdataconversionerrorrequiredcolumnmissing"></a>OutputDataConversionError.RequiredColumnMissing

* Neden: Çıkış için gerekli sütun yok. Örneğin, Azure tablo PartitionKey olan tanımlanmış bir sütunu yok.
* Sağlanan portal bildirimi: Evet
* Tanılama günlüğü düzeyi: Uyarı
* Günlük Ayrıntıları
   * Sütun ve kayıt tanımlayıcısı veya kaydının bir parçası adı.

**hata iletisi**

```json
"Message": "The output record does not contain primary key property: [deviceId] Ensure the query output contains the column [deviceId] with a unique non-empty string less than '255' characters."
```

### <a name="outputdataconversionerrorcolumnnameinvalid"></a>OutputDataConversionError.ColumnNameInvalid

* Neden: Sütun değeri ile çıktı uygun değil. Örneğin, sütun adı, geçerli Azure tablo sütunu değil.
* Sağlanan portal bildirimi: Evet
* Tanılama günlüğü düzeyi: Uyarı
* Günlük Ayrıntıları
   * Sütun ve kayıt tanımlayıcısı veya kaydının bir parçası adı.

**hata iletisi**

```json
"Message": "Invalid property name #deviceIdValue. Please refer MSDN for Azure table property naming convention."
```

### <a name="outputdataconversionerrortypeconversionerror"></a>OutputDataConversionError.TypeConversionError

* Neden: Bir sütun, geçerli bir çıkış türüne dönüştürülemez. Örneğin, sütunun değeri kısıtlamaları veya SQL tablosunda tanımlı türü ile uyumlu değil.
* Sağlanan portal bildirimi: Evet
* Tanılama günlüğü düzeyi: Uyarı
* Günlük Ayrıntıları
   * Sütunun adı.
   * Kayıt tanımlayıcısı veya kaydının bir parçası.

**hata iletisi**

```json
"Message": "The column [id] value null or its type is invalid. Ensure to provide a unique non-empty string less than '255' characters."
```

### <a name="outputdataconversionerrorrecordexceededsizelimit"></a>OutputDataConversionError.RecordExceededSizeLimit

* Neden: İletinin değeri desteklenen çıkış boyutundan büyük. Örneğin, bir kaydı bir olay hub'ı çıkışı için 1 MB'tan büyük.
* Sağlanan portal bildirimi: Evet
* Tanılama günlüğü düzeyi: Uyarı
* Günlük Ayrıntıları
   * Kayıt tanımlayıcısı veya kaydının bir parçası.

**hata iletisi**

```json
"BriefMessage": "Single output event exceeds the maximum message size limit allowed (262144 bytes) by Event Hub."
```

### <a name="outputdataconversionerrorduplicatekey"></a>OutputDataConversionError.DuplicateKey

* Neden: Bir kayıt sistemi sütun aynı ada sahip bir sütun zaten var. Örneğin, farklı bir sütuna kimlik sütunu olduğunda CosmosDB çıkışı sütunu kimliği adı.
* Sağlanan portal bildirimi: Evet
* Tanılama günlüğü düzeyi: Uyarı
* Günlük Ayrıntıları
   * Sütunun adı.
   * Kayıt tanımlayıcısı veya kaydının bir parçası.

```json
"BriefMessage": "Column 'devicePartitionKey' is being mapped to multiple columns."
```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stream Analytics, tanılama günlükleri kullanarak sorun giderme](stream-analytics-job-diagnostic-logs.md)

* [Stream Analytics işi izleme ve sorguları izleme anlama](stream-analytics-monitoring.md)
