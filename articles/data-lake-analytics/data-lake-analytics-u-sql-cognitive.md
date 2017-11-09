---
title: "Azure Data Lake Analytics U-SQL Bilişsel özelliklerini kullanarak | Microsoft Docs"
description: "U-SQL Bilişsel yetenekleri Intelligence kullanmayı öğrenin"
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: jhubbard
editor: cgronlun
ms.assetid: 019c1d53-4e61-4cad-9b2c-7a60307cbe19
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: saveenr
ms.openlocfilehash: ec48a07af0aba78f2e508bad232f34102f0c2073
ms.sourcegitcommit: ce934aca02072bdd2ec8d01dcbdca39134436359
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2017
---
# <a name="tutorial-get-started-with-the-cognitive-capabilities-of-u-sql"></a>Öğretici: U-SQL Bilişsel yetenekleri ile çalışmaya başlama

## <a name="overview"></a>Genel Bakış
U-SQL için bilişsel özellikleri geliştiricilerin kendi büyük veri programlarda put Intelligence kullanmak etkinleştirin. 

Aşağıdaki bilişsel özellikleri kullanılabilir:
* Görüntüleme: yüz algılama
* Görüntüleme: duygu algılama
* Görüntüleme: (etiketleme) nesneleri Algıla
* Görüntüleme: OCR (optik karakter tanıma)
* Metin: Anahtar tümcecik ayıklama
* Metin: Düşünceleri analizi

## <a name="how-to-use-cognitive-in-your-u-sql-script"></a>U-SQL komut dosyanıza Cognitive kullanma

Genel işlem basittir:

* U-SQL komut dosyası için bilişsel özelliklerini etkinleştirmek için referans DERLEMESİNİ deyimi kullanın
* Bilişsel UDO bir çıktı oluşturmak üzere kullanarak bir giriş satır kümesi üzerinde işlem kullanmak satır kümesi

### <a name="detecting-objects-in-images"></a>Görüntülerde nesneleri algılama

Aşağıdaki örnek bilişsel yetenekleri nesneleri görüntülerinde algılamak için nasıl kullanılacağını gösterir.

```
REFERENCE ASSEMBLY ImageCommon;
REFERENCE ASSEMBLY FaceSdk;
REFERENCE ASSEMBLY ImageEmotion;
REFERENCE ASSEMBLY ImageTagging;
REFERENCE ASSEMBLY ImageOcr;

// Get the image data

@imgs =
    EXTRACT 
        FileName string, 
        ImgData byte[]
    FROM @"/usqlext/samples/cognition/{FileName}.jpg"
    USING new Cognition.Vision.ImageExtractor();

//  Extract the number of objects on each image and tag them 

@tags =
    PROCESS @imgs 
    PRODUCE FileName,
            NumObjects int,
            Tags SQL.MAP<string, float?>
    READONLY FileName
    USING new Cognition.Vision.ImageTagger();

@tags_serialized =
    SELECT FileName,
           NumObjects,
           String.Join(";", Tags.Select(x => String.Format("{0}:{1}", x.Key, x.Value))) AS TagsString
    FROM @tags;

OUTPUT @tags_serialized
    TO "/tags.csv"
    USING Outputters.Csv();
```
Daha fazla örnek için bakmak **U-SQL/Cognitive örnekleri** içinde **sonraki adımlar** bölümü.

## <a name="next-steps"></a>Sonraki adımlar
* [U-SQL/Bilişsel örnekleri](https://github.com/Azure-Samples?utf8=✓&q=usql%20cognitive)
* [Visual Studio için Data Lake Araçları'nı kullanarak U-SQL betikleri geliştirme](data-lake-analytics-data-lake-tools-get-started.md)
* [Azure Data Lake Analytics işleri için U-SQL penceresi işlevlerini kullanma](data-lake-analytics-use-window-functions.md)
