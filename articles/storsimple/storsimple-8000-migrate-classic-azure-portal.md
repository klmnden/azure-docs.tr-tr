---
title: Depolama hesapları, StorSimple cihaz Yöneticisi hizmetiniz için abonelik geçirme | Microsoft Docs
description: Abonelik, depolama hesapları StorSimple Aygıt Yöneticisi'ni service8000 için geçiş öğrenin.
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
ms.date: 05/21/2018
ms.author: alkohli
ms.openlocfilehash: bbcb598d075868a6c6aab27d108e91afa90b62b1
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34651092"
---
# <a name="migrate-subscriptions-and-storage-accounts-associated-with-storsimple-device-manager-service"></a>Abonelikler ve StorSimple cihaz Yöneticisi hizmeti ile ilişkili depolama hesapları geçirme

Yeni bir kayıt veya yeni bir abonelik için StorSimple hizmetinize taşımanız gerekebilir. Bu geçiş senaryoları hesap değişiklikleri veya datacenter değişiklikler var. Bu senaryoların taşımak için ayrıntılı adımlar da dahil olmak üzere desteklenen anlamak için aşağıdaki tabloyu kullanın.

## <a name="account-changes"></a>Hesap değişiklikleri

| Taşıyabilirsiniz...| Desteklenen| Çalışmama süresi| Azure destek işlemi| Yaklaşım|
|-----|-----|-----|-----|-----|
| (StorSimple hizmeti ve depolama hesapları içerir) bir tüm abonelik başka bir kayıt için? | Evet       | Hayır       | **Kayıt aktarımı**<br>Kullanım:<li>Ne zaman yeni bir anlaşma altında yeni bir Azure taahhüt satın alın.</li><li>Tüm hesaplar ve abonelikler eski kayıt işlemini yeni geçirmek istediğiniz. Bu, eski abonelik altında tüm Azure hizmetlerini içerir.</li> | **1. adım: Azure Enterprise işlemi destek bileti açın.**<li>[http://aka.ms/AzureEnt](http://aka.ms/AzureEnt) kısmına gidin.</li><li> Seçin **kayıt Yönetim** ve ardından **yeni bir kayıt için bir kayıt işlemini aktarım**.<br>**2. adım: İstenen bilgileri sağlayın**<br>Şunları içerir:<li>Kaynak kayıt numarası</li><li> hedef kayıt numarası</li><li>Aktarım geçerlilik tarihi|
| Yeni bir kayıt için StorSimple hizmeti var olan bir hesaptan?    | Evet       | Hayır       | **Hesap aktarımı**<br>Kullanım:<li>Ne zaman bir tam kayıt aktarımı istemezsiniz.</li><li>Yalnızca belirli hesapları için yeni bir kayıt geçmek istiyorsunuz.</li>| **1. adım: Azure Enterprise işlemi destek bileti açın.**<li>[http://aka.ms/AzureEnt](http://aka.ms/AzureEnt) kısmına gidin.</li><li>Seçin **kayıt Yönetim** ve ardından **EA hesabınız için yeni bir kayıt aktarım**.<br>**2. adım: İstenen bilgileri sağlayın**<br>Şunları içerir:<li>Kaynak kayıt numarası</li><li> hedef kayıt numarası</li><li>Aktarım geçerlilik tarihi|
| Başka bir abonelik için StorSimple hizmeti bir aboneliğe ilişkin?      | Hayır        |    Evet         | None, el ile işlem|<li>StorSimple cihazı verileri geçirin.</li><li>Bir cihazı fabrika ayarlarına gerçekleştirmek, bu cihazdaki yerel tüm veriler silinir.</li><li>Bir StorSimple cihaz Yöneticisi hizmeti için yeni abonelik ile cihazı kaydedin.</li><li>Verileri geri cihaza geçirin.|
  |Bir Azure aboneliği sahipliğini başka bir dizine aktarabilir miyim? | Evet       | Hayır       | Azure AD dizininizi mevcut bir aboneliğe ilişkilendirme | Başvuru [Azure AD dizininizi mevcut bir aboneliğe ilişkilendirilecek](../active-directory/active-directory-how-subscriptions-associated-directory.md). Her şeyin doğru şekilde gösterilmesi 10 dakikaya kadar sürebilir.|
| Farklı bir bölgede başka bir hizmete StorSimple cihazı bir StorSimple Aygıt Yöneticisi'ni hizmetinden?      | Hayır        | Evet            | None, el ile işlem |Yukarıdaki ile aynı.|
| Yeni bir abonelik veya kaynak grubu için depolama hesabı?     | Evet        | Hayır             |Depolama hesabı farklı abonelik veya kaynak grubuna taşıma |Depolama hesabı erişim anahtarlarını güncelleştirdiyseniz taşımadan sonra kullanıcı el ile StorSimple cihaz Yöneticisi hizmeti üzerinden geçirilen depolama hesabı için erişim anahtarları yapılandırmanız gerekecektir.|
| Klasik depolama hesabı bir Azure Resource Manager depolama hesabı      | Evet        | Hayır             |Klasikten Azure Resource Manager geçirme |<li>Bir depolama hesabı Klasikten Azure Resource Manager geçirme hakkında ayrıntılı yönergeler için Git [Klasik depolama hesabını geçirme](../virtual-machines/windows/migration-classic-resource-manager-ps.md#step-62-migrate-a-storage-account).</li><li> Depolama hesabı erişim anahtarlarını geçişten sonra güncelleştirilmişse, kullanıcının StorSimple cihaz Yöneticisi hizmeti üzerinden geçirilen depolama hesabı için erişim anahtarları eşitlemek gerekir. StorSimple cihazlarını normal şekilde çalışmaya devam eder ve birincil/yedekleme verilerini Azure katmanı mümkün olmasını budur. Erişim tuşları eşitleme ayrıntılı yönergeler için Git [döndürme iş akışı](storsimple-8000-manage-storage-accounts.md#key-rotation-of-storage-accounts).</li><li> StorSimple bulut uygulaması söz konusu olduğunda, Klasik depolama hesabı geçirilir, ancak temel alınan sanal makine hala Klasik içinde kalır Gereci düzgün çalışması. Temel alınan sanal makine bulut uygulaması için geçirdiyseniz, devre dışı bırakma ve silme işlevleri çalışmaz.</li><li> Azure portalında yeni bir StorSimple bulut uygulamaları oluşturmak ve ardından eski bulut gereçlerini yük devri gerekir. Klasik depolama hesabı kullanarak yeni Azure portalında bir StorSimple bulut uygulaması oluşturulamıyor, bir Azure Resource Manager depolama hesabınızın olması gerekir. Daha fazla bilgi için Git [dağıtma ve StorSimple bulut uygulaması yönetmek](storsimple-8000-cloud-appliance-u2.md).</li>|Yukarıdaki ile aynı.|

## <a name="datacenter-changes"></a>Veri Merkezi değişiklikleri

| Taşıyabilirsiniz...| Desteklenen|Çalışmama süresi| Azure destek işlemi| Yaklaşım|
|-----|-----|-----|-----|-----|
| Bir Azure veri merkezi StorSimple hizmetinden diğerine? | Hayır | Evet |None, el ile işlem  |<li>StorSimple cihazı verileri geçirin.</li><li>Bir cihazı fabrika ayarlarına gerçekleştirmek, bu cihazdaki yerel tüm veriler silinir.</li><li>Yeni bir StorSimple cihaz Yöneticisi hizmeti için yeni abonelik ile cihazı kaydedin.</li><li>Verileri geri cihaza geçirin.|
| Bir Azure veri merkezi depolama hesabından başka bir? | Hayır |Evet  |None, el ile işlem  | Yukarıdaki ile aynı.|

## <a name="next-steps"></a>Sonraki adımlar

* [StorSimple cihaz Yöneticisi hizmetini dağıtma](storsimple-8000-manage-service.md)
* [Azure portalında StorSimple 8000 serisi Cihazınızı dağıtma](storsimple-8000-deployment-walkthrough-u2.md)
