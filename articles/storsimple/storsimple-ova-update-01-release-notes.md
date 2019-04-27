---
title: StorSimple sanal dizisi güncelleştirmeleri sürüm notları | Microsoft Docs
description: StorSimple sanal güncelleştirme 0.2 ve 0.1 çalıştıran dizisi için kritik açık sorunlar ve çözümleri açıklanmaktadır.
services: storsimple
documentationcenter: ''
author: alkohli
manager: carmonm
editor: ''
ms.assetid: 3993864d-2ddd-4302-a2f1-8d737fba6eab
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/16/2016
ms.author: alkohli
ms.openlocfilehash: aad60024187ca180c002f119f4b975e8f69796e5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60629297"
---
# <a name="storsimple-virtual-array-update-02-and-01-release-notes"></a>StorSimple Virtual Array güncelleştirme 0.2 ve 0.1 sürüm notları
## <a name="overview"></a>Genel Bakış
Aşağıdaki sürüm notları, kritik açık sorunlar ve çözümlenen sorunlar için Microsoft Azure StorSimple Virtual Array güncelleştirme belirleyin. (Microsoft Azure StorSimple Virtual Array de şirket içi StorSimple sanal cihazı veya StorSimple sanal cihazı denir.) 

Sürüm Notları sürekli olarak güncelleştirilir ve geçici bir çözüm gerektiren kritik sorunlar bulundukça eklenir. StorSimple sanal Cihazınızı dağıtmadan önce bu sürüm notlarında yer alan bilgileri dikkatle gözden geçirin.

Güncelleştirme 0.2 yazılım sürümüne karşılık gelen **10.0.10280.0**; Güncelleştirme 0.1 sürüm olan **10.0.10279.0**. Aşağıdaki bölümler, her güncelleştirme için değişir. 

> [!NOTE]
> Güncelleştirmeler kesintiye uğratan bir durumdur ve Cihazınızı yeniden başlatır. Cihaz, g/ç ediyor, kapalı kalma süresi neden olur.
> 
> 

## <a name="issues-fixed-in-the-update-02"></a>Güncelleştirme 0.2 giderilen sorunlar
Güncelleştirme 0.2 güncelleştirme 0.1 aşağıdaki tabloda açıklanan düzeltmeyi ek olarak tüm değişiklikleri içerir:

| Özellik | Sorun |
| --- | --- |
| Güncelleştirmeler |Güncelleştirmeleri yüklemek için yerel Web kullanıcı arabirimini kullanmanız gerekiyordu, dolayısıyla son sürümde, güncelleştirmeleri otomatik olarak Klasik Azure portalında algılanan olmayan. Bu sürümde bu sorun düzeltilmiştir. Güncelleştirme 0.2 yükledikten sonra Klasik Azure portalını kullanarak gelecekteki güncelleştirmeleri yükleyebilirsiniz. |

## <a name="whats-new-in-the-update-01"></a>Güncelleştirme 0.1 yenilikler nelerdir?
Güncelleştirme 0.1, aşağıdaki hata düzeltmeleri ve geliştirmeleri içerir. 

* **Bulut kesintiler için geliştirilmiş dayanıklılık**: Bu sürüm çeşitli hata düzeltmeleri olağanüstü durum kurtarma, yedekleme, geri yükleme ve bir bulutun bağlantı kesintisi olması durumunda katmanlama sahiptir. 
* **Geliştirilmiş geri yükleme performansı**: Bu sürüm hata düzeltmeleri, önemli ölçüde geri yükleme işlerinin tamamlanma süresini yarıya indirdik sahiptir.
* **Alan geri kazanma iyileştirme otomatik**: Veri ölçülü kaynak kullanan birimler silindiğinde, kullanılmayan depolama blokları kazanılacak gerekir. Bu sürüm, önceki sürümlere göre daha hızlı kullanılabilir hale gelmeden kullanılmayan alanı výsledek buluttan alan geri kazanma işlemi geliştirilmiştir.
* **Yeni sanal disk görüntülerini**: Yeni VHD, VHDX ve VMDK artık Klasik Azure portalı ile kullanılabilir. Yeni güncelleştirme 0.1 cihazları sağlamak için bu görüntüleri indirebilirsiniz.
* **İyileştirme işleri durumu Portalı'nda doğruluğunu**: Önceki yazılım sürümünde, portalda raporlama işi durumu ayrıntılı değildi. Bu sürümde bu sorun giderildi.
* **Etki alanına katılım deneyimi**: Etki alanına katılma ve aygıt yeniden adlandırma ile ilgili hata düzeltmeleri.

## <a name="issues-fixed-in-the-update-01"></a>Güncelleştirme 0.1 giderilen sorunlar
Aşağıdaki tabloda, bu sürümde giderilen sorunlar özetini sağlar.

| Hayır. | Özellik | Sorun |
| --- | --- | --- |
| 1 |VMDK |VMware bazı sürümlerinde, işletim sistemi diski seyrek uyarıya neden ve normal işlemleri kesintiye olarak görüldü. Bu, bu sürümde düzeltildi. |
| 2 |iSCSI sunucusu |Son sürümde, kullanıcının her etkin ağ arabiriminin StorSimple sanal cihazınız için bir ağ geçidi belirtmek için gereklidir. Bu davranışı Bu sürümde, tüm etkin ağ arabirimleri için en az bir ağ geçidini yapılandırmak kullanıcının sahip olacak şekilde değiştirilir. |
| 3 |Destek paketi |Paket boyutu 1 GB'tan büyük yazılım önceki sürümünde destek paket koleksiyonu başarısız oldu. Bu sürümde bu sorun düzeltilmiştir. |
| 4 |Bulut erişimi |StorSimple sanal dizisi, ağ bağlantısı yok ve yeniden başlatıldı ve son yayından devretmiş bağlantı sorunları yerel kullanıcı Arabirimi yoktur. Bu sürümde bu sorun düzeltilmiştir. |
| 5 |İzleme grafiklerini |Önceki sürümde, bir cihaz yük devretme aşağıdaki bulut kapasite kullanımı grafikleri hatalı değerler Klasik Azure portalında görüntülenir. Bu, geçerli sürümde sabittir. |

## <a name="known-issues-in-the-update-01"></a>Güncelleştirme 0.1'de bilinen sorunlar
Aşağıdaki tabloda StorSimple sanal dizisi için bilinen sorunların bir Özet sağlar ve önceki sürümlerden yayın belirtildiği sorunları içerir. **Bu sürümde belirtildiği sorunları sürüm bir yıldız işareti ile işaretlenir. Bu listedeki neredeyse tüm sorunları StorSimple Virtual Array GA sürümünden devreden.**

| Hayır. | Özellik | Sorun | Geçici çözüm/açıklamaları |
| --- | --- | --- | --- |
| **1.** |Güncelleştirmeler |Önizleme sürümünde oluşturulan sanal cihazlar için desteklenen genel kullanılabilirlik sürümü güncelleştirilemiyor. |Bu sanal cihazlar için genel kullanım sürümünde bir olağanüstü durum kurtarma (DR) iş akışı kullanarak devredilen gerekir. |
| **2.** |Sağlanan veri diski |Belirli bir belirtilen boyutta bir veri diski sağladığınız ve karşılık gelen StorSimple sanal cihazı oluşturdunuz, gerekir değil genişletin veya veri diski küçültmeye sonra. Bunu yapma girişimi, cihaz yerel katmanlarda tüm verilerin kaybıyla sonuçlanır. | |
| **3.** |Grup ilkesi |Bir cihaz etki alanına katılmış olduğunda, bir Grup İlkesi uygulama cihaz işlemi olumsuz yönde etkileyebilir. |Sanal diziniz kendi kuruluş birimi (OU) için Active Directory olduğundan ve hiçbir Grup İlkesi nesneleri (GPO) uygulanmış emin olun. |
| **4.** |Yerel web kullanıcı Arabirimi |Internet Explorer (IE ESC) Artırılmış güvenlik özellikleri etkinleştirilirse, bazı sorun giderme veya bakım gibi yerel web kullanıcı Arabirimi sayfalarını düzgün çalışmayabilir. Bu sayfa düğmelerini de çalışmayabilir. |Internet Explorer Gelişmiş güvenlik özelliklerini devre dışı bırakın. |
| **5.** |Yerel web kullanıcı Arabirimi |Bir Hyper-V sanal makine, GB/sn ağ arabirimlerinin de kullanıcı Arabirimi olarak 10 görüntülenen web arabirimleri. |Bir yansıma Hyper-V, davranıştır. Hyper-V, sanal ağ bağdaştırıcıları için 10 GB/sn her zaman gösterilir. |
| **6.** |Katmanlı birimler veya paylaşımlar |Katmanlı birimlerin desteklenmiyor StorSimple ile çalışan uygulamalar için kilitleme bayt aralığı. Bayt aralığı kilitleme etkinse, StorSimple katmanlama çalışmaz. |Önerilen ölçüleri içerir: <br></br>Bayt aralığı uygulama mantığınızın kilitleme devre dışı bırakın.<br></br>Bu uygulama için verileri yerel olarak sabitlenmiş birim katmanlı birimlerin yerine koymak seçin.<br></br>*Uyarı*: Kullanılarak yerel olarak sabitlenmiş birimler ve bayt aralığı kilitlemenin etkin olup, geri yükleme tamamlamadan önce yerel olarak sabitlenmiş birimin çevrimiçi olabileceğini unutmayın. Bir geri yükleme devam ediyor, bu gibi durumlarda, daha sonra tamamlamak geri yüklemek için beklemeniz gerekir. |
| **7.** |Katmanlı paylaşımları |Büyük dosyaları ile çalışma, yavaş bir katmanın ölçeğini sonuçlanabilir. |Büyük dosyalarla çalışırken, en büyük dosya paylaşım boyutunun %3 küçükse öneririz. |
| **8.** |Kapasite paylaşımlar için kullanılan |Görebileceğiniz paylaşımındaki herhangi bir veri olmadığında tüketiminin paylaşın. Kullanılan kapasite paylaşımları için meta veriler içeren olmasıdır. | |
| **9.** |Olağanüstü durum kurtarma |Yalnızca dosya sunucusu aynı etki, kaynak cihaz için olağanüstü durum kurtarma gerçekleştirebilirsiniz. Olağanüstü durum kurtarma için başka bir etki alanındaki bir hedef cihaz, bu sürümde desteklenmiyor. |Bu, bir sonraki sürümde uygulanacaktır. |
| **10.** |Azure PowerShell |StorSimple sanal cihazları, bu sürüm Azure PowerShell aracılığıyla yönetilemez. |Tüm sanal cihazların yönetimini, Klasik Azure portalı ve yerel web UI aracılığıyla yapılmalıdır. |
| **11.** |Parola değiştirme |Sanal dizi cihaz konsolu yalnızca en-US klavye biçimde giriş kabul eder. | |
| **12.** |CHAP |CHAP kimlik oluşturulduktan sonra kaldırılamaz. Ayrıca, CHAP kimlik bilgilerini değiştirirseniz, birimlerin çevrimdışına alın ve değişikliğin etkili olması çevrimiçi bunları getirmek gerekir. |Bu, sonraki sürümlerde ele alınacaktır. |
| **13.** |iSCSI sunucusu |'Depolama görüntülenen bir iSCSI birimi için kullanılan' StorSimple Yöneticisi hizmeti ve iSCSI konağının farklı olabilir. |İSCSI ana bilgisayar dosya sistemi görünüme sahiptir.<br></br>Cihaz, birim maksimum boyutta olduğunda ayrılan blokları görür. |
| **14.** |Dosya sunucusu * |Bir klasördeki bir dosyaya bir alternatif veri Stream (ilişkili REKLAM) varsa, REKLAM değil yedeklenen veya olağanüstü durum kurtarma, kopyalama ve öğe düzeyinde Kurtarma ile geri. | |

## <a name="next-step"></a>Sonraki adım
[Güncelleştirmeleri yüklemek](storsimple-ova-install-update-01.md) StorSimple Virtual Array'iniz üzerinde.

