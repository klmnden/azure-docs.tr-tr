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
ms.date: 02/22/2019
ms.author: sbowles
ms.openlocfilehash: bf3af8f5d1d2f063199a8275c2f49c70140e8732
ms.sourcegitcommit: 89b5e63945d0c325c1bf9e70ba3d9be6888da681
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57588780"
---
# <a name="get-face-detection-data"></a>Yüz algılama veri alma

Bu kılavuz, cinsiyet, yaş veya poz gibi öznitelikleri verilen bir görüntüyü ayıklamak için yüz algılama kullanmayı gösterilecektir. Bu kılavuzdaki kod parçacıkları yazılan C# yüz tanıma API'si istemci kitaplığı, ancak aynı işlevleri kullanarak aracılığıyla [REST API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

Bu kılavuz şunları nasıl yapacağınızı gösterilecek için:

- Görüntüdeki yüzleri boyutları ve konumları Al
- Çeşitli yüz yer işareti (göz bebeklerinin, burun, ağız ve benzeri) görüntüdeki konumunu alır.
- Cinsiyet, geçerlilik süresini ve duygu tanıma ve diğer özniteliklerini algılanan yüz tahmin.

## <a name="setup"></a>Kurulum

Bu kılavuzu zaten oluşturulmuş olduğunu varsayar. bir **[FaceClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceclient?view=azure-dotnet)** adlı nesne `faceClient`, yüz tanıma abonelik anahtarını ve uç nokta URL'si ile. Buradan, yüz algılama özelliğini ya da çağırarak kullanabilirsiniz **[DetectWithUrlAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceoperationsextensions.detectwithurlasync?view=azure-dotnet)** (Bu kılavuzda kullanılan) veya **[DetectWithStreamAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceoperationsextensions.detectwithstreamasync?view=azure-dotnet)**. Bkz: [yüzleri algılayın Hızlı Başlangıç için C# ](../quickstarts/csharp-detect-sdk.md) bunu ayarlamak yönergeler.

Bu kılavuz Algıla çağrı ayrıntılarına bağlı odaklanacaktır&mdash;iletebilir, hangi bağımsız değişkenleri ve döndürülen verilerle yapabilecekleriniz. Her işlemi tamamlamak için ek süre yararlanırken duyduğunuz özellikleri yalnızca sorgulama öneririz.

## <a name="get-basic-face-data"></a>Yüz temel veri alma

Yüzleri bulun ve görüntünün konumlarını almak için yöntemi çağırın _returnFaceId_ parametresini **true** (varsayılan).

```csharp
IList<DetectedFace> faces = await faceClient.Face.DetectWithUrlAsync(imageUrl, true, false, null);
```

Döndürülen **[DetectedFace](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.detectedface?view=azure-dotnet)** nesneleri benzersiz kimliklerinin ve yüz piksel koordinatları sağlayan bir dikdörtgen için sorgulanabilir.

```csharp
foreach (var face in faces)
{
    string id = face.FaceId.ToString();
    FaceRectangle rect = face.FaceRectangle;
}
```

Bkz: **[FaceRectangle](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.facerectangle?view=azure-dotnet)** yüzü boyutunu ve konumunu ayrıştırma hakkında bilgi için. Genellikle, bu dikdörtgen göz, eyebrows, burun ve ağız içerir; Baş, kulağına ve şirketinden chin üstüne mutlaka dahil değildir. Yüz dikdörtgeni tam baş veya Orta görüntüsü dikey (fotoğraf kimliği türü görüntü) kırpmak için kullanmak istiyorsanız, belirli bir kenar her yönde dikdörtgen genişletmek isteyebilirsiniz.

## <a name="get-face-landmarks"></a>Yüz tanıma yer işareti Al

Yüz tanıma yer işareti göz bebeklerinin gibi bir yüz noktalarında Bul kolay bir dizi veya burun ucunu ' dir. Yüz tanıma yer işareti veri ayarlayarak alabilirsiniz _returnFaceLandmarks_ parametresi **true**.

```csharp
IList<DetectedFace> faces = await faceClient.Face.DetectWithUrlAsync(imageUrl, true, true, null);
```

Varsayılan olarak önceden tanımlanmış 27 yer işareti noktası vardır. Tüm 27 noktaları aşağıdaki şekilde gösterilmiştir:

![Etiketli tüm 27 yer işareti ile bir yüz diyagramı](../Images/landmarks.1.jpg)

Döndürülen piksel, yüz dikdörtgeni çerçeve gibi birimi noktalarıdır. Aşağıdaki kod, burun ve göz bebeklerinin yerini nasıl aldığını gösterir:

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

Yüz tanıma yer işareti veriler, yüzey yönü doğru bir şekilde hesaplamak için de kullanılabilir. Örneğin, biz ağız ortasından vektörü yardımcı pilotluk görevini üstlenir merkezi olarak yüz tanıma döndürmesini tanımlayabilirsiniz. Aşağıdaki kod bu vektörü hesaplar:

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

Yüz tanıma yönünü bilerek daha düzgün şekilde hizalamaya dikdörtgen yüzü çerçeve döndürebilirsiniz. Görüntüdeki yüzleri kırpmak isterseniz, böylece her zaman dik zaman çizelgesi resmi programlı bir şekilde döndürebilirsiniz.

## <a name="get-face-attributes"></a>Yüz tanıma özniteliklerini alma

Yüz algılama API'si, yüz dikdörtgenler ve önemli yerleri yanı sıra yüz birkaç kavramsal öznitelikleri analiz edebilirsiniz. Bunlar:

- Yaş
- Cinsiyet
- Gülümseme yoğunluğu
- Yüz kılı
- Gözlük
- 3B baş poz
- Duygu Tanıma

> [!IMPORTANT]
> Bu öznitelikler, istatistiksel algoritmalar kullanılarak öngörülen ve her zaman doğru olmayabilir. Öznitelik verilere dayalı kararları verirken dikkatli olun.
>

Yüz öznitelikleri analiz etmek için ayarlama _returnFaceAttributes_ parametre listesine **[FaceAttributeType Enum](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.faceattributetype?view=azure-dotnet)** değerleri.

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

Ardından, döndürülen veriler başvurular alma ve daha fazla yapın gereksinimlerinize göre operations.

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

Özniteliklerin her biri hakkında daha fazla bilgi için bkz [sözlüğü](../Glossary.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu kılavuzda, yüz algılama, çeşitli işlevlere kullanmayı öğrendiniz. Ardından, bkz: [sözlüğü](../Glossary.md) alınan yüz verileri daha ayrıntılı bilgi için.

## <a name="related-topics"></a>İlgili Konular

- [Başvuru belgeleri (REST)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
- [Başvuru belgeleri (.NET SDK)](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/face?view=azure-dotnet)
