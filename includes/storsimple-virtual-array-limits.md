---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 658fd9178495f14274c85eab2129c9dcd3be7693
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66143286"
---
| **Sınır tanımlayıcı** | **Sınırı** | **Yorumlar** |
| --- | --- | --- |
| Toplam Kapasite (bulut dahil) |Sanal cihaz başına 64 TB'a kadar |Tam bir StorSimple Virtual Array için başka bir boş dizi devredebilir. Aynı cihaza geri yüklemeye çalışırsanız, bu işlemi tamamlamak için cihazda yeterli alan olduğundan emin olun. 32 TB'a aştınız sonra aynı cihaza geri yükleyemezsiniz. |
| Depolama hesabı kimlik bilgilerini cihaz başına en fazla sayısı |1 | |
| Birimleri/paylaşımları sayısı |16 | |
| En düşük katmanlı bir paylaşım boyutu |500 GB | |
| En düşük katmanlı birim boyutu |500 GB | |
| En çok katmanlı bir paylaşım boyutu |20 TB | |
| Katmanlı birim en büyük boyutu |5 TB | |
| Yerel olarak sabitlenmiş bir paylaşım boyutu için alt |50 GB | |
| Yerel olarak sabitlenmiş bir birim, en küçük boyut |50 GB | |
| Yerel olarak sabitlenmiş bir paylaşımı en büyük boyutu |2 TB | |
| Yerel olarak sabitlenmiş bir birim, en büyük boyutu |200 GB | |
| İSCSI başlatıcılarının bağlantılarından sayısı |512 | |
| Erişim denetimi kayıtları cihaz başına en fazla sayısı |64 | |
| En fazla sanal cihazı tarafından korunan yedekleme sayısına *.backups* dosya sunucusu için klasör |5 |Bu en son içerir (varsayılan yedekleme İlkesi tarafından oluşturulan) zamanlanmış ve el ile yedeklemeler. |
| Cihaz tarafından korunan zamanlanmış yedekleme sayısı |55 |30 günlük yedeklemeler<br>12 aylık yedeklemeler<br>13 yıllık yedeklemeler |
| Cihaz tarafından korunan el ile yedekleme sayısı |45 | |
| Bir dosya sunucusu için dosya paylaşımı başına en fazla sayısı |1 milyon |Cihaz geri yükleme gerçekleştirirken, geri yükleme sürelerini cihazdaki tüm paylaşımlar arasında dosya sayısı için orantılıdır. |
| Birim başına bir dosya bir iSCSI sunucusu için en yüksek sayısı |1 milyon | |
| Sanal dizi başına en fazla sayısı |4 milyonluk | |
| Kurtarma zamanı geri yükleme |Hızlı geri yükleme |Geri yükleme ısı Haritası üzerinde bağlıdır ve birimin boyutuna bağlıdır.<br>Yedekleme işlemleri, bir geri yükleme işlemi devam ederken ortaya çıkabilir. |

