---
title: Azure güvenlik duvarı kuralı işleme mantığı
description: Azure güvenlik duvarı kuralı işleme mantığı hakkında bilgi edinin
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 9/27/2018
ms.author: victorh
ms.openlocfilehash: 12d86793c0d75413559aad77c558c4adb7ac91af
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60193654"
---
# <a name="azure-firewall-rule-processing-logic"></a>Azure güvenlik duvarı kuralı işleme mantığı
Azure güvenlik duvarı, NAT kuralları, ağ kuralları ve uygulamaları kuralları vardır. Kuralları kural türüne göre işlenir.


## <a name="network-rules-and-applications-rules"></a>Ağ kuralları ve uygulama kuralları 
Ağ, uygulanan ve ardından uygulama kuralları kurallardır. Kuralları sonlandırılıyor. Ağ kuralları bir eşleşme bulunursa, bu nedenle daha sonra uygulama kuralları işlenmez.  Ağ kuralı eşleşmesi yoksa ve paket protokolü HTTP/HTTPS ise bu durumda paket uygulama kuralları tarafından değerlendirilir. Ardından hala eşleşme bulunursa, paket altyapı kural koleksiyonunda değerlendirilir. Ardından hala eşleşme yoksa paket varsayılan olarak reddedilir.

## <a name="nat-rules"></a>NAT kuralları
Gelen bağlantı açıklandığı gibi hedef ağ adresi çevirisi (dnat'ı) yapılandırarak etkinleştirilebilir [Öğreticisi: Azure güvenlik duvarı Azure portalını kullanarak dnat'ı ile gelen trafiği filtreleyecek](tutorial-firewall-dnat.md). Öncelikle, dnat'ı kuralları uygulanır. Bir eşleşme bulunursa, çevrilmiş trafiğine izin verecek şekilde karşılık gelen örtük bir ağ kuralı eklenir. Bu davranışı, çevrilen trafikle eşleşen reddetme kuralları olan bir ağ kural koleksiyonunu açıkça ekleyerek geçersiz kılabilirsiniz. Hiçbir uygulama kuralları, bu bağlantıları için uygulanır.


## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [dağıtma ve bir Azure güvenlik duvarı yapılandırma](tutorial-firewall-deploy-portal.md).