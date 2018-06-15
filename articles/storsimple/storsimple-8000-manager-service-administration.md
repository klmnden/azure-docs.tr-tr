---
title: StorSimple cihaz Yöneticisi Hizmeti Yönetimi | Microsoft Docs
description: Azure portalında StorSimple cihaz Yöneticisi hizmetini kullanarak StorSimple Cihazınızı yönetmeyi öğrenin.
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
ms.openlocfilehash: 0e7d7f44a70278a7777ba6c32c8e546074953fdc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23875042"
---
# <a name="use-the-storsimple-device-manager-service-to-administer-your-storsimple-device"></a>StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma

## <a name="overview"></a>Genel Bakış

Bu makalede, çeşitli seçenekleri ve bu kullanıcı Arabirimi gerçekleştirilebilir belirli iş akışları bağlantıları bağlanmak nasıl dahil olmak üzere StorSimple cihaz Yöneticisi hizmet arabirimden açıklanmaktadır. Bu kılavuz hem de geçerlidir; StorSimple fiziksel cihazı ve bulut uygulaması.

Bu makaleyi okuduktan sonra öğreneceksiniz:

* StorSimple cihaz Yöneticisi hizmetine bağlanmak
* StorSimple Cihazınızı StorSimple cihaz Yöneticisi hizmeti aracılığıyla yönetmek

## <a name="connect-to-storsimple-device-manager-service"></a>StorSimple cihaz Yöneticisi hizmetine bağlanmak

StorSimple cihaz Yöneticisi hizmeti Microsoft Azure üzerinde çalışır ve birden çok StorSimple cihazını bağlanır. Bu cihazları yönetmek için bir tarayıcıda çalışan merkezi bir Microsoft Azure portalını kullanın. StorSimple cihaz Yöneticisi hizmetine bağlanmak için aşağıdakileri yapın.

#### <a name="to-connect-to-the-service"></a>Hizmete bağlanmak için
1. Gidin [https://portal.azure.com/](https://portal.azure.com/).
2. Microsoft hesabı kimlik bilgilerinizi kullanarak (sağ üst tarafında bölmesinin bulunur) Microsoft Azure portalında oturum açın.
3. StorSimple cihaz Yöneticisi hizmetine erişmek için sol gezinti bölmesinde kaydırın.


## <a name="administer-storsimple-device-using-storsimple-device-manager-service"></a>StorSimple cihaz Yöneticisi hizmetini kullanarak StorSimple cihazı yönetme

Aşağıdaki tabloda genel yönetim görevleri ve StorSimple Aygıt Yöneticisi'ni Hizmeti'nde UI gerçekleştirilebilir karmaşık iş akışları bir özetini gösterir. Bu görevler üzerinde bunlar başlatılan UI Kanatlar göre düzenlenir.

Her bir iş akışı hakkında daha fazla bilgi için tablodaki uygun yordamı tıklatın.

#### <a name="storsimple-device-manager-workflows"></a>StorSimple cihaz Manager iş akışları

| Bunu yapmak istiyorsanız... | Bu yordamı kullanın. |
| --- | --- |
| Hizmet oluşturma</br>Bir hizmeti silin</br>Hizmet kayıt anahtarını alın</br>Hizmet kayıt anahtarını yeniden oluşturma |[StorSimple cihaz Yöneticisi hizmet dağıtma](storsimple-8000-manage-service.md) |
| Etkinlik günlükleri görüntüleme |[StorSimple cihaz Yöneticisi hizmet özeti kullanın](storsimple-8000-service-dashboard.md) |
| Değişiklik hizmeti veri şifreleme anahtarı</br>İşlem günlükleri görüntüleme |[StorSimple cihaz Yöneticisi hizmet panosunu kullanma](storsimple-8000-service-dashboard.md) |
| Bir cihazı devre dışı</br>Bir aygıtı silme |[Bir cihazı silmek veya devre dışı bırakma](storsimple-8000-deactivate-and-delete-device.md) |
| Olağanüstü durum kurtarma ve aygıt yük devretme hakkında bilgi edinin</br>Fiziksel bir cihaza yük devretme</br>Sanal cihaza yük devretme</br>İş sürekliliği olağanüstü durum kurtarma (BCDR) |[StorSimple cihazınız için yük devretme ve olağanüstü durum kurtarma](storsimple-8000-device-failover-disaster-recovery.md) |
| Bir birim için liste yedeklemeleri</br>Bir yedekleme kümesi seçin</br>Bir yedekleme kümesi Sil |[Yedeklemeleri yönetme](storsimple-8000-manage-backup-catalog.md) |
| Bir birimi kopyalama |[Bir birimi kopyalama](storsimple-8000-clone-volume-u2.md) |
| Bir yedekleme kümesi geri yükleme |[Bir yedekleme kümesi geri yükleme](storsimple-8000-restore-from-backup-set-u2.md) |
| Depolama hesapları hakkında</br>Depolama hesabı ekleme</br>Bir depolama hesabı Düzenle</br>Bir depolama hesabını silme</br>Depolama hesaplarının anahtar döndürme |[Depolama hesaplarını yönetme](storsimple-8000-manage-storage-accounts.md) |
| Bant genişliği şablonları hakkında</br>Bant genişliği şablonu ekleyin</br>Bant genişliği Şablonu Düzenle</br>Bant genişliği Şablonu Sil</br>Varsayılan bant genişliği şablonu kullanın</br>Belirli bir zamanda başlatır süren bir bant genişliği şablonu oluştur |[Bant genişliği şablonlarını yönetme](storsimple-8000-manage-bandwidth-templates.md) |
| Erişim denetimi kayıtları hakkında</br>Bir erişim denetimi kaydı oluşturun</br>Bir erişim denetimi kaydı Düzenle</br>Bir erişim denetimi kaydını sil |[Erişim denetimi kayıtlarını yönetme](storsimple-8000-manage-acrs.md) |
| İş ayrıntılarını görüntüleme</br>Bir işi iptal etme |[İşleri yönetme](storsimple-8000-manage-jobs-u2.md) |
| Uyarı bildirimleri alma</br>Uyarıları yönetme</br>Uyarıları gözden geçirin |[StorSimple uyarıları görüntüleyin ve yönetin](storsimple-8000-manage-alerts.md) |
| İzleme grafikleri oluşturma |[StorSimple Cihazınızı izleme](storsimple-monitor-device.md) |
| Birim kapsayıcısı Ekle</br>Birim kapsayıcısı değiştirme</br>Birim kapsayıcısı Sil |[Birim kapsayıcıları yönetme](storsimple-8000-manage-volume-containers.md) |
| Birim Ekle</br>Bir birim değiştirme</br>Bir birim çevrimdışı duruma getirin</br>Bir birim Sil</br>Bir birimi izleyin |[Birimleri yönetme](storsimple-8000-manage-volumes-u2.md) |
| Cihaz ayarlarını değiştirme</br>Saat ayarlarını değiştirme</br>DNS.md ayarlarını değiştirme</br>Ağ arabirimleri yapılandırın |[StorSimple cihazınız için aygıt yapılandırmasını Değiştir](storsimple-8000-modify-device-config.md) |
| Görünümü web proxy ayarları |[Cihazınız için Web Proxy'yi Yapılandır](storsimple-8000-configure-web-proxy.md) |
| Cihaz Yöneticisi parolasını değiştirme</br>StorSimple Snapshot Manager parolasını değiştirme |[StorSimple parolalarını değiştirme](storsimple-8000-change-passwords.md) |
| Uzaktan Yönetimi yapılandırma |[StorSimple cihazınıza uzaktan bağlanma](storsimple-8000-remote-connect.md) |
| Uyarı ayarlarını yapılandırma |[StorSimple uyarıları görüntüleyin ve yönetin](storsimple-8000-manage-alerts.md) |
| StorSimple cihazınız için CHAP yapılandırma |[StorSimple cihazınız için CHAP yapılandırma](storsimple-configure-chap.md) |
| Yedekleme ilkesi ekleme</br>Ekleyin veya bir zamanlama değiştirin</br>Bir yedekleme ilkesi silme</br>El ile yedekleyin</br>Birden çok birimleri ve zamanlamaları özel bir yedekleme ilkesi oluşturma |[Yedekleme ilkelerini yönetme](storsimple-8000-manage-backup-policies-u2.md) |
| Aygıt denetleyicileri Durdur</br>Cihaz denetleyicilerini yeniden başlatın</br>Aygıt denetleyicileri Kapat</br>Cihazınızı fabrika varsayılan ayarlarına sıfırlama</br>(Yalnızca şirket içi cihaz için yukarıdaki dışında) |[StorSimple cihaz denetleyicisi yönetme](storsimple-8000-manage-device-controller.md) |
| StorSimple donanım bileşenleri hakkında bilgi edinin</br>Donanım durumunu izleyin</br>(Yalnızca şirket içi cihaz için yukarıdaki dışında) |[İzleyici donanım bileşenleri](storsimple-8000-monitor-hardware-status.md) |
| Bir destek paketi oluştur |[Oluşturma ve Destek Paketi yönetme](storsimple-8000-contact-microsoft-support.md#start-a-support-session-in-windows-powershell-for-storsimple) |
| Yazılım güncelleştirmelerini yükle |[Cihazınızı güncelleştirme](storsimple-update-device.md) |

## <a name="next-steps"></a>Sonraki adımlar

StorSimple Cihazınızı günlük işlemleri veya herhangi bir donanım bileşenlerinden biri ile herhangi bir sorunla karşılaşırsanız, bakın:

* [Tanılama Aracı'nı kullanarak sorun giderme](storsimple-8000-diagnostics.md)
* [Gösterge LED'lerinin İzleme StorSimple kullanın](storsimple-monitoring-indicators.md)

Sorunları çözümlenemiyor ve bir hizmet isteği oluşturmak ihtiyacınız varsa başvurmak [Microsoft Destek birimine başvurun](storsimple-8000-contact-microsoft-support.md).

