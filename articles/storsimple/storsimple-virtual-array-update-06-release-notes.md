---
title: "StorSimple sanal dizinin güncelleştirme 0,6 sürüm notları | Microsoft Docs"
description: "StorSimple sanal güncelleştirme 0,6 çalışan dizisi için açık kritik sorunlar ve çözümleri açıklar."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/24/2017
ms.author: alkohli
ms.openlocfilehash: af202cf652300ee7897eb2dede33f38058fc2837
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="storsimple-virtual-array-update-06-release-notes"></a>StorSimple sanal dizinin güncelleştirme 0,6 sürüm notları

## <a name="overview"></a>Genel Bakış

Aşağıdaki sürüm notları, kritik açık sorunları ve Microsoft Azure StorSimple sanal dizinin güncelleştirmeleri çözümlenen sorunları tanımlayın.

Sürüm Notları sürekli olarak güncelleştirilir ve geçici bir çözüm gerektiren kritik sorunlar bulundukça olarak eklenir. StorSimple sanal dizinizi dağıtmadan önce dikkatle Sürüm Notları'nda yer alan bilgileri gözden geçirin.

Güncelleştirme 0,6 yazılımı sürümüne karşılık gelen **10.0.10293.0**.

> [!IMPORTANT]
> - Güncelleştirmeleri dağıtıldığından ve aygıtınızı yeniden başlatın. G/ç sürüyor, aygıt kapalı kalma süresi doğurur. Ayrıntılı yönergeler için güncelleştirmeyi uygulamak, Git [güncelleştirme 0,6](storsimple-virtual-array-install-update-06.md).
>
> - İçerdiğinden kritik güvenlik düzeltmelerini hemen güncelleştirme 0,6 yüklemenizi öneririz.


## <a name="whats-new-in-the-update-06"></a>0,6 güncelleştirmesindeki yenilikler
Güncelleştirme 0,6 kritik güncelleştirme ve hemen dağıtılmalıdır. Bu güncelleştirme aşağıdaki düzeltmeleri içerir: 

- **Windows güvenlik düzeltmelerini** -bu sürüme sahip **Windows kritik güvenlik düzeltmelerini**. Güvenlik sorunlarını ve ilişkili düzeltmeler hakkında daha fazla bilgi için aşağıdaki güvenlik güncelleştirmelerini gözden geçirin:
    - [Aralık 2016 güvenlik yalnızca kalite güncelleştirmesi Windows 8.1 ve Windows Server 2012 R2](https://support.microsoft.com/help/3205400/december-2016-security-only-quality-update-for-windows-8.1-and-windows-server-2012-r2)
    - [Mart 2017 güvenlik yalnızca kalite güncelleştirmesi Windows 8.1 ve Windows Server 2012 R2](https://support.microsoft.com/help/4012213/march-2017-security-only-quality-update-for-windows-8-1-and-windows-server-2012-r23)
    - [9 May 2017 — KB4019213 (yalnızca güvenlik güncelleştirmesi)](https://support.microsoft.com/help/4019213/windows-8-update-kb4019213)

- **Düzeltme geri** -önceki sürümlerde, geri yüklemenin tamamlanmasını önleyen bir hata oluştu. Bu hata, bu sürümde düzeltilmiştir.


## <a name="issues-fixed-in-the-update-06"></a>0,6 güncelleştirmede giderilen sorunlar

Aşağıdaki tabloda bu sürümde giderilen sorunlar özetini sağlar.

| Hayır. | Özellik | Sorun |
| --- | --- | --- |
| 1 |Güvenlik| Bu sürüm kritik Windows Güvenlik güncelleştirmeleri içerir. Bu güncelleştirmeyi hemen yüklemenizi öneririz.|
| 2 |Geri Yükleme| Bir geri yükleme sırasında geri yükleme işi tamamlanmasını önleyen bir yarış durumu oluştu. Hata düzeltmesi bu yarış durumu giderir.|


## <a name="known-issues-in-the-update-06"></a>0,6 güncelleştirmedeki bilinen sorunlar

Aşağıdaki tabloda StorSimple sanal dizi için bilinen sorunlar özetini sağlar ve önceki sürümlerden yayın-not ettiğiniz sorunları içerir.

| Hayır. | Özellik | Sorun | Geçici çözüm/açıklamaları |
| --- | --- | --- | --- |
| **1.** |Güncelleştirmeler |Önizleme sürümünde oluşturulan sanal cihazlar için desteklenen bir genel kullanılabilirlik sürümü güncelleştirilemiyor. |Bu sanal cihazlar üzerinden bir olağanüstü durum kurtarma (DR) iş akışı kullanarak genel kullanılabilirlik sürümü için başarısız gerekir. |
| **2.** |Sağlanan veri diski |Belirli bir belirtilen boyutta bir veri diski sağlamış ve karşılık gelen StorSimple sanal cihaz oluşturulan, gerekir değil genişletin veya veri diski küçültmeye sonra. Cihaz yerel katmanlarda tüm verilerin kaybıyla sonuçlarını yapmak çalışılıyor. | |
| **3.** |Grup İlkesi |Bir cihaz etki alanına katılmış olduğunda, bir Grup İlkesi uygulama aygıt işlemi olumsuz etkileyebilir. |Sanal dizinizi kendi kuruluş biriminde (OU) için Active Directory ve hiçbir Grup İlkesi nesneleri (GPO) için uygulanan olduğundan emin olun. |
| **4.** |Yerel web kullanıcı Arabirimi |Internet Explorer (IE ESC) Artırılmış güvenlik özellikleri etkinleştirilirse, sorun giderme veya bakım gibi bazı yerel web kullanıcı Arabirimi sayfalarını düzgün çalışmayabilir. Bu sayfaları düğmeleri de çalışmayabilir. |Internet Explorer Artırılmış güvenlik özelliklerini kapatın. |
| **5.** |Yerel web kullanıcı Arabirimi |Bir Hyper-V sanal makine ağ kullanıcı Arabirimi olarak 10 görüntülenen Web Gbps arabirimleri. |Hyper-V yansımasını davranıştır. Hyper-V sanal ağ bağdaştırıcıları için 10 GB/sn her zaman gösterir. |
| **6.** |Katmanlı birimler veya paylaşımlar |Katmanlı birimlerin desteklenmiyor StorSimple ile çalışan uygulamalar için kilitleme bayt aralığı. Bayt aralığı kilitleme etkinse, StorSimple katmanlama çalışmaz. |Önerilen ölçüleri içerir: <br></br>Uygulama mantığınızın kilitleme bayt aralığı kapatın.<br></br>Yerel olarak sabitlenmiş birimlerin katmanlı birimleri aksine bu uygulama için veri koymak seçin.<br></br>*Uyarı*: bile geri yükleme tamamlanmadan önce kullanarak yerel olarak sabitlenmiş birimleri ve bayt aralığı kilitleme etkin olduğunda, yerel olarak sabitlenmiş birimin çevrimiçi olabilir. Bir geri yükleme devam ediyor, böyle durumlarda, daha sonra tamamlamak geri yüklemek için beklemeniz gerekir. |
| **7.** |Katmanlı paylaşımları |Büyük dosyaları ile çalışma yavaş katmanı genişletmek neden olabilir. |Büyük dosyaları ile çalışırken en büyük dosya paylaşımı boyutunu %3 küçüktür öneririz. |
| **8.** |Kapasite paylaşımlar için kullanılan |Görebileceğiniz paylaşımında hiçbir veri olduğunda tüketim paylaşın. Paylaşımlar için kullanılan kapasitesi meta veriler içerdiğinden bu tüketim ' dir. | |
| **9.** |Olağanüstü durum kurtarma |Yalnızca bir dosya sunucusu aynı etki alanında, kaynak cihaz için olağanüstü durum kurtarma gerçekleştirebilirsiniz. Başka bir etki alanındaki bir hedef cihaz için olağanüstü durum kurtarma, bu sürümde desteklenmiyor. |Bu, bir sonraki sürümde uygulanır. Daha fazla bilgi için Git [, StorSimple sanal dizisi için yük devretme ve olağanüstü durum kurtarma](storsimple-virtual-array-failover-dr.md) |
| **10.** |Azure PowerShell |StorSimple sanal cihazlar, bu sürümde Azure PowerShell aracılığıyla yönetilemez. |Azure portalı ve yerel web kullanıcı Arabirimi sanal aygıtların tüm yönetim yapılması gerekir. |
| **11.** |Parola değiştirme |Sanal dizinin cihaz konsoluna yalnızca tr girişinde kabul-bize biçimi klavye. | |
| **12.** |CHAP |CHAP kimlik oluşturulduktan sonra kaldırılamaz. Ayrıca, CHAP kimlik bilgilerini değiştirirseniz, birimlerin çevrimdışına alın ve ardından çevrimiçi bunları değişikliğin etkili olması çevrimiçine gerekir. |Bu sorunu sonraki bir sürümde değinilmiştir. |
| **13.** |iSCSI sunucusu |' Depolama birimi için iSCSI biriminde görüntülenen kullanılan' StorSimple cihaz Yöneticisi hizmeti ve iSCSI konağının farklı olabilir. |İSCSI konağının filesystem görünüme sahiptir.<br></br>Cihaz en büyük boyutta birimi olduğu zaman ayrılan blok görür. |
| **14.** |Dosya sunucusu |Bir klasördeki bir dosyaya bir alternatif veri akışı (ilişkili ADS) varsa, REKLAMLARI değil yedeklenemez veya olağanüstü durum kurtarma, kopyalama ve öğe düzeyinde Kurtarma geri. | |
| **15.** |Dosya sunucusu |Sembolik bağlantılar desteklenmez. | |
| **16.** |Dosya sunucusu |Dosyaları tarafından Windows şifreleme dosya sistemi (üzerinden kopyaladığınızda EFS) korumalı veya StorSimple sanal dizinin dosya sunucusu sonucu desteklenmeyen bir yapılandırmayla depolanmış.  | |
| **17.** |Güncelleştirmeler |Hata görürseniz kodu: 2359302 (onaltılık 0x240006) bir düzeltme yerel kullanıcı Arabirimi aracılığıyla yüklenmeye çalışırken sonra bu düzeltmenin Cihazınızda zaten yüklenmiş olup olmadığını anlamına gelir.   | |

## <a name="next-step"></a>Sonraki adım
[0,6 güncelleştirmesini](storsimple-virtual-array-install-update-06.md) , StorSimple sanal dizi.

## <a name="references"></a>Başvurular
Eski bir sürüm notu için mi arıyorsunuz? Şuraya gidin:

* [StorSimple sanal dizinin güncelleştirme 0,5 sürüm notları](storsimple-virtual-array-update-05-release-notes.md)
* [StorSimple sanal dizinin güncelleştirme 0,4 sürüm notları](storsimple-virtual-array-update-04-release-notes.md)
* [StorSimple sanal dizinin güncelleştirme 0,3 sürüm notları](storsimple-ova-update-03-release-notes.md)
* [StorSimple sanal dizinin güncelleştirme 0,1 ve 0.2 sürüm notları](storsimple-ova-update-01-release-notes.md)
* [StorSimple sanal dizinin genel kullanılabilirlik sürüm notları](storsimple-ova-pp-release-notes.md)

