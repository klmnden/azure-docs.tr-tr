---
title: Azure Stream Analytics tanılama günlüğü veri hataları
description: Bu makalede, farklı giriş ve çıkış veri Azure Stream Analytics kullanırken oluşabilecek bir hataları açıklanır.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.openlocfilehash: ecc7077bf208adf1ac89adcce2f2e480ce34888e
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2019
ms.locfileid: "67329591"
---
# <a name="azure-stream-analytics-data-errors"></a>Azure Stream Analytics veri hataları

Verileri, verilerin işlenmesi sırasında oluşan hataları hatalardır.  Bu hatalar çoğunlukla veri serinin sırasında seri hale getirme, oluşur ve yazma işlemleri.  Veri hatalar oluştuğunda, Stream Analytics için tanılama günlükleri ayrıntılı bilgi ve örnek olayları yazar.  Bazı durumlarda, bu bilgilerin özeti de portal bildirimleri sağlanır.

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
* Etkisi: Geçersiz sıkıştırma türü de dahil olmak üzere herhangi bir seri durumundan çıkarma hata iletileriyle girdiden bırakılır.
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
* Etkisi: Geçersiz üst bilgisi dahil olmak üzere herhangi bir seri durumundan çıkarma hata iletileriyle girdiden bırakılır.
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
* Etkisi: Eksik sütunlar ile olayları girdiden bırakılır.
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
* Etkisi: Tür dönüştürme hatası olaylarla girdiden bırakılır.
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
* Etkisi: Geçersiz veri hatayla karşılaştı sonra iletinin tüm olayları girdiden bırakılır.
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
* Etkisi: Geçersiz giriş zaman damgasına sahip olayları girdiden bırakılır.
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
* Etkisi: Giriş geçersiz zaman damgası anahtarı ile olayları girdiden bırakılır.
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
* Etkisi:  Geç giriş olayları, "göre diğer olayları işleme" işlenir olay iş Yapılandırması bölümünü sıralama ayarlama. Daha fazla bilgi için [zaman işleme ilkeleri](https://docs.microsoft.com/stream-analytics-query/time-skew-policies-azure-stream-analytics).
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
* Etkisi:  Erken giriş olayları, "göre diğer olayları işleme" işlenir olay iş Yapılandırması bölümünü sıralama ayarlama. Daha fazla bilgi için [zaman işleme ilkeleri](https://docs.microsoft.com/stream-analytics-query/time-skew-policies-azure-stream-analytics).
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
* Etkisi:  Olay sıralama iş Yapılandırması bölümünü olayların işlenmesini "göre diğer olayları işleme" sıra dışında ayarlama. Daha fazla bilgi için [zaman işleme ilkeleri](https://docs.microsoft.com/stream-analytics-query/time-skew-policies-azure-stream-analytics).
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
* Etkisi:  Gerekli sütun eksik dahil olmak üzere tüm çıkış veri dönüştürme hataları şunlara göre işleneceğini [çıkış veri İlkesi](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-output-error-policy) ayarı.
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
* Etkisi:  Geçersiz sütun adı dahil olmak üzere tüm çıkış veri dönüştürme hataları şunlara göre işleneceğini [çıkış veri İlkesi](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-output-error-policy) ayarı.
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
* Etkisi:  Tür dönüştürme hatası dahil olmak üzere tüm çıkış veri dönüştürme hataları şunlara göre işleneceğini [çıkış veri İlkesi](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-output-error-policy) ayarı.
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
* Etkisi:  Kayıt aşıldı boyut sınırını da dahil olmak üzere tüm çıkış veri dönüştürme hataları şunlara göre işleneceğini [çıkış veri İlkesi](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-output-error-policy) ayarı.
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
* Etkisi:  Yinelenen anahtar dahil olmak üzere tüm çıkış veri dönüştürme hataları şunlara göre işleneceğini [çıkış veri İlkesi](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-output-error-policy) ayarı.
* Günlük Ayrıntıları
   * Sütunun adı.
   * Kayıt tanımlayıcısı veya kaydının bir parçası.

```json
"BriefMessage": "Column 'devicePartitionKey' is being mapped to multiple columns."
```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stream Analytics, tanılama günlükleri kullanarak sorun giderme](stream-analytics-job-diagnostic-logs.md)

* [Stream Analytics işi izleme ve sorguları izleme anlama](stream-analytics-monitoring.md)
