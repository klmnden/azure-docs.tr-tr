---
title: Azure Service Fabric CLI sfctl deposu | Microsoft Docs
description: "Service Fabric CLI sfctl deposu komutlarını açıklar."
services: service-fabric
documentationcenter: na
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: cli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 09/22/2017
ms.author: ryanwi
ms.openlocfilehash: 2af6dff4ffcdf295731f2d61b5f9e35af40615e5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="sfctl-store"></a>sfctl deposu
Temel dosya düzeyinde küme Image store işlemleri.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
|    Sil| Görüntü deposu içeriği varolan siler.|
|    kök bilgisi| Image store kökünde içerik bilgilerini alır.|
|    STAT  | Görüntü deposu içerik bilgilerini alır.|


## <a name="sfctl-store-delete"></a>sfctl deposunu Sil
Görüntü deposu içeriği varolan siler.

Görüntü deposu içerik içinde belirtilen görüntü bulundu varolan siler göreli yol depolar. Bu komut, bunlar sağlandıktan sonra karşıya yüklenen uygulama paketleri silmek için kullanılabilir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --İçerik yolu [gerekli]| Göreli yolu dosya veya klasör Image store kök.|
| --zaman aşımı -t          | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama               | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| ---h Yardım             | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı           | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu               | JMESPath sorgu dizesi. Daha fazla bilgi ve örnekler için http://jmespath.org/ bakın.|
| --ayrıntılı             | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-store-stat"></a>sfctl deposu stat
Görüntü deposu içerik bilgilerini alır.

Image store köküne ilişkin belirtilen contentPath görüntü deposu içeriği hakkında bilgi döndürür.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --İçerik yolu [gerekli]| Göreli yolu dosya veya klasör Image store kök.|
| --zaman aşımı -t          | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama               | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| ---h Yardım             | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı           | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu               | JMESPath sorgu dizesi. Http://jmespath.org/ daha fazla bilgi ve örnekler için bkz.|
| --ayrıntılı             | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="next-steps"></a>Sonraki adımlar
- [Kurulum](service-fabric-cli.md) Service Fabric CLI.
- Service Fabric CLI kullanarak kullanmayı öğrenin [örnek komutlar](/azure/service-fabric/scripts/sfctl-upgrade-application).