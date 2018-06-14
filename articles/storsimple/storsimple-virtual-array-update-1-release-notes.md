---
title: StorSimple sanal dizinin Update 1.0 sürüm notları | Microsoft Docs
description: StorSimple sanal güncelleştirme 1.0 çalıştıran dizisi için açık kritik sorunlar ve çözümleri açıklar.
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
ms.openlocfilehash: 777718c4893f1edab3613be5dc941e2716d4c972
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2017
ms.locfileid: "24012109"
---
# <a name="storsimple-virtual-array-update-10-release-notes"></a>StorSimple sanal dizinin Update 1.0 sürüm notları

## <a name="overview"></a>Genel Bakış

Aşağıdaki sürüm notları, kritik açık sorunları ve Microsoft Azure StorSimple sanal dizinin güncelleştirmeleri çözümlenen sorunları tanımlayın.

Sürüm Notları sürekli olarak güncelleştirilir ve geçici bir çözüm gerektiren kritik sorunlar bulundukça olarak eklenir. StorSimple sanal dizinizi dağıtmadan önce dikkatle Sürüm Notları'nda yer alan bilgileri gözden geçirin.

Güncelleştirme 1.0 yazılımı sürümüne karşılık gelen **10.0.10296.0**.

> [!IMPORTANT]
> - Güncelleştirmeleri dağıtıldığından ve aygıtınızı yeniden başlatın. G/ç sürüyor, aygıt kapalı kalma süresi doğurur. Ayrıntılı yönergeler için güncelleştirmeyi uygulamak, Git [güncelleştirme 1.0 yüklemek](storsimple-virtual-array-install-update-1.md).
>
> - Cihazınızı güncelleştirme 0,6 çalışıyorsa, güncelleştirme 1 yalnızca Azure portalı üzerinden kullanılabilir.

## <a name="whats-new-in-update-10"></a>Update 1.0 sürümünde yenilikler nelerdir?

**Güncelleştirme 1.0 StorSimple cihaz Yöneticisi hizmeti kimlik doğrulaması ile ilgili değişiklikleri içerir ve dağıtılması gerekir, erken adresindeki.** Bu güncelleştirme aşağıdaki geliştirmeler ve hata düzeltmeleri içerir:

 - **Kullanım, Azure Active Directory (StorSimple cihaz Yöneticisi hizmeti ile kimlik doğrulaması için AAD)** – öğesinden başlayarak Update 1.0, Azure Active Directory StorSimple cihaz Yöneticisi hizmeti ile kimlik doğrulaması için kullanılır. Eski kimlik doğrulama mekanizması aralık 2017 kullanım dışı kalacaktır. Tüm kullanıcıların yeni kimlik doğrulaması URL'lerini kendi güvenlik duvarı kuralları eklemeniz gerekir. Daha fazla bilgi için listelenen kimlik doğrulaması URL'lerini gidin [StorSimple sanal dizinizi için ağ gereksinimleri](storsimple-ova-system-requirements.md).
 
    Kimlik doğrulaması URL'sini güvenlik duvarı kurallarında dahil edilmezse, kullanıcılar, StorSimple cihazını hizmetiyle kimlik doğrulaması yapamıyor kritik bir uyarı görürsünüz. Kullanıcılar bu uyarıyı görürseniz, yeni kimlik doğrulaması URL'sini eklemeniz gerekir. Daha fazla bilgi için Git [uyarıları ağ StorSimple](storsimple-virtual-array-manage-alerts.md).

 - **Performans geliştirmesi** -bulut okuma, katmanı bileşenleri ve katman aşımı ayarlarına hızının artırmak için çeşitli hata düzeltmeleri yapıldığını. Sonuç olarak, yedekleme ve geri yükleme performans iSCSI ve dosya sunucusu aygıtları için geliştirilmiştir.

 - **Çöp toplama geliştirme** -bu sürüm aygıt ve depolama hesabı iki uzak bölgelerde olduğunda çöp toplama döngüsü performansını hata düzeltmeleri sahiptir.

 - **Günlüğe kaydetme geliştirme** -bu sürüm çöp toplama ve g/ç yolu ile ilgili günlük geliştirmeleri içerir.


## <a name="issues-fixed-in-update-10"></a>Update 1.0 sürümünde giderilen sorunlar

Aşağıdaki tabloda bu sürümde giderilen sorunlar özetini sağlar.

| Hayır. | Özellik | Sorun |
| --- | --- | --- |
| 1 |AAD tabanlı kimlik doğrulaması| Bu sürüm kimlik doğrulaması AAD ile StorSimple Aygıt Yöneticisi'ni sağlar değişiklikler içeriyor.|
| 2 |Çöp toplama| Bu sorunun nerede farklı bölgelerde cihaz ve depolama hesaplarıdır ve böylece faturalama etkileyen aralıklı ağ hataları müşteri bildirilen bir müşteri sitede bildirildi. Bu sürümde, bu sorun giderilmiştir. |
| 3 |Performans| Bu sürüm, geri yükleme/bulut okuma/katmanında sonucunda / performans geliştirmesi katmanı değişiklikler içerir.|
| 4 |Güncelleştirme| Müşteri sitesindeki yedekleme hataları ile sonuçlandı önceki sürümde güncelleştirme ile ilgili bir sorun oluştu. Bu sürümde bu sorun düzeltilmiştir.|

## <a name="known-issues-in-update-10"></a>Güncelleştirme 1.0 bilinen sorunlar

Aşağıdaki tabloda StorSimple sanal dizi için bilinen sorunlar özetini sağlar ve önceki sürümlerden yayın-not ettiğiniz sorunları içerir.

| Hayır. | Özellik | Sorun | Geçici çözüm/açıklamaları |
| --- | --- | --- | --- |
| **1.** |Güncelleştirmeler |Önizleme sürümünde oluşturulan sanal diziler için desteklenen bir genel kullanılabilirlik sürümü güncelleştirilemiyor. |Bu sanal diziler üzerinden bir olağanüstü durum kurtarma (DR) iş akışı kullanarak genel kullanılabilirlik sürümü için başarısız gerekir. |
| **2.** |Sağlanan veri diski |Belirli bir belirtilen boyutta bir veri diski kez sağlamış ve karşılık gelen StorSimple sanal dizinin oluşturulan gerekir değil genişletin veya veri diski küçültmeye. Cihaz yerel katmanlarda tüm verilerin kaybıyla sonuçlarını yapmak çalışılıyor. | |
| **3.** |Grup İlkesi |Bir cihaz etki alanına katılmış olduğunda, bir Grup İlkesi uygulama aygıt işlemi olumsuz etkileyebilir. |Sanal dizinizi kendi kuruluş biriminde (OU) için Active Directory ve hiçbir Grup İlkesi nesneleri (GPO) için uygulanan olduğundan emin olun. |
| **4.** |Yerel web kullanıcı Arabirimi |Internet Explorer (IE ESC) Artırılmış güvenlik özellikleri etkinleştirilirse, sorun giderme veya bakım gibi bazı yerel web kullanıcı Arabirimi sayfalarını düzgün çalışmayabilir. Bu sayfaları düğmeleri de çalışmayabilir. |Internet Explorer Artırılmış güvenlik özelliklerini kapatın. |
| **5.** |Yerel web kullanıcı Arabirimi |Bir Hyper-V sanal makine ağ kullanıcı Arabirimi olarak 10 görüntülenen Web Gbps arabirimleri. |Hyper-V yansımasını davranıştır. Hyper-V sanal ağ bağdaştırıcıları için 10 GB/sn her zaman gösterir. |
| **6.** |Katmanlı birimler veya paylaşımlar |Katmanlı birimlerin desteklenmiyor StorSimple ile çalışan uygulamalar için kilitleme bayt aralığı. Bayt aralığı kilitleme etkinse, StorSimple katmanlama çalışmaz. |Önerilen ölçüleri içerir: <br></br>Uygulama mantığınızın kilitleme bayt aralığı kapatın.<br></br>Yerel olarak sabitlenmiş birimlerin katmanlı birimleri aksine bu uygulama için veri koymak seçin.<br></br>*Uyarı*: bile geri yükleme tamamlanmadan önce kullanarak yerel olarak sabitlenmiş birimleri ve bayt aralığı kilitleme etkin olduğunda, yerel olarak sabitlenmiş birimin çevrimiçi olabilir. Bir geri yükleme devam ediyor, böyle durumlarda, daha sonra tamamlamak geri yüklemek için beklemeniz gerekir. |
| **7.** |Katmanlı paylaşımları |Büyük dosyaları ile çalışma yavaş katmanı genişletmek neden olabilir. |Büyük dosyaları ile çalışırken en büyük dosya paylaşımı boyutunu %3 küçüktür öneririz. |
| **8.** |Kapasite paylaşımlar için kullanılan |Görebileceğiniz paylaşımında hiçbir veri olduğunda tüketim paylaşın. Paylaşımlar için kullanılan kapasitesi meta veriler içerdiğinden bu tüketim ' dir. | |
| **9.** |Olağanüstü durum kurtarma |Yalnızca bir dosya sunucusu aynı etki alanında, kaynak cihaz için olağanüstü durum kurtarma gerçekleştirebilirsiniz. Başka bir etki alanındaki bir hedef cihaz için olağanüstü durum kurtarma, bu sürümde desteklenmiyor. |Bu, bir sonraki sürümde uygulanır. Daha fazla bilgi için Git [, StorSimple sanal dizisi için yük devretme ve olağanüstü durum kurtarma](storsimple-virtual-array-failover-dr.md) |
| **10.** |Azure PowerShell |StorSimple sanal diziler bu sürümde Azure PowerShell aracılığıyla yönetilemez. |Azure portalı ve yerel web kullanıcı Arabirimi sanal aygıtların tüm yönetim yapılması gerekir. |
| **11.** |Parola değiştirme |Sanal dizinin cihaz konsoluna yalnızca tr girişinde kabul-bize biçimi klavye. | |
| **12.** |CHAP |CHAP kimlik oluşturulduktan sonra kaldırılamaz. Ayrıca, CHAP kimlik bilgilerini değiştirirseniz, birimlerin çevrimdışına alın ve ardından çevrimiçi bunları değişikliğin etkili olması çevrimiçine gerekir. |Bu sorunu sonraki bir sürümde değinilmiştir. |
| **13.** |iSCSI sunucusu |' Depolama birimi için iSCSI biriminde görüntülenen kullanılan' StorSimple cihaz Yöneticisi hizmeti ve iSCSI konağının farklı olabilir. |İSCSI konağının filesystem görünüme sahiptir.<br></br>Cihaz en büyük boyutta birimi olduğu zaman ayrılan blok görür. |
| **14.** |Dosya sunucusu |Bir klasördeki bir dosyaya bir alternatif veri akışı (ilişkili ADS) varsa, REKLAMLARI değil yedeklenemez veya olağanüstü durum kurtarma, kopyalama ve öğe düzeyinde Kurtarma geri. | |
| **15.** |Dosya sunucusu |Sembolik bağlantılar desteklenmez. | |
| **16.** |Dosya sunucusu |Dosyaları tarafından Windows şifreleme dosya sistemi (üzerinden kopyaladığınızda EFS) korumalı veya StorSimple sanal dizinin dosya sunucusu sonucu desteklenmeyen bir yapılandırmayla depolanmış.  | |
| **17.** |Güncelleştirmeler |Hata görürseniz kodu: 2359302 (onaltılık 0x240006) bir düzeltme yerel kullanıcı Arabirimi aracılığıyla yüklenmeye çalışırken sonra bu düzeltmenin Cihazınızda zaten yüklenmiş olup olmadığını anlamına gelir.   | |
| **18.** |Güncelleştirmeler |Sanal dizinizi güncelleştirme 1'i yüklemek için yerel web kullanıcı arabirimini kullanıyorsanız, güncelleştirme 0,6 çalıştırdığından emin olunmalıdır. 0,6 güncelleştirme daha düşük bir sürümünü çalıştırıyorsanız, 0,6 ilk güncelleştirmesini ve güncelleştirme 1'i uygulayın. Bir güncelleştirme öncesi 0,6 sürümünden doğrudan Update 1.0 yüklerseniz, bazı güncelleştirmeler eksik ve izleme grafikleri çalışmaz.   | |


## <a name="next-steps"></a>Sonraki adımlar
[1.0 güncelleştirmesi](storsimple-virtual-array-install-update-1.md) , StorSimple sanal dizi.

## <a name="references"></a>Başvurular
Eski bir sürüm notu için mi arıyorsunuz? Şuraya gidin:
*  [StorSimple sanal dizinin güncelleştirme 0,6 sürüm notları](storsimple-virtual-array-update-06-release-notes.md)
* [StorSimple sanal dizinin güncelleştirme 0,5 sürüm notları](storsimple-virtual-array-update-05-release-notes.md)
* [StorSimple sanal dizinin güncelleştirme 0,4 sürüm notları](storsimple-virtual-array-update-04-release-notes.md)
* [StorSimple sanal dizinin güncelleştirme 0,3 sürüm notları](storsimple-ova-update-03-release-notes.md)
* [StorSimple sanal dizinin güncelleştirme 0,1 ve 0.2 sürüm notları](storsimple-ova-update-01-release-notes.md)
* [StorSimple sanal dizinin genel kullanılabilirlik sürüm notları](storsimple-ova-pp-release-notes.md)
