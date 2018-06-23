---
title: Yazıtipleri yüz API ile ekleme | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Yüz API Bilişsel Hizmetleri'nde yüzeyleri görüntüleri eklemek için kullanın.
services: cognitive-services
author: SteveMSFT
manager: corncar
ms.service: cognitive-services
ms.component: face-api
ms.topic: article
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: 3306c13d6c3d231ddbda38cfcbc5419fcdbd30db
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351574"
---
# <a name="how-to-add-faces"></a>Yazıtipleri ekleme

Bu kılavuz, kişiler ve yüzler yoğun sayısı için bir PersonGroup eklemek için en iyi uygulama gösterir.
Aynı stratejisi FaceList ve LargePersonGroup için de geçerlidir.
Örnekler C# ' ta yüz API'si istemci kitaplığı kullanılarak yazılır.

## <a name="step1"></a> 1. adım: başlatma

Çeşitli değişkenler bildirilir ve yardımcı işlevini istekleri zamanlamak için uygulanır.

- `PersonCount` kişilerin toplam sayısıdır.
- `CallLimitPerSecond` Abonelik katmanı göre saniye başına en fazla çağrıdır.
- `_timeStampQueue` İstek zaman damgaları kaydetmek için bir sıra var.
- `await WaitCallLimitPerSecondAsync()` sonraki istek göndermek için geçerli tamamlanana kadar bekler.

```CSharp
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

## <a name="step2"></a> 2. adım: API çağrısı yetkilendirme

Bir istemci kitaplığı kullanırken, abonelik anahtarı içinde FaceServiceClient sınıf oluşturucu kullanılarak geçirilir. Örneğin:

```CSharp
FaceServiceClient faceServiceClient = new FaceServiceClient("<Subscription Key>");
```

Abonelik anahtarı Azure portal'ınızın Market sayfasından elde edilebilir. Bkz: [abonelikleri](https://www.microsoft.com/cognitive-services/en-us/sign-up).

## <a name="step3"></a> 3. adım: PersonGroup oluşturma

"MyPersonGroup" adlı bir PersonGroup kişilerin kaydetmek için oluşturulur.
İstek süresi sıraya alınan için olduğunu `_timeStampQueue` genel doğrulama sağlamak için.

```CSharp
const string personGroupId = "mypersongroupid";
const string personGroupName = "MyPersonGroup";
_timeStampQueue.Enqueue(DateTime.UtcNow);
await faceServiceClient.CreatePersonGroupAsync(personGroupId, personGroupName);
```

## <a name="step4"></a> 4. adım: PersonGroup kişilere oluşturma

Kişi eşzamanlı olarak oluşturulur ve `await WaitCallLimitPerSecondAsync()` çağrısı sınırını aşmamak için uygulanır.

```CSharp
CreatePersonResult[] persons = new CreatePersonResult[PersonCount];
Parallel.For(0, PersonCount, async i =>
{
    await WaitCallLimitPerSecondAsync();

    string personName = $"PersonName#{i}";
    persons[i] = await faceServiceClient.CreatePersonAsync(personGroupId, personName);
});
```

## <a name="step5"></a> 5. adım: yüzeyleri kişilere ekleme

Belirli kişi sıralı için farklı kişilerin yüzeyleri eşzamanlı olarak işlenir, eklenirken.
Yeniden `await WaitCallLimitPerSecondAsync()` isteği sıklığı sınırlaması kapsamında olduğundan emin olmak için çağrılır.

```CSharp
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

## <a name="summary"></a> Özet

Bu kılavuzda, kişiler ve yüzler yoğun sayısı ile bir PersonGroup oluşturma işleminin öğrendiniz. Birkaç anımsatıcıları:

- Bu strateji, FaceList ve LargePersonGroup için de geçerlidir.
- Farklı FaceLists veya LargePersonGroup kişilerin yüzeyleri ekleme/silme aynı anda işlenebilir.
- Belirli bir FaceList veya kişisel LargePersonGroup olarak aynı işlemleri ardışık olarak yapılması gerekir.
- Basitlik tutmak için bu kılavuzda olası özel durum işleme atlanır. Daha fazla sağlamlık artırmak istiyorsanız, uygun yeniden deneme ilkesi uygulanmalıdır.

Daha önce açıklandığı ve gösterildiği özelliklerinin hızlı anımsatıcısı şunlardır:

- PersonGroups kullanarak oluşturma [PersonGroup - oluşturmak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244) API
- Kullanarak kişileri oluşturma [PersonGroup kişi - oluşturmak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c) API
- Yazıtipleri kullanan kişilere ekleme [PersonGroup kişi - eklemek yüz](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) API

## <a name="related"></a> İlgili Konular
- [Görüntüde yüzeyleri belirleme](HowtoIdentifyFacesinImage.md)
- [Görüntüde nasıl algılanacağını yüzler](HowtoDetectFacesinImage.md)
- [Büyük ölçekli özelliğinin nasıl kullanılacağı](how-to-use-large-scale.md)
