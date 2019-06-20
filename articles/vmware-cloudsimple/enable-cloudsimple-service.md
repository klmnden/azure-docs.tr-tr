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
ms.openlocfilehash: 2553aa95d5028c510b4e1a1b7f51a9f410bcea51
ms.sourcegitcommit: 1289f956f897786090166982a8b66f708c9deea1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67154856"
---
# <a name="register-the-microsoftvmwarecloudsimple-resource-provider-on-your-azure-subscription"></a>Azure aboneliğinizde Microsoft.VMwareCloudSimple kaynak sağlayıcısını kaydetme

CloudSimple service, Azure VMware CloudSimple çözümüyle kullanmasını sağlar. Kaynak sağlayıcınız olarak Microsoft.VMwareCloudSimple hizmet kaydedebilirsiniz.

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
