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
ms.openlocfilehash: 04fe9251ba124ed5d218daf915339c7f84efdeb6
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64704174"
---
# <a name="how-to-add-faces-to-a-persongroup"></a>İçin bir PersonGroup yüzleri ekleme

Bu kılavuz, çok sayıda kişi ve yüz PersonGroup nesnesine eklemek için en iyi uygulamaları gösterir. Strateji LargePersonGroup FaceList ve LargeFaceList için de geçerlidir. Bu örnekte yazılan C# yüz API .NET istemci kitaplığını kullanarak.

## <a name="step-1-initialization"></a>1. Adım: Başlatma

Aşağıdaki kod, birkaç değişken ve yüz zamanlamak için bir yardımcı işlevini eklemek uygulayan istekleri bildirir.

- `PersonCount`, toplam kişi sayısıdır.
- `CallLimitPerSecond`, abonelik katmanına göre saniyedeki maksimum çağrı sayısıdır.
- `_timeStampQueue`, istek zaman damgalarını kaydetmek için kullanılan bir Kuyruktur.
- `await WaitCallLimitPerSecondAsync()`, sonraki isteği göndermenin geçerli olmasını bekler.

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

## <a name="step-2-authorize-the-api-call"></a>2. Adım: API çağrısı Yetkilendir

Bir istemci Kitaplığı kullanıldığında FaceServiceClient sınıfının oluşturucusu için abonelik anahtarınızı geçmesi gerekir. Örneğin:

```csharp
FaceServiceClient faceServiceClient = new FaceServiceClient("<Subscription Key>");
```

Abonelik anahtarı, Azure portalınızın Market sayfasından elde edilebilir. Bkz. [Abonelikler](https://www.microsoft.com/cognitive-services/sign-up).

## <a name="step-3-create-the-persongroup"></a>3. Adım: PersonGroup oluşturma

Kişileri kaydetmek için "MyPersonGroup" adlı bir PersonGroup oluşturulur.
Genel doğrulama sağlamak için istek süresi, `_timeStampQueue` hedefinde kuyruğa alınır.

```csharp
const string personGroupId = "mypersongroupid";
const string personGroupName = "MyPersonGroup";
_timeStampQueue.Enqueue(DateTime.UtcNow);
await faceServiceClient.CreatePersonGroupAsync(personGroupId, personGroupName);
```

## <a name="step-4-create-the-persons-to-the-persongroup"></a>4. Adım: PersonGroup kişilere oluşturma

Çağrı sınırının aşılmasını önlemek için kişiler eş zamanlı olarak oluşturulur ve `await WaitCallLimitPerSecondAsync()` uygulanır.

```csharp
CreatePersonResult[] persons = new CreatePersonResult[PersonCount];
Parallel.For(0, PersonCount, async i =>
{
    await WaitCallLimitPerSecondAsync();

    string personName = $"PersonName#{i}";
    persons[i] = await faceServiceClient.CreatePersonAsync(personGroupId, personName);
});
```

## <a name="step-5-add-faces-to-the-persons"></a>5. Adım: Yüzleri kişilere ekleyin

Farklı kişilere farklı yüzler ekleme işlemi eş zamanlı olarak işlenirken, tek bir kişi için bu işlem sıralı olarak gerçekleştirilir.
İstek sıklığının sınırlama kapsamında olduğundan emin olmak için tekrar `await WaitCallLimitPerSecondAsync()` çağrılır.

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
            await faceServiceClient.AddPersonFaceAsync(personGroupId, personId, stream);
        }
    }
});
```

## <a name="summary"></a>Özet

Bu kılavuzda, çok sayıda kişi ve yüz içeren bir PersonGroup oluşturma işlemini öğrendiniz. Bazı anımsatıcılar:

- Aynı strateji, FaceList ve LargePersonGroup için de geçerlidir.
- LargePersonGroup içinde farklı FaceList veya Kişiler için yüz Ekleme/Silme işlemi eş zamanlı olarak işlenebilir.
- LargePersonGroup içinde belirli bir FaceList veya Kişi için aynı işlem sıralı olarak gerçekleştirilmelidir.
- Kolaylık sağlaması için bu kılavuzda olası özel durumlar ele alınmamıştır. Sağlamlığı daha fazla geliştirmek istiyorsanız uygun yeniden deneme ilkesi uygulanmalıdır.

Aşağıda, önceden açıklanmış ve gösterilmiş olan özelliklerin hızlı bir anımsatıcısı yer almaktadır:

- [PersonGroup - Oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244) API’sini kullanarak PersonGroup oluşturma
- [PersonGroup Kişisi - Oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c) API’sini kullanarak kişiler oluşturma
- [PersonGroup Kişisi - Yüz Ekleme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) API’sini kullanarak kişilere yüz ekleme

## <a name="related-topics"></a>İlgili Konular

- [Görüntüdeki Yüzleri Belirleme](HowtoIdentifyFacesinImage.md)
- [Görüntüdeki Yüzleri Algılama](HowtoDetectFacesinImage.md)
- [Büyük ölçek özelliğini kullanma](how-to-use-large-scale.md)
