---
title: Yazıtipleri yüz API görüntülerle algılamak | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Yüz API Bilişsel Hizmetleri'nde yüzeyleri görüntülerinde algılamak için kullanın.
services: cognitive-services
author: SteveMSFT
manager: corncar
ms.service: cognitive-services
ms.component: face-api
ms.topic: article
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: 57cd0915450428399fd680638aa4fae2cdbe17c9
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351730"
---
# <a name="how-to-detect-faces-in-image"></a>Görüntüde nasıl algılanacağını yüzler

Bu kılavuz, cinsiyetiniz, yaş veya ayıklanan poz gibi yüz özniteliklere sahip bir görüntüden yüzeyleri algılamak nasıl gösterecek. Örnekler C# ' ta yüz API'si istemci kitaplığı kullanılarak yazılır. 

## <a name="concepts"></a> Kavramları

Bu kılavuzda aşağıdaki kavramlar hiçbirini alışık değilseniz tanımlarında Lütfen bizim [sözlüğü](../Glossary.md) herhangi bir zamanda: 

- Yüz algılama
- Yüz bilinen yerler
- HEAD teşkil
- Yüz öznitelikleri

## <a name="preparation"></a> Hazırlama

Bu örnekte, biz aşağıdaki özellikleri gösterilmektedir: 

- Bir görüntüden yüz algılama ve işaretlemeden dikdörtgen çerçeveleme kullanma
- Göz bebeklerinin, burun veya ağzınıza konumlarını çözümleme ve bunları görüntüde işaretleme
- Head poz, cinsiyetiniz ve yüz yaşını analiz etme

Bu özellikler yürütmek için en az bir Temizle yüz görüntüyle hazırlamak gerekir. 

## <a name="step1"></a> 1. adım: API çağrısı yetkilendirme

Her yüz API çağrısı bir abonelik anahtarı gerektirir. Bu anahtar, istek başlığında belirtilen veya bir sorgu dizesi parametresi geçirilen gerekiyor. Sorgu dizesi abonelik tuşuyla geçirmek için istek URL'si için lütfen [yüz - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) bir örnek olarak:

```
https://westus.api.cognitive.microsoft.com/face/v1.0/detect[?returnFaceId][&returnFaceLandmarks][&returnFaceAttributes]
&subscription-key=<Subscription Key>
```

Alternatif olarak, abonelik anahtarı da HTTP istek üstbilgisinde belirtilebilir: **apim abonelik anahtar ocp: &lt;abonelik anahtarı&gt;**  bir istemci kitaplığı kullanırken, abonelik anahtarı geçirilen FaceServiceClient sınıf oluşturucu kullanılarak. Örneğin:
```CSharp
faceServiceClient = new FaceServiceClient("<Subscription Key>");
```

## <a name="step2"></a> 2. adım: bir görüntü hizmetine yüklemek ve yüz algılama yürütme

Yüz algılama gerçekleştirmek için en temel görüntü doğrudan karşıya yükleyerek yoludur. Bu, bir JPEG görüntüsünden okunan veriler ile uygulama/octet-akış içerik türüyle bir "POST" isteği göndererek gerçekleştirilir. Resmin en büyük boyutu 4 MB'tır.

İstemci kitaplığı kullanılarak, karşıya yükleme yoluyla yüz algılama geçirme akış nesnesindeki gerçekleştirilir. Aşağıdaki örneğe bakın:
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

FaceServiceClient DetectAsync yöntemini zaman uyumsuz olduğunu unutmayın. Bekleme yan tümcesi kullanmak için arama yöntemi de, zaman uyumsuz olarak işaretlenmesi gerekir.
Görüntü zaten Web'de ve bir URL'ye sahip ayrıca URL sağlayarak yüz algılama çalıştırılabilir. Bu örnekte, istek gövdesini URL'si içeren bir JSON dizesinde olacaktır.
İstemci kitaplığı kullanılarak, bir URL yoluyla yüz algılama kolayca başka bir aşırı yüklemesini DetectAsync yöntemi kullanılarak çalıştırılabilir.
```CSharp
string imageUrl = "http://news.microsoft.com/ceo/assets/photos/06_web.jpg";
var faces = await faceServiceClient.DetectAsync(imageUrl, true, true);
 
foreach (var face in faces)
{
    var rect = face.FaceRectangle;
    var landmarks = face.FaceLandmarks;
}
``` 

Algılanan yazıtipleri ile döndürülen FaceRectangle temelde piksel cinsinden yüz konumlarının özelliğidir. Genellikle, bu dikdörtgen göz, eyebrows, burun ve ağzınıza içerir – en head, kulağına ve şirketinden chin dahil değildir. Bir tam head veya Orta görüntüsü dikey (fotoğraf kimliği türü görüntü) kırparsanız yüzey alanını bazı uygulamalar için çok küçük olabilir çünkü dikdörtgen yüz çerçeve bölümünü genişletin isteyebilirsiniz. Yüz işaretleri kullanarak bir yazıtipi daha hassas bir şekilde bulmak için (yüz özellikleri bulmak veya yüz yön mekanizmalarını) açıklanan kullanışlı olması için sonraki bölümde kanıtlamak.

## <a name="step3"></a> 3. adım: Anlama ve yüz yer işaretlerini kullanma

Yüz işaretleri bir dizi bir yazıtipi ayrıntılı noktalarında; yine de uygun istiyor musunuz? genellikle yüz bileşenlerinin noktaları göz bebeklerinin, canthus veya burun gibi. Yüz işaretleri yüz algılama sırasında çözümlenebilir isteğe bağlı öznitelikleridir. İki geçiş 'true' olarak returnFaceLandmarks sorgu parametresi için bir Boole değeri çağrılırken yapabilecekleriniz [yüz - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), veya FaceServiceClient sınıf sırayla DetectAsync yöntemi için returnFaceLandmarks isteğe bağlı parametresini kullanın Algılama sonuçları yüz işaretleri eklemek için.

Varsayılan olarak, 27 önceden tanımlanmış önemli nokta vardır. Aşağıdaki şekil gösterir tüm 27 noktaları tanımlanan nasıl:

![HowToDetectFace](../Images/landmarks.1.jpg)

Döndürülen yüz Dikdörtgen Çerçeve gibi piksel birimlerindeki noktalarıdır. Görüntüde ilgilendiğiniz belirli noktaları işaretlemek kolaylaştırır. Aşağıdaki kod, burun ve göz bebeklerinin konumlarını alma gösterir:
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

Görüntüdeki yüz özellikleri işaretleme yanı sıra yüz yer işaretlerini de doğru şekilde yüz yönünü hesaplamak için kullanılabilir. Örneğin, biz yüz yönünü ağzınıza Merkezi'nden vektör gözler merkezi olarak tanımlayabilir. Aşağıdaki kod bu ayrıntılı olarak açıklanmaktadır:

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

Yüz bulunduğu yönü öğrenerek ile yüz hizalamak için dikdörtgen yüz çerçeve döndürebilirsiniz. Yüz yer işaretlerini kullanma'nün daha fazla ayrıntı ve yardımcı programı sağlayabilir işaretlenmemiştir.

## <a name="step4"></a> 4. adım: diğer yüz öznitelikleri kullanma

Yüz bilinen yerler yanı sıra, yüz – algılamak API de birkaç bir yazıtipi özniteliklerinde analiz edebilirsiniz. Bu öznitelikler içerir:

- Yaş
- Cinsiyet
- Gülümseme gönderme yoğunluğu
- Yüz artı
- Bir 3B baş poz

Bu öznitelikler istatistiksel algoritmaları kullanarak tahmin ve her zaman % 100 kesin olmayabilir. Bu özniteliklerle yüzeyleri sınıflandırmak istediğiniz zaman ancak, bunlar hala faydalıdır. Her bir öznitelikleri hakkında daha fazla bilgi için lütfen başvurmak [sözlüğü](../Glossary.md).

Yüz algılama sırasında yüz öznitelikleri ayıklanması, basit bir örnek aşağıda verilmiştir:
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
## <a name="summary"></a> Özet

Bu kılavuzda işlevlerini öğrendiniz [yüz - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) API'si, yerel resim veya görüntü URL'leri Web'de karşıya için nasıl yüzeyleri algılayabildiği; nasıl yüzeyleri dikdörtgen yüz çerçeveleri; döndürerek algılayabildiği ve nasıl de analiz yüz işaretleri, 3B baş üzerinden ve diğer yüz özellikleri de.

İçin API Başvurusu Kılavuzu API ayrıntıları hakkında daha fazla bilgi için lütfen bakın [yüz - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

## <a name="related"></a> İlgili Konular

[Görüntüde yüzeyleri belirleme](HowtoIdentifyFacesinImage.md)
