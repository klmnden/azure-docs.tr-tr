---
title: "StorSimple 8000 serisi güncelleştirme 5 sürüm notları | Microsoft Docs"
description: "StorSimple 8000 serisi güncelleştirme 5 için yeni özellikler, sorunlar ve geçici çözümleri açıklar."
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/13/2017
ms.author: alkohli
ms.openlocfilehash: 672757e82bcf645b705f46a9975e09c9dc5eef92
ms.sourcegitcommit: 659cc0ace5d3b996e7e8608cfa4991dcac3ea129
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2017
---
# <a name="storsimple-8000-series-update-5-release-notes"></a>StorSimple 8000 serisi güncelleştirme 5 sürüm notları

## <a name="overview"></a>Genel Bakış

Aşağıdaki sürüm notları, yeni özellikleri açıklar ve StorSimple 8000 serisi güncelleştirme 5 için kritik açık sorunlar tanımlayın. Ayrıca bu sürümde dahil StorSimple yazılım güncelleştirmelerinin bir listesini içerir.

Güncelleştirme 5 güncelleştirme 4 ile güncelleştirme 0.1 çalıştıran herhangi bir StorSimple aygıtı uygulanabilir. Güncelleştirme 5 ile ilişkili aygıt 6.3.9600.17845 sürümüdür.

StorSimple çözümünüzde güncelleştirme dağıtmadan önce sürüm notları'nda yer alan bilgileri gözden geçirin.

> [!IMPORTANT]
> * Güncelleştirme 5 zorunlu bir güncelleştirme ve hemen yüklü olması gerekir. Daha fazla bilgi için bkz: nasıl yapılır [uygulamak güncelleştirme 5](storsimple-8000-install-update-5.md).
> * Güncelleştirme 5 aygıt yazılımı, disk bellenim, işletim sistemi güvenlik ve diğer işletim sistemi güncelleştirmelerini sahiptir. Bu güncelleştirmeyi yüklemek için yaklaşık 4 saat sürer. Disk bellenim güncelleştirme kesintiye uğratan güncelleştirme ve cihazınız için bir kapalı kalma süresi ile sonuçlanır. Cihazınız güncel tutmak için güncelleştirme 5 uygulamanızı öneririz.
> * Yeni sürümler için güncelleştirmelerinin göremeyebilirsiniz hemen biz aşamalı güncelleştirmeler yapmak için. Birkaç gün bekleyin ve ardından güncelleştirmeleri taramak üzere bu güncelleştirmeleri yeniden olarak yakında kullanılabilir hale gelecektir.

## <a name="whats-new-in-update-5"></a>Güncelleştirme 5'teki yenilikler

Güncelleştirme 5'te, aşağıdaki anahtar geliştirmeler ve hata düzeltmeleri yapıldı.

* **Kullanım, Azure Active Directory (StorSimple cihaz Yöneticisi hizmeti ile kimlik doğrulaması için AAD)** – StorSimple cihaz Yöneticisi hizmeti ile kimlik doğrulaması için gelen güncelleştirme 5'ten başlayarak, Azure Active Directory kullanılır. Eski kimlik doğrulama mekanizması aralık 2017 kullanım dışı kalacaktır. Tüm kullanıcıların yeni kimlik doğrulaması URL'lerini kendi güvenlik duvarı kuralları eklemeniz gerekir. Daha fazla bilgi için Git [kimlik doğrulaması URL'lerini listelenen StorSimple cihazınız için ağ gereksinimleri](storsimple-8000-system-requirements.md#url-patterns-for-azure-portal).

    Kimlik doğrulaması URL'sini güvenlik duvarı kurallarında dahil edilmezse, kullanıcılar, StorSimple cihazını hizmetiyle kimlik doğrulaması yapamıyor kritik bir uyarı görürsünüz. Kullanıcılar bu uyarıyı görürseniz, yeni kimlik doğrulaması URL'sini eklemeniz gerekir. Daha fazla bilgi için Git [uyarıları ağ StorSimple](storsimple-8000-manage-alerts.md#networking-alerts).

* **StorSimple anlık görüntü Yöneticisi'nin yeni sürümü** -StorSimple anlık görüntü Yöneticisi'nin yeni bir sürümü güncelleştirme 5 ile serbest ve güncelleştirme 4 çalıştıran tüm StorSimple cihazlar uyumlu ya da daha yeni. Bu sürüme güncelleştirmenizi öneririz. StorSimple anlık görüntü Yöneticisi'nin önceki sürümü güncelleştirme 3'ü çalıştıran StorSimple cihazlar için kullanılan veya önceki bir sürümü. [StorSimple anlık görüntü Yöneticisi'nin uygun sürümü karşıdan](https://www.microsoft.com/en-us/download/details.aspx?id=44220) ve başvurulacak [StorSimple Snapshot Manager dağıtmak](storsimple-snapshot-manager-deployment.md).


## <a name="issues-fixed-in-update-5"></a>Güncelleştirme 5'te giderilen sorunlar

Aşağıdaki tabloda güncelleştirme 5'te düzeltilen sorunlardan özetini sağlar.

| Hayır | Özellik | Sorun | Fiziksel cihaz için geçerlidir | Sanal cihaz için geçerlidir |
| --- | --- | --- | --- | --- |
| 1 |Windows PowerShell uzaktan iletişim |Önceki sürümde, bir kullanıcı Windows PowerShell aracılığıyla StorSimple bulut uygulaması uzak bir bağlantı kurmaya çalışırken bir hata alırsınız. Bu sorun kök neden oldu ve bu sürümde sabit. |Hayır |Evet |
| 2 |Bant genişliği şablonları |Önceki sürümü, cihaz için yapılandırılan daha düşük bant genişliği ile sonuçlandı bant genişliği şablonları ile ilgili bir sorun oluştu. Bu sorun, bu sürümde çözümlenir. |Evet |Evet |
| 3 |Yük devretme |Çok sayıda birimleri sahip bir aygıt üzerinden güncelleştirme 4 çalıştıran başka bir cihaza başarısız oldu, erişim denetimi kayıtları uygulanmaya çalışılırken önceki sürümde işlemi başarısız olur. Bu sürümde bu sorun düzeltilmiştir. |Evet |Evet |



## <a name="known-issues-in-update-5-from-previous-releases"></a>Önceki sürümlerden güncelleştirme 5'te bilinen sorunlar

Güncelleştirme 5'te yeni bilinen bir sorun vardır. Önceki sürümlerden güncelleştirme 5'e taşınır sorunların listesi için Git [güncelleştirme 3 Sürüm Notları](storsimple-update3-release-notes.md#known-issues-in-update-3).

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-update-5"></a>Seri Bağlı SCSI (SAS) denetleyicisi ve bellenim güncelleştirmeleri güncelleştirme 5

Bu sürüm, SAS denetleyicisi ve LSI sürücü ve bellenim güncelleştirmeleri vardır. Bu güncelleştirmeleri yükleme hakkında daha fazla bilgi için bkz: [güncelleştirme 5 yükleme](storsimple-8000-install-update-5.md) , StorSimple Cihazınızda.

## <a name="storsimple-cloud-appliance-updates-in-update-5"></a>Güncelleştirme 5 StorSimple bulut uygulaması güncelleştirmeleri

Bu güncelleştirme, StorSimple bulut uygulaması (sanal aygıt olarak da bilinir) uygulanamaz. Yeni bulut cihazları güncelleştirme 5 görüntü kullanılarak oluşturulması gerekir. Bir StorSimple bulut uygulaması oluşturma hakkında daha fazla bilgi için Git [dağıtma ve StorSimple bulut uygulaması yönetmek](storsimple-8000-cloud-appliance-u2.md).

## <a name="next-step"></a>Sonraki adım

Bilgi nasıl [güncelleştirme 5 yükleme](storsimple-8000-install-update-5.md) , StorSimple Cihazınızda.

