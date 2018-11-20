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
ms.date: 11/19/2018
ms.author: sethm
ms.reviewer: efemmano
ms.openlocfilehash: 9a10d4fc90b916b3cb1eda7b9bac99c5d5f9deba
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52160792"
---
# <a name="region-management-in-azure-stack"></a>Azure stack'teki bölge Yönetimi

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack kavramını kullanır *bölgeleri*, Azure Stack altyapısını oluşturan donanım kaynakları oluşan mantıksal varlıkların olduğu. Bölge Yönetimi'nde, Azure Stack altyapısının başarılı biçimde çalışması için gerekli tüm kaynakları bulabilirsiniz.

Bir tümleşik sistemi dağıtımı (olarak adlandırılan bir *Azure Stack bulut*) tek bir bölgede yapar. Her Azure Stack geliştirme Seti'ni adlı tek bir bölge sahip **yerel**. İkinci bir Azure Stack tümleşik sistemi dağıtmak ya da başka bir örneğinin ayrı donanım geliştirme setinde ayarlamak, bu Azure Stack Bulutu farklı bir bölgedir.

## <a name="information-available-through-the-region-management-tile"></a>Bölge Yönetimi kutucuğu aracılığıyla kullanılabilir bilgileri

Azure Stack sahip bir dizi içinde kullanılabilen bölge yönetim özellikleri **bölge Yönetimi** Döşe. Bu kutucuk, Yönetici portalı'ndaki varsayılan Panoda Azure Stack operatörü için kullanılabilir. Bu kutucuğu aracılığıyla izleyin ve Azure Stack bölgeniz ve bölgeye özgü bileşenlerinden güncelleştirin.

![Bölge Yönetimi kutucuğu](media/azure-stack-manage-region/image1.png)

Bir bölgede tıklarsanız **bölge Yönetimi** kutucuğu, aşağıdaki bilgileri erişebilirsiniz:

![Bölge Yönetimi dikey penceresinde bölmeleri açıklaması](media/azure-stack-manage-region/image2.png)

1. **Kaynak menüsünde**. Belirli bir altyapı Yönetimi alanlara erişmek ve görüntüleyin ve depolama hesapları ve sanal ağlar gibi kullanıcı kaynaklarını yönetin.

2. **Uyarılar**. Sistem genelinde uyarılarını listeler ve her uyarıların ayrıntılarını sağlar.

3. **Güncelleştirmeleri**. Geçerli sürümü, Azure Stack altyapısını, kullanılabilir güncelleştirmeleri ve güncelleştirme geçmişini görüntüleyin. Ayrıca, tümleşik sistem güncelleştirebilirsiniz.

4. **Kaynak sağlayıcıları**. Azure Stack'i çalıştırmak için gerekli bileşenler tarafından sunulan kullanıcı işlevselliği yönetin. Her kaynak sağlayıcısı, bir yönetim deneyimi ile birlikte gelir. Bu deneyim, belirli bir sağlayıcıyı, ölçümleri ve diğer yönetim özelliklerini kaynak sağlayıcıya özel uyarıları içerebilir.

5. **Altyapı rollerini**. Azure Stack'i çalıştırmak için gereken bileşenler. Uyarı raporu altyapı rolleri listelenir. Bir rolü seçerek rolü ve bu rolün çalıştığı rol örneği ile ilişkili uyarıları görüntüleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Sistem durumu ve Azure stack'teki uyarıları izleme](azure-stack-monitor-health.md)
- [Azure stack'teki güncelleştirmelerini yönetme](azure-stack-updates.md)
