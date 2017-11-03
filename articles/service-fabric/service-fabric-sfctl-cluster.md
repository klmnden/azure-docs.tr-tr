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
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 09/22/2017
ms.author: ryanwi
ms.openlocfilehash: dc2ed59d6adaca97b23dddcb7ec968d90171b483
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="sfctl-cluster"></a>sfctl küme
Seçin, yönetmek ve Service Fabric kümeleri çalışmayabilir.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
|    kod sürümleri| Service Fabric kümesi içinde sağlanan kod sürümleri doku listesini alır.|
|    config sürümleri | Service Fabric kümesi içinde sağlanan yapılandırma sürümleri doku listesini alır.|
|    Sistem durumu       | Service Fabric kümesi durumunu alır.|
|    Bildirimi     | Service Fabric küme bildirimi alırsınız.|
|    işlemi iptal etme| Bir kullanıcı kaynaklanan hata işlemi iptal eder.|
|    operationgit | Kullanıcı kaynaklanan hata işlemleri tarafından sağlanan girişin filtrelenmiş bir listesini alır.|
|    Sağlama     | Service Fabric kümesi kod veya yapılandırma paketleri sağlayın.|
|    Sistem Kurtarma  | Service Fabric kümesi şu anda çekirdek kaybında takıldı Hizmetler'i kurtarmayı denemesi belirtir.|
|Sistem Durumu raporu   | Service Fabric kümesi üzerinde bir sistem durumu raporu gönderir.|
|    seçin       | Service Fabric kümesi uç noktasına bağlanır.|
| Sağlamayı kaldırma     | Service Fabric kümesi kod veya yapılandırma paketleri sağlama.|
|    Yükseltme         | Service Fabric kümesi kod veya yapılandırma sürümüne yükseltme başlatın.|
|    Yükseltme devam et  | Bir sonraki yükseltme etki alanına taşıma Küme yükseltme yapın.|
|    Yükseltmeyi geri alma| Service Fabric kümesini yükseltme işlemi geri alın.|
|    Yükseltme durumu  | Geçerli Küme yükseltmesinin ilerleme durumunu alır.|
|Yükseltme güncelleştirme  | Service Fabric Küme Yükseltme yükseltme parametreleri güncelleştirin.|


## <a name="sfctl-cluster-health"></a>sfctl küme durumu
Service Fabric kümesi durumunu alır.

Service Fabric kümesi durumunu alır. Sistem durumu olayları sistem durumuna bağlıdır küme üzerinde bildirilen koleksiyonu filtrelemek için EventsHealthStateFilter kullanın. Benzer şekilde, düğümleri ve toplanan sistem durumlarına dayalı döndürülen uygulamaların koleksiyonu filtrelemek için NodesHealthStateFilter ve ApplicationsHealthStateFilter kullanın. 

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --uygulamaları Sistem Durumu Filtresi| Uygulama sistem durumu filtrelemeye izin verir
                                                    objects returned in the result of cluster health
                                                    query based on their health state. The possible
                                                    values for this parameter include integer value
                                                    obtained from members or bitwise operations on
                                                    members of HealthStateFilter enumeration. Only
                                                    applications that match the filter are returned.
                                                    All applications are used to evaluate the
                                                    aggregated health state. If not specified, all
                                                    entries are returned. The state values are flag
                                                    based enumeration, so the value could be a
                                                    combination of these values obtained using
                                                    bitwise 'OR' operator. For example, if the
                                                    provided value is 6 then health state of
                                                    applications with HealthState value of OK (2)
                                                    and Warning (4) are returned. - Default -
                                                    Default value. Matches any HealthState. The
                                                    value is zero. - None - Filter that doesn't
                                                    match any HealthState value. Used in order to
                                                    return no results on a given collection of
                                                    states. The value is 1. - Ok - Filter that
                                                    matches input with HealthState value Ok. The
                                                    value is 2. - Warning - Filter that matches
                                                    input with HealthState value Warning. The value
                                                    is 4. - Error - Filter that matches input with
                                                    HealthState value Error. The value is 8. - All -
                                                    Filter that matches input with any HealthState value. The value is 65535.|
| --Sağlık Durumu Filtresi olayları | Döndürülen HealthEvent nesnelerin sistem durumuna bağlıdır koleksiyonu filtrelemeye izin verir. Bu parametre için olası değerler aşağıdaki sistem durumlarının bir tamsayı değeri içerir. Filtreyle eşleşen olaylar döndürülür. Tüm olayları toplanmış sistem durumunu değerlendirmek için kullanılır. Belirtilmezse, tüm girişleri döndürülür. Değer, bu değerlerin Bitsel 'Veya' işleci kullanılarak edinilen bir bileşimi olabilir şekilde bayrağı numaralandırma, tabanlı durumu değerlerdir. Sağlanan değer 6 ise, örneğin, ardından tüm olaylar Tamam (2) ve uyarı (4), HealthState değeriyle döndürülür. -Varsayılan - varsayılan değer. Tüm HealthState eşleşir. Değer sıfır olur. -Hiçbiri - herhangi bir HealthState değer eşleşmeyen filtreleyin. Sonuç durumları belirli bir koleksiyon döndürmek için kullanılır. Değer 1'dir. -Tamam - eşleşmeleri HealthState değerle Tamam giriş filtreleyin. Değer 2'dir. -Uyarı - filtre HealthState eşleşme girişle uyarı değer.
Değer 4'tür. -Hata - Giriş hata HealthState değeriyle eşleşen Filtresi. Değer 8'dir. -Tüm - giriş herhangi bir HealthState değeri ile eşleşen filtre. 65535 değeridir. | |--Dışlama sağlık istatistikleri | Sistem durumu istatistikleri sorgu sonucu bir parçası olarak döndürülüp döndürülmeyeceğini gösterir. Varsayılan değer false. Sistem durumu Tamam, uyarı ve hata alt sayısını istatistiklerini varlıklar göster. | |   --dahil-sistem-uygulama-sistem durumu-istatistikleri | Doku sistem durumu istatistikleri içerip içermeyeceğini gösterir: / Sistem uygulaması sağlık istatistikleri. Varsayılan değer false. IncludeSystemApplicationHealthStatistics ayarlanmışsa, true olarak sistem istatistikleri dokuya ait varlıkları içerir: / Sistem uygulaması. Aksi takdirde, sorgu sonucu kullanıcı uygulamaları için yalnızca sistem durumu istatistikleri içerir. Sistem durumu istatistikleri uygulanacak bu parametre için sorgu sonucu bulunması gerekir. | | --düğümler sağlık Durumu Filtresi | Kendi sistem durumuna bağlıdır küme durumu sorgusunun sonucu döndürdü düğümü sağlık durumu nesnelerin filtrelemeye izin verir. Bu parametre için olası değerler aşağıdaki sistem durumlarının bir tamsayı değeri içerir. Filtreyle eşleşen düğümleri döndürülür. Tüm düğümleri toplanan sistem durumunu değerlendirmek için kullanılır. Belirtilmezse, tüm girişleri döndürülür. Durum değerleri bayrağı tabanlı numaralandırma olduğundan, değer, bu değerlerin Bitsel 'Veya' işleci kullanılarak edinilen bir bileşimi olabilir. Örneğin, "6" sağlanan değer ise ardından düğümler sistem durumunu Tamam (2) ve uyarı (4), HealthState değeriyle döndürülür. -Varsayılan - varsayılan değer. Tüm HealthState eşleşir. Değer sıfır olur. -Hiçbiri - herhangi bir HealthState değer eşleşmeyen filtreleyin. Sonuç durumları belirli bir koleksiyon döndürmek için kullanılır. Değer 1'dir. -Tamam - eşleşmeleri HealthState değerle Tamam giriş filtreleyin. Değer 2'dir. -Uyarı - filtre HealthState eşleşme girişle uyarı değer.
Değer 4'tür. -Hata - Giriş hata HealthState değeriyle eşleşen Filtresi. Değer 8'dir. -Tüm - giriş herhangi bir HealthState değeri ile eşleşen filtre. 65535 değeridir. | | --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama                        | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| ---h Yardım                      | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı                    | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.                    Varsayılan: json.|
| --Sorgu                        | JMESPath sorgu dizesi. Http://jmespath.org/ daha fazla bilgi ve örnekler için bkz.|
| --ayrıntılı                      | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

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
| --hata ayıklama  | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| ---h Yardım| Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu  | JMESPath sorgu dizesi. Http://jmespath.org/ daha fazla bilgi ve örnekler için bkz.|
| --ayrıntılı| Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-cluster-provision"></a>sfctl küme sağlama
Service Fabric kümesi kod veya yapılandırma paketleri sağlayın.

        Validate and provision the code or configuration packages of a Service Fabric cluster.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
|--Küme bildirimi dosyası yolu| Küme bildirimi dosyası yolu.|
|    --kod dosyasının yolu            | Küme kod paketi dosyasının yolu.|
|    --zaman aşımı -t                | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| ---h Yardım  | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı| Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu | JMESPath sorgu dizesi. Http://jmespath.org/ daha fazla bilgi ve örnekler için bkz.|
| --ayrıntılı  | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-cluster-select"></a>sfctl küme seçin
Service Fabric kümesi uç noktasına bağlanır.

Güvenli kümesine bağlanma, hem (.pem) bir sertifika (.crt) ve anahtar dosyası (.key) veya tek bir dosya belirtin. Her ikisini birden belirtmeyin. İsteğe bağlı olarak, güvenli bir kümeye bağlanılıyorsa, ayrıca bir CA paket dosyası ya da güvenilir CA sertifikaları dizinin yolunu belirtin.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --[gerekli] uç noktası| Uç nokta URL'si, bağlantı noktası ve HTTP veya HTTPS önekini dahil olmak üzere küme.|
| --aad             | Azure Active Directory kimlik doğrulaması için kullanın.|
| --ca              | Geçerli olarak işlemek için CA sertifikaları dizini veya CA paket dosyası yolu.|
| --Sertifika            | Bir istemci sertifikası dosyasının yolu.|
| --anahtarı             | İstemci sertifikası anahtar dosyasının yolu.|
| --yok doğrulayın       | Doğrulama sertifikaları için HTTPS kullanırken devre dışı bırakmak, Not: Bu güvenli bir seçenektir ve üretim ortamları için kullanılmamalıdır.|
| --pem             | .Pem dosyası olarak istemci sertifikası yolu.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama           | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| ---h Yardım         | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı       | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu           | JMESPath sorgu dizesi. Http://jmespath.org/ daha fazla bilgi ve örnekler için bkz.|
| --ayrıntılı         | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-cluster-unprovision"></a>sfctl küme sağlamayı kaldırma
Service Fabric kümesi kod veya yapılandırma paketleri sağlama.

        Unprovision the code or configuration packages of a Service Fabric cluster.

### <a name="arguments"></a>Bağımsız Değişkenler
|Bağımsız değişken|Açıklama|
| --- | --- |
|--kod sürümü  | Küme kodu Paket sürümü.|
|    --config sürümü| Küme bildirimi sürümü.|
|    --zaman aşımı -t    | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler
|Bağımsız değişken|Açıklama|
| --- | --- |
|--hata ayıklama         | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
 |   ---h Yardım       | Bu yardım iletisini ve çıkış gösterir.|
 |   ---o çıktı     | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
 |   --Sorgu         | JMESPath sorgu dizesi. Daha fazla bilgi için http://jmespath.org/ bakın ve
                      örnekler.|
 |   --ayrıntılı       | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|


## <a name="sfctl-cluster-upgrade"></a>sfctl Küme yükseltme
Service Fabric kümesi kod veya yapılandırma sürümüne yükseltme başlatın.

        Validate the supplied upgrade parameters and start upgrading the code or configuration
        version of a Service Fabric cluster if the parameters are valid.

### <a name="arguments"></a>Bağımsız Değişkenler
|Bağımsız değişken|Açıklama|
| --- | --- |
|    --Uygulama sistem durumu eşleme                      | Uygulama adı çiftleri JSON olarak kodlanmış sözlüğü ve
                                            hata oluşmadan önce sağlıklı en yüksek yüzde.|
 |   --uygulama türü sistem durumu eşleme                 | Uygulama türü çiftlerini sözlüğü JSON kodlanmış
                                            name and maximum percentage unhealthy before raising
                                            error.|
 |   --kod sürümü | Küme kod sürümü. | |   --config sürümü | Küme yapılandırması sürümü. | |   --delta sistem durumu değerlendirmesi | Her bir yükseltme etki alanı tamamlanmasından sonra mutlak sistem durumu değerlendirmesi yerine delta sistem durumu değerlendirmesi etkinleştirir. | |   --delta sağlıksız-düğümleri | En düğümlerinin yüzdesi Küme yükseltme sırasında izin verilen sistem durumu düşüşü izin verilir.  Varsayılan: 10.
Delta yükseltme işleminin başında düğümlerinin durumunu ve sistem durumu değerlendirmesi zaman düğümlerin durumunun arasında ölçülür. Onay kümenin genel durumunu toleranslı sınırlarda olduğundan emin olmak için her yükseltme etki alanı yükseltme tamamlandıktan sonra gerçekleştirilen. | |   --hatası eylemi | Olası değerler şunlardır: 'Geçersiz', 'Geri', 'Manual'. | |   --zorla yeniden | Yeniden başlatma. | |   --Sistem durumu denetimi yeniden | Sistem durumu denetimi yeniden deneme zaman aşımı milisaniye cinsinden ölçülür. | |   --Sistem durumu denetimi kararlı | Sistem durumu denetimi kararlı süresinin milisaniye cinsinden ölçülür. | |  --Sistem durumu denetimi bekleme | Sistem durumu denetimi bekleme süresini milisaniye olarak ölçülür. | |  --çoğaltma kümesi onay aşımı | Yükseltme kopya kümesi onay zaman aşımı saniye cinsinden ölçülen. | |   --çalışırken yükseltme-modu | Olası değerler şunlardır: 'Geçersiz', 'UnmonitoredAuto', 'UnmonitoredManual', 'İzlenen'.  Varsayılan: UnmonitoredAuto. | |  --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60. | |  --sağlıksız uygulamaları | Sağlıksız uygulamaları yüzdesi hata raporlamadan önce izin verilen en fazla.
Örneğin, %10 sağlıksız uygulamalarının izin vermek için bu değer 10 olur. Yüzdesini küme hata olarak kabul edilmeden önce sağlıksız uygulamaları maksimum toleranslı yüzdesini temsil eder. Yüzde dikkate ancak en az bir düzgün çalışmayan uygulama olduğundan, sistem durumu uyarı olarak değerlendirilir. Bu uygulama örnekleri ApplicationTypeHealthPolicyMap içerdiği uygulama türleri uygulamaların hariç kümedeki toplam sayısı üzerinden sağlıksız uygulamaları sayısının bölünmesiyle hesaplanır. Küçük sayıda uygulamalar üzerinde bir hatasını tolere için hesaplama yukarı yuvarlar. | |   --sağlıksız düğümleri | Sağlıksız düğümleri yüzdesi hata raporlamadan önce izin verilen en fazla.
Örneğin, %10 sağlıksız düğümlerinin izin vermek için bu değer 10 olur. Yüzde küme hata olarak kabul edilmeden önce sağlıksız düğümleri maksimum toleranslı yüzdesini temsil eder. Yüzde dikkate halde en az bir sağlıksız düğüm yoksa, sistem durumu uyarı olarak değerlendirilir. Yüzde, kümedeki düğümler toplam sayısı üzerinden sağlıksız düğüm sayısını bölünmesiyle hesaplanır. Küçük sayıda düğüm üzerinde bir hatasını tolere için hesaplama yukarı yuvarlar. Bu yüzde, tolerans şekilde yapılandırılmalıdır büyük kümelerinde, bazı düğümler her zaman aşağı veya çıkışı onarım için olacağından. | |   --Yükseltme etki alanı-delta-sağlıksız-düğümler | Sistem durumu düşüşü Küme yükseltme sırasında izin izin verilen en fazla yükseltme etki alanı düğümleri yüzdesi.
Varsayılan: 15.
Delta yükseltme işleminin başında yükseltme etki alanı düğümleri durumunu ve sistem durumu değerlendirmesi zaman yükseltme etki alanı düğümleri durumunu arasında ölçülür. Tüm tamamlanan yükseltme etki alanlarının durumunu emin olmak için yükseltme etki alanları için her yükseltme etki alanı yükseltme tamamlama toleranslı sınırlarda sonra denetimi gerçekleştirilir. | |   --Yükseltme etki alanı timeout | Yükseltme etki alanı zaman aşımı milisaniye cinsinden ölçülür. | |   --Yükseltme zaman aşımı | Yükseltme zaman aşımı milisaniye cinsinden ölçülür. | |   --hata olarak uyarı | Uyarılar aynı önem derecesi hata olarak kabul edilir. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler
    |Bağımsız değişken|Açıklama|
| --- | --- |
|--hata ayıklama                               | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
|    ---h Yardım                             | Bu yardım iletisini ve çıkış gösterir.|
|    ---o çıktı                           | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.
                                            Varsayılan: json.|
|    --Sorgu                               | JMESPath sorgu dizesi. Http://jmespath.org/ daha fazla bilgi için bkz.
                                            bilgi ve örnekler.|
|    --ayrıntılı                             | Günlüğün ayrıntı düzeyini artırın. Kullanım--tam hata ayıklama için hata ayıklama
                                            günlüğe kaydeder.|

## <a name="next-steps"></a>Sonraki adımlar
- [Kurulum](service-fabric-cli.md) Service Fabric CLI.
- Service Fabric CLI kullanarak kullanmayı öğrenin [örnek komutlar](/azure/service-fabric/scripts/sfctl-upgrade-application).