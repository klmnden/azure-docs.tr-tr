---
title: Azure Stream Analytics JavaScript kullanıcı tanımlı işlevler | Microsoft Docs
description: JavaScript kullanıcı tanımlı işlevler ile Gelişmiş sorgu mekanizması gerçekleştirmek
keywords: JavaScript, kullanıcı tanımlı işlevler, udf
services: stream-analytics
author: jseb225
manager: ryanw
ms.assetid: ''
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeanb
ms.openlocfilehash: f2b14029ebea7f9cf1fa74a384ecbb72b08b7ad6
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="azure-stream-analytics-javascript-user-defined-functions"></a>Azure Stream Analytics JavaScript kullanıcı tanımlı işlevler
Azure akış analizi, kullanıcı tanımlı işlevler JavaScript'te yazılmış destekler. İle zengin kümesi **dize**, **RegExp**, **matematik**, **dizi**, ve **tarih** yöntemleri, JavaScript sağlar, karmaşık veri dönüşümleri ile Stream Analytics işleri oluşturmak daha kolay hale gelir.

## <a name="javascript-user-defined-functions"></a>JavaScript kullanıcı tanımlı işlevler
JavaScript kullanıcı tanımlı işlevler harici bağlantı gerektirmeyen durum bilgisiz, yalnızca işlem skaler işlevler destekler. Bir işlevin dönüş değeri yalnızca skaler (tek) bir değer olabilir. Kullanıcı tanımlı bir JavaScript işlevi işe ekledikten sonra işlevi herhangi bir yere sorgusunda yerleşik bir skaler işlev gibi kullanabilirsiniz.

Burada JavaScript kullanıcı tanımlı işlevler kullanışlı bulabileceğiniz bazı senaryolar verilmiştir:
* Ayrıştırma ve normal ifade İşlevler, örneğin, sahip dizeleri düzenleme **Regexp_Replace()** ve **Regexp_Extract()**
* Kod çözme ve kodlama verileri, örneğin, ikili onaltılık dönüştürme
* JavaScript ile mathematic hesaplamalar gerçekleştirmek **matematik** işlevleri
* Sıralama, birleştirme, bulma ve dolgu gibi dizi işlemlerini gerçekleştirme

Stream Analytics içinde JavaScript kullanıcı tanımlı işlev ile yapamayacağınız bazı noktalar şunlardır:
* Duyurmak gerçekleştirme dış REST uç noktalar, örneğin, bir dış kaynaktan IP arama veya başvuru veri çekilmesini ters
* Özel olay biçimi serileştirme gerçekleştirmek veya giriş/çıkış üzerinde seri durumundan çıkarma
* Özel toplamaları oluşturun

Gibi çalışır ancak **Date.GetDate()** veya **Math.random()** engellenmediğinden işlevleri tanımı'nda, bunları yapmaktan kaçınmalısınız. Bu işlevler **sağlamadığı** bunları arayın ve Azure akış analizi hizmetine işlev çağrılarını günlüğün korumaz her zaman aynı sonucu dönün ve sonuç döndürmedi. Bir işlev, farklı sonuç aynı olaylarına döndürürse, sizin tarafınızdan veya akış analizi hizmeti tarafından bir iş yeniden başlatıldığında Yinelenebilirlik garanti edilmez.

## <a name="add-a-javascript-user-defined-function-in-the-azure-portal"></a>Azure portalında kullanıcı tanımlı bir JavaScript işlevi ekleme
Basit JavaScript kullanıcı tanımlı bir işlev altında varolan bir Stream Analytics işi oluşturmak için bu adımları uygulayın:

1.  Azure portalında, Stream Analytics işi bulun.
2.  Altında **iş TOPOLOJİ**, işlevinizi seçin. İşlevler boş bir listesi görüntülenir.
3.  Yeni bir kullanıcı tanımlı işlev oluşturmak için seçin **Ekle**.
4.  Üzerinde **yeni işlev** dikey penceresinde için **işlev türü**seçin **JavaScript**. Varsayılan işlev şablonu Düzenleyicisi'nde görüntülenir.
5.  İçin **UDF diğer**, girin **hex2Int**ve işlev uygulama aşağıdaki gibi değiştirin:

    ```
    // Convert Hex value to integer.
    function main(hexValue) {
        return parseInt(hexValue, 16);
    }
    ```

6.  **Kaydet**’i seçin. İşlevinizi işlevleri listesinde görüntülenir.
7.  Yeni **hex2Int** işlev ve işlev tanımı kontrol edin. Tüm İşlevler sahip bir **UDF** işlevi diğer eklenen önek. Yapmanız *önekini dahil* Stream Analytics sorgunuzda işlevi çağırdığınızda. Bu durumda, çağrı **UDF.hex2Int**.

## <a name="call-a-javascript-user-defined-function-in-a-query"></a>Sorguda kullanıcı tanımlı bir JavaScript işlevi çağırma

1. Sorgu Düzenleyicisi'nde altında **iş TOPOLOJİ**seçin **sorgu**.
2.  Sorgunuzu düzenleme ve bu gibi kullanıcı tanımlı işlev çağrısı:

    ```
    SELECT
        time,
        UDF.hex2Int(offset) AS IntOffset
    INTO
        output
    FROM
        InputStream
    ```

3.  Örnek veri dosyasını karşıya yüklemek için iş girişi sağ tıklatın.
4.  Sorgunuz test etme seçeneğini belirleyin **Test**.


## <a name="supported-javascript-objects"></a>Desteklenen JavaScript nesneleri
Azure Stream Analytics JavaScript kullanıcı tanımlı işlevler, standart, yerleşik JavaScript nesneleri destekler. Bu nesnelerin bir listesi için bkz: [genel nesneler](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects).

### <a name="stream-analytics-and-javascript-type-conversion"></a>Akış analizi ve JavaScript tür dönüştürmeleri

Stream Analytics dil ve JavaScript desteği sorgu türleri farklılıklar vardır. Bu tabloda ikisi arasında dönüştürme eşlemeleri listelenmektedir:

Akış Analizi | JavaScript
--- | ---
bigint | Sayı (JavaScript yalnızca tam olarak 2 kadar tamsayılar temsil eden ^ 53)
DateTime | Tarih (JavaScript yalnızca destekler milisaniye)
Çift | Sayı
nvarchar(max) | Dize
Kaydet | Nesne
Dizi | Dizi
NULL | Null


JavaScript Stream Analytics dönüşümleri şunlardır:


JavaScript | Akış Analizi
--- | ---
Sayı | Bigint (sayı yuvarlak ve uzun arasında ise. MinValue ve uzun süre. MaxValue; Aksi takdirde, çift)
Tarih | DateTime
Dize | nvarchar(max)
Nesne | Kaydet
Dizi | Dizi
Null, tanımlanmamış | NULL
Herhangi bir türü (örneğin, bir işlev veya hata) | (Çalışma zamanı hatası sonuçlarında) desteklenmiyor

## <a name="troubleshooting"></a>Sorun giderme
JavaScript çalışma zamanı hataları önemli kabul edilir ve etkinlik günlüğü ortaya çıkmış. Günlük almak için Azure portalında, iş'e gidin ve seçin **etkinlik günlüğü**.


## <a name="other-javascript-user-defined-function-patterns"></a>Diğer JavaScript kullanıcı tanımlı işlev desenleri

### <a name="write-nested-json-to-output"></a>İç içe geçmiş JSON çıktısını almak için yazma
Çıkış akış analizi işi giriş olarak kullanan bir izleme işleme adımı vardır ve bir JSON biçimi gerektiriyorsa, çıkış için bir JSON dizesi yazabilirsiniz. Sonraki örnekte çağrıları **JSON.stringify()** tüm ad/değer çiftleri giriş paketi için işlev ve bunları tek bir dize değeri çıktı olarak yazar.

**JavaScript kullanıcı tanımlı işlev tanımı:**

```
function main(x) {
return JSON.stringify(x);
}
```

**Örnek Sorgu:**
```
SELECT
    DataString,
    DataValue,
    HexValue,
    UDF.json_stringify(input) As InputEvent
INTO
    output
FROM
    input PARTITION BY PARTITIONID
```

## <a name="get-help"></a>Yardım alın
Ek Yardım için deneyin bizim [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream Analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Stream Analytics sorgu dili başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Stream Analytics Yönetimi REST API Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
