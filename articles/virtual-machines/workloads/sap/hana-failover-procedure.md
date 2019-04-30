---
title: HANA (büyük örnekler) Azure üzerinde SAP HANA için olağanüstü durum siteye yük devretme yordamı | Microsoft Docs
description: SAP hana (büyük örnekler) azure'da olağanüstü durum kurtarma sitesine yük devretme gerçekleştirme
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/22/2019
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 76d8bb816bdf229d13a49fa61337899a8bf29ecd
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62098295"
---
# <a name="disaster-recovery-failover-procedure"></a>Olağanüstü durum kurtarma yük devretme yordamı


>[!IMPORTANT]
>Bu belge, yönetim belgeleri SAP HANA veya SAP notları hiçbir yerini almaktadır. Okuyucu düz bir anlayış ve SAP HANA yönetimi ve işlemleri, özellikle yedekleme, geri yükleme, yüksek kullanılabilirlik ve olağanüstü durum kurtarma konuları uzmanlığa sahip beklenmektedir. Bu belgede, SAP HANA Studio ekran görüntüleri gösterilmektedir. SAP HANA'dan yayın için yayın içerik, yapısı ve SAP Yönetim Araçları ve araçları ekranlar yapısını değiştirebilirsiniz.

DR sitesine devretmek yaparken dikkate alınması gereken iki durum vardır:

- Verilerin son durumuna geri dönmek için SAP HANA veritabanı ihtiyacınız vardır. Bu durumda, bir Self Servis betiği ile Microsoft'a başvurun gerek olmadan bir yük devretme gerçekleştirebilirsiniz yoktur. Ancak yeniden çalışma için Microsoft ile çalışması gerekir.
- Son çoğaltılmış anlık görüntü olmayan bir depolama anlık görüntüye geri yüklemek istediğiniz. Bu durumda, Microsoft ile çalışması gerekir. 

>[!NOTE]
>Aşağıdaki adımları DR birimini temsil eder HANA büyük örneği biriminde yürütülmesi gerekir. 
 
Son çoğaltılan depolama anlık görüntüleri için geri yüklemek için bölümünde listelenen adımları **'tam DR - azure_hana_dr_failover yük devri'** belgenin [Microsoft azure'da SAP HANA için araçları snapshot ](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf). 

Birden çok SAP HANA örnekleri üzerinde başarısız olmasını istiyorsanız, birkaç kez azure_hana_dr_failover komutu çalıştırmak gerekir. İstendiğinde, SAP HANA, yük devretme ve geri yüklemek istediğiniz SID yazın. 


Gerçek bir çoğaltma ilişkisi etkilemeden DR yük devretmeyi de test edebilirsiniz. Yük devretme testi gerçekleştirmek için adımları izleyin. **'DR yük devretme - azure_hana_test_dr_failover test gerçekleştirmek'** belgenin [Microsoft azure'da SAP HANA için araçları snapshot](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf). 

>[!IMPORTANT]
>Yapmak *değil* üretim işlemler DR sitenin işlemi boyunca oluşturduğunuz örneğinde çalışan **bir yük devretme testi**. Bu komut azure_hana_test_dr_failover birincil siteye hiçbir ilişki bulunmayan birimleri kümesi oluşturur. Sonuç olarak, birincil siteye eşitleme olduğundan *değil* mümkün. 

Test etmek için birden çok SAP HANA örnekleri olmasını istiyorsanız, birkaç kez betiği çalıştırmak gerekir. İstendiğinde, yük devretme için test etmek istediğiniz örnek SAP HANA SID'si yazın. 

>[!NOTE]
>Daha önceki bir anlık görüntüye ayarlamak için DR birimleri saat önce silinmiş olan bazı veriler kurtarması ve DR sitesine yük devretme gerekiyorsa, bu yordamı uygular. 

1. Kullanmakta olduğunuz HANA büyük örnekleri olağanüstü durum kurtarma birimi HANA üretim dışı örneğini kapatın. Önceden yüklenmiş bir etkinliği olmayan HANA üretim örneği olduğundan budur.
1. SAP HANA işlem çalışır durumda olduğundan emin olun. Bu denetim için aşağıdaki komutu kullanın: `/usr/sap/hostctrl/exe/sapcontrol –nr <HANA instance number> - function GetProcessList`. Çıktı göstermesi gerekir **hdbdaemon** işlem durdurulmuş bir duruma ve bir çalışan veya başlatılan durumda diğer HANA işlem yok.
1. Hangi anlık görüntü adı veya SAP HANA yedekleme kimliği geri olağanüstü durum kurtarma siteniz istediğinize karar verin. Gerçek bir olağanüstü durum kurtarma durumlarda, bu anlık görüntü, genellikle en son anlık görüntüsüdür. Kayıp verileri kurtarmanız gerekiyorsa, önceki bir anlık görüntü seçin.
1. Azure desteği yüksek öncelikli destek talebi başvurun. Bu anlık görüntü (anlık görüntü tarih ve ad ile) veya DR sitesi HANA yedekleme Kimliğinde geri isteyin. İşlemleri yan/hana/veri hacmi yalnızca geri yükleyen varsayılandır. / Hana/logbackups birimleri de istiyorsanız, bu durum özellikle gerekir. */Hana/shared birime geri yüklemeyin.* Bunun yerine, belirli dosyaları global.ini tanesi gibi seçmeniz gerekir **.snapshot** dizin ve alt, / hana/yeniden bağlamak sonra paylaşılan birim PRD için. 

   İşlemleri tarafında aşağıdaki adımlardan oluşur:

   a. Üretim birimden olağanüstü durum kurtarma birimleri için anlık görüntü çoğaltma durdurulur. Üretim sitede sistemin çalışmaması için olağanüstü durum kurtarma yordamı gerçekleştirmeniz gerekir. Bunun nedeni ise bu kesintisi zaten gerçekleşmiş olabilir.
   
   b. Depolama anlık görüntü adı veya olağanüstü durum kurtarma birimlerin geri seçtiğiniz yedekleme Kimliğe sahip anlık görüntüsünü alın.
   
   c. Geri yüklemeden sonra olağanüstü durum kurtarma bölgesindeki HANA büyük örneği birimi takılamadı olağanüstü durum kurtarma birimleri kullanılabilir.
      
1. Olağanüstü durum kurtarma birimleri olağanüstü durum kurtarma siteniz olarak HANA büyük örneği birimine bağlayın. 
1. Etkin olmayan SAP HANA üretim örneği başlatın.
1. RPO süresini azaltmak için işlem günlüğü yedekleme günlükleri kopyalamak isterseniz, bu işlem günlüğü yedeklemeleri yeni oluşturulmuş DR/hana/logbackups dizine birleştirmek gerekir. Var olan yedeklerin üzerine yazmayın. En son depolama anlık görüntü çoğaltma ile çoğaltılmamış yeni yedeklemelere kopyalayın.
1. Ayrıca, DR Azure bölgesinde /hana/shared/PRD birimine çoğaltılan anlık görüntüleri dışında tek dosyaları geri yükleyebilirsiniz.

Geri yüklenen depolama anlık görüntüleri ve mevcut işlem günlüğü yedeklemeleri dayalı SAP HANA üretim örneği kurtarma adımları dizisini içerir:

1. Yedekleme konumuna değiştirme **/hana/logbackups** SAP HANA Studio kullanarak.
   ![DR kurtarması için yedekleme konumu değiştirin](./media/hana-overview-high-availability-disaster-recovery/change_backup_location_dr1.png)

1. SAP HANA yedekleme dosyası konumları tarar ve geri yüklemek için en son işlem günlüğü yedeklemesi önerir. Tarama aşağıdakiler görünür gibi bir ekran kadar birkaç dakikayı bulabilir: ![DR kurtarması için işlem günlüğü yedeklemeleri listesi](./media/hana-overview-high-availability-disaster-recovery/backup_list_dr2.PNG)

1. Varsayılan ayarlardan bazılarını ayarlayın:

      - NET **değişim yedeklemeleri kullanın**.
      - Seçin **başlatmak günlük alanı**.

   ![Başlatma günlüğü alanını ayarlayın](./media/hana-overview-high-availability-disaster-recovery/initialize_log_dr3.PNG)

1. **Son**’u seçin.

   ![DR geri yüklemeyi tamamlayın](./media/hana-overview-high-availability-disaster-recovery/finish_dr4.PNG)

Burada gösterildiği gibi bir ilerleme durumu penceresi görüntülenmelidir. Bir olağanüstü durum kurtarma geri yükleme, bir üç düğümlü genişleme SAP HANA yapılandırmasının örnektir göz önünde bulundurun.

![Geri yükleme ilerleme durumu](./media/hana-overview-high-availability-disaster-recovery/restore_progress_dr5.PNG)

Geri yükleme sırasında yanıt vermemesine görünüyorsa **son** ekranında ve değil ilerleme ekranı göstermek, tüm SAP HANA örnekleri çalışan düğümleri üzerinde çalıştığını doğrulayın. Gerekirse, SAP HANA örnekleri el ile başlatın.


## <a name="failback-from-a-dr-to-a-production-site"></a>Bir DR Azure'dan bir üretim sitesine
Bir üretim sitesini bir DR geri dönebilirsiniz. Yük devretme olağanüstü durum kurtarma siteniz olarak bir Azure bölgesine üretim sorunlarını ve kayıp veri kurtarmak için değil, gereksinimi kaynaklandı senaryo bakalım. SAP üretim iş yükünüz bir süredir olağanüstü durum kurtarma sitesinde zamandır çalışmakta olan. Üretim sitesini sorunlarını çözümlendi olarak, üretim sitede yeniden çalıştırmak istersiniz. Veri kaybına neden olduğundan, üretim sitesini adımla geri çeşitli adımları ve Kapat işbirliği yaparak Azure operasyon ekibinin üzerinde SAP HANA ile ilgilidir. Bu sorunları çözüldükten sonra üretim sitesine eşitlemeye başlamak için operasyon ekibinin tetiklemek için size aittir.

Bu adımlar dizisi.

1. Azure operasyon ekibinin üzerinde SAP HANA üretim artık üretim durumu temsil eden olağanüstü durum kurtarma depolama birimleri depolama birimlerden eşitlemek için tetikleyici alır. Bu durumda, üretim sitesini HANA büyük örneği biriminde kapatılır.
1. Azure operasyon ekibinin üzerinde SAP HANA çoğaltma izler ve bunu size bildiren önce yakalanan emin olmanızı sağlar.
1. Olağanüstü durum kurtarma siteniz üretim HANA örneği kullanan uygulamaları kapatın. Ardından bir HANA işlem günlüğü yedeklemesi gerçekleştirin. Ardından, olağanüstü durum kurtarma siteniz HANA büyük örneği birimleri üzerinde çalışan HANA örneği durdurun.
1. Olağanüstü durum kurtarma siteniz HANA büyük örneği biriminde çalışan HANA örneği kapattığınızda, operasyon ekibinin el ile disk birimleri yeniden eşitler.
1. Azure operasyon ekibinin üzerinde SAP HANA, HANA büyük örneği birim üretim sitenin yeniden başlatır ve bunu size uygulamalı. SAP HANA örneği HANA büyük örneği birim başlangıç zamanında bir kapanma durumunda olduğundan emin olun.
1. Daha önce olağanüstü durum kurtarma sitesine devretmek zaman yaptığınız gibi aynı veritabanı geri yükleme adımlarını gerçekleştirin.

## <a name="monitor-disaster-recovery-replication"></a>Olağanüstü durum kurtarma çoğaltmayı izleme

Betik yürüterek, depolama çoğaltma ilerleme durumunu izleyebilirsiniz `azure_hana_replication_status`. Bu komut, beklendiği gibi olağanüstü durum kurtarma konumunuz çalışan bir birimden çalıştırmanız gerekir. Komutu, çoğaltma etkin olup bağımsız olarak çalışır. Komutu her HANA büyük örneği birimine kiracınızın olağanüstü durum kurtarma konumunuz olarak çalıştırabilirsiniz. Önyükleme birimi hakkındaki ayrıntıları almak için kullanılamaz. Komut ve çıktısını ayrıntılarını okumak **'DR çoğaltma durumu - azure_hana_replication_status'Get '** belgenin [Microsoft azure'da SAP HANA için araçları snapshot](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).


## <a name="next-steps"></a>Sonraki adımlar
- Başvurmak [izleme ve sorun giderme HANA taraftan](hana-monitor-troubleshoot.md).
