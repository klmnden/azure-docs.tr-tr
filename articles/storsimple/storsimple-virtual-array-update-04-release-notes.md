---
title: StorSimple sanal dizisi güncelleştirme 0,4 sürüm notları | Microsoft Docs
description: StorSimple sanal güncelleştirme 0.4 çalıştıran dizisi için kritik açık sorunlar ve çözümleri açıklanmaktadır.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/05/2017
ms.author: alkohli
ms.openlocfilehash: 06a3469507631d032535bce62b01d964e99dc603
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60334803"
---
# <a name="storsimple-virtual-array-update-04-release-notes"></a>StorSimple sanal dizisi güncelleştirme 0,4 sürüm notları

## <a name="overview"></a>Genel Bakış

Aşağıdaki sürüm notları, kritik açık sorunlar ve çözümlenen sorunlar için Microsoft Azure StorSimple Virtual Array güncelleştirme belirleyin.

Sürüm Notları sürekli olarak güncelleştirilir ve geçici bir çözüm gerektiren kritik sorunlar bulundukça eklenir. StorSimple Virtual Array'iniz dağıtmadan önce bu sürüm notlarında yer alan bilgileri dikkatle gözden geçirin.

Güncelleştirme 0.4 yazılım sürümüne karşılık gelen **10.0.10289.0**.

> [!NOTE]
> Güncelleştirmeler kesintiye uğratan bir durumdur ve Cihazınızı yeniden başlatın. Cihaz g/ç ediyor, kapalı kalma süresi artmasına neden olur.


## <a name="whats-new-in-the-update-04"></a>Güncelleştirme 0.4 yenilikler nelerdir?
Güncelleştirme 0.4 öncelikle bazı geliştirmeler eşleşmiş bir hata düzeltmesi derleme ' dir. Bu sürümde çeşitli hatalar önceki sürümde yedekleme hataları výsledek çözüldü. Ana geliştirmeleri ve hata düzeltmeleri aşağıdaki gibidir:

- **Yedekleme performans geliştirmeleri** -bu sürüm, yedekleme performansını artırmak için birkaç önemli geliştirmeler yapılmıştır. Sonuç olarak, çok sayıda dosya içeren yedeklemeler, tam ve artımlı yedeklemeler için tamamlanma süresi önemli azalmaya bakın.

- **Geri yükleme performansı Gelişmiş** -bu sürüm, çok sayıda dosya kullanırken geri yükleme performansını önemli ölçüde artırmak geliştirmeler içerir. 2-4 milyon dosyaları kullanıyorsanız, 16 GB RAM, yenilikleri görmek için bir sanal dizin sağlamanızı öneririz. Sanal makine için en düşük gereksinimi 2 milyondan az dosyalarını kullanırken, 8 GB RAM olmaya devam eder.

- **Destek Paketi geliştirmeleri** -disk, CPU, bellek, ağ ve böylece cihaz sorunlarını tanılamaya/ayıklama işlemi artırma destek paketi buluta istatistikleri de günlüğe kaydetme geliştirmeleri içerir.

- **İSCSI birimlerin 200 GB sınırını yerel olarak sabitlenmiş** -yerel olarak sabitlenmiş birimler için StorSimple sanal dizisi üzerinde bir 200 GB iSCSI birimi sınırlamak olan öneririz. Katmanlı birim için yerel ayırma için sağlanan birim hacminin % 10 olmaya devam eder ancak 200 GB üzerinden ücret alınır. 

- **Yedekleme ile ilgili hata düzeltmeleri** - önceki sürümlerinde yazılım, yedeklemeleri için yedekleme hatalarına neden olan ilgili sorunlar oluştu. Bu hatalar, bu sürümde çözüldü.


## <a name="issues-fixed-in-the-update-04"></a>Güncelleştirme 0.4 giderilen sorunlar

Aşağıdaki tabloda, bu sürümde giderilen sorunlar özetini sağlar.

| Hayır. | Özellik | Sorun |
| --- | --- | --- |
| 1 |Yedekleme performansı|Önceki sürümlerde, çok sayıda dosya içeren yedeklemeler (gün sırasına göre) tamamlanması uzun zaman alır. Bu sürümde, tam ve artımlı yedeklemeler tamamlanma süresi önemli azalmaya bakın. |
| 2 |Destek Paketi|Disk, CPU, bellek, ağ ve bulut istatistikleri artık destek paketleri cihaz sorunları gidermeye çok etkili hale getirme destek günlükleri için oturum açtınız.|
| 3 |Backup |Önceki sürümlerde, yedeklemeleri uzun süre çalışan bir alanı işlemden geçirin yedekleme hataları kaynaklanan cihazda neden olabilir. Bu hatayı kuyruğuna tek seferde en fazla 5 yedeklemeleri sağlayarak bu sürümde giderilen.|
| 4 |iSCSI | Önceki sürümlerde, katmanlı veya yerel olarak sabitlenmiş birim için yerel ayırma için sağlanan birim hacminin % 10 idi. Bu sürümde, tüm iSCSI birimler (yerel olarak sabitlenmiş veya katmanlı) için yerel ayırma en fazla 200 GB'lık (katmanlı birimlerin 2 TB'tan büyük için) % 10 sınırlıdır böylece boşaltma yerel diskte daha fazla alan boşaltın. Bu sürümde yerel olarak sabitlenmiş birimlerin 200 GB ile sınırlı olmasını öneririz.|


## <a name="known-issues-in-the-update-04"></a>Güncelleştirme 0.4'de bilinen sorunlar

Aşağıdaki tabloda StorSimple sanal dizisi için bilinen sorunların bir Özet sağlar ve önceki sürümlerden yayın belirtildiği sorunları içerir. 

| Hayır. | Özellik | Sorun | Geçici çözüm/açıklamaları |
| --- | --- | --- | --- |
| **1.** |Güncelleştirmeler |Önizleme sürümünde oluşturulan sanal cihazlar için desteklenen genel kullanılabilirlik sürümü güncelleştirilemiyor. |Bu sanal cihazlar için genel kullanım sürümünde bir olağanüstü durum kurtarma (DR) iş akışı kullanarak devredilen gerekir. |
| **2.** |Sağlanan veri diski |Belirli bir belirtilen boyutta bir veri diski sağladığınız ve karşılık gelen StorSimple sanal cihazı oluşturdunuz, gerekir değil genişletin veya veri diski küçültmeye sonra. Cihaz yerel katmanlarda tüm verilerin kaybıyla sonuçları yapmak çalışıyor. | |
| **3.** |Grup İlkesi |Bir cihaz etki alanına katılmış olduğunda, bir Grup İlkesi uygulama cihaz işlemi olumsuz yönde etkileyebilir. |Sanal diziniz kendi kuruluş birimi (OU) için Active Directory olduğundan ve hiçbir Grup İlkesi nesneleri (GPO) uygulanmış emin olun. |
| **4.** |Yerel web kullanıcı Arabirimi |Internet Explorer (IE ESC) Artırılmış güvenlik özellikleri etkinleştirilirse, bazı sorun giderme veya bakım gibi yerel web kullanıcı Arabirimi sayfalarını düzgün çalışmayabilir. Bu sayfa düğmelerini de çalışmayabilir. |Internet Explorer Gelişmiş güvenlik özelliklerini devre dışı bırakın. |
| **5.** |Yerel web kullanıcı Arabirimi |Bir Hyper-V sanal makine, GB/sn ağ arabirimlerinin de kullanıcı Arabirimi olarak 10 görüntülenen web arabirimleri. |Bir yansıma Hyper-V, davranıştır. Hyper-V, sanal ağ bağdaştırıcıları için 10 GB/sn her zaman gösterilir. |
| **6.** |Katmanlı birimler veya paylaşımlar |Katmanlı birimlerin desteklenmiyor StorSimple ile çalışan uygulamalar için kilitleme bayt aralığı. Bayt aralığı kilitleme etkinse, StorSimple katmanlama çalışmaz. |Önerilen ölçüleri içerir: <br></br>Bayt aralığı uygulama mantığınızın kilitleme devre dışı bırakın.<br></br>Bu uygulama için verileri yerel olarak sabitlenmiş birim katmanlı birimlerin yerine koymak seçin.<br></br>*Uyarı*: Geri yükleme tamamlamadan önce kullanarak yerel olarak sabitlenmiş birimler ve bayt aralığı kilitleme etkin olduğunda, yerel olarak sabitlenmiş birimin çevrimiçi olabilir. Bir geri yükleme devam ediyor, bu gibi durumlarda, daha sonra tamamlamak geri yüklemek için beklemeniz gerekir. |
| **7.** |Katmanlı paylaşımları |Büyük dosyaları ile çalışma, yavaş bir katmanın ölçeğini sonuçlanabilir. |Büyük dosyalarla çalışırken, en büyük dosya paylaşım boyutunun %3 küçükse öneririz. |
| **8.** |Kapasite paylaşımlar için kullanılan |Görebileceğiniz paylaşımında veri olduğunda tüketim paylaşın. Kullanılan kapasite paylaşımları için meta veriler içeren olmasıdır. | |
| **9.** |Olağanüstü durum kurtarma |Yalnızca dosya sunucusu aynı etki, kaynak cihaz için olağanüstü durum kurtarma gerçekleştirebilirsiniz. Olağanüstü durum kurtarma için başka bir etki alanındaki bir hedef cihaz, bu sürümde desteklenmiyor. |Bu, bir sonraki sürümde uygulanır. |
| **10.** |Azure PowerShell |StorSimple sanal cihazları, bu sürüm Azure PowerShell aracılığıyla yönetilemez. |Tüm sanal cihazların yönetimini, Klasik Azure portalı ve yerel web UI aracılığıyla yapılmalıdır. |
| **11.** |Parola değiştirme |Sanal dizi cihaz konsolu yalnızca en-US klavye biçimde giriş kabul eder. | |
| **12.** |CHAP |CHAP kimlik oluşturulduktan sonra kaldırılamaz. Ayrıca, CHAP kimlik bilgilerini değiştirirseniz, birimlerin çevrimdışına alın ve değişikliğin etkili olması çevrimiçi bunları getirmek gerekir. |Bu sorun, bir sonraki sürümde değinilmiştir. |
| **13.** |iSCSI sunucusu |'Depolama görüntülenen bir iSCSI birimi için kullanılan' StorSimple Yöneticisi hizmeti ve iSCSI konağının farklı olabilir. |İSCSI ana bilgisayar dosya sistemi görünüme sahiptir.<br></br>Cihaz, birim maksimum boyutta olduğunda ayrılan blokları görür. |
| **14.** |Dosya sunucusu |Bir klasördeki bir dosyaya bir alternatif veri Stream (ilişkili REKLAM) varsa, REKLAM değil yedeklenen veya olağanüstü durum kurtarma, kopyalama ve öğe düzeyinde Kurtarma ile geri. | |
| **15.** |Dosya sunucusu |Sembolik bağlantılar desteklenmez. | |
| **16.** |Dosya sunucusu |Tarafından Windows şifreleme dosya sistemi (üzerinden kopyaladığınızda EFS) korumalı veya desteklenmeyen bir yapılandırma StorSimple sanal dizisi dosya sunucusu sonucu üzerinde depolanan dosyalar.  | |

## <a name="next-step"></a>Sonraki adım
[Güncelleştirme 0.4 yükleme](storsimple-virtual-array-install-update-04.md) StorSimple Virtual Array'iniz üzerinde.

## <a name="references"></a>Başvurular
Eski bir sürüm notu için mi arıyorsunuz? Şuraya gidin: 

* [StorSimple sanal dizisi güncelleştirme 0.3 sürüm notları](storsimple-ova-update-03-release-notes.md)
* [StorSimple sanal dizisi güncelleştirme 0.1 ve 0.2 sürüm notları](storsimple-ova-update-01-release-notes.md)
* [StorSimple sanal dizisi genel kullanılabilirlik sürüm notları](storsimple-ova-pp-release-notes.md)

