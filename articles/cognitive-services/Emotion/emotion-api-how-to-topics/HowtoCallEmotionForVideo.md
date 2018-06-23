---
title: Video için duygu tanıma API'si çağırma | Microsoft Docs
description: Bilişsel hizmetler videoda duygu API çağrısı öğrenin.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 02/06/2017
ms.author: anroth
ms.openlocfilehash: 0875013b2061a84e3e23ae90c1106382672fdca6
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352445"
---
# <a name="how-to-call-emotion-api-for-video"></a>Video için duygu API çağrısı yapma

> [!IMPORTANT]
> Video API Önizleme 30 Ekim 2017 sona erer. Yeni deneyin [Video dizin oluşturucu API önizlemesi](https://azure.microsoft.com/services/cognitive-services/video-indexer/) kolayca videoların öngörüleri ayıklamak ve konuşulan sözcüklerin, yüzler, karakterler ve duygular algılayarak arama sonuçları gibi içerik bulma deneyimlerini geliştirmek üzere. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview).

Bu kılavuz Video için duygu tanıma API'si çağırmayı gösterir. Örnekler C# ' ta duygu tanıma API'si için Video istemci kitaplığı kullanılarak yazılmıştır.

### <a name="Prep">Hazırlama</a> 
Video için duygu tanıma API'si kullanabilmeniz için kişiler, tercihen video kamerayı kişilerin burada karşılaştığı içeren bir video gerekir.

### <a name="Step1">1. adım: API çağrısı yetkilendirme</a> 
Her duygu API çağrısı Video için bir abonelik anahtarı gerektirir. Bu anahtarı, istek başlığında belirtilen ya da bir sorgu dizesi parametresi geçirilen gerekir. Bir sorgu dizesi abonelik tuşuyla geçirmek için istek URL'si duygu tanıma API'si Video için örnek olarak bakın:

```
https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognizeInVideo&subscription-key=<Your subscription key>
```

Alternatif olarak, abonelik anahtarı HTTP istek üstbilgisinde belirtilebilir:

```
ocp-apim-subscription-key: <Your subscription key>
```

Bir istemci kitaplığı kullanırken, abonelik anahtarı içinde VideoServiceClient sınıf oluşturucu kullanılarak geçirilir. Örneğin:

```
var emotionServiceClient = new emotionServiceClient("Your subscription key");
```
Abonelik anahtarı edinmek için [abonelikleri] bakın (https://azure.microsoft.com/try/cognitive-services/). 

### <a name="Step2">2. adım: bir video hizmetine yüklemek ve durumunu denetleyin</a>
Video çağrıları için duygu API gerçekleştirin en temel bir video doğrudan karşıya yükleyerek yoludur. Bu, bir video dosyasından okunan veriler ile birlikte uygulama/octet-akış içerik türüyle bir "POST" isteği göndererek gerçekleştirilir. Video en büyük boyutu 100 MB'tır.

İstemci kitaplığı kullanılarak, karşıya yükleme yoluyla sabitlemeyi geçirme akış nesnesindeki gerçekleştirilir. Aşağıdaki örneğe bakın:

```
Operation videoOperation;
using (var fs = new FileStream(@"C:\Videos\Sample.mp4", FileMode.Open))
{
      videoOperation = await videoServiceClient.CreateOperationAsync(fs, OperationType.recognizeInVideo);
}
```

Lütfen VideoServiceClient CreateOperationAsync yöntemini zaman uyumsuz olduğunu unutmayın. Arama yöntemi zaman uyumsuz olarak bekleme yan tümcesi kullanmak için de işaretlenmesi gerekir.
Video Web'de zaten ve genel bir URL varsa, Video duygu tanıma API'si URL sağlayarak erişilebilir. Bu örnekte, istek gövdesini URL içeren bir JSON dizesinde olacaktır.

İstemci kitaplığı kullanılarak, bir URL yoluyla sabitlemeyi kolayca başka bir aşırı yüklemesini CreateOperationAsync yöntemi kullanılarak çalıştırılabilir.


```
var videoUrl = "http://www.example.com/sample.mp4";
Operation videoOperation = await videoServiceClient.CreateOperationAsync(videoUrl, OperationType. recognizeInVideo);

```

Bu yükleme yöntemi Video çağrıları için tüm duygu tanıma API'si için aynı olacaktır. 

Bir video karşıya yüklediğiniz sonra gerçekleştirmek istediğiniz sonraki durumunu denetlemek için bir işlemdir. Video dosyaları genellikle daha büyük ve diğer dosyaları daha farklı olduğundan, kullanıcılar bir uzun bekleyebilirsiniz Bu adımda işleme süresi. Zaman boyutu ve dosya uzunluğu bağlıdır.

İstemci kitaplığı kullanılarak, işlem durumunu ve sonuç GetOperationResultAsync yöntemini kullanarak alabilirsiniz.


```
var operationResult = await videoServiceClient.GetOperationResultAsync(videoOperation);

```
Genellikle, istemci tarafı düzenli aralıklarla durumu "Başarılı" veya "Başarısız" gösterilir kadar işlem durumunu alma.

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

VideoOperationResult durumunu "sonucu başarılı olarak" zaman gösterilen alınabilir bir VideoOperationInfoResult için VideoOperationResult atama tarafından<VideoAggregateRecognitionResult> ve ProcessingResult alan erişme.

```
var emotionRecognitionJsonString = ((VideoOperationInfoResult<VideoAggregateRecognitionResult>)operationResult).ProcessingResult;
```

### <a name="Step3">3. adım: Alma ve duygu tanıma anlama ve JSON çıktısını izleme</a>

Elde edilen sonucu yüzeyleri JSON biçiminde verilen dosyasındaki meta verileri içerir.

Durumunu "Başarılı" gösterildiğinde 2. adımda açıklandığı gibi JSON çıktısını OperationResult, ProcessingResult alanında kullanılabilir.

Yüz algılama ve JSON izleme aşağıdaki öznitelikleri içerir:

Öznitelik | Açıklama
-------------|-------------
Sürüm | Video JSON için duygu API sürümüne başvuruyor.
Zaman Çizelgesi | Videonun saniyede "çizgilerine".
Uzaklık  |Zaman damgaları zaman uzaklığı. Sürümünde videolar için duygu API 1.0, bu her zaman 0 olacaktır. Gelecekteki desteklenen senaryolarda, bu değeri değiştirebilirsiniz.
Kare hızı | Videonun Saniyedeki çerçeve sayısı.
Parçaları   | Meta veri parçaları olarak adlandırılan farklı daha küçük parçalara kesilir. Her parça başlangıç, süre, aralığı sayısı ve olay içerir.
Başlatma   | Çizgilerine ilk olay, başlangıç saati.
Süre |  Parçadaki çizgilerine uzunluğu.
Aralık |  Her olay parçadaki çizgilerine içinde uzunluğu.
Olaylar  | Olaylar dizisi. Dış dizi bir zaman aralığı temsil eder. İç dizi 0 veya zamandaki o noktada daha fazla olayları oluşur.
windowFaceDistribution |    Olayı sırasında belirli bir duygu tanıma için yüzde yüz.
windowMeanScores |  Puanları görüntüdeki yazıtipleri, her duygu tanıma için anlamına gelir.

Bu şekilde JSON biçimlendirme için burada meta verileri hızlı bir şekilde almak ve sonuçları büyük bir akışa yönetmek önemli olacak gelecekteki senaryoları için API'ları ayarlamak için nedenidir. Bu biçimlendirme teknikleri (zamana dayalı bölümleri meta bölmeniz sağlayarak, yalnızca ihtiyacınız yükleyebileceğiniz) parçalanma ve kesimleme (çok büyük alırsanız olaylarını Kes sağlayarak) kullanıyor. Bazı basit hesaplamalar, veri dönüştürme yardımcı olabilir. Örneğin, bir olay 2997 (çizgilerine/sn) bir ölçeğini 6300 (çizgilerine) başlatılan ve kare 29.97 (Çerçeve/sn), ardından hızına:

*   Başlat/ölçeği = 2.1 saniye
*   Saniye (kare hızı/zaman çizelgesi) x 63 çerçeveler =

JSON içinde ayıklanması, basit bir örnek aşağıdadır bir çerçeve biçimi yüz algılama ve izleme için başına:

```
var emotionRecognitionTrackingResultJsonString = operationResult.ProcessingResult;
var emotionRecognitionTracking = JsonConvert.DeserializeObject<EmotionRecognitionResult>(emotionRecognitionTrackingResultJsonString, settings);
```
Şimdiye kadar sonuçlarınız üstteki özgün video yer paylaşımı için bir görsel öğe yapı duygular zamanla düzleştirilmiş olduğundan, sağlanan zaman damgaları gelen 250 milisaniye çıkarın.

### <a name="Summary">Özet</a>
Bu kılavuzda duygu API işlevleri hakkında Video için öğrendiniz: video yükleyebilirsiniz nasıl durumunu denetleyin, duygu tanıma meta verilerini almak.

API Başvurusu Kılavuzu API ayrıntıları hakkında daha fazla bilgi için bkz: "[Video başvurusu için duygu API](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/56f8d40e1984551ec0a0984e)".
