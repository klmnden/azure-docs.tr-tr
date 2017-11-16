---
title: "Bulut hizmetinin (portal) nasıl yapılandırılacağı | Microsoft Docs"
description: "Bulut Hizmetleri ile Azure yapılandırmayı öğrenin. Bulut hizmeti yapılandırmasını güncelleştirin ve rol örnekleri için uzaktan erişim yapılandırma hakkında bilgi edinme. Bu örnekler Azure Portalı'nı kullanın."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 7308f3c0-825e-499d-bfa5-c60f86371921
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2016
ms.author: adegeo
ms.openlocfilehash: 409b3bb26648ef1b811dfaaf37690c8220046729
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2017
---
# <a name="how-to-configure-cloud-services"></a>Bulut Hizmetleri Yapılandırma
Azure portalında bir bulut hizmeti için en yaygın olarak kullanılan ayarları yapılandırabilirsiniz. Ya da yapılandırma dosyalarınızı doğrudan güncelleştirmek isterseniz güncelleştirilecek hizmet yapılandırma dosyasını indirin ve ardından güncelleştirilmiş dosyayı yükleyin ve bulut hizmetini yapılandırma değişiklikleriyle güncelleştirin. Her iki şekilde de yapılandırma güncelleştirmeleri tüm rol örneklerine atılır.

Ayrıca, bulut hizmeti rollerinizi ya da Uzak Masaüstü örneklerini bunları içine yönetebilirsiniz.

Her rol için en az iki rol örneği varsa azure yapılandırma güncelleştirmeleri sırasında yalnızca 99,95 yüzde hizmet kullanılabilirlik sağlayabilirsiniz. Diğer güncelleştirilirken istemci isteklerini işlemek bir sanal makine sağlar. Daha fazla bilgi için bkz: [hizmet düzeyi sözleşmeleri](https://azure.microsoft.com/support/legal/sla/).

## <a name="change-a-cloud-service"></a>Bir bulut hizmeti değiştirme
Açılış sonra [Azure portal](https://portal.azure.com/), bulut hizmetine gidin. Buradan, birçok özelliklerini yönetebilir.

![Ayarları sayfası](./media/cloud-services-how-to-configure-portal/cloud-service.png)

**Ayarları** veya **tüm ayarları** bağlantılar açılır **ayarları** değiştirebileceğiniz dikey **özellikleri**, değiştirmek **yapılandırma**, yönetmek **sertifikaları**, Kurulum **uyarı kuralları**ve yönetmek **kullanıcılar** kimin bu bulut hizmeti erişimi vardır.

![Azure bulut hizmeti ayarlar dikey penceresi](./media/cloud-services-how-to-configure-portal/cs-settings-blade.png)

### <a name="manage-guest-os-version"></a>Konuk işletim sistemi sürümü yönetme

Varsayılan olarak, Azure Windows Server 2016 gibi konuk işletim sistemi, hizmet yapılandırma (.cscfg), belirttiğiniz işletim sistemi ailesi içinde desteklenen en son görüntü düzenli olarak güncelleştirir.

Belirli bir işletim sistemi sürümü hedeflemesini ihtiyacınız varsa, onu ayarlayabilirsiniz **yapılandırma** dikey.

![Küme işletim sistemi sürümü](./media/cloud-services-how-to-configure-portal/cs-settings-config-guestosversion.png)


>[!IMPORTANT]
> Belirli bir işletim sistemi sürümüne otomatik işletim sistemi devre dışı bırakır seçme güncelleştirir ve sizin sorumluluğunuzdadır düzeltme eki uygulama yapar. Rolü örneklerinizi güncelleştirmeleri almakta veya güvenlik açıkları uygulamanıza getirebilir emin olmalısınız.

## <a name="monitoring"></a>İzleme
Uyarılar, bulut hizmetine ekleyebilirsiniz. Tıklatın **ayarları** > **uyarı kuralları** > **uyarı Ekle**.

![](./media/cloud-services-how-to-configure-portal/cs-alerts.png)

Buradan, bir uyarı ayarlayabilirsiniz. İle **ölçüm** açılan kutusu, aşağıdaki veri türleri için uyarı ayarlayabilirsiniz.

* Disk okuma
* Disk yazma
* Ağ içine
* Ağ dışına
* CPU yüzdesi

![](./media/cloud-services-how-to-configure-portal/cs-alert-item.png)

### <a name="configure-monitoring-from-a-metric-tile"></a>Bir ölçüm kutucuğunda İzlemeyi Yapılandır
Yerine **ayarları** > **uyarı kuralları**, ölçüm döşemeleri birinde tıklayabilirsiniz **izleme** bölümünü **bulut hizmeti** dikey.

![Bulut hizmeti izleme](./media/cloud-services-how-to-configure-portal/cs-monitoring.png)

Buradan içeren kutucuğa kullanılan grafiği özelleştirme veya bir uyarı kuralı ekleyin.

## <a name="reboot-reimage-or-remote-desktop"></a>Yeniden başlatma, yeniden görüntü oluşturma ya da Uzak Masaüstü
Uzak Masaüstü aracılığıyla ayarlayabilirsiniz [Azure portal (Kurulum Uzak Masaüstü)](cloud-services-role-enable-remote-desktop-new-portal.md), [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md), aracılığıyla veya [Visual Studio](../vs-azure-tools-remote-desktop-roles.md).

Yeniden başlatma için yeniden görüntü oluşturma veya uzak bir bulut hizmeti içine tıklatın bulut hizmeti örneği.

![Bulut hizmeti örneği](./media/cloud-services-how-to-configure-portal/cs-instance.png)

Dikey penceresinden bir Uzak Masaüstü bağlantısı başlatmak, uzaktan örneğini yeniden başlatın veya Uzaktan yeniden görüntü oluşturma açar (yeni bir görüntüyle örneği başladığını).

![Bulut hizmeti örneği düğmeleri](./media/cloud-services-how-to-configure-portal/cs-instance-buttons.png)

## <a name="reconfigure-your-cscfg"></a>.cscfg yeniden yapılandırın
Bulut hizmeti aracılığıyla yeniden yapılandırmanız gerekebilir [hizmet yapılandırma (cscfg)](cloud-services-model-and-package.md#cscfg) dosyası. İlk .cscfg dosyanızı indirin, değiştirin ve sonra dosyayı karşıya gerekir.

1. Tıklayın **ayarları** simgesine veya **tüm ayarları** açık bağlantı **ayarları** dikey.

    ![Ayarları sayfası](./media/cloud-services-how-to-configure-portal/cloud-service.png)
2. Tıklayın **yapılandırma** öğesi.

    ![Yapılandırma dikey penceresi](./media/cloud-services-how-to-configure-portal/cs-settings-config.png)
3. Tıklayın **karşıdan** düğmesi.

    ![İndir](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-download.png)
4. Hizmet yapılandırma dosyası güncelleştirdikten sonra karşıya yükleme ve yapılandırma güncelleştirmeleri uygulayın:

    ![Karşıya Yükle](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-upload.png)
5. .Cscfg dosyasını tıklatıp **Tamam**.

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [bir bulut hizmetinin dağıtılacağı](cloud-services-how-to-create-deploy-portal.md).
* Yapılandırma bir [özel etki alanı adı](cloud-services-custom-domain-name-portal.md).
* [Bulut hizmetinizi yönetme](cloud-services-how-to-manage-portal.md).
* Yapılandırma [ssl sertifikalarını](cloud-services-configure-ssl-certificate-portal.md).
