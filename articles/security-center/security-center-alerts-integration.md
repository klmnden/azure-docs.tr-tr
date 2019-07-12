---
title: Güvenlik Merkezi Azure güvenlik ürünleri ile tümleştirme | Microsoft Docs
description: Bu konu, Azure Güvenlik Merkezi ile tümleşik Azure güvenlik ürünleri gösterir.
services: security-center
documentationcenter: na
author: monhaber
manager: rkarlin
editor: ''
ms.assetid: ad4b0373-08ee-46ca-a573-638ed93a647c
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/02/2019
ms.author: monhaber
ms.openlocfilehash: 805c770f1a7e9bb4e0619b27ac937a2451421dc6
ms.sourcegitcommit: 1e347ed89854dca2a6180106228bfafadc07c6e5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67571744"
---
# <a name="security-center-integration-with-azure-security-products-in-asc"></a>Azure güvenlik ürünleri ASC ile Güvenlik Merkezi tümleştirmesi

Güvenlik Merkezi eklemek için ek Microsoft lisansları olan müşteriler için Güvenlik Merkezi'ni kullanarak bulgularını sunar ve birleştirilmiş bir şekilde görüntüleyebilirsiniz.

* [Azure WAF](#azure-waf)
* [Azure DDoS](#azure-ddos)

## Azure WAF <a name="azure-waf"></a>

Azure Application Gateway, web uygulamalarınız için açıklardan yararlanmaya ve güvenlik açıklarına karşı merkezi koruma sağlayan bir web uygulaması güvenlik duvarı (WAF) sunar.

Web uygulamaları tarafından yaygın olarak bilinen açıklarından kötü amaçlı saldırılar giderek hedeflenir. Application Gateway WAF, üzerinde çekirdek kural kümesi (CRS) 3.0 veya 2.2.9'daki gelen açık Web uygulaması güvenlik Project (OWASP) temel alır. WAF, gereken ek bir yapılandırma olmadan yeni güvenlik açıklarına karşı korumak için otomatik olarak güncelleştirilir. WAF tarafından oluşturulan uyarıları Güvenlik Merkezi aktarılır. WAF tarafından oluşturulan uyarıları hakkında daha fazla bilgi için bkz. Bu [makale](https://docs.microsoft.com/azure/application-gateway/application-gateway-crs-rulegroups-rules?tabs=owasp3#crs911).

## Azure DDoS <a name="azure-ddos"></a>

Dağıtılmış Hizmet engelleme (DDoS) saldırılarının yürütmek kolayca bilinir. Bu nedenle, uygulamaları buluta taşıyor müşteriler için güçlü bir güvenlik önemli hale gelmiştir. 

DDoS saldırıları, bir uygulamanın kaynaklarını tüketerek uygulamanın geçerli kullanıcılar için kullanılamaz duruma gelmesini amaçlar. DDoS saldırıları, Internet üzerinden erişilebilen herhangi bir uç noktaya hedefleyebilirsiniz.

Uygulama tasarımı, en iyi ile birlikte, azure DDoS koruması, DDoS saldırılarına karşı savunma sağlayın. Azure DDoS koruması, farklı hizmet katmanlarında sağlar. Daha fazla bilgi için [Azure DDoS koruması genel bakış](https://docs.microsoft.com/azure/virtual-network/ddos-protection-overview).

DDoS koruması standart aşağıdaki türdeki saldırıları azaltabilirsiniz:

> [!div class="mx-tableFixed"]

|Uyarı|Açıklama|
|---|---|
|**Volumetric saldırısı algılandı**|Bu saldırı 's görünüşte yasal trafiği önemli miktarda ağ katmanı doldurmak için hedeftir. Bu, UDP sel, yükseltme sel ve diğer sahte paket sel içerir. DDoS koruması standart azaltırız ve bunları Azure'nın küresel ağı ölçek ile otomatik olarak temizleme bu olası çok gigabayt saldırıları azaltır.|
|**Protokol saldırısı algılandı**|Bu tür saldırıları, zayıflıklar Katman 3 olarak ve katman 4 protokol yığınının yararlanarak bir hedef erişilemez, işleyin. İçerdiği SYN baskın saldırılarına, yansıma saldırılarını ve diğer protokol saldırıları. DDoS koruması standart bu saldırıları, istemci ile etkileşim kurma ve kötü amaçlı trafiği engelleyen, kötü amaçlı ve yasal trafiği arasında ayrım azaltır.|
|**Kaynak (uygulama) katman saldırısı algılandı**|Bu tür saldırıları, konaklar arasında veri aktarımını kesintiye web uygulama paketleri hedefleyin. HTTP protokolü ihlallerine, SQL ekleme, siteler arası betik ve diğer 7. Katman saldırıları saldırıları içerir. Bu saldırılarına karşı korumak için Azure Application Gateway web uygulaması güvenlik duvarı, DDoS koruması standardı ile kullanın. Kullanılabilir ayrıca üçüncü taraf web uygulaması güvenlik duvarı teklifleri Azure Market'te.|
