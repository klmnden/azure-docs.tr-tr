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
ms.date: 05/01/2019
ms.author: sbowles
ms.openlocfilehash: 35ab2d36a5d6c9977398fdbc16ba22eb1d9656a4
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65229841"
---
# <a name="example-how-to-use-the-large-scale-feature"></a>Örnek: Büyük ölçekli özelliğinin nasıl kullanılacağı

Bu kılavuzda varolandan ölçeği artırma konusunda Gelişmiş bir makaledir **PersonGroup** ve **FaceList** için **LargePersonGroup** ve **LargeFaceList**sırasıyla. Bu kılavuz, geçiş işlemini gösterir ve temel olarak bilindiğini varsayar **PersonGroup**, **FaceList**, [eğitme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ae2d16ac60f11b48b5aa4) işlemi yanı sıra, yüz tanıma İşlevler. Bkz: [yüz tanıma](../concepts/face-recognition.md) bunlar hakkında daha fazla bilgi edinmek için kavramsal Kılavuzu.

LargePersonGroup ve LargeFaceList topluca için büyük ölçekli işlem olarak adlandırılır. LargePersonGroup 248 yüz, en fazla her 1.000.000 kişi içerebilir ve en fazla 1.000.000 yüzleri LargeFaceList içerebilir. Büyük ölçekli işlemler için geleneksel PersonGroup ve FaceList benzerdir, ancak yeni mimarisi nedeniyle bazı önemli farklılıklar vardır. 

Örnekler, Yüz Tanıma API’si istemci kitaplığı kullanılarak C# dilinde yazılır.

> [!NOTE]
> Büyük ölçekte Tanımlama ve FindSimilar için Yüz arama performansını etkinleştirmek için, LargeFaceList ve LargePersonGroup’u önceden işlemek için bir Eğitim işlemi sunmanız gerekir. Eğitim süresi, gerçek kapasiteye bağlı olarak, saniyeler ile yarım saat arasında değişiklik gösterir. Eğitim dönemi boyunca, daha önce başarılı bir eğitim yapıldıysa, yine de Tanımlama ve FindSimilar işlemleri gerçekleştirilebilir. Ancak dezavantajı, büyük ölçekli eğitime yeni geçiş sonrası tamamlanıncaya kadar yeni eklenen kişilerin/yüzlerin sonuçta görüntülenmemesidir.

## <a name="step-1-initialize-the-client-object"></a>1. Adım: İstemci nesnesini başlatır

Yüz Tanıma API’si istemci kitaplığı kullanılırken abonelik anahtarı ve abonelik uç noktası, FaceServiceClient sınıfının oluşturucusu aracılığıyla geçirilir. Örneğin:

```CSharp
string SubscriptionKey = "<Subscription Key>";
// Use your own subscription endpoint corresponding to the subscription key.
string SubscriptionRegion = "https://westcentralus.api.cognitive.microsoft.com/face/v1.0/";
FaceServiceClient FaceServiceClient = new FaceServiceClient(SubscriptionKey, SubscriptionRegion);
```

İlgili uç nokta ile birlikte abonelik anahtarı, Azure portalınızın Market sayfasından elde edilebilir.
Bkz. [Abonelikler](https://azure.microsoft.com/services/cognitive-services/directory/vision/).

## <a name="step-2-code-migration"></a>2. Adım: Kod geçişi

Bu bölümde yalnızca PersonGroup/FaceList uygulamasının LargePersonGroup/LargeFaceList uygulamasına geçişine odaklanılmaktadır. Tasarım ve iç uygulama LargePersonGroup/LargeFaceList PersonGroup/FaceList farklı olsa da, geriye dönük uyumluluk için API Arabirimi benzerdir.

Veri geçişi desteklenmez, bunun yerine LargePersonGroup/LargeFaceList’i yeniden oluşturmanız gerekir.

### <a name="migrate-persongroup-to-largepersongroup"></a>İçin LargePersonGroup PersonGroup geçirme

Bunlar tam olarak aynı grup düzeyinde işlemlerine paylaştıkça LargePersonGroup PersonGroup geçiş basit bir işlemdir.

PersonGroup/Kişi ile ilgili uygulama için API yollarının veya SDK sınıfı/modülünün yalnızca LargePersonGroup ve LargePersonGroup Kişisine değiştirilmesi gereklidir.

Tüm yüzleri ve kişiler için yeni LargePersonGroup PersonGroup eklemeniz gerekir. Bkz: [ekleme yüzler nasıl](how-to-add-faces.md) başvuru.

### <a name="migrate-facelist-to-largefacelist"></a>İçin LargeFaceList FaceList geçirme

| FaceList API’leri | LargeFaceList API’leri |
|:---:|:---:|
| Oluştur | Oluştur |
| Sil | Sil |
| Al | Al |
| Liste | Liste |
| Güncelle | Güncelle |
| - | Eğit |
| - | Eğitim Durumunu Alma |

Yukarıdaki tabloda, FaceList ile LargeFaceList arasındaki liste düzeyinde işlemlerin karşılaştırması yer almaktadır. Gösterildiği gibi LargeFaceList, FaceList’e kıyasla yeni işlemler (Eğitim ve Eğitim Durumunu Al) ile birlikte gelir. FaceList için Eğitim gerekmese de, LargeFaceList’in eğitimi için [FindSimilar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) işlemi ön koşuldur. Aşağıdaki kod parçacığı, LargeFaceList eğitimini beklemek için kullanılan bir yardımcı işlevdir.

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

Daha önce yüz ve FindSimilar ekleme ile FaceList tipik bir kullanımı aşağıdaki gibi görünür:

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

LargeFaceList için geçiş yaparken, aşağıdaki duruma gelir:

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

Yukarıda gösterildiği gibi, veri yönetimi ve FindSimilar kısmı neredeyse aynıdır. Tek istisna, FindSimilar’ın çalışması için önce LargeFaceList’te yeni bir Eğitim ön işleminin tamamlanmasının gerekmesidir.

## <a name="step-3-train-suggestions"></a>3. adım: Train önerileri

Eğitim işlemi, [FindSimilar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) ve [Tanımlama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) işlemini hızlandırıyor olsa da, özellikle de büyük ölçek söz konusu olduğunda eğitim süresi uzar. Aşağıdaki tabloda, farklı ölçeklerde tahmini eğitim süresi listelenmektedir:

| Ölçek (yüzler veya kişiler) | Tahmini Eğitim Süresi |
|:---:|:---:|
| 1000 | 1-2 s |
| 10,000 | 5-10 s |
| 100,000 | 1 - 2 dak |
| 1.000.000 | 10 - 30 dak |

Büyük ölçekli özelliği daha iyi kullanmak için dikkate alınması için bazı stratejiler önerilir.

## <a name="step-31-customize-time-interval"></a>Adım 3.1: Zaman aralığı özelleştirme

`TrainLargeFaceList()` içinde gösterildiği gibi, sonsuz eğitim durumu denetleme işlemini geciktirmek için `timeIntervalInMilliseconds` vardır. Daha fazla yüz içeren LargeFaceList için, büyük bir aralık kullanıldığında çağrı sayıları ve maliyeti azaltılır. Zaman aralığı, beklenen LargeFaceList kapasitesine göre özelleştirilmelidir.

Strateji LargePersonGroup için de geçerlidir. Örneğin, bir LargePersonGroup 1.000.000 kişiler ile eğitimindeki `timeIntervalInMilliseconds` 60.000 (1 dakikalık aralık) olabilir.

## <a name="step-32-small-scale-buffer"></a>Adım 3.2: Küçük ölçekli arabellek

LargePersonGroup/LargeFaceList’teki Kişiler/Yüzler yalnızca eğitildikten sonra aranabilir. Dinamik bir senaryoda, yeni kişiler/yüzler sürekli olarak eklenir ve bunların hemen aranabilir olması gerekir; ancak eğitim istenenden uzun sürebilir. Bu sorunu azaltmak için yalnızca yeni eklenen girişler için arabellek olarak ekstra küçük ölçekli bir LargePersonGroup/LargeFaceList kullanabilirsiniz. Bu arabelleğin boyutu çok daha küçük olduğundan bu geçici arabellekte anında arama yapılabilmesi gerektiğinden bu arabelleğin eğitilmesi daha kısa sürer. Ana eğitimi daha seyrek aralıklarla (örneğin, gece yarısı) ve günlük olarak yürüterek ana LargePersonGroup/LargeFaceList’te eğitim ile birlikte bu arabelleği kullanın.

Örnek bir iş akışı:
1. Bir ana LargePersonGroup/LargeFaceList (ana koleksiyon) ve arabellek LargePersonGroup/LargeFaceList (arabellek koleksiyonu) oluşturun. Arabellek koleksiyonu yalnızca yeni eklenen Kişiler/Yüzler içindir.
1. Hem ana koleksiyona hem de arabellek koleksiyonuna yeni Kişiler/Yüzler ekleyin.
1. Yeni eklenen girişlerin geçerli olduğundan emin olmak için yalnızca kısa zaman aralığı ile arabellek koleksiyonunu eğitin.
1. Hem ana koleksiyona hem de arabellek koleksiyonuna karşı Tanımlama/FindSimilar çağrısı yapın ve sonuçları birleştirin.
1. Arabellek koleksiyonu boyutu bir eşiğe arttığında veya sistemin boşta kalma anında yeni bir arabellek koleksiyonu oluşturun ve ana koleksiyonda eğitimi tetikleyin.
1. Ana koleksiyonda eğitim bittikten sonra eski arabellek koleksiyonunu silin.

## <a name="step-33-standalone-training"></a>Adım 3.3 tek başına eğitim

Nispeten uzun bir gecikme süresi kabul edilebiliyorsa, yeni veriler eklendikten hemen sonra Eğitim işleminin tetiklenmesi gerekmez. Bunun yerine Eğitim işlemi, ana mantıktan ayrılabilir ve düzenli olarak tetiklenebilir. Bu strateji, kabul edilebilir gecikme süresi olan dinamik senaryolar için uygundur ve Eğitim sıklığını azaltmak için statik senaryolara uygulanabilir.

`TrainLargeFaceList` öğesine benzer bir `TrainLargePersonGroup` işlevi olduğunu varsayın. Tek başına tipik bir uygulaması üzerinde LargePersonGroup çağırarak eğitim [ `Timer` ](https://msdn.microsoft.com/library/system.timers.timer(v=vs.110).aspx) sınıfını `System.Timers` olacaktır:

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