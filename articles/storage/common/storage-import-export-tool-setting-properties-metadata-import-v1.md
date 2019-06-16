---
title: Özellikleri ve Azure içeri/dışarı aktarma - v1 kullanarak meta verileri ayarlama | Microsoft Docs
description: Özellikleri ve meta verileri üzerinde hedef BLOB'ları Azure içeri/dışarı aktarma Aracı çalıştırırken sürücülerinizin hazırlamak için ayarlanacak belirleme konusunda bilgi edinin. Bu, içeri/dışarı aktarma Aracı'nın v1 başvurur.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.subservice: common
ms.openlocfilehash: 66ae7045cfb94ec70f3b14046af736f784b88ea6
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60320559"
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
  
## <a name="create-a-copy-session-including-the-properties-or-metadata-files"></a>Meta veri dosyaları ve özellikler dahil olmak üzere bir kopya oturumu oluşturma  
İçeri aktarma işine hazırlamak için Azure içeri/dışarı aktarma aracı çalıştırdığınızda, komut satırı kullanarak Özellikler dosyasını belirtin `PropertyFile` parametresi. Komut satırı kullanarak meta verileri dosyasını belirtin `/MetadataFile` parametresi. Her iki dosyayı belirten bir örnek aşağıda verilmiştir:  
  
```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:BlueRayIso /srcfile:K:\Temp\BlueRay.ISO /dstblob:favorite/BlueRay.ISO /MetadataFile:c:\WAImportExport\SampleMetadata.txt /PropertyFile:c:\WAImportExport\SampleProperties.txt  
```
  
## <a name="next-steps"></a>Sonraki adımlar

* [İçeri/Dışarı Aktarma hizmeti meta veriler ve özellikler dosyası biçimi](../storage-import-export-file-format-metadata-and-properties.md)
