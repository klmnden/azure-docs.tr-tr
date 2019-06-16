---
title: Depolama hesapları, abonelikleri için StorSimple cihaz Yöneticisi hizmetine geçirme | Microsoft Docs
description: Abonelik, depolama hesapları için StorSimple cihaz Yöneticisi service8000 geçirmeyi öğrenin.
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
ms.date: 03/14/2019
ms.author: alkohli
ms.openlocfilehash: 3cce18fa1890fc9e518294e294cc43e0e55065aa
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60631544"
---
# <a name="migrate-subscriptions-and-storage-accounts-associated-with-storsimple-device-manager-service"></a>Abonelikler ve StorSimple cihaz Yöneticisi hizmeti ile ilişkilendirilen depolama hesapları geçirme

Yeni bir kayıt veya yeni bir abonelik için StorSimple hizmetinize taşımanız gerekebilir. Bu geçiş senaryoları hesap değişikliklerini veya veri merkezi değişiklikleri var. Bu senaryolar hangisinin taşımak için ayrıntılı adımlar da dahil olmak üzere desteklenen anlamak için aşağıdaki tabloyu kullanın.

## <a name="account-changes"></a>Hesap değişiklikleri

| Taşıyabilirsiniz...| Desteklenen| Kapalı kalma süresi| Azure destek sürecini| Yaklaşım|
|-----|-----|-----|-----|-----|
| (StorSimple hizmeti ve depolama hesapları dahil) tüm bir abonelik başka bir kayıt için? | Evet       | Hayır       | **Kayıt aktarımı**<br>Kullanım:<li>Ne zaman yeni bir sözleşme altında yeni bir Azure taahhüt satın alın.</li><li>Tüm hesaplar ve abonelikler eski kayıt yeni geçirmek istediğiniz. Bu, eski abonelik altındaki tüm Azure hizmetleri içerir.</li> | **1. adım: Azure Kurumsal işlem destek bileti açın.**<li>[https://aka.ms/AzureEntSupport](https://aka.ms/AzureEntSupport) kısmına gidin.</li><li> Seçin **kayıt yönetimi** seçip **tek kayıt için yeni bir kayıt aktarım**.<br>**2. adım: İstenen bilgileri sağlayın**<br>Şunları içerir:<li>Kaynak kayıt numarası</li><li> hedef kayıt numarası</li><li>Aktarım geçerlilik tarihi|
| Var olan bir hesaptan StorSimple hizmeti için yeni bir kayıt?    | Evet       | Hayır       | **Hesap aktarımı**<br>Kullanım:<li>Ne zaman bir tam kayıt aktarımı istemezsiniz.</li><li>Yalnızca belirli hesap için yeni bir kayıt taşımak istediğiniz.</li>| **1. adım: Azure Kurumsal işlem destek bileti açın.**<li>[https://aka.ms/AzureEntSupport](https://aka.ms/AzureEntSupport) kısmına gidin.</li><li>Seçin **kayıt yönetimi** seçip **bir EA hesap için yeni bir kayıt aktarım**.<br>**2. adım: İstenen bilgileri sağlayın**<br>Şunları içerir:<li>Kaynak kayıt numarası</li><li> hedef kayıt numarası</li><li>Aktarım geçerlilik tarihi|
| Başka bir aboneliğe StorSimple hizmeti bir abonelikten?      | Hayır        |    Evet         | None, el ile işlemi|<li>StorSimple cihazını devre dışı verileri geçirin.</li><li>Cihazın Fabrika sıfırlaması yapmasını, bu cihazdaki tüm yerel veriler silinir.</li><li>Yeni Abonelik için StorSimple cihaz Yöneticisi hizmeti ile cihazı kaydedin.</li><li>Verileri cihaza geri geçirin.|
|Başka bir dizine Azure aboneliği sahipliğini aktarabilir miyim? | Evet       | Hayır       | Azure AD dizininize mevcut bir aboneliğe ilişkilendirin | Başvuru [Azure AD dizininize mevcut bir aboneliğe ilişkilendirilecek](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md). Her şeyin doğru şekilde gösterilmesi 10 dakikaya kadar sürebilir.|
| StorSimple cihaz Yöneticisi hizmeti bir StorSimple cihazından başka bir hizmete farklı bir bölgede?      | Hayır        | Evet            | None, el ile işlemi |Yukarıdakiyle aynı.|
| Yeni bir abonelik veya kaynak grubu için depolama hesabı?     | Evet        | Hayır             |Depolama hesabı farklı abonelik veya kaynak grubuna taşıma |Depolama hesabı erişim anahtarlarını güncelleştirdiyseniz, taşımadan sonra kullanıcı StorSimple cihaz Yöneticisi hizmeti aracılığıyla geçirilen depolama hesabı erişim anahtarlarını el ile yapılandırmanız gerekecektir.|
| Klasik depolama hesabı bir Azure Resource Manager depolama hesabına      | Evet        | Hayır             |Klasik modelden Azure Resource Manager'a geçiş |<li>Bir depolama hesabı Klasik modelden Azure Resource Manager'a geçirme konusunda ayrıntılı yönergeler için Git [Klasik depolama hesabını geçirme](../virtual-machines/windows/migration-classic-resource-manager-ps.md#step-62-migrate-a-storage-account).</li><li> Depolama hesabı erişim anahtarlarını geçişten sonra güncelleştirdiyseniz, kullanıcı StorSimple cihaz Yöneticisi hizmeti aracılığıyla geçirilen depolama hesabı için erişim anahtarları eşitlemeniz gerekir. Bu StorSimple cihazını normal şekilde çalışmaya devam eder ve birincil/yedekleme verileri azure'a katmanı mümkün emin olmaktır. Erişim anahtarları eşitleniyor ayrıntılı yönergeler için Git [döndürme iş akışı](storsimple-8000-manage-storage-accounts.md#key-rotation-of-storage-accounts).</li><li> StorSimple Cloud Appliance söz konusu olduğunda, Klasik depolama hesabı geçirilir, ancak temel sanal makine hala Klasik modelde kalır gereç düzgün çalışması. Bulut Gereci için temel sanal makine geçirdiyseniz, devre dışı bırakma ve silme işlevleri çalışmaz.</li><li> Azure portalında yeni bir StorSimple Cloud Appliance'lar oluşturmanız ve ardından eski bulut gereçlerini yük devretme gerekir. Klasik depolama hesabı kullanarak yeni Azure portalında StorSimple Cloud Appliance oluşturulamıyor, bir Azure Resource Manager depolama hesabınızın olması gerekir. Daha fazla bilgi için Git [Dağıt ve StorSimple bulut Gereci yönetme](storsimple-8000-cloud-appliance-u2.md).</li>|

## <a name="datacenter-changes"></a>Veri Merkezi değişiklikleri

| Taşıyabilirsiniz...| Desteklenen|Kapalı kalma süresi| Azure destek sürecini| Yaklaşım|
|-----|-----|-----|-----|-----|
| Bir Azure veri merkezi StorSimple hizmetinden diğerine? | Hayır | Evet |None, el ile işlemi  |<li>StorSimple cihazını devre dışı verileri geçirin.</li><li>Cihazın Fabrika sıfırlaması yapmasını, bu cihazdaki tüm yerel veriler silinir.</li><li>Cihazı yeni bir abonelik yeni bir StorSimple cihaz Yöneticisi hizmetine kaydedin.</li><li>Verileri cihaza geri geçirin.|
| Bir Azure veri merkezi depolama hesabından diğerine? | Hayır |Evet  |None, el ile işlemi  | Yukarıdakiyle aynı.|

## <a name="next-steps"></a>Sonraki adımlar

* [StorSimple cihaz Yöneticisi hizmetini dağıtma](storsimple-8000-manage-service.md)
* [Azure portalında StorSimple 8000 serisi Cihazınızı dağıtma](storsimple-8000-deployment-walkthrough-u2.md)
