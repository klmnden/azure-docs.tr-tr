---
title: "Bağlantı giriş Azure Ağ İzleyicisi'ni kontrol edin | Microsoft Docs"
description: "Bu sayfa Ağ İzleyicisi bağlantı yeteneğini genel bir bakış sağlar"
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/11/2017
ms.author: jdial
ms.openlocfilehash: 16ceef9c923b6a933a5caf752991b466346e0ebc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-connectivity-check-in-azure-network-watcher"></a>Azure Ağ İzleyicisi bağlantı iade giriş

Ağ İzleyicisi'nin bağlantı özelliği bir doğrudan bir sanal makine (VM), tam etki alanı adı (FQDN) URI, TCP bağlantısı bir sanal makineden denetleme özelliği sağlar ya da IPv4 adresi. Ağ senaryoları karmaşık, ağ güvenlik grupları, güvenlik duvarları, kullanıcı tanımlı yollar ve Azure tarafından sağlanan kaynaklar kullanılarak uygulanır. Karmaşık Yapılandırmalar bağlantı sorunlarını giderme zor olun. Ağ İzleyicisi'ni bulun ve bağlantısı sorunlarını algılayacak süreyi azaltılmasına yardımcı olur. Döndürülen sonuçların bir bağlantı sorunu bir platform veya kullanıcı yapılandırma sorunu nedeniyle olup içine Öngörüler sağlayabilir. Bağlantı kontrol edileceği ile [PowerShell](network-watcher-connectivity-powershell.md), [Azure CLI](network-watcher-connectivity-cli.md), ve [REST API](network-watcher-connectivity-rest.md).

> [!IMPORTANT]
> Bağlantı onay gerektiren bir sanal makine uzantısı `AzureNetworkWatcherExtension`. Bir Windows VM uzantısı yüklemek için ziyaret [Windows için Azure Ağ İzleyicisi Aracısı sanal makine uzantısı](../virtual-machines/windows/extensions-nwa.md) ve Linux VM ziyaret edin: [Linux için Azure Ağ İzleyicisi Aracısı sanal makine uzantısı](../virtual-machines/linux/extensions-nwa.md).

## <a name="response"></a>Yanıt

Aşağıdaki tabloda bağlantı denetimi çalışmasını bitirdikten sonra döndürülen özellikleri gösterir.

|Özellik  |Açıklama  |
|---------|---------|
|ConnectionStatus     | Bağlantı denetimi durumu. Olası sonuçları **erişilebilir** ve **ulaşılamıyor**.        |
|AvgLatencyInMs     | Milisaniye cinsinden bağlantı denetimi sırasında ortalama gecikme süresi. (Yalnızca durumunu denetleme erişilebilir olup olmadığını gösterilen)        |
|MinLatencyInMs     | Bağlantı sırasında en düşük gecikme süresi, milisaniye cinsinden denetleyin. (Yalnızca durumunu denetleme erişilebilir olup olmadığını gösterilen)        |
|MaxLatencyInMs     | Bağlantı sırasında en fazla gecikme süresi, milisaniye cinsinden denetleyin. (Yalnızca durumunu denetleme erişilebilir olup olmadığını gösterilen)        |
|ProbesSent     | Denetimi sırasında gönderilen araştırmalar sayısı. En yüksek değer 100'dür.        |
|ProbesFailed     | Denetimi sırasında başarısız araştırmalar sayısı. En yüksek değer 100'dür.        |
|Atlama     | Atlama yoluyla atlama kaynaktan hedefe.        |
|[] Atlama sayısı. Türü     | Kaynak türü. Olası değerler şunlardır: **kaynak**, **değerinin VirtualAppliance**, **VnetLocal**, ve **Internet**.        |
|[] Atlama sayısı. Kimliği | Atlama benzersiz tanımlayıcısı.|
|[] Atlama sayısı. Adres | Atlamanın IP adresi.|
|[] Atlama sayısı. ResourceId | Atlama bir Azure kaynağı ise atlama ResourceId. Bu bir Internet kaynağına ResourceId ise, **Internet**. |
|[] Atlama sayısı. NextHopIds | Gerçekleştirilecek sonraki atlama benzersiz tanımlayıcısı.|
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

Bağlantı denetimi hata türleri bağlantı hakkında döndürür. Aşağıdaki tabloda, döndürülen geçerli hata türlerinin bir listesini sağlar.

|Tür  |Açıklama  |
|---------|---------|
|CPU     | Yüksek CPU kullanımı.       |
|Bellek     | Yüksek bellek kullanımı.       |
|GuestFirewall     | Trafiği bir sanal makine güvenlik duvarı yapılandırması nedeniyle engellendi.        |
|DNSResolution     | Hedef adresi için DNS çözümlemesi başarısız oldu.        |
|NetworkSecurityRule    | Trafik bir NSG kuralı tarafından engellenmiş (Kural döndürülür)        |
|UserDefinedRoute|Trafiği kullanıcı tanımlı veya sistem yolu nedeniyle bırakılır. |

### <a name="next-steps"></a>Sonraki adımlar

Bir kaynak bağlantısını ziyaret ederek doğrulamak öğrenin: [Azure Ağ İzleyicisi ile Bağlanılırlığı denetleyin](network-watcher-connectivity-powershell.md).

<!--Image references-->
[1]: ./media/network-watcher-next-hop-overview/figure1.png

