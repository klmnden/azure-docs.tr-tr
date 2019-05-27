---
title: "Örnek: -Yüz tanıma API'si büyük ölçekli özelliğini kullanın"
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
ms.openlocfilehash: 5a4085f713d66859a464ab59b00d856921db8ec3
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66124474"
---
# <a name="example-use-the-large-scale-feature"></a>Örnek: Büyük ölçek özelliğini kullanma

Bu kılavuz, mevcut PersonGroup ve FaceList nesnelerden LargePersonGroup ve LargeFaceList nesnelere sırasıyla ölçeği konusunda Gelişmiş bir makaledir. Bu kılavuz, geçiş işlemi gösterilmektedir. PersonGroup ve FaceList nesneleri temel olarak bilindiğini varsayar [eğitme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ae2d16ac60f11b48b5aa4) işlemi yanı sıra, yüz tanıma işlevleri. Bu konu hakkında daha fazla bilgi edinmek için [yüz tanıma](../concepts/face-recognition.md) kavramsal Kılavuzu.

LargePersonGroup ve LargeFaceList topluca için büyük ölçekli işlem olarak adlandırılır. LargePersonGroup her 248 yüzleri en fazla 1 milyona kadar kişi içerebilir. LargeFaceList 1 milyona kadar yüzleri içerebilir. Büyük ölçekli işlem geleneksel PersonGroup ve FaceList benzerdir, ancak yeni mimarisi nedeniyle bazı farklılıkları vardır. 

Örnekleri yazılan C# Azure Bilişsel hizmetler yüz tanıma API'si istemci kitaplığı kullanarak.

> [!NOTE]
> Büyük ölçek tanımlama ve FindSimilar için yüz Arama performansını LargeFaceList ve LargePersonGroup önişle için Train işlemi dağıtır. Eğitim süresini saniyelerden gerçek kapasiteye bağlı yaklaşık yarım saat için farklılık gösterir. Eğitim dönemi boyunca işletim başarılı eğitim önce yapıldıysa tanımlama ve FindSimilar gerçekleştirmek mümkündür. Dezavantajı, büyük ölçekli eğitim için yeni bir gönderi Geçiş tamamlanana kadar yeni eklenen kişi ve yüz sonucunda görüntülenmediğini olmasıdır.

## <a name="step-1-initialize-the-client-object"></a>1. Adım: İstemci nesnesini başlatır

Yüz tanıma API'si istemci kitaplığı kullandığınızda, abonelik anahtarını ve abonelik uç noktası FaceServiceClient sınıf oluşturucu üzerinden geçirilir. Örneğin:

```CSharp
string SubscriptionKey = "<Subscription Key>";
// Use your own subscription endpoint corresponding to the subscription key.
string SubscriptionRegion = "https://westcentralus.api.cognitive.microsoft.com/face/v1.0/";
FaceServiceClient FaceServiceClient = new FaceServiceClient(SubscriptionKey, SubscriptionRegion);
```

Kendi karşılık gelen uç noktası ile aboneliği anahtarı almak için Azure portalından Azure Marketi'nde gidin.
Daha fazla bilgi için [abonelikleri](https://azure.microsoft.com/services/cognitive-services/directory/vision/).

## <a name="step-2-code-migration"></a>2. Adım: Kod geçişi

Bu bölümde LargePersonGroup veya LargeFaceList PersonGroup veya FaceList uygulama geçirme ele alınmaktadır. LargePersonGroup veya LargeFaceList PersonGroup veya FaceList tasarım ve iç uygulama farklı olsa da, geriye dönük uyumluluk için API Arabirimi benzerdir.

Veri geçişi desteklenmez. LargePersonGroup veya LargeFaceList bunun yerine yeniden oluşturun.

### <a name="migrate-a-persongroup-to-a-largepersongroup"></a>İçin bir LargePersonGroup bir PersonGroup geçirme

Bir LargePersonGroup bir PersonGroup geçiş, basit bir işlemdir. Tam olarak aynı grup düzeyinde işlemlerine paylaşırlar.

PersonGroup veya kişi ilgili uygulama için yalnızca API yolları veya SDK sınıfı/modülü LargePersonGroup ve LargePersonGroup kişi değiştirmek gereklidir.

Tüm yüzleri ve kişiler için yeni LargePersonGroup PersonGroup ekleyin. Daha fazla bilgi için [ekleme yüzleri](how-to-add-faces.md).

### <a name="migrate-a-facelist-to-a-largefacelist"></a>İçin bir LargeFaceList bir FaceList geçirme

| FaceList API’leri | LargeFaceList API’leri |
|:---:|:---:|
| Oluştur | Oluştur |
| Sil | Sil |
| Al | Al |
| Liste | Liste |
| Güncelle | Güncelle |
| - | Eğit |
| - | Eğitim Durumunu Alma |

Yukarıdaki tabloda, FaceList ile LargeFaceList arasındaki liste düzeyinde işlemlerin karşılaştırması yer almaktadır. Gösterildiği, LargeFaceList yeni işlemleriyle eğitme ve eğitim durumunu Al FaceList ile karşılaştırıldığında gelir. Bir önkoşulu olan LargeFaceList eğitim [FindSimilar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) işlemi. Eğitim için FaceList gerekli değildir. Aşağıdaki kod parçacığında, bir LargeFaceList eğitim için beklenecek bir yardımcı işlevdir:

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

Eklenen yüzlerin ve FindSimilar tipik bir kullanımı FaceList, daha önce aşağıdaki gibi görünüyordu:

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

LargeFaceList için geçiş sırasında aşağıdakiler olur:

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

Daha önce gösterildiği, veri yönetimi ve FindSimilar bölümü neredeyse aynıdır. Tek özel durum FindSimilar çalışır önce yeni bir ön işleme Train işlemi içinde LargeFaceList tamamlamalısınız ' dir.

## <a name="step-3-train-suggestions"></a>3. adım: Train önerileri

Train işlemi hızlandırır rağmen [FindSimilar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) ve [kimlik](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239), özellikle büyük ölçek Bekletmeden çıkarken eğitim süresini düşebilir. Aşağıdaki tabloda farklı ölçekler tahmini eğitim sürede listelenir.

| Yüzleri veya kişi için ölçek | Tahmini eğitim süresini |
|:---:|:---:|
| 1000 | 1-2 sn |
| 10,000 | 5-10 sn |
| 100,000 | 1-2 dk |
| 1.000.000 | 10-30 dakika |

Büyük ölçekli özellik daha iyi kullanmak için aşağıdaki stratejilerden öneririz.

### <a name="step-31-customize-time-interval"></a>Adım 3.1: Zaman aralığı özelleştirme

İçinde gösterildiği `TrainLargeFaceList()`, işlem denetimi sonsuz eğitim durumu geciktirmek için milisaniye cinsinden zaman aralığı yok. Daha fazla yüz içeren LargeFaceList için, büyük bir aralık kullanıldığında çağrı sayıları ve maliyeti azaltılır. Zaman aralığı LargeFaceList beklenen kapasitesine göre özelleştirin.

Strateji LargePersonGroup için de geçerlidir. Örneğin, ne zaman eğittiğiniz bir LargePersonGroup 1 milyon kişi ile `timeIntervalInMilliseconds` 60.000 1 dakikalık bir aralığı olan olabilir.

### <a name="step-32-small-scale-buffer"></a>Adım 3.2: Küçük ölçekli arabelleği

Kişi veya bir LargeFaceList ya da bir LargePersonGroup yüzeylerine yalnızca eğitimli sonra aranabilir. Dinamik bir senaryoda, yeni bir kişi veya yüz sürekli olarak eklenir ve hemen aranabilir olması gerekir, ancak eğitim istenen daha uzun sürebilir. 

Bu sorunu çözmek için sadece yeni eklenen girişleri için bir arabellek olarak fazladan küçük ölçekli bir LargePersonGroup veya LargeFaceList kullanın. Bu arabelleği, daha küçük boyutu nedeniyle eğitmek için daha kısa bir zaman alır. Bu geçici bir arabelleğe hemen arama özelliği çalışması gerekir. Ana eğitim sparser bir aralıkta çalıştırarak ana LargePersonGroup veya LargeFaceList eğitim ile birlikte bu arabellek kullanın. Gece ve günlük ortasında verilebilir.

Örnek bir iş akışı:

1. Bir ana LargePersonGroup veya LargeFaceList, ana koleksiyon olduğu oluşturun. Bir arabellek LargePersonGroup veya arabellek koleksiyonu olan LargeFaceList oluşturun. Arabellek koleksiyon, yalnızca yeni eklenen kişiler veya yüzeyleri için oluşturulur.
1. Yeni bir kişi veya yüz hem ana koleksiyonu ve arabellek koleksiyonuna ekleyin.
1. Yalnızca yeni eklenen girişlerin etkili olmasını sağlamak için bir kısa zaman aralığı arabellek koleksiyonuyla eğitin.
1. Kimlik veya FindSimilar ana koleksiyonu ve arabellek koleksiyon karşı çağırın. Sonuçları birleştirin.
1. Bir eşiğe veya sistem boşta kalma zaman arabellek koleksiyon boyutu artar, yeni bir arabellek koleksiyonu oluşturun. Ana koleksiyonu Train işlemi tetikler.
1. Ana koleksiyonunda Train işlemi tamamlandıktan sonra eski arabellek koleksiyonu silin.

### <a name="step-33-standalone-training"></a>Adım 3.3: Tek başına eğitim

Görece uzun bir gecikme süresi kabul edilebilir ise, yeni veri ekledikten sonra tetikleyici doğru Train işlemi için gerekli değildir. Bunun yerine Eğitim işlemi, ana mantıktan ayrılabilir ve düzenli olarak tetiklenebilir. Bu strateji, kabul edilebilir gecikme süresi ile dinamik senaryoları için uygundur. Daha fazla Train sıklığını azaltmak için statik senaryoları için uygulanabilir.

Orada olduğunu varsayın bir `TrainLargePersonGroup` işlevi benzer `TrainLargeFaceList`. Tek başına eğitimle çağırarak bir LargePersonGroup tipik bir uygulaması [ `Timer` ](https://msdn.microsoft.com/library/system.timers.timer(v=vs.110).aspx) sınıfını `System.Timers` olan:

```CSharp
private static void Main()
{
    // Create a LargePersonGroup.
    const string LargePersonGroupId = "mylargepersongroupid_001";
    const string LargePersonGroupName = "MyLargePersonGroupDisplayName";
    FaceServiceClient.CreateLargePersonGroupAsync(LargePersonGroupId, LargePersonGroupName).Wait();

    // Set up standalone training at regular intervals.
    const int TimeIntervalForStatus = 1000 * 60; // 1-minute interval for getting training status.
    const double TimeIntervalForTrain = 1000 * 60 * 60; // 1-hour interval for training.
    var trainTimer = new Timer(TimeIntervalForTrain);
    trainTimer.Elapsed += (sender, args) => TrainTimerOnElapsed(LargePersonGroupId, TimeIntervalForStatus);
    trainTimer.AutoReset = true;
    trainTimer.Enabled = true;

    // Other operations like creating persons, adding faces, and identification, except for Train.
    // ...
}

private static void TrainTimerOnElapsed(string largePersonGroupId, int timeIntervalInMilliseconds)
{
    TrainLargePersonGroup(largePersonGroupId, timeIntervalInMilliseconds).Wait();
}
```

Veri Yönetimi ve kimlik ile ilgili uygulamaları hakkında daha fazla bilgi için bkz. [ekleme yüzleri](how-to-add-faces.md) ve [tanımlamak bir görüntüdeki yüzleri](HowtoIdentifyFacesinImage.md).

## <a name="summary"></a>Özet

Bu kılavuzda, verileri değil, mevcut PersonGroup veya FaceList kod LargePersonGroup veya LargeFaceList geçirmenizi nasıl yapılacağını öğrendiniz:

- Train işlemi tarafından LargeFaceList gereklidir dışında LargePersonGroup ve LargeFaceList PersonGroup veya FaceList, benzer çalışır.
- Büyük ölçekli veri kümeleri için dinamik veri güncelleştirme uygun eğitimi stratejisi taşıyın.

## <a name="next-steps"></a>Sonraki adımlar

Yüz için bir PersonGroup eklemek veya bir PersonGroup belirleme işlemi yürütmek hakkında bilgi edinmek için bir nasıl yapılır Kılavuzu izleyin.

- [Yüzleri Ekle](how-to-add-faces.md)
- [Bir görüntüdeki yüzleri belirleme](HowtoIdentifyFacesinImage.md)