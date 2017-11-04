---
title: "Akış günlüğü Azure Ağ İzleyicisi ile ağ güvenlik grupları için giriş | Microsoft Docs"
description: "Bu sayfayı NSG akış günlükleri kullanımı Azure Ağ İzleyicisi, bir özellik açıklanmaktadır"
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: 47d91341-16f1-45ac-85a5-e5a640f5d59e
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: be29b993592e494053353aac1067bfb7eff90ed7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-flow-logging-for-network-security-groups"></a>Akış günlüğü ağ güvenlik grupları için giriş

Ağ güvenlik grubu akış günlükleri, giriş ve çıkış IP trafiği bir ağ güvenlik grubu ile ilgili bilgileri görüntülemek izin veren bir Ağ İzleyicisi özelliğidir. Bu akış günlükleri json biçiminde yazılır ve Kural başına temelinde, akış uygulanır, akış (kaynak/hedef IP, kaynak/hedef bağlantı noktası, Protokolü), 5-tanımlama grubu bilgilerini NIC giden ve gelen akışları gösterir ve trafiğe izin verilen veya reddedilen.

![Akış günlükleri genel bakış][1]

Hedef ağ güvenlik grupları akış günlükleri, ancak bunlar değil aynı diğer günlükler görüntülenir. Akış günlükleri, aşağıdaki örnekte gösterildiği gibi yalnızca bir depolama hesabı içinde ve günlük yolu aşağıdaki depolanır:

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId%3D/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/microsoft.network/networksecuritygroups/{nsgName}/{year}/{month}/{day}/PT1H.json
```

Diğer günlükler üzerinde görülen olarak aynı bekletme ilkeleri, akış günlüklerine uygulanır. Günlükleri günden 1 gün 365 gün olarak ayarlanabilir bir bekletme ilkesi vardır. Bekletme ilkesi ayarlanmazsa günlükler süresiz olarak saklanır.

## <a name="log-file"></a>Günlük dosyası

Akış günlükleri, birden çok özelliği vardır. Aşağıdaki listede NSG akış günlükte döndürülen özellikleri listesi bulunmaktadır:

* **zaman** - olayın günlüğe yazıldığı zaman zaman
* **SistemKimliği** -ağ güvenlik grubu kaynak kimliği
* **Kategori** -kategori olayı, bu her zaman olmaya NetworkSecurityGroupFlowEvent
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


Akış günlüğü örneği verilmiştir. Gördüğünüz gibi önceki bölümde açıklanan özellik listesi izleyin birden çok kayıt vardır. 

> [!NOTE]
> FlowTuples özelliği virgülle ayrılmış bir liste değerler.
 
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

Akış günlükleri ziyaret ederek etkinleştirmeyi öğrenin [günlüğü etkinleştirme akışı](network-watcher-nsg-flow-logging-portal.md).

NSG oturum açma hakkında bilgi edinin ziyaret ederek [günlük analizi için ağ güvenlik grupları (Nsg'ler)](../virtual-network/virtual-network-nsg-manage-log.md).

Trafik izin verilen veya bir VM'ye ziyaret ederek reddedilen varsa öğrenmek [IP akış ile doğrulama trafiği doğrulayın](network-watcher-check-ip-flow-verify-portal.md)

<!-- Image references -->
[1]: ./media/network-watcher-nsg-flow-logging-overview/figure1.png

