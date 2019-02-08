---
title: "Örnek: Video için duygu tanıma API'si çağırma"
titlesuffix: Azure Cognitive Services
description: Bilişsel Hizmetlerde Video için Duygu Tanıma API'sini çağırmayı öğrenin.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: emotion-api
ms.topic: sample
ms.date: 02/06/2017
ms.author: anroth
ROBOTS: NOINDEX
ms.openlocfilehash: 191e20ba608adfdfd1ea4e5479cf1d326640d378
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55862794"
---
# <a name="example-call-emotion-api-for-video"></a>Örnek: Video için duygu tanıma API'si çağırma

> [!IMPORTANT]
> Duygu Tanıma API'si 15 Şubat 2019 tarihinde kullanım dışı bırakılacaktır. Duygu tanıma özelliği [Yüz Tanıma API'sinin](https://docs.microsoft.com/azure/cognitive-services/face/) bir parçası olarak genel kullanıma sunulmuştur. 

Bu kılavuzda Video için Duygu Tanıma API'sini çağırma adımları gösterilmektedir. Örnekler, Video için Duygu Tanıma API'si istemci kitaplığı kullanılarak C# dilinde yazılmıştır.

### <a name="Prep">Hazırlık</a>
Video için Duygu Tanıma API'sini kullanmak üzere insanların bulunduğu bir videoya ihtiyacınız olacaktır. İnsanların yüzlerinin kameraya dönük olduğu bir video kullanmanız önerilir.

### <a name="Step1">1. adım: API çağrısı Yetkilendir</a>
Video için Duygu Tanıma API'sine yapılan her çağrı için bir abonelik anahtarı gerekir. Bu anahtarın bir sorgu dizesi parametresi aracılığıyla geçirilmesi veya istek üst bilgisinde belirtilmesi gerekir. Sorgu dizesi aracılığıyla abonelik anahtarını geçirmek için aşağıdaki örnek Video için Duygu Tanıma API'si istek URL'sine bakın:

```
https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognizeInVideo&subscription-key=<Your subscription key>
```

Abonelik anahtarı alternatif olarak HTTP isteği üst bilgisinde de belirtilebilir:

```
ocp-apim-subscription-key: <Your subscription key>
```

Bir istemci kitaplığı kullanılırken, VideoServiceClient sınıfının oluşturucusu aracılığıyla abonelik anahtarı geçirilir. Örneğin:

```
var emotionServiceClient = new emotionServiceClient("Your subscription key");
```
Bir abonelik anahtarı almak için bkz. [Abonelikler](https://azure.microsoft.com/try/cognitive-services/).

### <a name="Step2">2. adım: Hizmete karşıya video yükleme ve durumunu denetle</a>
Video için Duygu Tanıma API'si çağrısı yapmanın en kolay yolu doğrudan bir video yüklemektir. Video dosyasından okunan verilerle uygulama/sekizli akış içerik türü ile bir "POST" isteği gönderilerek bu yapılır. Maksimum video boyutu 100 MB’tır.

İstemci kitaplığı kullanılarak karşıya yükleme aracılığıyla sabitleme bir akış nesnesi geçirilerek gerçekleştirilir. Aşağıdaki örneğe bakın:

```
Operation videoOperation;
using (var fs = new FileStream(@"C:\Videos\Sample.mp4", FileMode.Open))
{
      videoOperation = await videoServiceClient.CreateOperationAsync(fs, OperationType.recognizeInVideo);
}
```

VideoServiceClient CreateOperationAsync metodunun zaman uyumsuz olduğuna dikkat edin. await yan tümcesini kullanmak için çağırma yöntemi de zaman uyumsuz olarak işaretlenmelidir.
Video web üzerindeyse ve genel URL'si varsa Video için Duygu Tanıma API'sine URL'yi de sağlayabilirsiniz. Bu örnekte istek gövdesi, URL’yi içeren bir JSON dizesi olacaktır.

İstemci kitaplığını kullanarak URL aracılığıyla sabitleme başka bir CreateOperationAsync metodu aşırı yüklemesiyle kolayca yürütülebilir.


```
var videoUrl = "http://www.example.com/sample.mp4";
Operation videoOperation = await videoServiceClient.CreateOperationAsync(videoUrl, OperationType. recognizeInVideo);

```

Bu yükleme metodu, tüm Video için Duygu Tanıma API'si çağrıları için aynı olacaktır.

Videoyu yükledikten sonra yapmak isteyeceğini işlem, durumunu denetlemektir. Video dosyaları diğer dosyalara kıyasla genellikle daha büyük ve karmaşık olduğundan kullanıcılar bu adımda uzun bir işlem süresiyle karşılaşabilir. İşlemin süresi dosyanın boyutuna ve uzunluğuna göre farklılık gösterecektir.

İstemci kitaplığını kullanarak GetOperationResultAsync metoduyla işlem durumunu ve sonucu alabilirsiniz.


```
var operationResult = await videoServiceClient.GetOperationResultAsync(videoOperation);

```
Normalde istemci tarafı “Başarılı” veya “Başarısız” durumunu alana kadar belirli aralıklarla işlem durumunu almalıdır.

```
OperationResult operationResult;
while (true)
{
      operationResult = await videoServiceClient.GetOperationResultAsync(videoOperation);
      if (operationResult.Status == OperationStatus.Succeeded || operationResult.Status == OperationStatus.Failed)
      {
           break;
      }

      Task.Delay(30000).Wait();
}

```

VideoOperationResult durumu “Başarılı” olarak gösterildiğinde sonuç VideoOperationResult değerinin VideoOperationInfoResult<VideoAggregateRecognitionResult> öğesine iletilmesi ve ProcessingResult alanına erişilmesiyle alınabilir.

```
var emotionRecognitionJsonString = ((VideoOperationInfoResult<VideoAggregateRecognitionResult>)operationResult).ProcessingResult;
```

### <a name="Step3">3. adım: Alma ve duygu tanıma anlama ve JSON çıkışını izleme</a>

Çıkış sonucunda verilen dosyadaki yüzlerin meta verileri JSON biçiminde sağlanır.

2. Adımda anlatıldığı gibi JSON çıktısı, durum "Başarılı" olarak gösterildikten sonra OperationResult öğesinin ProcessingResult alanında bulunur.

Yüz algılama ve tanıma JSON çıktısı şu özniteliklere sahiptir:

Öznitelik | Açıklama
-------------|-------------
Sürüm | Video için Duygu Tanıma API'si JSON sürümünü belirtir.
Timescale | Videonun her saniyesinde tıklar.
Uzaklık  |Zaman damgası için saat farkıdır. Videolar için Duygu Tanıma API'sinin 1.0 sürümünde bu değer her zaman 0 olacaktır. İleride desteklenen senaryolarda bu değer değişebilir.
Framerate | Videodaki saniye başına kare hızı.
Fragments   | Meta veriler, parçalar olarak adlandırılan daha küçük parçalara ayrılır. Her parçada başlangıç, süre, aralık sayısı ve olaylar vardır.
Başlatma   | İlk olayın başlangıç zamanı, tık cinsinde belirtilir.
Süre |  Parçanın uzunluğu, tık cinsinde belirtilir.
Interval |  Parça içindeki her bir olayın uzunluğu, tık cinsinde belirtilir.
Olaylar  | Olaylardan oluşan bir dizidir. Dış dizi bir zaman aralığını temsil eder. İç dizi belirtilen noktada gerçekleşen 0 veya daha fazla olaydan oluşur.
windowFaceDistribution |    Olay sırasında belirli bir duyguya sahip olan yüzlerin yüzdesi.
windowMeanScores |  Görüntüdeki yüzlerin her bir duygusu için ortalama puanlar.

JSON verilerinin bu şekilde biçimlendirilmesinin nedeni, API'leri gelecekteki senaryolar için hazırlamaktır. Gelecekte meta verileri hızlı bir şekilde alma ve büyük çaplı bir sonuç akışını yönetme ihtiyacı doğabilir. Bu biçimlendirmede hem parçalama (meta verilerini zamana dayalı parçalara ayırarak yalnızca ihtiyacınız olan bölümü indirme) hem de bölümleme (çok büyük olayları bölümlere ayırma) teknikleri kullanılmaktadır. Bazı basit hesaplamalar, verileri dönüştürmenize yardımcı olabilir. Örneğin bir olay 6300 (tık) noktasında başlıyorsa ve ölçeği 2997 (tık/sn), kare hızı da 29,97 (kare/sn) ise, :

*   Başlangıç/Ölçek = 2,1 saniye
*   Saniye x (Kare hızı/Ölçek) = 63 kare

Aşağıdaki basit örnekte JSON verilerinin yüz algılama ve izleme için kare başına biçimde ayıklama adımları gösterilmiştir:

```
var emotionRecognitionTrackingResultJsonString = operationResult.ProcessingResult;
var emotionRecognitionTracking = JsonConvert.DeserializeObject<EmotionRecognitionResult>(emotionRecognitionTrackingResultJsonString, settings);
```
Duygular zaman içinde değiştiğinden sonuçlarınızı özgün videonun üzerine yerleştiren bir görselleştirme oluşturmanız durumunda verilen zaman damgalarından 250 milisaniye çıkarmanız gerekir.

### <a name="Summary">Özet</a>
Bu kılavuzda video yükleme, durumunu denetleme ve duygu tanıma meta verilerini alma gibi Video için Duygu Tanıma API'si işlevleri hakkında bilgi edindiniz.

API ayrıntıları hakkında daha fazla bilgi için API başvuru kılavuzunu inceleyin: “[Video için Duygu Tanıma API'si Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/56f8d40e1984551ec0a0984e)”.
