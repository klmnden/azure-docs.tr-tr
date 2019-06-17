---
title: HeadPose özniteliğini kullanma
titleSuffix: Azure Cognitive Services
description: HeadPose öznitelik yüz dikdörtgeni otomatik olarak döndürün veya bir video akışı baş hareketlerini algılamak için nasıl kullanılacağını öğrenin.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: sample
ms.date: 05/29/2019
ms.author: pafarley
ms.openlocfilehash: 168b4fce873206e39a32a83da3dc5509b431d6a1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67058583"
---
# <a name="use-the-headpose-attribute"></a>HeadPose özniteliğini kullanma

Bu kılavuzda, bazı önemli senaryoları etkinleştirmek için algılanan yüz HeadPose öznitelik nasıl kullanabileceğinizi görürsünüz.

## <a name="rotate-the-face-rectangle"></a>Yüz dikdörtgeni Döndür

Döndürülen algılanan her yüz tanıma, yüz dikdörtgeni resimdeki yüz boyutunu ve konumunu işaretler. Varsayılan olarak, dikdörtgeni görüntünün (yatay ve dikey alt kenarı) ile her zaman hizalanır; Bu, açılı yüzleri çerçeveleme için verimsiz olabilir. Program aracılığıyla bir görüntüdeki yüzleri kırpmak için istediğiniz durumlarda kırpmak için dikdörtgen döndürmek iyidir.

[Bilişsel hizmetler yüz tanıma WPF](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/Cognitive-Services-Face-WPF) örnek uygulama, algılanan yüz dikdörtgenler döndürmek için HeadPose özniteliğini kullanır.

### <a name="explore-the-sample-code"></a>Örnek kodu inceleyin

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

### <a name="display-the-updated-rectangle"></a>Güncelleştirilmiş dikdörtgen görüntüleme

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

## <a name="detect-head-gestures"></a>Baş hareket algılama

Baş hareketlerini nodding ve baş HeadPose değişiklikleri gerçek zamanlı izleme tarafından sallayabilir gibi algılayabilir. Bu özellik, bir özel canlılık algılayıcısı kullanabilirsiniz.

Canlılık algılama gerçek bir insanın ve olmayan bir resim veya video gösterimi bir konudur belirleme görevdir. Baş hareket algılayıcısı, özellikle bir görüntü temsili bir kişinin aksine kapsayıcısında eşdeğerlik doğrulamak amacıyla bir şekilde yanıt verebilir.

> [!CAUTION]
> Gerçek zamanlı olarak baş hareketlerini algılamak için bir yüksek oranda (saniye başına birden fazla) yüz tanıma API'sini çağırmak gerekir. Ücretsiz katman (f0) aboneliğiniz varsa, bu mümkün olmayacaktır. Bir Ücretli katman aboneliğiniz varsa, hızlı API'si maliyetlerini hesaplanan emin çağrıları head için hareket algılama.

Bkz: [yüz tanıma API'si HeadPose örnek](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/FaceAPIHeadPoseSample) kafasının çalışan bir örnek için GitHub üzerinde hareket algılama.

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [Bilişsel hizmetler yüz tanıma WPF](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/Cognitive-Services-Face-WPF) Döndürülmüş yüz dikdörtgenlerin çalışan bir örnek için GitHub üzerindeki uygulama. Veya bakın [yüz tanıma API'si HeadPose örnek](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples) baş hareketleri algılamak için gerçek zamanlı HeadPose öznitelik izleyen bir uygulama.