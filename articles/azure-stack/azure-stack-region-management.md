---
title: Azure yığınında bölge Yönetimi | Microsoft Docs
description: Azure yığınında bölge yönetimine genel bakış.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: e94775d5-d473-4c03-9f4e-ae2eada67c6c
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2018
ms.author: brenduns
ms.reviewer: efemmano
ms.openlocfilehash: 0286ed9c7b3fe320b936d33fe3beaddccd6ac0fa
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35247543"
---
# <a name="region-management-in-azure-stack"></a>Azure yığınında bölge Yönetimi

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Azure yığın Azure yığın altyapısını oluşturan donanım kaynakları oluşan mantıksal varlıklar bölgeler kavramı kullanır. Bölge Yönetimi içinde Azure yığın altyapı başarıyla çalışması için gereken tüm kaynakları bulabilirsiniz.

Bir tümleşik sistemi dağıtımı (olarak adlandırılan bir *Azure yığın bulut*) tek bir bölge yapar. Her Azure yığın Geliştirme Seti adlı tek bir bölge vardır **yerel**. İkinci bir Azure tümleşik yığını sistemi dağıtmak veya başka bir örneği ayrı bir donanıma Geliştirme Seti ayarlayın, bu Azure yığın bulut farklı bir bölgeye olur.

## <a name="information-available-through-the-region-management-tile"></a>Bölge yönetim döşemenin kullanılabilir bilgileri

Azure yığın sahip bir dizi bölge yönetim özelliği bulunan **bölge Yönetimi** döşeme. Bu kutucuğu, Azure yığın operatörün Yönetici portalı'nda varsayılan Panoda kullanılabilir. Bu kutucuğu izlemek ve Azure yığın bölgeniz ve bölgeye özgü bileşenlerini güncelleştirin.

 ![Bölge yönetim döşeme](media/azure-stack-manage-region/image1.png)

 Bir bölge yönetim döşeme bölgede tıklarsanız, aşağıdaki bilgileri erişebilirsiniz:

  ![Bölge yönetim dikey bölmeleri açıklaması](media/azure-stack-manage-region/image2.png)

1. **Kaynak menü**. Burada, belirli altyapı Yönetimi alanlara erişebilir ve görüntülemek ve depolama hesapları ve sanal ağlar gibi kullanıcı kaynaklarını yönetme.

2. **Uyarılar**. Bu, sistem genelinde uyarıları listeler ve her uyarıların ayrıntıları sağlar.

3. **Güncelleştirmeleri**. Burada, geçerli sürümünü Azure yığın altyapınızı, kullanılabilir güncelleştirmeleri ve güncelleştirme geçmişini görüntüleyebilirsiniz. Tümleşik sisteminizi da güncelleştirebilirsiniz.

4. **Kaynak sağlayıcıları**. Kaynak sağlayıcıları yerdir Azure yığın çalıştırmak için gerekli bileşenleri tarafından sunulan kullanıcı işlevselliği yönetmek için. Her kaynak sağlayıcısı, bir yönetim deneyimi ile birlikte gelir. Bu deneyim, belirli bir sağlayıcıyı, ölçümleri ve diğer yönetim özelliklerini kaynak sağlayıcıya özel uyarılar içerebilir.

5. **Altyapı rollerini**. Altyapı, Azure yığın çalıştırmak gerekli bileşenleri rolleridir. Yalnızca uyarı raporu altyapı rolleri listelenir. Bir rolü seçerek rolü ve bu rolün çalıştığı rol örneği ile ilişkili uyarıları görüntüleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Sistem durumu ve Uyarıları Azure yığınında izleme](azure-stack-monitor-health.md)
- [Azure yığınında güncelleştirmelerini yönetme](azure-stack-updates.md)
