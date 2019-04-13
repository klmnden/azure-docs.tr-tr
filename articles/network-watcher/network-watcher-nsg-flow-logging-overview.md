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
ms.openlocfilehash: 6e15149dec9fdbb7413745d36b3f6a158113b586
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2019
ms.locfileid: "59547031"
---
# <a name="introduction-to-flow-logging-for-network-security-groups"></a>Ağ güvenlik grupları için akış günlüğü giriş

Ağ güvenlik grubu (NSG) akış günlüklerini NSG üzerinden giriş ve çıkış IP trafiğini hakkındaki bilgileri görüntülemek izin veren bir Ağ İzleyicisi'nin bir özelliğidir. Akış günlüklerini JSON biçiminde yazılır ve giden Göster ve trafik varsa gelen akışlar Kural başına temelinde ağ arabirimine (NIC) akış için 5 demet bilgi (kaynak/hedef IP, kaynak/hedef bağlantı noktası ve protokol) akışla ilgili uygular. izin verilen veya reddedilen ve sürüm 2, aktarım hızı bilgilerini (bayt ve paketleri).


![Akış günlüklerine genel bakış](./media/network-watcher-nsg-flow-logging-overview/figure1.png)

Hedef Nsg akış günlükleri, ancak bunlar değil aynı diğer günlükleri olarak görüntülenir. Akış günlükleri yalnızca bir depolama hesabında depolanır ve aşağıdaki örnekte gösterilen günlük yolunu izleyin:

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId=/SUBSCRIPTIONS/{subscriptionID}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/{nsgName}/y={year}/m={month}/d={day}/h={hour}/m=00/macAddress={macAddress}/PT1H.json
```
Akış günlüklerini analiz etme ve ağ trafiğini kullanarak öngörü [trafik analizi](traffic-analytics.md).

Akış günlüklerini diğer günlükler için görülen aynı bekletme ilkeleri uygulayın. 1 günden 2147483647 gün için günlük bekletme ilkesi ayarlayabilirsiniz. Bekletme ilkesi ayarlanmazsa günlükler süresiz olarak saklanır.

> [!NOTE] 
> NSG akış günlüğü ile elde tutma ilkesi özelliği kullanarak, depolama işlemleri yüksek hacimli ve ilişkili maliyetleri neden olabilir. Elde tutma ilkesi özelliği gerekmiyorsa, bu değeri 0 olarak ayarlayın öneririz.


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
                    * **Trafiği karar** - trafiğe izin verilen veya reddedilen olup olmadığını. Geçerli değerler **A** için izin verilen ve **D** için reddedildi.
                    * **Akış durumu - yalnızca sürüm 2** -flow durumunu yakalar. Olası durumlar şunlardır **B**: Bir akış oluşturulduğunda başlar. İstatistikleri sağlanmayan. **C**: Devam eden bir akış için devam ediliyor. İstatistikleri, 5 dakikalık aralıklarla sağlanır. **E**: Bir akış sonlandırıldığında son. İstatistikleri sağlanır.
                    * **-Paket kaynağı hedefe - yalnızca sürüm 2** TCP veya UDP paketlerini kaynaktan hedefe son güncelleştirmeden bu yana gönderilen toplam sayısı.
                    * **Bayt gönderilen - kaynaktan hedefe - yalnızca sürüm 2** kaynaktan hedefe son güncelleştirmeden bu yana TCP veya UDP paket baytlarının toplam sayısı. Paket üst bilgisi ve yük paket bayt içerir.
                    * **Paketler - kaynak - yalnızca sürüm 2 için hedef** TCP veya UDP paketlerini hedeften kaynağa son güncelleştirmeden bu yana gönderilen toplam sayısı.
                    * **Bayt gönderilen - kaynak - yalnızca sürüm 2 için hedef** TCP ve UDP paket bayt hedeften kaynağa son güncelleştirmeden bu yana gönderilen toplam sayısı. Paket üst bilgisi ve yük paket bayt içerir.

## <a name="nsg-flow-logs-version-2"></a>Sürüm 2 NSG akış günlükleri

Sürüm 2 günlüklerinin akışı durumu tanıtır. Akış günlüklerini'nın hangi sürümünün aldığınız yapılandırabilirsiniz. Akış günlüklerini etkinleştirme hakkında bilgi için bkz: [etkinleştirme NSG akış günlüğü](network-watcher-nsg-flow-logging-portal.md).

Akış durumu *B* akış ne zaman başlatılacağını kaydedilir. Akış durumu *C* ve akış durumu *E* akışı ve akışın sonlandırma devamlılık sırasıyla işaretlemek durumlarıdır. Her ikisi de *C* ve *E* durumları trafiği bant genişliği bilgileri içerir.

Devamlılık için *C* ve son *E* akış durumları, bayt ve paket sayıları tarihten önceki akış demet kaydın toplam sayısı. Önceki örnek konuşma başvuran, aktarılan paketlerin toplam sayısı 1021 + 52 + 8005 + 47 = 9125. Aktarılan baytların toplam sayısı 588096 + 29952 + 4610880 + 27072 = 5256000.

**Örnek**: Bir TCP konuşma 185.170.185.105:35370 10.2.0.4:23 arasındaki gelen akış diziler:

"1493763938,185.170.185.105,10.2.0.4,35370,23,T,I,A,B,," "1493695838,185.170.185.105,10.2.0.4,35370,23,T,I,A,C,1021,588096,8005,4610880" "1493696138,185.170.185.105,10.2.0.4,35370,23,T,I,A,E,52,29952,47,27072"

Devamlılık için *C* ve son *E* akış durumları, bayt ve paket sayıları tarihten önceki akış demet kaydın toplam sayısı. Önceki örnek konuşma başvuran, aktarılan paketlerin toplam sayısı 1021 + 52 + 8005 + 47 = 9125. Aktarılan baytların toplam sayısı 588096 + 29952 + 4610880 + 27072 = 5256000.

Aşağıdaki metni, bir akış günlüğü örneğidir. Gördüğünüz gibi önceki bölümde açıklanan özellik listesi izleyen birden çok kayıt vardır.

## <a name="nsg-flow-logging-considerations"></a>NSG akış günlüğü konuları

**NSG akış günlüğü üzerinde tüm Nsg'ler bir kaynağa bağlı etkinleştirme**: Azure'da oturum akış NSG kaynağı olarak yapılandırılır. Bir akış yalnızca bir NSG kuralı ilişkilendirilir. Birden fazla Nsg burada kullanıldığı senaryolarda, NSG akış günlüğü üzerinde tüm Nsg'ler etkin olduğunu olan öneririz tüm trafiği kaydedildiğinden emin olmak için kaynak alt ağ veya ağ arabirimi uygulanır. Bkz: [trafik nasıl değerlendirilir](../virtual-network/security-overview.md#how-traffic-is-evaluated) ağ güvenlik grupları hakkında daha fazla bilgi için. 

**Akış günlüğü maliyetleri**: NSG akış günlüğü, üretilen günlükleri hacmine göre faturalandırılır. Yüksek trafik hacmi büyük akış günlük birimi ve ilişkili maliyetleri neden olabilir. NSG akış günlüğü fiyatlandırması, temel alınan depolama maliyetlerini içermez. NSG akış günlüğü ile elde tutma ilkesi özelliği kullanarak, depolama işlemleri yüksek hacimli ve ilişkili maliyetleri neden olabilir. Elde tutma ilkesi özelliği gerekmiyorsa, bu değeri 0 olarak ayarlayın öneririz. Bkz: [Ağ İzleyicisi fiyatlandırma](https://azure.microsoft.com/en-us/pricing/details/network-watcher/) ve [Azure depolama fiyatlandırması](https://azure.microsoft.com/en-us/pricing/details/storage/) ek ayrıntılar için.

**Akışları genel IP'ler olmayan VM'ler için İnternet'ten IP'ler oturum gelen**: Örnek düzeyi genel IP olarak NIC ile ilişkili bir genel IP adresi aracılığıyla atanan bir genel IP adresi yok ya da bir temel yük dengeleyici arka uç havuzu, kullanım parçası olan Vm'leri [SNAT varsayılan](../load-balancer/load-balancer-outbound-connections.md#defaultsnat) ve tarafından atanan bir IP adresi Giden bağlantıyı kolaylaştırmak için azure'ı tıklatın. Sonuç olarak, akışları için akış günlüğü girişleri görebilirsiniz akışı SNAT için atanan bağlantı noktası aralığını bağlantı noktasına yönlendirilir, internet'ten IP adresleri. Azure VM bu akışlar izin vermez ancak denemesi kaydedilir ve Ağ İzleyicisi'nin NSG akış günlüğü tasarım gereği görüntülenir. İstenmeyen gelen internet trafiğini NSG ile açıkça engellenmesi önerilir.

## <a name="sample-log-records"></a>Örnek günlük kayıtları

Aşağıdaki metni, bir akış günlüğü örneğidir. Gördüğünüz gibi önceki bölümde açıklanan özellik listesi izleyen birden çok kayıt vardır.


> [!NOTE]
> Değerler **flowTuples* özelliği olan bir virgülle ayrılmış listesi.
 
### <a name="version-1-nsg-flow-log-format-sample"></a>Sürüm 1 NSG akış günlüğü biçim örneği
```json
{
    "records": [
        {
            "time": "2017-02-16T22:00:32.8950000Z",
            "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434",
            "category": "NetworkSecurityGroupFlowEvent",
            "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
            "operationName": "NetworkSecurityGroupFlowEvents",
            "properties": {
                "Version": 1,
                "flows": [
                    {
                        "rule": "DefaultRule_DenyAllInBound",
                        "flows": [
                            {
                                "mac": "000D3AF8801A",
                                "flowTuples": [
                                    "1487282421,42.119.146.95,10.1.0.4,51529,5358,T,I,D"
                                ]
                            }
                        ]
                    },
                    {
                        "rule": "UserRule_default-allow-rdp",
                        "flows": [
                            {
                                "mac": "000D3AF8801A",
                                "flowTuples": [
                                    "1487282370,163.28.66.17,10.1.0.4,61771,3389,T,I,A",
                                    "1487282393,5.39.218.34,10.1.0.4,58596,3389,T,I,A",
                                    "1487282393,91.224.160.154,10.1.0.4,61540,3389,T,I,A",
                                    "1487282423,13.76.89.229,10.1.0.4,53163,3389,T,I,A"
                                ]
                            }
                        ]
                    }
                ]
            }
        },
        {
            "time": "2017-02-16T22:01:32.8960000Z",
            "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434",
            "category": "NetworkSecurityGroupFlowEvent",
            "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
            "operationName": "NetworkSecurityGroupFlowEvents",
            "properties": {
                "Version": 1,
                "flows": [
                    {
                        "rule": "DefaultRule_DenyAllInBound",
                        "flows": [
                            {
                                "mac": "000D3AF8801A",
                                "flowTuples": [
                                    "1487282481,195.78.210.194,10.1.0.4,53,1732,U,I,D"
                                ]
                            }
                        ]
                    },
                    {
                        "rule": "UserRule_default-allow-rdp",
                        "flows": [
                            {
                                "mac": "000D3AF8801A",
                                "flowTuples": [
                                    "1487282435,61.129.251.68,10.1.0.4,57776,3389,T,I,A",
                                    "1487282454,84.25.174.170,10.1.0.4,59085,3389,T,I,A",
                                    "1487282477,77.68.9.50,10.1.0.4,65078,3389,T,I,A"
                                ]
                            }
                        ]
                    }
                ]
            }
        },
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
### <a name="version-2-nsg-flow-log-format-sample"></a>Sürüm 2 NSG akış günlüğü biçim örneği
```json
 {
    "records": [
        {
            "time": "2018-11-13T12:00:35.3899262Z",
            "systemId": "a0fca5ce-022c-47b1-9735-89943b42f2fa",
            "category": "NetworkSecurityGroupFlowEvent",
            "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
            "operationName": "NetworkSecurityGroupFlowEvents",
            "properties": {
                "Version": 2,
                "flows": [
                    {
                        "rule": "DefaultRule_DenyAllInBound",
                        "flows": [
                            {
                                "mac": "000D3AF87856",
                                "flowTuples": [
                                    "1542110402,94.102.49.190,10.5.16.4,28746,443,U,I,D,B,,,,",
                                    "1542110424,176.119.4.10,10.5.16.4,56509,59336,T,I,D,B,,,,",
                                    "1542110432,167.99.86.8,10.5.16.4,48495,8088,T,I,D,B,,,,"
                                ]
                            }
                        ]
                    },
                    {
                        "rule": "DefaultRule_AllowInternetOutBound",
                        "flows": [
                            {
                                "mac": "000D3AF87856",
                                "flowTuples": [
                                    "1542110377,10.5.16.4,13.67.143.118,59831,443,T,O,A,B,,,,",
                                    "1542110379,10.5.16.4,13.67.143.117,59932,443,T,O,A,E,1,66,1,66",
                                    "1542110379,10.5.16.4,13.67.143.115,44931,443,T,O,A,C,30,16978,24,14008",
                                    "1542110406,10.5.16.4,40.71.12.225,59929,443,T,O,A,E,15,8489,12,7054"
                                ]
                            }
                        ]
                    }
                ]
            }
        },
        {
            "time": "2018-11-13T12:01:35.3918317Z",
            "systemId": "a0fca5ce-022c-47b1-9735-89943b42f2fa",
            "category": "NetworkSecurityGroupFlowEvent",
            "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
            "operationName": "NetworkSecurityGroupFlowEvents",
            "properties": {
                "Version": 2,
                "flows": [
                    {
                        "rule": "DefaultRule_DenyAllInBound",
                        "flows": [
                            {
                                "mac": "000D3AF87856",
                                "flowTuples": [
                                    "1542110437,125.64.94.197,10.5.16.4,59752,18264,T,I,D,B,,,,",
                                    "1542110475,80.211.72.221,10.5.16.4,37433,8088,T,I,D,B,,,,",
                                    "1542110487,46.101.199.124,10.5.16.4,60577,8088,T,I,D,B,,,,",
                                    "1542110490,176.119.4.30,10.5.16.4,57067,52801,T,I,D,B,,,,"
                                ]
                            }
                        ]
                    }
                ]
            }
        },
        ...
```

## <a name="next-steps"></a>Sonraki adımlar

- Akış günlüklerini etkinleştirme hakkında bilgi için bkz: [etkinleştirme NSG akış günlüğü](network-watcher-nsg-flow-logging-portal.md).
- Akış günlüklerini okuma hakkında bilgi almak için bkz: [NSG akış günlüklerini okuma](network-watcher-read-nsg-flow-logs.md).
- NSG günlüğe kaydetme hakkında daha fazla bilgi için bkz: [Azure İzleyici, ağ güvenlik grubu (Nsg) için günlükleri](../virtual-network/virtual-network-nsg-manage-log.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).
- Trafiğin izin verilen veya için veya bir VM'den reddedildi olup olmadığını belirlemek için bkz: [bir VM ağ trafik filtresi sorununu tanılama](diagnose-vm-network-traffic-filtering-problem.md)
