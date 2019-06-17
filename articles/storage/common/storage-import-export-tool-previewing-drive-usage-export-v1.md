---
title: Azure içeri/dışarı aktarma dışarı aktarma işi - v1 için sürücü kullanımının önizlemesini yapma | Microsoft Docs
description: Azure içeri/dışarı aktarma hizmeti, dışarı aktarma işi için seçtiğiniz BLOB listesini Önizleme öğrenin.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
ms.subservice: common
ms.openlocfilehash: 53ab1e28c5864b403d52bf5e73f0c5c41b8f18a8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61478462"
---
# <a name="previewing-drive-usage-for-an-export-job"></a>Dışarı aktarma işi için sürücü kullanımının önizlemesini yapma
Dışarı aktarma işi oluşturmadan önce BLOB'ları dışarı aktarılmasına izin kümesi seçin gerekir. Microsoft Azure içeri/dışarı aktarma hizmeti, blob yollarının listesini kullanın veya seçtiğiniz blobları temsil etmek için ön ekleri blob olanak tanır.  
  
Ardından, göndermem gerekiyor kaç sürücüleri belirlemek gerekir. İçeri/dışarı aktarma aracı sağlar `PreviewExport` komutu, seçtiğiniz, BLOB tabanlı için sürücüleri boyutuna sürücü kullanımının önizlemesini görüntülemek için kullanılacak seçeceğiz.

## <a name="command-line-parameters"></a>Komut satırı parametreleri

Kullanırken aşağıdaki parametreleri kullanabilirsiniz `PreviewExport` içeri/dışarı aktarma Aracı'nın komutu.

|Komut satırı parametresi|Açıklama|  
|--------------------------|-----------------|  
|**/ LOGDIR:** < LogDirectory\>|İsteğe bağlı. Günlük dizini. Ayrıntılı günlük dosyası bu dizine yazılır. Hiçbir günlük dizini belirtilmezse, geçerli dizin günlük dizini kullanılır.|  
|**/sn:** < StorageAccountName\>|Gereklidir. Dışarı aktarma işi için depolama hesabı adı.|  
|**/sk:** <StorageAccountKey\>|Bir kapsayıcı SAS belirtilmedi ve yalnızca, gerekli. Dışarı aktarma işi için depolama hesabı için hesap anahtarı.|  
|**/csas:** <ContainerSas\>|Depolama hesabı anahtarı belirtilmedi ve yalnızca, gerekli. Dışarı aktarma işi verilecek blobları listeleme kapsayıcısı SAS.|  
|**/ ExportBlobListFile:** < ExportBlobListFile\>|Gereklidir. XML yolu içeren blob yollarının listesini dosya veya yol önekleri dışarı aktarılacak bloblar için blob. Kullanılan dosya biçimi `BlobListBlobPath` öğesinde [Put işlemini](/rest/api/storageimportexport/jobs) içeri/dışarı aktarma hizmeti REST API işlemi.|  
|**/ DriveSize:** < DriveSize\>|Gereklidir. Bir dışarı aktarma işi için kullanılacak sürücüleri boyutunu *örn*, 500 GB, 1,5 TB.|  

## <a name="command-line-example"></a>Komut satırı örneği

Aşağıdaki örnek, gösterir `PreviewExport` komutu:  
  
```  
WAImportExport.exe PreviewExport /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /ExportBlobListFile:C:\WAImportExport\mybloblist.xml /DriveSize:500GB    
```  
  
Dışarı aktarma blob listesi dosyası blob adları içeren ve ön ekleri, burada gösterildiği gibi blob:  
  
```xml 
<?xml version="1.0" encoding="utf-8"?>  
<BlobList>  
<BlobPath>pictures/animals/koala.jpg</BlobPath>  
<BlobPathPrefix>/vhds/</BlobPathPrefix>  
<BlobPathPrefix>/movies/</BlobPathPrefix>  
</BlobList>  
```

Azure içeri/dışarı aktarma aracı aktarılabilmesi için tüm blobları listeler ve gerekli tüm ek yükü dikkate alarak, belirtilen boyut sürücülere paketlenecek nasıl hesaplar ve ardından sürücü kullanım bilgilerini ve BLOB'ları beklemesi gereken sürücü sayısını tahmin eder.  
  
Atlanmış bilgilendirici günlükleri ile çıktının bir örneği aşağıda verilmiştir:  
  
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
