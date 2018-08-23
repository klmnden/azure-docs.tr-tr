---
title: Giriş akış günlüğe kaydetme için ağ güvenlik grupları ile Azure Ağ İzleyicisi | Microsoft Docs
description: Bu makalede, Azure Ağ İzleyicisi, NSG akış günlükleri özelliğinin nasıl kullanılacağı açıklanmaktadır.
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
ms.openlocfilehash: ae4edb82fa5e192a30d297dae82199bb7efca0c2
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2018
ms.locfileid: "42062133"
---
# <a name="introduction-to-flow-logging-for-network-security-groups"></a>Ağ güvenlik grupları için akış günlüğü giriş

Ağ güvenlik grubu (NSG) akış günlüklerini NSG üzerinden giriş ve çıkış IP trafiğini hakkındaki bilgileri görüntülemek izin veren bir Ağ İzleyicisi'nin bir özelliğidir. Akış günlüklerini json biçiminde yazılır ve giden ve gelen akış Göster Kural başına temel akışı uygulandığı ağ arabirimi (NIC), 5 demet bilgi (kaynak/hedef IP, kaynak/hedef bağlantı noktası ve protokol) akışla ilgili ve trafiği olup olmadığını izin verilen veya reddedilen.

![Akış günlüklerine genel bakış](./media/network-watcher-nsg-flow-logging-overview/figure1.png)

Hedef Nsg akış günlükleri, ancak bunlar değil aynı diğer günlükleri olarak görüntülenir. Akış günlükleri yalnızca bir depolama hesabında depolanır ve aşağıdaki örnekte gösterilen günlük yolunu izleyin:

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId=/SUBSCRIPTIONS/{subscriptionID}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/{nsgName}/y={year}/m={month}/d={day}/h={hour}/m=00/macAddress={macAddress}/PT1H.json
```

Akış günlüklerini diğer günlükler için görülen aynı bekletme ilkeleri uygulayın. 1 günden 2147483647 gün için günlük bekletme ilkesi ayarlayabilirsiniz. Bekletme ilkesi ayarlanmazsa günlükler süresiz olarak saklanır.

## <a name="log-file"></a>Günlük dosyası

Akış günlükleri aşağıdaki özellikleri içerir:

* **zaman** - olayın günlüğe yazıldığı zaman zaman
* **Systemıd** -ağ güvenlik grubu kaynak kimliği
* **Kategori** -olayın kategorisi. Her zaman kategorisidir **NetworkSecurityGroupFlowEvent**
* **ResourceId** -kaynak kimliğini nsg
* **operationName** -her zaman NetworkSecurityGroupFlowEvents
* **özellikleri** -flow özelliklerini koleksiyonu
    * **Sürüm** -akış günlüğü olay şeması sürümü sayısı
    * **Akışlar** -akış koleksiyonu. Bu özelliği farklı kuralları için birden fazla varlık içeriyor
        * **Kural** -kural akışlar listelenen
            * **Akışlar** -akış koleksiyonu
                * **Mac** -burada akış toplandı sanal makine için NIC MAC adresi
                * **flowTuples** -virgülle ayrılmış biçimde akış demet için birden çok özelliği içeren bir dize
                    * **Zaman damgası** -flow UNIX dönem biçiminde oluştuğunda zaman damgasını değerdir
                    * **Kaynak IP** -kaynak IP
                    * **Hedef IP** -hedef IP
                    * **Kaynak bağlantı noktası** -kaynak bağlantı noktası
                    * **Hedef bağlantı noktası** -' % s'hedef bağlantı noktası
                    * **Protokol** -akışın protokol. Geçerli değerler **T** TCP için ve **U** UDP
                    * **Trafik akışı** -trafik akış yönü. Geçerli değerler **miyim** gelen ve **O** için giden.
                    * **Trafik** - trafiğe izin verilen veya reddedilen olup olmadığını. Geçerli değerler **A** için izin verilen ve **D** için reddedildi.

Aşağıdaki metni, bir akış günlüğü örneğidir. Gördüğünüz gibi önceki bölümde açıklanan özellik listesi izleyen birden çok kayıt vardır.

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

- Akış günlüklerini etkinleştirme hakkında bilgi için bkz: [etkinleştirme NSG akış günlüğü](network-watcher-nsg-flow-logging-portal.md).
- NSG günlüğe kaydetme hakkında daha fazla bilgi için bkz: [Log analytics için ağ güvenlik grupları (Nsg'ler)](../virtual-network/virtual-network-nsg-manage-log.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).
- Trafiğin izin verilen veya için veya bir VM'den reddedildi olup olmadığını belirlemek için bkz: [bir VM ağ trafik filtresi sorununu tanılama](diagnose-vm-network-traffic-filtering-problem.md)
