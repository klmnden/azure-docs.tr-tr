---
title: Azure güvenlik duvarı kuralı işleme mantığı
description: Azure güvenlik duvarı kuralı işleme mantığı hakkında bilgi edinin
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 9/27/2018
ms.author: victorh
ms.openlocfilehash: 542682037a932a2e3b08c71b38b64b2694ad40f3
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47228440"
---
# <a name="azure-firewall-rule-processing-logic"></a>Azure güvenlik duvarı kuralı işleme mantığı
Azure güvenlik duvarı, NAT kuralları, ağ kuralları ve uygulamaları kuralları vardır. Kuralları kural türüne göre işlenir.


## <a name="network-rules-and-applications-rules"></a>Ağ kuralları ve uygulama kuralları 
Ağ, uygulanan ve ardından uygulama kuralları kurallardır. Kuralları sonlandırılıyor. Ağ kuralları bir eşleşme bulunursa, bu nedenle daha sonra uygulama kuralları işlenmez.  Ağ kuralı eşleşme yoksa ve paket Protokolü HTTP/HTTPS ise, paket ardından uygulama kuralları tarafından değerlendirilir. Ardından hala eşleşme bulunursa, paket altyapı kural koleksiyonunda değerlendirilir. Ardından hala eşleşme yoksa, paket varsayılan olarak reddedilir.

## <a name="nat-rules"></a>NAT kuralları
Gelen bağlantı açıklandığı gibi hedef ağ adresi çevirisi (dnat'ı) yapılandırarak etkinleştirilebilir [öğretici: Azure güvenlik duvarı Azure portalını kullanarak dnat'ı ile gelen trafiği filtreleyecek](tutorial-firewall-dnat.md). Öncelikle, dnat'ı kuralları uygulanır. Bir eşleşme bulunursa, çevrilmiş trafiğine izin verecek şekilde karşılık gelen örtük bir ağ kuralı eklenir. Bir ağ kural koleksiyonu çevrilmiş trafiği eşleşen verme kuralları ile açıkça ekleyerek bu davranışı geçersiz kılabilirsiniz. Hiçbir uygulama kuralları, bu bağlantıları için uygulanır.


## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [dağıtma ve bir Azure güvenlik duvarı yapılandırma](tutorial-firewall-deploy-portal.md).