---
title: "Özellikleri ve Azure içeri/dışarı aktarma - v1 kullanarak meta verileri ayarlama | Microsoft Docs"
description: "Özellikler ve hedef BLOB'ları üzerinde Azure içeri/dışarı aktarma Aracı çalıştırırken sürücülerinizin hazırlamak için ayarlanacak meta veri belirtin öğrenin. İçeri/Dışarı Aktarma Aracı'nın v1 başvuruyor."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: e8541695-bcfb-4bf0-84d9-72c9de32a0a8
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 77bdaa5559de86cd1de9f30e70656e47fd5719e2
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
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
  
## <a name="create-a-copy-session-including-the-properties-or-metadata-files"></a>Özellikler veya meta veri dosyaları dahil olmak üzere bir kopya oturum oluşturma  
İçeri aktarma işi hazırlama Azure içeri/dışarı aktarma aracı çalıştırdığınızda, komut satırını kullanarak üzerinde özellikleri dosyası belirtin `PropertyFile` parametresi. Komut satırını kullanarak üzerinde meta veri dosyası belirtin `/MetadataFile` parametresi. Her iki dosya belirten örnek aşağıda verilmiştir:  
  
```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:BlueRayIso /srcfile:K:\Temp\BlueRay.ISO /dstblob:favorite/BlueRay.ISO /MetadataFile:c:\WAImportExport\SampleMetadata.txt /PropertyFile:c:\WAImportExport\SampleProperties.txt  
```
  
## <a name="next-steps"></a>Sonraki adımlar

* [İçeri/Dışarı Aktarma hizmeti meta veriler ve özellikler dosyası biçimi](../storage-import-export-file-format-metadata-and-properties.md)
