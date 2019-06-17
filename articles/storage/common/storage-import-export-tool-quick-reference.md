---
title: Azure içeri/dışarı aktarma aracı içeri aktarma işi komutlar için hızlı başvuru | Microsoft Docs
description: Sık kullanılan içeri aktarma işi komutları için Azure içeri/dışarı aktarma aracı komut başvurusu.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
ms.subservice: common
ms.openlocfilehash: 7d21ab42188d5b8d6cb8c179a28d1b5bee822f05
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61478428"
---
# <a name="quick-reference-for-frequently-used-commands-for-import-jobs"></a>İçeri aktarma işlerinde sık kullanılan komutlar için hızlı başvuru

Bu makalede, sık kullanılan komutlar için bazı hızlı başvuru sağlar. Ayrıntılı kullanım için bkz: [içeri aktarma işi için sabit disk hazırlama](../storage-import-export-tool-preparing-hard-drives-import.md).

## <a name="first-session"></a>İlk oturum

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1 /sk:************* /InitialDriveSet:driveset-1.csv /DataSet:dataset-1.csv /logdir:F:\logs
```

## <a name="second-session"></a>İkinci oturum

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2 /DataSet:dataset-2.csv
```

## <a name="abort-latest-session"></a>En son oturumunu durdur

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2 /AbortSession
```

## <a name="resume-latest-interrupted-session"></a>Son kesintiye oturumu Sürdür

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#3 /ResumeSession
```

## <a name="add-drives-to-latest-session"></a>En son oturumuna sürücü ekleme

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#3 /AdditionalDriveSet:driveset-2.csv
```

## <a name="next-steps"></a>Sonraki adımlar

* [Sabit sürücüleri içeri aktarma işine hazırlamak için örnek iş akışı](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow.md)
