---
title: Azure stack'teki bölge Yönetimi | Microsoft Docs
description: Azure stack'teki bölge yönetimine genel bakış.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: e94775d5-d473-4c03-9f4e-ae2eada67c6c
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2018
ms.author: sethm
ms.reviewer: efemmano
ms.openlocfilehash: 401b81ceb7ab71528a4ad11bc7d8944b4d732933
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "49078877"
---
# <a name="region-management-in-azure-stack"></a>Azure stack'teki bölge Yönetimi

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack, Azure Stack altyapısını oluşturan donanım kaynakları oluşan mantıksal varlıklar bölgeler, kavramını kullanır. Bölge Yönetimi içinde Azure Stack altyapısının başarılı biçimde çalışması için gerekli tüm kaynakları bulabilirsiniz.

Bir tümleşik sistemi dağıtımı (olarak adlandırılan bir *Azure Stack bulut*) tek bir bölgede yapar. Her Azure Stack geliştirme Seti'ni adlı tek bir bölge sahip **yerel**. İkinci bir Azure Stack tümleşik sistemi dağıtmak ya da başka bir örneğinin ayrı donanım geliştirme setinde ayarlamak, bu Azure Stack Bulutu farklı bir bölgedir.

## <a name="information-available-through-the-region-management-tile"></a>Bölge Yönetimi kutucuğu aracılığıyla kullanılabilir bilgileri

Azure Stack sahip bir dizi içinde kullanılabilen bölge yönetim özellikleri **bölge Yönetimi** Döşe. Bu kutucuk, Yönetici portalı'ndaki varsayılan Panoda Azure Stack operatörü için kullanılabilir. Bu kutucuğu aracılığıyla izleyin ve Azure Stack bölgeniz ve bölgeye özgü bileşenlerinden güncelleştirin.

 ![Bölge Yönetimi kutucuğu](media/azure-stack-manage-region/image1.png)

 Bölge Yönetimi kutucuğu bölgede tıklarsanız aşağıdaki bilgileri erişebilirsiniz:

  ![Bölge Yönetimi dikey penceresinde bölmeleri açıklaması](media/azure-stack-manage-region/image2.png)

1. **Kaynak menüsünde**. Burada, belirli bir altyapı yönetim alanları erişebilirsiniz görüntülemek ve depolama hesapları ve sanal ağlar gibi kullanıcı kaynaklarını yönetin.

2. **Uyarılar**. Bu, sistem genelinde uyarılarını listeler ve her uyarıların ayrıntılarını sağlar.

3. **Güncelleştirmeleri**. Burada, geçerli sürümü, Azure Stack altyapısını, kullanılabilir güncelleştirmeleri ve güncelleştirme geçmişini görüntüleyebilirsiniz. Ayrıca, tümleşik sistem güncelleştirebilirsiniz.

4. **Kaynak sağlayıcıları**. Kaynak sağlayıcıları, Azure Stack'i çalıştırmak için gerekli bileşenler tarafından sunulan kullanıcı işlevselliği yönetmek için bir yerde olur. Her kaynak sağlayıcısı, bir yönetim deneyimi ile birlikte gelir. Bu deneyim, belirli bir sağlayıcıyı, ölçümleri ve diğer yönetim özelliklerini kaynak sağlayıcıya özel uyarıları içerebilir.

5. **Altyapı rollerini**. Altyapı, Azure Stack'i çalıştırmak için gerekli bileşenleri rolleridir. Uyarı raporu altyapı rolleri listelenir. Bir rolü seçerek rolü ve bu rolün çalıştığı rol örneği ile ilişkili uyarıları görüntüleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Sistem durumu ve Azure stack'teki uyarıları izleme](azure-stack-monitor-health.md)
- [Azure stack'teki güncelleştirmelerini yönetme](azure-stack-updates.md)
