---
title: Bir Azure içeri/dışarı aktarma dışarı aktarma işinin - v1 için sürücü kullanımı Önizleme | Microsoft Docs
description: Azure içeri/dışarı aktarma hizmetinde dışa aktarma işi için seçtiğiniz BLOB'ları listesi Önizleme öğrenin.
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: ''
ms.assetid: 7707d744-7ec7-4de8-ac9b-93a18608dc9a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
ms.openlocfilehash: 6ec74ae0b0931f3fed99a43f4f7e58f9d425b138
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23873649"
---
# <a name="previewing-drive-usage-for-an-export-job"></a>Dışarı aktarma işi için sürücü kullanımının önizlemesini yapma
Bir dışarı aktarma işinin oluşturmadan önce BLOB'ları dışarı kümesini seçmeniz gerekir. Microsoft Azure içeri/dışarı aktarma hizmeti, blob yolların listesini kullanın veya seçtiğiniz BLOB'ları temsil etmek için önekleri blob olanak sağlar.  
  
Ardından, göndermesi gerekir. kaç tane sürücüleri belirlemeniz gerekir. İçeri/dışarı aktarma aracı sağlar `PreviewExport` bulacağınızı kullanmak için seçtiğiniz BLOB tabanlı için sürücüleri boyutuna sürücü kullanımı önizlemesini görmek için komutu.

## <a name="command-line-parameters"></a>Komut satırı parametreleri

Kullanırken aşağıdaki parametreleri kullanabilirsiniz `PreviewExport` içeri/dışarı aktarma aracı komutu.

|Komut satırı parametresi|Açıklama|  
|--------------------------|-----------------|  
|**/ LOGDIR:**< LogDirectory\>|İsteğe bağlı. Günlük dosyası dizini. Bu dizin için ayrıntılı günlük dosyalarına yazılır. Günlük dizini belirtilmezse, geçerli dizin günlük dizini olarak kullanılır.|  
|**/sn:**< StorageAccountName\>|Gereklidir. Dışa aktarma işi için depolama hesabı adı.|  
|**/SK:**< StorageAccountKey\>|Bir kapsayıcı SAS varsa ve yalnızca belirtilmemişse gereklidir. Dışa aktarma işi için depolama hesabı için hesap anahtarı.|  
|**/csas:**< ContainerSas\>|Bir depolama hesabı anahtarı varsa ve yalnızca belirtilmemişse gerekli. BLOB'ları dışarı aktarma işinin dışarı aktarılmasına izin listesi için kapsayıcı SAS.|  
|**/ ExportBlobListFile:**< ExportBlobListFile\>|Gereklidir. XML yolu içeren blob yollar listesi dosya veya yol önekleri verilecek BLOB'ları için blob. Kullanılan dosya biçimi `BlobListBlobPath` öğesinde [Put işlemini](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) içeri/dışarı aktarma hizmeti REST API'si işlemi.|  
|**/ DriveSize:**< DriveSize\>|Gereklidir. Bir dışarı aktarma işi için kullanılacak sürücüleri boyutunu *ör*, 500 GB, 1,5 TB.|  

## <a name="command-line-example"></a>Komut satırı örneği

Aşağıdaki örnekte gösterilmiştir `PreviewExport` komutu:  
  
```  
WAImportExport.exe PreviewExport /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /ExportBlobListFile:C:\WAImportExport\mybloblist.xml /DriveSize:500GB    
```  
  
Dışarı aktarma blob listeyi dosyası blob adları içeren ve önekleri, aşağıda gösterildiği gibi blob olabilir:  
  
```xml 
<?xml version="1.0" encoding="utf-8"?>  
<BlobList>  
<BlobPath>pictures/animals/koala.jpg</BlobPath>  
<BlobPathPrefix>/vhds/</BlobPathPrefix>  
<BlobPathPrefix>/movies/</BlobPathPrefix>  
</BlobList>  
```

Azure içeri/dışarı aktarma aracı verilecek tüm BLOB'ları listeler ve gerekli tüm ek yükü dikkate alarak belirtilen boyutu, sürücü halinde paketlemek nasıl hesaplar, sonra BLOB'ları ve sürücü kullanım bilgilerini tutmak için gerekli sürücüleri sayısını tahmin eder.  
  
Atlanmış bilgilendirme günlükleriyle çıktısı örneği şöyledir:  
  
```  
Number of unique blob paths/prefixes:   3  
Number of duplicate blob paths/prefixes:        0  
Number of nonexistent blob paths/prefixes:      1  
  
Drive size:     500.00 GB  
Number of blobs that can be exported:   6  
Number of blobs that cannot be exported:        2  
Number of drives needed:        3  
        Drive #1:       blobs = 1, occupied space = 454.74 GB  
        Drive #2:       blobs = 3, occupied space = 441.37 GB  
        Drive #3:       blobs = 2, occupied space = 131.28 GB    
```  
  
## <a name="next-steps"></a>Sonraki adımlar

* [Azure içeri/dışarı aktarma aracı başvurusu](../storage-import-export-tool-how-to-v1.md)
