---
title: "Azure Service Fabric CLI - sfctl küme | Microsoft Docs"
description: "Service Fabric CLI sfctl küme komutlarını açıklar."
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
ms.date: 02/22/2018
ms.author: ryanwi
ms.openlocfilehash: c83dc3eeb6ca0d66b0c70236354fd7bab80f355f
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="sfctl-cluster"></a>sfctl cluster
Seçin, yönetmek ve Service Fabric kümeleri çalışmayabilir.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
|    kod sürümleri| Service Fabric kümesi içinde sağlanan kod sürümleri doku listesini alır.|
|    config-versions | Service Fabric kümesi içinde sağlanan yapılandırma sürümleri doku listesini alır.|
|    sistem durumu       | Service Fabric kümesi durumunu alır.|
|    Bildirimi     | Service Fabric küme bildirimi alırsınız.|
|    işlemi iptal etme| Bir kullanıcı kaynaklanan hata işlemi iptal eder.|
|    operationgit | Kullanıcı kaynaklanan hata işlemleri tarafından sağlanan girişin filtrelenmiş bir listesini alır.|
|    Sağlama     | Service Fabric kümesi kod veya yapılandırma paketleri sağlayın.|
|    Sistem Kurtarma  | Service Fabric kümesi şu anda çekirdek kaybında takıldı Hizmetler'i kurtarmayı denemesi belirtir.|
|Sistem Durumu raporu   | Service Fabric kümesi üzerinde bir sistem durumu raporu gönderir.|
|    seç       | Service Fabric kümesi uç noktasına bağlanır.|
| Sağlamayı kaldırma     | Service Fabric kümesi kod veya yapılandırma paketleri sağlama.|
|    Yükseltme         | Service Fabric kümesi kod veya yapılandırma sürümüne yükseltme başlatın.|
|    upgrade-resume  | Bir sonraki yükseltme etki alanına taşıma Küme yükseltme yapın.|
|    upgrade-rollback| Service Fabric kümesini yükseltme işlemi geri alın.|
|    Yükseltme durumu  | Geçerli Küme yükseltmesinin ilerleme durumunu alır.|
|upgrade-update  | Service Fabric Küme Yükseltme yükseltme parametreleri güncelleştirin.|


## <a name="sfctl-cluster-health"></a>sfctl küme durumu
Service Fabric kümesi durumunu alır.

Service Fabric kümesi durumunu alır. Sistem durumu olayları sistem durumuna bağlıdır küme üzerinde bildirilen koleksiyonu filtrelemek için EventsHealthStateFilter kullanın. Benzer şekilde, düğümleri ve toplanan sistem durumlarına dayalı döndürülen uygulamaların koleksiyonu filtrelemek için NodesHealthStateFilter ve ApplicationsHealthStateFilter kullanın. 

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --uygulamaları Sistem Durumu Filtresi| Kendi sistem durumuna bağlıdır küme durumu sorgusunun sonucu döndürdü uygulama sistem durumu nesnelerinin filtrelemeye izin verir. Bu parametre için olası değerler üyeleri veya HealthStateFilter numaralandırma üyeleri üzerinde bit düzeyinde işlemler alınan tamsayı değeri içerir. Filtreyle eşleşen uygulamaları döndürülür.  Tüm uygulamalar, toplanan sistem durumunu değerlendirmek için kullanılır. Belirtilmezse, tüm girişleri döndürülür. Değer, bu değerlerin Bitsel 'Veya' işleci kullanılarak edinilen bir bileşimi olabilir bayrağı numaralandırma, dayalı, bu yüzden durumu değerlerdir. Örneğin, 6 sağlanan değer ise, ardından Tamam (2) ve uyarı (4), HealthState değeriyle uygulamaların sistem durumu döndürülür. -Varsayılan - varsayılan değer. Tüm HealthState eşleşir. Değer sıfır olur. -Hiçbiri - herhangi bir HealthState değer eşleşmeyen filtreleyin. Sonuç durumları belirli bir koleksiyon döndürmek için kullanılır. Değer 1'dir. -Tamam - eşleşmeleri HealthState değerle Tamam giriş filtreleyin. Değer 2'dir. -Uyarı - filtre HealthState eşleşme girişle uyarı değer. Değer 4'tür. -Hata - Giriş hata HealthState değeriyle eşleşen Filtresi. Değer 8'dir. -Tüm - giriş herhangi bir HealthState değeri ile eşleşen filtre. Değer, 65535 ' dir.|
| --events-health-state-filter   | Döndürülen HealthEvent nesnelerin sistem durumuna bağlıdır koleksiyonu filtrelemeye izin verir. Bu parametre için olası değerler aşağıdaki sistem durumlarının bir tamsayı değeri içerir. Filtreyle eşleşen olaylar döndürülür. Tüm olayları toplanmış sistem durumunu değerlendirmek için kullanılır. Belirtilmezse, tüm girişleri döndürülür. Değer, bu değerlerin Bitsel 'Veya' işleci kullanılarak edinilen bir bileşimi olabilir şekilde bayrağı numaralandırma, tabanlı durumu değerlerdir. Sağlanan değer 6 ise, örneğin, ardından tüm olaylar Tamam (2) ve uyarı (4), HealthState değeriyle döndürülür. -Varsayılan - varsayılan değer. Tüm HealthState eşleşir. Değer sıfır olur. -Hiçbiri - herhangi bir HealthState değer eşleşmeyen filtreleyin. Sonuç durumları belirli bir koleksiyon döndürmek için kullanılır. Değer 1'dir. -Tamam - eşleşmeleri HealthState değerle Tamam giriş filtreleyin. Değer 2'dir. -Uyarı - filtre HealthState eşleşme girişle uyarı değer.  Değer 4'tür. -Hata - Giriş hata HealthState değeriyle eşleşen Filtresi. Değer 8'dir. -Tüm - giriş herhangi bir HealthState değeri ile eşleşen filtre. Değer, 65535 ' dir.|
|--exclude-health-statistics                   | Sistem durumu istatistikleri sorgu sonucu bir parçası olarak döndürülüp döndürülmeyeceğini gösterir. Varsayılan değer false. Sistem durumu Tamam, uyarı ve hata istatistiklerini varlıklar alt sayısını gösterir.|
 |   --include-system-application-health-statistics| Doku sistem durumu istatistikleri içerip içermeyeceğini gösterir: / Sistem uygulaması sağlık istatistikleri. Varsayılan değer false. IncludeSystemApplicationHealthStatistics ayarlanmışsa, true olarak sistem istatistikleri dokuya ait varlıkları içerir: / Sistem uygulaması. Aksi takdirde, sorgu sonucu kullanıcı uygulamaları için yalnızca sistem durumu istatistikleri içerir. Sistem durumu istatistikleri uygulanacak bu parametre için sorgu sonucu eklenmesi gerekir.|
| --düğümler sağlık Durumu Filtresi    | Kendi sistem durumuna bağlıdır küme durumu sorgusunun sonucu döndürdü düğümü sağlık durumu nesnelerin filtrelemeye izin verir. Bu parametre için olası değerler aşağıdaki sistem durumlarının bir tamsayı değeri içerir. Filtreyle eşleşen düğümleri döndürülür. Tüm düğümleri toplanan sistem durumunu değerlendirmek için kullanılır. Belirtilmezse, tüm girişleri döndürülür. Durum değerleri bayrağı tabanlı numaralandırma olduğundan, değer, bu değerlerin Bitsel 'Veya' işleci kullanılarak edinilen bir bileşimi olabilir. Örneğin, "6" sağlanan değer ise ardından düğümler sistem durumunu Tamam (2) ve uyarı (4), HealthState değeriyle döndürülür. -Varsayılan - varsayılan değer. Tüm HealthState eşleşir. Değer sıfır olur. -Hiçbiri - herhangi bir HealthState değer eşleşmeyen filtreleyin. Sonuç durumları belirli bir koleksiyon döndürmek için kullanılır. Değer 1'dir. -Tamam - eşleşmeleri HealthState değerle Tamam giriş filtreleyin. Değer 2'dir. -Uyarı - filtre HealthState eşleşme girişle uyarı değer.  Değer 4'tür. -Hata - Giriş hata HealthState değeriyle eşleşen Filtresi. Değer 8'dir. -Tüm - giriş herhangi bir HealthState değeri ile eşleşen filtre. Değer, 65535 ' dir.|
| --zaman aşımı -t                   | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --debug                        | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| --help -h                      | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı                    | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.                    Varsayılan: json.|
| --Sorgu                        | JMESPath sorgu dizesi. Http://jmespath.org/ daha fazla bilgi ve örnekler için bkz.|
| --verbose                      | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-cluster-manifest"></a>sfctl küme bildirimi
Service Fabric küme bildirimi alırsınız.

Service Fabric küme bildirimi alırsınız. Küme bildiriminde küme, güvenlik yapılandırmalarını, hata ve yükseltme etki alanı topolojileri vb. farklı bir düğüme türler küme özelliklerini içerir. Bu özellikler, tek başına küme dağıtırken ClusterConfig.JSON dosyasının bir parçası olarak belirtilir. Ancak, küme bildiriminde bilgilerin çoğunu oluşturulur dahili olarak service fabric tarafından diğer dağıtım senaryolarında Küme dağıtımı sırasında (kullanırken, örneğin [Azure portal](https://portal.azure.com)). Küme bildiriminde içerikleri yalnızca bilgilendirme amaçlıdır ve kullanıcıların dosya içeriğini veya kendi yorumlama biçimi üzerinde bir bağımlılık olması beklenmez. 

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --zaman aşımı -t| Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --debug  | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| --help -h| Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu  | JMESPath sorgu dizesi. Http://jmespath.org/ daha fazla bilgi ve örnekler için bkz.|
| --verbose| Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-cluster-provision"></a>sfctl küme sağlama
Service Fabric kümesi kod veya yapılandırma paketleri sağlayın.
Doğrulama ve Service Fabric kümesi kod veya yapılandırma paketleri sağlayın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
|--cluster-manifest-file-path| Küme bildirimi dosyası yolu.|
|    --code-file-path            | Küme kod paketi dosyasının yolu.|
|    --zaman aşımı -t                | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --debug | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| --help -h  | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı| Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu | JMESPath sorgu dizesi. Http://jmespath.org/ daha fazla bilgi ve örnekler için bkz.|
| --verbose  | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-cluster-select"></a>sfctl küme seçin
Service Fabric kümesi uç noktasına bağlanır.

Güvenli kümesine bağlanma, hem (.pem) bir sertifika (.crt) ve anahtar dosyası (.key) veya tek bir dosya belirtin. Her ikisini birden belirtmeyin. İsteğe bağlı olarak, güvenli bir kümeye bağlanılıyorsa, ayrıca bir CA paket dosyası ya da güvenilir CA sertifikaları dizinin yolunu belirtin.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --[gerekli] uç noktası| Uç nokta URL'si, bağlantı noktası ve HTTP veya HTTPS önekini dahil olmak üzere küme.|
| --aad             | Azure Active Directory kimlik doğrulaması için kullanın.|
| --ca              | Geçerli olarak işlemek için CA sertifikaları dizini veya CA paket dosyası yolu.|
| --cert            | Bir istemci sertifikası dosyasının yolu.|
| --anahtarı             | İstemci sertifikası anahtar dosyasının yolu.|
| --yok doğrulayın       | Doğrulama sertifikaları için HTTPS kullanırken devre dışı bırakmak, Not: Bu güvenli bir seçenektir ve üretim ortamları için kullanılmamalıdır.|
| --pem             | .Pem dosyası olarak istemci sertifikası yolu.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --debug           | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| --help -h         | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı       | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu           | JMESPath sorgu dizesi. Http://jmespath.org/ daha fazla bilgi ve örnekler için bkz.|
| --verbose         | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-cluster-unprovision"></a>sfctl küme sağlamayı kaldırma
Service Fabric kümesi kod veya yapılandırma paketleri sağlama.

Service Fabric kümesi kod veya yapılandırma paketleri sağlama. Kod ve yapılandırma ayrı olarak sağlamasını desteklenir.

### <a name="arguments"></a>Bağımsız Değişkenler
|Bağımsız değişken|Açıklama|
| --- | --- |
|--code-version  | Küme kodu Paket sürümü.|
|    --config-version| Küme bildirimi sürümü.|
|    --zaman aşımı -t    | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler
|Bağımsız değişken|Açıklama|
| --- | --- |
|--debug         | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
 |   --help -h       | Bu yardım iletisini ve çıkış gösterir.|
 |   ---o çıktı     | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
 |   --Sorgu         | JMESPath sorgu dizesi. Http://jmespath.org/ daha fazla bilgi ve örnekler için bkz.|
 |   --verbose       | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|


## <a name="sfctl-cluster-upgrade"></a>sfctl Küme yükseltme
Service Fabric kümesi kod veya yapılandırma sürümüne yükseltme başlatın.
Sağlanan yükseltme parametreleri doğrulayın ve parametreleri geçerli ise bir Service Fabric kümesi kod veya yapılandırma sürümüne yükseltme başlatın.

### <a name="arguments"></a>Bağımsız Değişkenler
|Bağımsız değişken|Açıklama|
| --- | --- |
|    --Uygulama sistem durumu eşleme                      | Uygulama adı ve en yüksek yüzde hata oluşmadan önce sağlıklı çiftleri JSON olarak kodlanmış sözlüğü.|
 |   --uygulama türü sistem durumu eşleme                 | Uygulama türü adı ve en yüksek yüzde hata oluşmadan önce sağlıklı çiftleri JSON olarak kodlanmış sözlüğü.|
 |   --code-version                        | Küme kod sürümü.|
 |   --config-version                      | Küme yapılandırması sürümü.|
 |   --delta sistem durumu değerlendirmesi             | Her bir yükseltme etki alanı tamamlanmasından sonra mutlak sistem durumu değerlendirmesi yerine delta sistem durumu değerlendirmesi sağlar.|
 |   --delta sağlıksız-düğümler               | En düğümlerinin yüzdesi Küme yükseltme sırasında izin verilen sistem durumu düşüşü izin verilir.  Varsayılan: 10. Delta yükseltme işleminin başında düğümlerinin durumunu ve sistem durumu değerlendirmesi zaman düğümlerin durumunun arasında ölçülür. Onay, kümenin genel durumunu toleranslı sınırlarda olduğundan emin olmak için her yükseltme etki alanı yükseltme tamamlandıktan sonra gerçekleştirilir.|
 |   --hatası eylemi                      | Olası değerler şunlardır: 'Geçersiz', 'Geri', 'Manual'.|
 |   --zorla yeniden başlatma                       | Yeniden başlatma.|
 |   --health-check-retry                  | Sistem durumu denetimi yeniden deneme zaman aşımı, milisaniye cinsinden ölçülür.|
 |   --Sistem durumu denetimi kararlı                 | Sistem durumu denetimi kararlı süresini milisaniye olarak ölçülür.|
  |  --health-check-wait                   | Sistem durumu denetimi bekleme süresini milisaniye olarak ölçülür.|
  |  --replica-set-check-timeout           | Yükseltme çoğaltma onay zaman aşımı saniye cinsinden ölçülen ayarlayın.|
 |   --rolling-upgrade-mode                | Olası değerler şunlardır: 'Geçersiz', 'UnmonitoredAuto', 'UnmonitoredManual', 'İzlenen'.  Varsayılan: UnmonitoredAuto.|
  |  --zaman aşımı -t                          | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|
  |  --sağlıksız uygulamaları              | Sağlıksız uygulamaları yüzdesi hata raporlamadan önce izin verilen en fazla. Örneğin, %10 sağlıksız uygulamalarının izin vermek için bu değer 10 olur. Yüzdesini küme hata olarak kabul edilmeden önce sağlıksız uygulamaları maksimum toleranslı yüzdesini temsil eder. Yüzde dikkate ancak en az bir düzgün çalışmayan uygulama olduğundan, sistem durumu uyarı olarak değerlendirilir. Bu uygulama örnekleri ApplicationTypeHealthPolicyMap içerdiği uygulama türleri uygulamaların hariç kümedeki toplam sayısı üzerinden sağlıksız uygulamaları sayısının bölünmesiyle hesaplanır. Küçük sayıda uygulamalar üzerinde bir hatasını tolere için hesaplama yukarı yuvarlar.|
 |   --sağlıksız düğümleri                     | Sağlıksız düğümleri yüzdesi hata raporlamadan önce izin verilen en fazla. Örneğin, %10 sağlıksız düğümlerinin izin vermek için bu değer 10 olur. Yüzde küme hata olarak kabul edilmeden önce sağlıksız düğümleri maksimum toleranslı yüzdesini temsil eder. Yüzde dikkate halde en az bir sağlıksız düğüm yoksa, sistem durumu uyarı olarak değerlendirilir. Yüzde, kümedeki düğümler toplam sayısı üzerinden sağlıksız düğüm sayısını bölünmesiyle hesaplanır. Küçük sayıda düğüm üzerinde bir hatasını tolere için hesaplama yukarı yuvarlar. Bu yüzde, tolerans şekilde yapılandırılmalıdır büyük kümelerinde, bazı düğümler her zaman aşağı veya çıkışı onarım için olacağından.|
 |   --upgrade-domain-delta-unhealthy-nodes| Sistem durumu düşüşü Küme yükseltme sırasında izin izin verilen en fazla yükseltme etki alanı düğümleri yüzdesi. Varsayılan: 15. Delta yükseltme işleminin başında yükseltme etki alanı düğümleri durumunu ve sistem durumu değerlendirmesi zaman yükseltme etki alanı düğümleri durumunu arasında ölçülür. Yükseltme etki alanlarının durumunu toleranslı sınırlarda olduğundan emin olmak için yükseltme etki alanlarının her bir yükseltme etki alanı yükseltme tamamlama tüm tamamlandıktan sonra denetimi gerçekleştirilir.|
 |   --upgrade-domain-timeout              | Yükseltme etki alanı zaman aşımı, milisaniye cinsinden ölçülür.|
 |   --upgrade-timeout                     | Yükseltme zaman aşımı, milisaniye cinsinden ölçülür.|
 |   --warning-as-error                    | Uyarılar aynı önem derecesi hata olarak kabul edilir.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler
|Bağımsız değişken|Açıklama|
| --- | --- |
|--debug                               | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
|    --help -h                             | Bu yardım iletisini ve çıkış gösterir.|
|    ---o çıktı                           | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv. Varsayılan: json.|
|    --Sorgu                               | JMESPath sorgu dizesi. Http://jmespath.org/ daha fazla bilgi ve örnekler için bkz.|
|    --verbose                             | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="next-steps"></a>Sonraki adımlar
- [Kurulum](service-fabric-cli.md) Service Fabric CLI.
- Service Fabric CLI kullanarak kullanmayı öğrenin [örnek komutlar](/azure/service-fabric/scripts/sfctl-upgrade-application).