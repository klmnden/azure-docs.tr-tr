---
title: Azure VMware çözümü CloudSimple - hizmet olarak oluşturma
description: Azure portalında CloudSimple hizmet oluşturmayı açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/04/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: fc08ce48b3b5a4b2cd28cb66e6dd4d9eae3f3885
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66676961"
---
# <a name="create-azure-vmware-solution-by-cloudsimple---service"></a>CloudSimple - hizmet olarak Azure VMware çözümü oluşturun

Azure ile VMware çözümü tarafından CloudSimple kullanmaya başlamak için Azure portalında CloudSimple hizmeti tarafından Azure VMware çözümü oluşturun.

> [!NOTE]
> CloudSimple hizmet oluşturmadan önce Azure aboneliğinizde Microsoft.VMwareCloudSimple kaynak sağlayıcısını kaydetmeniz gerekir. Bağlantısındaki [Azure aboneliğinize Microsoft.VMwareCloudSimple kaynak sağlayıcısını etkinleştirmek](enable-cloudsimple-service.md).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-the-service"></a>Hizmeti oluşturma

1. **Tüm Hizmetler**’i seçin.

2. Arama **CloudSimple Hizmetleri**.

    ![Arama CloudSimple hizmeti](media/create-cloudsimple-service-search.png)

3. Seçin **CloudSimple Hizmetleri**.

4. Tıklayın **Ekle** yeni bir hizmet oluşturmak için.

    ![CloudSimple Hizmet Ekle](media/create-cloudsimple-service-add.png)

5. CloudSimple hizmetini oluşturmak istediğiniz aboneliği seçin.

6. Hizmeti için kaynak grubunu seçin. Yeni bir kaynak grubu eklemek için tıklatın **Yeni Oluştur**.

7. Hizmetinizi tanımlayabilmek için adı girin.

8. Hizmet ağ geçidi için CIDR girin. / 28 belirtin herhangi biri, mevcut alt ağlar ile çakışmayacak bir alt.  Şirket içi alt ağlar, Azure alt ağlar, bunlar veya herhangi bir CloudSimple alt ağlar planlanmış. Hizmet oluşturulduktan sonra CIDR değiştiremezsiniz.

    ![CloudSimple hizmeti oluşturma](media/create-cloudsimple-service.png)

9. **Tamam**'ı tıklatın.

Hizmet oluşturulur ve hizmetler listesine eklenir.

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi edinmek için nasıl [özel bulut oluşturma](https://docs.azure.cloudsimple.com/create-private-cloud/)
* Bilgi edinmek için nasıl [bir özel bulut ortamı yapılandırma](quickstart-create-private-cloud.md)
