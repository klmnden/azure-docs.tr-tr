---
title: StorSimple Virtual Array güncelleştirme 1.0 sürüm notları | Microsoft Docs
description: StorSimple sanal güncelleştirme 1.0 çalıştıran dizisi için kritik açık sorunlar ve çözümleri açıklanmaktadır.
services: storsimple
documentationcenter: ''
author: alkohli
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: alkohli
ms.openlocfilehash: fdf37a8360ec69017458fabee2a9e16aa2c160aa
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60789680"
---
# <a name="storsimple-virtual-array-update-10-release-notes"></a>StorSimple Virtual Array güncelleştirme 1.0 sürüm notları

## <a name="overview"></a>Genel Bakış

Aşağıdaki sürüm notları, kritik açık sorunlar ve çözümlenen sorunlar için Microsoft Azure StorSimple Virtual Array güncelleştirme belirleyin.

Sürüm Notları sürekli olarak güncelleştirilir ve geçici bir çözüm gerektiren kritik sorunlar bulundukça eklenir. StorSimple Virtual Array'iniz dağıtmadan önce bu sürüm notlarında yer alan bilgileri dikkatle gözden geçirin.

Güncelleştirme 1.0 yazılım sürümüne karşılık gelen **10.0.10296.0**.

> [!IMPORTANT]
> - Güncelleştirmeler kesintiye uğratan bir durumdur ve Cihazınızı yeniden başlatın. Cihaz g/ç ediyor, kapalı kalma süresi artmasına neden olur. Güncelleştirmeyi uygulamak nasıl hakkında ayrıntılı yönergeler için Git [güncelleştirme 1.0 yükleme](storsimple-virtual-array-install-update-1.md).
>
> - Cihazınızı güncelleştirme 0.6 çalışıyorsa güncelleştirme 1 yalnızca Azure portalı üzerinden kullanılabilir.

## <a name="whats-new-in-update-10"></a>Update 1.0 sürümünde yenilikler nelerdir?

**Güncelleştirme 1.0 StorSimple cihaz Yöneticisi hizmeti kimlik doğrulaması ile ilgili değişiklikleri içerir ve dağıtılması gerektiğini, erken konumunda.** Bu güncelleştirme, aşağıdaki geliştirmeleri ve hata düzeltmeleri içerir:

 - **Kullanım, Azure Active Directory (StorSimple cihaz Yöneticisi hizmeti ile kimlik doğrulaması için AAD)** – öğesinden başlayarak güncelleştirme 1.0, Azure Active Directory StorSimple cihaz Yöneticisi hizmeti ile kimlik doğrulaması için kullanılır. Eski kimlik doğrulama mekanizması aralık 2017 tarihinden itibaren kullanımdan kaldırılacaktır. Tüm kullanıcılar, kendi güvenlik duvarı kurallarında yeni kimlik doğrulaması URL'lerini içermelidir. Daha fazla bilgi için listelenen kimlik doğrulaması URL'lerini Git [StorSimple Virtual Array'iniz için ağ gereksinimleri](storsimple-ova-system-requirements.md).
 
    Güvenlik duvarı kurallarında kimlik doğrulama URL'si dahil edilmemişse, kullanıcılar, StorSimple cihazını hizmeti ile doğrulanamadı kritik bir uyarı görürsünüz. Kullanıcılar bu uyarıyı görürseniz, bunlar yeni kimlik doğrulama URL'si eklemeniz gerekir. Daha fazla bilgi için Git [uyarılar ağ StorSimple](storsimple-virtual-array-manage-alerts.md).

 - **Performans iyileştirmesi** -bulut okuma, katman açma ve katmandan çıkarmayı hızı artırmak için çeşitli hata düzeltmeleri yapıldığını. Sonuç olarak, yedekleme ve geri yükleme performansı için iSCSI ve dosya sunucusu cihazları geliştirdi.

 - **Çöp toplama geliştirme** -bu sürüm hata düzeltmeleri, cihaz ve depolama hesabı, uzaktaki iki bölgede olduğunda çöp toplama döngüsü performansını sahiptir.

 - **Günlük geliştirme** -bu sürüm günlük çöp toplama ve g/ç yolu ile ilgili iyileştirmeler içerir.


## <a name="issues-fixed-in-update-10"></a>Update 1.0 sürümünde giderilen sorunlar

Aşağıdaki tabloda, bu sürümde giderilen sorunlar özetini sağlar.

| Hayır. | Özellik | Sorun |
| --- | --- | --- |
| 1 |AAD tabanlı kimlik doğrulaması| Bu sürüm, kimlik doğrulaması StorSimple cihaz Yöneticisi ile AAD veren değişiklikleri içerir.|
| 2 |Çöp toplama| Bu sorun, burada cihaz ve depolama hesapları farklı bölgelerde durumdadır ve müşteri faturalandırma böylece etkileyen aralıklı ağ hataları bildirilen bir müşteri sitede bildirildi. Bu sürümde, bu sorunu düzeltildi. |
| 3 |Performans| Bu sürüm, geri yükleme/bulut okuma/katmanında neden / performans geliştirmesi katmanı değişiklikler içeriyor.|
| 4 |Güncelleştirme| Müşteri sitesindeki yedekleme hataları içinde sonuçlanan önceki sürümde güncelleştirme ile ilgili bir sorun oluştu. Bu sürümde bu sorun düzeltilmiştir.|

## <a name="known-issues-in-update-10"></a>Güncelleştirme 1.0 bilinen sorunlar

Aşağıdaki tabloda StorSimple sanal dizisi için bilinen sorunların bir Özet sağlar ve önceki sürümlerden yayın belirtildiği sorunları içerir.

| Hayır. | Özellik | Sorun | Geçici çözüm/açıklamaları |
| --- | --- | --- | --- |
| **1.** |Güncelleştirmeler |Önizleme sürümünde oluşturulan sanal diziler için desteklenen genel kullanılabilirlik sürümü güncelleştirilemiyor. |Bu sanal diziler için bir olağanüstü durum kurtarma (DR) iş akışı kullanarak genel kullanım sürümünde yük devretti gerekir. |
| **2.** |Sağlanan veri diski |Bir veri diskinin belirli bir belirtilen boyutta bir kez sağladığınız ve karşılık gelen StorSimple sanal dizisi oluşturulan gerekir değil genişletin veya veri diski küçültmeye. Cihaz yerel katmanlarda tüm verilerin kaybıyla sonuçları yapmak çalışıyor. | |
| **3.** |Grup ilkesi |Bir cihaz etki alanına katılmış olduğunda, bir Grup İlkesi uygulama cihaz işlemi olumsuz yönde etkileyebilir. |Sanal diziniz kendi kuruluş birimi (OU) için Active Directory olduğundan ve hiçbir Grup İlkesi nesneleri (GPO) uygulanmış emin olun. |
| **4.** |Yerel web kullanıcı Arabirimi |Internet Explorer (IE ESC) Artırılmış güvenlik özellikleri etkinleştirilirse, bazı sorun giderme veya bakım gibi yerel web kullanıcı Arabirimi sayfalarını düzgün çalışmayabilir. Bu sayfa düğmelerini de çalışmayabilir. |Internet Explorer Gelişmiş güvenlik özelliklerini devre dışı bırakın. |
| **5.** |Yerel web kullanıcı Arabirimi |Bir Hyper-V sanal makine, GB/sn ağ arabirimlerinin de kullanıcı Arabirimi olarak 10 görüntülenen web arabirimleri. |Bir yansıma Hyper-V, davranıştır. Hyper-V, sanal ağ bağdaştırıcıları için 10 GB/sn her zaman gösterilir. |
| **6.** |Katmanlı birimler veya paylaşımlar |Katmanlı birimlerin desteklenmiyor StorSimple ile çalışan uygulamalar için kilitleme bayt aralığı. Bayt aralığı kilitleme etkinse, StorSimple katmanlama çalışmaz. |Önerilen ölçüleri içerir: <br></br>Bayt aralığı uygulama mantığınızın kilitleme devre dışı bırakın.<br></br>Bu uygulama için verileri yerel olarak sabitlenmiş birim katmanlı birimlerin yerine koymak seçin.<br></br>*Uyarı*: Geri yükleme tamamlamadan önce kullanarak yerel olarak sabitlenmiş birimler ve bayt aralığı kilitleme etkin olduğunda, yerel olarak sabitlenmiş birimin çevrimiçi olabilir. Bir geri yükleme devam ediyor, bu gibi durumlarda, daha sonra tamamlamak geri yüklemek için beklemeniz gerekir. |
| **7.** |Katmanlı paylaşımları |Büyük dosyaları ile çalışma, yavaş bir katmanın ölçeğini sonuçlanabilir. |Büyük dosyalarla çalışırken, en büyük dosya paylaşım boyutunun %3 küçükse öneririz. |
| **8.** |Kapasite paylaşımlar için kullanılan |Görebileceğiniz paylaşımında veri olduğunda tüketim paylaşın. Kullanılan kapasite paylaşımları için meta veriler içeren bu tüketim olmasıdır. | |
| **9.** |Olağanüstü durum kurtarma |Yalnızca dosya sunucusu aynı etki, kaynak cihaz için olağanüstü durum kurtarma gerçekleştirebilirsiniz. Olağanüstü durum kurtarma için başka bir etki alanındaki bir hedef cihaz, bu sürümde desteklenmiyor. |Bu, bir sonraki sürümde uygulanır. Daha fazla bilgi için Git [StorSimple Virtual Array'iniz için yük devretme ve olağanüstü durum kurtarma](storsimple-virtual-array-failover-dr.md) |
| **10.** |Azure PowerShell |StorSimple sanal dizilerine, bu sürüm Azure PowerShell aracılığıyla yönetilemez. |Tüm sanal cihazların yönetimini Azure portalı ve yerel web UI yapılmalıdır. |
| **11.** |Parola değiştirme |Sanal dizi cihaz konsolu yalnızca en girişi kabul-bize klavye biçimi. | |
| **12.** |CHAP |CHAP kimlik oluşturulduktan sonra kaldırılamaz. Ayrıca, CHAP kimlik bilgilerini değiştirirseniz, birimlerin çevrimdışına alın ve değişikliğin etkili olması çevrimiçi bunları getirmek gerekir. |Bu sorun, bir sonraki sürümde değinilmiştir. |
| **13.** |iSCSI sunucusu |'Depolama görüntülenen bir iSCSI birimi için kullanılan' StorSimple cihaz Yöneticisi hizmeti ve iSCSI konağının farklı olabilir. |İSCSI ana bilgisayar dosya sistemi görünüme sahiptir.<br></br>Cihaz, birim maksimum boyutta olduğunda ayrılan blokları görür. |
| **14.** |Dosya sunucusu |Bir klasördeki bir dosyaya bir alternatif veri Stream (ilişkili REKLAM) varsa, REKLAM değil yedeklenen veya olağanüstü durum kurtarma, kopyalama ve öğe düzeyinde Kurtarma ile geri. | |
| **15.** |Dosya sunucusu |Sembolik bağlantılar desteklenmez. | |
| **16.** |Dosya sunucusu |Tarafından Windows şifreleme dosya sistemi (üzerinden kopyaladığınızda EFS) korumalı veya desteklenmeyen bir yapılandırma StorSimple sanal dizisi dosya sunucusu sonucu üzerinde depolanan dosyalar.  | |
| **17.** |Güncelleştirmeler |Hata görürseniz kod: 2359302 (onaltılık 0x240006) yerel UI aracılığıyla bir düzeltme yüklenmeye çalışılırken sonra bu düzeltmenin zaten Cihazınızda yüklenmiş olup olmadığını gösterir.   | |
| **18.** |Güncelleştirmeler |Sanal diziniz güncelleştirme 1'i yüklemek için yerel web kullanıcı arabirimini kullanıyorsanız, güncelleştirme 0.6 çalıştırdığınız emin olmanız gerekir. Güncelleştirme 0.6 daha düşük bir sürümünü çalıştırıyorsanız, güncelleştirme 0.6 önce yüklemeniz ve ardından güncelleştirme 1 uygulayın. Bir güncelleştirme öncesi 0,6 sürümünden doğrudan güncelleştirme 1.0 yüklerseniz, bazı güncelleştirmeler eksik ve izleme grafiklerini çalışmaz.   | |


## <a name="next-steps"></a>Sonraki adımlar
[1.0 güncelleştirmesi](storsimple-virtual-array-install-update-1.md) StorSimple Virtual Array'iniz üzerinde.

## <a name="references"></a>Başvurular
Eski bir sürüm notu için mi arıyorsunuz? Şuraya gidin:
*  [StorSimple sanal dizisi güncelleştirme 0.6 sürüm notları](storsimple-virtual-array-update-06-release-notes.md)
* [StorSimple sanal dizisi güncelleştirme 0.5 sürüm notları](storsimple-virtual-array-update-05-release-notes.md)
* [StorSimple sanal dizisi güncelleştirme 0,4 sürüm notları](storsimple-virtual-array-update-04-release-notes.md)
* [StorSimple sanal dizisi güncelleştirme 0.3 sürüm notları](storsimple-ova-update-03-release-notes.md)
* [StorSimple sanal dizisi güncelleştirme 0.1 ve 0.2 sürüm notları](storsimple-ova-update-01-release-notes.md)
* [StorSimple sanal dizisi genel kullanılabilirlik sürüm notları](storsimple-ova-pp-release-notes.md)
