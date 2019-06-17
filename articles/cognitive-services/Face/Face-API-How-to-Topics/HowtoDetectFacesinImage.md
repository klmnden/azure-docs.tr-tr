---
title: Bir görüntüde - yüz tanıma API'si yüz algılama
titleSuffix: Azure Cognitive Services
description: Yüz algılama özelliği tarafından döndürülen çeşitli veriler kullanmayı öğrenin.
services: cognitive-services
author: SteveMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 04/18/2019
ms.author: sbowles
ms.openlocfilehash: 46bd1bdd55725878bc7b1bd55d5e24b78d82aada
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66124545"
---
# <a name="get-face-detection-data"></a>Yüz algılama veri alma

Bu kılavuz, cinsiyet, yaş veya poz gibi öznitelikleri verilen bir görüntüyü ayıklamak için yüz algılama nasıl yapılacağı açıklanır. Bu kılavuzdaki kod parçacıkları yazılan C# Azure Bilişsel hizmetler yüz tanıma API'si istemci kitaplığı kullanarak. Aynı işlevleri aracılığıyla kullanılabilir [REST API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

Bu kılavuz size nasıl gösterir için:

- Görüntüdeki yüzleri boyutları ve konumları Al
- Göz bebeklerinin burun ve görüntünün ağız gibi çeşitli yüz işaretleri konumları Al
- Cinsiyet, yaş, duygu ve diğer özniteliklerini algılanan yüz tahmin.

## <a name="setup"></a>Kurulum

Bu kılavuzu zaten oluşturulmuş olduğunu varsayar. bir [FaceClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceclient?view=azure-dotnet) adlı nesne `faceClient`, yüz tanıma abonelik anahtarını ve uç nokta URL'si ile. Buradan, yüz algılama özelliğini ya da çağırarak kullanabilirsiniz [DetectWithUrlAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceoperationsextensions.detectwithurlasync?view=azure-dotnet), bu kılavuzda kullanılan veya [DetectWithStreamAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceoperationsextensions.detectwithstreamasync?view=azure-dotnet). Bu özelliğin ayarlanması konusunda yönergeler için bkz. [Algıla yüzler için Hızlı Başlangıç C# ](../quickstarts/csharp-detect-sdk.md).

Bu kılavuz Algıla çağrı ayrıntılarına bağlı odaklanır, hangi bağımsız değişkenler gibi geçirebilirsiniz ve döndürülen verilerle yapabilecekleriniz. Yalnızca gereksinim duyduğunuz özellikleri için sorgu öneririz. Her işlemi tamamlamak için ek zaman alır.

## <a name="get-basic-face-data"></a>Yüz temel veri alma

Yüzleri bulun ve görüntünün konumlarını almak için yöntemi çağırın _returnFaceId_ parametresini **true**. Bu varsayılan ayardır.

```csharp
IList<DetectedFace> faces = await faceClient.Face.DetectWithUrlAsync(imageUrl, true, false, null);
```

Döndürülen sorgu [DetectedFace](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.detectedface?view=azure-dotnet) kendi benzersiz nesneleri kimlikleri ve yüz piksel koordinatları sunan bir dikdörtgen.

```csharp
foreach (var face in faces)
{
    string id = face.FaceId.ToString();
    FaceRectangle rect = face.FaceRectangle;
}
```

Yüz boyutunu ve konumunu ayrıştırma hakkında daha fazla bilgi için bkz: [FaceRectangle](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.facerectangle?view=azure-dotnet). Genellikle, bu dikdörtgen göz, eyebrows, burun ve ağız içerir. Baş, kulağına ve şirketinden chin üstüne mutlaka dahil değildir. Eksiksiz bir head kırpma veya belki de bir fotoğraf kimliği türü görüntüsü için bir orta görüntüsü dikey almak için yüz dikdörtgeni kullanmak için her yönde bir dikdörtgen genişletebilirsiniz.

## <a name="get-face-landmarks"></a>Yüz tanıma yer işareti Al

[Yüz tanıma, yer işareti](../concepts/face-detection.md#face-landmarks) bir kolayca bulunabilen noktalarında göz bebeklerinin veya burun ucunu gibi bir yüz yer alır. Yüz tanıma yer işareti veri almak üzere _returnFaceLandmarks_ parametresi **true**.

```csharp
IList<DetectedFace> faces = await faceClient.Face.DetectWithUrlAsync(imageUrl, true, true, null);
```

Aşağıdaki kod, burun ve göz bebeklerinin yerini nasıl aldığını gösterir:

```csharp
foreach (var face in faces)
{
    var landmarks = face.FaceLandmarks;

    double noseX = landmarks.NoseTip.X;
    double noseY = landmarks.NoseTip.Y;

    double leftPupilX = landmarks.PupilLeft.X;
    double leftPupilY = landmarks.PupilLeft.Y;

    double rightPupilX = landmarks.PupilRight.X;
    double rightPupilY = landmarks.PupilRight.Y;
}
```

Yüz tanıma yer işareti veri, yüzey yönü doğru bir şekilde hesaplamak için de kullanabilirsiniz. Örneğin, ağız ortasından vektörü yardımcı pilotluk görevini üstlenir merkezi olarak yüz tanıma döndürmesini tanımlayabilirsiniz. Aşağıdaki kod bu vektörü hesaplar:

```csharp
var upperLipBottom = landmarks.UpperLipBottom;
var underLipTop = landmarks.UnderLipTop;

var centerOfMouth = new Point(
    (upperLipBottom.X + underLipTop.X) / 2,
    (upperLipBottom.Y + underLipTop.Y) / 2);

var eyeLeftInner = landmarks.EyeLeftInner;
var eyeRightInner = landmarks.EyeRightInner;

var centerOfTwoEyes = new Point(
    (eyeLeftInner.X + eyeRightInner.X) / 2,
    (eyeLeftInner.Y + eyeRightInner.Y) / 2);

Vector faceDirection = new Vector(
    centerOfTwoEyes.X - centerOfMouth.X,
    centerOfTwoEyes.Y - centerOfMouth.Y);
```

Yüz tanıma yönünü bildiğinizde, daha doğru bir şekilde hizalamak için dikdörtgen yüzü çerçeve döndürebilirsiniz. Böylece her zaman dik zaman çizelgesi görüntüdeki yüzleri kırpmak için program aracılığıyla resim döndürebilirsiniz.

## <a name="get-face-attributes"></a>Yüz tanıma özniteliklerini alma

Yüz algılama API'si, yüz dikdörtgenler ve önemli yerleri yanı sıra yüz birkaç kavramsal öznitelikleri analiz edebilirsiniz. Tam bir listesi için bkz [yüz öznitelikleri](../concepts/face-detection.md#attributes) kavramsal bölümü.

Yüz öznitelikleri analiz etmek için ayarlama _returnFaceAttributes_ parametre listesine [FaceAttributeType Enum](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.faceattributetype?view=azure-dotnet) değerleri.

```csharp
var requiredFaceAttributes = new FaceAttributeType[] {
    FaceAttributeType.Age,
    FaceAttributeType.Gender,
    FaceAttributeType.Smile,
    FaceAttributeType.FacialHair,
    FaceAttributeType.HeadPose,
    FaceAttributeType.Glasses,
    FaceAttributeType.Emotion
};
var faces = await faceClient.DetectWithUrlAsync(imageUrl, true, false, requiredFaceAttributes);
```

Ardından, başvuruları döndürülen verileri alın ve ihtiyaçlarınıza göre daha fazla işlem yapın.

```csharp
foreach (var face in faces)
{
    var attributes = face.FaceAttributes;
    var age = attributes.Age;
    var gender = attributes.Gender;
    var smile = attributes.Smile;
    var facialHair = attributes.FacialHair;
    var headPose = attributes.HeadPose;
    var glasses = attributes.Glasses;
    var emotion = attributes.Emotion;
}
```

Özniteliklerin her biri hakkında daha fazla bilgi için bkz: [yüz algılama ve öznitelikleri](../concepts/face-detection.md) kavramsal Kılavuzu.

## <a name="next-steps"></a>Sonraki adımlar

Bu kılavuzda, yüz algılama, çeşitli işlevlere kullanmayı öğrendiniz. Ardından, bu özelliklerin kapsamlı bir öğreticiden yararlanmayı izleyerek uygulamanızla tümleştirmek.

- [Öğretici: Bir resimdeki yüz verileri görüntülemek için bir WPF uygulaması oluşturma](../Tutorials/FaceAPIinCSharpTutorial.md)
- [Öğretici: Çerçevenin bir resimdeki yüz ve algılamak için Android uygulaması oluşturma](../Tutorials/FaceAPIinJavaForAndroidTutorial.md)

## <a name="related-topics"></a>İlgili konular

- [Başvuru belgeleri (REST)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
- [Başvuru belgeleri (.NET SDK)](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/face?view=azure-dotnet)