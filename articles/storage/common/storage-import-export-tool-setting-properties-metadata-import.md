---
title: Özellikleri ve Azure içeri/dışarı aktarma kullanarak meta verileri ayarlama | Microsoft Docs
description: Özellikleri ve meta verileri üzerinde hedef BLOB'ları Azure içeri/dışarı aktarma Aracı çalıştırırken sürücülerinizin hazırlamak için ayarlanacak belirleme konusunda bilgi edinin.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.subservice: common
ms.openlocfilehash: 3e2796d943fbc4a4ce99576de74b2fc5c77bcca9
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60320542"
---
# <a name="setting-properties-and-metadata-during-the-import-process"></a>İçeri aktarma işlemi sırasında özellikleri ve meta verileri ayarlama

Sürücülerinizi hazırlamak için Microsoft Azure içeri/dışarı aktarma aracı çalıştırdığınızda, özellikleri ve meta verileri hedef bloblarda ayarlanacak belirtebilirsiniz. Şu adımları uygulayın:

1.  BLOB özellikleri ayarlamak için yerel bilgisayarınızda özellik adlarını ve değerlerini belirten bir metin dosyası oluşturun.
2.  BLOB meta verilerini ayarlamak için meta veri adları ve değerleri belirtir, yerel bilgisayarınızda bir metin dosyası oluşturun.
3.  Bir parçası olarak birini veya her ikisini bu dosyaları Azure içeri/dışarı aktarma aracı için tam yolunu geçirin `PrepImport` işlemi.

> [!NOTE]
>  Özellikleri veya meta veri dosyasının bir kopyasını oturumun bir parçası olarak belirttiğinizde, bu kopya oturumu bir parçası olarak içeri her blob için bu özellikleri veya meta veriler ayarlanır. İçeri aktarılan blobları bazıları için farklı bir özellik veya meta veri kümesi belirtmek istiyorsanız, farklı özellikleri ya da meta dosyaları ile ayrı kopyasını oturum oluşturmak gerekir.

## <a name="specify-blob-properties-in-a-text-file"></a>Bir metin dosyasına BLOB özelliklerini belirtin

BLOB özelliklerini belirtmek için bir yerel metin dosyası oluşturun ve öğeleri ve özellik değeri olarak değerleri olarak özellik adlarını belirtir. XML içerir. Bazı özellik değerleri belirten bir örnek aşağıda verilmiştir:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Properties>
    <Content-Type>application/octet-stream</Content-Type>
    <Content-MD5>Q2hlY2sgSW50ZWdyaXR5IQ==</Content-MD5>
    <Cache-Control>no-cache</Cache-Control>
</Properties>
```

Dosyayı gibi yerel bir konuma kaydedin `C:\WAImportExport\ImportProperties.txt`.

## <a name="specify-blob-metadata-in-a-text-file"></a>BLOB meta verilerini bir metin dosyasına belirtin

Benzer şekilde, blob meta verilerini belirtmek için meta veri adlarının öğelerin ve meta veri değerlerini değerler olarak belirten bir yerel metin dosyası oluşturun. Bazı meta veri değerlerini belirten bir örnek aşağıda verilmiştir:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Metadata>
    <UploadMethod>Windows Azure Import/Export service</UploadMethod>
    <DataSetName>SampleData</DataSetName>
    <CreationDate>10/1/2013</CreationDate>
</Metadata>
```

Dosyayı gibi yerel bir konuma kaydedin `C:\WAImportExport\ImportMetadata.txt`.

## <a name="add-the-path-to-properties-and-metadata-files-in-datasetcsv"></a>Özellikleri ve meta veri dosyalarında dataset.csv yolu ekleyin

```
BasePath,DstBlobPathOrPrefix,BlobType,Disposition,MetadataFile,PropertiesFile
H:\Video\,https://mystorageaccount.blob.core.windows.net/video/,BlockBlob,rename,None,H:\mydirectory\properties.xml
H:\Photo\,https://mystorageaccount.blob.core.windows.net/photo/,BlockBlob,rename,None,H:\mydirectory\properties.xml
K:\Temp\FavoriteVideo.ISO,https://mystorageaccount.blob.core.windows.net/favorite/FavoriteVideo.ISO,BlockBlob,rename,None,H:\mydirectory\properties.xml
\\myshare\john\music\,https://mystorageaccount.blob.core.windows.net/music/,BlockBlob,rename,None,H:\mydirectory\properties.xml
```

## <a name="next-steps"></a>Sonraki adımlar

* [İçeri/Dışarı Aktarma hizmeti meta veriler ve özellikler dosyası biçimi](../storage-import-export-file-format-metadata-and-properties.md)
