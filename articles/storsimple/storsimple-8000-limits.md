---
title: StorSimple 8000 serisi sistem sınırlarını | Microsoft Docs
description: Sistem sınırlarını ve StorSimple 8000 serisi bileşenleri ve bağlantıları için önerilen boyutları açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: c7392678-0924-46c6-9c59-1665cb9b6586
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 03/28/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a0053f950b36351b06d08630cbf9977f53f2ed47
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60319573"
---
# <a name="what-are-storsimple-8000-series-system-limits"></a>StorSimple 8000 serisi sistem sınırları nelerdir?

## <a name="overview"></a>Genel Bakış

StorSimple, veri merkezi için ölçeklenebilir ve esnek depolama sağlar. Ancak, planlama, dağıtma ve StorSimple çözümünüzün çalışması, dikkat etmeniz gereken bazı sınırlamalar vardır. Aşağıdaki tabloda, bu sınırları açıklar ve bazı öneriler sunar, böylece en çok StorSimple çözümünüzün dışında alabilirsiniz.

| Sınır tanımlayıcı | Sınır | Yorumlar |
| --- | --- | --- |
| Depolama hesabı kimlik bilgileri sayısı |64 | |
| Birim kapsayıcılarının sayısı |64 | |
| En fazla birim sayısı |255 | |
| Yerel olarak sabitlenmiş birim sayısı |32 | |
| Zamanlamalar bant genişliği şablonu başına en fazla sayısı |168 |Her saat (24 * 7) haftanın her günü için bir zamanlama belirleyin. |
| Katmanlı birim fiziksel cihazlarda en yüksek boyutu |8100 ve 8600 için 64 TB |8100 ve 8600 fiziksel cihazlardır. |
| Azure sanal cihazda katmanlı birim en büyük boyutu |8010'un 30 TB <br></br> 8020 için 64 TB |8010 ve 8020 sırasıyla standart depolama ve Premium depolama kullanan sanal azure'da cihazlardır. |
| Fiziksel cihazlarda yerel olarak sabitlenmiş bir birim, en büyük boyutu |8100 için 8,5 TB <br></br> 8600 için 22,5 TB'a |8100 ve 8600 fiziksel cihazlardır. |
| İSCSI bağlantı sayısı üst sınırı |512 | |
| İSCSI başlatıcılarının bağlantılarından sayısı |512 | |
| Erişim denetimi kayıtları cihaz başına en fazla sayısı |64 | |
| Yedekleme İlkesi başına birim sayısı |20 | |
| En fazla (bir yedekleme İlkesi) zamanlama başına korunan yedekleme sayısına |64 | |
| Yedekleme İlkesi başına zamanlamaları sayısı |10 | |
| Birim başına korunabilir herhangi bir türde anlık görüntü sayısı |256 |Bu sayı, yerel anlık görüntüleri içerir ve bulut anlık görüntüleri. |
| İçinde herhangi bir cihazda mevcut olabilecek anlık görüntü sayısı |10,000 | |
| En fazla sayıda paralel yedekleme, geri yükleme için işlenen ya da kopyalama |16 |<ul><li>16'dan fazla birim varsa, işleme yuvaları kullanılabilir oldukça, sıralı olarak işlenir.</li><li>İşlemi tamamlanana kadar kopyalanmış bir yeni yedeklerini veya geri yüklenen bir katmanlı birim oluşamaz. Ancak, yerel bir birim için birim çevrimiçi olduktan sonra yedeklemeler izin verilir.</li></ul> |
| Geri yükleme ve kopyalama, katmanlı birimlerin zaman Kurtar |< 2 dakika |<ul><li>Birim boyutu ne olursa olsun, geri yükleme ya da kopyalama işleminin 2 dakika içinde birim kullanılabilir hale getirilir.</li><li>Toplu performans başlangıçta çoğu verileri ve meta veriler yine de bulunduğu olarak bulutta normalden daha yavaş olabilir. StorSimple cihazına buluttan veri akışları olarak performansı artırabilir.</li><li>Meta verileri indirmek için toplam süreyi ayrılmış birimin boyutuna bağlıdır. Meta verileri, cihaz arka planda hızında TB veri ayrılmış birim başına 5 dakika içinde otomatik olarak getirilir. Bu oran, Internet bant genişliği buluta tarafından etkilenebilir.</li><li>Tüm meta verileri cihaz üzerinde olduğunda geri yükleme veya kopyalama işlemi tamamlanmıştır.</li><li>Kopyalama işlemi tam olarak bitmiş durumda veya yedekleme işlemleri kadar geri yükleme gerçekleştirilemez. |
| Yerel olarak sabitlenmiş birimler için Kurtarma geri yükleme |< 2 dakika |<ul><li>Birim boyutu ne olursa olsun geri yükleme işleminin 2 dakika içinde birim kullanılabilir hale getirilir.</li><li>Toplu performans başlangıçta çoğu verileri ve meta veriler yine de bulunduğu olarak bulutta normalden daha yavaş olabilir. StorSimple cihazına buluttan veri akışları olarak performansı artırabilir.</li><li>Meta verileri indirmek için toplam süreyi ayrılmış birimin boyutuna bağlıdır. Meta verileri, cihaz arka planda hızında TB veri ayrılmış birim başına 5 dakika içinde otomatik olarak getirilir. Bu oran, Internet bant genişliği buluta tarafından etkilenebilir.</li><li>Yerel olarak sabitlenmiş birimlerin, katmanlı birimlerin aksine birimdeki verileri de yerel olarak cihazda indirilir. Tüm birim verilerinin duruma zaman cihaza geri yükleme işlemi tamamlanır.</li><li>Geri yükleme işlemlerini uzun olabilir. Geri yüklemenin tamamlanması için toplam süreyi sağlanan yerel birimin, Internet bant genişliğiniz ve cihazdaki mevcut verilerin boyutuna bağlıdır. Geri yükleme işlemi devam ederken yerel olarak sabitlenmiş birim üzerindeki yedekleme işlemlerine izin verilir. |
| Bulut anlık görüntüleri için işlem hızı |15 dakika/TB |<ul><li>Bulut yapmak için minimum süre anlık görüntü yedekleme verilerinde ayrılmış birim başına karşıya yükleme için hazır. </li><li> Toplam bulut anlık görüntü zaman buluta Internet bant genişliği tarafından etkilenen anlık görüntüsünü karşıya yükleme zamanı bu süre eklenerek hesaplanır. |
| En fazla istemci okuma/yazma performans (SSD katmanından sunulduğunda) * |920/720 MB/s ile tek bir 10 GbE ağ arabirimi |En fazla 2 x ile MPIO ve iki ağ arabirimi. |
| En fazla istemci okuma/yazma performans (HDD katmanı sunulduğunda) * |120/250 MB/s | |
| (Bulut katmanından sunulduğunda) en fazla istemci okuma/yazma performans * güncelleştirme 3 ve sonraki ** |40 60 MB/s için katmanlı birimler<br><br>60/80 MB/s için katmanlı birimler birim oluşturma sırasında seçili arşiv seçeneğiyle |Okuma aktarım hızı, oluşturma ve yeterli g/ç sıra derinliğini korumak istemcilerde bağlıdır. <br><br>Elde edilen hızı, kullanılan temel alınan depolama hesabı hızına bağlıdır. |

&#42;G/ç türü en fazla üretilen iş yüzde 100 okuma ve yüzde 100 yazma senaryoları ile ölçülmüştür. Gerçek aktarım hızı, daha düşük olabilir ve g/ç üzerinde bağlıdır karışımı ve ağ koşulları.

&#42;&#42;Güncelleştirme 3'ü önce performans numaraları daha düşük olabilir.

## <a name="next-steps"></a>Sonraki adımlar
Gözden geçirme [StorSimple sistem gereksinimleri](storsimple-8000-system-requirements.md).

