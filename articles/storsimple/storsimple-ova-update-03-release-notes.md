---
title: "StorSimple sanal dizinin güncelleştirmeleri sürüm notları | Microsoft Docs"
description: "StorSimple sanal güncelleştirme 0.3 çalıştıran dizisi için açık kritik sorunlar ve çözümleri açıklar."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: b197651a-3c40-4185-b23d-4c8f22cfa8f4
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/15/2016
ms.author: alkohli
ms.openlocfilehash: fe9d4f6b232e9abcf1fe9fc5657044b6c72fedb8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="storsimple-virtual-array-update-03-release-notes"></a>StorSimple sanal dizinin güncelleştirme 0,3 sürüm notları
## <a name="overview"></a>Genel Bakış
Aşağıdaki sürüm notları, kritik açık sorunları ve Microsoft Azure StorSimple sanal dizinin güncelleştirmeleri çözümlenen sorunları tanımlayın.

Sürüm Notları sürekli olarak güncelleştirilir ve geçici bir çözüm gerektiren kritik sorunlar bulundukça olarak eklenir. StorSimple sanal dizinizi dağıtmadan önce dikkatle Sürüm Notları'nda yer alan bilgileri gözden geçirin.

Güncelleştirme 0.3 yazılımı sürümüne karşılık gelen **10.0.10288.0**.

> [!NOTE]
> Güncelleştirmeleri dağıtıldığından ve aygıtınızı yeniden başlatın. G/ç sürüyor, aygıt kapalı kalma süresi doğurur.
> 
> 

## <a name="whats-new-in-the-update-03"></a>Güncelleştirme 0.3 yenilikler nelerdir?
Güncelleştirme 0.3 öncelikle bir hata düzeltmesi yapıdır. Bu sürümde, yedekleme hataları önceki sürümde sonuçta çeşitli hatalar giderilmiştir.

## <a name="issues-fixed-in-the-update-03"></a>Güncelleştirme 0.3 giderilen sorunlar
Aşağıdaki tabloda bu sürümde giderilen sorunlar özetini sağlar.

| Hayır. | Özellik | Sorun |
| --- | --- | --- |
| 1 |Yedeklemeler |Bir sorun, bir dosya paylaşımı için tamamlamak için yedeklemeleri nerede başarısız olacağı önceki sürümde görüldü. Bu sorun oluşursa, yedekleme işi başarısız olur ve kritik uyarı kullanıcıya bildirmek için StorSimple Yöneticisi hizmeti oluşturuldu. Bu sorunu paylaşımları veya verilere erişimin verileri etkilemedi. Kök nedeni tanımlanır ve bu sürümde sabit. <br></br> Düzeltme firmalarda geriye dönük zaten bu sorunu görüyorsunuz paylaşımları için geçerli değildir. Bu sorunu görüyorsunuz müşteriler ilk güncelleştirme 0.3 uygulamak ve sonra sorunu gidermek için tam sistem yedeklemesi gerçekleştirmek için Microsoft Support başvurun. Microsoft Support iletişim yerine müşterileri de yeni bir paylaşım etkilenen paylaşımları için iyi bir yedekten geri yükleyebilirsiniz. |
| 2 |iSCSI |Bir sorun, StorSimple sanal dizinin üzerindeki bir birime veri kopyalama işlemi sırasında birimleri burada kaybolur önceki sürümde görüldü. Bu sürümde bu sorun giderilmiştir. <br></br> Düzeltmeler yalnızca birimleri yeni oluşturulan etkili. Düzeltmeler firmalarda geriye dönük zaten bu sorunu görüyorsunuz birimleri için geçerli değildir. Müşteriler, etkilenen birimleri çevrimiçi Klasik Azure Portalı aracılığıyla getirin, bu birimler için yedekleme ve yeni birimleri için bu birimleri geri yüklemek için önerilir. |

## <a name="known-issues-in-the-update-03"></a>Güncelleştirme 0.3 bilinen sorunlar
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

## <a name="next-step"></a>Sonraki adım
[Güncelleştirme 0.3 yüklemek](storsimple-ova-install-update-01.md) , StorSimple sanal dizi.

## <a name="references"></a>Başvurular
Eski bir sürüm notu için mi arıyorsunuz? Şuraya gidin: 

* [StorSimple sanal dizinin güncelleştirme 0,1 ve 0.2 sürüm notları](storsimple-ova-update-01-release-notes.md)
* [StorSimple sanal dizinin genel kullanılabilirlik sürüm notları](storsimple-ova-pp-release-notes.md)

