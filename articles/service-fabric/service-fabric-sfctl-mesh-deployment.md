---
title: Azure Service Fabric CLI - sfctl kafes dağıtım | Microsoft Docs
description: Service Fabric CLI'sını sfctl kafes dağıtım komutlarını açıklamaktadır.
services: service-fabric
documentationcenter: na
author: Christina-Kang
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/06/2018
ms.author: bikang
ms.openlocfilehash: e6b484dabd77a142961db2d97242896790fa3d8b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60836969"
---
# <a name="sfctl-mesh-deployment"></a>sfctl mesh deployment
Service Fabric Mesh kaynaklar oluşturun.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| oluşturmaya | Service Fabric Mesh kaynakları dağıtımı oluşturur. |

## <a name="sfctl-mesh-deployment-create"></a>sfctl kafes dağıtımı oluşturma
Service Fabric Mesh kaynakları dağıtımı oluşturur.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Giriş-yaml-dosyaları [gerekli] | Yaml dosyaları içeren virgülle ayrılmış göreli/mutlak dosya yolları tüm yaml dosyası ya da (yinelemeli) dizininin göreli/mutlak yolu. |
| --Parametreler | Yaml dosyası ya da geçersiz kılınacak gereken parametreler içeren bir json nesnesi bir göreli/mutlak yolu. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

### <a name="examples"></a>Örnekler

Birleştirir ve yaml dosyası içinde belirtilen parametreler geçersiz kılarak küme için tüm kaynaklarını dağıtır

```
sfctl mesh deployment create --input-yaml-files ./app.yaml,./network.yaml --parameters
./param.yaml
```

Birleştirir ve bir dizindeki tüm kaynakları yaml dosyası içinde belirtilen parametreler geçersiz kılarak kümeye dağıtır.

```
sfctl mesh deployment create --input-yaml-files ./resources --parameters ./param.yaml
```

Birleştirir ve bir dizindeki tüm kaynaklar json nesnesi olarak doğrudan geçirilen parametreler geçersiz kılarak kümeye dağıtır.

```
sfctl mesh deployment create --input-yaml-files ./resources --parameters "{ 'my_param' :
{'value' : 'my_value'} }"
```


## <a name="next-steps"></a>Sonraki adımlar
- [Ayarlanan](service-fabric-cli.md) Service Fabric CLI.
- Service Fabric CLI kullanarak kullanmayı öğrenin [örnek betikleri](/azure/service-fabric/scripts/sfctl-upgrade-application).