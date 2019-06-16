---
title: StorSimple 8000 serisi güncelleştirme 4 sürüm notları | Microsoft Docs
description: StorSimple 8000 serisi güncelleştirme 4 için yeni özellikler, sorunlar ve geçici çözümleri açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 01/23/2018
ms.author: alkohli
ms.openlocfilehash: ef95ca7b9f94690b607e37fbf5d9378c2f2bcfda
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64698633"
---
# <a name="storsimple-8000-series-update-4-release-notes"></a>StorSimple 8000 serisi güncelleştirme 4 sürüm notları

## <a name="overview"></a>Genel Bakış

Aşağıdaki sürüm notları, yeni özellikleri açıklar ve kritik açık sorunlar için StorSimple 8000 serisi güncelleştirme 4 belirleyin. Ayrıca bu sürüme dahil edilen StorSimple yazılım güncelleştirmeleri listesini içerirler. 

Güncelleştirme 4 ile güncelleştirme 3.1 sürüm (GA) ya da güncelleştirme 0.1 çalıştıran tüm StorSimple cihazı için uygulanabilir. Güncelleştirme 4 ile ilişkili cihaz 6.3.9600.17820 sürümüdür.

Lütfen StorSimple çözümünüzde güncelleştirme dağıtmadan önce bu sürüm notlarında yer alan bilgileri gözden geçirin.

> [!IMPORTANT]
> * Güncelleştirme 4 cihaz yazılımı, USM üretici yazılımı, LSI sürücü ve bellenim, disk üretici yazılımı, Storport ve Spaceport, güvenlik ve diğer işletim sistemi güncelleştirmeleri var. Bu güncelleştirmeyi yüklemek için yaklaşık 4 saat sürer. Disk üretici yazılımı güncelleştirmesi, kesintiye uğratan bir güncelleştirmedir ve cihazınız için bir kapalı kalma süresi ile sonuçlanır. Cihazınızı güncel tutmak, güncelleştirme 4 uygulamanızı öneririz. 
> * Yeni yayınlar için güncelleştirmeleri göremeyebilirsiniz hemen güncelleştirmeleri aşamalı olarak sunulmasını çünkü. Birkaç gün bekleyin ve ardından güncelleştirmeleri bu olarak yeniden tarama yakında kullanıma sunulacaktır.

## <a name="whats-new-in-update-4"></a>Güncelleştirme 4'teki yenilikler

Güncelleştirme 4'te aşağıdaki anahtar geliştirmeleri ve hata düzeltmeleri yapıldı.

* **Daha Akıllı otomatik alan geri kazanma algoritmaları** – güncelleştirme 4, algoritmaları beklenen geri kazanılan kullanılabilir alan bulutta göre alan geri kazanma döngüleri ayarlamak için geliştirilmiş otomatik alan geri kazanma. 

* **Yerel olarak sabitlenmiş birimler için performans geliştirmeleri** – güncelleştirme 4, yerel olarak sabitlenmiş birimlerin yüksek veri alma (birim boyutu için karşılaştırılabilir veri) sahip senaryolarda performansı geliştirildi.

* **Isı Haritası tabanlı geri yükleme** -önceki sürümlerde, olağanüstü durum kurtarma (DR), aşağıdaki veriler bir yavaş performans erişim desenlerini göre buluttan geri yüklendi. 

    Yeni bir özellik güncelleştirme 4'te parçaları cihaz DR önce kullanımda olduğunda bir ısı haritası oluşturmak için veri sık erişilen uygulanır (en çok kullanılan veri öbekleri, öbekleri daha az kullanılan ise yüksek ısı sahip düşük ısı vardır). DR sonra StorSimple, otomatik olarak geri yüklemek ve buluttaki verileri yeniden doldurma ısı Haritası kullanır. 

    Tüm geri yükleme tabanlı ısı Haritası geri yüklemeler sunulmuştur. Sorgu ve ısı Haritası tabanlı geri yükleme ve yeniden doldurma işleri iptal etme hakkında daha fazla bilgi için Git [StorSimple cmdlet başvurusu için Windows PowerShell](https://technet.microsoft.com/library/dn688168.aspx).

* **StorSimple Tanılama Aracı** – kolayca tanımak için izin vermek için güncelleştirme 4, StorSimple Tanılama Aracı yayımlanmaktadır ve sistem, ağ, performans ve donanım bileşeni sistem durumu için ilgili sorunları giderme. Bu araç, StorSimple için Windows PowerShell çalıştırılır. Daha fazla bilgi için Git [StorSimple Tanılama aracını kullanmayla ilgili sorun giderme](storsimple-8000-diagnostics.md).

* **StorSimple Geçiş Aracı kullanıcı Arabirimi tabanlı** -Bu sürümden önce 5000-7000 serisinden veri geçişini kullanıcılar Azure PowerShell arabirimini kullanarak geçiş iş akışının bir parçası olarak çalıştırmak gerekli. Bu sürümde, kullanımı kolay bir UI tabanlı StorSimple Geçiş Aracı aynı geçiş iş akışı kolaylaştırmak, destek için kullanılabilir hale getirilir. Bu araç ayrıca kurtarma demet birleştirilmesi için izin verir. 

* **FIPS ile ilgili değişiklikleri** - bu sürüm ve sonraki sürümlerde, FIPS etkin varsayılan olarak tüm StorSimple 8000 serisi cihazlar için hesapları Microsoft Azure kamu ve Azure genel bulut.

* **Güncelleştirme değişiklikleri** -bu sürümde, hatalarını güncelleştir ilgili hatalar düzeltildi.

* **Disk hataları için uyarı** -bu sürümde kullanıcı yaklaşan bir disk hatalarının uyaran yeni bir uyarı eklenir. Bu uyarıyla karşılaşırsanız, Microsoft bir disk eklenmemiştir göndermeye Support başvurun. Daha fazla bilgi için Git [donanım uyarılar StorSimple Cihazınızda](storsimple-8000-manage-alerts.md#hardware-alerts).

* **Denetleyici değiştirme değişiklikleri** -denetleyici değiştirme işleminin durumunu sorgulamak kullanıcıya izin veren bir cmdlet bu sürümde eklenir. Daha fazla bilgi için Git [sorgu denetleyicisi değiştirme durumu cmdlet'e](https://technet.microsoft.com/library/dn688168.aspx).


## <a name="issues-fixed-in-update-4"></a>Güncelleştirme 4'te giderilen sorunlar

Aşağıdaki tabloda, güncelleştirme 4'te düzeltilen sorunlara ilişkin bir Özet sağlar.    

| Hayır | Özellik | Sorun | Fiziksel cihaz için geçerlidir | Sanal cihaza uygulanır |
| --- | --- | --- | --- | --- |
| 1 |Yük devretme |Önceki sürümde, yük devretme sonrasında müşteri sitesinde gözlemlenen temizleme ilgili bir sorun oluştu. Bu sürümde bu sorun düzeltilmiştir. |Evet |Evet |
| 2 |Yerel olarak sabitlenmiş birimler |Önceki sürümde, birim oluşturma hatalarına neden olan yerel olarak sabitlenmiş birimler için ilgili birim oluşturma için bir sorun oluştu. Bu sorun, kök neden ve bu sürümde giderilen. |Evet |Hayır |
| 3 |Destek Paketi |Önceki bir sürümde System.OutOfMemory özel durumu veya bir destek paketi oluşturma hatasıyla sonuçlanan diğer hataları neden olacağından destek paketi ile ilgili sorunlar oluştu. Bu hatalar, bu sürümde giderilen. |Evet |Evet |
| 4 |İzleme |Önceki bir sürümde var grafikler için yerel olarak izleme ile ilgili bir sorun tüketim EB burada gösterilen sabitlenmiş birimler. Bu hata, bu sürümde çözülür. |Evet |Evet |
| 5 |Geçiş |Önceki bir sürümde 8000 serisi cihazlar için 5000-7000 serisinden geçiş güvenilirliğini ilgili çeşitli sorunlar oluştu. Bu sürümde bu sorun çözüldü. |Evet |Evet |
| 6 |Güncelleştirme |Önceki sürümlerde, bir güncelleştirme hatası olduysa, denetleyiciler kurtarma moduna gitmesi gerekiyordu ve bu nedenle kullanıcı güncelleştirme ile devam edilemedi Microsoft Support iletişime geçmeniz. <br> Bu sürümde bu davranış değiştirilmiştir. Her iki denetleyici aynı sürümünü (güncelleştirme 4) çalıştırıldıktan sonra kullanıcının bir güncelleştirme hatası varsa, denetleyiciler kurtarma moduna girmemektedir. Kullanıcı bu hatayla karşılaşılırsa, bunlar için biraz bekleyin ve ardından güncelleştirmeyi yeniden öneririz. Yeniden deneme başarılı olabilir. Daha sonra yeniden deneme başarısız olursa, Microsoft Support başvurmalısınız. |Evet |Evet |


## <a name="known-issues-in-update-4-from-previous-releases"></a>Önceki sürümlerine yönelik güncelleştirme 4'te bilinen sorunlar

Güncelleştirme 4'te yeni bilinen bir sorun vardır. Önceki sürümlerden Update 4'e taşınır sorunların listesi için Git [güncelleştirme 3 Sürüm Notları](storsimple-update3-release-notes.md#known-issues-in-update-3).

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-update-4"></a>Seri Bağlı SCSI (SAS) denetleyicisi ve üretici yazılımı güncelleştirmelerini güncelleştirme 4

Bu sürüm, SAS Denetleyici ve LSI sürücü ve bellenim güncelleştirmeleri sahiptir. Bu güncelleştirmeleri yükleme hakkında daha fazla bilgi için bkz. [güncelleştirme 4'ü yükleyin](storsimple-install-update-4.md) StorSimple Cihazınızda.

## <a name="virtual-device-updates-in-update-4"></a>Güncelleştirme 4'te sanal cihaz güncelleştirmeleri

Bu güncelleştirme, StorSimple Cloud Appliance (sanal cihaz olarak da bilinir) uygulanamaz. Yeni sanal cihazları oluşturulması gerekir. 

## <a name="next-step"></a>Sonraki adım

Bilgi edinmek için nasıl [güncelleştirme 4'ü yükleyin](storsimple-install-update-4.md) StorSimple Cihazınızda.

