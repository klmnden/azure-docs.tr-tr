---
title: StorSimple 8000 serisi güncelleştirme 5 sürüm notları | Microsoft Docs
description: StorSimple 8000 serisi güncelleştirme 5 için yeni özellikler, sorunlar ve geçici çözümleri açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/13/2017
ms.author: alkohli
ms.openlocfilehash: 462cbd6261723aa91bbfd23292611e758a800ed2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60844100"
---
# <a name="storsimple-8000-series-update-5-release-notes"></a>StorSimple 8000 serisi güncelleştirme 5 sürüm notları

## <a name="overview"></a>Genel Bakış

Aşağıdaki sürüm notları, yeni özellikleri açıklar ve StorSimple 8000 serisi güncelleştirme 5 için kritik açık sorunları tanımlayın. Ayrıca bu sürüme dahil edilen StorSimple yazılım güncelleştirmeleri listesini içerirler.

Güncelleştirme 5, güncelleştirme 4 ile güncelleştirme 0.1 çalıştıran tüm StorSimple cihazı için uygulanabilir. Güncelleştirme 5 ile ilişkili cihaz 6.3.9600.17845 sürümüdür.

StorSimple çözümünüzde güncelleştirme dağıtmadan önce bu sürüm notlarında yer alan bilgileri gözden geçirin.

> [!IMPORTANT]
> * Güncelleştirme 5 zorunlu bir güncelleştirme ve hemen yüklenmesi gerekir. Daha fazla bilgi için bkz. nasıl [güncelleştirme 5'i Uygula](storsimple-8000-install-update-5.md).
> * Güncelleştirme 5 cihaz yazılımı, disk üretici yazılımı, işletim sistemi güvenlik ve diğer işletim sistemi güncelleştirmeleri var. Bu güncelleştirmeyi yüklemek için yaklaşık 4 saat sürer. Disk üretici yazılımı güncelleştirmesi, kesintiye uğratan bir güncelleştirmedir ve cihazınız için bir kapalı kalma süresi ile sonuçlanır. Cihazınızı güncel tutmak için güncelleştirme 5 uygulamanızı öneririz.
> * Yeni yayınlar için güncelleştirmeleri göremeyebilirsiniz hemen güncelleştirmeleri aşamalı olarak sunulmasını çünkü. Birkaç gün bekleyin ve sonra bu güncelleştirmeler yeniden Güncelleştirmeleri tara yakında kullanıma sunulacaktır.

## <a name="whats-new-in-update-5"></a>Güncelleştirme 5'teki yenilikler

Güncelleştirme 5'te aşağıdaki anahtar geliştirmeleri ve hata düzeltmeleri yapıldı.

* **Kullanım, Azure Active Directory (StorSimple cihaz Yöneticisi hizmeti ile kimlik doğrulaması için AAD)** – StorSimple cihaz Yöneticisi hizmeti ile kimlik doğrulaması için gelen güncelleştirme 5'ten başlayarak, Azure Active Directory kullanılır. Eski kimlik doğrulama mekanizması aralık 2017 tarihinden itibaren kullanımdan kaldırılacaktır. Tüm kullanıcılar, kendi güvenlik duvarı kurallarında yeni kimlik doğrulaması URL'lerini içermelidir. Daha fazla bilgi için Git [kimlik doğrulaması URL'lerini listelenen StorSimple cihazınız için ağ gereksinimleri](storsimple-8000-system-requirements.md#url-patterns-for-azure-portal).

    Güvenlik duvarı kurallarında kimlik doğrulama URL'si dahil edilmemişse, kullanıcılar, StorSimple cihazını hizmeti ile doğrulanamadı kritik bir uyarı görürsünüz. Kullanıcılar bu uyarıyı görürseniz, bunlar yeni kimlik doğrulama URL'si eklemeniz gerekir. Daha fazla bilgi için Git [uyarılar ağ StorSimple](storsimple-8000-manage-alerts.md#networking-alerts).

* **Yeni sürüm, StorSimple Snapshot Manager** -StorSimple Snapshot Manager'ın yeni bir sürümü güncelleştirme 5 ile yayımlanan ve Update 4 çalıştıran tüm StorSimple cihazlar uyumlu veya üzeri. Bu sürüme güncelleştirmenizi öneririz. StorSimple Snapshot Manager'ın önceki sürümü önceki veya güncelleştirme 3'ü çalıştıran StorSimple cihazlar için kullanılabilir. [StorSimple Snapshot Manager'ın uygun sürümü indirme](https://www.microsoft.com/en-us/download/details.aspx?id=44220) başvurun [StorSimple Snapshot Manager'ı Dağıtma](storsimple-snapshot-manager-deployment.md).


## <a name="issues-fixed-in-update-5"></a>Güncelleştirme 5'te giderilen sorunlar

Aşağıdaki tabloda, güncelleştirme 5'te düzeltilen sorunlara ilişkin bir Özet sağlar.

| Hayır | Özellik | Sorun | Fiziksel cihaz için geçerlidir | Sanal cihaza uygulanır |
| --- | --- | --- | --- | --- |
| 1 |Windows PowerShell uzaktan iletişimi |Önceki sürümde, bir kullanıcı, StorSimple Cloud Appliance Windows PowerShell aracılığıyla bir uzak bağlantı kurmaya çalışırken bir hata alırsınız. Bu sorun, kök neden ve bu sürümde giderilen. |Hayır |Evet |
| 2 |Bant genişliği şablonları |Önceki sürümde, cihaz için yapılandırılan daha düşük bant genişliği sonuçlanan bant genişliği şablonları ile ilgili bir sorun oluştu. Bu sürümde bu sorun giderildi. |Evet |Evet |
| 3 |Yük devretme |Çok sayıda birimleri ile bir cihaz üzerinden güncelleştirme 4 ile birlikte çalışan başka bir cihaza başarısız oldu, erişim denetimi kayıtları uygulanmaya çalışılırken önceki bir sürümde işlemi başarısız olur. Bu sürümde bu sorun düzeltilmiştir. |Evet |Evet |



## <a name="known-issues-in-update-5-from-previous-releases"></a>Önceki sürümlerine yönelik güncelleştirme 5'te bilinen sorunlar

Güncelleştirme 5'te yeni bilinen bir sorun vardır. Önceki sürümlerden güncelleştirme 5'e taşınır sorunların listesi için Git [güncelleştirme 3 Sürüm Notları](storsimple-update3-release-notes.md#known-issues-in-update-3).

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-update-5"></a>Seri Bağlı SCSI (SAS) denetleyicisi ve üretici yazılımı güncelleştirmelerini güncelleştirme 5

Bu sürüm, SAS Denetleyici ve LSI sürücü ve bellenim güncelleştirmeleri sahiptir. Bu güncelleştirmeleri yükleme hakkında daha fazla bilgi için bkz. [güncelleştirme 5'i yükleme](storsimple-8000-install-update-5.md) StorSimple Cihazınızda.

## <a name="storsimple-cloud-appliance-updates-in-update-5"></a>Güncelleştirme 5'te StorSimple Cloud Appliance güncelleştirme

Bu güncelleştirme, StorSimple Cloud Appliance (sanal cihaz olarak da bilinir) uygulanamaz. Yeni bulut Gereçleri güncelleştirme 5 görüntüsünü kullanarak oluşturulması gerekir. StorSimple Cloud Appliance oluşturma hakkında daha fazla bilgi için Git [Dağıt ve StorSimple bulut Gereci yönetme](storsimple-8000-cloud-appliance-u2.md).

## <a name="next-step"></a>Sonraki adım

Bilgi edinmek için nasıl [güncelleştirme 5'i yükleyin](storsimple-8000-install-update-5.md) StorSimple Cihazınızda.

