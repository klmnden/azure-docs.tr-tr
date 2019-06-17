---
title: Hızlı başvuru için Azure içeri/dışarı aktarma aracı içeri aktarma işi komutları - v1 | Microsoft Docs
description: Sık kullanılan içeri aktarma işi komutları için Azure içeri/dışarı aktarma aracı komut başvurusu. Bu, içeri/dışarı aktarma Aracı'nın v1 başvurur.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
ms.subservice: common
ms.openlocfilehash: 44ee23a510bc15d5cb52a338be1652180b922db9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61478445"
---
# <a name="quick-reference-for-frequently-used-commands-for-import-jobs"></a>İçeri aktarma işlerinde sık kullanılan komutlar için hızlı başvuru
Bu bölümde, sık kullanılan komutlar için bazı hızlı başvuru sağlar. Ayrıntılı kullanım için bkz: [içeri aktarma işi için sabit disk hazırlama](../storage-import-export-tool-preparing-hard-drives-import-v1.md).  

## <a name="prepare-a-hard-drive-when-data-has-already-been-copied-to-the-hard-drive"></a>Verileri sabit sürücüye kopyalanan bir sabit sürücüsünü hazırlayın
 Veriler için zaten kopyalandı ancak henüz BitLocker ile şifrelenmiş değil, aşağıdaki komutu bir sabit sürücü hazırlar:  
  
```  
  WAImportExport.exe PrepImport /j:9WM35C2V.jrn /id:session#1 /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /t:d /encrypt /srcdir:d:\movies\drama /dstdir:movies/drama/ /skipwrite
```    

## <a name="copy-a-single-directory-to-a-hard-drive"></a>Tek bir dizin sabit sürücüye kopyalayın.  
 Aşağıdaki komut, tek bir kaynak dizin henüz BitLocker ile şifrelenmiş olmayan bir sabit sürücüye kopyalar:  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:movies /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:d:\Movies /dstdir:entertainment/movies/  
```  
  
## <a name="copy-two-directories-to-a-hard-drive"></a>Bir sabit sürücüye iki dizinleri kopyalama  
 İki kaynak dizinleri bir sürücüye kopyalamak için aşağıdaki komutları kullanın:  
  
 İlk komut, günlük dizini, depolama hesabı anahtarı, hedef sürücü harfini belirtir `format/encrypt` gereksinimleri ve ortak parametreleri:  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:movies /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:d:\Movies /dstdir:entertainment/movies/  
```  
  
 İkinci komut, günlük dosyası, yeni oturum kimliği ve kaynak ve hedef konumların belirtir:  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:music /srcdir:d:\Music /dstdir:entertainment/music/  
```  
  
## <a name="copy-a-large-file-to-a-hard-drive-in-a-second-copy-session"></a>Büyük boyutlu bir dosyayı bir sabit sürücü ikinci bir kopyası oturumda kopyalayın.  
 Aşağıdaki komut, bir önceki kopyalama oturumunda hazırlanan bir sabit sürücüye tek büyük bir dosya kopyalar:  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:dvd /srcfile:d:\dvd\favoritemovie.vhd /dstblob:dvd/favoritemovie.vhd  
```  
  
## <a name="next-steps"></a>Sonraki adımlar

* [Sabit sürücüleri içeri aktarma işine hazırlamak için örnek iş akışı](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow-v1.md)
