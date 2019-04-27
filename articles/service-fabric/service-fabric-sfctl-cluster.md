---
title: Azure Service Fabric CLI - sfctl küme | Microsoft Docs
description: Service Fabric CLI'sını sfctl küme komutlarını açıklamaktadır.
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
ms.openlocfilehash: 7bb399472d7e0ab14e6399fc8652d2eb132a866a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60837343"
---
# <a name="sfctl-cluster"></a>sfctl cluster
Seçin, yönetmek ve Service Fabric kümeleri çalışır.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| kod-versions | Bir Service Fabric kümesinde sağlanan kod sürümlerini fabric listesini alır. |
| config-versions | Bir Service Fabric kümesinde sağlanan yapılandırma sürümlerini fabric listesini alır. |
| sağlık | Service Fabric kümesi durumunu alır. |
| Bildirimi | Service Fabric küme bildirimi alın. |
| işlemi iptal etme | Bir kullanıcı nedenli hata işlemi iptal eder. |
| işlem listesi | Hata kullanıcı nedenli işlemleri tarafından sağlanan girişin filtrelenmiş bir listesini alır. |
| sağlama | Bir Service Fabric kümesinin kod veya yapılandırma paketleri sağlayın. |
| Sistem Kurtarma | Service Fabric kümesine çekirdek kaybına şu anda takılı kalıyor Sistem Hizmetleri, kurtarılır denemesi gösterir. |
| durumu- | Service Fabric kümesinde bir sistem durumu raporu gönderir. |
| seç | Bir Service Fabric küme uç noktasına bağlanır. |
| show-connection | Bu sfctl örneği bağlı hangi Service Fabric kümesi gösterir. |
| sağlamayı kaldırma | Kod veya yapılandırma paketleri bir Service Fabric kümesinin sağlamasını kaldırma. |
| yükselt | Service Fabric kümesi kod veya yapılandırma sürümü yükseltme başlatın. |
| Yükseltme devam et | Bir sonraki yükseltme etki alanına taşıma Küme yükseltme yapın. |
| Yükseltmeyi geri alma | Bir Service Fabric kümesini yükseltme işlemi geri alın. |
| Yükseltme durumu | Geçerli Küme yükseltmesinin ilerleme durumunu alır. |
| Yükseltme güncelleştirme | Bir Service Fabric kümesini yükseltme yükseltme parametrelerini güncelleştirin. |

## <a name="sfctl-cluster-code-versions"></a>sfctl küme kod-versions
Bir Service Fabric kümesinde sağlanan kod sürümlerini fabric listesini alır.

Kümeye sağlanan kod sürümlerini doku hakkında bilgi listesini alır. ' % S'parametre CodeVersion isteğe bağlı olarak yalnızca bu belirli sürümü çıkışı filtrelemek için kullanılabilir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --kod sürümü | Service Fabric ürün sürümü. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-cluster-config-versions"></a>sfctl küme yapılandırma-versions
Bir Service Fabric kümesinde sağlanan yapılandırma sürümlerini fabric listesini alır.

Kümeye sağlanan yapılandırma sürümlerini doku hakkında bilgi listesini alır. ConfigVersion parametresi isteğe bağlı olarak yalnızca bu belirli sürümü çıkışı filtrelemek için kullanılabilir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --config-version | Service Fabric config sürümü. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-cluster-health"></a>sfctl küme durumu
Service Fabric kümesi durumunu alır.

Sistem durumu olaylarını sistem durumuna bağlıdır küme üzerinde bildirilen koleksiyonunu filtrelemek için EventsHealthStateFilter kullanın. Benzer şekilde, NodesHealthStateFilter kullanın ve ApplicationsHealthStateFilter düğümleri ve döndürülen uygulama koleksiyonu filtrelemek için toplanan sistem durumuna bağlıdır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --applications-health-state-filter | Kendi sistem durumuna bağlıdır küme sistem durumu sorgu sonucu döndürdü uygulama sistem durumu nesnelerinin filtrelemeye izin verir. Bu parametre için olası değerler üyeleri veya HealthStateFilter numaralandırma üyeleri üzerinde bit düzeyinde işlemler elde edilen tamsayı değeri içerir. Filtreyle eşleşen uygulamaları döndürülür. Tüm uygulamalar, toplanan sistem durumunu değerlendirmek için kullanılır. Belirtilmezse, tüm girişleri döndürülür. Durum değerleri numaralandırma bayrağı tabanlı olduğundan, değer Bitsel 'Veya' işlecini kullanarak elde ettiğiniz bu değerlerin bir birleşimi olabilir. Örneğin, sağlanan değer 6 ise ardından uygulamaları sistem durumunu Tamam (2) ve (4) uyarı HealthState değeriyle döndürülür.  <br> -Default - varsayılan değer. Tüm HealthState eşleşir. Değer sıfırdır.  <br> -Hiçbiri - herhangi bir HealthState değer eşleşmeyen filtreleyin. Belirli bir koleksiyon durumlarının sonuç döndürmek için kullanılır. Değer 1'dir.  <br> -Tamam - eşleşme HealthState değeriyle Tamam giriş filtreleyin. Değeri 2'dir.  <br> -Uyarı - filtre HealthState girişle eşleşir uyarı değeri. Değer 4'tür.  <br> -Hata - giriş hatası HealthState değeri ile eşleşen filtre. Değer 8'dir.  <br> -All - giriş herhangi bir HealthState değeri ile eşleşen filtreleyin. Değer 65535'tir. |
| --events-health-state-filter | Döndürülen sistem durumu olayı nesnelerinin koleksiyonunu sistem durumuna göre filtrelemeye olanak tanır. Bu parametre için olası değerler aşağıdaki sistem durumlarının bir tamsayı değeri içerir. Yalnızca filtreyle eşleşen olaylar döndürülür. Tüm olaylar, toplanan sistem durumunu değerlendirmek için kullanılır. Belirtilmezse, tüm girişleri döndürülür. Durum değerleri numaralandırma bayrağı tabanlı olduğundan, değer Bitsel 'Veya' işlecini kullanarak elde ettiğiniz bu değerlerin bir birleşimi olabilir. 6 sağlanan değer, örneğin, ardından tüm olayları Tamam (2) ve (4) uyarı HealthState değeriyle döndürülür.  <br> -Default - varsayılan değer. Tüm HealthState eşleşir. Değer sıfırdır.  <br> -Hiçbiri - herhangi bir HealthState değer eşleşmeyen filtreleyin. Belirli bir koleksiyon durumlarının sonuç döndürmek için kullanılır. Değer 1'dir.  <br> -Tamam - eşleşme HealthState değeriyle Tamam giriş filtreleyin. Değeri 2'dir.  <br> -Uyarı - filtre HealthState girişle eşleşir uyarı değeri. Değer 4'tür.  <br> -Hata - giriş hatası HealthState değeri ile eşleşen filtre. Değer 8'dir.  <br> -All - giriş herhangi bir HealthState değeri ile eşleşen filtreleyin. Değer 65535'tir. |
| --exclude sağlık istatistikleri | Sistem durumu istatistikleri sorgu sonucu bir parçası olarak döndürülüp döndürülmeyeceğini gösterir. Varsayılan değer false. İstatistik alt öğe sayısı durumunun Tamam, uyarı ve hata varlıkları gösterir. |
| --dahil-sistem-uygulaması-sistem durumu-statistics | Doku sistem durumu istatistiklerini içerip içermeyeceğini belirten \: /sistem uygulaması sistem durumu istatistikleri. Varsayılan değer false. Includesystemapplicationhealthstatistics ayarlanmışsa true olarak sistem istatistikleri dokuya ait varlıklar dahil \: /sistem uygulaması. Aksi takdirde, yalnızca kullanıcı uygulamaları için sistem durumu istatistikleri sorgu sonucunu içerir. Uygulanacak bu parametre için sorgu sonucundaki durumu istatistiklerini eklenmesi gerekir. |
| --düğümler sağlık Durumu Filtresi | Kendi sistem durumuna bağlıdır küme sistem durumu sorgu sonucu döndürdü düğümü sağlık durumu nesnelerin filtrelemeye izin verir. Bu parametre için olası değerler aşağıdaki sistem durumlarının bir tamsayı değeri içerir. Filtreyle eşleşen düğümleri döndürülür. Tüm düğümleri toplanan sistem durumunu değerlendirmek için kullanılır. Belirtilmezse, tüm girişleri döndürülür. Durum değerleri numaralandırma bayrağı tabanlı olduğundan, değer Bitsel 'Veya' işlecini kullanarak elde ettiğiniz bu değerlerin bir birleşimi olabilir. Örneğin, sağlanan değer 6 ise ardından düğümlerinin sistem durumunu Tamam (2) ve (4) uyarı HealthState değeriyle döndürülür.  <br> -Default - varsayılan değer. Tüm HealthState eşleşir. Değer sıfırdır.  <br> -Hiçbiri - herhangi bir HealthState değer eşleşmeyen filtreleyin. Belirli bir koleksiyon durumlarının sonuç döndürmek için kullanılır. Değer 1'dir.  <br> -Tamam - eşleşme HealthState değeriyle Tamam giriş filtreleyin. Değeri 2'dir.  <br> -Uyarı - filtre HealthState girişle eşleşir uyarı değeri. Değer 4'tür.  <br> -Hata - giriş hatası HealthState değeri ile eşleşen filtre. Değer 8'dir.  <br> -All - giriş herhangi bir HealthState değeri ile eşleşen filtreleyin. Değer 65535'tir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-cluster-manifest"></a>sfctl küme bildirimi
Service Fabric küme bildirimi alın.

Service Fabric küme bildirimi alın. Küme bildiriminde farklı bir düğüme türlerinin küme, güvenlik yapılandırmalarını, hata ve yükseltme etki alanı topolojiler, vb. içeren küme özelliklerini içerir. Bu özellikler, tek başına Küme dağıtımı sırasında ClusterConfig.JSON dosyasının bir parçası olarak belirtilir. Ancak, küme bildiriminde ilgili bilgilerin çoğunu oluşturulan dahili olarak service fabric tarafından diğer dağıtım senaryolarında Küme dağıtımı sırasında (örneğin, Azure portalını kullanarak). İçerikleri gibi küme bildiriminin yalnızca bilgilendirme amaçlıdır ve kullanıcıların dosya içeriğini veya bir yorumlamasını biçimi üzerinde bir bağımlılık olması beklenmez.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-cluster-operation-cancel"></a>sfctl küme işlemi iptal etme
Bir kullanıcı nedenli hata işlemi iptal eder.

Aşağıdaki API'leri CancelOperation kullanarak iptal edilebilir hata operations Başlat\: StartDataLoss, StartQuorumLoss, StartPartitionRestart, StartNodeTransition. Zorla false ise, ardından belirtilen kullanıcı nedenli işlemi düzgün biçimde durdurulacak ve temizlenir.  Zorla true ise, komutu iptal edilecek ve bazı iç durumu bırakılmış olabilir.  Zorla true belirten dikkatli kullanılmalıdır. Zorla true olarak ayarlanmış bu API'yi çağıran bu API zaten false öncelikle zorla kümesiyle aynı test komutunda çağrılana kadar veya test komutu, bir OperationState OperationState.RollingBack olmadıkça izin verilmez. 

Açıklama\: sistem olacaktır/iç sisteminizi temizleme OperationState.RollingBack anlamına gelir durumuna neden komutu yürüterek.  Test komutu veri kaybına neden olursa, verileri geri yüklemez.  StartDataLoss çağırın, sonra bu API çağrısı, örneğin, sistem yalnızca dahili durumdan çalıştırarak temizleyin. Komutu, veri kaybına neden için yeteri kadar progressed, hedef bölümün veri geri yüklemez. 

> [!NOTE]
> Bu API ile zorla çağrılırsa == true, iç durumu geride.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --işlem kimliği [gerekli] | Bu API çağrısının tanımlayan bir GUID.  Bu, ilgili GetProgress API geçirilir. |
| --force | Düzgün bir şekilde geri alma ve kullanıcı nedenli işlemi yürüterek değiştirilmiş iç sistem durumu temizlemek görüntülenip görüntülenmeyeceğini gösterir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-cluster-operation-list"></a>sfctl küme işlemi-listesi
Hata kullanıcı nedenli işlemleri tarafından sağlanan girişin filtrelenmiş bir listesini alır.

Tarafından sağlanan girişin filtrelenmiş hata kullanıcı nedenli işlemlerin listesini alır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --state-filter | Kullanıcı nedenli işlemlerinde OperationState üzerinde ait filtrelemek için kullanılır. <br> 65535 - Tümünü Seç <br> 1 - çalışan seçin <br> 2 - RollingBack seçin <br>8 - tamamlandı seçin <br>16 - hatalı seçin <br>32 - iptal edildi seçin <br>64 - ForceCancelled seçin.  <br>Varsayılan\: 65535. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |
| --type-filter | Kullanıcı nedenli işlemlerinde üzerinde OperationType filtrelemek için kullanılır. <br> 65535 - Tümünü Seç <br> 1 - PartitionDataLoss seçin. <br> 2 - PartitionQuorumLoss seçin. <br> 4 - PartitionRestart seçin. <br> 8 - NodeTransition seçin.  <br> Varsayılan\: 65535. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-cluster-provision"></a>sfctl küme sağlama
Bir Service Fabric kümesinin kod veya yapılandırma paketleri sağlayın.

Bir Service Fabric kümesinin kod veya yapılandırma paketleri sağlayın ve doğrulayın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --cluster-manifest-file-path | Küme bildirimi dosyasının yolu. |
| --kod dosya yolu | Küme kod paket dosyası yolu. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-cluster-recover-system"></a>sfctl küme kurtarma sistemi
Service Fabric kümesine çekirdek kaybına şu anda takılı kalıyor Sistem Hizmetleri, kurtarılır denemesi gösterir.

Service Fabric kümesine çekirdek kaybına şu anda takılı kalıyor Sistem Hizmetleri, kurtarılır denemesi gösterir. Bu işlemi yalnızca kapalı çoğaltmaları kurtarılamaz biliniyorsa gerçekleştirilmelidir. Bu API'nin yanlış kullanımı, olası veri kaybına neden olabilir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-cluster-report-health"></a>sfctl küme durumu-
Service Fabric kümesinde bir sistem durumu raporu gönderir.

Rapor üzerinde bildirilen özellik ve sistem durumu raporu kaynağı hakkındaki bilgileri içermelidir. Rapor sistem durumu deposu için ileten bir Service Fabric ağ geçidi düğümü gönderilir. Rapor ağ geçidi tarafından kabul edilen, ancak sistem durumu deposu tarafından ek doğrulama sonrasında reddedilen. Örneğin, sistem durumu deposu raporu eski sıra numarası gibi geçersiz bir parametre nedeniyle reddedebilir. Health store içinde rapor uygulanıp uygulanmadığını görmek için raporu kümenin HealthEvents içinde görünür denetleyin.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --[gerekli] sistem durumu özelliği | Sistem durumu bilgileri özelliği. <br><br> Bir varlık sistem durumu raporlarının farklı özellikler için sahip olabilir. , Bir dize ve rapor tetikleyen durumu koşulu kategorilere ayırmak muhabir esnekliğini tanımak için olmayan bir sabit numaralandırma özelliğidir. Örneğin, "AvailableDisk" özelliği, düğüm üzerinde rapor için bir Raporlayıcı SourceId "LocalWatchdog" ile bir düğümde, kullanılabilir disk durumunu izleyebilirsiniz. Bu özellik "Bağlantı" aynı düğümde raporlamak için aynı muhabir düğüm bağlantısı izleyebilirsiniz. Health store içinde bu raporları belirtilen düğüm için ayrı bir sistem durumu olayları olarak kabul edilir. SourceId birlikte özelliği sistem durumu bilgileri benzersiz olarak tanımlar. |
| --[gerekli] sistem durumu | Olası değerler şunlardır\: 'Geçersiz', 'Tamam', 'Warning', 'Error', 'Bilinmeyen'. |
| --Kaynak Kimliği [gerekli] | Kaynak adı, sistem durumu bilgileri oluşturulan izleme/istemci/sistem bileşeni belirtir. |
| --açıklaması | Sistem durumu bilgileri açıklaması. <br><br> Bu, insan tarafından okunabilir rapor bilgilerini eklemek için kullanılan serbest metin temsil eder. Açıklama maksimum dize uzunluğu 4096 karakter olabilir. Sağlanan dize uzun olduğunda otomatik olarak kesilir. Kesirli kısmı, bir işaretçi "[kesildi]" açıklama son karakterleri içeren ve toplam dize boyutu 4096 karakter. İşaretleyici varlığı kullanıcılara bu kesme gösterir oluştu. Kesirli kısmı, açıklama orijinal dizeden küçüktür 4096 karakter olduğuna dikkat edin. |
| --hemen | Raporun hemen gönderilmesi gerekip gerekmediğini gösteren bir bayrak. <br><br> Sistem Durumu raporu, Service Fabric için sistem durumu deposu ileten uygulama ağ geçidi için gönderilir. Hemen ayarlanmışsa true, raporun hemen sistem durumu deposu, HTTP ağ geçidi uygulaması kullanarak doku istemci ayarlarına bakılmaksızın HTTP ağ geçidi'ndeki gönderilir. Bu, olabildiğince çabuk gönderilmesi gereken kritik raporlar için kullanışlıdır. Zamanlama ve diğer koşullara bağlı olarak, rapor gönderme yine de, örneğin HTTP ağ geçidini kapalı veya ağ geçidi ileti ulaşmaz başarısız olabilir. Hemen false olarak ayarlarsanız, raporun durumu istemci ayarlarının HTTP ağ geçidi'nden göre gönderilir. Bu nedenle, bunu HealthReportSendInterval yapılandırmasına göre toplu olarak. Sistem Durumu raporu işleme yanı sıra health store iletilere raporlama sistem durumu iyileştirmek sistem durumu istemci izin verdiğinden Önerilen ayar budur. Varsayılan olarak, raporları hemen gönderilmez. |
| --remove-zaman süresi | Belirtecin süresi dolduğunda, health Store'dan rapor kaldırılmış olup olmadığını gösteren değer. <br><br> Süresi dolduktan sonra health Store'dan true olarak rapor kaldırılırsa. Rapor false olarak ayarlanırsa süresi dolduğunda hata kabul edilir Bu özellik varsayılan olarak false değeridir. İstemciler düzenli aralıklarla bildirdiğinde RemoveWhenExpired false (varsayılan) ayarlamanız gerekir. Bu şekilde muhabir sorunları (örneğin, kilitlenme) ve rapor veremez, varlık sistem durumu raporu süresi dolduğunda hatası değerlendirilir ' dir. Bu varlık sistem durumu hatası olarak işaretler. |
| --sıra numarası | Bu sistem durumu raporu sayısal dize olarak için sıra numarası. <br><br> Rapor sıra numarası, eski raporlar algılamak için sistem durumu deposu tarafından kullanılır. Bir rapora eklendiğinde belirtilmezse, bir sıra numarası otomatik olarak sistem istemci tarafından üretilir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |
| --ttl | Bu sistem durumu raporu geçerli olduğu süre. Bu alan, süresi belirtmek için ISO8601 biçimini kullanır. <br><br> İstemciler düzenli aralıklarla rapor, yaşam süresi daha yüksek sıklıkta raporları göndermelisiniz. İstemcileri geçişi bildirirse, bunlar sonsuz için yaşam süresi ayarlayabilirsiniz. Yaşam süresi dolduğunda, sistem durumu bilgilerini içeren sistem durumu olayı RemoveWhenExpired ise health Store'dan kaldırıldı ya da doğru veya hata sırasında değerlendirilen ise RemoveWhenExpired false. Aksi durumda sonsuz değer varsayılan olarak belirtilen, süresi. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-cluster-select"></a>sfctl kümesi seçin
Bir Service Fabric küme uç noktasına bağlanır.

Güvenli kümeye bağlanma, her iki (.pem) bir sertifika (.crt) ve anahtar dosyası (.key) veya tek bir dosya için mutlak bir yol belirtin. Her ikisini birden belirtmeyin. İsteğe bağlı olarak, güvenli bir kümeye bağlanma, ayrıca bir CA paket dosyası ya da güvenilir CA sertifikaları dizinin mutlak bir yol belirtin. CA sertifikaları dizini kullanıyorsanız `c_rehash <directory>` tarafından sağlanan OpenSSL çalıştırılmalıdır ilk sertifika karma değerleri hesaplamak ve uygun simgesel bağlantılar oluştur.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --uç noktası [gerekli] | Uç nokta URL'si bağlantı noktası ve HTTP veya HTTPS ön ekini de dahil olmak üzere, küme. |
| --aad | Azure Active Directory kimlik doğrulaması için kullanın. |
| --ca | Geçerli olarak değerlendirilecek CA sertifikaları dizini veya CA paket dosyasının mutlak yolu. |
| --cert | İstemci sertifika dosyasının mutlak yolu. |
| --anahtarı | İstemci sertifika anahtar dosyasının mutlak yolu. |
| --no-doğrulama | HTTPS kullanarak sertifika doğrulaması devre dışı, Not\: bu güvenli bir seçenektir ve üretim ortamları için kullanılmamalıdır. |
| --pem | .Pem dosyası olarak istemci sertifikası mutlak yolu. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-cluster-show-connection"></a>sfctl küme show-connection
Bu sfctl örneği bağlı hangi Service Fabric kümesi gösterir.

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-cluster-unprovision"></a>sfctl küme sağlamayı kaldırma
Kod veya yapılandırma paketleri bir Service Fabric kümesinin sağlamasını kaldırma.

Kod ve yapılandırma ayrı olarak sağlama desteklenir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --kod sürümü | Küme kod Paket sürümü. |
| --config-version | Küme bildirimi sürümü. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-cluster-upgrade"></a>sfctl küme yükseltmesi
Service Fabric kümesi kod veya yapılandırma sürümü yükseltme başlatın.

Sağlanan yükseltme parametreleri doğrulayın ve parametrelerin geçerli olduğundan, bir Service Fabric kümesine kod veya yapılandırma sürümü yükseltme başlatın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Uygulama sistem durumu eşleme | Uygulama adı ve en yüksek yüzdesi hatası tetiklenmeden önce sağlıksız çiftleri kodlamalı JSON sözlüğü. |
| --app-type-health-map | Uygulama türü adı ve en yüksek yüzdesi hatası tetiklenmeden önce sağlıksız çiftleri kodlamalı JSON sözlüğü. |
| --kod sürümü | Küme kodu sürümü. |
| --config-version | Küme yapılandırması sürümü. |
| --delta sistem durumu değerlendirmesi | Her bir yükseltme etki alanı tamamlandıktan sonra mutlak sistem durumu değerlendirme yerine delta sistem durumu değerlendirmesi sağlar. |
| --delta-unhealthy-nodes | Küme yükseltme sırasında izin verilen sistem durumu performans düşüşü izin verilen maksimum düğüm yüzdesi.  Varsayılan\: 10. <br><br> Delta düğümlerinin başına yükseltme durumunu ve sistem durumu değerlendirme sırasındaki düğümlerinin durumunu arasında ölçülür. Onay, kümenin genel durumunu toleranslı sınırlarda olduğundan emin olmak için her yükseltme etki alanı yükseltme tamamlandıktan sonra gerçekleştirilir. |
| --hatası eylemi | Olası değerler şunlardır\: 'Geçersiz', 'Geri', 'Manual'. |
| --force-yeniden başlatma | Hatta kod sürümü değiştirilmedi işlemleri zorla yükseltme sırasında yeniden başlatılır. <br><br> Yükseltme, yapılandırma veya veri yalnızca değiştirir. |
| --Sistem durumu denetimi deneme | Uygulama veya kümenin iyi durumda değilse, sistem durumu denetimleri gerçekleştirmek için girişimleri arasındaki süre uzunluğu. |
| --Sistem durumu denetimi kararlı | Süreyi sonraki yükseltme etki alanına yükseltmeye devam etmeden önce uygulama veya kümenin sağlıklı kalmasını gerekir. <br><br> Bu, önce bir ISO 8601 süre temsil eden bir dize olarak yorumlanır. Bu başarısız olursa, milisaniye cinsinden toplam sayısını temsil eden bir sayı olarak yorumlanır. |
| --Sistem durumu denetimi bekleme | Sistem başlatmadan önce bir yükseltme etki alanını tamamladıktan sonra beklenecek süreyi işlemi denetler. |
| --replica-set-check-timeout | En uzun süreyi bir yükseltme etki alanını işlenmesini engellemek ve beklenmeyen sorunları kullanılabilirlik kaybını önlemek için. <br><br> Bu zaman aşımı süresi dolduğunda işlem yükseltme etki alanının kullanılabilirlik kaybı sorunları bağımsız olarak devam eder. Zaman aşımı, her bir yükseltme etki alanı başlangıcında sıfırlanır. Geçerli değerler 0 ile kapsamlı 42949672925 arasındadır. |
| --sıralı yükseltme-modu | Olası değerler şunlardır\: 'Geçersiz', 'UnmonitoredAuto', 'UnmonitoredManual', 'İzlenen'.  Varsayılan\: UnmonitoredAuto. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |
| --iyi durumda olmayan uygulamalar | İyi durumda olmayan uygulamalar yüzdesi hata raporlamadan önce izin verilen en fazla. <br><br> Örneğin, %10 sağlıksız uygulamalarının izin vermek için bu değer 10 olacaktır. Yüzde küme hata olarak kabul edilmeden önce iyi durumda olmayan uygulamalar maksimum toleranslı yüzdesini temsil eder. Yüzde uyulduğundan, ancak en az bir iyi durumda olmayan uygulama sistem durumu uyarı olarak değerlendirilir. Bu, iyi durumda olmayan uygulamalar uygulama örnekleri ApplicationTypeHealthPolicyMap içinde bulunan uygulama türleri uygulamaları hariç kümedeki toplam sayısı üzerinden bölünmesiyle hesaplanır. Az sayıda uygulamalar üzerinde bir hatasını tolere için hesaplama yukarı yuvarlar. |
| --iyi durumda olmayan düğümler | İyi durumda olmayan düğümler yüzdesi hata raporlamadan önce izin verilen en fazla. <br><br> Örneğin, %10 sağlıksız düğümleri izin vermek için bu değer 10 olacaktır. Yüzde küme hata olarak kabul edilmeden önce iyi durumda olmayan düğümlerin en yüksek toleranslı yüzdesini temsil eder. Yüzde uyulduğundan, ancak en az bir iyi durumda olmayan düğüm, sistem durumu uyarı olarak değerlendirilir. Yüzde, kümedeki düğümlerin toplam sayısı üzerinden iyi durumda olmayan düğüm sayısına bölünmesiyle hesaplanır. Az sayıda düğüm üzerinde bir hatasını tolere için hesaplama yukarı yuvarlar. Bu yüzdesi, tolerans yapılandırılması için büyük kümelerde bazı düğümleri her zaman aşağı veya çıkış onarımı için olur. |
| --Yükseltme etki alanı-delta-sağlıksız-düğümler | Küme yükseltme sırasında izin verilen sistem durumu performans düşüşü izin verilen en fazla yükseltme etki düğümler yüzdesi.  Varsayılan\: 15. <br><br> Delta yükseltme başına yükseltme etki düğümler durumunu ve sistem durumu değerlendirme sırasındaki yükseltme etki düğümler durumu arasında ölçülür. Yükseltme etki alanlarında durumunu toleranslı sınırlarda olduğundan emin olmak için yükseltme etki alanlarının her bir yükseltme etki alanı yükseltme tamamlama tüm tamamlandıktan sonra denetimi gerçekleştirilir. |
| --Yükseltme-etki-zaman aşımı | Süreyi FailureAction yürütülmeden önce tamamlamak her bir yükseltme etki alanı vardır. <br><br> Bu, önce bir ISO 8601 süre temsil eden bir dize olarak yorumlanır. Bu başarısız olursa, milisaniye cinsinden toplam sayısını temsil eden bir sayı olarak yorumlanır. |
| --Yükseltme zaman aşımı | Süreyi genel yükseltme FailureAction yürütülmeden önce tamamlanması gerekir. <br><br> Bu, önce bir ISO 8601 süre temsil eden bir dize olarak yorumlanır. Bu başarısız olursa, milisaniye cinsinden toplam sayısını temsil eden bir sayı olarak yorumlanır. |
| --warning-as-error | Uyarıları hata olarak aynı önem derecesi kabul edilip edilmeyeceğini belirtir. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-cluster-upgrade-resume"></a>sfctl Küme yükseltme sürdürme
Bir sonraki yükseltme etki alanına taşıma Küme yükseltme yapın.

Küme kod veya yapılandırma yükseltme üzerinde bir sonraki yükseltme etki alanında uygun durumlarda harekete geçin.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Yükseltme etki [gerekli] | Bir sonraki yükseltme etki alanı bu küme yükseltmesi için. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-cluster-upgrade-rollback"></a>sfctl Küme yükseltme-geri alma
Bir Service Fabric kümesini yükseltme işlemi geri alın.

Bir Service Fabric kümesinin kod veya yapılandırma yükseltmeyi geri.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-cluster-upgrade-status"></a>sfctl Küme Yükseltme durumu
Geçerli Küme yükseltmesinin ilerleme durumunu alır.

Devam eden Küme yükseltme geçerli durumunu alır. Şu anda yükseltme devam ediyor, en son önceki Küme yükseltme durumunu alın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-cluster-upgrade-update"></a>sfctl Küme yükseltme-güncelleştirme
Bir Service Fabric kümesini yükseltme yükseltme parametrelerini güncelleştirin.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Uygulama sistem durumu eşleme | Uygulama adı ve en yüksek yüzdesi hatası tetiklenmeden önce sağlıksız çiftleri kodlamalı JSON sözlüğü. |
| --app-type-health-map | Uygulama türü adı ve en yüksek yüzdesi hatası tetiklenmeden önce sağlıksız çiftleri kodlamalı JSON sözlüğü. |
| --delta sistem durumu değerlendirmesi | Her bir yükseltme etki alanı tamamlandıktan sonra mutlak sistem durumu değerlendirme yerine delta sistem durumu değerlendirmesi sağlar. |
| --delta-unhealthy-nodes | Küme yükseltme sırasında izin verilen sistem durumu performans düşüşü izin verilen maksimum düğüm yüzdesi.  Varsayılan\: 10. <br><br> Delta düğümlerinin başına yükseltme durumunu ve sistem durumu değerlendirme sırasındaki düğümlerinin durumunu arasında ölçülür. Onay, kümenin genel durumunu toleranslı sınırlarda olduğundan emin olmak için her yükseltme etki alanı yükseltme tamamlandıktan sonra gerçekleştirilir. |
| --hatası eylemi | Olası değerler şunlardır\: 'Geçersiz', 'Geri', 'Manual'. |
| --force-yeniden başlatma | Hatta kod sürümü değiştirilmedi işlemleri zorla yükseltme sırasında yeniden başlatılır. <br><br> Yükseltme, yapılandırma veya veri yalnızca değiştirir. |
| --Sistem durumu denetimi deneme | Uygulama veya kümenin iyi durumda değilse, sistem durumu denetimleri gerçekleştirmek için girişimleri arasındaki süre uzunluğu. |
| --Sistem durumu denetimi kararlı | Süreyi sonraki yükseltme etki alanına yükseltmeye devam etmeden önce uygulama veya kümenin sağlıklı kalmasını gerekir. <br><br> Bu, önce bir ISO 8601 süre temsil eden bir dize olarak yorumlanır. Bu başarısız olursa, milisaniye cinsinden toplam sayısını temsil eden bir sayı olarak yorumlanır. |
| --Sistem durumu denetimi bekleme | Sistem başlatmadan önce bir yükseltme etki alanını tamamladıktan sonra beklenecek süreyi işlemi denetler. |
| --replica-set-check-timeout | En uzun süreyi bir yükseltme etki alanını işlenmesini engellemek ve beklenmeyen sorunları kullanılabilirlik kaybını önlemek için. <br><br> Bu zaman aşımı süresi dolduğunda işlem yükseltme etki alanının kullanılabilirlik kaybı sorunları bağımsız olarak devam eder. Zaman aşımı, her bir yükseltme etki alanı başlangıcında sıfırlanır. Geçerli değerler 0 ile kapsamlı 42949672925 arasındadır. |
| --sıralı yükseltme-modu | Olası değerler şunlardır\: 'Geçersiz', 'UnmonitoredAuto', 'UnmonitoredManual', 'İzlenen'.  Varsayılan\: UnmonitoredAuto. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |
| --iyi durumda olmayan uygulamalar | İyi durumda olmayan uygulamalar yüzdesi hata raporlamadan önce izin verilen en fazla. <br><br> Örneğin, %10 sağlıksız uygulamalarının izin vermek için bu değer 10 olacaktır. Yüzde küme hata olarak kabul edilmeden önce iyi durumda olmayan uygulamalar maksimum toleranslı yüzdesini temsil eder. Yüzde uyulduğundan, ancak en az bir iyi durumda olmayan uygulama sistem durumu uyarı olarak değerlendirilir. Bu, iyi durumda olmayan uygulamalar uygulama örnekleri ApplicationTypeHealthPolicyMap içinde bulunan uygulama türleri uygulamaları hariç kümedeki toplam sayısı üzerinden bölünmesiyle hesaplanır. Az sayıda uygulamalar üzerinde bir hatasını tolere için hesaplama yukarı yuvarlar. |
| --iyi durumda olmayan düğümler | İyi durumda olmayan düğümler yüzdesi hata raporlamadan önce izin verilen en fazla. <br><br> Örneğin, %10 sağlıksız düğümleri izin vermek için bu değer 10 olacaktır. Yüzde küme hata olarak kabul edilmeden önce iyi durumda olmayan düğümlerin en yüksek toleranslı yüzdesini temsil eder. Yüzde uyulduğundan, ancak en az bir iyi durumda olmayan düğüm, sistem durumu uyarı olarak değerlendirilir. Yüzde, kümedeki düğümlerin toplam sayısı üzerinden iyi durumda olmayan düğüm sayısına bölünmesiyle hesaplanır. Az sayıda düğüm üzerinde bir hatasını tolere için hesaplama yukarı yuvarlar. Bu yüzdesi, tolerans yapılandırılması için büyük kümelerde bazı düğümleri her zaman aşağı veya çıkış onarımı için olur. |
| --Yükseltme etki alanı-delta-sağlıksız-düğümler | Küme yükseltme sırasında izin verilen sistem durumu performans düşüşü izin verilen en fazla yükseltme etki düğümler yüzdesi.  Varsayılan\: 15. <br><br> Delta yükseltme başına yükseltme etki düğümler durumunu ve sistem durumu değerlendirme sırasındaki yükseltme etki düğümler durumu arasında ölçülür. Yükseltme etki alanlarında durumunu toleranslı sınırlarda olduğundan emin olmak için yükseltme etki alanlarının her bir yükseltme etki alanı yükseltme tamamlama tüm tamamlandıktan sonra denetimi gerçekleştirilir. |
| --Yükseltme-etki-zaman aşımı | Süreyi FailureAction yürütülmeden önce tamamlamak her bir yükseltme etki alanı vardır. <br><br> Bu, önce bir ISO 8601 süre temsil eden bir dize olarak yorumlanır. Bu başarısız olursa, milisaniye cinsinden toplam sayısını temsil eden bir sayı olarak yorumlanır. |
| --yükseltme türü | Olası değerler şunlardır\: 'Geçersiz', 'Çalışırken', 'Rolling_ForceRestart'.  Varsayılan\: alınıyor. |
| --Yükseltme zaman aşımı | Süreyi genel yükseltme FailureAction yürütülmeden önce tamamlanması gerekir. <br><br> Bu, önce bir ISO 8601 süre temsil eden bir dize olarak yorumlanır. Bu başarısız olursa, milisaniye cinsinden toplam sayısını temsil eden bir sayı olarak yorumlanır. |
| --warning-as-error | Uyarıları hata olarak aynı önem derecesi kabul edilip edilmeyeceğini belirtir. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |


## <a name="next-steps"></a>Sonraki adımlar
- [Kurulum](service-fabric-cli.md) Service Fabric CLI.
- Service Fabric CLI kullanarak kullanmayı öğrenin [örnek betikleri](/azure/service-fabric/scripts/sfctl-upgrade-application).