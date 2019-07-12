---
title: "Örnek: Resimlerdeki - yüz tanıma API'si yüzleri belirleyin"
titleSuffix: Azure Cognitive Services
description: Görüntülerdeki yüzleri belirlemek için Yüz Tanıma API’sini kullanın.
services: cognitive-services
author: SteveMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: sample
ms.date: 04/10/2019
ms.author: sbowles
ms.openlocfilehash: 5806c17b0532f4d18b7ac57fbf70c92ed9d47daa
ms.sourcegitcommit: 441e59b8657a1eb1538c848b9b78c2e9e1b6cfd5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67827509"
---
# <a name="example-identify-faces-in-images"></a>Örnek: Resimlerdeki yüzleri belirleyin

Bu kılavuz, bilinen kişilerden önceden oluşturulmuş PersonGroup nesneleri kullanarak bilinmeyen yüzleri belirleyin gösterilmiştir. Örnekleri yazılan C# Azure Bilişsel hizmetler yüz tanıma API'si istemci kitaplığı kullanarak.

## <a name="preparation"></a>Hazırlık

Bu örnek gösterir:

- Bir PersonGroup oluşturma Bu PersonGroup bilinen kişi listesini içerir.
- Yüzleri her kişi için atama. Bu yüz temel olarak kişiler tanımlamak için kullanılır. Yüzleri tamamen temizleyin çıplak görünümlerini kullanmanızı öneririz. Örnek bir fotoğraf kimliğidir. Fotoğraf iyi bir dizi farklı yürütmelisiniz, giysi renkleri veya hairstyles aynı kişinin yüzlerini içerir.

Bu örnek gösterimini yapmak için hazırlayın:

- Kişinin yüzünü içeren birkaç fotoğraf. [Örnek fotoğraf Yükle](https://github.com/Microsoft/Cognitive-Face-Windows/tree/master/Data) Anna'nın, fatura ve Clare.
- Test fotoğraf bir dizi. Fotoğrafları olabilir ya da Anna'nın, fatura veya Clare yüzlerini içermeyebilir. Bunlar kimlik test etmek için kullanılır. Ayrıca bazı örnek görüntüleri yukarıdaki bağlantıyı seçin.

## <a name="step-1-authorize-the-api-call"></a>1\. adım: API çağrısı Yetkilendir

Yüz Tanıma API’sine yapılan her çağrı için bir abonelik anahtarı gerekir. Bu anahtarı ya da bir sorgu dizesi parametresi aracılığıyla geçirilmesi veya istek üst bilgisinde belirtilen. İstek URL'si abonelik anahtarını bir sorgu dizesi aracılığıyla geçirmek için bkz [yüz tanıma - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) örnek olarak:
```
https://westus.api.cognitive.microsoft.com/face/v1.0/detect[?returnFaceId][&returnFaceLandmarks][&returnFaceAttributes]
&subscription-key=<Subscription key>
```

Alternatif olarak, HTTP istek bağlığında bir abonelik anahtarı belirtin **ocp-apim-subscription-key: &lt;Abonelik anahtarı&gt;** .
Bir istemci kitaplığı kullandığınızda, abonelik anahtarını FaceClient sınıf oluşturucu üzerinden geçirilir. Örneğin:
 
```csharp 
private readonly IFaceClient faceClient = new FaceClient(
            new ApiKeyServiceClientCredentials("<subscription key>"),
            new System.Net.Http.DelegatingHandler[] { });
```
 
Abonelik anahtarını almak için Azure portalından Azure Marketi'nde gidin. Daha fazla bilgi için [abonelikleri](https://azure.microsoft.com/try/cognitive-services/).

## <a name="step-2-create-the-persongroup"></a>2\. adım: PersonGroup oluşturma

Bu adımda, Anna'nın, fatura ve Clare "MyFriends" adlı bir PersonGroup içerir. Her kişinin kayıtlı birkaç yüzü vardır. Yüzleri görüntülerden algılanabilmesi gerekir. Tüm bu adımlardan sonra, aşağıdaki görüntüye benzer bir PersonGroup öğeniz olur:

![MyFriends](../Images/group.image.1.jpg)

### <a name="step-21-define-people-for-the-persongroup"></a>2\.1. adım: PersonGroup kişileri tanımlayın
Kişi, temel bir tanımlama birimidir. Bir kişinin bir veya daha fazla bilinen yüzü kayıtlı olabilir. Bir PersonGroup İnsan topluluğudur. Herkes, belirli bir PersonGroup içinde tanımlanır. Kimliği bir PersonGroup karşı yapılır. Bir PersonGroup oluşturun ve ardından kişiler içinde Anna'nın, fatura ve Clare gibi oluşturmak için bir görevdir.

İlk olarak, yeni PersonGroup kullanarak oluşturma [PersonGroup - oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244) API. Karşılık gelen istemci kitaplığı API'sini FaceClient sınıfı CreatePersonGroupAsync yöntemidir. Grubu oluşturmak için belirtilen grup kimliği, her abonelik için benzersizdir. Ayrıca alma, güncelleştirme veya kişi diğer PersonGroup API'lerini kullanarak silebilirsiniz. 

Bir grubu tanımlandıktan sonra kullanarak içindeki kişilerin tanımlayabilirsiniz [PersonGroup kişi - oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c) API. İstemci kitaplığı yöntemi, CreatePersonAsync yöntemidir. Bunlar oluşturduktan sonra her bir kişi için yüz ekleyebilirsiniz.

```csharp 
// Create an empty PersonGroup
string personGroupId = "myfriends";
await faceClient.PersonGroup.CreateAsync(personGroupId, "My Friends");
 
// Define Anna
CreatePersonResult friend1 = await faceClient.PersonGroupPerson.CreateAsync(
    // Id of the PersonGroup that the person belonged to
    personGroupId,    
    // Name of the person
    "Anna"            
);
 
// Define Bill and Clare in the same way
```
### <a name="step2-2"></a> 2.2. adım: Yüz algılama ve bunları doğru kişiye kaydetme
HTTP istek gövdesinde görüntü dosyası ile [Yüz - Algılama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) API’sine bir “POST” web isteği gönderilerek algılama gerçekleştirilir. İstemci Kitaplığı kullandığınızda, yüz algılama FaceClient sınıfı için DetectAsync yöntemi aracılığıyla gerçekleştirilir.

Algılanan her yüz için çağrı [PersonGroup kişi – yüz ekleme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) ve doğru kişiye eklemek için.

Aşağıdaki kod, bir görüntüden yüzü algılama ve bu yüzü bir kişiye ekleme işlemini göstermektedir:

```csharp 
// Directory contains image files of Anna
const string friend1ImageDir = @"D:\Pictures\MyFriends\Anna\";
 
foreach (string imagePath in Directory.GetFiles(friend1ImageDir, "*.jpg"))
{
    using (Stream s = File.OpenRead(imagePath))
    {
        // Detect faces in the image and add to Anna
        await faceClient.PersonGroupPerson.AddFaceFromStreamAsync(
            personGroupId, friend1.PersonId, s);
    }
}
// Do the same for Bill and Clare
``` 
Görüntünün birden fazla yüz içeriyorsa, yalnızca en büyük yüz eklenir. Diğer yüzleri kişiye ekleyebilirsiniz. Dize biçiminde geçirin "targetFace sol, üst düzey, genişlik, yükseklik =" için [PersonGroup kişi - yüz tanıma ekleme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) API'nin targetFace sorgu parametresi. AddPersonFaceAsync yöntemi için targetFace isteğe bağlı parametresi, diğer yüzleri eklemek için de kullanabilirsiniz. Her yüz için kişi bir benzersiz kalıcı yüz kimliği verilir. Bu kimliği kullanabileceğiniz [PersonGroup kişi – silme yüz](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523e) ve [yüz – tanımlamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239).

## <a name="step-3-train-the-persongroup"></a>3\. adım: PersonGroup eğitin

Bunu kullanarak bir kimlik gerçekleştirilmeden önce PersonGroup eğitim gerekir. Herhangi bir kişi ekleyip sonra veya bir kişinin kayıtlı yüz düzenlerseniz PersonGroup eğitilebileceği gerekir. Eğitim, [PersonGroup – Eğitim](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249) API’si tarafından gerçekleştirilir. İstemci Kitaplığı kullandığınızda, bu TrainPersonGroupAsync yönteme bir çağrı değil:
 
```csharp 
await faceClient.PersonGroup.TrainAsync(personGroupId);
```
 
Eğitim zaman uyumsuz bir işlemdir. Hatta TrainPersonGroupAsync yöntemin dönüşünün ardından, tamamlanmış olabilir değil. Eğitim durum sorgu gerekebilir. Kullanım [PersonGroup - eğitim durumunu Al](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395247) istemci Kitaplığı'nın API veya GetPersonGroupTrainingStatusAsync yöntemi. Aşağıdaki kod, PersonGroup için bekleyen, basit bir mantıksal gösterir. son için eğitim alın:
 
```csharp 
TrainingStatus trainingStatus = null;
while(true)
{
    trainingStatus = await faceClient.PersonGroup.GetTrainingStatusAsync(personGroupId);
 
    if (trainingStatus.Status != TrainingStatusType.Running)
    {
        break;
    }
 
    await Task.Delay(1000);
} 
``` 

## <a name="step-4-identify-a-face-against-a-defined-persongroup"></a>4\. Adım: Tanımlanan PersonGroup karşı bir yüz tanımlama

Yüz tanıma API'si kimlikleri gerçekleştirdiğinde, bir grup içindeki tüm yüzeyleri arasında test yüz benzerlik hesaplar. En çok benzer kişiler test yüz için döndürür. Bu işlem aracılığıyla gerçekleştirilir [yüz tanıma - tanımlamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) API veya istemci Kitaplığı'nın IdentifyAsync yöntemi.

Önceki adımları kullanarak test yüz algılandı gerekir. Ardından yüz Kimliği ' % s'tanıma API'si ikinci bağımsız değişkeni olarak geçirilir. Birden çok yüzü kimlikleri aynı anda tanımlanabilir. Sonuç tanımlanan tüm sonuçları içerir. Varsayılan olarak, test yüz en iyi şekilde eşleşen tek bir kişi kimlik işlemine döndürür. İsterseniz, daha fazla aday iade kimlik işlemine izin vermek için isteğe bağlı parametre maxNumOfCandidatesReturned belirtin.

Aşağıdaki kod, kimlik sürecini gösterir:

```csharp 
string testImageFile = @"D:\Pictures\test_img1.jpg";

using (Stream s = File.OpenRead(testImageFile))
{
    var faces = await faceClient.Face.DetectAsync(s);
    var faceIds = faces.Select(face => face.FaceId).ToArray();
 
    var results = await faceClient.Face.IdentifyAsync(faceIds, personGroupId);
    foreach (var identifyResult in results)
    {
        Console.WriteLine("Result of face: {0}", identifyResult.FaceId);
        if (identifyResult.Candidates.Length == 0)
        {
            Console.WriteLine("No one identified");
        }
        else
        {
            // Get top 1 among all candidates returned
            var candidateId = identifyResult.Candidates[0].PersonId;
            var person = await faceClient.PersonGroupPerson.GetAsync(personGroupId, candidateId);
            Console.WriteLine("Identified as {0}", person.Name);
        }
    }
}
``` 

Adımları tamamladıktan sonra farklı yüzleri belirlemeye çalışın. Anna, fatura veya Clare yüzlerini doğru yüz algılama için karşıya yüklenen görüntüleri göre tanımlanabilir, bkz. Aşağıdaki örneklere bakın:

![Farklı yüzleri belirleyin](../Images/identificationResult.1.jpg )

## <a name="step-5-request-for-large-scale"></a>5\. Adım: Büyük ölçekli iste

Bir PersonGroup önceki tasarım sınırlamasını tabanlı 10.000 adede kadar kişi içerebilir.
Milyon ölçekli senaryolar hakkında daha fazla bilgi için bkz. [Büyük ölçek özelliğini kullanma](how-to-use-large-scale.md).

## <a name="summary"></a>Özet

Bu kılavuzda, bir kişiyi tanımlamak ve bir PersonGroup oluşturma işlemini öğrendiniz. Aşağıdaki özellikleri açıklandığı ve gösterildiği:

- Kullanarak yüzleri algılayın [yüz tanıma - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d) API.
- Kişi kullanarak oluşturma [PersonGroup - oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244) API.
- Kişiler kullanarak oluşturma [PersonGroup kişi - oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c) API.
- Kullanarak bir PersonGroup eğitme [PersonGroup – eğit](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249) API.
- Kullanarak PersonGroup karşı bilinmeyen yüzleri belirleyin [yüz tanıma - tanımlamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) API.

## <a name="related-topics"></a>İlgili konular

- [Yüz tanıma kavramları](../concepts/face-recognition.md)
- [Görüntüdeki yüzleri algılayın](HowtoDetectFacesinImage.md)
- [Yüzleri Ekle](how-to-add-faces.md)
- [Büyük ölçekli özelliğini kullanma](how-to-use-large-scale.md)
