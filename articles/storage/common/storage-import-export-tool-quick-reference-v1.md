---
title: "Hızlı başvuru için Azure içeri/dışarı aktarma aracı alma işi komutları - v1 | Microsoft Docs"
description: "Sık kullanılan alma işi komutları için Azure içeri/dışarı aktarma aracı komut başvurusu. İçeri/Dışarı Aktarma Aracı'nın v1 başvuruyor."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
ms.openlocfilehash: 370cf6fae7ae106e8341f65086c8b8187d335746
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="quick-reference-for-frequently-used-commands-for-import-jobs"></a>İçeri aktarma işlerinde sık kullanılan komutlar için hızlı başvuru
Bu bölüm, sık kullanılan komutlar için bazı hızlı başvuru sağlar. Ayrıntılı kullanımı için bkz: [bir içeri aktarma işi için sabit sürücüler hazırlama](../storage-import-export-tool-preparing-hard-drives-import-v1.md).  

## <a name="prepare-a-hard-drive-when-data-has-already-been-copied-to-the-hard-drive"></a>Verileri sabit sürücüye kopyalanan bir sabit sürücü hazırlama
 Veriler için zaten kopyalandı ancak henüz BitLocker ile şifrelenmiş değil, aşağıdaki komutu bir sabit sürücü hazırlar:  
  
```  
  WAImportExport.exe PrepImport /j:9WM35C2V.jrn /id:session#1 /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /t:d /encrypt /srcdir:d:\movies\drama /dstdir:movies/drama/ /skipwrite
```    

## <a name="copy-a-single-directory-to-a-hard-drive"></a>Bir sabit sürücüye tek bir dizin kopyalama  
 Aşağıdaki komut, henüz BitLocker ile şifrelenmiş olmayan bir sabit sürücü için tek bir kaynak dizin kopyalar:  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:movies /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:d:\Movies /dstdir:entertainment/movies/  
```  
  
## <a name="copy-two-directories-to-a-hard-drive"></a>Bir sabit sürücüye iki dizinleri kopyalama  
 İki kaynak dizini bir sürücüye kopyalamak için aşağıdaki komutları kullanın:  
  
 İlk komut, günlük dizini, depolama hesabı anahtarı, hedef sürücü harfini belirtir `format/encrypt` gereksinimleri ve ortak Parametreler:  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:movies /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:d:\Movies /dstdir:entertainment/movies/  
```  
  
 İkinci komut, günlük dosyası, yeni bir oturum kimliği ve kaynak ve hedef konumların belirtir:  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:music /srcdir:d:\Music /dstdir:entertainment/music/  
```  
  
## <a name="copy-a-large-file-to-a-hard-drive-in-a-second-copy-session"></a>Büyük bir dosya için bir sabit sürücü ikinci bir kopyası oturumda kopyalayın.  
 Aşağıdaki komut, önceki bir kopya oturumunda hazırlanan bir sabit diske tek büyük bir dosya kopyalar:  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:dvd /srcfile:d:\dvd\favoritemovie.vhd /dstblob:dvd/favoritemovie.vhd  
```  
  
## <a name="next-steps"></a>Sonraki adımlar

* [Sabit sürücüleri içeri aktarma işine hazırlamak için örnek iş akışı](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow-v1.md)
