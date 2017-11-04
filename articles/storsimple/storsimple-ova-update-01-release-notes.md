---
title: "StorSimple sanal dizinin güncelleştirmeleri sürüm notları | Microsoft Docs"
description: "StorSimple sanal güncelleştirme 0.2 ve 0,1 çalıştıran dizisi için açık kritik sorunlar ve çözümleri açıklar."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 3993864d-2ddd-4302-a2f1-8d737fba6eab
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/16/2016
ms.author: alkohli
ms.openlocfilehash: c4ccde9635b3874864baa9d4d262ff5ddcf2a425
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="storsimple-virtual-array-update-02-and-01-release-notes"></a>StorSimple sanal dizinin güncelleştirme 0.2 ve 0,1 sürüm notları
## <a name="overview"></a>Genel Bakış
Aşağıdaki sürüm notları, kritik açık sorunları ve Microsoft Azure StorSimple sanal dizinin güncelleştirmeleri çözümlenen sorunları tanımlayın. (Microsoft Azure StorSimple sanal olarak da bilinen şirket içi StorSimple sanal cihazı veya StorSimple sanal cihazı dizidir.) 

Sürüm Notları sürekli olarak güncelleştirilir ve geçici bir çözüm gerektiren kritik sorunlar bulundukça olarak eklenir. StorSimple sanal Cihazınızı dağıtmadan önce dikkatle Sürüm Notları'nda yer alan bilgileri gözden geçirin.

Güncelleştirme 0.2 yazılımı sürümüne karşılık gelen **10.0.10280.0**; Güncelleştirme 0.1 olduğu sürüm **10.0.10279.0**. Aşağıdaki bölümler, her bir güncelleştirme değişir. 

> [!NOTE]
> Güncelleştirmeleri dağıtıldığından ve aygıtınızı yeniden başlatılır. G/ç sürüyor, aygıt kapalı kalma süresi uygulanabilir.
> 
> 

## <a name="issues-fixed-in-the-update-02"></a>0.2 güncelleştirmede giderilen sorunlar
Güncelleştirme 0.2 güncelleştirme 0.1 aşağıdaki tabloda açıklanan düzeltme yanı sıra tüm değişiklikleri içerir:

| Özellik | Sorun |
| --- | --- |
| Güncelleştirmeler |Güncelleştirmeleri yüklemek için yerel Web kullanıcı arabirimini kullanmanız gerekiyordu böylece son sürümde, güncelleştirmeleri otomatik olarak Klasik Azure portalında algılanan doğru. Bu sürümde bu sorun düzeltilmiştir. Güncelleştirme 0.2 yükledikten sonra Klasik Azure portalını kullanarak gelecekteki güncelleştirmeleri yükleyebilirsiniz. |

## <a name="whats-new-in-the-update-01"></a>Güncelleştirme 0.1 yenilikler nelerdir?
Güncelleştirme 0.1 aşağıdaki hata düzeltmeleri ve geliştirmeleri içerir. 

* **Bulut kesintiler için geliştirilmiş dayanıklılık**: Bu sürüm çeşitli hata düzeltmeleri olağanüstü durum kurtarma, yedekleme, geri yükleme ve durumunda bulut bağlantı kesintisi sunabilmek çevresinde sahiptir. 
* **Geliştirilmiş geri yükleme performansı**: geri yükleme işlerinin tamamlanma zamanı önemli ölçüde Kes hata düzeltmeleri bu sürüme sahip.
* **Alan geri kazanma iyileştirme otomatik**: kullanılmayan depolama blokları veri ölçülü kaynak kullanan birimlerde silindiğinde, geri kazanılacak gerekir. Bu sürüm için alan geri kazanma işlemini önceki sürümlere kıyasla daha hızlı kullanılabilir hale gelmeden kullanılmayan alanı sonuçta bulut geliştirilmiştir.
* **Yeni sanal disk görüntüleri**: yeni VHD ve VHDX VMDK artık Azure Klasik portalı üzerinden kullanılabilir. Yeni güncelleştirme 0.1 cihazları sağlamak için bu görüntüleri indirebilirsiniz.
* **İşlerini durum Portalı'nda doğruluğu artırma**: yazılım'ın önceki sürümünde, portalda raporlama iş durumu ayrıntılı değildi. Bu sorun, bu sürümde çözümlenir.
* **Etki alanına katılım deneyimi**: hata düzeltmeleri ilgili etki alanına katılma ve aygıt yeniden adlandırılıyor.

## <a name="issues-fixed-in-the-update-01"></a>0,1 güncelleştirmede giderilen sorunlar
Aşağıdaki tabloda bu sürümde giderilen sorunlar özetini sağlar.

| Hayır. | Özellik | Sorun |
| --- | --- | --- |
| 1 |VMDK |Bazı VMware sürümlerinde, işletim sistemi diski seyrek uyarıya neden ve normal işlemlerin kesintiye olarak görüldü. Bu, bu sürümde giderilmiştir. |
| 2 |iSCSI sunucusu |Son sürümde, kullanıcının her etkin ağ arabiriminin, StorSimple sanal cihazınız için bir ağ geçidi belirtmek için gereklidir. Tüm etkin ağ arabirimleri için en az bir ağ geçidi yapılandırmak kullanıcının sahip böylece bu davranış bu sürümde değiştirildi. |
| 3 |Destek Paketi |Paket boyutu 1 GB'den daha büyük yazılım'ın önceki sürümünde, destek paket koleksiyonu başarısız oldu. Bu sürümde bu sorun düzeltilmiştir. |
| 4 |Bulut erişimi |StorSimple sanal dizinin ağ bağlantısı yok ve yeniden başlatıldı, son sürümde, yerel kullanıcı arabirimini bağlantı sorunları gerekir. Bu sürümde bu sorun düzeltilmiştir. |
| 5 |İzleme grafikleri |Önceki sürümde, bir aygıt yük devretme aşağıdaki bulut kapasite kullanımı grafikleri hatalı değerler Azure Klasik Portalı'nda görüntülenir. Bu geçerli sürümde düzeltilmiştir. |

## <a name="known-issues-in-the-update-01"></a>Güncelleştirme 0.1 bilinen sorunlar
Aşağıdaki tabloda StorSimple sanal dizi için bilinen sorunlar özetini sağlar ve önceki sürümlerden yayın-not ettiğiniz sorunları içerir. **Bu sürümde not ettiğiniz yayımlanan sorunlar yıldızla işaretlenmiş. Bu listedeki neredeyse tüm sorunları ve StorSimple sanal dizinin GA sürümünden devreden.**

| Hayır. | Özellik | Sorun | Geçici çözüm/açıklamaları |
| --- | --- | --- | --- |
| **1.** |Güncelleştirmeler |Önizleme sürümünde oluşturulan sanal cihazlar için desteklenen bir genel kullanılabilirlik sürümü güncelleştirilemiyor. |Bu sanal cihazlar üzerinden bir olağanüstü durum kurtarma (DR) iş akışı kullanarak genel kullanılabilirlik sürümü için başarısız gerekir. |
| **2.** |Sağlanan veri diski |Belirli bir belirtilen boyutta bir veri diski sağlamış ve karşılık gelen StorSimple sanal cihaz oluşturulan, gerekir değil genişletin veya veri diski küçültmeye sonra. Bunu yapmak çalışırken cihaz yerel katmanlarda tüm verilerin kaybıyla sonuçlanır. | |
| **3.** |Grup İlkesi |Bir cihaz etki alanına katılmış olduğunda, bir Grup İlkesi uygulama aygıt işlemi olumsuz etkileyebilir. |Sanal dizinizi kendi kuruluş biriminde (OU) için Active Directory ve hiçbir Grup İlkesi nesneleri (GPO) için uygulanan olduğundan emin olun. |
| **4.** |Yerel web kullanıcı Arabirimi |Internet Explorer (IE ESC) Artırılmış güvenlik özellikleri etkinleştirilirse, sorun giderme veya bakım gibi bazı yerel web kullanıcı Arabirimi sayfalarını düzgün çalışmayabilir. Bu sayfaları düğmeleri de çalışmayabilir. |Internet Explorer Artırılmış güvenlik özelliklerini kapatın. |
| **5.** |Yerel web kullanıcı Arabirimi |Bir Hyper-V sanal makine ağ kullanıcı Arabirimi olarak 10 görüntülenen Web Gbps arabirimleri. |Hyper-V yansımasını davranıştır. Hyper-V sanal ağ bağdaştırıcıları için 10 GB/sn her zaman gösterir. |
| **6.** |Katmanlı birimler veya paylaşımlar |Katmanlı birimlerin desteklenmiyor StorSimple ile çalışan uygulamalar için kilitleme bayt aralığı. Bayt aralığı kilitleme etkinse, StorSimple katmanlama çalışmaz. |Önerilen ölçüleri içerir: <br></br>Uygulama mantığınızın kilitleme bayt aralığı kapatın.<br></br>Yerel olarak sabitlenmiş birimlerin katmanlı birimleri aksine bu uygulama için veri koymak seçin.<br></br>*Uyarı*: kullanarak yerel olarak sabitlenmiş birimleri ve bayt aralığı kilitleme etkinse, geri yükleme tamamlamadan önce yerel olarak sabitlenmiş birimin çevrimiçi olabileceğini unutmayın. Bir geri yükleme devam ediyor, böyle durumlarda, daha sonra tamamlamak geri yüklemek için beklemeniz gerekir. |
| **7.** |Katmanlı paylaşımları |Büyük dosyaları ile çalışma yavaş katmanı genişletmek neden olabilir. |Büyük dosyaları ile çalışırken en büyük dosya paylaşımı boyutunu %3 küçüktür öneririz. |
| **8.** |Kapasite paylaşımlar için kullanılan |Görebileceğiniz paylaşmak paylaşımındaki herhangi bir veri olmadığında tüketiminin. Paylaşımlar için kullanılan kapasitesi meta veriler içerdiğinden budur. | |
| **9.** |Olağanüstü durum kurtarma |Yalnızca bir dosya sunucusu aynı etki alanında, kaynak cihaz için olağanüstü durum kurtarma gerçekleştirebilirsiniz. Başka bir etki alanındaki bir hedef cihaz için olağanüstü durum kurtarma, bu sürümde desteklenmiyor. |Bu, bir sonraki sürümde uygulanacaktır. |
| **10.** |Azure PowerShell |StorSimple sanal cihazlar, bu sürümde Azure PowerShell aracılığıyla yönetilemez. |Klasik Azure portalı ve yerel web kullanıcı Arabirimi sanal aygıtların tüm yönetim yapılması gerekir. |
| **11.** |Parola değiştirme |Sanal dizinin aygıt konsolu yalnızca en-US klavye biçiminde giriş kabul eder. | |
| **12.** |CHAP |CHAP kimlik oluşturulduktan sonra kaldırılamaz. Ayrıca, CHAP kimlik bilgilerini değiştirirseniz, birimlerin çevrimdışına alın ve ardından çevrimiçi bunları değişikliğin etkili olması çevrimiçine gerekir. |Bunlar daha sonraki bir sürümde ele alınacaktır. |
| **13.** |iSCSI sunucusu |' Depolama birimi için iSCSI biriminde görüntülenen kullanılan' StorSimple Yöneticisi hizmeti ve iSCSI konağının farklı olabilir. |İSCSI konağının filesystem görünüme sahiptir.<br></br>Cihaz en büyük boyutta birimi olduğu zaman ayrılan blok görür. |
| **14.** |Dosya sunucusu * |Bir klasördeki bir dosyaya bir alternatif veri akışı (ilişkili ADS) varsa, REKLAMLARI değil yedeklenemez veya olağanüstü durum kurtarma, kopyalama ve öğe düzeyinde Kurtarma geri. | |

## <a name="next-step"></a>Sonraki adım
[Güncelleştirmeleri yüklemek](storsimple-ova-install-update-01.md) , StorSimple sanal dizi.

