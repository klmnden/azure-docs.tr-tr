---
title: "StorSimple 8000 serisi güncelleştirme 4 sürüm notları | Microsoft Docs"
description: "Yeni özellikler, sorunlar ve geçici çözümler için StorSimple 8000 serisi güncelleştirme 4 açıklar."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 04/04/2017
ms.author: alkohli
ms.openlocfilehash: 23f1bbb066c5b6481988ee841ad8979d78abf084
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="storsimple-8000-series-update-4-release-notes"></a>StorSimple 8000 serisi güncelleştirme 4 sürüm notları

## <a name="overview"></a>Genel Bakış

Aşağıdaki sürüm notları, yeni özellikleri açıklar ve kritik açık sorunlar için StorSimple 8000 serisi güncelleştirme 4 tanımlayın. Ayrıca bu sürümde dahil StorSimple yazılım güncelleştirmelerinin bir listesini içerir. 

Güncelleştirme 4, sürüm (GA) veya güncelleştirme 0.1 güncelleştirme 3.1 çalışan herhangi bir StorSimple aygıtına uygulanabilir. Güncelleştirme 4 ile ilişkili aygıt 6.3.9600.17820 sürümüdür.

Lütfen StorSimple çözümünüzde güncelleştirme dağıtmadan önce sürüm notları'nda yer alan bilgileri gözden geçirin.

> [!IMPORTANT]
> * Güncelleştirme 4 aygıt yazılımı, USM üretici yazılımı, LSI sürücü ve bellenim, disk üretici yazılımı, Storport ve Spaceport, güvenlik ve diğer işletim sistemi güncelleştirmelerini sahiptir. Bu güncelleştirmeyi yüklemek için yaklaşık 4 saat sürer. Disk bellenim güncelleştirme kesintiye uğratan güncelleştirme ve cihazınız için bir kapalı kalma süresi ile sonuçlanır. Cihazınız güncel tutmak için güncelleştirme 4 uygulamanızı öneririz. 
> * Yeni sürümler için güncelleştirmelerinin göremeyebilirsiniz hemen biz aşamalı güncelleştirmeler yapmak için. Birkaç gün bekleyin ve ardından güncelleştirmeleri taramak üzere yeniden bunlar olarak yakında kullanılabilir hale gelecektir.

## <a name="whats-new-in-update-4"></a>Güncelleştirme 4'te Yenilikler

Güncelleştirme 4'te, aşağıdaki anahtar geliştirmeler ve hata düzeltmeleri yapıldı.

* **Daha Akıllı otomatik alan geri kazanma algoritmaları** – güncelleştirme 4, algoritmaları beklenen reclaimed kullanılabilir alanı bulutta göre alan geri kazanma döngüleri ayarlamak için geliştirilmiş otomatik alan geri kazanma. 

* **Yerel olarak sabitlenmiş birimleri için performans geliştirmeleri** – güncelleştirme 4, yerel olarak sabitlenmiş birimlerin yüksek veri alımı (veri birim boyutu karşılaştırılabilir) sahip senaryolarda performansını iyileştirilmiştir.

* **Heatmap tabanlı geri yükleme** -önceki sürümlerde, olağanüstü durum kurtarma (DR), aşağıdaki veriler bir yavaş performansı erişimi desenleri dayalı buluttan geri yüklendi. 

    Yeni bir özellik güncelleştirme 4'te parçaları cihaz DR önce kullanımda olduğunda bir heatmap oluşturmak için veri sık erişilen uygulanır (en fazla kullanılan veri öbekleri sahip yüksek ısı küçük parçalara kullanılan ancak düşük ısı vardır). DR sonra otomatik olarak geri yüklemek ve bulut verilerden rehydrate heatmap StorSimple kullanır. 

    Tüm geri yüklemeler dayalı heatmap geri yüklemeler sunulmuştur. Sorgulamak ve temel heatmap geri yükleme ve rehydration işleri iptal hakkında daha fazla bilgi için Git [StorSimple cmdlet başvurusu için Windows PowerShell](https://technet.microsoft.com/library/dn688168.aspx).

* **StorSimple Tanılama Aracı** – güncelleştirme 4'te, StorSimple Tanılama Aracı yayınlandığı kolaydır tanılamak için izin vermek ve sistem, ağ, performans ve donanım bileşen sistem durumu için ilgili sorunları giderme. Bu araç, StorSimple için Windows PowerShell üzerinden çalıştırılır. Daha fazla bilgi için Git [StorSimple Tanılama aracını kullanarak sorun giderme](storsimple-8000-diagnostics.md).

* **UI tabanlı StorSimple geçiş aracı** -Bu sürümden önce 5000-7000 Serisi veri geçişini kullanıcılar Azure PowerShell arabirimini kullanarak geçiş iş akışının parçası yürütmek gerekli. Bu sürümde, kullanımı kolay UI tabanlı StorSimple geçiş aracı için destek aynı geçiş iş akışını kolaylaştırmak kullanılabilir hale getirilir. Bu aracı kurtarma demet birleştirilmesi için de olanak tanır. 

* **FIPS ile ilgili değişiklikleri** - bu başlayarak sürüm, FIPS varsayılan olarak etkinleştirilmiştir tüm StorSimple 8000 serisi cihazlar üzerinde hesapları Microsoft Azure kamu ve Azure genel bulut için.

* **Değişiklikleri güncelleştirme** -bu sürümde, hataları güncelleştirmek için ilgili hatalar düzeltildi.

* **Disk hataları için uyarı** -bu sürümde yaklaşan disk hatası kullanıcıyı uyarır yeni bir uyarı eklenir. Bu uyarı karşılaşırsanız, Microsoft yedek diski dağıtmayı Support başvurun. Daha fazla bilgi için Git [donanım uyarıları, StorSimple Cihazınızda](storsimple-manage-alerts.md#hardware-alerts).

* **Denetleyici değiştirme değişiklikleri** -denetleyici değiştirme işleminin durumunu sorgulamak kullanıcıya izin veren bir cmdlet bu sürümde eklenir. Daha fazla bilgi için Git [cmdlet sorgu denetleyicisi değiştirme durumuna](https://technet.microsoft.com/library/dn688168.aspx).


## <a name="issues-fixed-in-update-4"></a>Güncelleştirme 4'te giderilen sorunlar

Aşağıdaki tabloda, güncelleştirme 4'te düzeltilen sorunlardan özetini sağlar.    

| Hayır | Özellik | Sorun | Fiziksel cihaz için geçerlidir | Sanal cihaz için geçerlidir |
| --- | --- | --- | --- | --- |
| 1 |Yük devretme |Önceki sürümde, yük devretme sonrasında müşteri sitesinde gözlenen temizleme ilgili bir sorun oluştu. Bu sürümde bu sorun düzeltilmiştir. |Evet |Evet |
| 2 |Yerel olarak sabitlenmiş birimleri |Önceki sürümde birim oluşturma hatalarına neden olan yerel olarak sabitlenmiş birimler için ilgili birim oluşturma için bir sorun oluştu. Bu sorun kök neden oldu ve bu sürümde sabit. |Evet |Hayır |
| 3 |Destek Paketi |Önceki sürümde System.OutOfMemory özel durum ya da diğer hataları destek paketi oluşturma karşılanamamasına neden olacağından destek paketi ile ilgili sorunlar vardı. Bu sürümde bu hatası düzeltilmiştir. |Evet |Evet |
| 4 |İzleme |Birimleri var. sabitlenmiş grafiklerde yerel olarak izleme ile ilgili bir sorunu tüketim EB içinde nerede gösterildi önceki sürümde. Bu hata, bu sürümde çözümlenir. |Evet |Evet |
| 5 |Geçiş |Önceki sürümde, 5000-7000 Serisi 8000 serisi cihazlar için gelen geçiş güvenilirliğini ilgili çeşitli sorunlar vardı. Bu sorunları bu sürümde giderilmiştir. |Evet |Evet |
| 6 |Güncelleştirme |Önceki sürümlerde, bir güncelleştirme hatası varsa, denetleyicileri kurtarma moduna geçecek ve bu nedenle kullanıcı güncelleştirmeye devam edilemiyor Microsoft Support iletişim kurmanız. <br> Bu davranış, bu sürümde değiştirildi. İki denetleyiciye de aynı sürüm (güncelleştirme 4) çalıştıran sonra kullanıcı bir güncelleştirme hatası varsa, denetleyicileri kurtarma moduna girmez. Kullanıcı bu hatayla karşılaşırsa için biraz bekleyin ve sonra güncelleştirmeyi yeniden deneyin öneririz. Yeniden deneme başarılı. Daha sonra yeniden deneme başarısız olursa, Microsoft Support başvurmalısınız. |Evet |Evet |


## <a name="known-issues-in-update-4-from-previous-releases"></a>Önceki sürümlerden güncelleştirme 4'te bilinen sorunlar

Güncelleştirme 4'te yeni bilinen bir sorun vardır. Önceki sürümlerden güncelleştirme 4'e taşınır sorunların listesi için Git [güncelleştirme 3 Sürüm Notları](storsimple-update3-release-notes.md#known-issues-in-update-3).

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-update-4"></a>Seri Bağlı SCSI (SAS) denetleyicisi ve bellenim güncelleştirmeleri güncelleştirme 4

Bu sürüm, SAS denetleyicisi ve LSI sürücü ve bellenim güncelleştirmeleri vardır. Bu güncelleştirmeleri yükleme hakkında daha fazla bilgi için bkz: [güncelleştirme 4'ü yükleyin](storsimple-install-update-4.md) , StorSimple Cihazınızda.

## <a name="virtual-device-updates-in-update-4"></a>Güncelleştirme 4'te sanal aygıt güncelleştirmeleri

Bu güncelleştirme, StorSimple bulut uygulaması (sanal aygıt olarak da bilinir) uygulanamaz. Yeni sanal cihazların oluşturulması gerekir. 

## <a name="next-step"></a>Sonraki adım

Bilgi edinmek için nasıl [güncelleştirme 4'ü yükleyin](storsimple-install-update-4.md) , StorSimple Cihazınızda.

