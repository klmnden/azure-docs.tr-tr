---
title: "Örnek: Yüzleri bir PersonGroup - yüz tanıma API'si ekleme"
titleSuffix: Azure Cognitive Services
description: Görüntülere yüz eklemek için Yüz Tanıma API’sini kullanın.
services: cognitive-services
author: SteveMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: sample
ms.date: 04/10/2019
ms.author: sbowles
ms.openlocfilehash: 0415dcae08c188c1758150c4b8b0df4dee014ce6
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67448598"
---
# <a name="add-faces-to-a-persongroup"></a>Yüz için bir PersonGroup Ekle

Bu kılavuz, çok sayıda kişi ve yüz PersonGroup nesnesine eklemek gösterilmiştir. Strateji de LargePersonGroup FaceList ve LargeFaceList nesneler için geçerlidir. Bu örnekte yazılan C# Azure Bilişsel hizmetler yüz tanıma API'si .NET istemci kitaplığı kullanarak.

## <a name="step-1-initialization"></a>1\. adım: Başlatma

Aşağıdaki kod, birkaç değişken ve yüz zamanlamak için bir yardımcı işlevini eklemek uygulayan istekleri bildirir:

- `PersonCount`, toplam kişi sayısıdır.
- `CallLimitPerSecond`, abonelik katmanına göre saniyedeki maksimum çağrı sayısıdır.
- `_timeStampQueue`, istek zaman damgalarını kaydetmek için kullanılan bir Kuyruktur.
- `await WaitCallLimitPerSecondAsync()` sonraki isteği göndermek için geçerli olana kadar bekler.

```csharp
const int PersonCount = 10000;
const int CallLimitPerSecond = 10;
static Queue<DateTime> _timeStampQueue = new Queue<DateTime>(CallLimitPerSecond);

static async Task WaitCallLimitPerSecondAsync()
{
    Monitor.Enter(_timeStampQueue);
    try
    {
        if (_timeStampQueue.Count >= CallLimitPerSecond)
        {
            TimeSpan timeInterval = DateTime.UtcNow - _timeStampQueue.Peek();
            if (timeInterval < TimeSpan.FromSeconds(1))
            {
                await Task.Delay(TimeSpan.FromSeconds(1) - timeInterval);
            }
            _timeStampQueue.Dequeue();
        }
        _timeStampQueue.Enqueue(DateTime.UtcNow);
    }
    finally
    {
        Monitor.Exit(_timeStampQueue);
    }
}
```

## <a name="step-2-authorize-the-api-call"></a>2\. adım: API çağrısı Yetkilendir

Bir istemci kitaplığı kullandığınızda, abonelik anahtarınızı oluşturucusuna geçirmeniz gerekir **FaceClient** sınıfı. Örneğin:

```csharp
private readonly IFaceClient faceClient = new FaceClient(
    new ApiKeyServiceClientCredentials("<SubscriptionKey>"),
    new System.Net.Http.DelegatingHandler[] { });
```

Abonelik anahtarını almak için Azure portalından Azure Marketi'nde gidin. Daha fazla bilgi için [abonelikleri](https://www.microsoft.com/cognitive-services/sign-up).

## <a name="step-3-create-the-persongroup"></a>3\. adım: PersonGroup oluşturma

Kişileri kaydetmek için "MyPersonGroup" adlı bir PersonGroup oluşturulur.
Genel doğrulama sağlamak için istek süresi, `_timeStampQueue` hedefinde kuyruğa alınır.

```csharp
const string personGroupId = "mypersongroupid";
const string personGroupName = "MyPersonGroup";
_timeStampQueue.Enqueue(DateTime.UtcNow);
await faceClient.LargePersonGroup.CreateAsync(personGroupId, personGroupName);
```

## <a name="step-4-create-the-persons-for-the-persongroup"></a>4\. Adım: Kişiler için PersonGroup oluşturma

Kişi aynı anda oluşturulur ve `await WaitCallLimitPerSecondAsync()` çağrı sınırını aşmamak için de uygulanır.

```csharp
CreatePersonResult[] persons = new CreatePersonResult[PersonCount];
Parallel.For(0, PersonCount, async i =>
{
    await WaitCallLimitPerSecondAsync();

    string personName = $"PersonName#{i}";
    persons[i] = await faceClient.PersonGroupPerson.CreateAsync(personGroupId, personName);
});
```

## <a name="step-5-add-faces-to-the-persons"></a>5\. Adım: Yüzleri kişilere ekleyin

Farklı kişilere eklenen yüzeyleri aynı anda işlenir. İçin belirli bir kişi yüzleri sıralı olarak işlenir.
Yeniden `await WaitCallLimitPerSecondAsync()` isteği sıklığı sınırlama kapsamında olduğundan emin olmak için çağrılır.

```csharp
Parallel.For(0, PersonCount, async i =>
{
    Guid personId = persons[i].PersonId;
    string personImageDir = @"/path/to/person/i/images";

    foreach (string imagePath in Directory.GetFiles(personImageDir, "*.jpg"))
    {
        await WaitCallLimitPerSecondAsync();

        using (Stream stream = File.OpenRead(imagePath))
        {
            await faceClient.PersonGroupPerson.AddFaceFromStreamAsync(personGroupId, personId, stream);
        }
    }
});
```

## <a name="summary"></a>Özet

Bu kılavuzda, çok büyük bir kişi ve yüz sayısı ile bir PersonGroup oluşturma işlemini öğrendiniz. Bazı anımsatıcılar:

- Bu strateji de belirlenmiştir ve LargePersonGroups için geçerlidir.
- Ekleme veya silme farklı belirlenmiştir veya LargePersonGroups kişilerin yüzlerini eşzamanlı olarak işlenir.
- Ekleme veya silme yüzleri FaceList veya bir LargePersonGroup kişinin belirli bir sırayla gerçekleştirilir.
- Kolaylık olması için bu kılavuzda olası bir özel durumu işlemek nasıl atlanır. Daha fazla sağlamlığını artırmak istiyorsanız, uygun bir yeniden deneme ilkesi uygulayın.

Aşağıdaki özellikleri açıklandığı ve gösterildiği:

- Kişi kullanarak oluşturma [PersonGroup - oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244) API.
- Kişiler kullanarak oluşturma [PersonGroup kişi - oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c) API.
- Yüzleri kullanarak kişilere eklemek [PersonGroup kişi - yüz tanıma ekleme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) API.

## <a name="related-topics"></a>İlgili konular

- [Bir görüntüdeki yüzleri belirleme](HowtoIdentifyFacesinImage.md)
- [Görüntüdeki yüzleri algılayın](HowtoDetectFacesinImage.md)
- [Büyük ölçekli özelliğini kullanma](how-to-use-large-scale.md)
