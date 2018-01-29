---
title: "Azure Ağ İzleyicisi bağlantısı giriş sorunlarını giderme | Microsoft Docs"
description: "Bu sayfa Ağ İzleyicisi bağlantı sorunlarını giderme özelliği genel bir bakış sağlar"
services: network-watcher
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/11/2017
ms.author: jdial
ms.openlocfilehash: f8825af71620722065c03a28c93e113876c5aa71
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2018
---
# <a name="introduction-to-connection-troubleshoot-in-azure-network-watcher"></a>Giriş bağlantı sorunlarını giderme Azure Ağ İzleyicisi

Bağlantı sorunlarını giderme özelliği olan Ağ İzleyicisi bir doğrudan bir sanal makine (VM), tam etki alanı adı (FQDN) URI, TCP bağlantısı bir sanal makineden denetleme olanağı sunar veya IPv4 adresi. Ağ senaryoları karmaşık, ağ güvenlik grupları, güvenlik duvarları, kullanıcı tanımlı yollar ve Azure tarafından sağlanan kaynaklar kullanılarak uygulanır. Karmaşık Yapılandırmalar bağlantı sorunlarını giderme zor olun. Ağ İzleyicisi'ni bulun ve bağlantısı sorunlarını algılayacak süreyi azaltılmasına yardımcı olur. Döndürülen sonuçların bir bağlantı sorunu bir platform veya kullanıcı yapılandırma sorunu nedeniyle olup içine Öngörüler sağlayabilir. Bağlantı kontrol edileceği ile [PowerShell](network-watcher-connectivity-powershell.md), [Azure CLI](network-watcher-connectivity-cli.md), ve [REST API](network-watcher-connectivity-rest.md).

> [!IMPORTANT]
> Bağlantı sorunlarını giderme bir sanal makine uzantısı gerektiren `AzureNetworkWatcherExtension`. Bir Windows VM uzantısı yüklemek için ziyaret [Windows için Azure Ağ İzleyicisi Aracısı sanal makine uzantısı](../virtual-machines/windows/extensions-nwa.md) ve Linux VM ziyaret edin: [Linux için Azure Ağ İzleyicisi Aracısı sanal makine uzantısı](../virtual-machines/linux/extensions-nwa.md).

## <a name="response"></a>Yanıt

Özellikler aşağıdaki tabloda gösterilmektedir döndürülen zaman bağlantı sorunlarını giderme çalışması sona erdi.

|Özellik  |Açıklama  |
|---------|---------|
|ConnectionStatus     | Bağlantı denetimi durumu. Olası sonuçları **erişilebilir** ve **ulaşılamıyor**.        |
|AvgLatencyInMs     | Milisaniye cinsinden bağlantı denetimi sırasında ortalama gecikme süresi. (Yalnızca durumunu denetleme erişilebilir olup olmadığını gösterilen)        |
|MinLatencyInMs     | Bağlantı sırasında en düşük gecikme süresi, milisaniye cinsinden denetleyin. (Yalnızca durumunu denetleme erişilebilir olup olmadığını gösterilen)        |
|MaxLatencyInMs     | Bağlantı sırasında en fazla gecikme süresi, milisaniye cinsinden denetleyin. (Yalnızca durumunu denetleme erişilebilir olup olmadığını gösterilen)        |
|ProbesSent     | Denetimi sırasında gönderilen araştırmalar sayısı. En yüksek değer 100'dür.        |
|ProbesFailed     | Denetimi sırasında başarısız araştırmalar sayısı. En yüksek değer 100'dür.        |
|Atlama     | Atlama yoluyla atlama kaynaktan hedefe.        |
|Hops[].Type     | Kaynak türü. Olası değerler şunlardır: **kaynak**, **değerinin VirtualAppliance**, **VnetLocal**, ve **Internet**.        |
|[] Atlama sayısı. Kimliği | Atlama benzersiz tanımlayıcısı.|
|[] Atlama sayısı. Adres | Atlamanın IP adresi.|
|Hops[].ResourceId | Atlama bir Azure kaynağı ise atlama ResourceId. Bu bir Internet kaynağına ResourceId ise, **Internet**. |
|Hops[].NextHopIds | Gerçekleştirilecek sonraki atlama benzersiz tanımlayıcısı.|
|[] Atlama sayısı. Sorunları | Bu atlama denetimi sırasında karşılaşılan sorunlar koleksiyonu. Herhangi bir sorun olsaydı, boş bir değerdir.|
|[] Atlama sayısı. [] Verir. Kaynak | Sorun oluştuğu geçerli atlamada. Olası değerler şunlardır:<br/> **Gelen** -sorundur geçerli atlama önceki atlama gelen bağlantı<br/>**Giden** -sorundur sonraki atlama geçerli atlama gelen bağlantı<br/>**Yerel** -geçerli atlamada bir sorundur.|
|[] Atlama sayısı. [] Verir. Önem derecesi | Sorunun önem derecesini algılandı. Olası değerler şunlardır: **hata** ve **uyarı**. |
|[] Atlama sayısı. [] Verir. Türü |Sorun bulundu türü. Olası değerler şunlardır: <br/>**CPU**<br/>**Bellek**<br/>**GuestFirewall**<br/>**DnsResolution**<br/>**NetworkSecurityRule**<br/>**UserDefinedRoute** |
|[] Atlama sayısı. [] Verir. Bağlam |Sorun bulundu ilişkin ayrıntılar.|
|[] Atlama sayısı. [] Verir. Bağlam [] .key |Anahtar değeri çifti anahtarı döndürüldü.|
|[] Atlama sayısı. [] Verir. Bağlam [] .value |Anahtar değeri çifti değeri döndürdü.|

Atlama bulunan bir sorun örneği verilmiştir.

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

Bağlantı döndürür hata türleri hakkında bağlantı sorunlarını giderin. Aşağıdaki tabloda, döndürülen geçerli hata türlerinin bir listesini sağlar.

|Tür  |Açıklama  |
|---------|---------|
|CPU     | Yüksek CPU kullanımı.       |
|Bellek     | Yüksek bellek kullanımı.       |
|GuestFirewall     | Trafiği bir sanal makine güvenlik duvarı yapılandırması nedeniyle engellendi.        |
|DNSResolution     | Hedef adresi için DNS çözümlemesi başarısız oldu.        |
|NetworkSecurityRule    | Trafik bir NSG kuralı tarafından engellenmiş (Kural döndürülür)        |
|UserDefinedRoute|Trafiği kullanıcı tanımlı veya sistem yolu nedeniyle bırakılır. |

### <a name="next-steps"></a>Sonraki adımlar

Bağlantıları kullanarak sorun giderme öğrenin [Azure portal](network-watcher-connectivity-portal.md), [PowerShell](network-watcher-connectivity-powershell.md), [Azure CLI](network-watcher-connectivity-cli.md), veya [REST API](network-watcher-connectivity-rest.md).