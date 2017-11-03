---
title: "Azure Stream Analytics JavaScript kullanıcı tanımlı toplamlarda | Microsoft Docs"
description: "JavaScript kullanıcı tanımlı toplamlarda ile Gelişmiş sorgu mekanizması gerçekleştirmek"
keywords: "JavaScript, toplamalar, uda kullanıcı tanımlı"
services: stream-analytics
author: minhe-msft
manager: santoshb
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 10/28/2017
ms.author: minhe
ms.openlocfilehash: b3863a34ed146e54c6d60e035957b942a1976ff9
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="azure-stream-analytics-javascript-user-defined-aggregates-preview"></a>Azure Stream Analytics JavaScript kullanıcı tanımlı toplamlarda (Önizleme)

Azure Stream Analytics JavaScript'te yazılmış kullanıcı tanımlı toplamlarda (UDA) destekler, karmaşık durum bilgisi olan iş mantığı uygulamanız olanak tanır. İçinde UDA durumu veri yapısı, durumu Birikme, durumu decumulation ve birleşik sonuç hesaplama tam denetime sahiptir. Makaleyi JavaScript UDA arabirimleri, iki farklı bir UDA ve akış analizi sorgu işlemleri Windows tabanlı olan UDA kullanmayı oluşturmak için adımları sunar.

## <a name="javascript-user-defined-aggregates"></a>JavaScript kullanıcı tanımlı toplamlarda

Bir kullanıcı tanımlı toplama en üstünde bir zaman penceresi belirtimi bu pencerede olayları üzerinden toplamak ve tek bir sonuç değeri üretmek için kullanılır. Stream Analytics AccumulateOnly ve AccumulateDeaccumulate bugün destekler UDA arabirimleri iki tür vardır. Her iki tür UDA atlayan pencereyi, atlamalı pencere ve kayan pencere tarafından kullanılabilir. AccumulateDeaccumulate UDA AccumulateOnly atlamalı pencere ve kayan pencere ile birlikte kullanıldığında UDA daha iyi gerçekleştirir. Kullandığınız algoritmadan yola çıkılarak iki türlerinden birini seçin.

### <a name="accumulateonly-aggregates"></a>AccumulateOnly toplar

AccumulateOnly toplamalar yalnızca yeni olayları durumuna birikebilir, algoritma değerlerin deaccumulation izin vermiyor. Bu toplama türünü seçin, bir olay deaccumulate uygulamak durum değeri bilgilerinden imkansızdır. AccumulatOnly Toplamalar için JavaScript şablonu aşağıda verilmiştir:

````JavaScript
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
````

### <a name="accumulatedeaccumulate-aggregates"></a>AccumulateDeaccumulate toplar

AccumulateDeaccumulate toplamalar durumundan bir önceki birikmiş değerinin deaccumulation örneğin izin, olay değerler listesinden bir anahtar-değer çifti kaldırın ya da bir değer Sum birleşik bir durumdan çıkarma. AccumulateDeaccumulate Toplamalar için JavaScript şablonu aşağıda verilmiştir:

````JavaScript
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
````

## <a name="uda---javascript-function-declaration"></a>UDA - JavaScript işlevi bildirimi

Her JavaScript UDA işlevi nesne bildirimi tarafından tanımlanır. Aşağıdaki UDA tanımında başlıca öğeleridir.

### <a name="function-alias"></a>İşlev diğer adı

İşlev diğer UDA tanımlayıcısıdır. Stream Analytics sorgu çağrıldığında, her zaman UDA diğer bir "uda." ile birlikte kullanın önek.

### <a name="function-type"></a>İşlev türü

UDA için işlevi türü olmalıdır. **Javascript UDA**.

### <a name="output-type"></a>Çıktı türü

Sorgunuzdaki türünü işlemek istediğiniz "Herhangi bir" IF veya belirli bir desteklenen, Stream Analytics işi yazın.

### <a name="function-name"></a>İşlev adı

Bu işlev nesnesinin adı. İşlev tam anlamıyla UDA diğer adıyla aynı olmalıdır (Önizleme davranışı, biz destek anonim işlevi dikkate zaman GA).

### <a name="method---init"></a>Yöntem - init()

İnit() yöntemi toplama durumunu başlatır. Pencere başlatıldığında, bu yöntem çağrılır.

### <a name="method--accumulate"></a>Yöntem – accumulate()

Accumulate() yöntemi önceki bir duruma ve geçerli olay değerlerine dayalı UDA durumu hesaplar. Bu yöntem, bir olay bir zaman penceresi (TUMBLINGWINDOW, HOPPINGWINDOW veya SLIDINGWINDOW) girdiğinde çağrılır.

### <a name="method--deaccumulate"></a>Yöntem – deaccumulate()

Deaccumulate() yöntemi önceki bir duruma ve geçerli olay değerlerine dayalı durumu yeniden hesaplar. Bir olay bir SLIDINGWINDOW ayrıldığında bu yöntem çağrılır.

### <a name="method--deaccumulatestate"></a>Yöntem – deaccumulateState()

DeaccumulateState() yöntemi önceki bir duruma ve bir atlama durumu göre durumu yeniden hesaplar. Bu yöntem, olayları bırakın HOPPINGWINDOW bir dizi olduğunda çağrılır.

### <a name="method--computeresult"></a>Yöntem – computeResult()

ComputeResult() yöntemi geçerli durumuna göre toplama sonucunu döndürür. Bu yöntem, bir zaman penceresi (TUMBLINGWINDOW, HOPPINGWINDOW ve SLIDINGWINDOW) sonunda çağrılır.

## <a name="javascript-uda-supported-input-and-output-data-types"></a>JavaScript UDA desteklenen girdi ve çıktı veri türleri
JavaScript UDA veri türleri için bölümüne bakın **akış analizi ve JavaScript türü dönüştürme** , [tümleştirmek JavaScript UDF'ler](stream-analytics-javascript-user-defined-functions.md).

## <a name="adding-a-javascript-uda-from-the-azure-portal"></a>Azure portalından bir JavaScript UDA ekleme

Aşağıda biz portalından bir UDA oluşturma işleminde size yol. Burada kullandığımız örnek ağırlıklı zaman ortalamalar bilgisayar.

Şimdi bir JavaScript UDA varolan ASA işini altında aşağıdaki adımlarla oluşturalım.

1. Azure portalında oturum açın ve var olan Stream Analytics işi bulun.
1. İşlev'in altında tıklayın **iş TOPOLOJİ**.
1. Tıklayın **Ekle** yeni bir işlev eklemek için simge.
1. Yeni işlev görünümü seçin **JavaScript UDA** işlevi türü olarak sonra Düzenleyicisi'nde görünmesini varsayılan UDA şablonu görürsünüz.
1. UDA diğer ad olarak "TWA" doldurun ve aşağıdaki olarak işlev uygulaması değiştirin:

    ````JavaScript
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
    ````

1. "Kaydet" düğmesine tıkladığınızda, UDA işlevi listede görüntülenir.

1. Tıklatın yeni işlev "TWA", işlev tanımının kontrol edebilirsiniz.

## <a name="calling-javascript-uda-in-asa-query"></a>JavaScript UDA ASA sorguda çağırma

Azure portalında işinizin açın, sorguyu düzenlemek ve zorunluluğuna önekiyle "uda." TWA() işlevini çağırın. Örneğin:

````SQL
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
````

## <a name="testing-query-with-uda"></a>Sorgu UDA ile test etme

İçerik aşağıda yerel JSON dosya oluştur, akış analizi işine dosyayı karşıya yüklemeyi ve sorgu test.

````JSON
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
````

## <a name="get-help"></a>Yardım alın

Ek Yardım için deneyin bizim [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stream Analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Stream Analytics sorgu dili başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Stream Analytics Yönetimi REST API Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
