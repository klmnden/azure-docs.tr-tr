---
title: Azure Service Fabric CLI sfctl düğümlü | Microsoft Docs
description: Service Fabric CLI'sını sfctl düğüm komutlarını açıklamaktadır.
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
ms.openlocfilehash: 08ea0081c84ea31b2b71d03679b1b527cf94c075
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60556789"
---
# <a name="sfctl-node"></a>sfctl node
Küme düğümleri yönetin.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| devre dışı bırak | Bir Service Fabric küme düğümü belirtilen devre dışı bırakma amacıyla devre dışı bırakın. |
| etkinleştir | Şu anda devre dışı bir Service Fabric küme düğümü etkinleştirin. |
| sağlık | Bir Service Fabric düğüm durumunu alır. |
| bilgi | Service Fabric kümesinde belirli bir düğüm hakkında bilgi alır. |
| list | Service Fabric kümesinde düğümlerin listesini alır. |
| yükleme | Bir Service Fabric düğümü yük bilgilerini alır. |
| durumu-Kaldır | Service Fabric kalıcı durum bir düğümde kalıcı olarak kaybolur veya kaldırılmış olduğunu bildirir. |
| durumu- | Service Fabric düğüm üzerinde bir sistem durumu raporu gönderir. |
| restart | Bir Service Fabric küme düğümü yeniden başlatır. |
| geçiş | Başlatır veya küme düğümü durdurur. |
| Geçiş durumu | StartNodeTransition kullanmaya başlamanıza yardımcı olan bir işlemin ilerlemesini alır. |

## <a name="sfctl-node-disable"></a>sfctl düğümü devre dışı bırak
Bir Service Fabric küme düğümü belirtilen devre dışı bırakma amacıyla devre dışı bırakın.

Bir Service Fabric küme düğümü belirtilen devre dışı bırakma amacıyla devre dışı bırakın. Devre dışı bırakma sürüyor sonra devre dışı bırakma hedefi artar, ancak değil azaldıkça (örneğin, duraklatma hedefle devre dışı bir düğüm daha fazla yeniden başlatma, ancak dönüştürülemeyeceği geçici olarak devre dışı bırakılabilir. Düğümler, düğüm işlemi devre dışı bırakılır sonra istediğiniz zaman etkinleştirin yeniden. Devre dışı bırakma tam değilse, bunu devre dışı bırakma iptal eder. Çalışmayı durdurursa ve devre dışı durumdayken geri gelir bir düğümü yine de o düğümde Hizmetleri yerleştirilecek önce etkinleştirilmesi gerekir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --[gerekli] düğüm adı | Düğümün adı. |
| --deactivation-intent | Amacı veya nedeni, düğümü devre dışı bırakma açıklar. Olası değerler aşağıda verilmiştir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-node-enable"></a>sfctl düğümü etkinleştir
Şu anda devre dışı bir Service Fabric küme düğümü etkinleştirin.

Şu anda devre dışı bir Service Fabric küme düğümü etkinleştirir. Etkin, düğüm yeniden Yeni çoğaltmaların yerleştirilmesi için uygun bir hedef olur ve kalan düğüm üzerinde devre dışı bırakılmış tüm çoğaltmaları yeniden sonra.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --[gerekli] düğüm adı | Düğümün adı. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-node-health"></a>sfctl düğüm durumu
Bir Service Fabric düğüm durumunu alır.

Bir Service Fabric düğüm durumunu alır. Sistem durumu olaylarını sistem durumuna bağlıdır düğümde bildirilen koleksiyonunu filtrelemek için EventsHealthStateFilter kullanın. Health store içinde belirttiğiniz ada göre düğüm mevcut değilse bu hata döndürür.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --[gerekli] düğüm adı | Düğümün adı. |
| --events-health-state-filter | Döndürülen sistem durumu olayı nesnelerinin koleksiyonunu sistem durumuna göre filtrelemeye olanak tanır. Bu parametre için olası değerler aşağıdaki sistem durumlarının bir tamsayı değeri içerir. Yalnızca filtreyle eşleşen olaylar döndürülür. Tüm olaylar, toplanan sistem durumunu değerlendirmek için kullanılır. Belirtilmezse, tüm girişleri döndürülür. Durum değerleri numaralandırma bayrağı tabanlı olduğundan, değer Bitsel 'Veya' işlecini kullanarak elde ettiğiniz bu değerlerin bir birleşimi olabilir. 6 sağlanan değer, örneğin, ardından tüm olayları Tamam (2) ve (4) uyarı HealthState değeriyle döndürülür.  <br> -Default - varsayılan değer. Tüm HealthState eşleşir. Değer sıfırdır.  <br> -Hiçbiri - herhangi bir HealthState değer eşleşmeyen filtreleyin. Belirli bir koleksiyon durumlarının sonuç döndürmek için kullanılır. Değer 1'dir.  <br> -Tamam - eşleşme HealthState değeriyle Tamam giriş filtreleyin. Değeri 2'dir.  <br> -Uyarı - filtre HealthState girişle eşleşir uyarı değeri. Değer 4'tür.  <br> -Hata - giriş hatası HealthState değeri ile eşleşen filtre. Değer 8'dir.  <br> -All - giriş herhangi bir HealthState değeri ile eşleşen filtreleyin. Değer 65535'tir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-node-info"></a>sfctl düğüm bilgileri
Service Fabric kümesinde belirli bir düğüm hakkında bilgi alır.

Yanıt adı, durumu, kimliği, sistem durumu, çalışma süresi ve diğer düğümün ayrıntılarını içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --[gerekli] düğüm adı | Düğümün adı. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-node-list"></a>sfctl düğüm listesi
Service Fabric kümesinde düğümlerin listesini alır.

Yanıt adı, durumu, kimliği, sistem durumu, çalışma süresi ve düğümleri hakkında diğer ayrıntıları içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --continuation-token | Devamlılık belirteci parametresi, sonraki sonuç kümesini almak için kullanılır. Sistem sonuçlardan tek bir yanıtta uymayan bir devamlılık belirteci boş olmayan bir değer ile API yanıt olarak dahil edilir. Bu değer geçirilen zaman sonraki API çağrısı, API, sonraki sonuç kümesini döndürür. Daha fazla sonuç varsa, devamlılık belirteci bir değer içermiyor. Bu parametrenin değeri, URL kodlanmış olmamalıdır. |
| --en fazla sonuç | En fazla disk belleğine alınan sorguları bir parçası olarak döndürülecek sonuç sayısı. Bu parametre, döndürülen sonuç sayısı üzerindeki üst sınırını tanımlar. İletinin en büyük ileti boyutu kısıtlamaları göre uymayan, belirtilen en fazla sonuç değerinden yapılandırmada tanımlanabilir sonuç döndürmedi. Bu parametre sıfıra eşit ya da belirtilmemiş disk belleğine alınan sorgu dönüş iletiye sığmayacak mümkün olduğunca çok sonuçları içerir. |
| --node-status-filter | Düğümleri üzerinde NodeStatus filtrelemeye izin verir. Yalnızca belirtilen filtre değeri ile eşleşen düğümleri döndürülür. Filtre değeri aşağıdakilerden biri olabilir.  Varsayılan\: varsayılan. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-node-load"></a>sfctl düğümü yükleme
Bir Service Fabric düğümü yük bilgilerini alır.

Bir Service Fabric düğümü yük veya kapasite tanımlanmış olan tüm ölçümleri için yük bilgilerini alır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --[gerekli] düğüm adı | Düğümün adı. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-node-remove-state"></a>sfctl düğüm durumu-Kaldır
Service Fabric kalıcı durum bir düğümde kalıcı olarak kaybolur veya kaldırılmış olduğunu bildirir.

Bu düğüm kalıcı durumunu kurtarma mümkün olmadığı anlamına gelir. Bu genellikle bir sabit disk temiz silinmişse veya bir sabit disk kilitlenmesi durumunda gerçekleşir. Bu işlemin başarılı olması için olacağını düğümü vardır. Bu işlem, Service Fabric düğüm çoğaltmalarda artık yok ve bu çoğaltmalar geri gelmesi beklenirken, Service Fabric durması gerektiğini bilmeniz olanak tanır. Bu cmdlet, durumu düğümünde kaldırılmaz ve düğüm sağlam durumunu ile geri dönerseniz çalıştırmayın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --[gerekli] düğüm adı | Düğümün adı. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-node-report-health"></a>sfctl düğüm durumu-
Service Fabric düğüm üzerinde bir sistem durumu raporu gönderir.

Service Fabric düğümü belirtilen sistem durumunu raporlar. Rapor üzerinde bildirilen özellik ve sistem durumu raporu kaynağı hakkındaki bilgileri içermelidir. Rapor sistem durumu deposu için ileten bir Service Fabric ağ geçidi düğümü gönderilir. Rapor ağ geçidi tarafından kabul edilen, ancak sistem durumu deposu tarafından ek doğrulama sonrasında reddedilen. Örneğin, sistem durumu deposu raporu eski sıra numarası gibi geçersiz bir parametre nedeniyle reddedebilir. Health store içinde rapor uygulanıp uygulanmadığını görmek için raporu HealthEvents bölümünde görünür denetleyin.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --[gerekli] sistem durumu özelliği | Sistem durumu bilgileri özelliği. <br><br> Bir varlık sistem durumu raporlarının farklı özellikler için sahip olabilir. , Bir dize ve rapor tetikleyen durumu koşulu kategorilere ayırmak muhabir esnekliğini tanımak için olmayan bir sabit numaralandırma özelliğidir. Örneğin, "AvailableDisk" özelliği, düğüm üzerinde rapor için bir Raporlayıcı SourceId "LocalWatchdog" ile bir düğümde, kullanılabilir disk durumunu izleyebilirsiniz. Bu özellik "Bağlantı" aynı düğümde raporlamak için aynı muhabir düğüm bağlantısı izleyebilirsiniz. Health store içinde bu raporları belirtilen düğüm için ayrı bir sistem durumu olayları olarak kabul edilir. SourceId birlikte özelliği sistem durumu bilgileri benzersiz olarak tanımlar. |
| --[gerekli] sistem durumu | Olası değerler şunlardır\: 'Geçersiz', 'Tamam', 'Warning', 'Error', 'Bilinmeyen'. |
| --[gerekli] düğüm adı | Düğümün adı. |
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

## <a name="sfctl-node-restart"></a>sfctl düğümü yeniden başlatma
Bir Service Fabric küme düğümü yeniden başlatır.

Zaten başlatılmış bir Service Fabric küme düğümü yeniden başlatır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --[gerekli] düğüm adı | Düğümün adı. |
| --oluşturma-fabric-dökümü | Fabric düğümü işlem dökümü oluşturmak için true değerini belirtin. Büyük/küçük harfe budur.  Varsayılan\: False. |
| --düğüm örnek kimliği | Hedef düğüm örneği kimliği. Örnek kimliği yalnızca düğümünün geçerli örnek ile eşleşiyorsa düğüm yeniden belirtilir. Bir varsayılan değer "0" herhangi bir örnek kimliği eşleşir Örnek kimliği get düğümü sorgusu kullanılarak edinilebilir.  Varsayılan\: 0. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-node-transition"></a>sfctl düğüm geçişi
Başlatır veya küme düğümü durdurur.

Başlatır veya küme düğümü durdurur.  Bir küme düğümünde bir işletim sistemi örneği kendisi değil işlemdir.  Bir düğüm başlatmak için NodeTransitionType parametresi için "Başlangıç" geçirin. Bir düğüm durdurmak için NodeTransitionType parametresi için "Durdur" geçirin. Bu API düğümü geçiş henüz tamamlanmadığından API döndürür - işlemi başlar. İşlemin ilerleme durumunu almak için aynı Operationıd ile GetNodeTransitionProgress çağırın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --düğümü-örnek-id [gerekli] | Hedef düğüm düğüm örneği kimliği. Bu GetNodeInfo API aracılığıyla belirlenebilir. |
| --[gerekli] düğüm adı | Düğümün adı. |
| --düğümü-geçiş-type [gerekli] | Gerçekleştirmek için geçiş türünü gösterir.  Durdurulmuş bir düğüm NodeTransitionType.Start başlar. NodeTransitionType.Stop çalışır durumda bir düğüm durdurur. |
| --işlem kimliği [gerekli] | Bu API çağrısının tanımlayan bir GUID.  Bu, ilgili GetProgress API geçirilir. |
| --stop-süresi-de-[gerekli] (saniye) | Saniye cinsinden süreyi düğümünü korumak için durduruldu.  En düşük değer 600, en fazla 14400.  Bu süre sona erdikten sonra düğümün otomatik olarak geri gerekecektir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-node-transition-status"></a>sfctl düğüm geçiş durumu
StartNodeTransition kullanmaya başlamanıza yardımcı olan bir işlemin ilerlemesini alır.

Sağlanan Operationıd kullanarak StartNodeTransition ile başlatılan bir işlemin ilerlemesini alır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --[gerekli] düğüm adı | Düğümün adı. |
| --işlem kimliği [gerekli] | Bu API çağrısının tanımlayan bir GUID.  Bu, ilgili GetProgress API geçirilir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

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