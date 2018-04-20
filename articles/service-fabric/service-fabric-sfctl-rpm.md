---
title: Azure Service Fabric CLI sfctl rpm | Microsoft Docs
description: Service Fabric CLI sfctl rpm komutlarını açıklar.
services: service-fabric
documentationcenter: na
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 02/22/2018
ms.author: ryanwi
ms.openlocfilehash: 34e7693ea40df2bf12fd6e9be2ef627f30748bcd
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/20/2018
---
# <a name="sfctl-rpm"></a>sfctl rpm
Sorgu ve komutları onarım Yöneticisi hizmetine gönderir.

## <a name="commands"></a>Komutlar
|Komut|Açıklama|
| --- | --- |
|    onaylama zorla| Verilen onarım görevi onayını zorlar.|
|    sil       | Tamamlanan onarım görevi siler.|
|    liste         | Belirtilen filtreleri ile eşleşen onarım görevlerin bir listesi alır.|

## <a name="sfctl-rpm-delete"></a>sfctl rpm Sil
Tamamlanan onarım görevi siler.

Bu API, Service Fabric platformundan destekler; doğrudan kodunuzdan kullanılmak üzere tasarlanmamıştır. 

### <a name="arguments"></a>Bağımsız Değişkenler
|Bağımsız değişken|Açıklama|
| --- | --- |
|    --Görev kimliğini [gerekli]| Silinecek tamamlanmış onarım görevi kimliği.|
|    --Sürüm           | Onarım görevi geçerli sürüm numarası. Onarım görevi gerçek geçerli sürümü bu değerle eşleşiyorsa sıfır değilse istek yalnızca başarılı. Sıfır ise, sürüm denetimi gerçekleştirilir.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler
|Bağımsız değişken|Açıklama|
| --- | --- |
|    --hata ayıklama             | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
|    ---h Yardım           | Bu yardım iletisini ve çıkış gösterir.|
|    ---o çıktı         | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.
|    --Sorgu             | JMESPath sorgu dizesi. Bkz: http://jmespath.org/ daha fazla bilgi ve örnekler.|
|    --ayrıntılı           | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|


## <a name="sfctl-rpm-list"></a>sfctl rpm listesi
Belirtilen filtreleri ile eşleşen onarım görevlerin bir listesi alır.

Bu API, Service Fabric platformundan destekler; doğrudan kodunuzdan kullanılmak üzere tasarlanmamıştır. 

### <a name="arguments"></a>Bağımsız Değişkenler
|Bağımsız değişken|Açıklama|
| --- | --- |
|    --Yürütücü filtresi| Talep edilen görevlerini listede eklenmesi gereken onarım Yürütücü adı.|
|    --Durum Filtresi   | Sonuç listesinden bir Bitsel veya hangi görev durumlarını belirtme şu değerlerden biri bulunması gerekir. -1 - oluşturulan - 2 - talep edilen - 4 - - 8 - hazırlama onaylanmış - tamamlandı 16 - yürütme - 32 - geri yükleme - 64 -.|
|    --Görev Kimliği Filtresi | Eşleştirilecek onarım görev kimliği öneki.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler
|Bağımsız değişken|Açıklama|
| --- | --- |
|    --hata ayıklama          | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
|    ---h Yardım        | Bu yardım iletisini ve çıkış gösterir.|
|    ---o çıktı      | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan| JSON.|
|    --Sorgu          | JMESPath sorgu dizesi. Bkz: http://jmespath.org/ daha fazla bilgi ve örnekler.|
|    --ayrıntılı        | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="next-steps"></a>Sonraki adımlar
- [Ayarlanan](service-fabric-cli.md) Service Fabric CLI.
- Service Fabric CLI kullanarak kullanmayı öğrenin [örnek komutlar](/azure/service-fabric/scripts/sfctl-upgrade-application).