---
title: StorSimple Snapshot Manager Yönetim | Microsoft Docs
description: Bir genel bakış ve StorSimple Snapshot Manager çözüm yönetim görevleri ve iş akışları hakkında daha fazla bilgi için bağlantılar sağlar.
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: ''
ms.assetid: 1cdbb61d-bd16-4be4-ade2-ceab11508acb
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2016
ms.author: v-sharos
ms.openlocfilehash: bc72da98800ef85ef14be0882ba856fbf01386b9
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60630028"
---
# <a name="use-storsimple-snapshot-manager-to-administer-your-storsimple-solution"></a>StorSimple çözümünüzü yönetmek için StorSimple anlık görüntü Yöneticisi'ni kullanın

## <a name="overview"></a>Genel Bakış
StorSimple Snapshot Manager, veri koruma ve Microsoft Azure StorSimple ortamında yedekleme yönetimini basitleştiren bir Microsoft Yönetim Konsolu (MMC) ek bileşenidir. StorSimple Snapshot Manager ile Microsoft Azure StorSimple veri veri merkezinde hem de bulutta bir tek bir tümleşik depolama çözümü olarak yedekleme süreçlerini basitleştirir ve maliyetlerin düşürülmesi yönetebilirsiniz.

StorSimple Snapshot Manager Merkezi Yönetim Konsolu yerel tutarlı, zamanında noktası yedek kopyalarını oluşturmanızı sağlar ve bulut veri. Örneğin, konsola kullanabilirsiniz:

* Yapılandırma, yedekleme ve birimleri silin.
* Birim gruplarını emin olmak için yapılandırma uygulamayla tutarlı yedeklenen verileri.
* Yedekleme ilkelerini yönetme, böylece verileri önceden belirlenmiş bir zamanlamayla yedeklenir.
* Bulutta depolanan ve olağanüstü durum kurtarma için kullanılan verileri bağımsız bir kopyasını oluşturun.

Bu makalede, StorSimple Snapshot Manager ve sistem yönetim görevleri ve iş akışlarının tamamlanması kullanılacağını anlatan bir öğretici için bağlantılar sağlar.

* StorSimple Snapshot Manager bileşenlerini ve mimarisi hakkında daha fazla bilgi için bkz: [StorSimple Snapshot Manager nedir?](storsimple-what-is-snapshot-manager.md) 
* StorSimple Snapshot Manager'ı indirmek için Git [StorSimple Snapshot Manager indirme sayfasına](https://www.microsoft.com/download/details.aspx?id=44220).
* StorSimple Snapshot Manager dağıtımı yordamları için Git [StorSimple Snapshot Manager'ı Dağıtma](storsimple-snapshot-manager-deployment.md).

> [!NOTE]
> Microsoft Azure StorSimple sanal (diğer adıyla StorSimple sanal cihazları şirket) dizilerini yönetmek için StorSimple Snapshot Manager'ı kullanamazsınız.


## <a name="storsimple-snapshot-manager-tasks-and-workflows"></a>StorSimple Snapshot Manager görevleri ve iş akışları
StorSimple Snapshot Manager'ı güncel, zamanlanan ve tamamlanan yedekleme işlerini yönetme ve izlemek için kullanabilirsiniz. Ayrıca, StorSimple Snapshot Manager en fazla 64 tamamlanmış yedekleme kataloğu sağlar. Katalog bulun ve birimleri veya tek tek dosyaları geri yüklemek için kullanabilirsiniz. 

| BUNU YAPMAK İSTİYORSANIZ... | BU ÖĞRETİCİYİ KULLANIN... |
|:--- |:--- |
| StorSimple Snapshot Manager hakkında daha fazla bilgi edinin |[StorSimple Snapshot Manager nedir?](storsimple-what-is-snapshot-manager.md) |
| StorSimple anlık görüntü Yöneticisi'ni yükleyin<br>StorSimple Snapshot Manager'ı yeniden yükleyin<br>StorSimple anlık görüntü Yöneticisini Kaldır |[StorSimple Snapshot Manager'ı dağıtma](storsimple-snapshot-manager-deployment.md) |
| StorSimple Snapshot Manager'ı kullanın menüleri ve özellikleri:<ul><li>Menü çubuğu</li><li>Araç çubuğu</li><li>Kapsam bölmesi</li><li>Sonuçlar Bölmesi</li><li>Eylemler bölmesi</li><li>Klavye ile gezintiyi ve kısayolları</li></ul> |[StorSimple Snapshot Manager kullanıcı arabirimi](storsimple-use-snapshot-manager.md) |
| StorSimple anlık görüntü Yöneticisi'nde bulunan ortak MMC özellikleri kullanın:<ul><li>Görünüm</li><li>Buradan yeni pencere</li><li>Yenile</li><li>Listeyi dışarı aktar</li><li>Help</li></ul> |[StorSimple Snapshot Manager'da MMC menü eylemlerinin kullanın](storsimple-snapshot-manager-mmc-menu.md) |
| Ekleme veya bir cihazı Değiştir<br>Bir cihazı bağlayın<br>İçeri aktarılan birim gruplarını doğrulayın<br>Bağlı cihazlar Yenile<br>Bir cihaz kimlik doğrulaması<br>Cihaz ayrıntılarını görüntüleme<br>Cihaz yapılandırmasını silme<br>Cihaz parolasını değiştirme<br>Başarısız bir cihazı Değiştir<br> |[Bağlanmak ve StorSimple cihazları yönetmek için StorSimple anlık görüntü Yöneticisi'ni kullanın](storsimple-snapshot-manager-manage-devices.md) |
| Bağlama birimleri<br>Birimler hakkındaki bilgileri görüntüleme<br>Birim Sil<br>Birimleri yeniden tara<br>Yapılandırma ve temel bir birimi yedekleyin<br>Yapılandırma ve dinamik yansıtılmış birim yedekleme |[StorSimple Snapshot Manager görüntülemek ve birimler yönetmek için kullanın](storsimple-snapshot-manager-manage-volumes.md) |
| Birim grupları görüntüleyin<br>Bir birim grubu oluşturun<br>Bir birim grubunu yedekleme<br>Bir birim grubu Düzenle<br>Bir birim grubu Sil |[Birim grupları oluşturmak ve yönetmek için StorSimple Snapshot Manager'ı kullanın](storsimple-snapshot-manager-manage-volume-groups.md) |
| Bir yedekleme ilkesi oluşturma <br>Yedekleme ilkesini Düzenle<br>Yedekleme ilkesini silme |[Yedekleme ilkeleri oluşturma ve yönetme için StorSimple Snapshot Manager'ı kullanın](storsimple-snapshot-manager-manage-backup-policies.md) |
| Zamanlanmış yedekleme işleri görüntüleme ve yönetme<br>Son yedekleme işleri görüntüleme ve yönetme<br>Görüntüleme ve şu anda çalışan yedekleme işi yönetme |[Yedekleme İşleri görüntüleme ve yönetme için StorSimple Snapshot Manager'ı kullanın](storsimple-snapshot-manager-manage-backup-jobs.md) |
| Bir birime geri yükleme<br>Bir birim veya birim grubu kopyalama<br>Yedek Sil<br>Dosyayı kurtarma<br>StorSimple Snapshot Manager veritabanını geri yükleme |[Yedekleme kataloğunu yönetmek için StorSimple anlık görüntü Yöneticisi'ni kullanın](storsimple-snapshot-manager-manage-backup-catalog.md) |

## <a name="next-steps"></a>Sonraki adımlar
[StorSimple Snapshot Manager'ı indirin](https://www.microsoft.com/download/details.aspx?id=44220).

