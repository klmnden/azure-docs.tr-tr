---
title: Azure Service Fabric CLI - sfctl özelliği | Microsoft Docs
description: Service Fabric CLI sfctl özelliği komutlarını açıklar.
services: service-fabric
documentationcenter: na
author: Christina-Kang
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 05/23/2018
ms.author: bikang
ms.openlocfilehash: acade3d828c785af9468baa30086d3b79542f9b7
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34764032"
---
# <a name="sfctl-property"></a>sfctl özelliği
Service Fabric adları altında deposu ve sorgu özellikleri.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| sil | Belirtilen Service Fabric özelliği siler. |
| Al | Belirtilen Service Fabric özelliğini alır. |
| liste | Bir verilen ad altında tüm Service Fabric özellikleri hakkında bilgi alır. |
| PUT | Service Fabric özelliğini güncelleştirir veya oluşturur. |

## <a name="sfctl-property-delete"></a>sfctl özelliği Sil
Belirtilen Service Fabric özelliği siler.

Bir verilen ad altında belirtilen Service Fabric özelliği siler. Silinebilmesi için önce bir özellik oluşturulması gerekir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --ad kimliği [gerekli] | Service Fabric adı, olmadan ' doku\:' URI düzeni. |
| --özellik adı [gerekli] | Alınacağı özellik adını belirtir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-property-get"></a>get sfctl özelliği
Belirtilen Service Fabric özelliğini alır.

Bir verilen ad altında belirtilen Service Fabric özelliğini alır. Bu her zaman değeri ve meta verileri döndürür.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --ad kimliği [gerekli] | Service Fabric adı, olmadan ' doku\:' URI düzeni. |
| --özellik adı [gerekli] | Alınacağı özellik adını belirtir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-property-list"></a>sfctl özellik listesi
Bir verilen ad altında tüm Service Fabric özellikleri hakkında bilgi alır.

Service Fabric adı özel bilgileri depolayan bir veya daha fazla adlandırılmış özelliklere sahip olabilir. Bu işlem, bu özellikler hakkında bilgi disk belleğine alınan listesini alır. Adını, değeri ve meta veri özelliklerin her biri hakkında bilgi içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --ad kimliği [gerekli] | Service Fabric adı, olmadan ' doku\:' URI düzeni. |
| --devamlılık belirteci | Devamlılık belirteci parametresi, bir sonraki sonuç kümesi elde etmek için kullanılır. Sistem sonuçlarından tek bir yanıtta uymayan bir devamlılık belirteci boş olmayan bir değer ile API yanıt olarak dahil edilir. Bu değer geçirilen zaman sonraki API çağrısı API sonraki sonuç kümesi döndürür. Daha fazla sonuç varsa, devamlılık belirteci bir değer içermiyor. Bu parametrenin değeri, URL kodlanmış olmamalıdır. |
| --dahil değerleri | Döndürülen özelliklerin değerlerine dahil edilip edilmeyeceğini belirtebilirsiniz. Değerler meta verileriyle döndürülmelidir true; Yalnızca özellik meta verileri döndürmek için false. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-property-put"></a>sfctl özelliğini put
Service Fabric özelliğini güncelleştirir veya oluşturur.

Bir verilen ad altında belirtilen Service Fabric özelliğini güncelleştirir veya oluşturur.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --ad kimliği [gerekli] | Service Fabric adı, olmadan ' doku\:' URI düzeni. |
| --özellik adı [gerekli] | Service Fabric özelliğinin adı. |
| --Değer [gerekli] | Bir Service Fabric özellik değeri açıklar. Bir JSON dizesinde budur. <br><br> Json dizesi verileri 'tür' ve veri 'değeri' iki alan vardır. 'Tür' değeri JSON dizesinde görünmesi ilk öğe olmalıdır ve 'Binary', 'Int64', 'Çift', 'String' veya 'Guid' değerleri olabilir. Değeri serileştirmek-verilen türleri edebilirsiniz. 'Tür' ve 'Data' değerleri, dize olarak sağlanmalıdır. |
| --Özel kimliği türü | Özelliğin özel türü kimliği Bu özelliği kullanarak, kullanıcı özelliğinin değeri türü etiketi mümkün değil. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="next-steps"></a>Sonraki adımlar
- [Ayarlanan](service-fabric-cli.md) Service Fabric CLI.
- Service Fabric CLI kullanarak kullanmayı öğrenin [örnek komutlar](/azure/service-fabric/scripts/sfctl-upgrade-application).