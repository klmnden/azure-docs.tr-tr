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
ms.openlocfilehash: 7da146cafaf9af5c91bbbb2a3a23d8a90d49d8cd
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64699336"
---
# <a name="example-identify-faces-in-images"></a>Örnek: Resimlerdeki yüzleri belirleyin

Bu kılavuzda, önceden bilinen kişilerden oluşturulan PersonGroups kullanılarak bilinmeyen yüzlerin nasıl belirleneceği gösterilmektedir. Örnekler, Yüz Tanıma API’si istemci kitaplığı kullanılarak C# dilinde yazılır.

## <a name="preparation"></a>Hazırlık

Bu örnekte aşağıdakileri göstereceğiz:

- PersonGroup oluşturma - Bu PersonGroup, bilinen kişilerin listesini içerir.
- Her bir kişiye yüz atama - Bu yüzler, kişileri belirlemek için temel olarak kullanılır. Fotoğraflı kimliğiniz gibi yüzünüzün ön kısmının net olduğu bir resim kullanmanız önerilir. İyi bir fotoğraf kümesinde, aynı kişinin farklı pozlarda, farklı renk kıyafetlerle ve farklı saç modelleriyle yüzleri yer almalıdır.

Bu örneğin gösterimini gerçekleştirmek için bir grup resim hazırlamanız gerekir:

- Kişinin yüzünü içeren birkaç fotoğraf. [Örnek fotoğraf Yükle](https://github.com/Microsoft/Cognitive-Face-Windows/tree/master/Data) Anna'nın, fatura ve Clare.
- Tanımlamayı test etmek için kullanılan, Anna, Bill veya Clare’in yüzlerini içerebilecek ya da içeremeyecek bir dizi test fotoğrafı. Yukarıdaki bağlantıdan bazı örnek görüntüleri de seçebilirsiniz.

## <a name="step-1-authorize-the-api-call"></a>1. Adım: API çağrısı Yetkilendir

Yüz Tanıma API’sine yapılan her çağrı için bir abonelik anahtarı gerekir. Bu anahtar, bir sorgu dizesi parametresi aracılığıyla geçirilebilir veya istek üst bilgisinde belirtilebilir. Sorgu dizesi aracılığıyla abonelik anahtarını geçirmek için örneğin, [Yüz - Algılama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) istek URL’sine bakın:
```
https://westus.api.cognitive.microsoft.com/face/v1.0/detect[?returnFaceId][&returnFaceLandmarks][&returnFaceAttributes]
&subscription-key=<Subscription key>
```

Alternatif olarak, abonelik anahtarını da HTTP istek bağlığında belirtilebilir: **ocp-apim-subscription-key: &lt;Abonelik anahtarı&gt;**  istemci kitaplığını kullanarak, abonelik anahtarını FaceServiceClient sınıf oluşturucu üzerinden geçirilir. Örneğin:
 
```CSharp 
faceServiceClient = new FaceServiceClient("<Subscription Key>");
```
 
Abonelik anahtarı, Azure portalınızın Market sayfasından elde edilebilir. Bkz. [Abonelikler](https://azure.microsoft.com/try/cognitive-services/).

## <a name="step-2-create-the-persongroup"></a>2. Adım: PersonGroup oluşturma

Bu adımda, "üç kişi içeren MyFriends" adlı bir PersonGroup oluşturduk: Anna, fatura ve Clare. Her kişinin kayıtlı birkaç yüzü vardır. Yüzlerin görüntülerden algılanması gerekir. Tüm bu adımlardan sonra, aşağıdaki görüntüye benzer bir PersonGroup öğeniz olur:

![HowToIdentify1](../Images/group.image.1.jpg)

### <a name="21-define-people-for-the-persongroup"></a>2.1 PersonGroup için kişileri tanımlama
Kişi, temel bir tanımlama birimidir. Bir kişinin bir veya daha fazla bilinen yüzü kayıtlı olabilir. Ancak PersonGroup, bir kişi koleksiyonu olup her bir kişi belirli bir PersonGroup içinde tanımlanır. Tanımlama bir PersonGroup’a karşı yapılır. Bu nedenle görev, bir PersonGroup oluşturmak ve sonra bunun içinde Anna, Bill ve Clare gibi kişiler oluşturmaktır.

İlk olarak, yeni bir PersonGroup oluşturmanız gerekir. Bu, [PersonGroup - Oluştur](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244) API’si kullanılarak yürütülür. İlgili istemci kitaplığı API’si, FaceServiceClient sınıfı için CreatePersonGroupAsync yöntemidir. Grubu oluşturmak için belirtilen grup kimliği, her abonelik için benzersizdir; diğer PersonGroup API’lerini kullanarak da PersonGroup’ları alabilir, güncelleştirebilir veya silebilirsiniz. Bir grup tanımlandıktan sonra, [PersonGroup Kişisi - Oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c) API’si kullanılarak grup içinde kişiler tanımlanabilir. İstemci kitaplığı yöntemi, CreatePersonAsync yöntemidir. Oluşturulduktan sonra her bir kişiye yüz ekleyebilirsiniz.

```CSharp 
// Create an empty PersonGroup
string personGroupId = "myfriends";
await faceServiceClient.CreatePersonGroupAsync(personGroupId, "My Friends");
 
// Define Anna
CreatePersonResult friend1 = await faceServiceClient.CreatePersonAsync(
    // Id of the PersonGroup that the person belonged to
    personGroupId,    
    // Name of the person
    "Anna"            
);
 
// Define Bill and Clare in the same way
```
### <a name="step2-2"></a> 2.2 Yüzleri algılama ve yüzleri doğru kişiye kaydetme
HTTP istek gövdesinde görüntü dosyası ile [Yüz - Algılama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) API’sine bir “POST” web isteği gönderilerek algılama gerçekleştirilir. İstemci kitaplığı kullanılırken yüz algılama işlemi, FaceServiceClient sınıfının DetectAsync yöntemi aracılığıyla yürütülür.

Algılanan her yüz için [PersonGroup Kişisi – Yüz Ekle](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) API’sini çağırarak bu yüzü doğru kişiye ekleyebilirsiniz.

Aşağıdaki kod, bir görüntüden yüzü algılama ve bu yüzü bir kişiye ekleme işlemini göstermektedir:
```CSharp 
// Directory contains image files of Anna
const string friend1ImageDir = @"D:\Pictures\MyFriends\Anna\";
 
foreach (string imagePath in Directory.GetFiles(friend1ImageDir, "*.jpg"))
{
    using (Stream s = File.OpenRead(imagePath))
    {
        // Detect faces in the image and add to Anna
        await faceServiceClient.AddPersonFaceAsync(
            personGroupId, friend1.PersonId, s);
    }
}
// Do the same for Bill and Clare
``` 
Görüntünün birden fazla yüz içermesi durumunda yalnızca en büyük yüzün eklendiğine dikkat edin. [PersonGroup Kişisi - Yüz Ekleme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) API’sinin targetFace sorgu parametresine "targetFace = left, top, width, height" biçiminde bir dize geçirerek veya başka yüzler eklemek için AddPersonFaceAsync yönteminin isteğe bağlı targetFace parametresini kullanarak kişiye başka yüzler ekleyebilirsiniz. Kişiye eklenen her yüze benzersiz bir yüz kimliği verilir ve bu, [PersonGroup Kişisi – Yüz Sil](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523e) ve [Yüz – Belirle](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) API’lerinde kullanılabilir.

## <a name="step-3-train-the-persongroup"></a>3. Adım: PersonGroup eğitin

PersonGroup kullanılarak bir tanımlama gerçekleştirilebilmesi için PersonGroup eğitilmelidir. Ayrıca herhangi bir kişi eklendikten veya kaldırıldıktan sonra ya da herhangi bir kişinin kayıtlı yüzünün düzenlenmesi durumunda da yeniden eğitilmesi gerekir. Eğitim, [PersonGroup – Eğitim](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249) API’si tarafından gerçekleştirilir. İstemci kitaplığı kullanılırken bu yalnızca TrainPersonGroupAsync yöntemine yapılan bir çağrıdır:
 
```CSharp 
await faceServiceClient.TrainPersonGroupAsync(personGroupId);
```
 
Eğitim, zaman uyumsuz bir işlemdir. TrainPersonGroupAsync yöntemi döndürüldükten sonra da tamamlanmayabilir. İstemci kitaplığının GetPersonGroupTrainingStatusAsync yöntemi veya [PersonGroup - Eğitim Durumunu Al](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395247) API’si ile eğitim durumunu sorgulamanız gerekebilir. Aşağıdaki kod, PersonGroup eğitiminin bitmesini beklemeye ilişkin basit bir mantığı göstermektedir:
 
```CSharp 
TrainingStatus trainingStatus = null;
while(true)
{
    trainingStatus = await faceServiceClient.GetPersonGroupTrainingStatusAsync(personGroupId);
 
    if (trainingStatus.Status != Status.Running)
    {
        break;
    }
 
    await Task.Delay(1000);
} 
``` 

## <a name="step-4-identify-a-face-against-a-defined-persongroup"></a>4. Adım: Tanımlanan PersonGroup karşı bir yüz tanımlama

Tanımlamalar gerçekleştirilirken Yüz Tanıma API’si bir grup içindeki tüm yüzler arasında bir test yüzünün benzerliğini hesaplayabilir ve o test yüzü için en karşılaştırılabilir kişileri döndürür. Bu, istemci kitaplığının IdentifyAsync yöntemi veya [Yüz - Belirleme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) API’si aracılığıyla gerçekleştirilir.

Test yüzünün, önceki adımlar kullanılarak algılanması gerekir ve yüz kimliği, ikinci bir bağımsız değişken olarak belirleme API’sine geçirilir. Aynı anda birden çok yüz kimliği belirlenebilir ve sonuç tüm belirleme sonuçlarını içerir. Varsayılan olarak belirleme yalnızca test yüzüyle en iyi eşleşen bir kişiyi döndürür. İsterseniz, belirleme API’sinin daha fazla aday döndürmesini sağlamak için isteğe bağlı maxNumOfCandidatesReturned parametresini belirtebilirsiniz.

Aşağıdaki kod, belirleme işlemini gösterir:

```CSharp 
string testImageFile = @"D:\Pictures\test_img1.jpg";

using (Stream s = File.OpenRead(testImageFile))
{
    var faces = await faceServiceClient.DetectAsync(s);
    var faceIds = faces.Select(face => face.FaceId).ToArray();
 
    var results = await faceServiceClient.IdentifyAsync(personGroupId, faceIds);
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
            var person = await faceServiceClient.GetPersonAsync(personGroupId, candidateId);
            Console.WriteLine("Identified as {0}", person.Name);
        }
    }
}
``` 

Adımları tamamladıktan sonra, farklı yüzleri belirlemeyi deneyebilir ve yüz algılama için karşıya yüklenen görüntülere göre, Anna, Bill veya Clare’in yüzlerinin doğru şekilde belirlenip belirlenemediğini görebilirsiniz. Aşağıdaki örneklere bakın:

![HowToIdentify2](../Images/identificationResult.1.jpg )

## <a name="step-5-request-for-large-scale"></a>5. Adım: İstek için büyük ölçekli

Bilindiği gibi bir PersonGroup, önceki tasarımın sınırlaması nedeniyle 10.000’e kadar kişiyi barındırabilir.
Milyon ölçekli senaryolar hakkında daha fazla bilgi için bkz. [Büyük ölçek özelliğini kullanma](how-to-use-large-scale.md).

## <a name="summary"></a>Özet

Bu kılavuzda, bir PersonGroup oluşturma ve bir kişiyi belirleme işlemini öğrendiniz. Aşağıda, önceden açıklanmış ve gösterilmiş olan özelliklerin hızlı bir anımsatıcısı yer almaktadır:

- [Yüz - Algılama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d) API’sini kullanarak yüzleri algılama
- [PersonGroup - Oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244) API’sini kullanarak PersonGroup oluşturma
- [PersonGroup Kişisi - Oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c) API’sini kullanarak kişiler oluşturma
- [PersonGroup – Eğitim](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249) API’sini kullanarak PersonGroup öğesini eğitme
- [Yüz - Belirleme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) API’sini kullanarak PersonGroup öğesine karşı bilinmeyen yüzleri belirleme

## <a name="related-topics"></a>İlgili Konular

- [Yüz tanıma kavramları](../concepts/face-recognition.md)
- [Görüntüdeki Yüzleri Algılama](HowtoDetectFacesinImage.md)
- [Yüz Ekleme](how-to-add-faces.md)
- [Büyük ölçek özelliğini kullanma](how-to-use-large-scale.md)
