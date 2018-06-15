---
title: Azure içeri/dışarı aktarma aracı sorunlarını giderme | Microsoft Docs
description: Azure içeri/dışarı aktarma aracı ve bunları nasıl ele alınacağını kullanılırken görülen yaygın sorunlardan bazıları hakkında bilgi edinin.
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: ''
ms.assetid: b91ca5eb-c557-460a-9afc-0590b38471f9
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
ms.openlocfilehash: 7bfda602dbc0ea47828a7c9243b8b9b09ec78432
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23873698"
---
# <a name="troubleshooting-the-azure-importexport-tool"></a>Azure İçeri/Dışarı Aktarma Aracı ile ilgili sorunları giderme
Sorunla çalıştırıyorsa, Microsoft Azure içeri/dışarı aktarma aracı hata iletilerini döndürür. Bu konuda, kullanıcıların içine çalışabilir bazı yaygın sorunlar listelenmiştir.  
  
## <a name="a-copy-session-fails-what-i-should-do"></a>Kopya oturumu başarısız oluyor, ne yapmalıyım?  
 Kopya oturumu başarısız olduğunda, iki seçenek vardır:  
  
 Örneğin bir ağ paylaşımı kısa bir süre için çevrimdışı ve şimdi yeniden çevrimiçi olduğundan yeniden denenebilir bir hatadır, kopya oturumu devam edebilirsiniz. Hata yeniden denenebilir, örneğin komut satırı parametrelerinizi yanlış kaynak dosyası dizini değilse, kopya oturumu abort gerekir. Bkz: [bir içeri aktarma işi için sabit sürücüler hazırlama](../storage-import-export-tool-preparing-hard-drives-import-v1.md) sürdürme ve kopyalama oturumları iptal etme hakkında daha fazla bilgi.  
  
## <a name="i-cant-resume-or-abort-a-copy-session"></a>I sürdürün veya bir kopya oturum iptal olamaz.  
 Bir sürücü için ilk kopyalama oturumun kopyalama oturumdur sonra hata iletisi belirtmeli: "ilk kopya oturumu durduruldu veya sürdürülemez." Bu durumda, eski günlük dosyasını silin ve komutu yeniden çalıştırın.  
  
 Kopya oturumu birinci bir sürücü için değilse, yeniden her zaman sürdürüldü durduruldu veya bırakılabilir.  
  
## <a name="i-lost-the-journal-file-can-i-still-create-the-job"></a>Günlük dosyası kayıp, iş oluşturabilirsiniz?  
 Bir sürücü için günlük dosyası bu sürücüye veri kopyalama tamamlandı bilgileri içerir ve sürücü için daha fazla dosya eklemek için gereken ve içe aktarma işi oluşturmak için kullanılabilir. Günlük dosyası kaybolursa, sürücü için tüm kopyalama oturumları Yinele gerekecektir.  
  
## <a name="next-steps"></a>Sonraki adımlar
 
* [Azure içeri/dışarı aktarma aracı ayarlama](../storage-import-export-tool-setup-v1.md)   
* [Sabit sürücüleri içeri aktarma işine hazırlama](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [Kopyalama günlük dosyalarıyla iş durumunu gözden geçirme](../storage-import-export-tool-reviewing-job-status-v1.md)   
* [Bir içeri aktarma işini onarma](../storage-import-export-tool-repairing-an-import-job-v1.md)   
* [Bir dışarı aktarma işini onarma](../storage-import-export-tool-repairing-an-export-job-v1.md)
