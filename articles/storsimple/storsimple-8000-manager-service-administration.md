---
title: StorSimple cihaz Yöneticisi Hizmeti Yönetimi | Microsoft Docs
description: Azure portalında StorSimple cihaz Yöneticisi hizmetini kullanarak StorSimple aygıtınızı yönetmeyi öğrenin.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/12/2017
ms.author: alkohli
ms.openlocfilehash: b5490c4e79ee1458b498f539c0db2cc189fce7f7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60723315"
---
# <a name="use-the-storsimple-device-manager-service-to-administer-your-storsimple-device"></a>StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanın

## <a name="overview"></a>Genel Bakış

Bu makalede, çeşitli seçenekler ve bağlantıları bu UI üzerinden gerçekleştirilen belirli iş akışlarına bağlanmak nasıl dahil olmak üzere StorSimple cihaz Yöneticisi hizmeti arabirimi. Bu kılavuz her ikisi için de geçerlidir; StorSimple fiziksel cihazı ve bulut Gereci.

Bu makaleyi okuduktan sonra öğreneceksiniz:

* StorSimple cihaz Yöneticisi hizmetine bağlayın
* StorSimple Cihazınızı StorSimple cihaz Yöneticisi hizmeti aracılığıyla yönetme

## <a name="connect-to-storsimple-device-manager-service"></a>StorSimple cihaz Yöneticisi hizmetine bağlayın

StorSimple cihaz Yöneticisi hizmeti, Microsoft Azure'da çalışan ve birden çok StorSimple aygıtına bağlanır. Bu cihazları yönetmek için bir tarayıcıda çalışan merkezi bir Microsoft Azure portal'ı kullanın. StorSimple cihaz Yöneticisi hizmetine bağlanmak için aşağıdakileri yapın.

#### <a name="to-connect-to-the-service"></a>Hizmetine bağlanmak için
1. Gidin [ https://portal.azure.com/ ](https://portal.azure.com/).
2. Microsoft hesabı kimlik bilgilerinizi kullanarak, (üst sağ bölmenin bulunur) Microsoft Azure Portal'da oturum açın.
3. StorSimple cihaz Yöneticisi hizmetine erişmek için sol gezinti bölmesinde kaydırın.


## <a name="administer-storsimple-device-using-storsimple-device-manager-service"></a>StorSimple cihazı StorSimple cihaz Yöneticisi hizmetini kullanarak yönetme

Aşağıdaki tabloda, genel yönetim görevleri ve StorSimple cihaz Yöneticisi hizmeti kullanıcı Arabirimi içinde gerçekleştirilen karmaşık iş akışları bir özetini gösterir. Bu görevler, bunlar üzerinde başlatılan UI dikey pencereleri göre düzenlenir.

Her bir iş akışı hakkında daha fazla bilgi için tablodaki ilgili yordamı tıklayın.

#### <a name="storsimple-device-manager-workflows"></a>StorSimple cihaz Yöneticisi iş akışları

| Bunu yapmak istiyorsanız... | Bu yordamı kullanın. |
| --- | --- |
| Bir hizmet oluşturma</br>Bir hizmeti Sil</br>Hizmet kayıt anahtarını alın</br>Hizmet kayıt anahtarını yeniden oluştur |[StorSimple cihaz Yöneticisi hizmetini dağıtma](storsimple-8000-manage-service.md) |
| Etkinlik günlüklerini görüntüleme |[StorSimple cihaz Yöneticisi hizmet özetini kullanma](storsimple-8000-service-dashboard.md) |
| Hizmet veri şifreleme anahtarı değiştirme</br>İşlem günlüklerini görüntüleme |[StorSimple cihaz Yöneticisi hizmet panosunu kullanma](storsimple-8000-service-dashboard.md) |
| Bir cihazı devre dışı</br>Bir cihazı silme |[Bir cihazı silmek veya devre dışı bırakma](storsimple-8000-deactivate-and-delete-device.md) |
| Olağanüstü durum kurtarma ve cihaz yük devretme hakkında bilgi edinin</br>Fiziksel cihaza yük devretme</br>Sanal cihaza yük devretme</br>İş sürekliliği, olağanüstü durum kurtarma (BCDR) |[StorSimple cihazınız için yük devretme ve olağanüstü durum kurtarma](storsimple-8000-device-failover-disaster-recovery.md) |
| Bir birim için yedekleri Listele</br>Yedek kümesi seçin</br>Yedek kümesini silme |[Yedekleri yönetme](storsimple-8000-manage-backup-catalog.md) |
| Bir birimi kopyalama |[Bir birimi kopyalama](storsimple-8000-clone-volume-u2.md) |
| Bir yedekleme kümesi geri yükleme |[Bir yedekleme kümesi geri yükleme](storsimple-8000-restore-from-backup-set-u2.md) |
| Depolama hesapları hakkında</br>Bir depolama hesabı ekleme</br>Bir depolama hesabı Düzenle</br>Bir depolama hesabını silme</br>Depolama hesabı anahtar rotasyonu |[Depolama hesaplarını yönetme](storsimple-8000-manage-storage-accounts.md) |
| Bant genişliği şablonları hakkında</br>Bir bant genişliği şablonu ekleme</br>Bant genişliği Şablonu Düzenle</br>Bir bant genişliği şablonunu silme</br>Varsayılan bant genişliği şablonu kullanın</br>Belirli bir zamanda başlatan bir günlük bant genişliği şablonu oluşturma |[Bant genişliği şablonlarını yönetme](storsimple-8000-manage-bandwidth-templates.md) |
| Erişim denetimi kayıtları hakkında</br>Erişim denetimi kaydı oluşturma</br>Bir erişim denetimi kaydını Düzenle</br>Bir erişim denetim kaydını silme |[Erişim denetimi kayıtları Yönet](storsimple-8000-manage-acrs.md) |
| İş ayrıntılarını görüntüleme</br>Bir işi iptal et |[İşleri yönetme](storsimple-8000-manage-jobs-u2.md) |
| Uyarı bildirimleri alma</br>Uyarıları yönetme</br>Uyarıları gözden geçirin |[StorSimple uyarıları görüntüleyin ve yönetin](storsimple-8000-manage-alerts.md) |
| İzleme grafiklerini oluşturma |[StorSimple Cihazınızı izleyin](storsimple-monitor-device.md) |
| Birim kapsayıcısı Ekle</br>Birim kapsayıcısını Değiştir</br>Birim kapsayıcısını silme |[Birim kapsayıcıları yönetme](storsimple-8000-manage-volume-containers.md) |
| Birim Ekle</br>Bir birimi Değiştir</br>Bir birimi çevrimdışı duruma getirin</br>Birim Sil</br>Bir birimi izleyin |[Birimleri yönetme](storsimple-8000-manage-volumes-u2.md) |
| Cihaz ayarlarını değiştirme</br>Saat ayarlarını değiştirme</br>DNS.md ayarlarını değiştirme</br>Ağ arabirimi yapılandırın |[StorSimple cihazınız için cihaz yapılandırmasını değiştirme](storsimple-8000-modify-device-config.md) |
| Görünümü web proxy ayarları |[Cihazınız için Web Proxy'yi Yapılandır](storsimple-8000-configure-web-proxy.md) |
| Cihaz Yöneticisi parolasını değiştirme</br>StorSimple Snapshot Manager parolasını değiştirme |[StorSimple parolalarını değiştirme](storsimple-8000-change-passwords.md) |
| Uzaktan Yönetimi yapılandırma |[StorSimple cihazınıza uzaktan bağlanma](storsimple-8000-remote-connect.md) |
| Uyarı ayarlarını yapılandır |[StorSimple uyarıları görüntüleyin ve yönetin](storsimple-8000-manage-alerts.md) |
| StorSimple cihazınız için CHAP yapılandırma |[StorSimple cihazınız için CHAP yapılandırma](storsimple-configure-chap.md) |
| Yedekleme ilkesi ekleme</br>Bir zamanlamayı değiştirmek veya ekleme</br>Yedekleme ilkesini silme</br>El ile yedekleyin</br>Birden çok birim ve zamanlamaları ile özel bir yedekleme ilkesi oluşturma |[Yedekleme ilkelerini yönetme](storsimple-8000-manage-backup-policies-u2.md) |
| Cihaz denetleyicileri Durdur</br>Cihaz denetleyiciler yeniden başlatılır</br>Cihaz denetleyicileri Kapat</br>Cihazınızı fabrika ayarlarına sıfırlama</br>(Yalnızca şirket içi cihaz için yukarıdaki olabilir) |[StorSimple cihaz denetleyicisini yönetme](storsimple-8000-manage-device-controller.md) |
| StorSimple donanım bileşenleri hakkında bilgi edinin</br>Donanım durumunu izleme</br>(Yalnızca şirket içi cihaz için yukarıdaki olabilir) |[İzleyici donanım bileşenleri](storsimple-8000-monitor-hardware-status.md) |
| Destek paketi oluşturma |[Oluşturma ve bir destek paketi yönetme](storsimple-8000-contact-microsoft-support.md#start-a-support-session-in-windows-powershell-for-storsimple) |
| Yazılım güncelleştirmelerini yükle |[Cihazınızı güncelleştirme](storsimple-update-device.md) |

## <a name="next-steps"></a>Sonraki adımlar

StorSimple cihazınızın günlük işlem veya tüm donanım bileşenleri ile herhangi bir sorun yaşarsanız bakın:

* [Tanılama aracını kullanmayla ilgili sorun giderme](storsimple-8000-diagnostics.md)
* [StorSimple izleme gösterge LED'lerini kullanma](storsimple-monitoring-indicators.md)

Sorunları çözmek olamaz ve bir hizmet isteği oluşturmak ihtiyacınız varsa, başvurmak [Microsoft Destek'e başvur](storsimple-8000-contact-microsoft-support.md).

