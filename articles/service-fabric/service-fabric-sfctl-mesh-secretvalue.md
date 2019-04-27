---
title: Azure Service Fabric CLI - sfctl kafes secretvalue | Microsoft Docs
description: Service Fabric CLI'sını sfctl kafes secretvalue komutlarını açıklamaktadır.
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
ms.openlocfilehash: 3f8e46f063d3e725e2174fd907169f3e0167586a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60836964"
---
# <a name="sfctl-mesh-secretvalue"></a>sfctl mesh secretvalue
Alın ve kafes secretvalue kaynakları silin.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| delete | Adlandırılmış gizli kaynağı için belirtilen değer siler. |
| list | Tüm değerlerin belirtilen gizli kaynak adlarını listeler. |
| göster | Gizli bir kaynağın belirtilen bir sürümünü değerini alın. |

## <a name="sfctl-mesh-secretvalue-delete"></a>sfctl kafes secretvalue Sil
Adlandırılmış gizli kaynağı için belirtilen değer siler.

Ada göre tanımlanan gizli değer Kaynak siler. Kaynak adı, genellikle bu değeriyle ilişkili sürümüdür. Belirtilen değer kullanılıyorsa silme işlemi başarısız olur.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Gizli-name - n [gerekli] | Gizli kaynağının adı. |
| --[gerekli] sürümü - v | Gizli dizi sürümü adı. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-mesh-secretvalue-list"></a>sfctl kafes secretvalue listesi
Tüm değerlerin belirtilen gizli kaynak adlarını listeler.

Belirtilen gizli kaynağın tüm gizli değer kaynaklar hakkında bilgi alır. Bilgi, gizli değer kaynakları, ancak gerçek değerlerin değil adlarını içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Gizli-name - n [gerekli] | Gizli kaynağının adı. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-mesh-secretvalue-show"></a>sfctl kafes secretvalue Göster
Gizli bir kaynağın belirtilen bir sürümünü değerini alın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Gizli-name - n [gerekli] | Gizli kaynağının adı. |
| --[gerekli] sürümü - v | Gizli dizi sürümü adı. |
| --değer Göster | Gizli dizi sürümü gerçek değeri gösterir. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |


## <a name="next-steps"></a>Sonraki adımlar
- [Ayarlanan](service-fabric-cli.md) Service Fabric CLI.
- Service Fabric CLI kullanarak kullanmayı öğrenin [örnek betikleri](/azure/service-fabric/scripts/sfctl-upgrade-application).