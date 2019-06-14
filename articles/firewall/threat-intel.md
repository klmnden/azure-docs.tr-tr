---
title: Azure güvenlik duvarı tehdit zekası tabanlı filtreleme
description: Azure güvenlik duvarı tehdit zekası filtreleme hakkında bilgi edinin
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 3/11/2019
ms.author: victorh
ms.openlocfilehash: 4ef9089c94d9e806cc519c4f8243cdcb7e73953a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60194062"
---
# <a name="azure-firewall-threat-intelligence-based-filtering---public-preview"></a>Azure güvenlik duvarı tehdit zekası tabanlı filtreleme - genel Önizleme

Güvenlik duvarınız için tehdit bilgileri tabanlı filtrelemeyi etkinleştirerek, kötü amaçlı olduğu bilinen IP adresleri ve etki alanları ile trafik yaşanması durumunda uyarı alabilir ve trafiği reddedebilirsiniz. IP adresleri ve etki alanları, Microsoft Tehdit Bilgileri akışından alınır. [Intelligent Security Graph](https://www.microsoft.com/en-us/security/operations/intelligence) Microsoft tehdit bilgileri güçlendirir ve Azure Güvenlik Merkezi de dahil olmak üzere birden çok hizmet tarafından kullanılır.

![Güvenlik Duvarı tehdit bilgileri](media/threat-intel/firewall-threat.png)

> [!IMPORTANT]
> Tehdit zekası tabanlı filtreleme, şu anda genel Önizleme aşamasındadır ve önizleme bir hizmet düzeyi sözleşmesi ile sağlanır. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.  Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Tehdit zekası tabanlı filtreleme etkinse, ilişkili kuralların herhangi bir NAT kuralları, ağ kuralları veya uygulama kuralları önce işlenir. Önizleme sırasında yalnızca en yüksek güvenilirlik kayıtları dahil edilir.

Yalnızca bir uyarı kuralı tetiklenir veya uyarı seçin ve modu Reddet oturum seçebilirsiniz.

Varsayılan olarak, tehdit zekası tabanlı filtreleme uyarı modu etkin. Bu özelliği devre dışı bırakmak ya da portal arabirimi Bölgenizde kullanılabilir olana kadar modunu değiştirme yapılamıyor.

![Tehdit zekası tabanlı filtreleme portal arabirimi](media/threat-intel/threat-intel-ui.png)

## <a name="logs"></a>Günlükler

Aşağıdaki günlük alıntı Tetiklenmiş bir kural gösterir:

```
{
    "category": "AzureFirewallNetworkRule",
    "time": "2018-04-16T23:45:04.8295030Z",
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/AZUREFIREWALLS/{resourceName}",
    "operationName": "AzureFirewallThreatIntelLog",
    "properties": {
         "msg": "HTTP request from 10.0.0.5:54074 to somemaliciousdomain.com:80. Action: Alert. ThreatIntel: Bot Networks"
    }
}
```

## <a name="testing"></a>Test Etme

- **Giden test** -giden trafik uyarıları, nadir bir oluşumu olmalıdır, ortamınızı tehlikede anlamına gelir. Yardımcı olmak için test giden uyarıları çalıştığınız, uyarıyı tetikleyen bir test FQDN oluşturuldu. Kullanım **testmaliciousdomain.eastus.cloudapp.azure.com** giden testleriniz için.

- **Test gelen** -Güvenlik Duvarı'nı dnat'ı kuralları yapılandırıldıysa, gelen trafiği uyarıları görmek bekleyebilirsiniz. Bu, yalnızca belirli kaynakları dnat'ı kuralı üzerinde izin verilir ve trafiği Aksi takdirde engellenir bile geçerlidir. Azure Güvenlik Duvarı tüm bilinen bağlantı noktası tarayıcıları uyarmaz; Ayrıca kötü amaçlı etkinliği etkileşim kurmak amacıyla bilinen yalnızca tarayıcıları.

## <a name="next-steps"></a>Sonraki adımlar

- Bkz: [Azure güvenlik duvarı Log Analytics örnekleri](log-analytics-samples.md)
- Bilgi edinmek için nasıl [dağıtma ve bir Azure güvenlik duvarı yapılandırma](tutorial-firewall-deploy-portal.md)
- Gözden geçirme [Microsoft Security zekası raporu](https://www.microsoft.com/en-us/security/operations/security-intelligence-report)