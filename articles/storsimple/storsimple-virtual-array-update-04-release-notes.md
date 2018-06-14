---
title: StorSimple sanal dizinin güncelleştirme 0,4 sürüm notları | Microsoft Docs
description: StorSimple sanal güncelleştirme 0.4 çalışan dizisi için açık kritik sorunlar ve çözümleri açıklar.
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
ms.openlocfilehash: cc2b025b7f3e28954c7f95409ffab03e5cbcf13d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23927764"
---
# <a name="storsimple-virtual-array-update-04-release-notes"></a>StorSimple sanal dizinin güncelleştirme 0,4 sürüm notları

## <a name="overview"></a>Genel Bakış

Aşağıdaki sürüm notları, kritik açık sorunları ve Microsoft Azure StorSimple sanal dizinin güncelleştirmeleri çözümlenen sorunları tanımlayın.

Sürüm Notları sürekli olarak güncelleştirilir ve geçici bir çözüm gerektiren kritik sorunlar bulundukça olarak eklenir. StorSimple sanal dizinizi dağıtmadan önce dikkatle Sürüm Notları'nda yer alan bilgileri gözden geçirin.

Güncelleştirme 0.4 yazılımı sürümüne karşılık gelen **10.0.10289.0**.

> [!NOTE]
> Güncelleştirmeleri dağıtıldığından ve aygıtınızı yeniden başlatın. G/ç sürüyor, aygıt kapalı kalma süresi doğurur.


## <a name="whats-new-in-the-update-04"></a>0.4 güncelleştirmesindeki yenilikler
0.4 öncelikle birkaç geliştirmeleri ile birlikte bir hata düzeltmesi yapı güncelleştirmesidir. Bu sürümde, yedekleme hataları önceki sürümde sonuçta çeşitli hatalar giderilmiştir. Ana geliştirmeler ve hata düzeltmeleri aşağıdaki gibidir:

- **Yedekleme performans geliştirmeleri** -bu sürümde yedekleme performansını artırmak için birkaç önemli geliştirmeler yaptı. Sonuç olarak, çok sayıda dosya içeren yedeklemeler, tam ve artımlı yedeklemeler için tamamlamak için geçen süre, önemli ölçüde azalma bakın.

- **Gelişmiş geri yükleme performans** -bu sürüm, çok sayıda dosya kullanırken geri yükleme performansını önemli ölçüde artıran geliştirmeler içerir. 2-4 milyon dosyaları kullanıyorsanız, 16 GB RAM yenilikleri görmek için sanal bir dizi sağlamanızı öneririz. 2 milyondan az dosyaları kullanırken, en düşük gereksinim sanal makine için 8 GB RAM olmaya devam ediyor.

- **Destek Paketi geliştirmeleri** -disk, CPU, bellek, ağ ve dolayısıyla aygıt sorunları tanılama/ayıklama işlemi geliştirme destek paketi bulutunu istatistikleri günlüğü geliştirmeleri içerir.

- **Sınır yerel olarak sabitlenmiş 200 GB iSCSI birimlere** -yerel olarak sabitlenmiş birimler için 200 GB iSCSI birime, StorSimple sanal dizisinde sınırlamak olan öneririz. Katmanlı birimleri için yerel ayırma sağlanan birim boyutu 10 yüzdesi olacak şekilde devam eder, ancak 200 GB olarak tutulabilir. 

- **Yedekleme ile ilgili hata düzeltmeleri** - önceki sürümlerinde, yazılım, yedekleme hataları neden olacak yedeklemeler ilgili sorunlar vardı. Bu hatalar bu sürümde giderilmiştir.


## <a name="issues-fixed-in-the-update-04"></a>0.4 güncelleştirmede giderilen sorunlar

Aşağıdaki tabloda bu sürümde giderilen sorunlar özetini sağlar.

| Hayır. | Özellik | Sorun |
| --- | --- | --- |
| 1 |Yedekleme performansı|Önceki sürümlerde, çok sayıda dosya içeren yedeklemeleri (gün sırasına göre) tamamlanması uzun zaman alır. Bu sürümde, tam ve artımlı yedeklemeler tamamlanma süresi, önemli ölçüde azalma bakın. |
| 2 |Destek Paketi|Disk, CPU, bellek, ağ ve bulut İstatistikleri şimdi destek paketleri cihaz sorunları gidermeye çok etkili yapmadan destek günlükleri oturum açmış.|
| 3 |Backup |Önceki sürümlerde, yedeklemeleri uzun süre çalışan yedekleme hatalarına kaynaklanan aygıtta alanı crunch neden olabilir. Bu hata, bir seferde sıraya almak en fazla 5 yedeklemeleri izin vererek bu sürümde değinilmiştir.|
| 4 |iSCSI | Önceki sürümlerde, katmanlı ya da yerel olarak sabitlenmiş birimleri için yerel ayırma sağlanan birim boyutu % 10 idi. Bu sürümde, yerel ayırma (yerel olarak sabitlenmiş veya katmanlı) tüm iSCSI birimleri için % 10'en fazla fazla ile sınırlıdır 200 GB (2 TB'den büyük katmanlı birimleri) daha fazla alan yerel diskteki böylece boşaltma. Bu sürümde yerel olarak sabitlenmiş birimlerin 200 GB ile sınırlı olmasını öneririz.|


## <a name="known-issues-in-the-update-04"></a>Güncelleştirme 0.4 bilinen sorunlar

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
| **8.** |Kapasite paylaşımlar için kullanılan |Görebileceğiniz paylaşımında hiçbir veri olduğunda tüketim paylaşın. Paylaşımlar için kullanılan kapasitesi meta veriler içerdiğinden budur. | |
| **9.** |Olağanüstü durum kurtarma |Yalnızca bir dosya sunucusu aynı etki alanında, kaynak cihaz için olağanüstü durum kurtarma gerçekleştirebilirsiniz. Başka bir etki alanındaki bir hedef cihaz için olağanüstü durum kurtarma, bu sürümde desteklenmiyor. |Bu, bir sonraki sürümde uygulanır. |
| **10.** |Azure PowerShell |StorSimple sanal cihazlar, bu sürümde Azure PowerShell aracılığıyla yönetilemez. |Klasik Azure portalı ve yerel web kullanıcı Arabirimi sanal aygıtların tüm yönetim yapılması gerekir. |
| **11.** |Parola değiştirme |Sanal dizinin aygıt konsolu yalnızca en-US klavye biçiminde giriş kabul eder. | |
| **12.** |CHAP |CHAP kimlik oluşturulduktan sonra kaldırılamaz. Ayrıca, CHAP kimlik bilgilerini değiştirirseniz, birimlerin çevrimdışına alın ve ardından çevrimiçi bunları değişikliğin etkili olması çevrimiçine gerekir. |Bu sorunu sonraki bir sürümde değinilmiştir. |
| **13.** |iSCSI sunucusu |' Depolama birimi için iSCSI biriminde görüntülenen kullanılan' StorSimple Yöneticisi hizmeti ve iSCSI konağının farklı olabilir. |İSCSI konağının filesystem görünüme sahiptir.<br></br>Cihaz en büyük boyutta birimi olduğu zaman ayrılan blok görür. |
| **14.** |Dosya sunucusu |Bir klasördeki bir dosyaya bir alternatif veri akışı (ilişkili ADS) varsa, REKLAMLARI değil yedeklenemez veya olağanüstü durum kurtarma, kopyalama ve öğe düzeyinde Kurtarma geri. | |
| **15.** |Dosya sunucusu |Sembolik bağlantılar desteklenmez. | |
| **16.** |Dosya sunucusu |Dosyaları tarafından Windows şifreleme dosya sistemi (üzerinden kopyaladığınızda EFS) korumalı veya StorSimple sanal dizinin dosya sunucusu sonucu desteklenmeyen bir yapılandırmayla depolanmış.  | |

## <a name="next-step"></a>Sonraki adım
[0.4 güncelleştirmesini](storsimple-virtual-array-install-update-04.md) , StorSimple sanal dizi.

## <a name="references"></a>Başvurular
Eski bir sürüm notu için mi arıyorsunuz? Şuraya gidin: 

* [StorSimple sanal dizinin güncelleştirme 0,3 sürüm notları](storsimple-ova-update-03-release-notes.md)
* [StorSimple sanal dizinin güncelleştirme 0,1 ve 0.2 sürüm notları](storsimple-ova-update-01-release-notes.md)
* [StorSimple sanal dizinin genel kullanılabilirlik sürüm notları](storsimple-ova-pp-release-notes.md)

