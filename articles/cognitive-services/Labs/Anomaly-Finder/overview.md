---
title: Anomali Bulucu nedir? -Microsoft Bilişsel hizmetler | Microsoft Docs
description: Gelişmiş algoritmalar Anomali Finder anormallikleri zaman serisi veri tanımlamak ve Microsoft Bilişsel Hizmetleri'nde döndürmesini yardımcı olması için kullanın.
services: cognitive-services
author: tonyxing
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 04/19/2018
ms.author: tonyxing
ms.openlocfilehash: 1080bb0ad1d901a8b9a5ace4993d4e0d46924a03
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353344"
---
# <a name="what-is-anomaly-finder"></a>Anomali Bulucu nedir?

Anomali Bulucu, zaman içinde verileri izlemek ve otomatik olarak endüstri, senaryo ya da veri birimi bağımsız olarak sağ istatistiksel modelini uygulayarak benzersiz verilerinizi uyum machine learning ile anormallikleri algılamak sağlar. Bir zaman serisinin girdi olarak kullanarak, Anomali Bulucu API döndürür bir anomali veri noktası olup olmadığını belirler beklenen değer ve görselleştirme için üst ve alt sınırları. Önceden oluşturulmuş AI hizmet olarak Anomali Bulucu nasıl bir RESTful API'si kullanılacağını anlamak ötesine uzmanlık öğrenme herhangi bir makineye gerektirmez. Herhangi bir zaman serisi veri çalışır ve akış verilerini sistemleri de oluşturulabilir bu geliştirme basit ve verimli hale getirir. Anomali Bulucu kullanım durumlarının – sahtekarlık, hırsızlık yönetme, pazarda ve potansiyel iş olaylar değiştirme veya gizlilik korurken IOT cihaz trafiğini izleme örneği için finansal araçları geniş bir aralık kapsar. Bu çözüm harcama verilerde yapılan değişiklikler anlamak, yatırım veya kullanıcı etkinliği döndürmek son-müşteriler için bir hizmetin parçası olarak da monetized.
Verilerinizi daha derinden anlayabilmek ve Anomali Bulucu API deneyin. 

Bu API ile neler oluşturabileceğinizi görün:

* Beklenen zaman serisinde geçmiş verileri temel alan değerleri tahmin etmek bilgi edinin
* Veri noktası geçmiş düzeni dışında bir anomali olup olmadığını
* "Normal" değeri aralığının görselleştirmek için bir bant oluştur

![Anomaly_Finder](./media/anomaly_detection1.png) 

Fig. 1: anormallikleri satış gelirleri Algıla

![Anomaly_Finder](./media/anomaly_detection2.png)

Fig. 2: hizmet isteklerini düzeni değişiklikleri algılama

## <a name="requirements"></a>Gereksinimler

- En az veri girişi için zaman serisi: Temizle dönemsellik olmadan zaman serisi için işaret 4 döngüleri verilerin en az zaman serisi için bilinen dönemsellikle işaret 13 veri en az. 
- Veri bütünlüğü: zaman serisi veri noktaları aynı aralığı ve başka eksik noktası ayrılır. 

## <a name="identify-anomalies"></a>Anormallikleri belirle

Anomali algılama API belirli veri noktalarına anormallikleri olsun olmasın sonuç döndüren ve aşağıdaki gibi ek bilgiler sağlar
* Dönem - API anomali algılamak için kullanılan dönemsellik işaret eder.
* WarningText - olası uyarı bilgileri.
* ExpectedValue - tahmin edilen bir değer öğrenme tabanlı modeli
* IsAnomaly - veri noktalarını anormallikleri veya olup sonucu
* IsAnomaly_Neg - veri noktaları (dıps) olumsuz yönde bozukluklar olup sonucu
* IsAnomaly_Pos - veri noktaları (ani) pozitif yönde bozukluklar olup sonucu
* Veri noktası üst sınır hala normal olarak zorlayıcı UpperMargin - ExpectedValue ve UpperMargin toplamı belirler
* LowerMargin - (ExpectedValue - LowerMargin) alt sınır veri noktası hala normal olarak düşünüldüğü olduğunu belirler

> [!Note]
> UpperMargin ve LowerMargin normal değerleri aralığı görselleştirmek için gerçek zaman serisi geçici bir bandı oluşturmak için kullanılabilir. 

## <a name="adjusting-lower-and-upper-bounds-in-post-processing-on-the-response"></a>Alt ve üst sınırları yanıtta işleme postasında ayarlama

Veri noktası anomali olup olmamasına üzerinde anomali algılama API varsayılan sonuç verir ve üst ve alt sınır ExpectedValue ve UpperMargin/LowerMargin hesaplanabilir. Bu varsayılan değerler, çoğu durumda düzgün çalışması gerekir. Ancak, bazı senaryolar olanları varsayılandan farklı sınırları gerektirir. Önerilen yöntem bir coefficiency dinamik sınırları ayarlamak için UpperMargin veya LowerMargin uygulanıyor.

### <a name="examples-with-1152-as-coefficiency"></a>1.5/1/2 coefficiency olarak örnekler

![Varsayılan duyarlılık](./media/sensitivity_1.png)

![1.5 duyarlılık](./media/sensitivity_1.5.png)

![2 duyarlılık](./media/sensitivity_2.png)

Örnek verilerle isteği

[!INCLUDE [Request](./includes/request.md)]

Örnek JSON yanıt

[!INCLUDE [Response](./includes/response.md)]
