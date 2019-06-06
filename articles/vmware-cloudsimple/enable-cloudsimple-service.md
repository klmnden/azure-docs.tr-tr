---
title: CloudSimple hizmeti tarafından Azure VMware çözümünü etkinleştirme
description: Bir Azure aboneliğine CloudSimple hizmette etkinleştirin ve ardından CLoudSimple kaynak sağlayıcısını kaydetme açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/04/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: cacd5510147ce997efec922f4b4656956a098d88
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66676946"
---
# <a name="register-the-microsoftvmwarecloudsimple-resource-provider-on-your-azure-subscription"></a>Azure aboneliğinizde Microsoft.VMwareCloudSimple kaynak sağlayıcısını kaydetme

CloudSimple service, Azure VMware CloudSimple çözümüyle kullanmasını sağlar. CloudSimple hizmetini kullanmak için önce Azure aboneliğinizde etkinleştirilmesi gerekir. Ardından, Microsoft.VMwareCloudSimple hizmet kaynak sağlayıcınız olarak kaydedebilirsiniz.

## <a name="enable-the-cloudsimple-service"></a>CloudSimple hizmetini etkinleştirme

Azure aboneliğinizde CloudSimple hizmetini etkinleştirmek için bir destek isteği açın [Microsoft Destek](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest). İsteği gönderdiğinizde, aşağıdaki seçenekleri belirleyin.

* Sorun türü: **Teknik**
* Abonelik: **Abonelik Kimliğiniz**
* Hizmet türü: **VMware çözümü CloudSimple tarafından**
* Sorun türü: **Adanmış düğümler kota**
* Sorun alt tür: **Adanmış düğümler kotasını artırma**
* Konu: **CloudSimple hizmetini etkinleştirme**

Ayrıca, Microsoft hesap temsilcinize başvurun [ azurevmwaresales@microsoft.com ](mailto:azurevmwaresales@microsoft.com). E-posta, Azure abonelik Kimliğinizi belirtin.  

Aboneliğiniz için CloudSimple hizmeti etkinleştirildikten sonra aboneliğinizin kaynak sağlayıcısı etkinleştirebilirsiniz.

## <a name="register-the-resource-provider"></a>Kaynak sağlayıcısını kaydetme

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. **Tüm Hizmetler**’i seçin.

3. Arayın ve seçin **abonelikleri**.

    ![Abonelikleri seçin](media/cloudsimple-service-select-subscriptions.png)

4. CloudSimple hizmetini etkinleştirmek istediğiniz aboneliği seçin.

5. Tıklayın **kaynak sağlayıcıları** abonelik için.

6. Kullanım **Microsoft.VMwareCloudSimple** kaynak sağlayıcısı filtre uygulamak için.

7. Kaynak Sağlayıcısı'nı seçin ve tıklayın **kaydetme**.

    ![Kaynak sağlayıcısını kaydetme](media/cloudsimple-service-enable-resource-provider.png)

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi edinmek için nasıl [CloudSimple hizmeti oluşturma](create-cloudsimple-service.md)
* Bilgi edinmek için nasıl [bir özel bulut ortamı yapılandırma](quickstart-create-private-cloud.md)