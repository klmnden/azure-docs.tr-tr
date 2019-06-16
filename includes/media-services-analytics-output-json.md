---
author: Juliako
ms.service: media-services
ms.topic: include
ms.date: 11/09/2018
ms.author: juliako
ms.openlocfilehash: 065cb4daa9501ee658d364dad43b9e03798e4083
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66160948"
---
İş, algılanan ve izlenen yüzleri hakkında meta veriler içeren bir JSON çıktı dosyası üretir. Meta veriler, ayrı ayrı izlenmesi belirten bir yüz kimliği sayı yanı sıra yüz konumunu belirten koordinatları içerir. Yüz Kimliği numaraları atanan birden çok kimlikler bazı kişiler kaynaklanan koşullar altında tamamen çıplak yüz kayıp veya çerçevede çakışan sıfırlama eğilimlidir.

Çıkış JSON aşağıdaki öğeleri içerir:

### <a name="root-json-elements"></a>Kök JSON öğeleri

| Öğe | Açıklama |
| --- | --- |
| version |Bu Video API'si sürümüne başvurur. |
| Zaman Çizelgesi |Videoyu saniye başına "ticks". |
| offset |Zaman damgaları için zaman uzaklığını budur. Video API'leri 1.0 sürümünde, bu her zaman 0 olacaktır. Gelecekte senaryoları destekliyoruz, bu değeri değiştirebilirsiniz. |
| Genişlik, yüksek |Genişlik ve piksel cinsinden çıkış video çerçevenin yüksek.|
| kare hızı |Videodaki saniye başına kare hızı. |
| [parçaları](#fragments-json-elements) |Meta verileri ayarlama parçaları olarak adlandırılan farklı parçalara öbekli. Her parçada başlangıç, süre, aralık sayısı ve olaylar vardır. |

### <a name="fragments-json-elements"></a>Parçaları JSON öğeleri

|Öğe|Açıklama|
|---|---|
| start |"Ticks." ilk olay başlangıç saati |
| Süresi |Uzunluğu parçasında "ticks." |
| index | (Yalnızca Azure Media Redactor için geçerlidir) geçerli olay çerçeve dizinini tanımlar. |
| interval |Her olay girişi parçasında "ticks." içinde aralığı |
| etkinlikler |Her bir olay algılandı ve bu süre içinde izlenen yüzleri içerir. Olayların bir dizidir. Dış dizi bir zaman aralığını temsil eder. İç dizi belirtilen noktada gerçekleşen 0 veya daha fazla olaydan oluşur. Bir boş köşeli ayraç [] yok yüz algılandı anlamına gelir. |
| id |İzlenmekte olan yüz kimliği. Bu sayı, yanlışlıkla bir yüz algılanmayan hale gelirse değişebilir. Belirli bir kişiye genel video boyunca aynı Kimliğe sahip olmalıdır, ancak bu (kapatma, vb.) algılama algoritması sınırlamaları nedeniyle garanti edilemez. |
| x, y |Sol üst tarafında X ve Y koordinatları sınırlayıcı kutu normalleştirilmiş bir ölçek 0.0 ile 1.0, yüz tanıma. <br/>-X ve Y koordinatları her zaman yatay olarak göreli bir dikey görüntü (veya baş aşağı, iOS söz konusu olduğunda) varsa, buna göre koordinatlar sırasını değiştirmek zorunda. |
| Genişlik, yükseklik |0\.0 ile 1.0 normalleştirilmiş bir ölçek kümesinde genişlik ve yükseklik yüz sınırlayıcı kutu. |
| facesDetected |Bu JSON sonuçları sonunda bulunan ve algoritma videonun sırasında algılanan yüzlerin sayısı özetler. Bir yüzü algılanmayan hale gelirse, yanlışlıkla kimlikleri sıfırlanabilir olmadığından (örneğin, yüz tanıma hemen görünür ekranı kapattıktan sağlanabilir), bu sayı her zaman true videoda yüzlerin sayısı eşit değil. |
