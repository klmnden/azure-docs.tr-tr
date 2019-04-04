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
ms.date: 03/21/2019
ms.author: sethm
ms.reviewer: efemmano
ms.lastreviewed: 11/27/2018
ms.openlocfilehash: a9fba2dc37646476ff2d802509da7b30ace85894
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58805542"
---
# <a name="region-management-in-azure-stack"></a>Azure stack'teki bölge Yönetimi

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack kavramını kullanır *bölgeleri*, Azure Stack altyapısını oluşturan donanım kaynakları oluşan mantıksal varlıkların olduğu. Bölge Yönetimi'nde, Azure Stack altyapısının başarılı biçimde çalışması için gerekli tüm kaynakları bulabilirsiniz.

Bir tümleşik sistemi dağıtımı (olarak adlandırılan bir *Azure Stack bulut*) tek bir bölgede yapar. Her Azure Stack geliştirme Seti'ni (ASDK) adlı bir bölge sahip **yerel**. İkinci bir Azure Stack tümleşik sistemi dağıtmak ya da başka bir örneğinin ayrı donanım geliştirme setinde ayarlamak, bu Azure Stack Bulutu farklı bir bölgedir.

## <a name="information-available-through-the-region-management-tile"></a>Bölge Yönetimi kutucuğu aracılığıyla kullanılabilir bilgileri

Azure Stack sahip bir dizi içinde kullanılabilen bölge yönetim özellikleri **bölge Yönetimi** Döşe. Bu kutucuk, Yönetici portalı'ndaki varsayılan Panoda Azure Stack operatörü için kullanılabilir. Bu kutucuğu aracılığıyla izleyin ve Azure Stack bölgeniz ve bölgeye özgü bileşenlerinden güncelleştirin.

![Bölge Yönetimi kutucuğu](media/azure-stack-region-management/image1.png)

Bir bölgede tıklarsanız **bölge Yönetimi** kutucuğu, aşağıdaki bilgileri erişebilirsiniz:

[![Bölge Yönetimi dikey penceresinde bölmeleri açıklamasını](media/azure-stack-region-management/regionssm.png "bölge Yönetimi dikey penceresi")](media/azure-stack-region-management/regions.png#lightbox)

1. **Kaynak menüsünde**. Belirli bir altyapı Yönetimi alanlara erişmek ve görüntüleyin ve depolama hesapları ve sanal ağlar gibi kullanıcı kaynaklarını yönetin.

2. **Uyarılar**. Sistem genelinde uyarıları listeleme ve her uyarıların ayrıntılarını sağlayın.

3. **Güncelleştirmeleri**. Geçerli sürümü, Azure Stack altyapısını, kullanılabilir güncelleştirmeleri ve güncelleştirme geçmişini görüntüleyin. Ayrıca, tümleşik sistem güncelleştirebilirsiniz.

4. **Kaynak sağlayıcıları**. Azure Stack'i çalıştırmak için gerekli bileşenler tarafından sunulan kullanıcı işlevselliği yönetin. Her kaynak sağlayıcısı, bir yönetim deneyimi ile birlikte gelir. Bu deneyim, belirli bir sağlayıcıyı, ölçümleri ve diğer yönetim özelliklerini kaynak sağlayıcıya özel uyarıları içerebilir.

5. **Altyapı rollerini**. Azure Stack'i çalıştırmak için gereken bileşenler. Uyarı raporu altyapı rolleri listelenir. Bir rolü seçerek rolü ve bu rolün çalıştığı rol örneği ile ilişkili uyarıları görüntüleyebilirsiniz.

6. **Özellikleri**. Kayıt durumu ve bölge yönetim dikey penceresindeki ortamınızın ayrıntılarını. Durum olabilir **kayıtlı** veya **kayıtlı**. Kaydettiyseniz, Azure Stack, adını ve kayıt kaynak grubunun yanı sıra kaydolmak için kullandığınız ayrıca Azure abonelik Kimliğini gösterir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack’de işlevsel durumu ve uyarıları izleme](azure-stack-monitor-health.md)
- [Azure Stack güncelleştirmelerini yönetme](azure-stack-updates.md)
