---
title: Azure içeri/dışarı aktarma aracı sorunlarını giderme | Microsoft Docs
description: Azure içeri/dışarı aktarma aracı ve bunları nasıl ele alınacağını kullanılırken görülen yaygın sorunların bazıları hakkında bilgi edinin.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
ms.subservice: common
ms.openlocfilehash: 9a4e47143515c7f9c21d701809c35d61853d91ec
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60320457"
---
# <a name="troubleshooting-the-azure-importexport-tool"></a>Azure İçeri/Dışarı Aktarma Aracı ile ilgili sorunları giderme
Bir sorunla karşılaşırsanız çalıştırıyorsa, Microsoft Azure içeri/dışarı aktarma aracı, hata iletileri döndürür. Bu konuda, kullanıcıların içine çalışabilir bazı yaygın sorunlar listelenmiştir.  
  
## <a name="a-copy-session-fails-what-i-should-do"></a>Bir kopya oturumu başarısız ne yapmalıyım?  
 Bir kopya oturumu başarısız olduğunda, iki seçenek vardır:  
  
 Örneğin bir ağ paylaşımı için kısa bir süre çevrimdışı ve şimdi yeniden çevrimiçi olana yeniden denenebilir bir hatadır, kopya oturumu devam edebilir. Hata yeniden denenebilir, komut satırı parametreleri yanlış kaynak dosyası dizini belirtilen örneğin değilse, kopya oturumu iptal etmek gerekir. Bkz: [içeri aktarma işi için sabit disk hazırlama](../storage-import-export-tool-preparing-hard-drives-import-v1.md) kopyalama oturumları iptal ediliyor ve sürdürme hakkında daha fazla bilgi.  
  
## <a name="i-cant-resume-or-abort-a-copy-session"></a>Ben sürdürmek veya kopya oturumu iptal değiştirilemez.  
 Kopya oturumu ilk kopyalama oturum için bir sürücü varsa, hata iletisi olması gerekir: "İlk kopya oturumu durduruldu veya sürdürülemez." Bu durumda, eski günlük dosyasını silin ve komutu yeniden çalıştırın.  
  
 Bir kopya oturumu İlki bir sürücü için değilse, bu her zaman sürdürüldü durduruldu veya.  
  
## <a name="i-lost-the-journal-file-can-i-still-create-the-job"></a>Ben günlük dosyası kayıp, işi oluşturabilirsiniz?  
 Sürücü için günlük dosyası bu sürücüye veri kopyalama ilişkin tüm bilgileri içerir ve daha fazla dosya sürücü eklemek için gereken ve içeri aktarma işi oluşturmak için kullanılır. Günlük dosyası kaybolursa, sürücü için tüm kopyalama oturumları Yinele gerekecektir.  
  
## <a name="next-steps"></a>Sonraki adımlar
 
* [Azure içeri/dışarı aktarma Aracı'nı ayarlama](../storage-import-export-tool-setup-v1.md)   
* [Sabit sürücüleri içeri aktarma işine hazırlama](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [Kopyalama günlük dosyalarıyla iş durumunu gözden geçirme](../storage-import-export-tool-reviewing-job-status-v1.md)   
* [Bir içeri aktarma işini onarma](../storage-import-export-tool-repairing-an-import-job-v1.md)   
* [Bir dışarı aktarma işini onarma](../storage-import-export-tool-repairing-an-export-job-v1.md)
