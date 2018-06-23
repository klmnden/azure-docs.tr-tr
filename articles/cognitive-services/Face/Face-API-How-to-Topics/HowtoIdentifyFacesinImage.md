---
title: Yüz API ile görüntülerinde yüzeyleri tanımlamak | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Yüz API Bilişsel Hizmetleri'nde görüntülerinde yüzeyleri tanımlamak için kullanın.
services: cognitive-services
author: SteveMSFT
manager: corncar
ms.service: cognitive-services
ms.component: face-api
ms.topic: article
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: 3f75db176055d9f784ec978497d7cae077ff629f
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355390"
---
# <a name="how-to-identify-faces-in-images"></a>Yazıtipleri görüntülerinde belirleme

Bu kılavuz, bilinen kişilerden önceden oluşturulan PersonGroups kullanarak bilinmeyen yüzeyleri tanımlamak gösterilmiştir. Örnekler C# ' ta yüz API'si istemci kitaplığı kullanılarak yazılır.

## <a name="concepts"></a> Kavramları

Lütfen bu kılavuzda aşağıdaki kavramlar hakkında bilgi sahibi değilseniz tanımlarında arama bizim [sözlüğü](../Glossary.md) herhangi bir zamanda:

- Yüz - Algıla
- Yüz - tanımlama
- PersonGroup

## <a name="preparation"></a> Hazırlama

Bu örnekte, biz aşağıda gösterilmektedir:

- Bir PersonGroup - oluşturma bu PersonGroup bilinen kişilerin listesini içerir.
- Her kişi için - yüzeyleri atama bu yüzeyleri kişiler tanımlamak için temel olarak kullanılır. Clear ön yüz, fotoğraf kimliğinizi gibi kullanmak için önerilir. Fotoğraf iyi bir dizi farklı üzerinden, elbise renkleri veya artı stiller aynı kişiye yüzeyleri içermelidir.

Bu örnek Tanıtım taşımak için bir grup resimleri hazırlamanız gerekir:

- Kişinin yüz sahip birkaç fotoğraf. [Örnek fotoğraf yüklemek için burayı tıklatın](https://github.com/Microsoft/Cognitive-Face-Windows/tree/master/Data) Gamze, fatura ve Clare.
- Test tanımı için bir dizi olabilir veya Gamze, fatura veya Clare yüzeyleri içermeyebilir test fotoğraf kullanılır. Önceki bağlantıdan bazı örnek görüntüleri öğesini de seçebilirsiniz.

## <a name="step1"></a> 1. adım: API çağrısı yetkilendirme

Her yüz API çağrısı bir abonelik anahtarı gerektirir. Bu anahtarı ya da bir sorgu dizesi parametresi geçirilen veya istek başlığında belirtilen. Sorgu dizesi abonelik tuşuyla geçirmek için istek URL'si için lütfen [yüz - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) bir örnek olarak:
```
https://westus.api.cognitive.microsoft.com/face/v1.0/detect[?returnFaceId][&returnFaceLandmarks][&returnFaceAttributes]
&subscription-key=<Subscription key>
```

Alternatif olarak, abonelik anahtarı da HTTP istek üstbilgisinde belirtilebilir: **apim abonelik anahtar ocp: &lt;abonelik anahtarı&gt;**  bir istemci kitaplığı kullanırken, abonelik anahtarı geçirilen FaceServiceClient sınıf oluşturucu kullanılarak. Örneğin:
 
```CSharp 
faceServiceClient = new FaceServiceClient("<Subscription Key>");
```
 
Abonelik anahtarı Azure portal'ınızın Market sayfasından elde edilebilir. Bkz: [abonelikleri](https://azure.microsoft.com/try/cognitive-services/).

## <a name="step2"></a> 2. adım: PersonGroup oluşturma

Bu adımda, "üç kişileri içeren MyFriends" adlı bir PersonGroup oluşturduk: Gamze, fatura ve Clare. Her kişi kayıtlı birkaç yüz sahiptir. Yazıtipleri görüntülerden algılanabilmesi gerekir. Tüm adımları aşağıdaki resimde gibi bir PersonGroup vardır:

![HowToIdentify1](../Images/group.image.1.jpg)

### <a name="step2-1"></a> 2.1 PersonGroup kişileri tanımlayın
Bir kişi tanımla temel birimidir. Bir kişi, kayıtlı bir veya daha fazla bilinen yüzeyleri olabilir. Ancak, bir PersonGroup kişiler koleksiyonudur ve her birinin belirli bir PersonGroup içinde tanımlanır. Kimliği PersonGroup karşı yapılır. Bu nedenle, bir PersonGroup oluşturmak ve ardından kişiler içinde Gamze, fatura ve Clare gibi oluşturmak için bir görevdir.

İlk olarak, yeni PersonGroup oluşturmanız gerekir. Bu kullanarak yürütülür [PersonGroup - oluşturmak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244) API. Karşılık gelen istemci kitaplığı API FaceServiceClient sınıfı için CreatePersonGroupAsync yöntemidir. Grup Kimliği grubu oluşturmak için her abonelik için benzersiz belirtilen – ayrıca alma, güncelleştirme veya diğer PersonGroup API'lerini kullanarak PersonGroups silin. Bir grup tanımlandıktan sonra kişilerin sonra içinde kullanılarak tanımlanabilir [PersonGroup kişi - oluşturmak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c) API. İstemci Kitaplığı CreatePersonAsync yöntemidir. Bunlar oluşturduktan sonra her kişi için yüz ekleyebilirsiniz.

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
### <a name="step2-2"></a> 2.2 Yüz algılama ve doğru kişiye kaydetme
Algılama, bir "POST" web isteği göndererek yapılır [yüz - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) HTTP istek gövdesindeki resim dosyasının API. İstemci Kitaplığı kullanırken, yüz algılama FaceServiceClient sınıfı DetectAsync yöntemiyle yürütülür.

Her yüz çağırabilirsiniz algılanan için [PersonGroup kişi – eklemek yüz](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) doğru kişi eklemek için.

Aşağıdaki kod, bir yazıtipi bir görüntüden algılar ve bir kişiye ekleme işlemini gösterir:
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
Görüntünün birden fazla yüz içeriyorsa, yalnızca büyük yüz eklenir dikkat edin. Diğer yüzeyleri biçiminde bir dize geçirerek kişiye ekleyebilmeniz için "targetFace = sol, üst düzey, genişlik, yükseklik" için [PersonGroup kişi - eklemek yüz](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) API'nin targetFace sorgu parametresi veya targetFace isteğe bağlı parametresi için kullanılıyor Diğer yüzeyleri eklemek için AddPersonFaceAsync yöntemi. Kullanılabilir bir benzersiz kalıcı yüz kimliği kişiye eklenen her yüz verilecek [PersonGroup kişi – silme yüz](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523e) ve [yüz – tanımlamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239).
## <a name="step3"></a> 3. adım: PersonGroup eğitme

Bir kimlik kullanmadan gerçekleştirilmeden önce PersonGroup eğitilmiş gerekir. Ayrıca, ekleme veya herhangi bir kişi kaldırma sonrasında retrained sahip veya herhangi bir kişi düzenlenebilir kendi kayıtlı yüz varsa. Eğitimi Bitti [PersonGroup – tren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249) API. İstemci kitaplığı kullanılarak yalnızca TrainPersonGroupAsync yöntemine bir çağrı olur:
 
```CSharp 
await faceServiceClient.TrainPersonGroupAsync(personGroupId);
```
 
Eğitim zaman uyumsuz bir işlemdir. Hatta TrainPersonGroupAsync yöntemi döndürülen sonra onu bitmedi. Eğitim durumuna göre sorgu gerekebilir [PersonGroup - eğitim durumunu Al](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395247) istemci kitaplığının API veya GetPersonGroupTrainingStatusAsync yöntemi. Aşağıdaki kod PersonGroup bekleyen basit bir mantık gösterir tamamlamak eğitim:
 
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

## <a name="step4"></a> 4. adım: bir yüzü tanımlanmış PersonGroup karşı tanımlayın
Kimlikleri gerçekleştirirken, yüz API test nominal bir grup içindeki tüm yüzeyleri arasında benzerlik hesaplayabilirsiniz ve bu test yüz için en karşılaştırılabilir kartına kopyalamasını döndürür. Bu yoluyla yapılır [yüz - tanımlama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) API veya istemci kitaplığının IdentifyAsync yöntemi.

Sınama yüz önceki adımları kullanarak algılanabilmesi gerekir ve ardından yüz kimliği tanımla API ikinci bağımsız değişkeni olarak geçirilir. Birden çok yüz kimlikleri aynı anda tanımlanabilir ve sonucu tüm tanımla sonuçlarını içerir. Varsayılan olarak, test yüz en iyi eşleşen yalnızca tek bir kişi tanımla döndürür. İsterseniz, daha fazla aday dönüş tanımla izin vermek için isteğe bağlı bir parametre maxNumOfCandidatesReturned belirtebilirsiniz.

Aşağıdaki kod tanımla sürecini gösterir:
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

Adımları tamamladıktan sonra farklı yüzeyleri tanımlamak ve Gamze, fatura veya Clare yüzeyleri doğru yüz algılama için karşıya yansımaları göre tanımlanabilir olmadığını görmek deneyebilirsiniz. Aşağıdaki örneklere bakın:

![HowToIdentify2](../Images/identificationResult.1.jpg )

## <a name="step5"></a> 5. adım: istekte büyük ölçekli

Bilinen gibi bir PersonGroup önceki tasarım sınırlaması nedeniyle 10.000 kişi basılı tutabilirsiniz.
Hakkında daha fazla bilgi kadar milyon ölçekli senaryolar bkz [büyük ölçekli özelliğinin nasıl kullanılacağı](how-to-use-large-scale.md).

## <a name="summary"></a> Özet

Bu kılavuzda, bir PersonGroup oluşturma ve bir kişi tanımlama işlemi öğrendiniz. Daha önce açıklandığı ve gösterildiği özelliklerinin hızlı anımsatıcısı şunlardır:

- Algılama bakarken kullanarak [yüz - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d) API
- PersonGroups kullanarak oluşturma [PersonGroup - oluşturmak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244) API
- Kullanarak kişileri oluşturma [PersonGroup kişi - oluşturmak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c) API
- Kullanarak bir PersonGroup eğitmek [PersonGroup – tren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249) API
- PersonGroup kullanarak karşı bilinmeyen yüzeyleri tanımlayan [yüz - tanımlama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) API

## <a name="related"></a> İlgili Konular

- [Görüntüde nasıl algılanacağını yüzler](HowtoDetectFacesinImage.md)
- [Yazıtipleri ekleme](how-to-add-faces.md)
- [Büyük ölçekli özelliğinin nasıl kullanılacağı](how-to-use-large-scale.md)
