---
title: "Örnek: Yüz tanıma API'si - büyük ölçekli özelliğini kullanın"
titleSuffix: Azure Cognitive Services
description: Yüz Tanıma API’sinde büyük ölçek özelliğini kullanın.
services: cognitive-services
author: SteveMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: sample
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: 52631d0b25527d204baa11a90401b60e437137a0
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64691035"
---
# <a name="example-how-to-use-the-large-scale-feature"></a>Örnek: Büyük ölçekli özelliğinin nasıl kullanılacağı

Bu kılavuz, sırasıyla mevcut PersonGroup ve FaceList’ten LargePersonGroup ve LargeFaceList’e ölçeği genişletmek için kod geçişiyle ilgili ileri düzey bir makaledir.
Bu kılavuzda, PersonGroup ve FaceList’in temel kullanımının bilindiği varsayımıyla geçiş işlemi gösterilmektedir.
Temel işlemler hakkında fikir sahip olmak için lütfen [Görüntülerdeki yüzleri belirleme](HowtoIdentifyFacesinImage.md) gibi diğer öğreticilere bakın.

Yüz Tanıma API’si yakın zamanda topluca Büyük ölçekli işlemler olarak ifade edilen büyük ölçekli senaryoları etkinleştirmek için iki özellik yayınladı: LargePersonGroup ve LargeFaceList.
LargePersonGroup, her biri maksimum 248 yüze sahip en fazla 1.000.000 kişi içerebilir ve LargeFaceList ise 1.000.000’a kadar yüzü barındırabilir.

Büyük ölçekli işlemler, geleneksel PersonGroup ve FaceList’e benzer, ancak yeni mimari nedeniyle bazı dikkat çeken farklara sahiptir.
Bu kılavuzda, PersonGroup ve FaceList’in temel kullanımının bilindiği varsayımıyla geçiş işlemi gösterilmektedir.
Örnekler, Yüz Tanıma API’si istemci kitaplığı kullanılarak C# dilinde yazılır.

Büyük ölçekte Tanımlama ve FindSimilar için Yüz arama performansını etkinleştirmek için, LargeFaceList ve LargePersonGroup’u önceden işlemek için bir Eğitim işlemi sunmanız gerekir.
Eğitim süresi, gerçek kapasiteye bağlı olarak, saniyeler ile yarım saat arasında değişiklik gösterir.
Eğitim dönemi boyunca, daha önce başarılı bir eğitim yapıldıysa, yine de Tanımlama ve FindSimilar işlemleri gerçekleştirilebilir.
Ancak dezavantajı, büyük ölçekli eğitime yeni geçiş sonrası tamamlanıncaya kadar yeni eklenen kişilerin/yüzlerin sonuçta görüntülenmemesidir.

## <a name="concepts"></a>Kavramlar

İleriye dönük önce aşağıdaki kavramlarını tanımanız:

- LargePersonGroup: 1.000.000 kadar kapasiteye sahip kişiler koleksiyonu.
- LargeFaceList: Bir yüz koleksiyonu 1.000.000 kadar kapasiteye sahip.
- Eğitimi: Kimliği/FindSimilar performansı elde etmek için bir ön işleme.
- Kimliği: Bir veya daha fazla PersonGroup veya LargePersonGroup yüzleri belirleyin.
- FindSimilar: FaceList veya LargeFaceList benzer yüzleri arayın.

## <a name="step-1-authorize-the-api-call"></a>1. Adım: API çağrısı Yetkilendir

Yüz Tanıma API’si istemci kitaplığı kullanılırken abonelik anahtarı ve abonelik uç noktası, FaceServiceClient sınıfının oluşturucusu aracılığıyla geçirilir. Örneğin:

```CSharp
string SubscriptionKey = "<Subscription Key>";
// Use your own subscription endpoint corresponding to the subscription key.
string SubscriptionRegion = "https://westcentralus.api.cognitive.microsoft.com/face/v1.0/";
FaceServiceClient FaceServiceClient = new FaceServiceClient(SubscriptionKey, SubscriptionRegion);
```

İlgili uç nokta ile birlikte abonelik anahtarı, Azure portalınızın Market sayfasından elde edilebilir.
Bkz. [Abonelikler](https://azure.microsoft.com/services/cognitive-services/directory/vision/).

## <a name="step-2-code-migration-in-action"></a>2. Adım: Uygulamada kod geçişi

Bu bölümde yalnızca PersonGroup/FaceList uygulamasının LargePersonGroup/LargeFaceList uygulamasına geçişine odaklanılmaktadır.
LargePersonGroup/LargeFaceList, tasarım ve iç uygulama açısından PersonGroup/FaceList’ten farklılık gösterse de API arabirimleri, geriye dönük uyumluluk açısından benzerdir.

Veri geçişi desteklenmez, bunun yerine LargePersonGroup/LargeFaceList’i yeniden oluşturmanız gerekir.

## <a name="step-21-migrate-persongroup-to-largepersongroup"></a>2.1. adım: İçin LargePersonGroup PersonGroup geçirme

PersonGroup ve LargePersonGroup tamamen aynı grup düzeyinde işlemleri paylaştığından PersonGroup’tan LargePersonGroup’a geçiş sorunsuzdur.

PersonGroup/Kişi ile ilgili uygulama için API yollarının veya SDK sınıfı/modülünün yalnızca LargePersonGroup ve LargePersonGroup Kişisine değiştirilmesi gereklidir.

Veri geçişi ile ilgili olarak başvuru için bkz. [Yüz Ekleme](how-to-add-faces.md).

## <a name="step-22-migrate-facelist-to-largefacelist"></a>2.2. adım: İçin LargeFaceList FaceList geçirme

| FaceList API’leri | LargeFaceList API’leri |
|:---:|:---:|
| Oluştur | Oluştur |
| Sil | Sil |
| Al | Al |
| Liste | Liste |
| Güncelleştirme | Güncelleştirme |
| - | Eğitim |
| - | Eğitim Durumunu Alma |

Yukarıdaki tabloda, FaceList ile LargeFaceList arasındaki liste düzeyinde işlemlerin karşılaştırması yer almaktadır.
Gösterildiği gibi LargeFaceList, FaceList’e kıyasla yeni işlemler (Eğitim ve Eğitim Durumunu Al) ile birlikte gelir.
FaceList için Eğitim gerekmese de, LargeFaceList’in eğitimi için [FindSimilar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) işlemi ön koşuldur.
Aşağıdaki kod parçacığı, LargeFaceList eğitimini beklemek için kullanılan bir yardımcı işlevdir.

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

Daha önce, FindSimilar ve yüz ekleme ile ilgili tipik FaceList kullanımı aşağıdaki gibiydi:

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

LargeFaceList’e geçiş yapıldığında aşağıdaki gibi olmalıdır:

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

Yukarıda gösterildiği gibi, veri yönetimi ve FindSimilar kısmı neredeyse aynıdır.
Tek istisna, FindSimilar’ın çalışması için önce LargeFaceList’te yeni bir Eğitim ön işleminin tamamlanmasının gerekmesidir.

## <a name="step-3-train-suggestions"></a>3. Adım: Train önerileri

Eğitim işlemi, [FindSimilar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) ve [Tanımlama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) işlemini hızlandırıyor olsa da, özellikle de büyük ölçek söz konusu olduğunda eğitim süresi uzar.
Aşağıdaki tabloda, farklı ölçeklerde tahmini eğitim süresi listelenmektedir:

| Ölçek (yüzler veya kişiler) | Tahmini Eğitim Süresi |
|:---:|:---:|
| 1000 | 1-2 s |
| 10,000 | 5-10 s |
| 100.000 | 1 - 2 dak |
| 1.000.000 | 10 - 30 dak |

Büyük ölçekli özelliği daha iyi kullanmak için dikkate alınması için bazı stratejiler önerilir.

## <a name="step-31-customize-time-interval"></a>Adım 3.1: Zaman aralığı özelleştirme

`TrainLargeFaceList()` içinde gösterildiği gibi, sonsuz eğitim durumu denetleme işlemini geciktirmek için `timeIntervalInMilliseconds` vardır.
Daha fazla yüz içeren LargeFaceList için, büyük bir aralık kullanıldığında çağrı sayıları ve maliyeti azaltılır.
Zaman aralığı, beklenen LargeFaceList kapasitesine göre özelleştirilmelidir.

Aynı strateji, LargePersonGroup için de geçerlidir.
Örneğin, bir LargePersonGroup 1.000.000 kişiler ile eğitimindeki `timeIntervalInMilliseconds` 60.000 (1 dakikalık aralık) olabilir.

## <a name="step-32-small-scale-buffer"></a>Adım 3.2: Küçük ölçekli arabellek

LargePersonGroup/LargeFaceList’teki Kişiler/Yüzler yalnızca eğitildikten sonra aranabilir.
Dinamik bir senaryoda, yeni kişiler/yüzler sürekli olarak eklenir ve bunların hemen aranabilir olması gerekir; ancak eğitim istenenden uzun sürebilir.
Bu sorunu azaltmak için yalnızca yeni eklenen girişler için arabellek olarak ekstra küçük ölçekli bir LargePersonGroup/LargeFaceList kullanabilirsiniz.
Bu arabelleğin boyutu çok daha küçük olduğundan bu geçici arabellekte anında arama yapılabilmesi gerektiğinden bu arabelleğin eğitilmesi daha kısa sürer.
Ana eğitimi daha seyrek aralıklarla (örneğin, gece yarısı) ve günlük olarak yürüterek ana LargePersonGroup/LargeFaceList’te eğitim ile birlikte bu arabelleği kullanın.

Örnek bir iş akışı:
1. Bir ana LargePersonGroup/LargeFaceList (ana koleksiyon) ve arabellek LargePersonGroup/LargeFaceList (arabellek koleksiyonu) oluşturun. Arabellek koleksiyonu yalnızca yeni eklenen Kişiler/Yüzler içindir.
1. Hem ana koleksiyona hem de arabellek koleksiyonuna yeni Kişiler/Yüzler ekleyin.
1. Yeni eklenen girişlerin geçerli olduğundan emin olmak için yalnızca kısa zaman aralığı ile arabellek koleksiyonunu eğitin.
1. Hem ana koleksiyona hem de arabellek koleksiyonuna karşı Tanımlama/FindSimilar çağrısı yapın ve sonuçları birleştirin.
1. Arabellek koleksiyonu boyutu bir eşiğe arttığında veya sistemin boşta kalma anında yeni bir arabellek koleksiyonu oluşturun ve ana koleksiyonda eğitimi tetikleyin.
1. Ana koleksiyonda eğitim bittikten sonra eski arabellek koleksiyonunu silin.

## <a name="step-33-standalone-training"></a>Adım 3.3 tek başına eğitim

Nispeten uzun bir gecikme süresi kabul edilebiliyorsa, yeni veriler eklendikten hemen sonra Eğitim işleminin tetiklenmesi gerekmez.
Bunun yerine Eğitim işlemi, ana mantıktan ayrılabilir ve düzenli olarak tetiklenebilir.
Bu strateji, kabul edilebilir gecikme süresi olan dinamik senaryolar için uygundur ve Eğitim sıklığını azaltmak için statik senaryolara uygulanabilir.

`TrainLargeFaceList` öğesine benzer bir `TrainLargePersonGroup` işlevi olduğunu varsayın.
[`Timer`](https://msdn.microsoft.com/library/system.timers.timer(v=vs.110).aspx) sınıfı
`System.Timers` içinde çağrılarak LargePersonGroup üzerinde bağımsız Eğitimin tipik bir uygulaması aşağıdaki gibidir:

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

Veri yönetimi ve tanımlama ile ilgili uygulamalar hakkında daha fazla bilgi için bkz. [Yüz Ekleme](how-to-add-faces.md) ve [Görüntüdeki Yüzleri Belirleme](HowtoIdentifyFacesinImage.md).

## <a name="summary"></a>Özet

Bu kılavuzda, mevcut PersonGroup/FaceList kodunun (veri değil) LargePersonGroup/LargeFaceList’e nasıl geçirileceğini öğrendiniz:

- LargePersonGroup ve LargeFaceList, PersonGroup/FaceList’e benzer şekilde çalışır; tek istisna, LargeFaceList tarafından Eğitim işleminin gerekmesidir.
- Büyük ölçekli veri kümesi için dinamik veri güncelleştirmesine yönelik uygun eğitim stratejisini uygulayın.

## <a name="next-steps"></a>Sonraki adımlar

Yüz için bir PersonGroup eklemek veya bir PersonGroup belirleme işlemi yürütmek hakkında bilgi edinmek için bir nasıl yapılır Kılavuzu izleyin.

- [Yüz Ekleme](how-to-add-faces.md)
- [Görüntüdeki Yüzleri Belirleme](HowtoIdentifyFacesinImage.md)