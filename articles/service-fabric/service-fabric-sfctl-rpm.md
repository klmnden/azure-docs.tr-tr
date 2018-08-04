---
title: Azure Service Fabric CLI - sfctl rpm | Microsoft Docs
description: Service Fabric CLI'sını sfctl rpm komutlarını açıklamaktadır.
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
ms.date: 07/31/2018
ms.author: bikang
ms.openlocfilehash: 57a9f0516175b459723a3dcdb2e3766f0fa039c1
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2018
ms.locfileid: "39495433"
---
# <a name="sfctl-rpm"></a>sfctl rpm
Sorgulamak ve onarım Yöneticisi hizmetine komutlar gönderebilirsiniz.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| onaylama zorla | Verilen onarım görevi onayını zorlar. |
| delete | Tamamlanan onarım görevi siler. |
| liste | Belirtilen filtrelerle eşleşen onarım görevlerinin listesini alır. |

## <a name="sfctl-rpm-approve-force"></a>onaylama sfctl rpm-force
Verilen onarım görevi onayını zorlar.

Bu API, Service Fabric platform destekler. doğrudan sizin kodunuzdan kullanılmak üzere tasarlanmamıştır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Görev kimliğini [gerekli] | Onarım görevi kimliği. |
| --sürümü | Onarım görevi geçerli sürüm numarası. Bu değer onarım görevi gerçek geçerli sürümüyle eşleşen sıfır olmayan, ardından istek yalnızca başarılı olur. Ardından sıfır ise, hiçbir sürüm denetimi gerçekleştirilir. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-rpm-delete"></a>sfctl rpm Sil
Tamamlanan onarım görevi siler.

Bu API, Service Fabric platform destekler. doğrudan sizin kodunuzdan kullanılmak üzere tasarlanmamıştır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Görev kimliğini [gerekli] | Silinecek tamamlanmış onarım görevi kimliği. |
| --sürümü | Onarım görevi geçerli sürüm numarası. Bu değer onarım görevi gerçek geçerli sürümüyle eşleşen sıfır olmayan, ardından istek yalnızca başarılı olur. Ardından sıfır ise, hiçbir sürüm denetimi gerçekleştirilir. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-rpm-list"></a>sfctl rpm listesi
Belirtilen filtrelerle eşleşen onarım görevlerinin listesini alır.

Bu API, Service Fabric platform destekler. doğrudan sizin kodunuzdan kullanılmak üzere tasarlanmamıştır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Yürütücü filtresi | Talep edilen olan görevler listesinde eklenmelidir onarım Yürütücü adı. |
| --Durumu Filtresi | Sonuç listesinden bir bit düzeyinde OR hangi görev durumları belirtme aşağıdaki değerlerden eklenmelidir. <br> 1 - oluşturuldu <br>2 - talep  <br>4 - hazırlama  <br>8 - Onaylandı  <br>16 - yürütülüyor  <br>32 - geri yükleme  <br>64 - tamamlandı |
| --Görev Kimliği Filtresi | Eşleştirilecek onarım görev kimliği öneki. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |


## <a name="next-steps"></a>Sonraki adımlar
- [Ayarlanan](service-fabric-cli.md) Service Fabric CLI.
- Service Fabric CLI kullanarak kullanmayı öğrenin [örnek betikleri](/azure/service-fabric/scripts/sfctl-upgrade-application).