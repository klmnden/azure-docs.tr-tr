---
title: Anomali Bulma nedir? -Microsoft Bilişsel hizmetler | Microsoft Docs
description: Gelişmiş algoritmalar Anomali Bulucu içinde zaman serisi verilerinde anormallikleri belirlemenize ve Microsoft Bilişsel hizmetler bilgi almak için kullanın.
services: cognitive-services
author: tonyxing
ms.service: cognitive-services
ms.subservice: anomaly-detection
ms.topic: article
ms.date: 04/19/2018
ms.author: tonyxing
ms.openlocfilehash: 41d20cfd9821fbd2a34a66754e5c2da3d6439fd2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60499141"
---
# <a name="what-is-anomaly-finder"></a>Anomali Bulma nedir?

[!INCLUDE [PrivatePreviewNote](../../../../includes/cognitive-services-anomaly-finder-private-preview-note.md)]

Anomali Bulucu zaman içinde veri izlemenizi ve benzersiz verileriniz otomatik olarak sektör, senaryo veya veri hacmine bakılmaksızın hemen istatistiksel modeli uygulayarak uyum sağlayan machine learning ile anomali algılama sağlar. Zaman serisi, giriş olarak kullanarak, Anomali Bulucu API döndürür bir veri noktasına bir anomali olup olmadığını belirler beklenen değer ve görselleştirme için üst ve alt sınırları. Önceden oluşturulmuş yapay ZEKA hizmet olarak, uzmanlık bir RESTful API'sini nasıl kullanabileceğiniz anlama ötesinde öğrenme herhangi bir makineye Anomali Bulucu gerektirmez. Hiçbir zaman serisi verileri ile çalışır ve akış verilerini sistemleri derlenebilir olduğundan basit ve çok yönlü Bu geliştirme sağlar. Anomali Bulucu kullanım örneklerinin – Örneğin, finansal araçları hırsızlık, sahtekarlık yönetme, pazarda ve olası iş olayları değiştirmek veya IOT cihaz trafiği gizlilik korurken izlenmesi için geniş bir aralık kapsar. Bu çözümün son-müşterilerin harcama verilerindeki değişiklikleri anlamak, yatırım veya kullanıcı etkinliğini döndürmek bir hizmetin parçası olarak ayrıca kazanca.
Anomali Bulucu API'sini deneme ve verilerinizi daha iyi anlayabilmek. 

Bu API ile neler oluşturabileceğinizi görün:

* Beklenen zaman serisinde geçmiş verileri temel alan değerleri tahmin etmeyi öğrenin
* Bir veri noktasına bir anomali geçmiş deseni dışında olup olmadığını
* "Normal" değer aralığını görselleştirmek için bir bant oluştur

![Anomaly_Finder](./media/anomaly_detection1.png) 

Fig. 1: Satış gelirleri anomalileri algılayın

![Anomaly_Finder](./media/anomaly_detection2.png)

Fig. 2: Hizmet isteklerini desen değişiklikleri algılama

## <a name="requirements"></a>Gereksinimler

- En az veriyi giriş zaman serisi için: En az 4 döngüleri veri için zaman serisi bilinen dönemsellikle işaret Temizle dönemsellik olmadan zaman serisi için en az 13 veri noktaları. 
- Veri bütünlüğü: zaman serisi veri noktaları aynı zaman aralığında hiçbir eksik noktaları de ayrılır. 

## <a name="identify-anomalies"></a>Desenlerinde

Anomali algılama API'si belirli veri noktalarına anomalileri olsun olmasın, sonuç verir ve aşağıdaki gibi ek bilgiler sağlar
* Dönem - anomali algılama API'si kullanılan dönemsellik işaret eder.
* WarningText - olası uyarı bilgileri.
* ExpectedValue - öğrenme tahmin edilen değer tabanlı modeli
* IsAnomaly - veya veri noktaları anomalileri mi olduğunuza sonucu
* IsAnomaly_Neg - veri noktaları (dıps) olumsuz yönde anomalileri olup üzerinde sonucu
* IsAnomaly_Pos - veri noktaları (ani) olumlu yönde anomalileri olup üzerinde sonucu
* Veri noktası üst sınırını hala normal olarak düşünülebilir ExpectedValue ve UpperMargin UpperMargin - belirler
* LowerMargin - (ExpectedValue - LowerMargin), alt sınır veri noktası hala normal olarak düşünüldüğü olduğunu belirler

> [!Note]
> UpperMargin ve LowerMargin normal değerleri aralığı görselleştirmek için gerçek zaman serisi etrafında bir bant oluşturmak için kullanılabilir. 

## <a name="adjusting-lower-and-upper-bounds-in-post-processing-on-the-response"></a>Alt ve üst sınırları yanıtta işleme gönderisinde ayarlama

Anomali algılama API'si, bir veri noktasının anomali olup olmamasına üzerinde varsayılan sonucu döndürür ve üst ve alt sınır ExpectedValue ve UpperMargin/LowerMargin hesaplanabilir. Bu varsayılan değerler çoğu için düzgün çalışması gerekir. Ancak, bazı senaryolar olanları varsayılandan farklı sınırları gerektirir. Önerilen yöntem bir coefficiency dinamik sınırlarını ayarlamak için UpperMargin veya LowerMargin uyguluyor.

### <a name="examples-with-1152-as-coefficiency"></a>1.5/1/2 olarak coefficiency örnekleri

![Varsayılan duyarlılık](./media/sensitivity_1.png)

![1.5 duyarlılık](./media/sensitivity_1.5.png)

![2 duyarlılık](./media/sensitivity_2.png)

Örnek verilerle iste

[!INCLUDE [Request](./includes/request.md)]

Örnek JSON yanıtı

[!INCLUDE [Response](./includes/response.md)]
