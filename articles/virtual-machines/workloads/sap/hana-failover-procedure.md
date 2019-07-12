---
title: SAP hana (büyük örnekler) azure'da olağanüstü durum siteye yük devretme yordamı HANA | Microsoft Docs
description: SAP hana (büyük örnekler) azure'da olağanüstü durum kurtarma sitesine yük devretme gerçekleştirme
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: gwallace
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/22/2019
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6454c82e3d9c73d1b5a4b2224abf1ab63a798355
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67709640"
---
# <a name="disaster-recovery-failover-procedure"></a>Olağanüstü durum kurtarma yük devretme yordamı


>[!IMPORTANT]
>Bu makalede bir ardılı yönetim belgelerine SAP HANA veya SAP notları değildir. Düz bir anlayış ve SAP HANA yönetim ve işlemler, yedekleme, geri yükleme, yüksek kullanılabilirlik ve olağanüstü durum kurtarma (DR) için özellikle uzmanlığa sahip bekliyoruz. Bu makalede, ekran görüntüleri, SAP HANA Studio gösterilmektedir. SAP HANA'dan yayın için yayın içerik, yapısı ve SAP Yönetim Araçları ve araçları ekranlar yapısını değiştirebilirsiniz.

Ne zaman bir DR sitesine yük devretme dikkate alınması gereken iki durum vardır:

- Verilerin son durumuna geri dönmek için SAP HANA veritabanı ihtiyacınız vardır. Bu durumda, bir Self Servis betiği ile Microsoft'a başvurun gerek olmadan bir yük devretme gerçekleştirebilirsiniz yoktur. Yeniden çalışma için Microsoft ile çalışması gerekir.
- Son çoğaltılmış anlık görüntü olmayan bir depolama anlık görüntüye geri yüklemek istediğiniz. Bu durumda, Microsoft ile çalışması gerekir. 

>[!NOTE]
>Aşağıdaki adımlar, DR birimini temsil eder HANA büyük örneği biriminde yapılmalıdır. 
 
Son çoğaltılan depolama anlık görüntüleri için geri yüklemek için "Gerçekleştirme tam DR yük devretme - azure_hana_dr_failover" adımları izleyin [Microsoft azure'da SAP HANA için araçları snapshot](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf). 

Birden çok SAP HANA örnekleri üzerinde başarısız olmasını istiyorsanız, azure_hana_dr_failover komutu birkaç kez çalıştırın. İstendiğinde, SAP HANA, yük devretme ve geri yüklemek istediğiniz SID girin. 


Gerçek bir çoğaltma ilişkisi etkilemeden DR yük devretmeyi de test edebilirsiniz. Yük devretme testi gerçekleştirmek için "Gerçekleştirme DR yük devretme - test azure_hana_test_dr_failover"'ndaki adımları takip edin [Microsoft azure'da SAP HANA için araçları snapshot](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf). 

>[!IMPORTANT]
>Yapmak *değil* üretim işlemler DR sitenin işlemi boyunca oluşturduğunuz örneğinde çalışan **bir yük devretme testi**. Komut azure_hana_test_dr_failover birincil siteye hiçbir ilişki bulunmayan birimleri kümesi oluşturur. Sonuç olarak, birincil siteye eşitleme olduğundan *değil* mümkün. 

Test etmek için birden çok SAP HANA örnekleri olmasını istiyorsanız, komut dosyası birkaç kez çalıştırın. İstendiğinde, yük devretme için test etmek istediğiniz örnek SAP HANA SID'sini girin. 

>[!NOTE]
>Daha önceki bir anlık görüntüye ayarlamak için DR birimleri saat önce silinmiş olan bazı veriler kurtarması ve DR sitesine yük devretme gerekiyorsa, bu yordamı uygular. 

1. Kullanmakta olduğunuz HANA büyük örnekleri olağanüstü durum kurtarma birimi HANA üretim dışı örneğini kapatın. Bir etkinliği olmayan HANA üretim örneği önceden yüklenir.
1. SAP HANA işlem çalışır durumda olduğundan emin olun. Bu denetim için aşağıdaki komutu kullanın:

      `/usr/sap/hostctrl/exe/sapcontrol –nr <HANA instance number> - function GetProcessList`.

      Çıktı göstermesi gerekir **hdbdaemon** işlem durdurulmuş bir duruma ve bir çalışan veya başlatılan durumda diğer HANA işlem yok.
1. Hangi anlık görüntü adı veya SAP HANA yedekleme kimliği geri olağanüstü durum kurtarma siteniz istediğinize karar verin. Gerçek bir olağanüstü durum kurtarma durumlarda, bu anlık görüntü, genellikle en son anlık görüntüsüdür. Kayıp verileri kurtarmanız gerekiyorsa, önceki bir anlık görüntü seçin.
1. Azure desteği yüksek öncelikli destek talebi başvurun. Bu anlık görüntünün adı ile geri yükleme ve anlık görüntü veya DR sitesi HANA yedekleme Kimliğinde tarihi isteyin. İşlemleri yan/hana/veri hacmi yalnızca geri yükleyen varsayılandır. / Hana/logbackups birimler çok olmasını istiyorsanız, bu durum özellikle gerekir. */Hana/shared birime geri yüklemeyin.* Bunun yerine, belirli dosyaları global.ini tanesi gibi seçin **.snapshot** dizin ve alt, / hana/yeniden bağlamak sonra paylaşılan birim PRD için. 

   İşlemleri tarafında aşağıdaki adımlardan oluşur:

   a. Üretim birimden olağanüstü durum kurtarma birimleri için anlık görüntü çoğaltma durdurulur. Üretim sitede sistemin çalışmaması için olağanüstü durum kurtarma yordamı gerçekleştirmeniz gerekir. Bunun nedeni ise bu kesintisi zaten gerçekleşmiş olabilir.
   
   b. Depolama anlık görüntü adı veya olağanüstü durum kurtarma birimlerin geri seçtiğiniz yedekleme Kimliğe sahip anlık görüntüsünü alın.
   
   c. Geri yüklemeden sonra olağanüstü durum kurtarma bölgesindeki HANA büyük örneği birimi takılamadı olağanüstü durum kurtarma birimleri kullanılabilir.
      
1. Olağanüstü durum kurtarma birimleri olağanüstü durum kurtarma siteniz olarak HANA büyük örneği birimine bağlayın. 
1. Etkin olmayan SAP HANA üretim örneği başlatın.
1. RPO süresini azaltmak için işlem günlüğü yedekleme günlüklerini kopyalamak seçerseniz, işlem günlüğü yedeklemeleri yeni oluşturulmuş DR/hana/logbackups dizine birleştirin. Var olan yedeklerin üzerine yazmayın. En son depolama anlık görüntü çoğaltma ile çoğaltılan olmayan yeni yedeklemelere kopyalayın.
1. Ayrıca, /hana/shared/PRD birimin DR Azure bölgesinde çoğaltılmasını uygulamasında anlık görüntüleri dışında tek dosyaları geri yükleyebilirsiniz.

Aşağıdaki adımlarda, geri yüklenen depolama anlık görüntüleri ve mevcut işlem günlüğü yedeklemeleri dayalı SAP HANA üretim örneği, kurtarılır gösterilmektedir.

1. Yedekleme konumuna değiştirme **/hana/logbackups** SAP HANA Studio kullanarak.

   ![DR kurtarması için yedekleme konumu değiştirin](./media/hana-overview-high-availability-disaster-recovery/change_backup_location_dr1.png)

1. SAP HANA yedekleme dosyası konumları tarar ve geri yüklemek için en son işlem günlüğü yedeklemesi önerir. Tarama aşağıdakiler görünür gibi bir ekran kadar birkaç dakikayı bulabilir:

   ![DR kurtarması için işlem günlüğü yedeklemeleri listesi](./media/hana-overview-high-availability-disaster-recovery/backup_list_dr2.PNG)

1. Varsayılan ayarlardan bazılarını ayarlayın:

      - NET **değişim yedeklemeleri kullanın**.
      - Seçin **başlatmak günlük alanı**.

   ![Başlatma günlüğü alanını ayarlayın](./media/hana-overview-high-availability-disaster-recovery/initialize_log_dr3.PNG)

1. **Son**’u seçin.

   ![DR geri yüklemeyi tamamlayın](./media/hana-overview-high-availability-disaster-recovery/finish_dr4.PNG)

Burada gösterildiği gibi bir ilerleme durumu penceresi görüntülenmelidir. Bir olağanüstü durum kurtarma geri yükleme, bir üç düğümlü genişleme SAP HANA yapılandırmasının örnektir göz önünde bulundurun.

![Geri yükleme ilerleme durumu](./media/hana-overview-high-availability-disaster-recovery/restore_progress_dr5.PNG)

Geri yükleme sırasında yanıt vermemeye başlarsa **son** ekranında ve değil ilerleme ekranı göstermek, tüm SAP HANA örnekleri çalışan düğümleri üzerinde çalıştığını doğrulayın. Gerekirse, SAP HANA örnekleri el ile başlatın.


## <a name="failback-from-a-dr-to-a-production-site"></a>Bir DR Azure'dan bir üretim sitesine
Bir üretim sitesini bir DR geri dönebilirsiniz. Yük devretme olağanüstü durum kurtarma siteniz olarak bir Azure bölgesine üretim sorunlarını ve kayıp veri kurtarmak için değil, gereksinimi kaynaklandı senaryo bakalım. 

SAP üretim iş yükünüz bir süredir olağanüstü durum kurtarma sitesinde çalıştırıyorsunuz. Üretim sitesini sorunlarını çözümlendi olarak, üretim sitede yeniden çalıştırmak istersiniz. Veri kaybına neden olduğundan, üretim sitesini adımla geri çeşitli adımları ve Kapat işbirliği yaparak Azure operasyon ekibinin üzerinde SAP HANA ile ilgilidir. Bu sorunları çözüldükten sonra üretim sitesine eşitlemeye başlamak için operasyon ekibinin tetiklemek için size aittir.

Şu adımları uygulayın:

1. Azure operasyon ekibinin üzerinde SAP HANA üretim artık üretim durumu temsil eden olağanüstü durum kurtarma depolama birimleri depolama birimlerden eşitlemek için tetikleyici alır. Bu durumda, üretim sitesini HANA büyük örneği biriminde kapatılır.
1. Azure operasyon ekibinin üzerinde SAP HANA çoğaltma izler ve sizi bilgilendirmek önce yakalanan emin olmanızı sağlar.
1. Olağanüstü durum kurtarma siteniz üretim HANA örneği kullanan uygulamaları kapatın. Ardından bir HANA işlem günlüğü yedeklemesi gerçekleştirin. Ardından, HANA büyük örneği birimleri olağanüstü durum kurtarma siteniz olarak çalışan HANA örneği durdurun.
1. Olağanüstü durum kurtarma siteniz HANA büyük örneği biriminde çalışan HANA örneği kapattığınızda, operasyon ekibinin el ile disk birimleri yeniden eşitler.
1. Azure operasyon ekibinin üzerinde SAP HANA HANA büyük örneği birim üretim sitenin yeniden başlatır. Bunlar, size teslim. SAP HANA örneği HANA büyük örneği birim başlangıç zamanında bir kapanma durumunda olduğundan emin olun.
1. Size, daha önce olağanüstü durum kurtarma sitesine devredilen yapmış olduğunuz veritabanı geri yükleme adımları gerçekleştirin.

## <a name="monitor-disaster-recovery-replication"></a>Olağanüstü durum kurtarma çoğaltmayı izleme

Depolama çoğaltma ilerleme durumunu izlemek için betiği çalıştırın. `azure_hana_replication_status`. Bu komut, beklendiği gibi olağanüstü durum kurtarma konumunuz çalıştırılan bir birim çalıştırılmalıdır. Komutu, çoğaltma etkin olup bağımsız olarak çalışır. Komutu her HANA büyük örneği birimine kiracınızın olağanüstü durum kurtarma konumunuz olarak çalıştırabilirsiniz. Önyükleme birimi hakkındaki ayrıntıları almak için kullanılamaz. 

İçinde "Azure_hana_replication_status DR çoğaltma durumu - Al" komutunu ve çıktısını hakkında daha fazla bilgi için bkz [Microsoft azure'da SAP HANA için araçları snapshot](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).


## <a name="next-steps"></a>Sonraki adımlar
- Bkz: [izleme ve sorun giderme HANA taraftan](hana-monitor-troubleshoot.md).
