---
title: Özellikleri ve Azure içeri/dışarı aktarma kullanarak meta verileri ayarlama | Microsoft Docs
description: Özellikler ve hedef BLOB'ları üzerinde Azure içeri/dışarı aktarma Aracı çalıştırırken sürücülerinizin hazırlamak için ayarlanacak meta veri belirtin öğrenin.
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: ''
ms.assetid: ''
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 1ba6d157402fae0c7d7bf841d2b4e4f6b1ee1084
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23873719"
---
# <a name="setting-properties-and-metadata-during-the-import-process"></a>İçeri aktarma işlemi sırasında özellikleri ve meta verileri ayarlama

Sürücülerinizin hazırlamak üzere Microsoft Azure içeri/dışarı aktarma aracı çalıştırdığınızda, özellikleri ve hedef BLOB'ları üzerinde ayarlamak için meta veriler belirtebilirsiniz. Şu adımları uygulayın:

1.  BLOB özelliklerini ayarlamak için özellik adları ve değerleri belirtir, yerel bilgisayarınızda bir metin dosyası oluşturun.
2.  BLOB meta verileri ayarlamak için yerel bilgisayarınızda meta verileri adları ve değerleri bir metin dosyası oluşturun.
3.  Bir parçası olarak geçişi birini veya her ikisini Azure içeri/dışarı aktarma aracı için bu dosyalar için tam yolu `PrepImport` işlemi.

> [!NOTE]
>  Özellikler veya meta veri dosya kopyalama oturumun bir parçası belirttiğinizde, bu kopya oturumu bir parçası olarak alınan her blob için bu özellikleri veya meta veriler ayarlanır. İçeri aktarılan BLOB'ları bazıları için özellikleri veya meta veriler farklı bir kümesini belirtmek istiyorsanız, farklı özellikleri veya meta veri dosyaları ile ayrı kopya oturumu oluşturmanız gerekir.

## <a name="specify-blob-properties-in-a-text-file"></a>Bir metin dosyasına BLOB özellikleri belirtin

BLOB özelliklerini belirtmek için bir yerel metin dosyası oluşturun ve öğeleri ve değerleri olarak özellik değerleri olarak özellik adlarını belirtir XML içerir. Bazı özellik değerleri belirten örnek aşağıda verilmiştir:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Properties>
    <Content-Type>application/octet-stream</Content-Type>
    <Content-MD5>Q2hlY2sgSW50ZWdyaXR5IQ==</Content-MD5>
    <Cache-Control>no-cache</Cache-Control>
</Properties>
```

Dosya gibi yerel bir konuma kaydetmeniz `C:\WAImportExport\ImportProperties.txt`.

## <a name="specify-blob-metadata-in-a-text-file"></a>Bir metin dosyasına BLOB meta verileri belirtin

Benzer şekilde, blob meta verileri belirtmek için meta veri adlarının öğeleri ve meta veri değerlerinin değerler olarak olarak belirten bir yerel metin dosyası oluşturun. Bazı meta veri değerleri belirten örnek aşağıda verilmiştir:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Metadata>
    <UploadMethod>Windows Azure Import/Export service</UploadMethod>
    <DataSetName>SampleData</DataSetName>
    <CreationDate>10/1/2013</CreationDate>
</Metadata>
```

Dosya gibi yerel bir konuma kaydetmeniz `C:\WAImportExport\ImportMetadata.txt`.

## <a name="add-the-path-to-properties-and-metadata-files-in-datasetcsv"></a>Özellikler ve meta veri dosyalarında dataset.csv yolu ekleyin

```
BasePath,DstBlobPathOrPrefix,BlobType,Disposition,MetadataFile,PropertiesFile
H:\Video\,https://mystorageaccount.blob.core.windows.net/video/,BlockBlob,rename,None,H:\mydirectory\properties.xml
H:\Photo\,https://mystorageaccount.blob.core.windows.net/photo/,BlockBlob,rename,None,H:\mydirectory\properties.xml
K:\Temp\FavoriteVideo.ISO,https://mystorageaccount.blob.core.windows.net/favorite/FavoriteVideo.ISO,BlockBlob,rename,None,H:\mydirectory\properties.xml
\\myshare\john\music\,https://mystorageaccount.blob.core.windows.net/music/,BlockBlob,rename,None,H:\mydirectory\properties.xml
```

## <a name="next-steps"></a>Sonraki adımlar

* [İçeri/Dışarı Aktarma hizmeti meta veriler ve özellikler dosyası biçimi](../storage-import-export-file-format-metadata-and-properties.md)
