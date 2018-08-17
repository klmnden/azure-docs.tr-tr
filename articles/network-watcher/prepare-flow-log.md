---
title: Azure ağ güvenlik grubu akış günlüğü değişikliklerini | Microsoft Docs
description: Azure ağ güvenlik grubu akış günlüğü değişiklikler hakkında bilgi edinin.
services: network-watcher
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2018
ms.author: jdial
ms.openlocfilehash: b3a5ca929c83f70c31d667c2d9c811623691cff8
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/17/2018
ms.locfileid: "40180903"
---
# <a name="network-security-group-flow-logs--version-2--upcoming-changes"></a>Ağ güvenlik grubu akış günlüklerini - sürüm 2 – yaklaşan değişiklikleri

Ağ güvenlik grubu akış günlüklerini biçimi 15 Eylül 2018'den itibaren değişiyor. Eklenen alanlar bilgilerle bayt ve her bir yönde aktarılan paketler durum ve artımlı sayıları akış sağlar. Akış günlüklerini ayrıştırmanıza uygulamaları bu değişiklikten etkilenecek ve uygun şekilde güncelleştirilmesi gerekir. Bu makalede, değişikliğin detayları nelerdir özetlenmektedir.

## <a name="what-is-changing"></a>Değişen nedir?

Ağ güvenlik grubu akış günlüklerini sürümü "Sürümünden" olarak değişir: "Sürüm" 1: 2, eklenen ek alanlarla *flowTuples* günlüklerinde özelliği. Biçim hakkında ayrıntılı bilgi verilmiştir [akış sürüm 2 nasıl modellenmiştir](#how-is-a-flow-modeled-in-version-2).

### <a name="version-1-flow-tuple-format"></a>Sürüm 1 akış demet biçimi

```
"1493763938,185.170.185.105,10.2.0.4,35370,23,T,I,D"
```

### <a name="version-2-flow-tuple-format"></a>Sürüm 2 akış demet biçimi

```
"1493763938,185.170.185.105,10.2.0.4,35370,23,T,I,A,C,1,103,1,186"
```

## <a name="when-will-this-change-take-place"></a>Ne zaman bu değişikliği gerçekleşecek?

Bu değişikliğin etkili başına içine geçer **15 Eylül 2018'den** Batı Orta ABD bölgesinde. Genel bulut bölgeleri değişiklik kademeli 31 Kasım hangi yalnızca sürüm 2 günlükleri kullanıma sunulacak sonra 2018'den tarafından tamamlanır.

## <a name="am-i-affected-by-the-change"></a>Ben değişiklikten etkilenen?

Derleme veya ağ güvenlik grubu akış günlüğü verileri işleyen bir uygulama kullanırsanız bu değişiklikten etkilenen.

### <a name="if-i-use-traffic-analytics"></a>Trafik analizi kullanırsam?

Hayır. Trafik analizi, sürüm 2 akış günlüğe kaydetme sürüm 1'den geçiş sırasında etkilenen olmaz. Çok yakında geliştirmeleri sürüm 2 akış günlüklerini sağlanan ek oturumu ve bant genişliği bilgileri yararlanır.

### <a name="if-im-using-third-party-tooling"></a>Üçüncü taraf araçları kullanmaya ediyorum varsa?

Değişiklik günlüğü biçiminde önceden tümüyle iletişim kurmadığı [Ağ İzleyicisi iş ortakları](https://azure.microsoft.com/services/network-watcher). Ayrıntılar için satıcınızın ulaşın.

### <a name="if-i-have-built-a-custom-application-or-integration"></a>Tümleştirme ya da özel bir uygulama geliştirdim?

**Evet**, NSG akış günlüklerini işleminde yeni biçime uygulamanıza değiştirmeniz gerekecektir.

## <a name="what-is-the-new-flow-logging-format"></a>Yeni akış günlük biçimini nedir?

Akış günlükleri sürüm 2 akış diziler şu biçimde içerir:

```
"1493763938,185.170.185.105,10.2.0.4,35370,23,T,I,A,C,1,103,1,186".
```

Aşağıdaki tablo, özellik adları ve açıklamaları için ağ güvenlik grubu sürüm 2 akış tanımlama grubu sağlar. Tam örnek kayıtları bulunabilir [sürüm 2 örnek olay](#version2sampleevent).

| Özellik adı                        | Değer           | Açıklama                                                                                                                                                 |
| ------------------------------------ | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Zaman damgası                           | 1493763938      | Akış girişe karşılık gelen bir zaman damgası. UNIX dönem biçimi kullanılır.                                                                                       |
| Kaynak IP adresi                    | 185.170.185.105 |                                                                                                                                                             |
| Hedef IP adresi               | 10.2.0.4        |                                                                                                                                                             |
| Kaynak bağlantı noktası                          | 35370           |                                                                                                                                                             |
| Hedef bağlantı noktası                     | 23              |                                                                                                                                                             |
| Trafik Protokolü                     | T               | **T**: TCP ve **U**: UDP.                                                                                                                                  |
| Trafik akış yönü               | I               | **Ben**: gelen trafik ve **O**: giden trafiği.                                                                                                         |
| Trafik karar                     | A               | **A**: akış izin ve **D**: akış reddedildi.                                                                                                         |
| Akış durumu                           | C               | **B**: bir akış oluşturulduğunda başlayın. İstatistikleri sağlanmayan. **C**: devam eden bir akış için devam. İstatistikleri, 5 dakikalık aralıklarla sağlanır. **E**: akış sonlandırıldığında son. İstatistikleri sağlanır.                                                                                                                                                        |
| Paketler - kaynaktan hedefe      | 1               | Kaynaktan hedefe son güncelleştirmeden bu yana gönderilen TCP ve UDP paketlerinin toplam sayısı.                                                                  |
| Bayt gönderilen - kaynaktan hedefe   | 103             | Kaynaktan hedefe son güncelleştirmeden bu yana TCP ve UDP paket baytlarının toplam sayısı. Paket üst bilgisi ve yük paket bayt içerir.         |
| Gönderilen - paketleri hedef kaynağı | 1               | TCP ve UDP paketlerini hedeften kaynağa son güncelleştirmeden bu yana gönderilen toplam sayısı.                                                                  |
| Bayt gönderilen - hedef kaynağı   | 186             | TCP ve UDP paket bayt hedeften kaynağa son güncelleştirmeden bu yana gönderilen toplam sayısı. Paket üst bilgisi ve yük paket bayt içerir.             |

## <a name="how-is-a-flow-modeled-in-version-2"></a>Nasıl bir akış sürüm 2 modellenmiştir?

Sürüm 2 günlüklerinin akışı durumu tanıtır. Akış durumu *B* akış ne zaman başlatılacağını kaydedilir. Akış durumu *C* ve akış durumu *E* akışı ve akışın sonlandırma devamlılık sırasıyla işaretlemek durumlarıdır. Her ikisi de *C* ve *E* durumları trafiği bant genişliği bilgileri içerir.

**Örnek**: Akış Gelen TCP konuşma 185.170.185.105:35370 10.2.0.4:23 arasındaki diziler:

"1493763938,185.170.185.105,10.2.0.4,35370,23,T,I,A,B,," "1493695838,185.170.185.105,10.2.0.4,35370,23,T,I,A,C,1021,588096,8005,4610880" "1493696138,185.170.185.105,10.2.0.4,35370,23,T,I,A,E,52,29952,47,27072"

### <a name="how-does-the-format-differ-from-version-1"></a>Biçimi sürüm 1 ' farkı nedir?

Bir akış kurulamadı veya kurulması çalışıldı her sürüm 1'de, bir akış tanımlama grubu ["1493763938,185.170.185.105,10.2.0.4,35370,23,T,I,D"] oluşturulur (çalışması Reddet).

### <a name="how-are-bytes-and-packets-accounted-for"></a>Nasıl bayt ve paket için tüketici?

Devamlılık için *C* ve son *E* akış durumları, bayt ve paket sayıları tarihten önceki akış demet kaydın toplam sayısı. Önceki örnek konuşma başvuran, aktarılan paketlerin toplam sayısı 1021 + 52 + 8005 + 47 = 9125. Aktarılan baytların toplam sayısı 588096 + 29952 + 4610880 + 27072 = 5256000.

### <a name="if-my-application-doesnt-understand-the-traffic-bandwidth-fields-how-does-version-2-affect-the-application"></a>Nasıl Uygulamam trafiği bant genişliği alanları anlamıyor sürüm 2 uygulama etkiliyor mu?

Ağ güvenliği sürüm 1'i kullanarak uygulama sayısı benzersiz akış sayısı genellikle günlük akışı gruplandırın. Akış durumu için hesap için ayrıştırma herhangi bir değişiklik yaptıysanız, bildirilen akışlarının sayısı yanlış bir artış görebilirsiniz. Benzersiz akışlar sayım gerçekleştirilebilir akış diziler de yoksayarak *C* ve *E* akış durumları.

## <a name="can-i-control-the-version-format-i-receive"></a>Sürüm biçimi alıyorum denetleyebilirim?

Hayır. Akış günlüklerini devre dışı bırakma ve etkinleştirme, günlükleri çıkış biçimini etkilemez. Sürüm 1'den sürüm 2 günlükleri değişikliği bir bölgeye göre bölge temelinde meydana gelir. Bölge 1 sürümünden 2 sürümüne yükseltirken, her iki biçimlerde NSG akış günlükleri görebilirsiniz. Dağıtım tamamlandıktan sonra yalnızca sürüm 2 günlüklerini kullanılabilir.

## <a name="while-the-change-occurs-can-i-see-both-formats-for-the-same-network-security-group"></a>Değişiklik gerçekleşirken, aynı ağ güvenlik grubu için iki biçim görebilir miyim?

Evet. Bir bölge içinde geçiş tek başına mac adresi temelinde meydana gelir. Bir ağ güvenlik grubu için çok sayıda makineyi uygulanabilir olduğundan, depoya yazılan her iki biçimde günlükleri görebilirsiniz. Günlükleri ya da sürüm 1 veya 2 sürüm olacaktır.

## <a name="will-i-see-duplicate-data"></a>Yinelenen verileri görüyorum?

Çoğaltma akışı biçimleri arasında günlük verilerini olmayacak. Değişiklik gerçekleşirken akış sürüm 1 veya sürüm 2 biçiminde kaydedilir.

## <a name="how-can-i-test-the-new-format-ahead-of-time"></a>Yeni biçim önceden nasıl test edebilirim?

Evet. Yapabilecekleriniz [indirme](https://aka.ms/nsgflowlogsv2blobsample) çözümünüzü test etmek için kullanılacak bir örnek sürüm 2 akış günlük dosyası.

## <a name="are-there-changes-to-configuration-and-management-of-network-security-group-flow-logging"></a>Yapılandırma ve yönetim ağ güvenlik grubu akış günlüğe kaydetme, değişiklikler var mı?

Hayır. Azure depolama hesapları aynı Azure Active Directory kiracısı paylaşan Aboneliklerdeki desteği olan [yayımlanan](https://azure.microsoft.com/blog/new-azure-network-watcher-integrations-and-network-security-group-flow-logging-updates/) bu yılın başlarında. Bu değişiklik, ağ güvenlik grubu akış günlüklerini yönetirken gereken depolama hesaplarının sayısını azaltmanıza yardımcı olur.

## <a name="questions-or-feedback"></a>Sorularınız veya Geri bildiriminiz mi?

Ağ güvenlik grubu akış günlüklerini ve Ağ İzleyicisi ile ilgili deneyiminizle işitme için hazırız. Çevrimiçi veya e-postası geri bildirim veya önerilerinizi sağlayabilir AzureNetworkWatcher@microsoft.com.

## <a name="version-2-sample"></a>Sürüm 2 örneği

```json
{

"records": [

{

"time": "2018-08-03T17:00:54.5657764Z",

"systemId": "bbd48473-8ce0-4834-99bb-2e13b9b23ff8",

"category": "NetworkSecurityGroupFlowEvent",

"resourceId":
"/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FLOWLOGSV2DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/MULTITIERAPP2-NSG",

"operationName": "NetworkSecurityGroupFlowEvents",

"properties": {

"Version": 2,

"flows": [

{

"rule": "DefaultRule_AllowInternetOutBound",

"flows": [

{

"mac": "002248034CC3",

"flowTuples": [

"1533315592,10.1.1.6,204.79.197.200,62375,80,T,O,A,B,,,,",

"1533315595,10.1.1.6,204.79.197.200,62373,80,T,O,A,E,30,1784,92,123631",

"1533315597,10.1.1.6,204.79.197.200,62376,80,T,O,A,B,,,,",

"1533315601,10.1.1.6,204.79.197.200,62374,80,T,O,A,E,13,866,87,123079",

"1533315603,10.1.1.6,204.79.197.200,62377,80,T,O,A,B,,,,",

"1533315604,10.1.1.6,204.79.197.200,62375,80,T,O,A,E,33,1946,90,123247",

"1533315608,10.1.1.6,204.79.197.200,62378,80,T,O,A,B,,,,",

"1533315610,10.1.1.6,204.79.197.200,62376,80,T,O,A,E,20,1244,92,123355",

"1533315613,10.1.1.6,204.79.197.200,62379,80,T,O,A,B,,,,",

"1533315616,10.1.1.6,204.79.197.200,62377,80,T,O,A,E,24,1460,92,123355",

"1533315619,10.1.1.6,204.79.197.200,62380,80,T,O,A,B,,,,",

"1533315622,10.1.1.6,204.79.197.200,62378,80,T,O,A,E,23,1406,93,123409",

"1533315624,10.1.1.6,204.79.197.200,62381,80,T,O,A,B,,,,",

"1533315628,10.1.1.6,204.79.197.200,62379,80,T,O,A,E,16,1028,88,123133",

"1533315630,10.1.1.6,204.79.197.200,62382,80,T,O,A,B,,,,",

"1533315631,10.1.1.6,204.79.197.200,62380,80,T,O,A,E,13,866,87,123079",

"1533315635,10.1.1.6,204.79.197.200,62384,80,T,O,A,B,,,,",

"1533315637,10.1.1.6,204.79.197.200,62381,80,T,O,A,E,15,974,86,123025",

"1533315640,10.1.1.6,204.79.197.200,62385,80,T,O,A,B,,,,",

"1533315643,10.1.1.6,204.79.197.200,62382,80,T,O,A,E,16,1028,88,123139",

"1533315646,10.1.1.6,204.79.197.200,62386,80,T,O,A,B,,,,",

"1533315649,10.1.1.6,204.79.197.200,62384,80,T,O,A,E,21,1298,92,123355",

"1533315651,10.1.1.6,204.79.197.200,62387,80,T,O,A,B,,,,"

]

}

]

}

]

}

},

{

"time": "2018-08-03T17:01:54.5668880Z",

"systemId": "bbd48473-8ce0-4834-99bb-2e13b9b23ff8",

"category": "NetworkSecurityGroupFlowEvent",

"resourceId":
"/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FLOWLOGSV2DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/MULTITIERAPP2-NSG",

"operationName": "NetworkSecurityGroupFlowEvents",

"properties": {

"Version": 2,

"flows": [

{

"rule": "DefaultRule_DenyAllInBound",

"flows": [

{

"mac": "002248034CC3",

"flowTuples": [

"1533315685,80.211.83.37,10.1.1.6,45321,81,T,I,D,B,,,,"

]

}

]

},

{

"rule": "DefaultRule_AllowInternetOutBound",

"flows": [

{

"mac": "002248034CC3",

"flowTuples": [

"1533315655,10.1.1.6,204.79.197.200,62385,80,T,O,A,E,20,1244,87,123079",

"1533315657,10.1.1.6,204.79.197.200,62388,80,T,O,A,B,,,,",

"1533315658,10.1.1.6,204.79.197.200,62386,80,T,O,A,E,26,1568,92,123355",

"1533315662,10.1.1.6,204.79.197.200,62389,80,T,O,A,B,,,,",

"1533315664,10.1.1.6,204.79.197.200,62387,80,T,O,A,E,15,974,88,123133",

"1533315667,10.1.1.6,204.79.197.200,62390,80,T,O,A,B,,,,",

"1533315670,10.1.1.6,204.79.197.200,62388,80,T,O,A,E,18,1136,88,123139",

"1533315673,10.1.1.6,204.79.197.200,62391,80,T,O,A,B,,,,",

"1533315676,10.1.1.6,204.79.197.200,62389,80,T,O,A,E,14,920,87,123079",

"1533315678,10.1.1.6,204.79.197.200,62392,80,T,O,A,B,,,,",

"1533315679,10.1.1.6,204.79.197.200,62390,80,T,O,A,E,31,1838,94,123739",

"1533315684,10.1.1.6,204.79.197.200,62393,80,T,O,A,B,,,,",

"1533315685,10.1.1.6,204.79.197.200,62391,80,T,O,A,E,28,1676,101,141199",

"1533315689,10.1.1.6,204.79.197.200,62394,80,T,O,A,B,,,,",

"1533315691,10.1.1.6,204.79.197.200,62392,80,T,O,A,E,21,1298,93,123409",

"1533315694,10.1.1.6,204.79.197.200,62395,80,T,O,A,B,,,,",

"1533315697,10.1.1.6,204.79.197.200,62393,80,T,O,A,E,26,1568,91,123393",

"1533315700,10.1.1.6,204.79.197.200,62396,80,T,O,A,B,,,,",

"1533315703,10.1.1.6,204.79.197.200,62394,80,T,O,A,E,14,920,89,123187",

"1533315705,10.1.1.6,204.79.197.200,62397,80,T,O,A,B,,,,",

"1533315706,10.1.1.6,204.79.197.200,62395,80,T,O,A,E,13,866,87,123079",

"1533315711,10.1.1.6,204.79.197.200,62398,80,T,O,A,B,,,,"

]

}

]

}

]

}

}

]

}
```

## <a name="version-1-sample"></a>Sürüm 1 örneği

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

        } 

    ] 
}
```