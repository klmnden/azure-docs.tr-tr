---
title: Azure Service Fabric CLI - sfctl choas | Microsoft Docs
description: "Service Fabric CLI sfctl chaos komutlarını açıklar."
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
ms.openlocfilehash: 336e74d8f69cb04e6bd0e85fc68ba38b218fabae
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="sfctl-chaos"></a>sfctl chaos
Hizmet başlatma, durdurma ve karmaşası Raporu sınayın.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
|    rapor| Geçilen devamlılık belirteci veya geçen zaman aralığı göre Chaos raporun sonraki kesimini alır.|
|    start | Chaos zaten kümedeki Chaos belirtilen ile çalışan başlatır çalışmıyorsa Chaos parametrelerinde.|
|    Durdur  | Durdurur Chaos zaten çalışıyorsa, kümede Aksi takdirde, hiçbir şey yapmaz.|


## <a name="sfctl-chaos-report"></a>sfctl chaos raporu
Geçilen devamlılık belirteci veya geçen zaman aralığı göre Chaos raporun sonraki kesimini alır.

Chaos rapor sonraki kesimin almak için ContinuationToken ya da belirtebilirsiniz veya StartTimeUtc ve EndTimeUtc aracılığıyla zaman aralığını belirtebilirsiniz, ancak aynı çağrısında ContinuationToken ve zaman aralığı belirtemezsiniz. 100'den fazla Chaos olayları olduğunda Chaos rapor, en fazla 100 Chaos olay kesimi içerdiği segmentlerinde döndürülür. 

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --devamlılık belirteci| Devamlılık belirteci parametresi, bir sonraki sonuç kümesi elde etmek için kullanılır. Sistem sonuçlarından tek bir yanıtta uymayan bir devamlılık belirteci boş olmayan bir değere sahip API yanıt olarak dahil edilir. Bu değer geçirilen zaman sonraki API çağrısı API sonraki sonuç kümesi döndürür. Daha fazla sonuç varsa devamlılık belirteci bir değer içermiyor. Bu parametrenin değeri, URL kodlanmış olmamalıdır.|
| --Bitiş zamanı utc   | Chaos rapor oluşturulacak zaman aralığı bitiş saati temsil eden çizgilerine sayısı. Lütfen bakın [DateTime.Ticks özelliği](https://msdn.microsoft.com/en-us/library/system.datetime.ticks%28v=vs.110%29) onay hakkında ayrıntılı bilgi için.|
| --Başlangıç zamanı utc | Chaos rapor oluşturulacak zaman aralığı başlangıç saati temsil eden çizgilerine sayısı. Lütfen bakın [DateTime.Ticks özelliği](https://msdn.microsoft.com/en-us/library/system.datetime.ticks%28v=vs.110%29) onay hakkında ayrıntılı bilgi için.|
| --zaman aşımı -t     | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama          | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| ---h Yardım        | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı      | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu          | JMESPath sorgu dizesi. Http://jmespath.org/ daha fazla bilgi ve örnekler için bkz.|
| --ayrıntılı        | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-chaos-start"></a>sfctl chaos Başlat
Chaos zaten kümedeki Chaos belirtilen ile çalışan başlatır çalışmıyorsa Chaos parametrelerinde.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --uygulama türü-sistem durumu-ilke-eşleme  | JSON listesi belirli uygulama türleri için en fazla yüzde sağlıksız uygulamalarla kodlanmış. Her girişin bir anahtar uygulama türü adı ve değeri belirtilen uygulama türünde uygulamalar değerlendirmek için kullanılan MaxPercentUnhealthyApplications yüzdesini temsil eden bir tamsayı olarak belirtir.|
| --taşıma çoğaltma hataları devre dışı bırak | Taşıma birincil devre dışı bırakır ve ikincil hataları taşıyın.|
| --en fazla küme sabitlemeyi| Tüm kararlı ve sağlam hale gelmesine varlık kümesi için beklenecek süreyi maksimum tutar.  Varsayılan: 60.|
| --en fazla eşzamanlı hataları    | Eşzamanlı hatalarının sayısı yineleme kopyaladığınızda.           Varsayılan: 1.|
| --max-yüzde-sağlıksız-uygulamalar  | Küme durumu sırasında Chaos değerlendirirken, sağlıksız uygulamaları yüzdesi hata raporlamadan önce izin verilen en fazla.|
| --max yüzde-sağlıksız-düğümler | Küme durumu sırasında Chaos değerlendirirken, sağlıksız düğümleri yüzdesi hata raporlamadan önce izin verilen en fazla.|
| --zaman çalıştırma              | Otomatik olarak durdurmadan önce Chaos çalışacağı toplam süre (saniye cinsinden). İzin verilen maksimum değer 4.294.967.295 (System.UInt32.MaxValue) ' dir.  Varsayılan: 4294967295.|
| --zaman aşımı -t               | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|
| --hataları arasındaki bekleme süresi | Tek bir yineleme içinde art arda hatalar arasında bekleme süresi (saniye cinsinden).  Varsayılan: 20.|
| --yinelemeleri arasındaki bekleme süresi| Zaman-(saniye cinsinden) arasında ayırmayı karmaşık dünyada iki ardışık yineleme.  Varsayılan: 30.|
| --hata olarak uyarı         | Küme durumu sırasında Chaos değerlendirirken, aynı önem uyarılarla hata olarak kabul eder.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama                    | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| ---h Yardım                  | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı                | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.           Varsayılan: json.|
| --Sorgu                    | JMESPath sorgu dizesi. Http://jmespath.org/ daha fazla bilgi ve örnekler için bkz.|
| --ayrıntılı                  | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-chaos-stop"></a>sfctl chaos Durdur
Durdurur Chaos zaten çalışıyorsa, kümede Aksi takdirde, hiçbir şey yapmaz.

Daha fazla hataları planlama durakları Chaos; Ancak, yürütülen hataları etkilenmez.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --zaman aşımı -t| Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama  | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| ---h Yardım| Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu  | JMESPath sorgu dizesi. Http://jmespath.org/ daha fazla bilgi ve örnekler için bkz.|
| --ayrıntılı| Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="next-steps"></a>Sonraki adımlar
- [Kurulum](service-fabric-cli.md) Service Fabric CLI.
- Service Fabric CLI kullanarak kullanmayı öğrenin [örnek komutlar](/azure/service-fabric/scripts/sfctl-upgrade-application).