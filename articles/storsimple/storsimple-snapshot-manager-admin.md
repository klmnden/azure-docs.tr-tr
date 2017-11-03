---
title: "StorSimple Snapshot Manager Yönetim | Microsoft Docs"
description: "Genel bir bakış ve StorSimple Snapshot Manager çözüm yönetim görevleri ve iş akışları hakkında daha fazla bilgi için bağlantılar sağlar."
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: 1cdbb61d-bd16-4be4-ade2-ceab11508acb
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2016
ms.author: v-sharos
ms.openlocfilehash: a99b3d7336c3dc1a1f249915d6971a49f4b69be6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-storsimple-snapshot-manager-to-administer-your-storsimple-solution"></a>StorSimple çözümünüzün yönetmek için StorSimple anlık görüntü Yöneticisi'ni kullanın

## <a name="overview"></a>Genel Bakış
StorSimple Snapshot Manager, veri koruma ve Microsoft Azure StorSimple ortamda yedekleme yönetimini basitleştirir Microsoft Yönetim Konsolu (MMC) ek bileşenidir. StorSimple Snapshot Manager ile Microsoft Azure StorSimple verileri veri merkezi ve bulut tek bir tümleşik depolama çözümü olarak, bu nedenle yedekleme işlemlerini basitleştirir ve maliyetlerini azaltır yönetebilirsiniz.

StorSimple Snapshot Manager Merkezi Yönetim Konsolu, yerel tutarlı, zaman içinde nokta yedek kopyalarını oluşturmanızı sağlar ve bulut veri. Örneğin, konsola kullanabilirsiniz:

* Yapılandırma, yedekleme ve birimleri silin.
* Sağlamak için birim grupları yapılandırmak, yedeklenen verileri uygulamayla tutarlı olan.
* Böylece verilerin belirlenmiş bir zamanlamayla yedeklendiğinden yedekleme ilkelerini yönetin.
* Bulutta depolanan ve olağanüstü durum kurtarma için kullanılan veri bağımsız kopyalarını oluşturun.

Bu makalede, StorSimple Snapshot Manager ve sistem yönetim görevleri ve iş akışları tamamlamak anlatan öğreticileri için bağlantılar sağlar.

* StorSimple Snapshot Manager bileşenleri ve mimarisi hakkında daha fazla bilgi için bkz: [StorSimple anlık görüntü Yöneticisi nedir?](storsimple-what-is-snapshot-manager.md) 
* StorSimple Snapshot Manager indirmek için Git [StorSimple Snapshot Manager indirme sayfası](https://www.microsoft.com/download/details.aspx?id=44220).
* StorSimple Snapshot Manager dağıtım yordamları için Git [dağıtmak StorSimple Snapshot Manager](storsimple-snapshot-manager-deployment.md).

> [!NOTE]
> StorSimple Snapshot Manager, Microsoft Azure StorSimple sanal (olarak da bilinen StorSimple sanal cihazlar şirket) diziler yönetmek için kullanamazsınız.


## <a name="storsimple-snapshot-manager-tasks-and-workflows"></a>StorSimple Snapshot Manager görevleri ve iş akışları
StorSimple Snapshot Manager geçerli, zamanlanmış ve tamamlanan yedekleme işlerini yönetmek ve izlemek için kullanabilirsiniz. Ayrıca, StorSimple Snapshot Manager en fazla 64 tamamlanmış yedekleme kataloğu sağlar. Katalog bulmak ve birimleri veya tek tek dosyaları geri yüklemek için kullanabilirsiniz. 

| BUNU YAPMAK İSTİYORSANIZ... | BU ÖĞRETİCİYİ KULLANIN... |
|:--- |:--- |
| StorSimple anlık görüntü Yöneticisi hakkında daha fazla bilgi edinin |[StorSimple anlık görüntü Yöneticisi nedir?](storsimple-what-is-snapshot-manager.md) |
| StorSimple anlık görüntü Yöneticisi'ni yükleyin<br>StorSimple anlık görüntü Yöneticisi'ni yeniden yükleyin<br>StorSimple anlık görüntü Yöneticisi'ni Kaldır |[StorSimple Snapshot Manager dağıtma](storsimple-snapshot-manager-deployment.md) |
| StorSimple anlık görüntü Yöneticisi'ni kullanın menüleri ve özellikleri:<ul><li>Menü çubuğu</li><li>Araç çubuğu</li><li>Kapsam bölmesi</li><li>Sonuçlar Bölmesi</li><li>Eylemler bölmesi</li><li>Klavye gezinme ve kısayollar</li></ul> |[StorSimple anlık görüntü Yöneticisi kullanıcı arabirimi](storsimple-use-snapshot-manager.md) |
| StorSimple anlık görüntü Yöneticisi'nde bulunan ortak MMC özelliklerini kullanın:<ul><li>Görünüm</li><li>Buradan yeni pencere</li><li>Yenileme</li><li>Listeyi dışarı aktar</li><li>Yardım</li></ul> |[StorSimple anlık görüntü Yöneticisi'nde MMC menü eylemlerini kullanın](storsimple-snapshot-manager-mmc-menu.md) |
| Ekleme veya bir aygıt değiştirme<br>Bir aygıtı bağlayın<br>İçeri aktarılan birim grupları doğrulayın<br>Bağlı aygıtlar Yenile<br>Bir cihaz kimlik doğrulaması<br>Cihaz ayrıntıları görüntüle<br>Cihaz yapılandırmasını Sil<br>Aygıt parola değiştirme<br>Başarısız aygıt değiştirin<br> |[StorSimple Snapshot Manager bağlanmak ve StorSimple cihazları yönetmek için kullanın](storsimple-snapshot-manager-manage-devices.md) |
| Bağlama birimleri<br>Birimler hakkındaki bilgileri görüntüleme<br>Bir birim Sil<br>Birimleri yeniden tara<br>Yapılandırma ve temel bir birimi yedekleyin<br>Yapılandırma ve dinamik yansıtılmış birim yedekleme |[StorSimple Snapshot Manager görüntülemek ve birimleri yönetmek için kullanın](storsimple-snapshot-manager-manage-volumes.md) |
| Birim grupları görüntüle<br>Birim grubu oluştur<br>Bir birim grubunu yedekleyin<br>Bir birim grubu Düzenle<br>Bir birim grubu Sil |[Birim grupları oluşturmak ve yönetmek için StorSimple anlık görüntü Yöneticisi'ni kullanın](storsimple-snapshot-manager-manage-volume-groups.md) |
| Bir yedekleme İlkesi Oluştur <br>Yedekleme ilkesini Düzenle<br>Bir yedekleme ilkesi silme |[StorSimple Snapshot Manager oluşturmak ve yedekleme ilkelerini yönetmek için kullanın](storsimple-snapshot-manager-manage-backup-policies.md) |
| Zamanlanmış yedekleme işleri görüntüle ve Yönet<br>Son yedekleme işleri görüntüle ve Yönet<br>Görüntüleme ve şu anda çalışan yedekleme işi yönetme |[StorSimple Snapshot Manager görüntülemek ve yedekleme işlerini yönetmek için kullanın](storsimple-snapshot-manager-manage-backup-jobs.md) |
| Bir birime geri yükleme<br>Bir birim veya birim grubu kopyalama<br>Yedek Sil<br>Bir dosya kurtarma<br>StorSimple Snapshot Manager veritabanını geri yükle |[Yedekleme kataloğu yönetmek için StorSimple anlık görüntü Yöneticisi'ni kullanın](storsimple-snapshot-manager-manage-backup-catalog.md) |

## <a name="next-steps"></a>Sonraki adımlar
[StorSimple anlık görüntü Yöneticisi karşıdan](https://www.microsoft.com/download/details.aspx?id=44220).

