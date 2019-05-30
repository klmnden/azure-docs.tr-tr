---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 93f4f74d435cc14130668da102d1246c5fad5872
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66238807"
---
| Sınır tanımlayıcı | Sınır | Açıklamalar |
| --- | --- | --- |
| Depolama hesabı kimlik bilgileri sayısı |64 | |
| Birim kapsayıcılarının sayısı |64 | |
| En fazla birim sayısı |255 | |
| Zamanlamalar bant genişliği şablonu başına en fazla sayısı |168 |Her saat, haftanın her günü için bir zamanlama belirleyin. |
| Katmanlı birim fiziksel cihazlarda en yüksek boyutu |StorSimple 8100 ve StorSimple 8600 64 TB |StorSimple 8100 ve StorSimple 8600 fiziksel cihazlardır. |
| Azure sanal cihazda katmanlı birim en büyük boyutu |Storsimple 8010'un 30 TB <br></br> StorSimple 8020 için 64 TB |StorSimple 8010 ve 8020 StorSimple sırasıyla standart depolama ve Premium depolama kullanan sanal azure'da cihazlardır. |
| Fiziksel cihazlarda yerel olarak sabitlenmiş bir birim, en büyük boyutu |StorSimple 8100 için 9 TB <br></br> StorSimple 8600 için 24 TB |StorSimple 8100 ve StorSimple 8600 fiziksel cihazlardır. |
| İSCSI bağlantı sayısı üst sınırı |512 | |
| İSCSI başlatıcılarının bağlantılarından sayısı |512 | |
| Erişim denetimi kayıtları cihaz başına en fazla sayısı |64 | |
| Yedekleme İlkesi başına birim sayısı |24 | |
| Yedekleme İlkesi korunan yedekleme sayısı |64 | |
| Yedekleme İlkesi başına zamanlamaları sayısı |10 | |
| Birim başına korunabilir herhangi bir türde anlık görüntü sayısı |256 |Bu miktar yerel anlık görüntüleri içerir ve bulut anlık görüntüleri. |
| İçinde herhangi bir cihazda mevcut olabilecek anlık görüntü sayısı |10,000 | |
| En fazla sayıda paralel yedekleme, geri yükleme için işlenen ya da kopyalama |16 |<ul><li>16'dan fazla birim varsa, işleme yuvaları kullanılabilir oldukça, sıralı olarak işlenen.</li><li>İşlemi tamamlanana kadar kopyalanmış bir yeni yedeklerini veya geri yüklenen bir katmanlı birim oluşamaz. Yerel bir birim için birim çevrimiçi olduktan sonra yedeklemeler izin verilir.</li></ul> |
| Geri yükleme ve kopyalama, katmanlı birimlerin zaman Kurtar |< 2 dakika |<ul><li>Bir geri yükleme ya da kopyalama işlemi, birim boyutu ne olursa olsun 2 dakika içinde birim kullanılabilir hale getirilir.</li><li>Toplu performans başlangıçta çoğu verileri ve meta veriler yine de bulunduğu olarak bulutta normalden daha yavaş olabilir. StorSimple cihazına buluttan veri akışları olarak performansı artırabilir.</li><li>Meta verileri indirmek için toplam süreyi ayrılmış birimin boyutuna bağlıdır. Meta verileri, cihaz arka planda hızında TB veri ayrılmış birim başına 5 dakika içinde otomatik olarak getirilir. Bu oran, Internet bant genişliği buluta tarafından etkilenebilir.</li><li>Tüm meta verileri cihaz üzerinde olduğunda geri yükleme veya kopyalama işlemi tamamlanmıştır.</li><li>Kopyalama işlemi tam olarak bitmiş durumda veya yedekleme işlemleri kadar geri yükleme gerçekleştirilemez. |
| Yerel olarak sabitlenmiş birimler için Kurtarma geri yükleme |< 2 dakika |<ul><li>Birim boyutu ne olursa olsun geri yükleme işleminin 2 dakika içinde birim kullanılabilir hale getirilir.</li><li>Toplu performans başlangıçta çoğu verileri ve meta veriler yine de bulunduğu olarak bulutta normalden daha yavaş olabilir. StorSimple cihazına buluttan veri akışları olarak performansı artırabilir.</li><li>Meta verileri indirmek için toplam süreyi ayrılmış birimin boyutuna bağlıdır. Meta verileri, cihaz arka planda hızında TB veri ayrılmış birim başına 5 dakika içinde otomatik olarak getirilir. Bu oran, Internet bant genişliği buluta tarafından etkilenebilir.</li><li>Yerel olarak sabitlenmiş birimler varsa katmanlı birimler, birimdeki verileri de yerel olarak cihazda indirilir. Tüm birim verilerinin duruma zaman cihaza geri yükleme işlemi tamamlanır.</li><li>Geri yükleme işlemlerini uzun olabilir ve geri yüklemenin tamamlanması için toplam süreyi sağlanan yerel birimin, Internet bant genişliğiniz ve cihazdaki mevcut verilerin boyutuna bağlı olacaktır. Geri yükleme işlemi devam ederken yerel olarak sabitlenmiş birim üzerindeki yedekleme işlemlerine izin verilir. |
| Kullanılabilirlik ince-geri yükleme |Son yük devretme | |
| SSD katmanı * sunulduğunda, en fazla istemci okuma/yazma aktarım hızı |Tek bir 10 gigabitlik Ethernet ağ arabirim ile 920/720 MB/sn |MPIO ve iki ağ arabirimi ile en fazla iki kez. |
| HDD katmanı * sunulduğunda, en fazla istemci okuma/yazma aktarım hızı |120/250 MB/sn | |
| Bulut katmanı * sunulduğunda, en fazla istemci okuma/yazma aktarım hızı |11/41 MB/sn |Okuma aktarım hızı, oluşturma ve yeterli g/ç sıra derinliğini korumak istemcilerde bağlıdır. |

&#42;G/ç türü en fazla üretilen iş yüzde 100 okuma ve yüzde 100 yazma senaryoları ile ölçülmüştür. Gerçek aktarım hızı, daha düşük olabilir ve g/ç üzerinde bağlıdır karışımı ve ağ koşulları.

