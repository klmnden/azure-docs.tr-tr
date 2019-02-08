---
title: "Örnek: Görüntüleri - yüz tanıma API'si yüz algılama"
titleSuffix: Azure Cognitive Services
description: Görüntülerde yüzleri algılamak için Yüz Tanıma API’sini kullanın.
services: cognitive-services
author: SteveMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: sample
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: 30d5294defe02ca6c8cfd588648429859bdf19ad
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55856403"
---
# <a name="example-how-to-detect-faces-in-image"></a>Örnek: Görüntüde yüz algılama konusunda

Bu kılavuzda, cinsiyet, yaş veya poz gibi özniteklerin ayıklanmasıyla birlikte bir görüntüdeki yüzlerin nasıl algılanacağı gösterilmektedir. Örnekler, Yüz Tanıma API’si istemci kitaplığı kullanılarak C# dilinde yazılır. 

## <a name="concepts"></a>Kavramlar

Bu kılavuzda yer alan aşağıdaki kavramlardan herhangi biri hakkında bilgi sahibi değilseniz istediğiniz zaman [Sözlük](../Glossary.md)’te yer alan tanımlara başvurabilirsiniz: 

- Yüz algılama
- Yüz tanıma yer işaretleri
- Baş pozu
- Yüz öznitelikleri

## <a name="preparation"></a>Hazırlık

Bu örnekte, aşağıdaki özellikleri göstereceğiz: 

- Bir görüntüden yüzleri algılama ve dikdörtgen çerçeve kullanarak işaretleme
- Göz bebeklerinin konumlarını, burnu veya ağzı analiz etme ve sonra bunları görüntüde işaretleme
- Baş pozunu, yüzün cinsiyetini ve yaşını analiz etme

Bu özellikleri yürütmek için en az bir yüzün net olduğu bir görüntü hazırlamanız gerekir. 

## <a name="step-1-authorize-the-api-call"></a>1. Adım: API çağrısı Yetkilendir

Yüz Tanıma API’sine yapılan her çağrı için bir abonelik anahtarı gerekir. Bu anahtarın bir sorgu dizesi parametresi aracılığıyla geçirilmesi veya istek üst bilgisinde belirtilmesi gerekir. Sorgu dizesi aracılığıyla abonelik anahtarını geçirmek için örneğin, [Yüz - Algılama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) istek URL’sine bakın:

```
https://westus.api.cognitive.microsoft.com/face/v1.0/detect[?returnFaceId][&returnFaceLandmarks][&returnFaceAttributes]
&subscription-key=<Subscription Key>
```

Alternatif olarak, abonelik anahtarını da HTTP istek bağlığında belirtilebilir: **ocp-apim-subscription-key: &lt;Abonelik anahtarı&gt;**  istemci kitaplığını kullanarak, abonelik anahtarını FaceServiceClient sınıf oluşturucu üzerinden geçirilir. Örneğin:
```CSharp
faceServiceClient = new FaceServiceClient("<Subscription Key>");
```

## <a name="step-2-upload-an-image-to-the-service-and-execute-face-detection"></a>2. Adım: Hizmete bir görüntüyü karşıya yükleme ve yüz algılama yürütme

Yüz algılama en temel şekilde doğrudan bir görüntü karşıya yüklenerek gerçekleştirilir. JPEG görüntüsünden okunan verilerle uygulama/sekizli akış içerik türü ile bir "POST" isteği gönderilerek bu yapılır. Maksimum görüntü boyutu 4 MB’tır.

İstemci kitaplığı kullanılarak karşıya yükleme aracılığıyla yüz algılama bir Akış nesnesi geçirilerek gerçekleştirilir. Aşağıdaki örneğe bakın:

```CSharp
using (Stream s = File.OpenRead(@"D:\MyPictures\image1.jpg"))
{
    var faces = await faceServiceClient.DetectAsync(s, true, true);
 
    foreach (var face in faces)
    {
        var rect = face.FaceRectangle;
        var landmarks = face.FaceLandmarks;
    }
}
```

FaceServiceClient sınıfının DetectAsync yönteminin zaman uyumsuz olduğunu unutmayın. await yan tümcesini kullanmak için çağırma yöntemi de zaman uyumsuz olarak işaretlenmelidir.
Görüntü zaten web üzerindeyse ve bir URL içeriyorsa, URL sağlanarak da yüz algılama yürütülebilir. Bu örnekte istek gövdesi, URL’yi içeren bir JSON dizesi olacaktır.
İstemci kitaplığı kullanılarak, DetectAsync yönteminin başka bir aşırı yüklemesi kolayca kullanılarak bir URL aracılığıyla yüz algılama yürütülebilir.

```CSharp
string imageUrl = "http://news.microsoft.com/ceo/assets/photos/06_web.jpg";
var faces = await faceServiceClient.DetectAsync(imageUrl, true, true);
 
foreach (var face in faces)
{
    var rect = face.FaceRectangle;
    var landmarks = face.FaceLandmarks;
}
``` 

Algılanan yüzlerle döndürülen FaceRectangle özelliği, piksel cinsinden yüzdeki temel konumlardır. Genellikle bu dikdörtgen, göz, kaş, burun ve ağız içerir; başın üst kısmı, kulaklar ve çene dahil edilmez. Başın tamamını veya vesikalık çekimi (fotoğraflı kimlik türü görüntüsü) kırparsanız, yüz alanı bazı uygulamalar için çok küçük olabileceğinden dikdörtgen yüz çerçevesinin alanını genişletmek isteyebilirsiniz. Bir yüzü daha net şekilde konumlandırmak için, sonraki bölümde açıklanan yüz tanıma yer işaretlerinin kullanılması (yüz özelliklerini konumlandırma veya yüz tanıma yönü mekanizmaları) faydalı olacaktır.

## <a name="step-3-understanding-and-using-face-landmarks"></a>3. Adım: Anlama ve yüz yer işaretlerini kullanma

Yüz tanıma yer işaretleri, bir yüzdeki ayrıntılı noktalardır; genellikle bu, göz bebekleri, gözün iç ve dış köşeleri veya burun gibi yüz bileşenlerinin noktalarıdır. Yüz tanıma yer işaretleri, yüz algılama sırasında analiz edilebilecek isteğe bağlı özniteliklerdir. Algılama sonuçlarında yüz yer işaretlerini dahil etmek için FaceServiceClient sınıfı DetectAsync yöntemi için returnFaceLandmarks isteğe bağlı parametresini kullanabilir veya [Yüz - Algılama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) çağırırken returnFaceLandmarks sorgu parametresine Boole değeri olarak ‘true’ değerini geçirebilirsiniz.

Varsayılan olarak önceden tanımlanmış 27 yer işareti noktası vardır. Aşağıdaki şekilde, 27 noktanın tamamının nasıl tanımlandığı gösterilmektedir:

![HowToDetectFace](../Images/landmarks.1.jpg)

Döndürülen noktalar, yüz dikdörtgen çerçevesi gibi piksel cinsindendir. Bu nedenle görüntüdeki belirli ilgi çekici noktaları işaretlemek kolaylaşır. Aşağıdaki kod, burun ve göz bebeklerinin konumlarının alınmasını gösterir:

```CSharp
var faces = await faceServiceClient.DetectAsync(imageUrl, returnFaceLandmarks:true);
 
foreach (var face in faces)
{
    var rect = face.FaceRectangle;
    var landmarks = face.FaceLandmarks;
 
    double noseX = landmarks.NoseTip.X;
    double noseY = landmarks.NoseTip.Y;
 
    double leftPupilX = landmarks.PupilLeft.X;
    double leftPupilY = landmarks.PupilLeft.Y;
 
    double rightPupilX = landmarks.PupilRight.X;
    double rightPupilY = landmarks.PupilRight.Y;
}
``` 

Resimdeki yüz özelliklerini işaretlemenin yanı sıra, yüz tanıma yer işaretleri yüzün yönünü doğru şekilde hesaplamak için de kullanılabilir. Örneğin, yüzün yönünü, ağzın merkezinden gözlerin merkezine doğru bir vektör olarak tanımlayabiliriz. Aşağıdaki kod bunu ayrıntılı şekilde açıklamaktadır:

```CSharp
var landmarks = face.FaceLandmarks;
 
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

Yüzün bulunduğu yönü bilerek dikdörtgen yüz çerçevesini yüzle hizalanacak şekilde döndürebilirsiniz. Yüz tanıma yer işaretleri kullanıldığında daha fazla ayrıntı sağlanabileceği açıktır.

## <a name="step-4-using-other-face-attributes"></a>4. Adım: Diğer yüz öznitelikleri kullanma

Yüz Tanıma - Algılama API’si, yüz tanıma yer işaretlerinin yanı sıra bir yüzdeki diğer birçok özniteliği de analiz edebilir. Bu öznitelikler arasında şunlar yer alır:

- Yaş
- Cinsiyet
- Gülümseme yoğunluğu
- Yüz kılı
- 3B baş pozu

Bu öznitelikler, istatistiksel algoritmalar kullanılarak tahmin edilir ve her zaman %100 kesin olmayabilir. Ancak yüzleri bu özniteliklere göre sınıflandırmak istediğinizde yararlı olur. Özniteliklerin her biri hakkında daha fazla bilgi için lütfen bkz. [Sözlük](../Glossary.md).

Aşağıda, yüz algılama sırasında yüz özniteliklerini ayıklamaya ilişkin basit bir örnek yer almaktadır:

```CSharp
var requiredFaceAttributes = new FaceAttributeType[] {
                FaceAttributeType.Age,
                FaceAttributeType.Gender,
                FaceAttributeType.Smile,
                FaceAttributeType.FacialHair,
                FaceAttributeType.HeadPose,
                FaceAttributeType.Glasses
            };
var faces = await faceServiceClient.DetectAsync(imageUrl,
    returnFaceLandmarks: true,
    returnFaceAttributes: requiredFaceAttributes);

foreach (var face in faces)
{
    var id = face.FaceId;
    var attributes = face.FaceAttributes;
    var age = attributes.Age;
    var gender = attributes.Gender;
    var smile = attributes.Smile;
    var facialHair = attributes.FacialHair;
    var headPose = attributes.HeadPose;
    var glasses = attributes.Glasses;
}
``` 

## <a name="summary"></a>Özet

Bu kılavuzda, [Yüz - Algılama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) API’sinin işlevlerini, yerel karşıya yüklenen görüntüler ve web üzerindeki görüntü URL’leri için yüzleri nasıl algılayabileceğini, dikdörtgen yüz çerçeveleri döndürerek yüzleri nasıl algılayabileceğini ve yüz yer işaretlerini, 3B baş pozlarını ve diğer yüz özniteliklerini nasıl analiz edebileceğini öğrendiniz.

API ayrıntıları hakkında daha fazla bilgi için lütfen [Yüz - Algılama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) için API başvuru kılavuzuna başvurun.

## <a name="related-topics"></a>İlgili Konular

[Görüntüdeki Yüzleri Belirleme](HowtoIdentifyFacesinImage.md)
