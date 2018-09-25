---
title: Azure Application Gateway'de - Azure portal Web uygulaması güvenlik duvarı kurallarını özelleştirme | Microsoft Docs
description: Bu makalede, Azure portal ile Application Gateway içindeki web uygulaması güvenlik duvarı kurallarını özelleştirme hakkında bilgi sağlar.
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
ms.openlocfilehash: 30df26dc3a9697d3435779f91c32b2d99a747b88
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46990476"
---
# <a name="customize-web-application-firewall-rules-through-the-azure-portal"></a>Azure portalı üzerinden Web uygulaması güvenlik duvarı kurallarını özelleştirme

> [!div class="op_single_selector"]
> * [Azure portal](application-gateway-customize-waf-rules-portal.md)
> * [PowerShell](application-gateway-customize-waf-rules-powershell.md)
> * [Azure CLI](application-gateway-customize-waf-rules-cli.md)

Azure Application Gateway web uygulaması Güvenlik Duvarı (WAF), web uygulamaları için koruma sağlar. Bu korumalar, açık Web uygulaması güvenlik Project (OWASP) çekirdek kural kümesi (CRS tarafından) sağlanır. Bazı kurallar, hatalı pozitif sonuçları neden ve gerçek trafiği engelleyin. Bu nedenle, uygulama ağ geçidi kural gruplarının ve kuralların özelleştirme yeteneği sağlar. Belirli bir kural gruplarının ve kuralların hakkında daha fazla bilgi için bkz. [web uygulaması güvenlik duvarı CRS kural gruplarının ve kuralların listesi](application-gateway-crs-rulegroups-rules.md).

>[!NOTE]
> Uygulama ağ geçidi WAF katmanının kullanmıyorsa, application gateway WAF katmanına yükseltme seçeneğini sağ bölmede görünür. 

![WAF etkinleştir][fig1]

## <a name="view-rule-groups-and-rules"></a>Görünüm kural gruplarının ve kuralların

**Kural gruplarının ve kuralların görüntülemek için**
   1. Uygulama ağ geçidine gidin ve ardından **Web uygulaması güvenlik duvarı**.  
   2. Seçin **Gelişmiş kural Yapılandırması**.  
   Bu görünüm sayfasında seçtiğiniz kural kümesi ile sağlanan tüm kural gruplarının bir tablo gösterir. Kuralın onay kutularının tümünü seçilir.

![Devre dışı kurallarını yapılandırma][1]

## <a name="search-for-rules-to-disable"></a>Arama kuralları devre dışı bırakmak için

**Web uygulaması güvenlik duvarı ayarları** dikey penceresi aracılığıyla metin arama kurallarını filtreleme özelliği sağlar. Sonucu, yalnızca kural gruplarının ve aradığınız metni içeren kuralları gösterir.

![Arama kuralları][2]

## <a name="disable-rule-groups-and-rules"></a>Kural gruplarının ve kuralların devre dışı bırak

Olduğunda, yaptığınız kuralları devre dışı bırakma, tüm kural grubu ya da bir veya daha fazla kural grubu altında belirli kuralları devre dışı bırakabilirsiniz. 

**Kural gruplarının veya belirli kuralları devre dışı bırakmak için**

   1. Kuralları veya devre dışı bırakmak istediğiniz kuralı grupları arayın.
   2. Devre dışı bırakmak istediğiniz kuralları için onay kutularını temizleyin. 
   2. **Kaydet**’i seçin. 

![Değişiklikleri kaydet][3]

## <a name="next-steps"></a>Sonraki adımlar

Devre dışı kurallarınızı yapılandırdıktan sonra WAF günlükleri görüntüleme hakkında bilgi edinebilirsiniz. Daha fazla bilgi için [Application Gateway tanılama](application-gateway-diagnostics.md#diagnostic-logging).

[fig1]: ./media/application-gateway-customize-waf-rules-portal/1.png
[1]: ./media/application-gateway-customize-waf-rules-portal/figure1.png
[2]: ./media/application-gateway-customize-waf-rules-portal/figure2.png
[3]: ./media/application-gateway-customize-waf-rules-portal/figure3.png
