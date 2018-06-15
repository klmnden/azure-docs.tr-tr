---
title: Giriş akışı günlüğü için ağ güvenlik grupları ile Azure Ağ İzleyicisi | Microsoft Docs
description: Bu makalede Azure Ağ İzleyicisinin NSG akış günlükleri özelliğinin nasıl kullanılacağı açıklanmaktadır.
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: 47d91341-16f1-45ac-85a5-e5a640f5d59e
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: c6a24fbca37d6aa1d775a70c708a139dfb70b813
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32182434"
---
# <a name="introduction-to-flow-logging-for-network-security-groups"></a>Akış günlüğü ağ güvenlik grupları için giriş

Ağ güvenlik grubu (NSG) akış günlükleri, bir NSG giriş ve çıkış IP trafiğinin hakkındaki bilgileri görüntülemek izin veren bir Ağ İzleyicisi özelliğidir. Akış günlükleri json biçiminde yazılır ve başına kural olarak, akış uygulandığı ağ arabirimi (NIC), 5-tanımlama grubu hakkında bilgi akışını (kaynak/hedef IP, kaynak/hedef bağlantı noktası ve protokol) giden ve gelen akışları Göster ve trafiği olup olmadığı izin verilen veya reddedilen.

![Akış günlükleri genel bakış](./media/network-watcher-nsg-flow-logging-overview/figure1.png)

Hedef Nsg'ler akış günlükleri, ancak bunlar değil aynı diğer günlükler görüntülenir. Akış günlükleri yalnızca bir depolama hesabında depolanır ve aşağıdaki örnekte gösterilen günlük yolu izleyin:

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId=/SUBSCRIPTIONS/{subscriptionID}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/{nsgName}/y={year}/m={month}/d={day}/h={hour}/m=00/macAddress={macAddress}/PT1H.json
```

Diğer günlükler için görülen aynı bekletme ilkeleri, akış günlüklerine uygulanır. 1 günden günlük bekletme ilkesi 365 gün olarak ayarlayabilirsiniz. Bekletme ilkesi ayarlanmazsa günlükler süresiz olarak saklanır.

## <a name="log-file"></a>Günlük dosyası

Akış günlükleri aşağıdaki özellikleri içerir:

* **zaman** - olayın günlüğe yazıldığı zaman zaman
* **SistemKimliği** -ağ güvenlik grubu kaynak kimliği
* **Kategori** -olay kategorisi. Her zaman kategorisidir **NetworkSecurityGroupFlowEvent**
* **ResourceId** -kaynak NSG kimliği
* **operationName** -her zaman NetworkSecurityGroupFlowEvents
* **Özellikler** -akışının özellikleri koleksiyonu
    * **Sürüm** -akış günlüğü olay şeması sürüm numarası
    * **Akışlar** -akışları koleksiyonu. Bu özelliği farklı kurallar için birden fazla varlık içeriyor
        * **Kural** -kural akışları listelenmiş
            * **Akışlar** -akışları koleksiyonu
                * **Mac** -burada akış toplandı VM için NIC MAC adresi
                * **flowTuples** -virgülle ayrılmış biçimde akış tanımlama grubu için birden çok özellik içeren bir dize
                    * **Zaman damgası** -akış UNIX dönem biçiminde oluştuğunda bu zaman damgası değerdir
                    * **Kaynak IP** -kaynak IP
                    * **Hedef IP** -hedef IP
                    * **Kaynak bağlantı noktası** -kaynak bağlantı noktası
                    * **Hedef bağlantı noktası** -hedef bağlantı noktası
                    * **Protokol** -Akış Protokolü. Geçerli değerler **T** TCP için ve **U** UDP için
                    * **Trafik akışı** -trafik akış yönü. Geçerli değerler **ı** gelen ve **O** için giden.
                    * **Trafik** - trafiğine izin verilen veya reddedilen olup olmadığını. Geçerli değerler **A** izin ve **D** reddedildi için.

Aşağıdaki akış günlüğü örneği metindir. Gördüğünüz gibi önceki bölümde açıklanan özellik listesi izleyin birden çok kayıt vardır.

> [!NOTE]
> Değerler **flowTuples* özelliği olan bir virgülle ayrılmış listesi.
 
```json
{
    "records":
    [
        
        {
             "time": "2017-02-16T22:00:32.8950000Z",
             "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434",
             "category": "NetworkSecurityGroupFlowEvent",
             "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
             "operationName": "NetworkSecurityGroupFlowEvents",
             "properties": {"Version":1,"flows":[{"rule":"DefaultRule_DenyAllInBound","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282421,42.119.146.95,10.1.0.4,51529,5358,T,I,D"]}]},{"rule":"UserRule_default-allow-rdp","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282370,163.28.66.17,10.1.0.4,61771,3389,T,I,A","1487282393,5.39.218.34,10.1.0.4,58596,3389,T,I,A","1487282393,91.224.160.154,10.1.0.4,61540,3389,T,I,A","1487282423,13.76.89.229,10.1.0.4,53163,3389,T,I,A"]}]}]}
        }
        ,
        {
             "time": "2017-02-16T22:01:32.8960000Z",
             "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434",
             "category": "NetworkSecurityGroupFlowEvent",
             "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
             "operationName": "NetworkSecurityGroupFlowEvents",
             "properties": {"Version":1,"flows":[{"rule":"DefaultRule_DenyAllInBound","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282481,195.78.210.194,10.1.0.4,53,1732,U,I,D"]}]},{"rule":"UserRule_default-allow-rdp","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282435,61.129.251.68,10.1.0.4,57776,3389,T,I,A","1487282454,84.25.174.170,10.1.0.4,59085,3389,T,I,A","1487282477,77.68.9.50,10.1.0.4,65078,3389,T,I,A"]}]}]}
        }
        ,
        {
             "time": "2017-02-16T22:02:32.9040000Z",
             "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434",
             "category": "NetworkSecurityGroupFlowEvent",
             "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
             "operationName": "NetworkSecurityGroupFlowEvents",
             "properties": {"Version":1,"flows":[{"rule":"DefaultRule_DenyAllInBound","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282492,175.182.69.29,10.1.0.4,28918,5358,T,I,D","1487282505,71.6.216.55,10.1.0.4,8080,8080,T,I,D"]}]},{"rule":"UserRule_default-allow-rdp","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282512,91.224.160.154,10.1.0.4,59046,3389,T,I,A"]}]}]}
        }
        ,
        ...
```

## <a name="next-steps"></a>Sonraki adımlar

- Akış günlükleri etkinleştirme hakkında bilgi edinmek için [NSG etkinleştirme akışı günlük](network-watcher-nsg-flow-logging-portal.md).
- NSG günlüğe kaydetme hakkında daha fazla bilgi için bkz: [günlük analizi için ağ güvenlik grupları (Nsg'ler)](../virtual-network/virtual-network-nsg-manage-log.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).
- Trafiğin izin verilen veya bir VM'den/VM'ye reddedildi olup olmadığını belirlemek için bkz: [bir VM ağ trafiği filtre sorunu tanılamak](diagnose-vm-network-traffic-filtering-problem.md)