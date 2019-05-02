---
title: HeadPose yüz dikdörtgeni ayarlamak için kullanın
titleSuffix: Azure Cognitive Services
description: HeadPose öznitelik yüz dikdörtgeni otomatik olarak döndürmek için kullanmayı öğrenin.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 04/26/2019
ms.author: pafarley
ms.openlocfilehash: ddc5bc522c0d3ac258581f2a48a5c3b755302f01
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2019
ms.locfileid: "64576509"
---
# <a name="use-the-headpose-attribute-to-adjust-the-face-rectangle"></a>Yüz dikdörtgeni ayarlamak için HeadPose özniteliğini kullanın

Bu kılavuzda, bir algılanan yüz özniteliği HeadPose, yüz tanıma nesnesinin dikdörtgeni döndürmek için kullanın. Örnek kodu bu kılavuzda, [Bilişsel hizmetler yüz tanıma WPF](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/Cognitive-Services-Face-WPF) örnek uygulama, .NET SDK'sını kullanır.

Döndürülen algılanan her yüz tanıma, yüz dikdörtgeni resimdeki yüz boyutunu ve konumunu işaretler. Varsayılan olarak, dikdörtgeni görüntünün (mükemmel dikey ve yatay, kenarlar) ile her zaman hizalanır; Bu, açılı yüzleri çerçeveleme için verimsiz olabilir. Program aracılığıyla bir görüntüdeki yüzleri kırpmak için istediğiniz durumlarda bu kırpılacağını dikdörtgeni döndürmek için avantajlıdır.

## <a name="explore-the-sample-code"></a>Örnek kodu inceleyin

Yüz dikdörtgeni HeadPose özniteliği kullanılarak programlı bir şekilde döndürebilirsiniz. Yüz algılama, bu öznitelik belirtirseniz (bkz [yüzleri algılamak nasıl](HowtoDetectFacesinImage.md)), daha sonra sorgu mümkün olacaktır. Aşağıdaki yöntemi [Bilişsel hizmetler yüz tanıma WPF](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/Cognitive-Services-Face-WPF) uygulama listesini alan **DetectedFace** nesneleri ve bir listesini döndürür **[yüz](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/app-samples/Cognitive-Services-Face-WPF/Sample-WPF/Controls/Face.cs)** nesneleri. **Yüz tanıma** depoları güncelleştirilmiş dikdörtgen koordinatları dahil olmak üzere veri, yüz tanıma, özel bir sınıf İşte. Yeni değerleri için hesaplanır **üst**, **sol**, **genişliği**, ve **yükseklik**ve yeni bir alan **FaceAngle**bir döndürme belirtir.

```csharp
/// <summary>
/// Calculate the rendering face rectangle
/// </summary>
/// <param name="faces">Detected face from service</param>
/// <param name="maxSize">Image rendering size</param>
/// <param name="imageInfo">Image width and height</param>
/// <returns>Face structure for rendering</returns>
public static IEnumerable<Face> CalculateFaceRectangleForRendering(IList<DetectedFace> faces, int maxSize, Tuple<int, int> imageInfo)
{
    var imageWidth = imageInfo.Item1;
    var imageHeight = imageInfo.Item2;
    var ratio = (float)imageWidth / imageHeight;
    int uiWidth = 0;
    int uiHeight = 0;
    if (ratio > 1.0)
    {
        uiWidth = maxSize;
        uiHeight = (int)(maxSize / ratio);
    }
    else
    {
        uiHeight = maxSize;
        uiWidth = (int)(ratio * uiHeight);
    }

    var uiXOffset = (maxSize - uiWidth) / 2;
    var uiYOffset = (maxSize - uiHeight) / 2;
    var scale = (float)uiWidth / imageWidth;

    foreach (var face in faces)
    {
        var left = (int)(face.FaceRectangle.Left * scale + uiXOffset);
        var top = (int)(face.FaceRectangle.Top * scale + uiYOffset);

        // Angle of face rectangles, default value is 0 (not rotated).
        double faceAngle = 0;

        // If head pose attributes have been obtained, re-calculate the left & top (X & Y) positions.
        if (face.FaceAttributes?.HeadPose != null)
        {
            // Head pose's roll value acts directly as the face angle.
            faceAngle = face.FaceAttributes.HeadPose.Roll;
            var angleToPi = Math.Abs((faceAngle / 180) * Math.PI);

            // _____       | / \ |
            // |____|  =>  |/   /|
            //             | \ / |
            // Re-calculate the face rectangle's left & top (X & Y) positions.
            var newLeft = face.FaceRectangle.Left +
                face.FaceRectangle.Width / 2 -
                (face.FaceRectangle.Width * Math.Sin(angleToPi) + face.FaceRectangle.Height * Math.Cos(angleToPi)) / 2;

            var newTop = face.FaceRectangle.Top +
                face.FaceRectangle.Height / 2 -
                (face.FaceRectangle.Height * Math.Sin(angleToPi) + face.FaceRectangle.Width * Math.Cos(angleToPi)) / 2;

            left = (int)(newLeft * scale + uiXOffset);
            top = (int)(newTop * scale + uiYOffset);
        }

        yield return new Face()
        {
            FaceId = face.FaceId?.ToString(),
            Left = left,
            Top = top,
            OriginalLeft = (int)(face.FaceRectangle.Left * scale + uiXOffset),
            OriginalTop = (int)(face.FaceRectangle.Top * scale + uiYOffset),
            Height = (int)(face.FaceRectangle.Height * scale),
            Width = (int)(face.FaceRectangle.Width * scale),
            FaceAngle = faceAngle,
        };
    }
}
```

## <a name="display-the-updated-rectangle"></a>Güncelleştirilmiş dikdörtgen görüntüleme

Buradan, döndürülen kullanabilirsiniz **yüz** ekranınıza nesneleri. Aşağıdaki satırları gelen [FaceDetectionPage.xaml](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/app-samples/Cognitive-Services-Face-WPF/Sample-WPF/Controls/FaceDetectionPage.xaml) yeni dikdörtgene bu verilerden nasıl oluşturulacağını gösterir:

```xaml
 <DataTemplate>
    <Rectangle Width="{Binding Width}" Height="{Binding Height}" Stroke="#FF26B8F4" StrokeThickness="1">
        <Rectangle.LayoutTransform>
            <RotateTransform Angle="{Binding FaceAngle}"/>
        </Rectangle.LayoutTransform>
    </Rectangle>
</DataTemplate>
```

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [Bilişsel hizmetler yüz tanıma WPF](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/Cognitive-Services-Face-WPF) Döndürülmüş yüz dikdörtgenlerin çalışan bir örnek için GitHub üzerindeki uygulama. Veya bakın [yüz tanıma API'si HeadPose örnek](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples) uygulamasını izler (nodding, sallayabilir) çeşitli baş hareketleri algılamak için gerçek zamanlı HeadPose öznitelik.