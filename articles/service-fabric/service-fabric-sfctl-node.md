---
title: Azure Service Fabric CLI sfctl düğümlü | Microsoft Docs
description: Service Fabric CLI sfctl düğümü komutlarını açıklar.
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
ms.openlocfilehash: fb8a310a131938e95f3d21b3962dbbd1944a57ed
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763434"
---
# <a name="sfctl-node"></a>sfctl node
Bir küme düğümleri yönetin.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| devre dışı bırak | Belirtilen devre dışı bırakma hedefine olan bir Service Fabric küme düğümü devre dışı bırakın. |
| etkinleştir | Şu anda devre dışı bir Service Fabric küme düğümü etkinleştirin. |
| sistem durumu | Service Fabric düğümü durumunu alır. |
| bilgi | Service Fabric kümesi içinde belirli bir düğüme hakkındaki bilgileri alır. |
| liste | Service Fabric kümedeki düğümlerin listesini alır. |
| yükleme | Service Fabric düğümü yük bilgilerini alır. |
| durumu kaldırma | Service Fabric kalıcı durum bir düğümde kalıcı olarak kaybolur veya kaldırılmış olduğunu bildirir. |
| Sistem Durumu raporu | Service Fabric düğümü üzerindeki sistem durumu raporu gönderir. |
| Yeniden başlatma | Service Fabric küme düğümü yeniden başlatır. |
| geçiş | Başlatır veya bir küme düğümü durdurur. |
| Geçiş durumu | StartNodeTransition kullanmaya bir işlemin ilerlemesini alır. |

## <a name="sfctl-node-disable"></a>sfctl düğümü devre dışı bırak
Belirtilen devre dışı bırakma hedefine olan bir Service Fabric küme düğümü devre dışı bırakın.

Belirtilen devre dışı bırakma hedefine olan bir Service Fabric küme düğümü devre dışı bırakın. Devre dışı bırakma işlemi devam ediyor sonra devre dışı bırakma hedefine artar, ancak azaltılan değil (örneğin, duraklatma amacıyla devre dışı bir düğüm daha fazla yeniden başlatma, ancak şekilde geçici devre dışı bırakılabilir. Düğümler, düğüm işlemi devre dışı bırakılır sonra istediğiniz zaman etkinleştir kullanarak yeniden. Devre dışı bırakma tam değilse, bu devre dışı bırakma işlemini iptal eder. Arıza ve devre dışı durumdayken geri gelir bir düğüm yine Hizmetleri bu düğümde yerleştirilecek önce yeniden etkinleştirilmesi gerekir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --düğüm adı [gerekli] | Düğümün adı. |
| --devre dışı bırakma hedefi | Hedefi veya düğüm devre dışı bırakma nedeni açıklanmaktadır. Olası değerler aşağıdaki. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-node-enable"></a>sfctl düğüm etkinleştir
Şu anda devre dışı bir Service Fabric küme düğümü etkinleştirin.

Şu anda devre dışı bir Service Fabric küme düğümü etkinleştirir. Sonra etkin, düğümün yeniden yeni çoğaltmaları yerleştirme için uygun bir hedef olacaktır ve düğüm üzerinde kalan devre dışı bırakılmış tüm yinelemeleri yeniden etkinleştirilmesi.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --düğüm adı [gerekli] | Düğümün adı. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-node-health"></a>sfctl düğüm durumu
Service Fabric düğümü durumunu alır.

Service Fabric düğümü durumunu alır. Sistem durumu olayları sistem durumuna bağlıdır düğümde bildirilen koleksiyonu filtrelemek için EventsHealthStateFilter kullanın. Health store içinde adına göre belirttiğiniz düğüm mevcut değilse, bu bir hata döndürür.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --düğüm adı [gerekli] | Düğümün adı. |
| --Sağlık Durumu Filtresi olayları | Döndürülen HealthEvent nesnelerin sistem durumuna bağlıdır koleksiyonu filtrelemeye izin verir. Bu parametre için olası değerler aşağıdaki sistem durumlarının bir tamsayı değeri içerir. Filtreyle eşleşen olaylar döndürülür. Tüm olayları toplanmış sistem durumunu değerlendirmek için kullanılır. Belirtilmezse, tüm girişleri döndürülür. Durum değerleri bayrağı tabanlı numaralandırma olduğundan, değer, bu değerlerin Bitsel 'Veya' işleci kullanılarak edinilen bir bileşimi olabilir. Sağlanan değer 6 ise, örneğin, ardından tüm olaylar Tamam (2) ve uyarı (4), HealthState değeriyle döndürülür.  <br> -Varsayılan - varsayılan değer. Tüm HealthState eşleşir. Değer sıfır olur.  <br> -Hiçbiri - herhangi bir HealthState değer eşleşmeyen filtreleyin. Sonuç durumları belirli bir koleksiyon döndürmek için kullanılır. Değer 1'dir.  <br> -Tamam - eşleşmeleri HealthState değerle Tamam giriş filtreleyin. Değer 2'dir.  <br> -Uyarı - filtre HealthState eşleşme girişle uyarı değer. Değer 4'tür.  <br> -Hata - Giriş hata HealthState değeriyle eşleşen Filtresi. Değer 8'dir.  <br> -Tüm - giriş herhangi bir HealthState değeri ile eşleşen filtre. Değer, 65535 ' dir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-node-info"></a>sfctl düğüm bilgisi
Service Fabric kümesi içinde belirli bir düğüme hakkındaki bilgileri alır.

Service Fabric kümesi içinde belirli bir düğüme hakkındaki bilgileri alır. Yanıt adı, durumu, kimliği, sistem durumu, açık kalma süresi ve diğer düğümün ayrıntılarını içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --düğüm adı [gerekli] | Düğümün adı. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-node-list"></a>sfctl düğüm listesi
Service Fabric kümedeki düğümlerin listesini alır.

Service Fabric kümedeki düğümlerin listesini alır. Yanıt adı, durumu, kimliği, sistem durumu, açık kalma süresi ve diğer düğümün ayrıntılarını içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --devamlılık belirteci | Devamlılık belirteci parametresi, bir sonraki sonuç kümesi elde etmek için kullanılır. Sistem sonuçlarından tek bir yanıtta uymayan bir devamlılık belirteci boş olmayan bir değer ile API yanıt olarak dahil edilir. Bu değer geçirilen zaman sonraki API çağrısı API sonraki sonuç kümesi döndürür. Daha fazla sonuç varsa, devamlılık belirteci bir değer içermiyor. Bu parametrenin değeri, URL kodlanmış olmamalıdır. |
| --düğüm Durumu Filtresi | Üzerinde NodeStatus bağlı düğümler filtrelemeye izin verir. Yalnızca belirtilen filtre değeri eşleşen düğümleri döndürülür. Filtre değeri aşağıdakilerden biri olabilir.  Varsayılan\: varsayılan. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-node-load"></a>sfctl düğümü yükleme
Service Fabric düğümü yük bilgilerini alır.

Yük ya da tanımlanmış kapasite sahip tüm ölçümler için Service Fabric düğümünün yük bilgilerini alır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --düğüm adı [gerekli] | Düğümün adı. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-node-remove-state"></a>sfctl düğüm Kaldır-durumu
Service Fabric kalıcı durum bir düğümde kalıcı olarak kaybolur veya kaldırılmış olduğunu bildirir.

Service Fabric kalıcı durum bir düğümde kalıcı olarak kaybolur veya kaldırılmış olduğunu bildirir.  Bu, o düğümün kalıcı durumunu kurtarmak mümkün olmadığını anlamına gelir. Bu genellikle bir sabit disk temiz silinmişse ya da bir sabit disk çökerse gerçekleşir. Düğüm, bu işlemin başarılı olması için aşağı olmak zorundadır. Bu işlem Service Fabric çoğaltmaları bu düğümde artık yok ve bu Service Fabric yinelemeler geri gelmesi bekleniyor durması gerektiğini bildiren olanak sağlar. Bu cmdlet, düğüm durumunu kaldırılmamıştır ve düğüm sağlam durumunu ile geri dönerseniz çalıştırmayın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --düğüm adı [gerekli] | Düğümün adı. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-node-report-health"></a>sfctl düğüm rapor-durum
Service Fabric düğümü üzerindeki sistem durumu raporu gönderir.

Belirtilen Service Fabric düğümü sağlık durumunu raporlar. Rapor kaynağı özelliği üzerinde bildirilir ve sistem durumu raporu hakkında bilgiler içermelidir. Rapor sistem durumu depoya ileten bir Service Fabric ağ geçidi düğümü gönderilir. Rapor ağ geçidi tarafından kabul edilen, ancak sonra ek doğrulama durumu mağaza tarafından reddedildi. Örneğin, sistem sağlığı deposunu raporu eski sıra numarası gibi geçersiz bir parametre nedeniyle reddedebilir. Health store içinde rapor uygulanıp uygulanmadığını görmek için raporu HealthEvents bölümünde görünür denetleyin.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Sistem durumu özelliği [gerekli] | Sistem durumu bilgisi özelliği. <br><br> Bir varlığın farklı özellikler için sistem durumu raporlarının olabilir. , Bir dize ve rapor tetikler durumu koşulu kategorilere ayırmak Raporlayıcı esnekliğini tanımak için olmayan bir sabit numaralandırması özelliğidir. Örneğin, bunu "AvailableDisk" özelliği bu düğümde raporlamak için bir Raporlayıcı SourceId "LocalWatchdog" ile bir düğümde, kullanılabilir disk durumunu izleyebilirsiniz. Bu özellik "Bağlantı" aynı düğümde raporlamak için aynı Raporlayıcı düğümü bağlantı izleyebilirsiniz. Health store içinde bu raporları ayrı sistem durumu olayları için belirtilen düğümün olarak kabul edilir. SourceId birlikte özelliği sistem durumu bilgilerini benzersiz şekilde tanımlar. |
| --Sistem durumu [gerekli] | Olası değerler şunlardır\: 'Geçersiz', 'Tamam', 'Uyarı', 'Error', 'Bilinmeyen'. |
| --düğüm adı [gerekli] | Raporlamak için düğüm adı. |
| --Kaynak Kimliği [gerekli] | Sistem durumu bilgisi oluşturulan izleme/istemci/sistem bileşeni tanımlayan kaynak adı. |
| --açıklaması | Sistem durumu bilgisi açıklaması. <br><br> Bu rapor hakkında İnsan okunabilir bilgi eklemek için kullanılan serbest metin temsil eder. Açıklama en çok dize uzunluğu 4096 karakter olabilir. Otomatik olarak sağlanan dize uzunsa kesilir. Kesildi, son açıklama işareti "[kesildi]" karakterinden ve toplam dize boyutu 4096 karakter olabilir. İşaretin durumunu kullanıcılara bu kesme gösteren oluştu. Kesirli kısmı, açıklama özgün dizesinden değerinden 4096 karakter olduğuna dikkat edin. |
| --hemen | Raporun hemen gönderilmesi gerekip gerekmediğini gösteren bir bayrak. <br><br> Sistem Durumu raporu bir Service Fabric ağ geçidi sistem durumu depoya iletir uygulama gönderilir. Hemen ayarlanmışsa true, rapor hemen HTTP ağ geçidi uygulaması kullanarak doku istemci ayarlarına bakılmaksızın sistem durumu deposuna HTTP geçidinden gönderilir. Mümkün olan en kısa sürede gönderilmesi gereken kritik raporlar için kullanışlıdır. Zamanlama ve diğer koşullara bağlı olarak, raporu göndermeden hala, örneğin HTTP ağ geçidi kapalı veya ağ geçidi ileti ulaşmak değil başarısız olabilir. Hemen false olarak ayarlanırsa, rapor sistem istemci ayarlarına HTTP ağ geçidi'nden göre gönderilir. Bu nedenle, onu HealthReportSendInterval yapılandırmasına göre toplu hale. Sistem durumu iletileri durum deposu ve bunun yanı sıra sistem durumu raporu işleme raporlama iyileştirmek sistem durumu istemci izin verdiği için Önerilen ayar budur. Varsayılan olarak, raporları hemen gönderilmez. |
| --Kaldır zaman süresi | Süresi dolduğunda, sistem durumu deposundan rapor kaldırılmış olup olmadığını belirten değer. <br><br> Bunu süresi dolduktan sonra sistem durumu deposundan true olarak rapor kaldırılırsa. Raporun false olarak ayarlanırsa dolduğunda hata kabul edilir Bu özellik varsayılan olarak false değeridir. İstemcileri düzenli aralıklarla bildirdiğinde RemoveWhenExpired false (varsayılan) ayarlamanız gerekir. Bu şekilde Raporlayıcı sorunları (örneğin kilitlenme) var ve rapor veremez, sistem durumu raporu sona erdiğinde varlık hatasında değerlendirilir olur. Bu varlık sistem durumu hatası olarak işaretler. |
| --sıra numarası | Bu sistem durumu raporu sayısal dize olarak sıra numarası. <br><br> Rapor sıra numarası, eski raporları algılamak için sistem durumu mağaza tarafından kullanılır. Bir rapor eklendiğinde belirtilmezse, bir sıra numarası sistem durumu istemcisi tarafından otomatik olarak oluşturulan. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |
| --ttl | Bu sistem durumu raporu geçerli olduğu süre. Bu alan, süresi belirtmek için ISO8601 biçimini kullanıyor. <br><br> İstemcileri düzenli aralıklarla bildirdiğinde, bunlar yaşam süresi'den daha yüksek sıklıkta raporları göndermesi gerekir. İstemcileri geçişi rapor varsa bunlar için sonsuz yaşam süresini ayarlayabilirsiniz. Yaşam süresi dolduğunda, sistem durumu bilgilerini içeren sistem durumu olayı RemoveWhenExpired ise, sistem durumu Mağazası'ndan kaldırılmış ya da doğru veya hatada, hesaplanan ise RemoveWhenExpired false. Aksi durumda belirtilen, süresi sonsuz değeri varsayılanlara. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-node-restart"></a>sfctl düğümü yeniden başlatma
Service Fabric küme düğümü yeniden başlatır.

Zaten başlatılmış bir Service Fabric küme düğümü yeniden başlatır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --düğüm adı [gerekli] | Düğümün adı. |
| --oluşturma-doku-dökümü | Fabric düğümü işleminin döküm oluşturmak için true değerini belirtin. Büyük küçük harfe duyarlı budur.  Varsayılan\: False. |
| --düğümü örnek kimliği | Hedef düğüm örnek kimliği. Örnek kimliği yalnızca düğüm geçerli örnekle eşleşmesi durumunda düğüm yeniden belirtilir. Varsayılan değer "0" herhangi bir örnek kimliği eşleşir Örnek kimliği get düğümü sorgusu kullanılarak edinilebilir.  Varsayılan\: 0. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-node-transition"></a>sfctl düğüm geçiş
Başlatır veya bir küme düğümü durdurur.

Başlatır veya bir küme düğümü durdurur.  Bir küme düğümü bir, işletim sistemi örneği kendisini değil işlemidir.  Bir düğüm başlatmak için NodeTransitionType parametresi için "Başlangıç" geçirin. Bir düğüm durdurmak için NodeTransitionType parametresi için "Durdur" geçirin. Düğüm henüz geçiş tamamlanmadığından API geri döndüğünde bu API - işlemini başlatır. İşlemin ilerlemesi almak için aynı Operationıd ile GetNodeTransitionProgress çağırın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --düğümü-örnek-kimliği [gerekli] | Hedef düğüm düğümü örnek kimliği. Bu GetNodeInfo API aracılığıyla belirlenebilir. |
| --düğüm adı [gerekli] | Düğümün adı. |
| --düğümü-geçiş-type [gerekli] | Gerçekleştirmek için geçiş türünü belirtir.  Durdurulmuş bir düğüm NodeTransitionType.Start başlar. NodeTransitionType.Stop çalışır durumda bir düğüm durdurur. |
| --işlem kimliği [gerekli] | Bu API çağrısının tanımlayan bir GUID.  Bu, karşılık gelen GetProgress API geçirilir. |
| --stop-süresi-içinde-[gerekli] (saniye) | Saniye cinsinden süre düğümünü korumak için durduruldu.  En düşük değer 600, en fazla 14400.  Bu süre dolduktan sonra düğümü otomatik olarak geri gelmesi. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-node-transition-status"></a>sfctl düğüm geçiş-durum
StartNodeTransition kullanmaya bir işlemin ilerlemesini alır.

Sağlanan Operationıd kullanarak StartNodeTransition ile başlatılan bir işlemin ilerlemesini alır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --düğüm adı [gerekli] | Düğümün adı. |
| --işlem kimliği [gerekli] | Bu API çağrısının tanımlayan bir GUID.  Bu, karşılık gelen GetProgress API geçirilir. |
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
- [Kurulum](service-fabric-cli.md) Service Fabric CLI.
- Service Fabric CLI kullanarak kullanmayı öğrenin [örnek komutlar](/azure/service-fabric/scripts/sfctl-upgrade-application).