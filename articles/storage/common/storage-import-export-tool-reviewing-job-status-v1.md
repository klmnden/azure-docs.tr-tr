---
title: Azure içeri/dışarı aktarma iş durumu - v1 gözden geçirme | Microsoft Docs
description: İçeri/dışarı aktarma işinin durumunu görmek için içe veya dışa aktarma işlemi çalıştırıldığında oluşturulan günlük dosyalarını kullanmayı öğrenin.
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: ''
ms.assetid: c69d1d69-6403-4eee-9949-0185faeecfa1
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2017
ms.author: muralikk
ms.openlocfilehash: bdb30bc28c36ab9e969efc8be3b87b97e4027b39
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23873712"
---
# <a name="reviewing-azure-importexport-job-status-with-copy-log-files"></a>Azure içeri/dışarı aktarma iş durumu kopyalama günlük dosyalarıyla gözden geçirme
Microsoft Azure içeri/dışarı aktarma hizmeti bir içe veya dışa aktarma işi ile ilişkili sürücüleri işlediğinde, depolama hesabı için günlük dosyalarını veya içinden, içeri aktarma veya BLOB'ları dışarı aktarma kopyalama yazar. Günlük dosyası, dışarı veya içeri aktarılan her dosya hakkında ayrıntılı durumunu içerir. Tamamlanmış iş durumunu sorguladığınızda her kopya günlük dosyası için bir URL döndürülür; bkz: [alma işi](/rest/api/storageservices/Get-Job3) daha fazla bilgi için.  

## <a name="example-urls"></a>Örnek URL'leri

Örnek URL'ler için iki sürücü içeri aktarma işlemiyle kopyalama günlük dosyalarını şunlardır:  
  
 `http://myaccount.blob.core.windows.net/ImportExportStatesPath/waies/myjob_9WM35C2V_20130921-034307-902_error.xml`  
  
 `http://myaccount.blob.core.windows.net/ImportExportStatesPath/waies/myjob_9WM45A6Q_20130921-042122-021_error.xml`  
  
 Bkz: [içeri/dışarı aktarma hizmeti günlük dosyası biçimi](../storage-import-export-file-format-log.md) günlükleri ve durum kodları tam listesi biçimi için.  
  
## <a name="next-steps"></a>Sonraki adımlar
 
 * [Azure içeri/dışarı aktarma aracı ayarlama](storage-import-export-tool-setup-v1.md)   
 * [Sabit sürücüleri içeri aktarma işine hazırlama](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
 * [Bir içeri aktarma işini onarma](../storage-import-export-tool-repairing-an-import-job-v1.md)   
 * [Bir dışarı aktarma işini onarma](../storage-import-export-tool-repairing-an-export-job-v1.md)   
 * [Azure İçeri/Dışarı Aktarma Aracı ile ilgili sorunları giderme](storage-import-export-tool-troubleshooting-v1.md)
