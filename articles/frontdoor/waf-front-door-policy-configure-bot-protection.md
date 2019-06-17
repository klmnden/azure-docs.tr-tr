---
title: Azure ön kapısı (Önizleme) web uygulaması güvenlik duvarı için robot korumayı yapılandırma
description: Web uygulaması Güvenlik Duvarı (WAF) hakkında bilgi edinin.
services: frontdoor
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/31/2019
ms.author: tyao;kumud
ms.openlocfilehash: af92f3b9049862fc19c69965f979b7dfe8c62526
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66481632"
---
# <a name="configure-bot-protection-for-web-application-firewall-preview"></a>Web uygulaması Güvenlik Duvarı (Önizleme) için robot korumayı yapılandırma
Bu makalede bot koruma kuralı, Azure CLI, Azure PowerShell veya Azure Resource Manager şablonu kullanarak ön kapısı Azure web uygulaması Güvenlik Duvarı (WAF) yapılandırma gösterilmektedir.

Yönetilen bir Bot koruma kural kümesi, isteklerde bilinen kötü amaçlı IP adreslerinden özel eylemleri WAF için etkinleştirilebilir. IP adreslerini akışı Microsoft tehdit Zekasından elde edilir. [Intelligent Security Graph](https://www.microsoft.com/security/operations/intelligence) Microsoft tehdit bilgileri güçlendirir ve Azure Güvenlik Merkezi de dahil olmak üzere birden çok hizmet tarafından kullanılır.

> [!IMPORTANT]
> Bot koruma kural kümesi şu anda genel Önizleme aşamasındadır ve önizleme bir hizmet düzeyi sözleşmesi ile sağlanır. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.  Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Önkoşullar

Açıklanan yönergeleri takip ederek temel bir WAF ilkesi için ön kapı oluşturmak [WAF ilke Azure portalını kullanarak Azure ön kapısı oluşturma](waf-front-door-create-portal.md).

## <a name="enable-bot-protection-rule-set"></a>Bot koruma kural kümesini etkinleştir

1. Altında önceki bölümde oluşturduğunuz temel İlkesi sayfasında **ayarları**, tıklayın **kuralları**.
2. Ayrıntılar sayfasında altında **kuralları yönetmek** bölümünde, aşağı açılan menüden kural önünde onay kutusunu seçin **BotProtection Önizleme 0,1**ve ardından **Kaydet**yukarıda.
    
   ![Bot koruma kuralı](./media/waf-front-door-configure-bot-protection/botprotect2.png)

## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [WAF izleme](waf-front-door-monitor.md).
