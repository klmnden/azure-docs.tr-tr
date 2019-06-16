---
title: StorSimple sanal dizisi güncelleştirme 0.6 sürüm notları | Microsoft Docs
description: StorSimple sanal güncelleştirme 0.6 çalıştıran dizisi için kritik açık sorunlar ve çözümleri açıklanmaktadır.
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
ms.date: 05/24/2017
ms.author: alkohli
ms.openlocfilehash: e984531feced2d61332e4c399848c12cd245a34a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60870715"
---
# <a name="storsimple-virtual-array-update-06-release-notes"></a>StorSimple sanal dizisi güncelleştirme 0.6 sürüm notları

## <a name="overview"></a>Genel Bakış

Aşağıdaki sürüm notları, kritik açık sorunlar ve çözümlenen sorunlar için Microsoft Azure StorSimple Virtual Array güncelleştirme belirleyin.

Sürüm Notları sürekli olarak güncelleştirilir ve geçici bir çözüm gerektiren kritik sorunlar bulundukça eklenir. StorSimple Virtual Array'iniz dağıtmadan önce bu sürüm notlarında yer alan bilgileri dikkatle gözden geçirin.

Güncelleştirme 0.6 yazılım sürümüne karşılık gelen **10.0.10293.0**.

> [!IMPORTANT]
> - Güncelleştirmeler kesintiye uğratan bir durumdur ve Cihazınızı yeniden başlatın. Cihaz g/ç ediyor, kapalı kalma süresi artmasına neden olur. Güncelleştirmeyi uygulamak nasıl hakkında ayrıntılı yönergeler için Git [güncelleştirme 0.6](storsimple-virtual-array-install-update-06.md).
>
> - İçerdiği gibi kritik güvenlik düzeltmelerini hemen güncelleştirme 0.6 yüklemenizi öneririz.


## <a name="whats-new-in-the-update-06"></a>Güncelleştirme 0.6 yenilikler nelerdir?
Güncelleştirme 0.6 kritik güncelleştirme ve hemen dağıtılmalıdır. Bu güncelleştirme aşağıdaki düzeltmeleri içerir: 

- **Windows güvenlik düzeltmelerini** -bu sürüm sahiptir **Windows kritik güvenlik düzeltmelerini**. Güvenlik sorunlarını ve ilişkili düzeltmeler hakkında daha fazla bilgi için aşağıdaki güvenlik güncelleştirmelerini gözden geçirin:
    - [Aralık 2016 güvenlik yalnızca kalite güncelleştirme için Windows 8.1 ve Windows Server 2012 R2](https://support.microsoft.com/help/3205400/december-2016-security-only-quality-update-for-windows-8.1-and-windows-server-2012-r2)
    - [Mart 2017 güvenlik yalnızca kalite güncelleştirme için Windows 8.1 ve Windows Server 2012 R2](https://support.microsoft.com/help/4012213/march-2017-security-only-quality-update-for-windows-8-1-and-windows-server-2012-r23)
    - [9 Mayıs 2017 — KB4019213 (yalnızca güvenlik güncelleştirmesi)](https://support.microsoft.com/help/4019213/windows-8-update-kb4019213)

- **Düzeltme geri** -önceki sürümlerde, geri yüklemenin tamamlanmasını önleyen bir hata oluştu. Bu sürümde bu hata düzeltildi.


## <a name="issues-fixed-in-the-update-06"></a>Güncelleştirme 0.6 giderilen sorunlar

Aşağıdaki tabloda, bu sürümde giderilen sorunlar özetini sağlar.

| Hayır. | Özellik | Sorun |
| --- | --- | --- |
| 1 |Güvenlik| Bu sürüm, önemli Windows Güvenlik güncelleştirmelerini içerir. Bu güncelleştirme hemen yüklemenizi öneririz.|
| 2 |Geri Yükleme| Geri yükleme sırasında geri yükleme işinin tamamlanmasını önleyen bir yarış durumu oluştu. Hata düzeltmesi bu yarış durumu ele alır.|


## <a name="known-issues-in-the-update-06"></a>Güncelleştirme 0.6'de bilinen sorunlar

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
| **8.** |Kapasite paylaşımlar için kullanılan |Görebileceğiniz paylaşımında veri olduğunda tüketim paylaşın. Kullanılan kapasite paylaşımları için meta veriler içeren bu tüketim olmasıdır. | |
| **9.** |Olağanüstü durum kurtarma |Yalnızca dosya sunucusu aynı etki, kaynak cihaz için olağanüstü durum kurtarma gerçekleştirebilirsiniz. Olağanüstü durum kurtarma için başka bir etki alanındaki bir hedef cihaz, bu sürümde desteklenmiyor. |Bu, bir sonraki sürümde uygulanır. Daha fazla bilgi için Git [StorSimple Virtual Array'iniz için yük devretme ve olağanüstü durum kurtarma](storsimple-virtual-array-failover-dr.md) |
| **10.** |Azure PowerShell |StorSimple sanal cihazları, bu sürüm Azure PowerShell aracılığıyla yönetilemez. |Tüm sanal cihazların yönetimini Azure portalı ve yerel web UI yapılmalıdır. |
| **11.** |Parola değiştirme |Sanal dizi cihaz konsolu yalnızca en girişi kabul-bize klavye biçimi. | |
| **12.** |CHAP |CHAP kimlik oluşturulduktan sonra kaldırılamaz. Ayrıca, CHAP kimlik bilgilerini değiştirirseniz, birimlerin çevrimdışına alın ve değişikliğin etkili olması çevrimiçi bunları getirmek gerekir. |Bu sorun, bir sonraki sürümde değinilmiştir. |
| **13.** |iSCSI sunucusu |'Depolama görüntülenen bir iSCSI birimi için kullanılan' StorSimple cihaz Yöneticisi hizmeti ve iSCSI konağının farklı olabilir. |İSCSI ana bilgisayar dosya sistemi görünüme sahiptir.<br></br>Cihaz, birim maksimum boyutta olduğunda ayrılan blokları görür. |
| **14.** |Dosya sunucusu |Bir klasördeki bir dosyaya bir alternatif veri Stream (ilişkili REKLAM) varsa, REKLAM değil yedeklenen veya olağanüstü durum kurtarma, kopyalama ve öğe düzeyinde Kurtarma ile geri. | |
| **15.** |Dosya sunucusu |Sembolik bağlantılar desteklenmez. | |
| **16.** |Dosya sunucusu |Tarafından Windows şifreleme dosya sistemi (üzerinden kopyaladığınızda EFS) korumalı veya desteklenmeyen bir yapılandırma StorSimple sanal dizisi dosya sunucusu sonucu üzerinde depolanan dosyalar.  | |
| **17.** |Güncelleştirmeler |Hata görürseniz kod: 2359302 (onaltılık 0x240006) yerel UI aracılığıyla bir düzeltme yüklenmeye çalışılırken sonra bu düzeltmenin zaten Cihazınızda yüklenmiş olup olmadığını gösterir.   | |

## <a name="next-step"></a>Sonraki adım
[Güncelleştirme 0.6 yükleme](storsimple-virtual-array-install-update-06.md) StorSimple Virtual Array'iniz üzerinde.

## <a name="references"></a>Başvurular
Eski bir sürüm notu için mi arıyorsunuz? Şuraya gidin:

* [StorSimple sanal dizisi güncelleştirme 0.5 sürüm notları](storsimple-virtual-array-update-05-release-notes.md)
* [StorSimple sanal dizisi güncelleştirme 0,4 sürüm notları](storsimple-virtual-array-update-04-release-notes.md)
* [StorSimple sanal dizisi güncelleştirme 0.3 sürüm notları](storsimple-ova-update-03-release-notes.md)
* [StorSimple sanal dizisi güncelleştirme 0.1 ve 0.2 sürüm notları](storsimple-ova-update-01-release-notes.md)
* [StorSimple sanal dizisi genel kullanılabilirlik sürüm notları](storsimple-ova-pp-release-notes.md)

