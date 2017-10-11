---
title: "Microsoft Azure StorSimple Yöneticisi Sanal dizinin Yönetim | Microsoft Docs"
description: "Azure portalında StorSimple cihaz Yöneticisi hizmetini kullanarak StorSimple şirket içi sanal dizinizi yönetmeyi öğrenin."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 958244a5-f9f5-455e-b7ef-71a65558872e
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/1/2016
ms.author: alkohli
ms.openlocfilehash: a74a160eae88a2d03460a1346479c333d8f9d524
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-device-manager-service-to-administer-your-storsimple-virtual-array"></a>StorSimple sanal dizinizi yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma
![Kurulum işlem akışı](./media/storsimple-virtual-array-manager-service-administration/manage4.png)

## <a name="overview"></a>Genel Bakış
Bu makalede, onu ve çeşitli seçenekleri, bağlanmak nasıl dahil olmak üzere StorSimple cihaz Yöneticisi hizmet arabirimden açıklar ve bu kullanıcı Arabirimi gerçekleştirilebilir belirli iş akışları için bağlantılar sağlar.

Bu makaleyi okuduktan sonra şunları öğrenmiş olacaksınız nasıl yapılır:

* StorSimple cihaz Yöneticisi hizmetine bağlanmak
* StorSimple Aygıt Yöneticisi'ni UI gidin
* StorSimple sanal dizinizi StorSimple cihaz Yöneticisi hizmeti aracılığıyla yönetmek

> [!NOTE]
> StorSimple 8000 serisi cihaz için yönetim seçenekleri görüntülemek için şu adrese gidin [StorSimple Cihazınızı yönetmek için StorSimple Yöneticisi hizmetini kullanma](storsimple-manager-service-administration.md).
> 
> 

## <a name="connect-to-the-storsimple-device-manager-service"></a>StorSimple cihaz Yöneticisi hizmetine bağlanmak
StorSimple cihaz Yöneticisi hizmeti Microsoft Azure üzerinde çalışır ve birden çok StorSimple sanal diziler bağlanır. Bu cihazları yönetmek için bir tarayıcıda çalışan merkezi bir Microsoft Azure portalını kullanın. StorSimple cihaz Yöneticisi hizmetine bağlanmak için aşağıdakileri yapın.

#### <a name="to-connect-to-the-service"></a>Hizmete bağlanmak için
1. Git [https://ms.portal.azure.com](https://ms.portal.azure.com).
2. Microsoft hesabı kimlik bilgilerinizi kullanarak (sağ üst tarafında bölmesinin bulunur) Microsoft Azure portalında oturum açın.
3. Gitmek için Gözat 'Filtrele' belirli bir aboneliğe tüm cihaz yöneticileri görüntülemek üzere StorSimple cihaz yöneticileri üzerinde-->.

## <a name="use-the-storsimple-device-manager-service-to-perform-management-tasks"></a>Yönetim görevlerini gerçekleştirmek için StorSimple cihaz Yöneticisi hizmetini kullanma
Aşağıdaki tabloda genel yönetim görevleri ve StorSimple cihaz Yöneticisi hizmeti Özet Dikey içinde gerçekleştirilebilir karmaşık iş akışları bir özetini gösterir. Bu görevler üzerinde bunlar başlatılan Kanatlar göre düzenlenir.

Her bir iş akışı hakkında daha fazla bilgi için tablodaki uygun yordamı tıklatın.

#### <a name="storsimple-device-manager-workflows"></a>StorSimple cihaz Manager iş akışları
| Bunu yapmak istiyorsanız... | Bu yordamı kullanın |
| --- | --- | --- |
| Hizmet oluşturma</br>Bir hizmeti silin</br>Hizmet kayıt anahtarı alma</br>Hizmet kayıt anahtarını yeniden oluşturma |[StorSimple cihaz Yöneticisi hizmetini dağıtma](storsimple-virtual-array-manage-service.md) |
| Etkinlik günlükleri görüntüleme |[StorSimple hizmet özeti kullanın](storsimple-virtual-array-service-summary.md) |
| Sanal bir dizi devre dışı bırakma</br>Sanal bir dizi Sil |[Sanal bir dizi silmek veya devre dışı bırakma](storsimple-virtual-array-deactivate-and-delete-device.md) |
| Olağanüstü durum kurtarma ve aygıt yük devretme</br>Yük devretme önkoşulları</br>İş sürekliliği olağanüstü durum kurtarma (BCDR)</br>Olağanüstü durum kurtarma sırasında hatalar |[Olağanüstü durum kurtarma ve aygıt yük devretme, StorSimple sanal dizini](storsimple-virtual-array-failover-dr.md) |
| Paylaşımları ve birimler yedekleme</br>El ile yedekleyin</br>Yedekleme zamanlamasını değiştirme</br>Var olan yedekleri görüntüleyin |[StorSimple sanal dizinizi yedekleyin](storsimple-virtual-array-backup.md) |
| Bir yedekleme kümesinden kopya paylaşımları</br>Bir yedekleme kümesi birimlerden kopyalama</br>Öğe düzeyinde Kurtarma (yalnızca dosya sunucusu) |[StorSimple sanal dizinizi yedekten kopyalama](storsimple-virtual-array-clone.md) |
| Depolama hesapları hakkında</br>Depolama hesabı ekleme</br>Bir depolama hesabı Düzenle</br>Bir depolama hesabını silme |[StorSimple sanal dizi için depolama hesaplarını yönetme](storsimple-virtual-array-manage-storage-accounts.md) |
| Erişim denetimi kayıtları hakkında</br>Ekleyin veya bir erişim denetimi kaydı değiştirin </br>Bir erişim denetimi kaydını sil |[StorSimple sanal dizi için erişim denetim kayıtlarını yönetme](storsimple-virtual-array-manage-acrs.md) |
| İş ayrıntılarını görüntüleme |[StorSimple sanal dizinin işlerini yönetme](storsimple-virtual-array-manage-jobs.md) |
| Uyarı ayarlarını yapılandırma</br>Uyarı bildirimleri alma</br>Uyarıları yönetme</br>Uyarıları gözden geçirin |[Uyarıları görüntüleyin ve StorSimple sanal dizi için yönetin](storsimple-virtual-array-manage-alerts.md) |
| Cihaz Yöneticisi parolasını değiştirme |[StorSimple sanal dizinin cihaz Yöneticisi parolasını değiştirme](storsimple-virtual-array-change-device-admin-password.md) |
| Yazılım güncelleştirmelerini yükle |[Sanal dizinizi güncelleştir](storsimple-virtual-array-install-update.md) |

> [!NOTE]
> Kullanmalısınız [yerel web kullanıcı Arabirimi](storsimple-ova-web-ui-admin.md) aşağıdaki görevler için:
> 
> * [Hizmet verileri şifreleme anahtarı alma](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key)
> * [Bir destek paketi oluştur](storsimple-ova-web-ui-admin.md#generate-a-log-package)
> * [Durdurun ve sanal dizinin yeniden başlatın](storsimple-ova-web-ui-admin.md#shut-down-and-restart-your-device)
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Web kullanıcı Arabirimi hakkında bilgi ve nasıl kullanılacağını için Git [StorSimple sanal dizinizi yönetmek için StorSimple web kullanıcı arabirimini kullanın](storsimple-ova-web-ui-admin.md).

