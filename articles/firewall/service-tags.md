---
title: Azure güvenlik duvarı hizmet etiketleri genel bakış
description: Bu makalede, Azure güvenlik duvarı hizmet etiketleri bir genel bakıştır.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 6/27/2019
ms.author: victorh
ms.openlocfilehash: d0ac36e415c056dffc9c75d00968ff74c2156e63
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67450161"
---
# <a name="azure-firewall-service-tags"></a>Azure güvenlik duvarı hizmet etiketleri

Hizmet etiketi, güvenlik kuralı oluşturma sırasındaki karmaşıklığı en aza indirmeye yardımcı olmak için bir IP adresi ön eki grubunu temsil eder. Kendi hizmet etiketinizi oluşturamaz veya bir etiket içinde yer alacak IP adreslerini belirleyemezsiniz. Hizmet etiketine dahil olan adres ön ekleri Microsoft tarafından yönetilir ve hizmet etiketi adresler değiştikçe otomatik olarak güncelleştirilir.

Azure güvenlik duvarı hizmet etiketleri ağ kuralları hedef alanında kullanılabilir. Bunları belirli IP adreslerinin yerine kullanabilirsiniz.

## <a name="supported-service-tags"></a>Hizmet etiketleri desteklenir

Bkz: [güvenlik grupları](../virtual-network/security-overview.md#service-tags) Azure güvenlik duvarı ağ kurallarında kullanılabilir hizmet etiketleri listesi.

## <a name="next-steps"></a>Sonraki adımlar

Azure güvenlik duvarı kuralları hakkında daha fazla bilgi için bkz: [işleme mantığı Azure güvenlik duvarı kuralı](rule-processing.md).