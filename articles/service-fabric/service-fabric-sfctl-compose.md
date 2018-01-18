---
title: "Azure Service Fabric CLI - sfctl oluştur | Microsoft Docs"
description: "Service Fabric CLI açıklar sfctl komutları oluşturun."
services: service-fabric
documentationcenter: na
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/22/2017
ms.author: ryanwi
ms.openlocfilehash: 0e35ac70125bc640114a4492498b12ea96800d42
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2018
---
# <a name="sfctl-compose"></a>sfctl oluştur
Oluşturma, silme ve Docker Compose dağıtımlarını yönetin.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
|    oluşturmaya| Service Fabric uygulaması oluşturma dosyasından dağıtın.|
|    liste  | Listesini oluşturan Service Fabric kümesi içinde oluşturulan dağıtımlar alır.|
|   kaldır| Siler var olan bir Service Fabric kümesinden dağıtım oluşturun.|
|   durum| Service Fabric alır bilgilerini dağıtım oluşturun.|
|Yükseltme       | Service Fabric kümesi oluştur dağıtımda yükseltmeyi başlatır.|
|    Yükseltme durumu| Bu Service Fabric üzerinde gerçekleştirilen en son yükseltme alır ayrıntılarını dağıtım oluşturun.|


## <a name="sfctl-compose-create"></a>sfctl oluşturan oluşturma
Service Fabric oluşturur dağıtım oluşturun.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --dosya yolu [gerekli]| Docker Compose hedef dosya yolu.|
 |   --Dağıtım adı [gerekli]| Dağıtım adı.|
|    --şifrelenmiş geçişi             | İçin bir kapsayıcı kayıt defteri parola isteyen yerine zaten şifrelenmiş bir parola kullanın.|
|    --sahip geçişi                   | Kapsayıcı kayıt için bir parola sorar.|
|    --zaman aşımı -t                 | Sunucu zaman aşımını saniye cinsinden.  Varsayılan: 60.|
 |   --kullanıcı                       | Kapsayıcı kayıt defterine bağlanmak için kullanıcı adı.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --debug                 | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| --help -h               | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı             | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu                 | JMESPath sorgu dizesi. Daha fazla bilgi ve örnekler için http://jmespath.org/ bakın.|
| --verbose               | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-compose-list"></a>sfctl listesi oluşturma
Listesini oluşturan Service Fabric kümesi içinde oluşturulan dağıtımlar alır.

Oluşturulan ya da Service Fabric kümesi oluşturuluyor sürecinde Oluştur dağıtımları hakkında durumunu alır. Yanıt adı, durum ve oluşturma dağıtım ile ilgili diğer ayrıntıları içerir. Dağıtımları bir sayfasında uymuyorsa sonuçlarının bir sayfa sonraki sayfaya almak için kullanılan bir devamlılık belirteci yanı sıra döndürülür.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --devamlılık belirteci| Devamlılık belirteci parametresi, bir sonraki sonuç kümesi elde etmek için kullanılır. Sistem sonuçlarından tek bir yanıtta uymayan bir devamlılık belirteci boş olmayan bir değere sahip API yanıt olarak dahil edilir.      Bu değer geçirilen zaman sonraki API çağrısı API sonraki sonuç kümesi döndürür. Daha fazla sonuç varsa, devamlılık belirteci bir değer içermiyor. Bu parametrenin değeri, URL kodlanmış olmamalıdır.|
| --max-results    | Disk belleğine alınan sorguları bir parçası olarak döndürülecek sonuç maksimum sayısı.      Bu parametre, döndürülen sonuç sayısı üst sınırını tanımlar.      Bunlar, yapılandırmada tanımlanan en büyük ileti boyutu kısıtlamaları göredir iletisindeki uymuyorsa döndürülen sonuçların belirtilen en fazla sonuç değerinden olabilir. Bu parametre sıfır veya belirtilmezse, disk belleğine alınan sorguları gibi çok sayıda sonuç dönüş iletiye sığmayacak mümkün olduğunca içerir.|
| --zaman aşımı -t     | Sunucu zaman aşımını saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --debug          | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| --help -h        | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı      | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu          | JMESPath sorgu dizesi. Daha fazla bilgi ve örnekler için http://jmespath.org/ bakın.|
| --verbose        | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-compose-remove"></a>sfctl oluşturan Kaldır
Siler var olan bir Service Fabric kümesinden dağıtım oluşturun.

Var olan bir Service Fabric siler dağıtım oluşturun. 

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Dağıtım adı [gerekli]| Dağıtım kimliği. Bu genellikle tam uygulamayı olmadan adıdır ' doku:' URI düzeni.|
| --zaman aşımı -t            | Sunucu zaman aşımını saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --debug                 | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| --help -h               | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı             | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu                 | JMESPath sorgu dizesi. Daha fazla bilgi ve örnekler için http://jmespath.org/ bakın.|
| --verbose               | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-compose-status"></a>sfctl durumu oluşturma
Service Fabric alır bilgilerini dağıtım oluşturun.

Durumu Oluşturuldu veya Service Fabric oluşturulan sürecinde küme ve parametre olarak belirtilen bir adı eşleşen dağıtımı oluşturan döndürür. Yanıt adı, durum ve uygulama ile ilgili diğer ayrıntıları içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Dağıtım adı [gerekli]| Dağıtım kimliği. |
| --zaman aşımı -t            | Sunucu zaman aşımını saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --debug                 | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| --help -h               | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı             | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu                 | JMESPath sorgu dizesi. Daha fazla bilgi ve örnekler için http://jmespath.org/ bakın.|
| --verbose               | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-compose-upgrade"></a>sfctl oluşturan yükseltme
Service Fabric kümesi oluştur dağıtımda yükseltmeyi başlatır.

Sağlanan yükseltme parametreleri doğrular ve dağıtım yükseltmeyi başlatır.

### <a name="arguments"></a>Bağımsız Değişkenler
|Bağımsız değişken|Açıklama|
| --- | --- |
|    --dosya yolu [gerekli]| Docker Compose hedef dosya yolu.|
|    --Dağıtım adı [gerekli]| Dağıtım adı.|
|    --Varsayılan svc-türü-sistem durumu-eşleme| JSON Hizmetleri durumunu değerlendirmek için kullanılan sistem durumu ilkesi açıklar sözlük kodlanmış.|
|    --şifrelenmiş geçişi             | İçin bir kapsayıcı kayıt defteri parola isteyen yerine zaten şifrelenmiş bir parola kullanın.|
 |   --hatası eylemi             | Olası değerler şunlardır: 'Geçersiz', 'Geri', 'Manual'.|
|    --zorla yeniden başlatma              | Yeniden başlatma.|
 |   --sahip geçişi                   | Kapsayıcı kayıt için bir parola sorar.|
|    --health-check-retry         | Sistem durumu denetimi yeniden deneme zaman aşımı, milisaniye cinsinden ölçülür.|
|    --Sistem durumu denetimi kararlı        | Sistem durumu denetimi kararlı süresini milisaniye olarak ölçülür.|
|    --health-check-wait          | Sistem durumu denetimi bekleme süresini milisaniye olarak ölçülür.|
|    --replica-set-check          | Yükseltme çoğaltma onay zaman aşımı saniye cinsinden ölçülen ayarlayın.|
|    --svc-type-health-map        | JSON kodlanmış farklı hizmet türlerinin durumunu değerlendirmek için kullanılan sistem durumu ilkeleri açıklayan nesnelerinin listesi.|
|    --zaman aşımı -t                 | Sunucu zaman aşımını saniye cinsinden.  Varsayılan: 60.|
|    --Uygulama sağlıksız              | Sağlıksız uygulamaları yüzdesi hata raporlamadan önce izin verilen en fazla.        Örneğin, %10 sağlıksız uygulamalarının izin vermek için bu değer 10 olur. Yüzdesini küme hata olarak kabul edilmeden önce sağlıksız uygulamaları maksimum toleranslı yüzdesini temsil eder. Yüzde dikkate ancak en az bir düzgün çalışmayan uygulama olduğundan, sistem durumu uyarı olarak değerlendirilir. Bu yüzde uygulama örnekleri kümedeki toplam sayısı üzerinden sağlıksız uygulamaları sayısının bölünmesiyle hesaplanır.|
|    --upgrade-domain-timeout     | Yükseltme etki alanı zaman aşımı, milisaniye cinsinden ölçülür.|
|    --upgrade-kind               | Varsayılan: alınıyor.|
|    --upgrade-mode               | Olası değerler şunlardır: 'Geçersiz', 'UnmonitoredAuto', 'UnmonitoredManual', 'İzlenen'.  Varsayılan: UnmonitoredAuto.|
|    --upgrade-timeout            | Yükseltme zaman aşımı, milisaniye cinsinden ölçülür.|
|    --kullanıcı                       | Kapsayıcı kayıt defterine bağlanmak için kullanıcı adı.|
|    --warning-as-error           | Uyarılar aynı önem derecesi hata olarak kabul edilir.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler
 |Bağımsız değişken|Açıklama|
| --- | --- |
|   --debug                      | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
|    --help -h                    | Bu yardım iletisini ve çıkış gösterir.|
 |   ---o çıktı                  | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.
                                   Varsayılan: json.|
 |   --Sorgu                      | JMESPath sorgu dizesi. Http://jmespath.org/ daha fazla bilgi ve örnekler için bkz.|
 |   --verbose                    | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="next-steps"></a>Sonraki adımlar
- [Ayarlanan](service-fabric-cli.md) Service Fabric CLI.
- Service Fabric CLI kullanarak kullanmayı öğrenin [örnek komutlar](/azure/service-fabric/scripts/sfctl-upgrade-application).