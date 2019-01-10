---
title: Azure Media Services ile bir küçük resim sprite oluştur | Microsoft Docs
description: Bu konuda, Azure Media Services ile bir küçük resim sprite oluşturma adımları anlatılmaktadır.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 09/24/2018
ms.author: juliako
ms.openlocfilehash: 93222129b80592ef5b4e1ed2e1420d975fe9f108
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2019
ms.locfileid: "54190754"
---
# <a name="generate-a-thumbnail-sprite"></a>Küçük resim sprite oluştur 

VTT dosyası ile birlikte tek bir (büyük) görüntüsü birbirine birleştirilmiş birden çok küçük çözümleme küçük resimleri içeren bir JPEG dosyası olan bir küçük resim sprite oluşturmak için Media Encoder Standard kullanabilirsiniz. Bu VTT dosyası boyutu ve büyük JPEG dosyası içinde küçük resim koordinatlarını birlikte her küçük resmi temsil eden giriş videosunun zaman aralığını belirtir. Video oynatıcı VTT dosyası ve sprite görüntünün geri temizleme sırasında bir Görüntüleyici ile görsel geri bildirim sağlayarak bir 'visual' seekbar göstermek ve video zaman çizelgesi boyunca iletmek için kullanın.

Küçük resim Sprite Önayarı oluşturmak için Medya Kodlayıcısı standart'ı kullanmak için:

1. JPG küçük resim biçimi kullanmanız gerekir
2. Başlangıç/adım/aralık değerleri olarak ya da zaman damgaları veya % değerleri (ve çerçeve sayısı değil) belirtmeniz gerekir 
    
    1. Zaman damgaları ve % değerleri karıştırmak uygundur

3. SpriteColumn değeri 1'e eşit veya daha büyük bir negatif olmayan sayı olarak olmalıdır

    1. SpriteColumn için M olarak ayarlanmışsa > = 1, M sütunlara sahip bir dikdörtgen çıkış görüntüsüdür. #2 oluşturulan küçük sayı bir tam katı m değil, son satır eksik ve siyah piksel ile sol olacaktır.  

Örnek aşağıda verilmiştir:

```json
{
    "Version": 1.0,
    "Codecs": [
    {
      "Start": "00:00:01",
      "Type": "JpgImage",
      "Step": "5%",
      "Range": "100%",
      "JpgLayers": [
        {
          "Type": "JpgLayer",
          "Width": "10%",
          "Height": "10%",
          "Quality": 90
        }
      ],
      "SpriteColumn": 10
    }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "JpgFormat"
          }
        }
   ]
}
```

## <a name="known-issues"></a>Bilinen Sorunlar

1.  Görüntü tek bir satır içeren bir sprite görüntüsü oluşturmak mümkün değildir (SpriteColumn görüntüdeki tek bir sütunla 1 sonuçları =).
2.  Orta boyutlu JPEG görüntüleri sprite görüntülerini Öbekleme henüz desteklenmiyor. Bu nedenle, sonuç birleştirilmiş küçük resim Sprite 8 M piksel geçici bir çözüm ya da daha az olması küçük resimler ve bunların boyutu, sayısını sınırlamak için dikkatli olunmalıdır.
3.  Azure Media Player, Microsoft Edge, Chrome ve Firefox tarayıcılarda dokunuşunu destekler. VTT ayrıştırma ıe11'de desteklenmiyor.

## <a name="next-steps"></a>Sonraki adımlar

[İçerik kodlama](media-services-encode-asset.md)