---
title: "StorSimple 8000 serisi sistem sınırlarını | Microsoft Docs"
description: "Sistem sınırlarını ve StorSimple 8000 serisi bileşenleri ve bağlantıları için önerilen boyutlar açıklar."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: c7392678-0924-46c6-9c59-1665cb9b6586
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3e4f7bfd117696ddb25156e027e29c0d21f27804
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="what-are-storsimple-8000-series-system-limits"></a>StorSimple 8000 serisi sistem sınırları nelerdir?
## <a name="overview"></a>Genel Bakış
StorSimple merkeziniz için ölçeklenebilir ve esnek depolama sağlar. Ancak, planlamak, dağıtmak ve StorSimple çözümünüzün çalıştırmak gibi göz önünde bulundurmanız gereken bazı sınırlamalar vardır. Aşağıdaki tabloda bu sınırları açıklar ve bazı öneriler sunar, böylece en StorSimple çözümünüzün dışında elde edebilirsiniz.

| Sınır tanımlayıcı | Sınır | Yorumlar |
| --- | --- | --- |
| Depolama hesabının kimlik bilgilerini maksimum sayısı |64 | |
| Birim kapsayıcıları maksimum sayısı |64 | |
| En fazla sayıda birime |255 | |
| Yerel olarak sabitlenmiş birimlerin sayısı |32 | |
| Zamanlamalar bant genişliği şablonu başına maksimum sayısı |168 |Her gün (24 * 7) haftanın her saat için bir zamanlama. |
| Katmanlı birim fiziksel aygıtlarda en büyük boyutu |8100 ve 8600 64 TB |8100 ve 8600 fiziksel aygıtlardır. |
| Azure'da sanal cihazlarda katmanlı birim en büyük boyutu |8010 30 TB <br></br> 8020 için 64 TB |8010 hem de 8020 sırasıyla standart depolama ve Premium depolama kullanan sanal Azure aygıtlardır. |
| Fiziksel cihazları yerel olarak sabitlenmiş bir birimde en büyük boyutu |8100 8,5 TB <br></br> 8600 22,5 TB |8100 ve 8600 fiziksel aygıtlardır. |
| İSCSI bağlantısı sayısı |512 | |
| İSCSI başlatıcıları bağlantılarından maksimum sayısı |512 | |
| Erişim denetimi kayıtları aygıt başına maksimum sayısı |64 | |
| En fazla sayıda birime yedekleme İlkesi başına |20 | |
| Yedekleme zamanlaması (Yedekleme ilkesinde) başına korunur maksimum sayısı |64 | |
| Zamanlamalar yedekleme İlkesi başına maksimum sayısı |10 | |
| Maksimum sayıda anlık görüntü birim başına korunabilir herhangi bir türde |256 |Bu sayı yerel anlık görüntülerini içerir ve bulut anlık görüntüleri. |
| Maksimum sayıda içinde herhangi bir cihazda mevcut olabilecek anlık görüntü |10,000 | |
| En fazla sayıda paralel yedekleme, geri yükleme için işlenen ya da kopyalama birime |16 |<ul><li>16'dan fazla birim varsa, işleme yuvaları kullanılabilir duruma geldiğinde, sıralı olarak işlenir.</li><li>İşlemi tamamlanana kadar bir kopyalanan yeni yedeklerini veya geri yüklenen bir katmanlı birim oluşamaz. Ancak, yerel bir birim için birim çevrimiçi olduktan sonra yedeklemeler izin verilir.</li></ul> |
| Geri yükleme ve kurtarma zamanı katmanlı birimler için kopyalama |< 2 dakika |<ul><li>Birim boyutu bağımsız olarak, geri yükleme veya kopyalama işleminin 2 dakika içinde birim kullanılabilir hale getirilir.</li><li>Birim performans başlangıçta veri ve meta veriler çoğunu hala yer aldığı bulutta normalden daha yavaş olabilir. StorSimple cihazı için buluttan veri akışları olarak performansı artırabilir.</li><li>Meta veri indirmek için toplam süreyi ayrılmış birimin boyutuna bağlıdır. Meta verileri otomatik olarak ayrılmış birim verilerini TB başına 5 dakika hızında arka planda bir aygıt halinde duruma getirilir. Internet bant genişliği bulut için bu oran etkilenebilir.</li><li>Tüm meta verilerin cihazda olduğunda geri yükleme veya kopyalama işlemi tamamlanır.</li><li>Kopya işlemi tam olarak tamamlandıktan veya yedekleme işlemleri kadar geri yükleme gerçekleştirilemez. |
| Yerel olarak sabitlenmiş birimler için kurtarma süresi geri yükleme |< 2 dakika |<ul><li>Geri yükleme işleminin bağımsız olarak birim boyutu 2 dakika içinde birim kullanılabilir hale getirilir.</li><li>Birim performans başlangıçta veri ve meta veriler çoğunu hala yer aldığı bulutta normalden daha yavaş olabilir. StorSimple cihazı için buluttan veri akışları olarak performansı artırabilir.</li><li>Meta veri indirmek için toplam süreyi ayrılmış birimin boyutuna bağlıdır. Meta verileri otomatik olarak ayrılmış birim verilerini TB başına 5 dakika hızında arka planda bir aygıt halinde duruma getirilir. Internet bant genişliği bulut için bu oran etkilenebilir.</li><li>Yerel olarak sabitlenmiş birimlerin, katmanlı birimleri aksine birim verilerini de yerel olarak cihazda indirilir. Tüm birim verilerini duruma zaman cihaza geri yükleme işlemi tamamlanır.</li><li>Geri yükleme işlemlerini uzun olabilir. Geri yüklemenin tamamlanması için gereken toplam süre, sağlanan yerel birim, Internet bant genişliği ve cihazda mevcut veri boyutuna bağlıdır. Geri yükleme işlemi devam ederken yerel olarak sabitlenmiş birim yedekleme işlemlerine izin verilir. |
| Bulut anlık görüntüleri için işlem hızı |15 dakika/TB |<ul><li>Bulut yapmak için minimum saat anlık görüntü ayrılmış birim verilerini yedekleme TB başına karşıya yükleme için hazır. </li><li> Toplam bulut anlık görüntü saati, bulut için Internet bant genişliği etkilenen anlık görüntü karşıya yükleme zamanı bu kez ekleyerek hesaplanır. |
| (SSD Katmanı'ndan sunulduğunda) en fazla istemci okuma/yazma verimlilik * |920/720 MB/s tek bir 10 GbE ağ arabirimine sahip |En çok 2 x MPIO ile ve iki ağ arabirimi. |
| (HDD Katmanı'ndan sunulduğunda) en fazla istemci okuma/yazma verimlilik * |120/250 MB/s | |
| (Bulut Katmanı'ndan sunulduğunda) en fazla istemci okuma/yazma verimlilik * güncelleştirme 3 ve sonraki sürümlerinde ** |40/60 MB/s için katmanlı birimler<br><br>60/80 MB/s için katmanlı birimlerin birim oluşturma sırasında seçilen arşivleme seçeneğiyle |Okuma üretilen işi oluşturmak ve yeterli g/ç sıra derinliğini koruyarak istemcilerde bağlıdır. <br><br>Elde edilen hızı kullanılan temel alınan depolama hesabı hızına bağlıdır. |

&#42; G/ç türü başına en fazla üretilen iş yüzde 100 okuma ve yüzde 100 yazma senaryoları ile ölçülen. Gerçek verimlilik daha düşük olabilir ve g/ç üzerinde bağlı karışımı ve ağ koşulları.

&#42; &#42; Güncelleştirme 3'ü önce performans numaraları daha düşük olabilir.

## <a name="next-steps"></a>Sonraki adımlar
Gözden geçirme [StorSimple sistem gereksinimleri](storsimple-system-requirements.md). 

