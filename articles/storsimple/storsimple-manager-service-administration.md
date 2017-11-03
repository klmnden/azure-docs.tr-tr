---
title: "StorSimple Yöneticisi Hizmeti Yönetimi | Microsoft Docs"
description: "Azure Klasik Portalı'nda StorSimple Yöneticisi hizmetini kullanarak StorSimple Cihazınızı yönetmeyi öğrenin."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 2586582e-d85c-42e1-afb3-be734c1c0461
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/21/2016
ms.author: alkohli
ms.openlocfilehash: 22924da07434a06f4c822d97a2afd02ea982e0e0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-storsimple-manager-service-to-administer-your-storsimple-device"></a>StorSimple Cihazınızı yönetmek için StorSimple Yöneticisi hizmetini kullanma
## <a name="overview"></a>Genel Bakış
Bu makalede, çeşitli seçenekleri ve bu kullanıcı Arabirimi gerçekleştirilebilir belirli iş akışları bağlantıları bağlanmak nasıl dahil olmak üzere StorSimple Yöneticisi hizmet arabirimden açıklanmaktadır. Bu kılavuz hem de geçerlidir; StorSimple fiziksel ve sanal cihaz.

Bu makaleyi okuduktan sonra öğreneceksiniz:

* StorSimple Yöneticisi hizmetine bağlanmak
* StorSimple Yöneticisi kullanıcı Arabirimi gidin
* StorSimple Cihazınızı StorSimple Yöneticisi hizmeti aracılığıyla yönetmek

## <a name="connect-to-storsimple-manager-service"></a>StorSimple Yöneticisi hizmetine bağlanmak
StorSimple Yöneticisi hizmeti Microsoft Azure üzerinde çalışır ve birden çok StorSimple cihazını bağlanır. Bir tarayıcıda çalışan merkezi bir Microsoft Azure Klasik portalı, bu cihazları yönetmek için kullanın. StorSimple Yöneticisi hizmetine bağlanmak için aşağıdakileri yapın.

#### <a name="to-connect-to-the-service"></a>Hizmete bağlanmak için
1. Gidin [https://manage.windowsazure.com/](https://manage.windowsazure.com/).
2. Microsoft hesabı kimlik bilgilerinizi kullanarak (sağ üst tarafında bölmesinin bulunur) Microsoft Azure Klasik portalında oturum açın.
3. StorSimple Yöneticisi hizmetine erişmek için sol gezinti bölmesinde kaydırın.

## <a name="navigate-storsimple-manager-service-ui"></a>StorSimple Yöneticisi hizmeti kullanıcı Arabirimi gidin
Gezinme hiyerarşi StorSimple Yöneticisi hizmeti için kullanıcı Arabirimi aşağıdaki tabloda gösterilir.

* **StorSimple Yöneticisi** giriş sayfası kullanıcı Arabirimi hizmet düzeyi sayfalarına bir hizmet kapsamındaki tüm aygıtlara uygulanabilir, alır.
* **Aygıtları** sayfa sayfalara aygıt – düzeyi UI belirli bir cihaza uygulanabilir, alır.
* **Birim kapsayıcıları** sayfa birim sayfasına bir cihaz ile ilişkili tüm birimleri gösterir, alır.

#### <a name="storsimple-manager-service-navigational-hierarchy"></a>StorSimple Yöneticisi hizmeti gezinme hiyerarşisi
| Giriş sayfası | Hizmet düzeyi sayfaları | Cihaz düzeyinde sayfaları | Cihaz düzeyinde sayfaları |
| --- | --- | --- | --- |
| StorSimple Yöneticisi hizmeti |Hizmet panosu |Cihaz Pano | |
| Aygıtları → |İzleme | | |
| Yedekleme kataloğu |Birim containers→ |Birimleri | |
| (Hizmeti) yapılandırma |Yedekleme ilkeleri | | |
| İşler |(Cihaz) yapılandırma | | |
| Uyarılar |Bakım | | |

![Video var](./media/storsimple-manager-service-administration/Video_icon.png) **Video var**

StorSimple Yöneticisi hizmeti kullanıcı arabirimi aracılığıyla anlatan bir videoyu izlemek için tıklatın [burada](https://azure.microsoft.com/documentation/videos/storsimple-manager-service-overview/).

## <a name="administer-storsimple-device-using-storsimple-manager-service"></a>StorSimple Yöneticisi hizmetini kullanarak StorSimple cihazı yönetme
Aşağıdaki tabloda genel yönetim görevleri ve StorSimple Yöneticisi hizmeti kullanıcı Arabirimi gerçekleştirilebilir karmaşık iş akışları bir özetini gösterir. Bu görevler üzerinde bunlar başlatılan kullanıcı Arabirimi sayfalarını göre düzenlenir.

Her bir iş akışı hakkında daha fazla bilgi için tablodaki uygun yordamı tıklatın.

#### <a name="storsimple-manager-workflows"></a>StorSimple Yöneticisi iş akışları
| Bunu yapmak istiyorsanız... | Bu UI sayfasına git... | Bu yordamı kullanın. |
| --- | --- | --- |
| Hizmet oluşturma</br>Bir hizmeti silin</br>Hizmet kayıt anahtarını alın</br>Hizmet kayıt anahtarını yeniden oluşturma |StorSimple Yöneticisi hizmeti |[StorSimple Yöneticisi hizmet dağıtma](storsimple-manage-service.md) |
| Değişiklik hizmeti veri şifreleme anahtarı</br>İşlem günlükleri görüntüleme |StorSimple Yöneticisi hizmet → Panosu |[StorSimple Yöneticisi hizmet panosunu kullanma](storsimple-service-dashboard.md) |
| Bir cihazı devre dışı</br>Bir aygıtı silme |StorSimple Yöneticisi hizmeti → cihazları |[Bir cihazı silmek veya devre dışı bırakma](storsimple-deactivate-and-delete-device.md) |
| Olağanüstü durum kurtarma ve aygıt yük devretme hakkında bilgi edinin</br>Fiziksel bir cihaza yük devretme</br>Sanal cihaza yük devretme</br>İş sürekliliği olağanüstü durum kurtarma (BCDR) |StorSimple Yöneticisi hizmeti → cihazları |[StorSimple cihazınız için yük devretme ve olağanüstü durum kurtarma](storsimple-device-failover-disaster-recovery.md) |
| Bir birim için liste yedeklemeleri</br>Bir yedekleme kümesi seçin</br>Bir yedekleme kümesi Sil |StorSimple Yöneticisi hizmet → yedekleme kataloğu |[Yedeklemeleri yönetme](storsimple-manage-backup-catalog.md) |
| Bir birimi kopyalama |StorSimple Yöneticisi hizmet → yedekleme kataloğu |[Bir birimi kopyalama](storsimple-clone-volume.md) |
| Bir yedekleme kümesi geri yükleme |StorSimple Yöneticisi hizmet → yedekleme kataloğu |[Bir yedekleme kümesi geri yükleme](storsimple-restore-from-backup-set.md) |
| Depolama hesapları hakkında</br>Depolama hesabı ekleme</br>Bir depolama hesabı Düzenle</br>Bir depolama hesabını silme</br>Depolama hesaplarının anahtar döndürme |StorSimple Yöneticisi hizmet → yapılandırın |[Depolama hesaplarını yönetme](storsimple-manage-storage-accounts.md) |
| Bant genişliği şablonları hakkında</br>Bant genişliği şablonu ekleyin</br>Bant genişliği Şablonu Düzenle</br>Bant genişliği Şablonu Sil</br>Varsayılan bant genişliği şablonu kullanın</br>Belirli bir zamanda başlatır süren bir bant genişliği şablonu oluştur |StorSimple Yöneticisi hizmet → yapılandırın |[Bant genişliği şablonlarını yönetme](storsimple-manage-bandwidth-templates.md) |
| Erişim denetimi kayıtları hakkında</br>Bir erişim denetimi kaydı oluşturun</br>Bir erişim denetimi kaydı Düzenle</br>Bir erişim denetimi kaydını sil |StorSimple Yöneticisi hizmet → yapılandırın |[Erişim denetimi kayıtlarını yönetme](storsimple-manage-acrs.md) |
| İş ayrıntılarını görüntüleme</br>Bir işi iptal etme |StorSimple Yöneticisi hizmet → işleri |[İşleri yönetme](storsimple-manage-jobs.md) |
| Uyarı bildirimleri alma</br>Uyarıları yönetme</br>Uyarıları gözden geçirin |StorSimple Yöneticisi hizmet → uyarıları |[StorSimple uyarıları görüntüleyin ve yönetin](storsimple-manage-alerts.md) |
| Bağlı başlatıcıları görüntüleyin</br>Cihaz seri numarasını Bul</br>Hedef IQN Bul |StorSimple Yöneticisi hizmet → cihazların → Panosu |[StorSimple cihaz Pano kullanın](storsimple-device-dashboard.md) |
| İzleme grafikleri oluşturma |StorSimple Yöneticisi hizmet → cihazların → izleme |[StorSimple Cihazınızı izleme](storsimple-monitor-device.md) |
| Birim kapsayıcısı Ekle</br>Birim kapsayıcısı değiştirme</br>Birim kapsayıcısı Sil |StorSimple Yöneticisi hizmet → cihazların → birim kapsayıcıları |[Birim kapsayıcıları yönetme](storsimple-manage-volume-containers.md) |
| Birim Ekle</br>Bir birim değiştirme</br>Bir birim çevrimdışı duruma getirin</br>Bir birim Sil</br>Bir birimi izleyin |StorSimple Yöneticisi hizmet → cihazların → birim kapsayıcıları → birimleri |[Birimleri yönetme](storsimple-manage-volumes.md) |
| Cihaz ayarlarını değiştirme</br>Saat ayarlarını değiştirme</br>DNS.md ayarlarını değiştirme</br>Ağ arabirimleri yapılandırın |StorSimple Yöneticisi hizmet → cihazların → yapılandırın |[StorSimple cihazınız için aygıt yapılandırmasını Değiştir](storsimple-modify-device-config.md) |
| Görünümü web proxy ayarları |StorSimple Yöneticisi hizmet → cihazların → yapılandırın |[Cihazınız için Web Proxy'yi Yapılandır](storsimple-configure-web-proxy.md) |
| Cihaz Yöneticisi parolasını değiştirme</br>StorSimple Snapshot Manager parolasını değiştirme |StorSimple Yöneticisi hizmet → cihazların → yapılandırın |[StorSimple parolalarını değiştirme](storsimple-change-passwords.md) |
| Uzaktan Yönetimi yapılandırma |StorSimple Yöneticisi hizmet → cihazların → yapılandırın |[StorSimple cihazınıza uzaktan bağlanma](storsimple-remote-connect.md) |
| Uyarı ayarlarını yapılandırma |StorSimple Yöneticisi hizmet → cihazların → yapılandırın |[StorSimple uyarıları görüntüleyin ve yönetin](storsimple-manage-alerts.md) |
| StorSimple cihazınız için CHAP yapılandırma |StorSimple Yöneticisi hizmet → cihazların → yapılandırın |[StorSimple cihazınız için CHAP yapılandırma](storsimple-configure-chap.md) |
| Yedekleme ilkesi ekleme</br>Ekleyin veya bir zamanlama değiştirin</br>Bir yedekleme ilkesi silme</br>El ile yedekleyin</br>Birden çok birimleri ve zamanlamaları özel bir yedekleme ilkesi oluşturma |StorSimple Yöneticisi hizmeti → cihazların → yedekleme ilkeleri |[Yedekleme ilkelerini yönetme](storsimple-manage-backup-policies.md) |
| Aygıt denetleyicileri Durdur</br>Cihaz denetleyicilerini yeniden başlatın</br>Aygıt denetleyicileri Kapat</br>Cihazınızı fabrika varsayılan ayarlarına sıfırlama</br>(Yalnızca şirket içi cihaz için yukarıdaki dışında) |StorSimple Yöneticisi hizmet → cihazların → bakım |[StorSimple cihaz denetleyicisi yönetme](storsimple-manage-device-controller.md) |
| StorSimple donanım bileşenleri hakkında bilgi edinin</br>Donanım durumunu izleyin</br>(Yalnızca şirket içi cihaz için yukarıdaki dışında) |StorSimple Yöneticisi hizmet → cihazların → bakım |[İzleyici donanım bileşenleri](storsimple-monitor-hardware-status.md) |
| Bir destek paketi oluştur |StorSimple Yöneticisi hizmet → cihazların → bakım |[Oluşturma ve Destek Paketi yönetme](storsimple-create-manage-support-package.md) |
| Yazılım güncelleştirmelerini yükle |StorSimple Yöneticisi hizmet → cihazların → bakım |[Cihazınızı güncelleştirme](storsimple-update-device.md) |

## <a name="next-steps"></a>Sonraki adımlar
StorSimple Cihazınızı günlük işlemleri veya herhangi bir donanım bileşenlerinden biri ile herhangi bir sorunla karşılaşırsanız, bakın:

* [İşletimsel bir aygıtı sorun giderme](storsimple-troubleshoot-operational-device.md)
* [Gösterge LED'lerinin İzleme StorSimple kullanın](storsimple-monitoring-indicators.md)

Sorunları çözümlenemiyor ve bir hizmet isteği oluşturmak ihtiyacınız varsa başvurmak [Microsoft Destek birimine başvurun](storsimple-contact-microsoft-support.md).

