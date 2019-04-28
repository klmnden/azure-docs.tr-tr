---
title: Nasıl bir bulut hizmetini yapılandırma (portal) | Microsoft Docs
description: Azure'da cloud services'ı yapılandırma konusunda bilgi edinin. Bulut hizmeti yapılandırmasını güncelleştirme ve rol örnekleri için uzaktan erişim yapılandırma hakkında bilgi edinme. Bu örnekler, Azure portalını kullanın.
services: cloud-services
documentationcenter: ''
author: jpconnock
manager: timlt
editor: ''
ms.assetid: 7308f3c0-825e-499d-bfa5-c60f86371921
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2016
ms.author: jeconnoc
ms.openlocfilehash: 4d8d3b93ef2a6347076fada53932b5fc56838d20
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61435870"
---
# <a name="how-to-configure-cloud-services"></a>Bulut Hizmetleri Yapılandırma

Azure portalında bir bulut hizmeti için en yaygın olarak kullanılan ayarları yapılandırabilirsiniz. Ya da yapılandırma dosyalarınızı doğrudan güncelleştirmek isterseniz güncelleştirilecek hizmet yapılandırma dosyasını indirin ve ardından güncelleştirilmiş dosyayı yükleyin ve bulut hizmetini yapılandırma değişiklikleriyle güncelleştirin. Her iki şekilde de yapılandırma güncelleştirmeleri tüm rol örneklerine atılır.

Ayrıca, Uzak Masaüstü veya Bulut Hizmeti rol örnekleri dosyanıza yönetebilirsiniz.

Her rol için en az iki rol örneği varsa azure sırasında yapılandırma güncelleştirmeleri yalnızca yüzde 99,95 hizmet kullanılabilirlik sağlayabilirsiniz. Bu, diğer güncelleştirilirken istemci isteklerini işlemek bir sanal makine sağlar. Daha fazla bilgi için [hizmet düzeyi sözleşmeleri](https://azure.microsoft.com/support/legal/sla/).

## <a name="change-a-cloud-service"></a>Bir bulut hizmeti değiştirme

Açılıştan sonra [Azure portalında](https://portal.azure.com/), bulut hizmetinize gidin. Buradan, birçok yönüyle yönetin.

![Ayarları sayfası](./media/cloud-services-how-to-configure-portal/cloud-service.png)

**Ayarları** veya **tüm ayarlar** bağlantıları açılır **ayarları** değiştirebileceğiniz **özellikleri**, değiştirme  **Yapılandırma**, Yönet **sertifikaları**, ayarlayın **uyarı kuralları**ve yönetme **kullanıcılar** bu bulut hizmetinin özelliklerine erişime sahiptir.

![Azure bulut hizmeti ayarları](./media/cloud-services-how-to-configure-portal/cs-settings-blade.png)

### <a name="manage-guest-os-version"></a>Konuk işletim sistemi sürümü yönetme

Varsayılan olarak, Azure gibi Windows Server 2016, ' % s'konuk işletim sistemi, hizmet yapılandırma (.cscfg), belirttiğiniz işletim sistemi ailesi içinde desteklenen en son görüntüye düzenli olarak güncelleştirir.

Belirli bir işletim sistemi sürümü hedeflemesini istiyorsanız gerekiyorsa, bunu ayarlayabilirsiniz **yapılandırma**.

![Küme işletim sistemi sürümü](./media/cloud-services-how-to-configure-portal/cs-settings-config-guestosversion.png)

>[!IMPORTANT]
> Belirli bir işletim sistemi sürümünün otomatik işletim sistemi devre dışı bırakır. seçme güncelleştirir ve sizin sorumluluğunuzdadır düzeltme eki uygulama yapar. Rol örneklerinizin güncelleştirmeleri aldığından veya uygulamanıza güvenlik açıklarını ortaya çıkarabilir emin olmanız gerekir.

## <a name="monitoring"></a>İzleme

Bulut hizmetinize uyarıları ekleyebilirsiniz. Tıklayın **ayarları** > **uyarı kuralları** > **uyarısı Ekle**.

![](./media/cloud-services-how-to-configure-portal/cs-alerts.png)

Buradan, bir alarm kurabilirsiniz. İle **ölçüm** açılan kutusunda, aşağıdaki veri türleri için bir uyarı ayarlayabilirsiniz.

* Disk okuma
* Disk yazma
* Ağ içine
* Ağ dışına
* CPU yüzdesi

![](./media/cloud-services-how-to-configure-portal/cs-alert-item.png)

### <a name="configure-monitoring-from-a-metric-tile"></a>Ölçüm kutucuklardan birini izlemeyi yapılandırma

Yerine **ayarları** > **uyarı kuralları**, ölçüm kutucuklarda birine tıklayabilirsiniz **izleme** bulut hizmetinin bölümü.

![Bulut hizmeti izleme](./media/cloud-services-how-to-configure-portal/cs-monitoring.png)

Buradan içeren bir kutucuk kullanılan grafiği özelleştirme veya bir uyarı kuralı ekleyin.

## <a name="reboot-reimage-or-remote-desktop"></a>Yeniden başlatma, yeniden görüntü oluşturma veya Uzak Masaüstü

Uzak Masaüstü aracılığıyla ayarlayabilirsiniz [(Uzak Masaüstü bağlantısı kurma ayarlanır) Azure portalında](cloud-services-role-enable-remote-desktop-new-portal.md), [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md), aracılığıyla veya [Visual Studio](cloud-services-role-enable-remote-desktop-visual-studio.md).

Yeniden başlatma için yeniden görüntü oluşturma veya uzak bir bulut hizmetine seçin bulut hizmeti örneği.

![Bulut hizmeti örneği](./media/cloud-services-how-to-configure-portal/cs-instance.png)

Uzak Masaüstü Bağlantısı'ı başlatmak, uzaktan örneği yeniden başlatma veya uzaktan görüntüsünü yeniden oluştur (yeni bir görüntü ile başlatma) örneği.

![Bulut hizmeti örneği düğmeleri](./media/cloud-services-how-to-configure-portal/cs-instance-buttons.png)

## <a name="reconfigure-your-cscfg"></a>.cscfg yeniden yapılandırın

Bulut hizmetinizi yeniden yapılandırmanız gerekebilir [hizmet yapılandırma (cscfg)](cloud-services-model-and-package.md#cscfg) dosya. İlk .cscfg dosyanızı indirin, değiştirin ve ardından karşıya gerekir.

1. Tıklayarak **ayarları** simgesi veya **tüm ayarlar** açın bağlantısına **ayarları**.

    ![Ayarları sayfası](./media/cloud-services-how-to-configure-portal/cloud-service.png)
2. Tıklayarak **yapılandırma** öğesi.

    ![Yapılandırma dikey penceresi](./media/cloud-services-how-to-configure-portal/cs-settings-config.png)
3. **İndir** düğmesine tıklayın.

    ![İndirme](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-download.png)
4. Hizmet yapılandırma dosyasının güncelleştirdikten sonra karşıya yükleme ve yapılandırma güncelleştirmeleri uygulayın:

    ![Karşıya Yükle](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-upload.png)
5. .Cscfg dosyasını seçin ve tıklayın **Tamam**.

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi edinmek için nasıl [bir bulut hizmeti dağıtma](cloud-services-how-to-create-deploy-portal.md).
* Yapılandırma bir [özel etki alanı adı](cloud-services-custom-domain-name-portal.md).
* [Bulut hizmetinizi yönetme](cloud-services-how-to-manage-portal.md).
* Yapılandırma [ssl sertifikaları](cloud-services-configure-ssl-certificate-portal.md).
