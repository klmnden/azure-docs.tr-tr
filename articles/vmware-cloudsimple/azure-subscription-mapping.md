---
title: Azure aboneliği Azure VMware çözümü CloudSimple tarafından kaynak havuzlarında eşleyin
description: Azure aboneliğinizde bir kaynak havuzu CloudSimple VMware çözümüyle Azure üzerinde eşleme açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/05/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: efda996e03d46a2f97d19558f7c2930b623a639e
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2019
ms.locfileid: "67332545"
---
# <a name="map-resource-pools-from-your-private-cloud-to-your-azure-subscription"></a>Özel bulut kaynak havuzlarından Azure aboneliğinize eşleme

Azure aboneliği eşleme özel bulut vCenter'ınıza Azure aboneliğinize kaynak havuzlarından eşlemenizi sağlar. Yalnızca CloudSimple hizmet burada oluşturduğunuz abonelik eşleyebilirsiniz.  Azure portalından bir VMware sanal makine oluştururken, eşleşen kaynak havuzundaki sanal makine dağıtır.  CloudSimple Portalı'nda görüntüleyebilir ve Azure aboneliği için özel Bulutlarınızın yönetin.

Bir aboneliğin birden fazla vCenter kaynak havuzlarına bir özel bulutun eşlenebilir.  Her bir özel buluta kaynak havuzlarını eşleme gerekir.  Yalnızca eşleşen kaynak havuzları bir VMware sanal makinesini Azure portalından oluşturmak için kullanılabilir.

> [!IMPORTANT]
> Bir kaynak havuzu eşleştirme, tüm alt kaynak havuzlarına eşler. Alt kaynak havuzu zaten eşlenirse üst kaynak havuzu eşlenemez.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, aboneliğinizde CloudSimple hizmeti ve özel bulut olduğunu varsayar.  CloudSimple hizmet oluşturmak için bkz: [hızlı başlangıç - hizmet oluşturma](quickstart-create-cloudsimple-service.md).  Özel bir bulut oluşturmak için ihtiyacınız varsa bkz [hızlı başlangıç - özel bir bulut ortamı yapılandırma](quickstart-create-private-cloud.md).

VCenter küme (kök kaynak havuzu) aboneliğiniz eşleyebilirsiniz.  Yeni bir kaynak havuzu oluşturmak istiyorsanız, bkz. [Create a Resource Pool](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.resmgmt.doc/GUID-0F6C6709-A5DA-4D38-BE08-6CB1002DD13D.html) makalede VMware belgeler sitesinde.

## <a name="default-resource-group"></a>Varsayılan kaynak grubu

Azure portalından yeni bir CloudSimple sanal makine oluştururken kaynak grubu seçmenizi sağlar.  Eşlenen kaynak havuzundaki özel bulut vCenter üzerinde oluşturulan bir sanal makinenin Azure portalında görünür.  Varsayılan Azure kaynak grubu içinde bulunan sanal makine yerleştirilir.  Varsayılan kaynak grubu adını değiştirebilirsiniz.

## <a name="map-azure-subscription"></a>Azure aboneliği eşleme

1. Erişim [CloudSimple portalı](access-cloudsimple-portal.md).

2. Açık **kaynakları** sayfasında ve eşlemek istediğiniz özel Bulutu seçin.

3. Seçin **eşleme Azure abonelikleri**.

4. Tıklayın **Düzenle Azure abonelik eşleme**.

5. Kullanılabilir kaynak havuzları eşlemek için sol taraftaki bunları seçin ve sağ taraftaki oka tıklayın.

6. Eşlemeleri kaldırmak için bunları sağ tarafta seçip sol taraftaki oka tıklayın.

    ![Azure abonelikleri](media/resources-azure-mapping.png)

7. **Tamam**'ı tıklatın.

## <a name="change-default-resource-group-name"></a>Varsayılan kaynak grubu adını değiştir

1. Erişim [CloudSimple portalı](access-cloudsimple-portal.md).

2. Açık **kaynakları** sayfasında ve eşlemek istediğiniz özel Bulutu seçin.

3. Seçin **eşleme Azure abonelikleri**.

4. Tıklayın **Düzenle** Azure kaynak grubu adı altında.

    ![Kaynak grubu adını Düzenle](media/resources-edit-resource-group-name.png)

5. Kaynak grubu için yeni bir ad girin ve tıklayın **Gönder**.

    ![Yeni kaynak grubu adı girin](media/resources-new-resource-group-name.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure'da VMware sanal makinelerini kullanma](quickstart-create-vmware-virtual-machine.md)
* Daha fazla bilgi edinin [CloudSimple sanal makineler](cloudsimple-virtual-machines.md)