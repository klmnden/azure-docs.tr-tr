---
title: Azure Stream Analytics’te JavaScript kullanıcı tanımlı toplamları
description: Bu makalede, Azure Stream Analytics'te JavaScript kullanıcı tanımlı toplamlarda ile Gelişmiş sorgu mekanizmalarını gerçekleştirmeyi açıklar.
services: stream-analytics
author: rodrigoamicrosoft
ms.author: rodrigoa
manager: kfile
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 10/28/2017
ms.openlocfilehash: 6663e3fc48408de83e92f39e8c8070005818852d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61479567"
---
# <a name="azure-stream-analytics-javascript-user-defined-aggregates-preview"></a>Azure Stream Analytics JavaScript kullanıcı tanımlı toplamlarda (Önizleme)
 
Azure Stream Analytics JavaScript dilinde yazılmış kullanıcı tanımlı toplamlarda (UDA) destekler, karmaşık bir durum bilgisi olan iş mantığı uygulamasına olanak tanır. İçinde UDA durum veri yapısı, durumu birikmesi, durumu decumulation ve toplam sonuç hesaplama üzerinde tam denetime sahip. Makalede JavaScript UDA arabirimleri, iki farklı bir UDA ve Stream Analytics sorgu pencere tabanlı işlem ile UDA kullanma oluşturma adımları tanıtılmaktadır.

## <a name="javascript-user-defined-aggregates"></a>JavaScript kullanıcı tanımlı toplamları

Bir kullanıcı tanımlı toplam en üstünde bir zaman penceresi belirtimi bu pencerede olaylar üzerinde toplama ve tek bir sonuç değeri üretmek için kullanılır. Stream Analytics AccumulateOnly ve AccumulateDeaccumulate bugün desteklemektedir UDA arabirimlerin iki türü vardır. Atlayan pencere, atlamalı pencere ve kayan pencere ile UDA her iki türdeki kullanılabilir. AccumulateDeaccumulate UDA AccumulateOnly atlamalı pencere ve kayan pencere ile birlikte kullanıldığında UDA daha iyi gerçekleştirir. Kullandığınız algoritmadan yola çıkılarak iki türlerinden birini seçin.

### <a name="accumulateonly-aggregates"></a>AccumulateOnly toplamaları

AccumulateOnly Toplamlar yalnızca yeni olayları durumuna birikmesini sağlayabilir, algoritma deaccumulation değerlerinin izin vermiyor. Bu toplama türünü seçin, bir olay deaccumulate uygulamak durum değeri bilgilerinden imkansızdır. AccumulatOnly toplamlar için JavaScript şablonu aşağıda verilmiştir:

```JavaScript
// Sample UDA which state can only be accumulated.
function main() {
    this.init = function () {
        this.state = 0;
    }

    this.accumulate = function (value, timestamp) {
        this.state += value;
    }

    this.computeResult = function () {
        return this.state;
    }
}
```

### <a name="accumulatedeaccumulate-aggregates"></a>AccumulateDeaccumulate toplamaları

AccumulateDeaccumulate toplamalar deaccumulation durumu, bir önceki birikmiş değer örneğin izin, bir olay değerler listesinden bir anahtar-değer çifti kaldırın veya sum toplama durumunun bir değerden çıkarmak. AccumulateDeaccumulate toplamlar için JavaScript şablonu aşağıda verilmiştir:

```JavaScript
// Sample UDA which state can be accumulated and deaccumulated.
function main() {
    this.init = function () {
        this.state = 0;
    }

    this.accumulate = function (value, timestamp) {
        this.state += value;
    }

    this.deaccumulate = function (value, timestamp) {
        this.state -= value;
    }

    this.deaccumulateState = function (otherState){
        this.state -= otherState.state;
    }

    this.computeResult = function () {
        return this.state;
    }
}
```

## <a name="uda---javascript-function-declaration"></a>UDA - JavaScript işlev bildirimi

JavaScript UDA her bir işlev nesnesi bildirimi tarafından tanımlanır. İlgili ana öğelerin bir UDA tanımı aşağıda verilmiştir.

### <a name="function-alias"></a>İşlev diğer adı

İşlev diğer adı UDA tanımlayıcısıdır. Stream Analytics sorgunuzda çağrıldığında, her zaman UDA diğer "uda." ile birlikte kullanın önek.

### <a name="function-type"></a>İşlev türü

UDA'için işlev türü olmalıdır **Javascript UDA**.

### <a name="output-type"></a>Çıkış türü

Sorgunuzda türünü işlemek istediğiniz "Bir" if veya belirli bir desteklenen, Stream Analytics işi yazın.

### <a name="function-name"></a>İşlev adı

Bu işlev nesnesinin adı. İşlev adı, tam anlamıyla UDA diğer adı eşleşmelidir (Önizleme davranışı, biz destek anonim işlev düşünüyorsanız, GA).

### <a name="method---init"></a>Yöntem - init()

İnit() yöntemi toplama durumunu başlatır. Penceresi başladığında, bu yöntem çağrılır.

### <a name="method--accumulate"></a>Yöntemi – accumulate()

Önceki bir duruma ve geçerli olay değerlerine dayalı UDA durumu accumulate() yöntemi hesaplar. Bu yöntem, olaya bir zaman penceresi (TUMBLINGWINDOW, HOPPINGWINDOW veya SLIDINGWINDOW) girdiğinde çağrılır.

### <a name="method--deaccumulate"></a>Yöntemi – deaccumulate()

Deaccumulate() yöntemi önceki durumu ve geçerli olay değerleri temel alan durumu yeniden hesaplar. Bu yöntem, bir olay bir SLIDINGWINDOW ayrıldığında çağrılır.

### <a name="method--deaccumulatestate"></a>Yöntemi – deaccumulateState()

DeaccumulateState() yöntemi önceki bir duruma ve bir atlama durumunu temel alan durumu yeniden hesaplar. Bu yöntem, olayları bırakın HOPPINGWINDOW bir dizi olduğunda çağrılır.

### <a name="method--computeresult"></a>Yöntemi – computeResult()

ComputeResult() yöntemi toplam sonuç geçerli durumuna göre döndürür. Bu yöntem, bir zaman penceresi (TUMBLINGWINDOW HOPPINGWINDOW ve SLIDINGWINDOW) sonunda çağrılır.

## <a name="javascript-uda-supported-input-and-output-data-types"></a>JavaScript UDA'ın desteklenen giriş ve çıkış veri türleri
JavaScript UDA veri türleri için bölümüne başvurun **tür dönüştürmesi Stream Analytics ve JavaScript** , [tümleştirme JavaScript UDF'leri](stream-analytics-javascript-user-defined-functions.md).

## <a name="adding-a-javascript-uda-from-the-azure-portal"></a>JavaScript UDA'ı Azure portalından ekleme

Aşağıda UDA portalından oluşturma sürecinde inceleyeceğiz. Burada kullandığımız örnek zaman ağırlıklı ortalamalar bilgi işlem nedir.

Artık bir JavaScript UDA'ın altında var olan bir ASA işi aşağıdaki adımlarla oluşturalım.

1. Azure portalında oturum açın ve var olan Stream Analytics işinizi bulun.
1. İşlevler'in altında ardından **iş TOPOLOJİSİ**.
1. Tıklayarak **Ekle** yeni bir işlev eklemek için simge.
1. Yeni işlev görünümde seçin **JavaScript UDA** işlev türü ardından düzenleyicide görünmesini varsayılan UDA şablonu görürsünüz.
1. UDA diğer ad olarak "TWA" doldurun ve işlev uygulamasını aşağıdaki değiştirin:

    ```JavaScript
    // Sample UDA which calculate Time-Weighted Average of incoming values.
    function main() {
        this.init = function () {
            this.totalValue = 0.0;
            this.totalWeight = 0.0;
        }

        this.accumulate = function (value, timestamp) {
            this.totalValue += value.level * value.weight;
            this.totalWeight += value.weight;

        }

        // Uncomment below for AccumulateDeaccumulate implementation
        /*
        this.deaccumulate = function (value, timestamp) {
            this.totalValue -= value.level * value.weight;
            this.totalWeight -= value.weight;
        }

        this.deaccumulateState = function (otherState){
            this.state -= otherState.state;
            this.totalValue -= otherState.totalValue;
            this.totalWeight -= otherState.totalWeight;
        }
        */

        this.computeResult = function () {
            if(this.totalValue == 0) {
                result = 0;
            }
            else {
                result = this.totalValue/this.totalWeight;
            }
            return result;
        }
    }
    ```

1. "Kaydet" düğmesine tıkladığınızda, UDA işlevi listede görünür.

1. Tıklayın yeni işlev "TWA", işlev tanımını denetleyin.

## <a name="calling-javascript-uda-in-asa-query"></a>JavaScript UDA ASA sorguda çağırma

Azure portalında ve işinizi açın, sorguyu düzenleyin ve "uda." öneki olan uyumluluğunu doğrulamıştır TWA() işlevi çağırın. Örneğin:

```SQL
WITH value AS
(
    SELECT
    NoiseLevelDB as level,
    DurationSecond as weight
FROM
    [YourInputAlias] TIMESTAMP BY EntryTime
)
SELECT
    System.Timestamp as ts,
    uda.TWA(value) as NoseDoseTWA
FROM value
GROUP BY TumblingWindow(minute, 5)
```

## <a name="testing-query-with-uda"></a>UDA ile sorgu testi

Yerel içeren bir JSON dosyası aşağıdaki içeriği oluşturun, dosyayı karşıya yükleme için Stream Analytics işi ve yukarıdaki sorgunun test.

```JSON
[
  {"EntryTime": "2017-06-10T05:01:00-07:00", "NoiseLevelDB": 80, "DurationSecond": 22.0},
  {"EntryTime": "2017-06-10T05:02:00-07:00", "NoiseLevelDB": 81, "DurationSecond": 37.8},
  {"EntryTime": "2017-06-10T05:02:00-07:00", "NoiseLevelDB": 85, "DurationSecond": 26.3},
  {"EntryTime": "2017-06-10T05:03:00-07:00", "NoiseLevelDB": 95, "DurationSecond": 13.7},
  {"EntryTime": "2017-06-10T05:03:00-07:00", "NoiseLevelDB": 88, "DurationSecond": 10.3},
  {"EntryTime": "2017-06-10T05:05:00-07:00", "NoiseLevelDB": 103, "DurationSecond": 5.5},
  {"EntryTime": "2017-06-10T05:06:00-07:00", "NoiseLevelDB": 99, "DurationSecond": 23.0},
  {"EntryTime": "2017-06-10T05:07:00-07:00", "NoiseLevelDB": 108, "DurationSecond": 1.76},
  {"EntryTime": "2017-06-10T05:07:00-07:00", "NoiseLevelDB": 79, "DurationSecond": 17.9},
  {"EntryTime": "2017-06-10T05:08:00-07:00", "NoiseLevelDB": 83, "DurationSecond": 27.1},
  {"EntryTime": "2017-06-10T05:09:00-07:00", "NoiseLevelDB": 91, "DurationSecond": 17.1},
  {"EntryTime": "2017-06-10T05:09:00-07:00", "NoiseLevelDB": 115, "DurationSecond": 7.9},
  {"EntryTime": "2017-06-10T05:09:00-07:00", "NoiseLevelDB": 80, "DurationSecond": 28.3},
  {"EntryTime": "2017-06-10T05:10:00-07:00", "NoiseLevelDB": 55, "DurationSecond": 18.2},
  {"EntryTime": "2017-06-10T05:10:00-07:00", "NoiseLevelDB": 93, "DurationSecond": 25.8},
  {"EntryTime": "2017-06-10T05:11:00-07:00", "NoiseLevelDB": 83, "DurationSecond": 11.4},
  {"EntryTime": "2017-06-10T05:12:00-07:00", "NoiseLevelDB": 89, "DurationSecond": 7.9},
  {"EntryTime": "2017-06-10T05:15:00-07:00", "NoiseLevelDB": 112, "DurationSecond": 3.7},
  {"EntryTime": "2017-06-10T05:15:00-07:00", "NoiseLevelDB": 93, "DurationSecond": 9.7},
  {"EntryTime": "2017-06-10T05:18:00-07:00", "NoiseLevelDB": 96, "DurationSecond": 3.7},
  {"EntryTime": "2017-06-10T05:20:00-07:00", "NoiseLevelDB": 108, "DurationSecond": 0.99},
  {"EntryTime": "2017-06-10T05:20:00-07:00", "NoiseLevelDB": 113, "DurationSecond": 25.1},
  {"EntryTime": "2017-06-10T05:22:00-07:00", "NoiseLevelDB": 110, "DurationSecond": 5.3}
]
```

## <a name="get-help"></a>Yardım alın

Ek yardım için [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics) deneyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stream analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Stream Analytics sorgu dili başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Stream Analytics Yönetimi REST API Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
