---
title: Azure uygulama ağ geçidi - Azure portalında Web uygulaması güvenlik duvarı kurallarında özelleştirme | Microsoft Docs
description: Bu makalede, Itanium tabanlı sistemler için web uygulaması güvenlik duvarı kuralları uygulama ağ geçidi, Azure portal ile özelleştirme hakkında bilgi sağlar.
documentationcenter: na
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
ms.assetid: 1159500b-17ba-41e7-88d6-b96986795084
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: ''
ms.workload: infrastructure-services
ms.date: 03/28/2017
ms.author: victorh
ms.openlocfilehash: ae61e3a8308e95c16ccde71de37fb10666ef0df9
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="customize-web-application-firewall-rules-through-the-azure-portal"></a>Web uygulaması güvenlik duvarı kuralları Azure Portalı aracılığıyla özelleştirme

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-customize-waf-rules-portal.md)
> * [PowerShell](application-gateway-customize-waf-rules-powershell.md)
> * [Azure CLI 2.0](application-gateway-customize-waf-rules-cli.md)

Azure uygulama ağ geçidi web uygulaması Güvenlik Duvarı (WAF) web uygulamaları için koruma sağlar. Bu korumalar açık Web uygulaması güvenlik proje (OWASP) çekirdek kuralı ayarlayın (CR tarafından) sağlanır. Bazı kurallar hatalı pozitif sonuç neden ve gerçek trafiği engelle. Bu nedenle, uygulama ağ geçidi Kural gruplarını ve kurallarını özelleştirme yeteneği sağlar. Özel kural gruplarını ve kurallarını hakkında daha fazla bilgi için bkz: [web uygulaması güvenlik duvarı CRS Kural gruplarını ve kurallarını listesi](application-gateway-crs-rulegroups-rules.md).

>[!NOTE]
> Uygulama ağ geçidiniz WAF katmanı kullanmıyorsa, uygulama ağ geçidi WAF katmanına yükseltme seçeneği sağ bölmede görüntülenir. 

![WAF etkinleştir][fig1]

## <a name="view-rule-groups-and-rules"></a>Görünüm kural gruplar ve kurallar

**Kural gruplarını ve kurallarını görüntülemek için**
   1. Uygulama ağ geçidi için göz atın ve ardından **Web uygulaması güvenlik duvarı**.  
   2. Seçin **Gelişmiş kural yapılandırmasını**.  
   Bu görünüm bir tablo sayfasında seçilen kuralı kümesiyle sağlanan tüm kural gruplarını gösterir. Tüm kuralın onay kutularını seçilir.

![Devre dışı kurallarını yapılandırma][1]

## <a name="search-for-rules-to-disable"></a>Arama kurallarının devre dışı bırakmak için

**Web uygulaması güvenlik duvarı ayarlarını** dikey metin arama kurallarla filtreleme özelliği sağlar. Sonuç, yalnızca kural gruplar ve aradığınız metin içeren kuralları görüntüler.

![Kuralları arayın][2]

## <a name="disable-rule-groups-and-rules"></a>Kural gruplarını ve kurallarını devre dışı bırak

Olduğunda, olduğunuz kuralları devre dışı bırakma, tüm kural grubu veya bir veya daha fazla kural grubu altında belirli kurallar devre dışı bırakabilirsiniz. 

**Kural gruplarını veya belirli kuralları devre dışı bırakmak için**

   1. Kuralları veya devre dışı bırakmak istediğiniz kuralı grupları arayın.
   2. Devre dışı bırakmak istediğiniz kuralları için onay kutularını temizleyin. 
   2. **Kaydet**’i seçin. 

![Değişiklikleri kaydet][3]

## <a name="next-steps"></a>Sonraki adımlar

Devre dışı kurallarınızı yapılandırdıktan sonra WAF günlükleri görüntülemek nasıl öğrenebilirsiniz. Daha fazla bilgi için bkz: [uygulama ağ geçidi tanılama](application-gateway-diagnostics.md#diagnostic-logging).

[fig1]: ./media/application-gateway-customize-waf-rules-portal/1.png
[1]: ./media/application-gateway-customize-waf-rules-portal/figure1.png
[2]: ./media/application-gateway-customize-waf-rules-portal/figure2.png
[3]: ./media/application-gateway-customize-waf-rules-portal/figure3.png
