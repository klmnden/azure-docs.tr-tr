---
title: Büyük ölçekli özelliği yüz API kullanın | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Büyük ölçekli özelliği yüz, Bilişsel içinde API hizmetlerini kullanın.
services: cognitive-services
author: SteveMSFT
manager: corncar
ms.service: cognitive-services
ms.component: face-api
ms.topic: article
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: f7b3ac57cf6b24c8a90b4ea59757d3a2cfafd781
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351862"
---
# <a name="how-to-use-the-large-scale-feature"></a>Büyük ölçekli özelliğinin nasıl kullanılacağı

Bu kılavuz var olan PersonGroup ve FaceList LargePersonGroup ve LargeFaceList sırasıyla ölçeği kod geçiş üzerinde Gelişmiş bir makaledir.
Bu kılavuz temel kullanım PersonGroup ve FaceList bilmesinin varsayım ile geçiş işlemi gösterilmektedir.
İle temel işlemleri hakkında bilgi almak için lütfen diğer öğreticileri gibi bakın [görüntülerinde yüzeyleri tanımlamak nasıl](HowtoIdentifyFacesinImage.md),

Microsoft yüz API son LargePersonGroup ve için büyük ölçekli işlemleri olarak anılan LargeFaceList, büyük ölçekli senaryoları etkinleştirmek için iki özellik yayımladı.
LargeFaceList kadar 1.000.000 yüzeyleri tutabilir ve LargePersonGroup 1.000.000 kişi her en fazla 248 yüzeyleri kadar içerebilir.

Büyük ölçekli işlemler geleneksel PersonGroup ve FaceList benzerdir, ancak nedeniyle yeni mimarisinin önemli bazı farklar vardır.
Bu kılavuz temel kullanım PersonGroup ve FaceList bilmesinin varsayım ile geçiş işlemi gösterilmektedir.
Örnekler C# ' ta yüz API'si istemci kitaplığı kullanılarak yazılır.

Büyük ölçekli tanımlanması ve FindSimilar yüz arama performansı etkinleştirmek için LargeFaceList ve LargePersonGroup önceden işlemek için bir tren işlemi tanıtmak için gerekir.
Eğitim süresini saniyeden yaklaşık yarım saat gerçek kapasite bağlı olarak farklılık gösterir.
Eğitim dönemde önce başarılı bir eğitim yapıldığında tanımlama ve FindSimilar gerçekleştirmek hala mümkündür.
Ancak, dezavantajı, büyük ölçekli eğitim için yeni bir posta geçiş tamamlanana kadar yeni eklendi kişi/yüzeyleri sonucunda görünmez olur.

## <a name="concepts"></a> Kavramları

Bu kılavuzda aşağıdaki kavramlar hakkında bilgi sahibi değilseniz tanımları bulunabilir [sözlüğü](../Glossary.md):

- LargePersonGroup: Koleksiyonu kişi 1.000.000 kadar kapasiteye sahip.
- LargeFaceList: Koleksiyonu yüzeyleri 1.000.000 kadar kapasiteye sahip.
- Tren: Kimliği/FindSimilar bir performans sağlamak için bir ön işleme.
- Tanımlama: bir veya daha fazla yüzeyleri PersonGroup veya LargePersonGroup tanımlayın.
- FindSimilar: FaceList veya LargeFaceList benzer yüzeyleri arayın.

## <a name="initialization"></a> 1. adım: API çağrısı yetkilendirme

Yüz API'si istemci kitaplığı kullanırken, abonelik anahtarı ve abonelik bitiş noktası içinde FaceServiceClient sınıf oluşturucu kullanılarak geçirilir. Örneğin:

```CSharp
string SubscriptionKey = "<Subscription Key>";
// Use your own subscription endpoint corresponding to the subscription key.
string SubscriptionRegion = "https://westcentralus.api.cognitive.microsoft.com/face/v1.0/";
FaceServiceClient FaceServiceClient = new FaceServiceClient(SubscriptionKey, SubscriptionRegion);
```

İlgili uç noktası ile aboneliği anahtar Azure portal'ınızın Market sayfasından elde edilebilir.
Bkz: [abonelikleri](https://azure.microsoft.com/services/cognitive-services/directory/vision/).

## <a name="migrate"></a> 2. adım: Kodu geçiş eylem

Bu bölüm yalnızca LargePersonGroup/LargeFaceList geçirme PersonGroup/FaceList uygulamasına odaklanır.
Tasarım ve iç uygulama LargePersonGroup/LargeFaceList PersonGroup/FaceList farklı rağmen geri uyumluluk için API Arabirimleri benzerdir.

Veri Taşıma desteklenmiyor, bunun yerine LargePersonGroup/LargeFaceList oluşturmanız gerekebilir.

## <a name="largepersongroup"></a> 2.1. adım: PersonGroup LargePersonGroup için geçirme

Tam olarak aynı grup düzeyinde işlemlerine paylaştıkları gibi LargePersonGroup PersonGroup geçiş kesintisiz gereklidir.

Uygulama, PersonGroup/kişi için ilgili API yolları veya SDK sınıf/modülü LargePersonGroup ve LargePersonGroup kişi değiştirmek yalnızca gereklidir.

Veri geçişi bakımından bkz [eklemek bakarken nasıl](how-to-add-faces.md) başvuru.

## <a name="largefacelist"></a> 2.2. adım: FaceList LargeFaceList için geçirme

| FaceList API'leri | LargeFaceList API'leri |
|:---:|:---:|
| Oluştur | Oluştur |
| Sil | Sil |
| Al | Al |
| Liste | Liste |
| Güncelleştirme | Güncelleştirme |
| - | Eğitin |
| - | Eğitim durumunu Al |

Önceki tabloda FaceList LargeFaceList arasındaki listesi düzeyi işlemlerin bir karşılaştırması bulunur.
Gösterilen şekilde LargeFaceList yeni işlemler, eğitme ve eğitim durumunu Al FaceList ile karşılaştırıldığında gelir.
Bir önkoşulu olan eğitilmiş LargeFaceList alma [FindSimilar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) işlemdir orada sırasında FaceList için gerekli hiçbir eğitimi.
Aşağıdaki kod parçacığında, bir LargeFaceList eğitim için beklenecek bir yardımcı işlevdir.

```CSharp
/// <summary>
/// Helper function to train LargeFaceList and wait for finish.
/// </summary>
/// <remarks>
/// The time interval can be adjusted considering the following factors:
/// - The training time which depends on the capacity of the LargeFaceList.
/// - The acceptable latency for getting the training status.
/// - The call frequency and cost.
///
/// Estimated training time for LargeFaceList in different scale:
/// -     1,000 faces cost about  1 to  2 seconds.
/// -    10,000 faces cost about  5 to 10 seconds.
/// -   100,000 faces cost about  1 to  2 minutes.
/// - 1,000,000 faces cost about 10 to 30 minutes.
/// </remarks>
/// <param name="largeFaceListId">The Id of the LargeFaceList for training.</param>
/// <param name="timeIntervalInMilliseconds">The time interval for getting training status in milliseconds.</param>
/// <returns>A task of waiting for LargeFaceList training finish.</returns>
private static async Task TrainLargeFaceList(
    string largeFaceListId,
    int timeIntervalInMilliseconds = 1000)
{
    // Trigger a train call.
    await FaceServiceClient.TrainLargeFaceListAsync(largeFaceListId);

    // Wait for training finish.
    while (true)
    {
        Task.Delay(timeIntervalInMilliseconds).Wait();
        var status = await FaceServiceClient.GetLargeFaceListTrainingStatusAsync(largeFaceListId);

        if (status.Status == Status.Running)
        {
            continue;
        }
        else if (status.Status == Status.Succeeded)
        {
            break;
        }
        else
        {
            throw new Exception("The train operation is failed!");
        }
    }
}
```

Daha önce yüzler ve FindSimilar ekleme ile FaceList tipik bir kullanımı olacaktır

```CSharp
// Create a FaceList.
const string FaceListId = "myfacelistid_001";
const string FaceListName = "MyFaceListDisplayName";
const string ImageDir = @"/path/to/FaceList/images";
FaceServiceClient.CreateFaceListAsync(FaceListId, FaceListName).Wait();

// Add Faces to the FaceList.
Parallel.ForEach(
    Directory.GetFiles(ImageDir, "*.jpg"),
    async imagePath =>
        {
            using (Stream stream = File.OpenRead(imagePath))
            {
                await FaceServiceClient.AddFaceToFaceListAsync(FaceListId, stream);
            }
        });

// Perform FindSimilar.
const string QueryImagePath = @"/path/to/query/image";
var results = new List<SimilarPersistedFace[]>();
using (Stream stream = File.OpenRead(QueryImagePath))
{
    var faces = FaceServiceClient.DetectAsync(stream).Result;
    foreach (var face in faces)
    {
        results.Add(await FaceServiceClient.FindSimilarAsync(face.FaceId, FaceListId, 20));
    }
}
```

LargeFaceList için geçiş yaparken duruma gelir

```CSharp
// Create a LargeFaceList.
const string LargeFaceListId = "mylargefacelistid_001";
const string LargeFaceListName = "MyLargeFaceListDisplayName";
const string ImageDir = @"/path/to/FaceList/images";
FaceServiceClient.CreateLargeFaceListAsync(LargeFaceListId, LargeFaceListName).Wait();

// Add Faces to the LargeFaceList.
Parallel.ForEach(
    Directory.GetFiles(ImageDir, "*.jpg"),
    async imagePath =>
        {
            using (Stream stream = File.OpenRead(imagePath))
            {
                await FaceServiceClient.AddFaceToLargeFaceListAsync(LargeFaceListId, stream);
            }
        });

// Train() is newly added operation for LargeFaceList.
// Must call it before FindSimilarAsync() to ensure the newly added faces searchable.
await TrainLargeFaceList(LargeFaceListId);

// Perform FindSimilar.
const string QueryImagePath = @"/path/to/query/image";
var results = new List<SimilarPersistedFace[]>();
using (Stream stream = File.OpenRead(QueryImagePath))
{
    var faces = FaceServiceClient.DetectAsync(stream).Result;
    foreach (var face in faces)
    {
        results.Add(await FaceServiceClient.FindSimilarAsync(face.FaceId, largeFaceListId: LargeFaceListId));
    }
}
```

Yukarıda gösterildiği gibi veri yönetim ve FindSimilar bölümü neredeyse aynıdır.
Yeni bir ön-işleme tren işlemi FindSimilar çalışır önce LargeFaceList tamamlamalısınız tek özel durumdur.

## <a name="train"></a>3. adım: Tren önerileri

Tren işlemi hızlandırır rağmen [FindSimilar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) ve [kimlik](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239), özellikle büyük ölçekli çıkarken eğitim süresini düşebilir.
Farklı ölçekler tahmini eğitim zamanında aşağıdaki tabloda listelenmiştir:

| Ölçek (yüzeyleri veya kişi) | Zaman eğitim tahmini |
|:---:|:---:|
| 1000 | 1-2 s |
| 10,000 | 5-10 s |
| 100,000 | 1 - 2 min |
| 1.000.000 | 10 - 30 dakika |

Daha iyi büyük ölçekli özelliğini kullanmak için bazı stratejiler dikkate için önerilir.

## <a name="interval"></a>3.1. adım: Zaman aralığı özelleştirme

De gösterildiği gibi `TrainLargeFaceList()`, var olan bir `timeIntervalInMilliseconds` işlemi denetleniyor sonsuz eğitim durumu gecikme.
Daha fazla yazıtipleri ile LargeFaceList için daha büyük bir zaman aralığına kullanarak çağrı sayısı ve maliyeti azaltır.
Zaman aralığını LargeFaceList beklenen kapasite göre özelleştirilmiş.

Aynı stratejisi LargePersonGroup için de geçerlidir.
Örneğin, bir LargePersonGroup 1.000.000 kişi ile eğitimindeki `timeIntervalInMilliseconds` 60.000 (paketini olabilir 1 dakika arayla).

## <a name="buffer"></a>Adım 3.2 küçük ölçekli arabellek

Kişi/yüzeyleri LargePersonGroup/LargeFaceList içinde yalnızca eğitilmiş sonra aranabilir.
Dinamik bir senaryoda, yeni kişiler/yüzeyleri sürekli olarak eklenir ve hemen aranabilir olması gerekir, ancak eğitim istenen daha uzun sürebilir.
Bu sorunu çözmek için ek çok küçük ölçekli bir LargePersonGroup/LargeFaceList yalnızca yeni eklenen girişlerinde arabellek olarak kullanabilirsiniz.
Bu arabellek çok küçük boyutu nedeniyle eğitmek için daha kısa sürer ve bu geçici arabellek hemen Search'te çalışması gerekir.
Orta gece ve günlük daha seyrek aralıkta, örneğin, ana eğitim yürüterek ana LargePersonGroup/LargeFaceList üzerinde eğitim ile birlikte bu arabellek kullanın.

Bir örnek iş akışı:
1. Bir ana LargePersonGroup/LargeFaceList (asıl toplama) ve bir arabellek LargePersonGroup/LargeFaceList (arabellek toplama) oluşturun. Yalnızca yeni eklenen kişiler/yüzeyleri için arabellek koleksiyonudur.
1. Yeni kişiler/yüzeyleri hem ana koleksiyonu ve arabellek koleksiyonu ekleyin.
1. Yalnızca yeni eklenen girişleri etkin duruma emin olmak için kısa bir süre aralığı arabellek koleksiyonuyla eğitmek.
1. Ana koleksiyonu ve arabellek koleksiyonu karşı kimlik/FindSimilar çağırın ve sonuçları birleştirir.
1. Bir eşiğe veya bir sistem boşta kalma zaman arabellek koleksiyon boyutunu artırır, yeni bir arabellek koleksiyonu oluşturun ve ana koleksiyonunda tren tetikler.
1. Eski ana koleksiyonunda eğitim arabellek koleksiyonu bitiş sonra silin.

## <a name="standalone"></a>3.3 adım tek başına eğitim

Görece uzun bir gecikme kabul edilebilir ise yeni verileri ekledikten sonra sağ tren işlemi tetiklemek gerekli değildir.
Bunun yerine, tren işlemi ana mantığından bölme ve değiştirebilirsiniz düzenli olarak tetiklenir.
Bu strateji, kabul edilebilir gecikme süresi ile dinamik senaryoları için uygundur ve daha da tren sıklığını azaltmak için statik senaryoları için uygulanabilir.

Var olduğunu varsayın bir `TrainLargePersonGroup` işlevi benzer `TrainLargeFaceList`.
Tipik bir uygulama, tek başına çağırarak üzerinde LargePersonGroup eğitim [ `Timer` ](https://msdn.microsoft.com/library/system.timers.timer(v=vs.110).aspx) sınıfını `System.Timers` olacaktır:

```CSharp
private static void Main()
{
    // Create a LargePersonGroup.
    const string LargePersonGroupId = "mylargepersongroupid_001";
    const string LargePersonGroupName = "MyLargePersonGroupDisplayName";
    FaceServiceClient.CreateLargePersonGroupAsync(LargePersonGroupId, LargePersonGroupName).Wait();

    // Setup a standalone training at regular intervals.
    const int TimeIntervalForStatus = 1000 * 60; // 1 minute interval for getting training status.
    const double TimeIntervalForTrain = 1000 * 60 * 60; // 1 hour interval for training.
    var trainTimer = new Timer(TimeIntervalForTrain);
    trainTimer.Elapsed += (sender, args) => TrainTimerOnElapsed(LargePersonGroupId, TimeIntervalForStatus);
    trainTimer.AutoReset = true;
    trainTimer.Enabled = true;

    // Other operations like creating persons, adding faces and Identification except for Train.
    // ...
}

private static void TrainTimerOnElapsed(string largePersonGroupId, int timeIntervalInMilliseconds)
{
    TrainLargePersonGroup(largePersonGroupId, timeIntervalInMilliseconds).Wait();
}
```

Veri Yönetimi ve kimliği ile ilgili uygulamaları hakkında daha fazla bilgi [eklemek bakarken nasıl](how-to-add-faces.md) ve [tanımlamak bakarken görüntüsündeki nasıl](HowtoIdentifyFacesinImage.md).

## <a name="summary"></a> Özet

Bu kılavuzda, var olan PersonGroup/FaceList kodu (verileri değil) LargePersonGroup/LargeFaceList geçirmek nasıl öğrendiniz:

- LargePersonGroup ve LargeFaceList çalışır PersonGroup/FaceList, benzer tren işlemi tarafından LargeFaceList gereken dışında.
- Büyük ölçekli veri kümesi için dinamik veri güncelleştirmesi için uygun tren stratejisi alın.

## <a name="related"></a> İlgili Konular

- [Görüntüde yüzeyleri belirleme](HowtoIdentifyFacesinImage.md)
- [Yazıtipleri ekleme](how-to-add-faces.md)
