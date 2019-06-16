---
title: Microsoft Azure StorSimple sanal dizisi güncelleştirme 1.2 sürüm notları | Microsoft Docs
description: StorSimple sanal güncelleştirme 1.2 çalıştıran dizisi için kritik açık sorunlar ve çözümleri açıklanmaktadır.
services: storsimple
author: alkohli
ms.service: storsimple
ms.topic: article
ms.date: 05/29/2019
ms.author: alkohli
ms.openlocfilehash: ea7e4801dfaad533403c0f927a03735ae409cc52
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66420611"
---
# <a name="storsimple-virtual-array-update-12-release-notes"></a>StorSimple sanal dizisi güncelleştirme 1.2 sürüm notları

Aşağıdaki sürüm notları, kritik açık sorunlar ve çözümlenen sorunlar için Microsoft Azure StorSimple Virtual Array güncelleştirme belirleyin.

Sürüm Notları sürekli olarak güncelleştirilir. Bunlar, geçici bir çözüm gerektiren kritik sorunlar bulundukça eklenir. StorSimple Virtual Array'iniz dağıtmadan önce bu sürüm notlarında yer alan bilgileri dikkatle gözden geçirin.

Güncelleştirme 1.2 yazılım sürümüne karşılık gelen **10.0.10311.0**.

> [!IMPORTANT]
> - Güncelleştirmeler kesintiye uğratan bir durumdur ve Cihazınızı yeniden başlatın. Cihaz g/ç ediyor, kapalı kalma süresi artmasına neden olur. Bu güncelleştirmeyi uygulamak için kullanılan paketler hakkında ayrıntılı yönergeler için Git [güncelleştirme 1.2 indirme](#download-update-12).
> - Güncelleştirme 1.2, yalnızca Cihazınızı güncelleştirme 1.0 veya 1.1 çalışıyorsa Azure portalı üzerinden kullanılabilir.

## <a name="whats-new-in-update-12"></a>Güncelleştirme 1.2 ile yenilikler nelerdir?

Bu güncelleştirme, aşağıdaki hata düzeltmeleri içerir:

- İşleme sırasında geliştirilmiş dayanıklılık dosyaları silindi.
- Geliştirilmiş işleme özel durumlar için önde gelen kod başlangıç yolunda yedeklemeler, geri yükleme, buluttan okuma hataları azalır ve alan geri kazanma otomatik.

## <a name="download-update-12"></a>1\.2 güncelleştirmesini indirme

Bu güncelleştirmeyi indirmek için Git [Microsoft Update Kataloğu](https://www.catalog.update.microsoft.com/Home.aspx) sunucusu ve KB4502035 indirme paketi. Bu paket, aşağıdaki paketler içeriyor:

 - **KB4493446** Nisan 2019 kadar 2012 R2 için toplu Windows güncelleştirmeleri içerir. Bu paketi içinde yer aldığı daha fazla bilgi için Git [Nisan aylık güvenlik dökümü](https://support.microsoft.com/help/4493446/windows-8-1-update-kb4493446).
 - **KB3011067** Microsoft Update tek başına paketin dosya WindowsTH KB3011067 x64 olduğu. Bu dosya, cihaz yazılımı güncelleştirmek için kullanılır.

KB4502035 indirin ve bu yönergeleri izleyin [güncelleştirmesini yerel web UI aracılığıyla](storsimple-virtual-array-install-update-11.md#use-the-local-web-ui).

## <a name="issues-fixed-in-update-12"></a>Güncelleştirme 1.2 ile giderilen sorunlar

Aşağıdaki tabloda, bu sürümde giderilen sorunlar özetini sağlar.

| Hayır. | Özellik | Sorun |
| --- | --- | --- |
| 1 |Silme| Yazılım önceki sürümlerinde, bir sorun oluştu olduğunda bile dosya silindi, cihazın kullanımı değişiklik olmadı. Bu sürümde bu sorun düzeltilmiştir. Katmanlama kod yolu silinen dosyaların işlerken karşı daha dayanıklı yapıldı.|
| 2 |Özel durum işleme| Yazılım önceki sürümlerinde, potansiyel olarak yedeklemeler, geri yükleme, buluttan okuma hatalarına neden olabilir ve alan geri kazanma otomatik sistem yeniden başlatma izleyen bir sorun oluştu. Bu sürüm için farklı başlangıç yolu özel durumların nasıl işlendiğini değişiklikler içeriyor.|

## <a name="known-issues-in-update-12"></a>Güncelleştirme 1.2'de bilinen sorunlar

Yayın belirtilen güncelleştirme 1.2 ile yeni bir sorun yok. Tüm yayın belirtildiği sorunlarını önceki sürümlerden taşınır. Bilinen sorunlar dahil önceki sürümlerden özetini görmek için Git [güncelleştirme 1.1 bilinen](storsimple-virtual-array-update-11-release-notes.md#known-issues-in-update-11).

## <a name="next-steps"></a>Sonraki adımlar

KB4502035 indirin ve [güncelleştirmesini yerel web UI aracılığıyla](storsimple-virtual-array-install-update-11.md#use-the-local-web-ui).

## <a name="references"></a>Başvurular

Eski bir sürüm notu için mi arıyorsunuz? Şuraya gidin:
* [StorSimple sanal dizisi güncelleştirme 1.1 sürüm notları](storsimple-virtual-array-update-11-release-notes.md)
* [StorSimple Virtual Array güncelleştirme 1.0 sürüm notları](storsimple-virtual-array-update-1-release-notes.md)
* [StorSimple sanal dizisi güncelleştirme 0.6 sürüm notları](storsimple-virtual-array-update-06-release-notes.md)
* [StorSimple sanal dizisi güncelleştirme 0.5 sürüm notları](storsimple-virtual-array-update-05-release-notes.md)
* [StorSimple sanal dizisi güncelleştirme 0,4 sürüm notları](storsimple-virtual-array-update-04-release-notes.md)
* [StorSimple sanal dizisi güncelleştirme 0.3 sürüm notları](storsimple-ova-update-03-release-notes.md)
* [StorSimple sanal dizisi güncelleştirme 0.1 ve 0.2 sürüm notları](storsimple-ova-update-01-release-notes.md)
* [StorSimple sanal dizisi genel kullanılabilirlik sürüm notları](storsimple-ova-pp-release-notes.md)
