---
title: Giriş Azure Ağ İzleyicisi bağlantı sorunlarını giderme | Microsoft Docs
description: Bu sayfa Ağ İzleyicisi bağlantı sorun giderme özelliği genel bir bakış sağlar.
services: network-watcher
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/11/2017
ms.author: kumud
ms.openlocfilehash: 4b1164c3dedc5d8a2fa02d70f66ff00afe604836
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60532468"
---
# <a name="introduction-to-connection-troubleshoot-in-azure-network-watcher"></a>Bağlantı giriş, Azure Ağ İzleyicisi sorun giderme

Bağlantı sorunlarını giderme özelliği, Ağ İzleyicisi, doğrudan TCP ile kurulan bir sanal makineden bir sanal makineye (VM), tam etki alanı adı (FQDN), URI denetleme olanağı sağlar veya IPv4 adresi. Ağ senaryoları karmaşık, ağ güvenlik grupları, güvenlik duvarları, kullanıcı tanımlı yollar ve Azure tarafından sağlanan kaynaklara uygulanır. Karmaşık yapılandırmalarla bağlantı sorunlarını giderme zor olun. Ağ İzleyicisi bağlantı sorunlarını algılanması için gereken süreyi azaltmaya yardımcı olur. Döndürülen sonuçlar bir platform veya bir kullanıcı yapılandırma sorunu nedeniyle bir bağlantı sorunu olup içine bilgiler sağlayabilir. Bağlantı işaretli ile [PowerShell](network-watcher-connectivity-powershell.md), [Azure CLI](network-watcher-connectivity-cli.md), ve [REST API](network-watcher-connectivity-rest.md).

> [!IMPORTANT]
> Bağlantı sorunlarını giderme, sorun giderme VM'ye sahip olması gerekir `AzureNetworkWatcherExtension` VM uzantısı yüklü. Bir Windows VM'de uzantıyı yüklemek için ziyaret [Windows için Azure Ağ İzleyicisi Aracısı sanal makine uzantısı](../virtual-machines/windows/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) ve Linux VM ziyaret [LinuxiçinAzureAğİzleyicisiAracısısanalmakineuzantısı](../virtual-machines/linux/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json). Hedef uç noktada bir uzantı gerekli değildir.

## <a name="response"></a>Yanıt

Aşağıdaki tabloda özelliklerini gösterir. döndürülen, bağlantı sorunlarını giderme çalışması sona erdi.

|Özellik  |Açıklama  |
|---------|---------|
|connectionStatus     | Bağlantı denetimi durumu. Olası sonuçları **erişilebilir** ve **ulaşılamıyor**.        |
|AvgLatencyInMs     | Milisaniye cinsinden bağlantı denetimi sırasında ortalama gecikme süresi. (Yalnızca denetim durumu erişilebilir olup olmadığını gösterilir)        |
|MinLatencyInMs     | Milisaniye cinsinden bağlantı denetimi sırasında en düşük gecikme süresi. (Yalnızca denetim durumu erişilebilir olup olmadığını gösterilir)        |
|MaxLatencyInMs     | Milisaniye cinsinden bağlantı denetimi sırasında en yüksek gecikme süresi. (Yalnızca denetim durumu erişilebilir olup olmadığını gösterilir)        |
|ProbesSent     | Denetimi sırasında gönderilen yoklamalar sayısı. En yüksek değer 100'dür.        |
|ProbesFailed     | Denetimi sırasında başarısız olan araştırmaları sayısı. En yüksek değer 100'dür.        |
|Atlamalar     | Atlama yoluyla atlama kaynaktan hedefe.        |
|[] Atlama sayısı. Türü     | Kaynak türü. Olası değerler **kaynak**, **VirtualAppliance**, **VnetLocal**, ve **Internet**.        |
|[] Atlama sayısı. Kimliği | Atlama benzersiz tanımlayıcısı.|
|[] Atlama sayısı. Adresi | Atlama IP adresi.|
|Hops[].ResourceId | Bir Azure kaynağı atlama ise atlama ResourceId. Bu bir Internet kaynağına ResourceId ise, **Internet**. |
|[] Atlama sayısı. NextHopIds | Alınan sonraki atlama benzersiz tanımlayıcısı.|
|[] Atlama sayısı. Sorunları | Bu atlama, denetimi sırasında karşılaşılan sorunları koleksiyonudur. Herhangi bir sorun varsa, boş bir değerdir.|
|[] Atlama sayısı. [] Verir. Kaynak | Sorunun oluştuğu geçerli atlamadan. Olası değerler şunlardır:<br/> **Gelen** -sorun, bağlantıdan önceki atlama geçerli atlama için açıktır<br/>**Giden** -sorun, sonraki atlama için geçerli bir atlama bağlantıdan açıktır<br/>**Yerel** -geçerli durakta sorunudur.|
|[] Atlama sayısı. [] Verir. Önem derecesi | Sorunun önem derecesi algılandı. Olası değerler **hata** ve **uyarı**. |
|[] Atlama sayısı. [] Verir. Türü |Sorun bulundu türü. Olası değerler şunlardır: <br/>**CPU**<br/>**Bellek**<br/>**GuestFirewall**<br/>**DnsResolution**<br/>**NetworkSecurityRule**<br/>**UserDefinedRoute** |
|[] Atlama sayısı. [] Verir. Bağlam |Ayrıntıları ilgili sorun bulundu.|
|[] Atlama sayısı. [] Verir. [] .key bağlamı |Anahtar değer çifti anahtar döndürdü.|
|[] Atlama sayısı. [] Verir. [] .value bağlamı |Anahtar değer çifti değerini döndürdü.|

Bir durakta bulunur bir sorun örneği verilmiştir.

```json
"Issues": [
    {
        "Origin": "Outbound",
        "Severity": "Error",
        "Type": "NetworkSecurityRule",
        "Context": [
            {
                "key": "RuleName",
                "value": "UserRule_Port80"
            }
        ]
    }
]
```
## <a name="fault-types"></a>Hata türleri

Bağlantı sorunlarını giderme bağlantısı ile ilgili hata türleri döndürür. Aşağıdaki tablo döndürülen geçerli hata türlerinin bir listesini sağlar.

|Tür  |Açıklama  |
|---------|---------|
|CPU     | Yüksek CPU kullanımı.       |
|Bellek     | Yüksek bellek kullanımı.       |
|GuestFirewall     | Trafiği bir sanal makine güvenlik duvarı yapılandırması nedeniyle engellendi.        |
|DNSResolution     | DNS çözümlemesi hedef adresi için başarısız oldu.        |
|NetworkSecurityRule    | Trafik, bir NSG kuralı tarafından engellenir (Kural döndürülür)        |
|UserDefinedRoute|Kullanıcı tanımlı veya sistem yolu nedeniyle trafik bırakıldı. |

### <a name="next-steps"></a>Sonraki adımlar

Kullanarak bağlantılarında sorun giderme öğrenin [Azure portalında](network-watcher-connectivity-portal.md), [PowerShell](network-watcher-connectivity-powershell.md), [Azure CLI](network-watcher-connectivity-cli.md), veya [REST API](network-watcher-connectivity-rest.md).