---
title: Taşıma Klasikten Azure portalı SSS | Microsoft Docs
description: StorSimple cihazları Azure portalında Klasikten taşıma hakkında sık sorulan soruların yanıtlarını sağlar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/28/2017
ms.author: alkohli
ms.openlocfilehash: ac57894e4f180f42f80479d2031f4dd5ddfdb1be
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
ms.locfileid: "27785188"
---
# <a name="move-storsimple-device-manager-service-from-classic-to-azure-portal-frequently-asked-questions-faq"></a>StorSimple cihaz Yöneticisi hizmeti Azure portalında Klasikten taşıma: sık sorulan sorular (SSS)

## <a name="overview"></a>Genel Bakış

Aşağıdaki soruları ve yanıtları, Klasik Azure portalında Azure portalına çalışan StorSimple Aygıt Yöneticisi'ni hizmetinizi taşıdığınızda, yaptığınız geçerlidir.

Sorular ve yanıtlar Aşağıdaki kategorilerde düzenlenir:

* StorSimple cihaz Yöneticisi hizmeti taşıma
* Depolama hesapları taşıma
* Cmdlet tabanlı Azure Resource Manager kullanarak
* StorSimple veri Yöneticisi hizmetini kullanma
* Muhtelif Hükümler

## <a name="moving-storsimple-device-manager-service"></a>StorSimple cihaz Yöneticisi hizmeti taşıma

### <a name="once-i-have-moved-to-azure-portal-can-i-still-create-a-storsimple-manager-service-in-the-classic-portal"></a>Bir kez ı taşınmış Azure portalına Klasik portalda hala bir StorSimple Yöneticisi hizmeti oluşturabilir mi?

Hayır. Azure portalında, StorSimple Yöneticisi hizmetiniz geçirdikten sonra Klasik portalda yeni bir hizmet oluşturulamıyor. Ayrıca, [Klasik Portalı'nı 8 Ocak 2018 kullanılamaz](https://azure.microsoft.com/updates/azure-portal-updates-for-classic-portal-users). 

### <a name="i-have-multiple-storsimple-managers-running-in-the-classic-portal-can-i-choose-which-ones-to-move-to-the-azure-portal"></a>Klasik portalda çalıştıran birden çok StorSimple yöneticileri var. Azure portalına taşımaya hangilerinin seçebilir miyim?

Hayır. Azure portalına taşımak için hangi StorSimple yöneticileri seçemezsiniz. Aboneliğinizdeki tüm StorSimple yöneticileri taşınır.


### <a name="i-initiated-the-migration-of-my-service-to-the-new-azure-portal-should-i-delete-the-storsimple-manager-from-the-classic-portal"></a>I Hizmetim yeni Azure portalına geçişi başlatıldı. StorSimple Yöneticisi Klasik portalından silmeniz gerekir? 

Hayır. Klasik Portalı'ndan taşınmış herhangi bir hizmeti silmek çalışmayın. Geçişin tamamlanmasını bekleyin, tüm StorSimple yöneticileri yeni portalına taşırken, hizmetlerin Klasik Portalı'ndan silmek olurdu değil. Sonunda Klasik portalı kullanım dışıdır.

### <a name="can-i-rename-my-storsimple-device-manager-service-in-the-azure-portal"></a>StorSimple Aygıt Yöneticisi'ni Hizmetim Azure portalında yeniden adlandırabilir miyim?

Hayır. Hizmet adı, hizmet Azure portalında oluşturulduktan sonra değiştirilemez. Aynı davranışı, cihazları, birimler, birim kapsayıcıları ve yedekleme ilkeleri gibi diğer varlıklar için de geçerlidir.

### <a name="can-i-create-the-80108020-storsimple-cloud-appliances-with-azure-resource-manager-deployment-model"></a>Azure Resource Manager dağıtım modeliyle 8010/8020 StorSimple bulut cihazları oluşturabilir miyim?

Evet. Yeni 8010/8020 StorSimple bulut uygulamaları Azure Resource Manager dağıtım modelinde oluşturabilirsiniz. Temel alınan VM'ler bu bulut uygulamaları için de Resource Manager dağıtım modelinde altındadır.

### <a name="when-we-migrate-subscriptions-to-the-azure-portal-will-the-underlying-vms-for-the-storsimple-cloud-appliances-also-migrate-to-azure-resource-management-deployment-model"></a>Abonelik Azure portalında geçirdiğinizde temel VM'ler StorSimple bulut uygulamaları için de Azure kaynak yönetimi dağıtım modeli için geçişini?

StorSimple hizmeti taşıma VM'ler yönetim StorSimple bulut uygulamaları için bağımsızdır. Tam olarak VM'ler StorSimple bulut uygulamaları için hem Klasik hem de Azure portalında bugün bile yönetebilirsiniz.

### <a name="can-i-manage-existing-classic-80108020-storsimple-cloud-appliance-from-the-azure-portal"></a>Varolan Klasik 8010/8020 StorSimple bulut uygulaması Azure portalından yönetebilir miyim?

Evet. Var olan 8010/8020 bulut uygulamaları ile ilişkili sanal makineleri Azure portaldan yönetilebilir.

Oluşturduysanız 8010/8020 güncelleştirme 3.0 çalıştıran StorSimple bulut cihazları model ve yukarıdaki, yeni Azure portalına taşınacağını hizmetiniz tarafından etkilenmez. Bulut gereçleriniz herhangi bir sorun olmadan tam olarak yönetmek olması gerekir. 

Klasik portalda Update 3. 0'den önceki sürümleri çalıştıran cihazları daha sonra yalnızca mevcut işlevselliğini sınırlı bulut varsa. Daha fazla bilgi için Git [güncelleştirme 3'den önceki sürümleri çalıştıran cihazlar için desteklenmeyen işlemler listesi](storsimple-8000-manage-service.md##supported-operations-on-devices-running-versions-prior-to-update-50).

Bulut gerecini güncelleştiremezsiniz. Yeni bir bulut uygulaması oluşturabilir ve varolan birim kapsayıcıları oluşturulan yeni bulut uygulaması için yük devretme için en son yazılım sürümünü kullanın. Daha fazla bilgi için Git [bulut uygulaması yük devri](storsimple-8000-cloud-appliance-u2.md#fail-over-to-the-cloud-appliance)


### <a name="my-storsimple-8000-series-device-is-running-update-20-i-migrated-my-service-to-new-azure-portal-my-device-connected-successfully-but-it-seems-that-i-am-not-able-to-fully-manage-my-device-how-do-i-resolve-this-behavior"></a>StorSimple 8000 serisi cihazımı Update 2.0 çalışıyor. I Hizmetim yeni Azure portalına geçirildi. Cihazımı başarıyla bağlandı, ancak tam olarak cihazımı yönetebilmek için değilim olduğunu gibi görünüyor. Bu davranışı nasıl giderebilirim?

Yeni Azure portalına yalnızca güncelleştirme 3.0 çalıştıran StorSimple cihazlar için ve daha yüksek desteklenir. Cihazınızı güncelleştirme 2.0 çalışıyorsa, bu cihaz için kullanılabilir olan işlevsellik yalnızca sınırlı. Daha fazla bilgi için Git [güncelleştirme 3'den önceki sürümleri çalıştıran cihazlar için desteklenmeyen işlemler listesi](storsimple-8000-manage-service.md##supported-operations-on-devices-running-versions-prior-to-update-50).

Cihazınız tam olarak yönetmek için Cihazınızda en son güncelleştirmeyi yükleyin. Daha fazla bilgi için Git [yükleme güncelleştirme 5](storsimple-8000-install-update-5.md).

### <a name="i-just-moved-my-storsimple-manager-service-to-the-azure-portal-i-am-seeing-some-alerts-related-to-my-device-is-this-behavior-related-to-the-move"></a>I yalnızca benim StorSimple Yöneticisi hizmeti Azure portalına taşındı. Cihazımı için ilgili bazı uyarılar görüyorum. Bu davranış taşımak için ilişkili mi?

Hayır. Taşıma hatalar veya uyarılar neden değil. Uyarıları gidermek için uyarı önerilere uyun.

### <a name="i-am-using-a-storsimple-50007000-series-device-are-these-also-supported-in-the-azure-portal"></a>Bir StorSimple 5000/7000 Serisi aygıtı kullanıyorum. Bunlar, ayrıca Azure portalında destekleniyor mu?

Hayır. StorSimple 5000/7000 Serisi cihazlar Azure portalında desteklenmez.

### <a name="i-am-planning-to-migrate-data-from-storsimple-50007000-series-device-to-a-storsimple-8000-series-device-how-does-moving-a-service-to-azure-portal-impact-my-data-migration"></a>Bir StorSimple 8000 serisi cihaza StorSimple 5000/7000 Serisi aygıttan veri geçirmeyi planlama. Bir hizmet Azure portalına taşınacağını my veri geçişinin nasıl etkiler? 

Azure portalında çalışan StorSimple 8000 serisi aygıta StorSimple 5000/7000 Serisi aygıtınızdan verileri geçirebilirsiniz. 8000 serisi 5000/7000 Serisi veri geçiş için etkinleştirmek için şunları yapmalısınız [bir destek bileti Microsoft Support oturum](storsimple-8000-contact-microsoft-support.md).


## <a name="moving-subscriptions-storage-accounts-resource-groups"></a>Taşıma abonelikler, depolama hesapları, kaynak grupları

### <a name="can-i-move-storsimple-device-manager-from-one-subscription-to-another-subscription-in-the-azure-portal"></a>I StorSimple Aygıt Yöneticisi'ni bir abonelikten Azure portalında başka bir abonelik için taşıyabilir miyim?

Hayır. StorSimple cihaz Yöneticisi hizmeti bir abonelikten taşıma desteklenmiyor. Aşağıdaki adımlardan oluşan bir el ile işlem gerçekleştirebilirsiniz:

* StorSimple cihazı verileri geçirin.
* Bir cihazı fabrika ayarlarına gerçekleştirmek, bu cihazdaki yerel tüm veriler silinir.
* Bir StorSimple cihaz Yöneticisi hizmeti için yeni abonelik ile cihazı kaydedin.
* Verileri geri cihaza geçirin.

### <a name="do-i-have-to-migrate-the-storage-account-to-azure-resource-manager-deployment-model"></a>Azure Resource Manager dağıtım modeli için depolama hesabını geçirin var mı?

Hayır. Klasik depolama hesaplarınızı Azure portalından tam olarak yönetilebilir. Azure portalında, StorSimple hizmetinize taşıdığınızda, depolama hesabınız önceki gibi çalışmaya devam eder.

### <a name="what-happens-to-the-storage-account-after-the-service-is-migrated-to-the-azure-portal"></a>Azure portalına hizmet geçirildikten sonra depolama hesabına ne olur?

Depolama hesabı Management'a Taşı servisin ilişkili değil. Tam olarak hem Klasik hem de Azure Resource Manager depolama hesaplarını Azure portalını yönetilebilir. Azure portalına hizmetinizi geçtiğinizde, Cihazınızı bu depolama hesabı ile çalışmaya devam etmelidir ve verileriniz için bir etki olmamalıdır.

### <a name="can-i-migrate-storsimple-device-manager-from-one-resource-group-to-another"></a>StorSimple Aygıt Yöneticisi'ni bir kaynak grubundan diğerine geçişini sağlayabilir miyim?

Hayır. Bir StorSimple Aygıt Yöneticisi'ni başka bir kaynak grubu için bir kaynak grubu ile oluşturulan taşınamıyor.

## <a name="using-azure-resource-manager-based-cmdlets"></a>Cmdlet tabanlı Azure Resource Manager kullanarak

### <a name="i-moved-to-the-new-azure-portal-now-my-scripts-based-on-azure-classic-deployment-model-powershell-cmdlets-are-not-working-what-do-i-need-to-do"></a>I yeni Azure portalına taşındı. Artık Azure Klasik dağıtım modeli PowerShell cmdlet'lerini temel alır my betikleri çalışmıyor? Yapmak neler gerekir?

Mevcut Azure Klasik dağıtım modeli PowerShell cmdlet'leri, Azure portalında desteklenmez. Cihazlarınızı aracılığıyla Azure Kaynak Yöneticisi'ni yönetmek için betikler güncelleştirin. Komut güncelleştirme hakkında daha fazla bilgi için Git [yeni Azure portalına için örnek](https://github.com/anoobbacker/storsimpledevicemgmttools).

### <a name="i-just-moved-to-the-azure-portal-and-needed-to-roll-over-the-service-data-encryption-key-where-is-this-option-in-the-azure-portal"></a>I yalnızca Azure portalına taşınır ve hizmet verileri şifreleme anahtarı alma için gerekli. Bu seçenek Azure portalında nerede?

Hizmet verileri şifreleme anahtarı alma seçeneği Azure portalında değil. Azure portal'ın bu geçiş yapma hakkında daha fazla bilgi için Git [hizmet verileri şifreleme anahtarı değiştirmek](storsimple-8000-manage-service.md#change-the-service-data-encryption-key).

### <a name="i-am-using-windows-powershell-for-storsimple-cmdlets-on-the-storsimple-device-to-perform-operations-such-extract-a-support-package-are-these-cmdlets-affected-when-i-move-to-the-new-azure-portal"></a>Windows PowerShell için StorSimple cmdlet'leri StorSimple cihazında böyle ayıklama işlemleri gerçekleştirmek için bir destek paketi kullanıyorum. Yeni Azure portalına taşıdığınızda bu cmdlet'leri etkilenen?

Hayır. Yeni Azure portalına taşınacağını hizmetiniz ile olmamalıdır etkilemez (kendisi tarafından taşıma etkilenmez) şirket içi StorSimple cihazı ile ilişkili StorSimple cmdlet'leri için Windows PowerShell üzerinde. Bir destek paketi herhangi bir sorun olmadan bile Azure Portalı'nda oluşturmak için bu cmdlet'leri kullanmaya devam edebilirsiniz. Yalnızca Azure Klasik dağıtım modeli PowerShell cmdlet'leri tarafından bu taşıma etkiler.

## <a name="moving-storsimple-data-manager-service"></a>StorSimple veri Yöneticisi hizmeti taşıma

### <a name="i-am-using-storsimple-data-manager-service-in-classic-azure-portal-how-should-i-proceed-with-this-move"></a>Klasik Azure Portalı'nda StorSimple veri Yöneticisi hizmeti kullanıyorum. Bu taşıma ile nasıl devam?

StorSimple veri Yöneticisi hizmeti kullanıyorsanız, otomatik olarak Azure portalına taşınmıştır.

## <a name="miscellaneous"></a>Muhtelif Hükümler

### <a name="i-am-running-storsimple-snapshot-manager-for-my-8000-series-device-is-there-any-impact-on-storsimple-snapshot-manager-when-i-move-to-azure-portal"></a>8000 serisi cihazımı için StorSimple Snapshot Manager çalıştıran. Azure portalına taşıdığınızda etkilerini StorSimple Snapshot Manager var?

Hayır. Azure portalına hizmetinizi taşıdığınızda için StorSimple Snapshot Manager etkisi yoktur. Cihaz ve StorSimple Snapshot Manager önceki gibi çalışmaya devam eder.

### <a name="can-i-rename-my-storsimple-device-volume-containers-or-volumes"></a>My StorSimple cihaz, birim kapsayıcıları veya birimleri yeniden adlandırabileceğini?

Hayır. Cihazlar, birimler, birim kapsayıcıları veya Azure portalında yedekleme ilkelerini adlandıramazsınız.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinmek [desteklenen işlemler güncelleştirme 5.0'den önceki sürümleri çalıştıran cihazlarda](storsimple-8000-manage-service.md#supported-operations-on-devices-running-versions-prior-to-update-50).



