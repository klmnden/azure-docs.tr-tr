---
title: Azure içeri/dışarı aktarma işi durumu - v1 gözden geçirme | Microsoft Docs
description: İçeri/dışarı aktarma işinin durumunu görmek için içeri veya dışarı aktarma işi çalıştırdığınızda oluşturulan günlük dosyalarını kullanmayı öğrenin.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/26/2017
ms.author: muralikk
ms.subservice: common
ms.openlocfilehash: 306e3ccf19ba8db2de01e4b20a52707215a4a040
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60320715"
---
# <a name="reviewing-azure-importexport-job-status-with-copy-log-files"></a>Kopyalama günlük dosyalarıyla Azure içeri/dışarı aktarma iş durumunu gözden geçirme
Microsoft Azure içeri/dışarı aktarma hizmeti sürücüler içeri veya dışarı aktarma işi ile ilişkili işlediğinde, günlük dosyaları için depolama hesabına veya içinden, içeri aktarma veya BLOB'ları dışarı aktarma kopyalama yazar. Günlük dosyası dışarı veya içeri aktarılan her dosya hakkında ayrıntılı durum içerir. Tamamlanan iş durumunu sorguladığınızda her kopya günlük dosyasının URL'si döndürülür; bkz: [alma işi](https://docs.microsoft.com/rest/api/storageimportexport/Jobs/Get) daha fazla bilgi için.  

## <a name="example-urls"></a>Örnek URL'ler

Kopyalama günlük dosyaları için iki sürücüsü ile içeri aktarma işi için örnek URL'ler şunlardır:  
  
 `http://myaccount.blob.core.windows.net/ImportExportStatesPath/waies/myjob_9WM35C2V_20130921-034307-902_error.xml`  
  
 `http://myaccount.blob.core.windows.net/ImportExportStatesPath/waies/myjob_9WM45A6Q_20130921-042122-021_error.xml`  
  
 Bkz: [içeri/dışarı aktarma hizmeti günlük dosyası biçimi](../storage-import-export-file-format-log.md) biçimi kopyalama günlüklerini ve durum kodları tam listesi için.  
  
## <a name="next-steps"></a>Sonraki adımlar
 
 * [Azure içeri/dışarı aktarma Aracı'nı ayarlama](storage-import-export-tool-setup-v1.md)   
 * [Sabit sürücüleri içeri aktarma işine hazırlama](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
 * [Bir içeri aktarma işini onarma](../storage-import-export-tool-repairing-an-import-job-v1.md)   
 * [Bir dışarı aktarma işini onarma](../storage-import-export-tool-repairing-an-export-job-v1.md)   
 * [Azure İçeri/Dışarı Aktarma Aracı ile ilgili sorunları giderme](storage-import-export-tool-troubleshooting-v1.md)
