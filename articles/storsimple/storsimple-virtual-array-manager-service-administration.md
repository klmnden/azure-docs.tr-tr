---
title: Microsoft Azure StorSimple Manager Virtual Array Yönetim | Microsoft Docs
description: Azure portalında StorSimple cihaz Yöneticisi hizmetini kullanarak şirket içi StorSimple sanal dizininiz yönetmeyi öğrenin.
services: storsimple
documentationcenter: ''
author: alkohli
manager: carmonm
editor: ''
ms.assetid: 958244a5-f9f5-455e-b7ef-71a65558872e
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/1/2016
ms.author: alkohli
ms.openlocfilehash: bb6bb491ca71e5ced5aecc8137e9e1cbd950e80b
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62123814"
---
# <a name="use-the-storsimple-device-manager-service-to-administer-your-storsimple-virtual-array"></a>StorSimple Virtual Array'iniz yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanın
![Kurulum işlem akışı](./media/storsimple-virtual-array-manager-service-administration/manage4.png)

## <a name="overview"></a>Genel Bakış
Bu makalede ve çeşitli seçenekler kullanılabilir bağlanma dahil olmak üzere StorSimple cihaz Yöneticisi hizmeti arabirimi açıklar ve bu kullanıcı Arabirimi gerçekleştirilen belirli iş akışları için bağlantılar sağlar.

Bu makaleyi okuduktan sonra şunları öğrenmiş olacaksınız nasıl yapılır:

* StorSimple cihaz Yöneticisi hizmetine bağlanın
* StorSimple cihaz Yöneticisi kullanıcı Arabirimine gidin
* StorSimple sanal Diziniz StorSimple cihaz Yöneticisi hizmeti aracılığıyla yönetme

> [!NOTE]
> StorSimple 8000 serisi cihaz için yönetim seçenekleri görüntülemek için Git [StorSimple Cihazınızı yönetmek için StorSimple Yöneticisi hizmetini kullanma](storsimple-manager-service-administration.md).
> 
> 

## <a name="connect-to-the-storsimple-device-manager-service"></a>StorSimple cihaz Yöneticisi hizmetine bağlanın
StorSimple cihaz Yöneticisi hizmeti, Microsoft Azure'da çalışan ve birden çok StorSimple sanal dizisi bağlanır. Bu cihazları yönetmek için bir tarayıcıda çalışan merkezi bir Microsoft Azure portal'ı kullanın. StorSimple cihaz Yöneticisi hizmetine bağlanmak için aşağıdakileri yapın.

#### <a name="to-connect-to-the-service"></a>Hizmetine bağlanmak için
1. [https://ms.portal.azure.com](https://ms.portal.azure.com) kısmına gidin.
2. Microsoft hesabı kimlik bilgilerinizi kullanarak, (üst sağ bölmenin bulunur) Microsoft Azure Portal'da oturum açın.
3. Gitmek için Gözat 'Filtre' belirli bir Abonelikteki tüm cihaz yöneticilerini görüntülemek için StorSimple cihaz yöneticileri üzerinde-->.

## <a name="use-the-storsimple-device-manager-service-to-perform-management-tasks"></a>Yönetim görevlerini gerçekleştirmek için StorSimple cihaz Yöneticisi hizmetini kullanma
Aşağıdaki tabloda, genel yönetim görevleri ve karmaşık iş akışları StorSimple cihaz Yöneticisi hizmeti Özet dikey pencerede gerçekleştirilebilecek bir özetini gösterir. Bu görevler, bunlar üzerinde başlatılan dikey pencereleri göre düzenlenir.

Her bir iş akışı hakkında daha fazla bilgi için tablodaki ilgili yordamı tıklayın.

#### <a name="storsimple-device-manager-workflows"></a>StorSimple cihaz Yöneticisi iş akışları
| Bunu yapmak istiyorsanız... | Bu yordamı kullanın |
| --- | --- |
| Hizmet oluşturma</br>Bir hizmeti Sil</br>Hizmet kayıt anahtarı alma</br>Hizmet kayıt anahtarını yeniden oluştur |[StorSimple cihaz Yöneticisi hizmetini dağıtma](storsimple-virtual-array-manage-service.md) |
| Etkinlik günlüklerini görüntüleme |[StorSimple hizmet özetini kullanma](storsimple-virtual-array-service-summary.md) |
| Bir sanal dizin devre dışı bırak</br>Sanal dizi Sil |[Sanal dizi silin veya devre dışı bırakma](storsimple-virtual-array-deactivate-and-delete-device.md) |
| Olağanüstü durum kurtarma ve cihaz yük devretme</br>Yük devretme önkoşulları</br>İş sürekliliği, olağanüstü durum kurtarma (BCDR)</br>Olağanüstü durum kurtarma sırasında hatalar |[Olağanüstü durum kurtarma ve cihaz yük devretme StorSimple Virtual Array'iniz için](storsimple-virtual-array-failover-dr.md) |
| Paylaşımları ve birimleri yedekleme</br>El ile yedekleyin</br>Yedekleme zamanlamasını değiştirme</br>Var olan yedekleri görüntüleme |[StorSimple Virtual Array'iniz yedekleme](storsimple-virtual-array-backup.md) |
| Bir yedekleme kümesinden kopya paylaşımları</br>Bir yedekleme kümesi birimlerden kopyalama</br>Öğe düzeyinde Kurtarma (yalnızca dosya sunucusu) |[StorSimple Virtual Array'iniz yedekten kopyalama](storsimple-virtual-array-clone.md) |
| Depolama hesapları hakkında</br>Bir depolama hesabı ekleme</br>Bir depolama hesabı Düzenle</br>Bir depolama hesabını silme |[StorSimple sanal dizisi için depolama hesaplarını yönetme](storsimple-virtual-array-manage-storage-accounts.md) |
| Erişim denetimi kayıtları hakkında</br>Ekleme veya erişim denetimi kaydı değiştirme </br>Bir erişim denetim kaydını silme |[StorSimple sanal dizisi için erişim denetimi kayıtları Yönet](storsimple-virtual-array-manage-acrs.md) |
| İş ayrıntılarını görüntüleme |[StorSimple sanal dizisi işlerini yönetme](storsimple-virtual-array-manage-jobs.md) |
| Uyarı ayarlarını yapılandır</br>Uyarı bildirimleri alma</br>Uyarıları yönetme</br>Uyarıları gözden geçirin |[Görüntüleme ve StorSimple sanal dizisi için uyarıları yönetme](storsimple-virtual-array-manage-alerts.md) |
| Cihaz Yöneticisi parolasını değiştirme |[Sanal diziyi StorSimple cihaz Yöneticisi parolasını değiştirme](storsimple-virtual-array-change-device-admin-password.md) |
| Yazılım güncelleştirmelerini yükle |[Sanal Diziniz güncelleştir](storsimple-virtual-array-install-update.md) |

> [!NOTE]
> Kullanmalısınız [yerel web kullanıcı Arabirimi](storsimple-ova-web-ui-admin.md) aşağıdaki görevler için:
> 
> * [Hizmet veri şifreleme anahtarı alma](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key)
> * [Destek paketi oluşturma](storsimple-ova-web-ui-admin.md#generate-a-log-package)
> * [Durdur ve bir sanal dizin yeniden başlatın](storsimple-ova-web-ui-admin.md#shut-down-and-restart-your-device)
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Web kullanıcı Arabirimi hakkında daha fazla bilgi ve nasıl kullanılacağını için Git [StorSimple Virtual Array'iniz yönetmek için StorSimple web kullanıcı arabirimini kullanın](storsimple-ova-web-ui-admin.md).

