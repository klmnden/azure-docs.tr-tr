---
title: Azure Service Fabric CLI - sfctl küme | Microsoft Docs
description: Service Fabric CLI sfctl küme komutlarını açıklar.
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
ms.openlocfilehash: 60f3f74778f0fb32677c3b87b3140131ccd37bea
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763638"
---
# <a name="sfctl-cluster"></a>sfctl cluster
Seçin, yönetmek ve Service Fabric kümeleri çalışmayabilir.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| kod sürümleri | Service Fabric kümesi içinde sağlanan kod sürümleri doku listesini alır. |
| config sürümleri | Service Fabric kümesi içinde sağlanan yapılandırma sürümleri doku listesini alır. |
| sistem durumu | Service Fabric kümesi durumunu alır. |
| Bildirimi | Service Fabric küme bildirimi alırsınız. |
| işlemi iptal etme | Bir kullanıcı kaynaklanan hata işlemi iptal eder. |
| işlem listesinde | Kullanıcı kaynaklanan hata işlemleri tarafından sağlanan girişin filtrelenmiş bir listesini alır. |
| Sağlama | Service Fabric kümesi kod veya yapılandırma paketleri sağlayın. |
| Sistem Kurtarma | Service Fabric kümesi şu anda çekirdek kaybında takıldı Sistem Hizmetleri kurtarmayı denemesi belirtir. |
| Sistem Durumu raporu | Service Fabric kümesi üzerinde bir sistem durumu raporu gönderir. |
| seç | Service Fabric kümesi uç noktasına bağlanır. |
| Sağlamayı kaldırma | Service Fabric kümesi kod veya yapılandırma paketleri sağlama. |
| Yükseltme | Service Fabric kümesi kod veya yapılandırma sürümüne yükseltme başlatın. |
| Yükseltme devam et | Bir sonraki yükseltme etki alanına taşıma Küme yükseltme yapın. |
| Yükseltmeyi geri alma | Service Fabric kümesini yükseltme işlemi geri alın. |
| Yükseltme durumu | Geçerli Küme yükseltmesinin ilerleme durumunu alır. |
| Yükseltme güncelleştirme | Service Fabric Küme Yükseltme yükseltme parametreleri güncelleştirin. |

## <a name="sfctl-cluster-code-versions"></a>sfctl küme kod-sürümleri
Service Fabric kümesi içinde sağlanan kod sürümleri doku listesini alır.

Kümede sağlanan kod sürümleri yapı hakkında bilgi listesini alır. CodeVersion parametresi, yalnızca bu belirli sürümü çıkışı isteğe bağlı olarak filtrelemek için kullanılabilir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --kod sürümü | Service Fabric ürün sürümü. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-cluster-config-versions"></a>sfctl küme config-sürümleri
Service Fabric kümesi içinde sağlanan yapılandırma sürümleri doku listesini alır.

Doku hakkında bilgi listesi kümede sağlanan yapılandırma sürümlerini alır. ConfigVersion parametresi, yalnızca bu belirli sürümü çıkışı isteğe bağlı olarak filtrelemek için kullanılabilir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --config sürümü | Service Fabric config sürümü. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-cluster-health"></a>sfctl küme durumu
Service Fabric kümesi durumunu alır.

Service Fabric kümesi durumunu alır. Sistem durumu olayları sistem durumuna bağlıdır küme üzerinde bildirilen koleksiyonu filtrelemek için EventsHealthStateFilter kullanın. Benzer şekilde, düğümleri ve toplanan sistem durumlarına dayalı döndürülen uygulamaların koleksiyonu filtrelemek için NodesHealthStateFilter ve ApplicationsHealthStateFilter kullanın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --uygulamaları Sistem Durumu Filtresi | Kendi sistem durumuna bağlıdır küme durumu sorgusunun sonucu döndürdü uygulama sistem durumu nesnelerinin filtrelemeye izin verir. Bu parametre için olası değerler üyeleri veya HealthStateFilter numaralandırma üyeleri üzerinde bit düzeyinde işlemler alınan tamsayı değeri içerir. Filtreyle eşleşen uygulamaları döndürülür. Tüm uygulamalar, toplanan sistem durumunu değerlendirmek için kullanılır. Belirtilmezse, tüm girişleri döndürülür. Durum değerleri bayrağı tabanlı numaralandırma olduğundan, değer, bu değerlerin Bitsel 'Veya' işleci kullanılarak edinilen bir bileşimi olabilir. Örneğin, 6 sağlanan değer ise, ardından Tamam (2) ve uyarı (4), HealthState değeriyle uygulamaların sistem durumu döndürülür.  <br> -Varsayılan - varsayılan değer. Tüm HealthState eşleşir. Değer sıfır olur.  <br> -Hiçbiri - herhangi bir HealthState değer eşleşmeyen filtreleyin. Sonuç durumları belirli bir koleksiyon döndürmek için kullanılır. Değer 1'dir.  <br> -Tamam - eşleşmeleri HealthState değerle Tamam giriş filtreleyin. Değer 2'dir.  <br> -Uyarı - filtre HealthState eşleşme girişle uyarı değer. Değer 4'tür.  <br> -Hata - Giriş hata HealthState değeriyle eşleşen Filtresi. Değer 8'dir.  <br> -Tüm - giriş herhangi bir HealthState değeri ile eşleşen filtre. Değer, 65535 ' dir. |
| --Sağlık Durumu Filtresi olayları | Döndürülen HealthEvent nesnelerin sistem durumuna bağlıdır koleksiyonu filtrelemeye izin verir. Bu parametre için olası değerler aşağıdaki sistem durumlarının bir tamsayı değeri içerir. Filtreyle eşleşen olaylar döndürülür. Tüm olayları toplanmış sistem durumunu değerlendirmek için kullanılır. Belirtilmezse, tüm girişleri döndürülür. Durum değerleri bayrağı tabanlı numaralandırma olduğundan, değer, bu değerlerin Bitsel 'Veya' işleci kullanılarak edinilen bir bileşimi olabilir. Sağlanan değer 6 ise, örneğin, ardından tüm olaylar Tamam (2) ve uyarı (4), HealthState değeriyle döndürülür.  <br> -Varsayılan - varsayılan değer. Tüm HealthState eşleşir. Değer sıfır olur.  <br> -Hiçbiri - herhangi bir HealthState değer eşleşmeyen filtreleyin. Sonuç durumları belirli bir koleksiyon döndürmek için kullanılır. Değer 1'dir.  <br> -Tamam - eşleşmeleri HealthState değerle Tamam giriş filtreleyin. Değer 2'dir.  <br> -Uyarı - filtre HealthState eşleşme girişle uyarı değer. Değer 4'tür.  <br> -Hata - Giriş hata HealthState değeriyle eşleşen Filtresi. Değer 8'dir.  <br> -Tüm - giriş herhangi bir HealthState değeri ile eşleşen filtre. Değer, 65535 ' dir. |
| --Dışlama sağlık istatistikleri | Sistem durumu istatistikleri sorgu sonucu bir parçası olarak döndürülüp döndürülmeyeceğini gösterir. Varsayılan değer false. Sistem durumu Tamam, uyarı ve hata istatistiklerini varlıklar alt sayısını gösterir. |
| --dahil-sistem-uygulama-sistem durumu-istatistikleri | Doku sistem durumu istatistikleri içerip içermeyeceğini belirten \: /sistem uygulaması sağlık istatistikleri. Varsayılan değer false. IncludeSystemApplicationHealthStatistics ayarlandıysa true, sistem durumu istatistikleri dokuya ait varlıklar eklenecek \: /sistem uygulaması. Aksi takdirde, sorgu sonucu kullanıcı uygulamaları için yalnızca sistem durumu istatistikleri içerir. Sistem durumu istatistikleri uygulanacak bu parametre için sorgu sonucu eklenmesi gerekir. |
| --düğümler sağlık Durumu Filtresi | Kendi sistem durumuna bağlıdır küme durumu sorgusunun sonucu döndürdü düğümü sağlık durumu nesnelerin filtrelemeye izin verir. Bu parametre için olası değerler aşağıdaki sistem durumlarının bir tamsayı değeri içerir. Filtreyle eşleşen düğümleri döndürülür. Tüm düğümleri toplanan sistem durumunu değerlendirmek için kullanılır. Belirtilmezse, tüm girişleri döndürülür. Durum değerleri bayrağı tabanlı numaralandırma olduğundan, değer, bu değerlerin Bitsel 'Veya' işleci kullanılarak edinilen bir bileşimi olabilir. Örneğin, sağlanan değer 6 ise ardından düğümler sistem durumunu Tamam (2) ve uyarı (4), HealthState değeriyle döndürülür.  <br> -Varsayılan - varsayılan değer. Tüm HealthState eşleşir. Değer sıfır olur.  <br> -Hiçbiri - herhangi bir HealthState değer eşleşmeyen filtreleyin. Sonuç durumları belirli bir koleksiyon döndürmek için kullanılır. Değer 1'dir.  <br> -Tamam - eşleşmeleri HealthState değerle Tamam giriş filtreleyin. Değer 2'dir.  <br> -Uyarı - filtre HealthState eşleşme girişle uyarı değer. Değer 4'tür.  <br> -Hata - Giriş hata HealthState değeriyle eşleşen Filtresi. Değer 8'dir.  <br> -Tüm - giriş herhangi bir HealthState değeri ile eşleşen filtre. Değer, 65535 ' dir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-cluster-manifest"></a>sfctl küme bildirimi
Service Fabric küme bildirimi alırsınız.

Service Fabric küme bildirimi alırsınız. Küme bildiriminde farklı bir düğüme türlerinde küme, güvenlik yapılandırmalarını, arıza ve yükseltme etki alanı topolojiler, vb. dahil küme özelliklerini içerir. Bu özellikler, tek başına bir küme dağıtırken ClusterConfig.JSON dosyasının bir parçası olarak belirtilir. Ancak, küme bildiriminde bilgilerin çoğunu oluşturulur dahili olarak service fabric tarafından diğer dağıtım senaryolarında Küme dağıtımı sırasında (örneğin Azure portalı kullanırken). Küme bildiriminde içerikleri yalnızca bilgilendirme amaçlıdır ve kullanıcıların dosya içeriğini veya kendi yorumlama biçimi üzerinde bir bağımlılık olması beklenmez.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-cluster-operation-cancel"></a>sfctl küme işlemi iptal etme
Bir kullanıcı kaynaklanan hata işlemi iptal eder.

Aşağıdaki API'leri CancelOperation kullanarak iptal edilebilir hataya işlemlerini başlatın: StartDataLoss, StartQuorumLoss, StartPartitionRestart, StartNodeTransition. Zorla false ise, ardından belirtilen kullanıcı indirgenen işlemi düzgün biçimde durduruldu ve olması temizlendi.  Zorla true ise, komut iptal edilecek ve bazı iç durum kalabilir.  Zorla doğru olarak belirterek dikkatli kullanılmalıdır. Zorla özelliği true olarak ayarlanmış bu API'yi çağıran bu API false ilk zorla kümesine aynı test komutuyla üzerinde zaten çağrılmış kadar ya da test komutu, bir OperationState OperationState.RollingBack olmadıkça izin verilmez. 

Açıklama\: sistem olacaktır/iç sistemi temizleme OperationState.RollingBack anlamına gelir durum neden komutunu yürüterek. Sınama komutunu veri kaybına neden olursa veri geri yüklenmeyecek.  StartDataLoss çağrısı sonra bu API çağrısı, örneğin, sistem yalnızca komutu çalışmasını iç durumu temizler. Komut yetecek kadar veri kaybına neden progressed varsa hedef bölümünün veri geri yüklenmeyecek. 

> [!NOTE]
> Bu API ile zorla çağrılırsa == true, iç durumu geride.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --işlem kimliği [gerekli] | Bu API çağrısının tanımlayan bir GUID.  Bu, karşılık gelen GetProgress API geçirilir. |
| --zorla | Düzgün bir şekilde geri alma ve kullanıcı indirgenen işlemi yürüterek değiştirilmiş iç sistem durumu temizleme gösterir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-cluster-operation-list"></a>sfctl küme işlemi listesi
Kullanıcı kaynaklanan hata işlemleri tarafından sağlanan girişin filtrelenmiş bir listesini alır.

Kullanıcı kaynaklanan hata işlemleri tarafından sağlanan girişin filtre listesini alır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Durum Filtresi | OperationState üzerinde ait kullanıcı indirgenen işlemleri için filtre uygulamak için kullanılır. <br> 65535 - Tümünü Seç <br> 1 - çalışan seçin <br> 2 - RollingBack seçin <br>8 - tamamlandı seçin <br>16 - Faulted seçin <br>32 - iptal edildi seçin <br>64 - ForceCancelled seçin.  <br>Varsayılan\: 65535. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |
| --Türü Filtresi | Kullanıcı indirgenen işlemleri için üzerinde OperationType filtrelemek için kullanılır. <br> 65535 - Tümünü Seç <br> 1 - PartitionDataLoss seçin. <br> 2 - PartitionQuorumLoss seçin. <br> 4 - PartitionRestart seçin. <br> 8 - NodeTransition seçin.  <br> Varsayılan\: 65535. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-cluster-provision"></a>sfctl küme sağlama
Service Fabric kümesi kod veya yapılandırma paketleri sağlayın.

Doğrulama ve Service Fabric kümesi kod veya yapılandırma paketleri sağlayın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Küme bildirimi dosyası yolu | Küme bildirimi dosyası yolu. |
| --kod dosyasının yolu | Küme kod paketi dosyasının yolu. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-cluster-recover-system"></a>sfctl küme recover-sistem
Service Fabric kümesi şu anda çekirdek kaybında takıldı Sistem Hizmetleri kurtarmayı denemesi belirtir.

Service Fabric kümesi şu anda çekirdek kaybında takıldı Sistem Hizmetleri kurtarmayı denemesi belirtir. Bu işlem, yalnızca kapalı çoğaltmaları kurtarılamıyor biliniyorsa gerçekleştirilmelidir. Bu API yanlış kullanımı, olası veri kaybına neden olabilir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-cluster-report-health"></a>sfctl küme rapor-durum
Service Fabric kümesi üzerinde bir sistem durumu raporu gönderir.

Rapor kaynağı özelliği üzerinde bildirilir ve sistem durumu raporu hakkında bilgiler içermelidir. Rapor sistem durumu depoya ileten bir Service Fabric ağ geçidi düğümü gönderilir. Rapor ağ geçidi tarafından kabul edilen, ancak sonra ek doğrulama durumu mağaza tarafından reddedildi. Örneğin, sistem sağlığı deposunu raporu eski sıra numarası gibi geçersiz bir parametre nedeniyle reddedebilir. Health store içinde rapor uygulanıp uygulanmadığını görmek için raporu küme HealthEvents içinde görünür denetleyin.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Sistem durumu özelliği [gerekli] | Sistem durumu bilgisi özelliği. <br><br> Bir varlığın farklı özellikler için sistem durumu raporlarının olabilir. , Bir dize ve rapor tetikler durumu koşulu kategorilere ayırmak Raporlayıcı esnekliğini tanımak için olmayan bir sabit numaralandırması özelliğidir. Örneğin, bunu "AvailableDisk" özelliği bu düğümde raporlamak için bir Raporlayıcı SourceId "LocalWatchdog" ile bir düğümde, kullanılabilir disk durumunu izleyebilirsiniz. Bu özellik "Bağlantı" aynı düğümde raporlamak için aynı Raporlayıcı düğümü bağlantı izleyebilirsiniz. Health store içinde bu raporları ayrı sistem durumu olayları için belirtilen düğümün olarak kabul edilir. SourceId birlikte özelliği sistem durumu bilgilerini benzersiz şekilde tanımlar. |
| --Sistem durumu [gerekli] | Olası değerler şunlardır\: 'Geçersiz', 'Tamam', 'Uyarı', 'Error', 'Bilinmeyen'. |
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

## <a name="sfctl-cluster-select"></a>sfctl küme seçin
Service Fabric kümesi uç noktasına bağlanır.

Güvenli kümesine bağlanma, hem (.pem) bir sertifika (.crt) ve anahtar dosyası (.key) veya tek bir dosya için mutlak bir yol belirtin. Her ikisini birden belirtmeyin. İsteğe bağlı olarak, güvenli bir kümeye bağlanılıyorsa, ayrıca bir CA paket dosyasını veya güvenilir CA sertifikaları dizininin mutlak bir yol belirtin.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --[gerekli] uç noktası | Uç nokta URL'si, bağlantı noktası ve HTTP veya HTTPS önekini dahil olmak üzere küme. |
| --aad | Azure Active Directory kimlik doğrulaması için kullanın. |
| --ca | Geçerli olarak işlemek için CA sertifikaları dizini veya CA paket dosyasını mutlak yolu. |
| --Sertifika | Bir istemci sertifikası dosyası mutlak yolu. |
| --anahtarı | İstemci sertifika anahtar dosyası mutlak yolu. |
| --yok doğrulayın | Doğrulama sertifikaları için HTTPS kullanırken devre dışı bırakmak, Not\: bu güvenli bir seçenektir ve üretim ortamları için kullanılmamalıdır. |
| --pem | .Pem dosyası olarak istemci sertifikası mutlak yolu. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-cluster-unprovision"></a>sfctl küme sağlamayı kaldırma
Service Fabric kümesi kod veya yapılandırma paketleri sağlama.

Service Fabric kümesi kod veya yapılandırma paketleri sağlama. Kod ve yapılandırma ayrı olarak sağlamasını desteklenir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --kod sürümü | Küme kodu Paket sürümü. |
| --config sürümü | Küme bildirimi sürümü. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-cluster-upgrade"></a>sfctl Küme yükseltme
Service Fabric kümesi kod veya yapılandırma sürümüne yükseltme başlatın.

Sağlanan yükseltme parametreleri doğrulayın ve parametreleri geçerli ise bir Service Fabric kümesi kod veya yapılandırma sürümüne yükseltme başlatın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Uygulama sistem durumu eşleme | Uygulama adı ve en yüksek yüzde hata oluşmadan önce sağlıklı çiftleri JSON olarak kodlanmış sözlüğü. |
| --uygulama türü sistem durumu eşleme | Uygulama türü adı ve en yüksek yüzde hata oluşmadan önce sağlıklı çiftleri JSON olarak kodlanmış sözlüğü. |
| --kod sürümü | Küme kod sürümü. |
| --config sürümü | Küme yapılandırması sürümü. |
| --delta sistem durumu değerlendirmesi | Her bir yükseltme etki alanı tamamlanmasından sonra mutlak sistem durumu değerlendirmesi yerine delta sistem durumu değerlendirmesi sağlar. |
| --delta sağlıksız-düğümler | En düğümlerinin yüzdesi Küme yükseltme sırasında izin verilen sistem durumu düşüşü izin verilir.  Varsayılan\: 10. <br><br> Delta yükseltme işleminin başında düğümlerinin durumunu ve sistem durumu değerlendirmesi zaman düğümlerin durumunun arasında ölçülür. Onay, kümenin genel durumunu toleranslı sınırlarda olduğundan emin olmak için her yükseltme etki alanı yükseltme tamamlandıktan sonra gerçekleştirilir. |
| --hatası eylemi | Olası değerler şunlardır\: 'Geçersiz', 'Geri', 'Manual'. |
| --zorla yeniden başlatma | Yeniden başlatma. |
| --Sistem durumu denetimi yeniden | Sistem durumu denetimi yeniden deneme zaman aşımı, milisaniye cinsinden ölçülür. |
| --Sistem durumu denetimi kararlı | Sistem durumu denetimi kararlı süresini milisaniye olarak ölçülür. |
| --Sistem durumu denetimi bekleme | Sistem durumu denetimi bekleme süresini milisaniye olarak ölçülür. |
| --çoğaltma kümesi onay aşımı | Yükseltme çoğaltma onay zaman aşımı saniye cinsinden ölçülen ayarlayın. |
| --çalışırken yükseltme-modu | Olası değerler şunlardır\: 'Geçersiz', 'UnmonitoredAuto', 'UnmonitoredManual', 'İzlenen'.  Varsayılan\: UnmonitoredAuto. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |
| --sağlıksız uygulamaları | Sağlıksız uygulamaları yüzdesi hata raporlamadan önce izin verilen en fazla. <br><br> Örneğin, %10 sağlıksız uygulamalarının izin vermek için bu değer 10 olur. Yüzdesini küme hata olarak kabul edilmeden önce sağlıksız uygulamaları maksimum toleranslı yüzdesini temsil eder. Yüzde dikkate ancak en az bir düzgün çalışmayan uygulama olduğundan, sistem durumu uyarı olarak değerlendirilir. Bu uygulama örnekleri ApplicationTypeHealthPolicyMap içerdiği uygulama türleri uygulamaların hariç kümedeki toplam sayısı üzerinden sağlıksız uygulamaları sayısının bölünmesiyle hesaplanır. Küçük sayıda uygulamalar üzerinde bir hatasını tolere için hesaplama yukarı yuvarlar. |
| --sağlıksız düğümleri | Sağlıksız düğümleri yüzdesi hata raporlamadan önce izin verilen en fazla. <br><br> Örneğin, %10 sağlıksız düğümlerinin izin vermek için bu değer 10 olur. Yüzde küme hata olarak kabul edilmeden önce sağlıksız düğümleri maksimum toleranslı yüzdesini temsil eder. Yüzde dikkate halde en az bir sağlıksız düğüm yoksa, sistem durumu uyarı olarak değerlendirilir. Yüzde, kümedeki düğümler toplam sayısı üzerinden sağlıksız düğüm sayısını bölünmesiyle hesaplanır. Küçük sayıda düğüm üzerinde bir hatasını tolere için hesaplama yukarı yuvarlar. Bu yüzde, tolerans şekilde yapılandırılmalıdır büyük kümelerinde, bazı düğümler her zaman aşağı veya çıkışı onarım için olacağından. |
| --Yükseltme etki alanı-delta-sağlıksız-düğümler | Sistem durumu düşüşü Küme yükseltme sırasında izin izin verilen en fazla yükseltme etki alanı düğümleri yüzdesi.  Varsayılan\: 15. <br><br> Delta yükseltme işleminin başında yükseltme etki alanı düğümleri durumunu ve sistem durumu değerlendirmesi zaman yükseltme etki alanı düğümleri durumunu arasında ölçülür. Yükseltme etki alanlarının durumunu toleranslı sınırlarda olduğundan emin olmak için yükseltme etki alanlarının her bir yükseltme etki alanı yükseltme tamamlama tüm tamamlandıktan sonra denetimi gerçekleştirilir. |
| --Yükseltme etki alanı timeout | Yükseltme etki alanı zaman aşımı, milisaniye cinsinden ölçülür. |
| --Yükseltme zaman aşımı | Yükseltme zaman aşımı, milisaniye cinsinden ölçülür. |
| --hata olarak uyarı | Uyarılar aynı önem derecesi hata olarak kabul edilir. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-cluster-upgrade-resume"></a>sfctl Küme yükseltme-Sürdür
Bir sonraki yükseltme etki alanına taşıma Küme yükseltme yapın.

Küme kod veya yapılandırma yükseltme üzerinde bir sonraki yükseltme etki alanına uygun durumlarda harekete geçin.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Yükseltme [gerekli] etki alanı | Bir sonraki yükseltme etki alanı için bu Küme yükseltme. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-cluster-upgrade-rollback"></a>sfctl Küme yükseltme-geri alma
Service Fabric kümesini yükseltme işlemi geri alın.

Service Fabric kümesi, kod veya yapılandırma yükseltmeyi geri alma.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-cluster-upgrade-status"></a>sfctl Küme yükseltme-durum
Geçerli Küme yükseltmesinin ilerleme durumunu alır.

Devam eden Küme yükseltme geçerli durumunu alır. Şu anda hiçbir yükseltme devam ediyor, en son önceki Küme yükseltme durumunu alın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-cluster-upgrade-update"></a>sfctl Küme yükseltme-güncelleştirme
Service Fabric Küme Yükseltme yükseltme parametreleri güncelleştirin.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Uygulama sistem durumu eşleme | Uygulama adı ve en yüksek yüzde hata oluşmadan önce sağlıklı çiftleri JSON olarak kodlanmış sözlüğü. |
| --uygulama türü sistem durumu eşleme | Uygulama türü adı ve en yüksek yüzde hata oluşmadan önce sağlıklı çiftleri JSON olarak kodlanmış sözlüğü. |
| --delta sistem durumu değerlendirmesi | Her bir yükseltme etki alanı tamamlanmasından sonra mutlak sistem durumu değerlendirmesi yerine delta sistem durumu değerlendirmesi sağlar. |
| --delta sağlıksız-düğümler | En düğümlerinin yüzdesi Küme yükseltme sırasında izin verilen sistem durumu düşüşü izin verilir.  Varsayılan\: 10. <br><br> Delta yükseltme işleminin başında düğümlerinin durumunu ve sistem durumu değerlendirmesi zaman düğümlerin durumunun arasında ölçülür. Onay, kümenin genel durumunu toleranslı sınırlarda olduğundan emin olmak için her yükseltme etki alanı yükseltme tamamlandıktan sonra gerçekleştirilir. |
| --hatası eylemi | Olası değerler şunlardır\: 'Geçersiz', 'Geri', 'Manual'. |
| --zorla yeniden başlatma | Yeniden başlatma. |
| --Sistem durumu denetimi yeniden | Sistem durumu denetimi yeniden deneme zaman aşımı, milisaniye cinsinden ölçülür. |
| --Sistem durumu denetimi kararlı | Sistem durumu denetimi kararlı süresini milisaniye olarak ölçülür. |
| --Sistem durumu denetimi bekleme | Sistem durumu denetimi bekleme süresini milisaniye olarak ölçülür. |
| --çoğaltma kümesi onay aşımı | Yükseltme çoğaltma onay zaman aşımı saniye cinsinden ölçülen ayarlayın. |
| --çalışırken yükseltme-modu | Olası değerler şunlardır\: 'Geçersiz', 'UnmonitoredAuto', 'UnmonitoredManual', 'İzlenen'.  Varsayılan\: UnmonitoredAuto. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |
| --sağlıksız uygulamaları | Sağlıksız uygulamaları yüzdesi hata raporlamadan önce izin verilen en fazla. <br><br> Örneğin, %10 sağlıksız uygulamalarının izin vermek için bu değer 10 olur. Yüzdesini küme hata olarak kabul edilmeden önce sağlıksız uygulamaları maksimum toleranslı yüzdesini temsil eder. Yüzde dikkate ancak en az bir düzgün çalışmayan uygulama olduğundan, sistem durumu uyarı olarak değerlendirilir. Bu uygulama örnekleri ApplicationTypeHealthPolicyMap içerdiği uygulama türleri uygulamaların hariç kümedeki toplam sayısı üzerinden sağlıksız uygulamaları sayısının bölünmesiyle hesaplanır. Küçük sayıda uygulamalar üzerinde bir hatasını tolere için hesaplama yukarı yuvarlar. |
| --sağlıksız düğümleri | Sağlıksız düğümleri yüzdesi hata raporlamadan önce izin verilen en fazla. <br><br> Örneğin, %10 sağlıksız düğümlerinin izin vermek için bu değer 10 olur. Yüzde küme hata olarak kabul edilmeden önce sağlıksız düğümleri maksimum toleranslı yüzdesini temsil eder. Yüzde dikkate halde en az bir sağlıksız düğüm yoksa, sistem durumu uyarı olarak değerlendirilir. Yüzde, kümedeki düğümler toplam sayısı üzerinden sağlıksız düğüm sayısını bölünmesiyle hesaplanır. Küçük sayıda düğüm üzerinde bir hatasını tolere için hesaplama yukarı yuvarlar. Bu yüzde, tolerans şekilde yapılandırılmalıdır büyük kümelerinde, bazı düğümler her zaman aşağı veya çıkışı onarım için olacağından. |
| --Yükseltme etki alanı-delta-sağlıksız-düğümler | Sistem durumu düşüşü Küme yükseltme sırasında izin izin verilen en fazla yükseltme etki alanı düğümleri yüzdesi.  Varsayılan\: 15. <br><br> Delta yükseltme işleminin başında yükseltme etki alanı düğümleri durumunu ve sistem durumu değerlendirmesi zaman yükseltme etki alanı düğümleri durumunu arasında ölçülür. Yükseltme etki alanlarının durumunu toleranslı sınırlarda olduğundan emin olmak için yükseltme etki alanlarının her bir yükseltme etki alanı yükseltme tamamlama tüm tamamlandıktan sonra denetimi gerçekleştirilir. |
| --Yükseltme etki alanı timeout | Yükseltme etki alanı zaman aşımı, milisaniye cinsinden ölçülür. |
| --yükseltme türü | Olası değerler şunlardır\: 'Geçersiz', 'Alınıyor', 'Rolling_ForceRestart'.  Varsayılan\: alınıyor. |
| --Yükseltme zaman aşımı | Yükseltme zaman aşımı, milisaniye cinsinden ölçülür. |
| --hata olarak uyarı | Uyarılar aynı önem derecesi hata olarak kabul edilir. |

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