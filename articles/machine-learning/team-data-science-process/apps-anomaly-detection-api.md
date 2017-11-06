---
title: "Azure Machine Learning Anomali algılama API | Microsoft Docs"
description: "Anomali algılama API zaman serisi veri zamanında hep aralıklı sayısal değerler ile anormallikleri algılar Microsoft Azure Machine Learning ile yerleşik bir örnektir."
services: machine-learning
documentationcenter: 
author: alokkirpal
manager: jhubbard
editor: cgronlun
ms.assetid: 52fafe1f-e93d-47df-a8ac-9a9a53b60824
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/05/2017
ms.author: alok;rotimpe
ms.openlocfilehash: cd7dab8514b41d930d01fd134229cc9da48b18fe
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="machine-learning-anomaly-detection-api"></a>Anomali algılama API makine
## <a name="overview"></a>Genel Bakış
[Anomali algılama API](https://gallery.cortanaintelligence.com/MachineLearningAPI/Anomaly-Detection-2) zaman serisi veri zamanında hep aralıklı sayısal değerler ile anormallikleri algılar Azure Machine Learning ile oluşturulmuş bir örnek verilmiştir.

Bu API zaman serisi veri anormal desenleri aşağıdaki türlerini saptayabilir:

* **Pozitif ve negatif eğilimleri**: Örneğin, ne zaman yukarı eğilimi bilgi işlem bellek kullanımını izleme ilgi çekici bir bellek sızıntısı göstergesi olabilir olarak olabilir
* **Dinamik aralık değerleri değişiklikleri**: Örneğin, bir bulut hizmeti tarafından oluşturulan özel durumları izlerken, dinamik aralık değerleri değişiklikler sistem durumu hizmetinin kararsızlığı olduğunu gösteriyor olabilir ve
* **Ani ve Dips**: Örneğin, bir hizmette oturum açma hatalarının sayısı veya bir e-ticaret sitesi kullanıma sayısında izlerken, ani veya dıps anormal davranışları olduğunu gösteriyor olabilir.

Bu makine öğrenme algılayıcılar değerleri gibi değişiklikleri anomali puanları olarak değerlerine zaman ve rapor süregiden değişiklikler üzerinden izleyebilirsiniz. Bunlar geçici eşiği ayarlama gerektirmez ve puanlarını yanlış pozitif hızı denetlemek için kullanılabilir. API, zaman içinde KPI'ları izleyerek hizmet izleme gibi çeşitli senaryolarda kullanışlıdır anomali algılama arama sayısı, sayılar tıklama, performans sayaçları CPU, bellek gibi aracılığıyla dosyasını okur izleme, vb. gibi ölçümleri aracılığıyla kullanımı izleme. zaman içinde.

Anomali algılama sunan başlamanıza yardımcı olmak için faydalı araçları ile birlikte gelir.

* [Web uygulaması](http://anomalydetection-aml.azurewebsites.net/) değerlendirmek ve verileriniz üzerinde anomali algılama API'leri sonuçlarını görselleştirmenize yardımcı olur.

> [!NOTE]
> Deneyin **BT Anomali Insights çözümü** tarafından sağlanmıştır [bu API](https://gallery.cortanaintelligence.com/MachineLearningAPI/Anomaly-Detection-2)
> 
> Azure aboneliğinize dağıtılan bu uçtan uca çözüm almak için <a href="https://gallery.cortanaintelligence.com/Solution/Anomaly-Detection-Pre-Configured-Solution-1" target="_blank"> **buradan başlayın >**</a>
> 
>

## <a name="api-deployment"></a>API dağıtımı
API kullanabilmeniz için burada bir Azure Machine Learning web hizmeti olarak barındırılacak Azure aboneliğinizde dağıtmanız gerekir.  Bunu yapmak [Cortana Intelligence Galerisi](https://gallery.cortanaintelligence.com/MachineLearningAPI/Anomaly-Detection-2).  Bu iki AzureML Web Hizmetleri (ve bunların ilgili kaynaklar), Azure aboneliğinizin - mevsimselliğin algılama anomali algılama için diğeri mevsimselliğin algılama olmadan dağıtır.  Dağıtım tamamlandıktan sonra API'yönetmek kullanamazsınız [AzureML web Hizmetleri](https://services.azureml.net/webservices/) sayfası.  Bu sayfadan API'sini çağırmak için uç noktalarına, API anahtarları yanı sıra örnek kod Bul mümkün olacaktır.  Daha ayrıntılı yönergeler kullanılabilir [burada](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-manage-new-webservice).

## <a name="scaling-the-api"></a>API ölçeklendirme
Varsayılan olarak, dağıtımınızı 1.000 işlemleri/ay ve 2 işlem saat/ay içeren boş bir fatura geliştirme ve Test planı sahip olur.  Başka bir plana gereksinimlerinize uygun şekilde yükseltebilirsiniz.  Farklı planları fiyatlandırma hakkında ayrıntıları [burada](https://azure.microsoft.com/en-us/pricing/details/machine-learning/) "Üretim Web API fiyatlandırma" altında.

## <a name="managing-aml-plans"></a>AML yönetme planları 
Faturalandırma planınıza yönetebilirsiniz [burada](https://services.azureml.net/plans/).  Plan adı API dağıtırken seçtiğiniz kaynak grubu adı yanı sıra, aboneliğiniz için benzersiz bir dizeye dayalı olacak.  Planınızı yükseltmek yönergeler kullanılabilir [burada](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-manage-new-webservice) "faturalandırma planları yönetme" bölümünün altında.

## <a name="api-definition"></a>API tanımı
Web hizmeti REST tabanlı bir API sağlayan bir web veya mobil uygulama, R, Python, Excel gibi farklı yollarla tüketilebilir HTTPS üzerinden vb.  Bu hizmet bir REST API çağrısı aracılığıyla zaman serisi veri göndermek ve aşağıda açıklanan üç anomali türleri birlikte çalışır.

## <a name="calling-the-api"></a>API çağırma
API çağrısı için API anahtarı ve uç nokta konumunu bilmeniz gerekir.  API'sini çağırmak için örnek kod ile birlikte bunların her ikisi de mevcuttur [AzureML web Hizmetleri](https://services.azureml.net/webservices/) sayfası.  İstenen API'sine gidin ve ardından bunları bulmak için "Tüket" sekmesini tıklatın.  API olarak Swagger API'si çağırabilirsiniz unutmayın (yani URL parametresi ile `format=swagger`) veya farklı bir harici - Swagger API'si (yani olmadan `format` URL parametresi).  Örnek kod Swagger biçimini kullanır.  Aşağıda bir örnek istek ve yanıt Swagger olmayan biçimindedir.  Bu örnek mevsimselliğin uç noktasına verilebilir.  Mevsimselliğin olmayan endpoint benzer.

### <a name="sample-request-body"></a>Örnek istek gövdesi
İki nesne istek içeriyor: `Inputs` ve `GlobalParameters`.  Diğerleri değildir aşağıdaki örnek istekte bazı parametreler açıkça gönderilen (aşağı kaydırın her bitiş noktasıyla ilgili parametrelerin tam bir listesi için).  Açıkça istekte gönderilmez Parametreler aşağıda verilen varsayılan değerleri kullanır.

    {
                "Inputs": {
                        "input1": {
                                "ColumnNames": ["Time", "Data"],
                                "Values": [
                                        ["5/30/2010 18:07:00", "1"],
                                        ["5/30/2010 18:08:00", "1.4"],
                                        ["5/30/2010 18:09:00", "1.1"]
                                ]
                        }
                },
        "GlobalParameters": {
            "tspikedetector.sensitivity": "3",
            "zspikedetector.sensitivity": "3",
            "bileveldetector.sensitivity": "3.25",
            "detectors.spikesdips": "Both"
        }
    }

### <a name="sample-response"></a>Örnek Yanıtı
Görmek için Not `ColumnNames` alan içermelidir `details=true` isteğiniz bir URL parametresi olarak.  Bu alanların her biri arkasında anlamı için aşağıdaki tablolara bakın.

    {
        "Results": {
            "output1": {
                "type": "table",
                "value": {
                    "Values": [
                        ["5/30/2010 6:07:00 PM", "1", "1", "0", "0", "-0.687952590518378", "0", "-0.687952590518378", "0", "-0.687952590518378", "0"],
                        ["5/30/2010 6:08:00 PM", "1.4", "1.4", "0", "0", "-1.07030497733224", "0", "-0.884548154298423", "0", "-1.07030497733224", "0"],
                        ["5/30/2010 6:09:00 PM", "1.1", "1.1", "0", "0", "-1.30229513613974", "0", "-1.173800281031", "0", "-1.30229513613974", "0"]
                    ],
                    "ColumnNames": ["Time", "OriginalData", "ProcessedData", "TSpike", "ZSpike", "BiLevelChangeScore", "BiLevelChangeAlert", "PosTrendScore", "PosTrendAlert", "NegTrendScore", "NegTrendAlert"],
                    "ColumnTypes": ["DateTime", "Double", "Double", "Double", "Double", "Double", "Int32", "Double", "Int32", "Double", "Int32"]
                }
            }
        }
    }


## <a name="score-api"></a>Puan API
Puan API anomali algılama Mevsimlik olmayan zaman serisi veri üzerinde çalıştırmak için kullanılır. API anomali algılayıcılar çeşitli verileri çalıştırır ve anomali puanlarını döndürür. Aşağıdaki şekilde puan API algılayabilir anormallikleri örneği gösterilmektedir. Bu zaman serisi 2 ayrı düzeyi değişir ve 3 ani sahiptir. Kırmızı nokta siyah noktalar algılanan ani gösterirken, düzey değişikliği, algılanan zamanı gösterir.
![Puan API][1]

### <a name="detectors"></a>Algılayıcılar
Anomali algılama API algılayıcılar 3 geniş kategorilerde destekler. Belirli giriş parametreleri ve her algılayıcı çıktıların hakkında ayrıntılar aşağıdaki tabloda bulunabilir.

| Algılayıcısı kategorisi | Algılayıcısı | Açıklama | Giriş parametreleri | Çıkışları |
| --- | --- | --- | --- | --- |
| Depo algılayıcılar |TSpike algılayıcısı |Ani ve şu ana kadar değerlerine göre dıps ilk ve üçüncü Dörttebirlikler saptamak |*tspikedetector.sensitivity:* aralığındaki tamsayı değeri 1-10, varsayılan alır: 3; Daha yüksek değerler, böylece daha az hassas yapmadan birden fazla aşırı değeri yakalar |TSpike: ikili değerler – '1' depo/DIP algılanırsa, '0' Aksi takdirde |
| Depo algılayıcılar | ZSpike algılayıcısı |Ani ve ne kadar datapoints'ler ortalamasını göre dıps Algıla |*zspikedetector.sensitivity:* aralığındaki tamsayı değeri 1-10, varsayılan alın: 3; Daha yüksek değerleri daha az hassas kolaylaştırarak birden fazla aşırı değeri yakalar |ZSpike: ikili değerler – '1' depo/DIP algılanırsa, '0' Aksi takdirde | |
| Yavaş eğilimi algılayıcısı |Yavaş eğilimi algılayıcısı |Yavaş pozitif eğilimi kümesi duyarlılık göredir Algıla |*trenddetector.sensitivity:* algılayıcısı puan eşiğine (varsayılan: 3,25, 3,25 – 5 öğesinden; seçmek için makul sınırlar. Yüksek daha az duyarlı) |tscore: anomali puan eğilimi üzerinde temsil eden kayan sayısı |
| Düzey değişikliği algılayıcılar | Çift yönlü düzeyi değişiklik algılayıcısı |Yukarı ve aşağı düzey değişiklik kümesi duyarlılık göredir Algıla |*bileveldetector.sensitivity:* algılayıcısı puan eşiğine (varsayılan: 3,25, 3,25 – 5 öğesinden; seçmek için makul sınırlar. Yüksek daha az duyarlı) |rpscore: anomali puan yukarı ve aşağı düzey değişiklik gösteren kayan sayı | |

### <a name="parameters"></a>Parametreler
Bu giriş parametreleri hakkında daha ayrıntılı bilgi aşağıdaki tabloda listelenmiştir:

| Giriş parametreleri | Açıklama | Varsayılan ayar | Tür | Geçerli aralık | Önerilen aralık |
| --- | --- | --- | --- | --- | --- |
| detectors.historyWindow |Anomali puan hesaplama için kullanılan geçmişinde (veri noktası sayısı) |500 |tamsayı |10-2000 |Zaman serisi bağımlı |
| detectors.spikesdips | Algılanmayacağını yalnızca ani, yalnızca dıps veya her ikisi |Her ikisi |Numaralandırılan |Her ikisi de, ani, Dıps |Her ikisi |
| bileveldetector.sensitivity |Çift yönlü düzeyi duyarlılık algılayıcısı değiştirin. |3.25 |Çift |None |3,25 5 (daha düşük değerler daha hassas anlamına gelir) |
| trenddetector.sensitivity |Pozitif eğilimi algılayıcısı duyarlılık. |3.25 |Çift |None |3,25 5 (daha düşük değerler daha hassas anlamına gelir) |
| tspikedetector.sensitivity |Duyarlılık TSpike algılayıcısı |3 |tamsayı |1-10 |3-5 (daha düşük değerler daha hassas anlamına gelir) |
| zspikedetector.sensitivity |Duyarlılık ZSpike algılayıcısı |3 |tamsayı |1-10 |3-5 (daha düşük değerler daha hassas anlamına gelir) |
| postprocess.tailRows |Çıkış sonuçlarında tutulacak en son veri noktası sayısı |0 |tamsayı |(tüm veri noktaları tutun) 0 veya sonuçlarında tutmak için noktası sayısını belirtin |Yok |

### <a name="output"></a>Çıktı
API tüm algılayıcılar zaman serisi verileriniz üzerinde çalışır ve anomali puanlarını ve her nokta için ikili depo göstergeleri zamanında döndürür. Aşağıdaki tabloda API'sinden çıkışları listeler. 

| Çıkışları | Açıklama |
| --- | --- |
| Zaman |Ham verileri veya toplanan (ve/veya) imputed verilerden zaman damgaları, toplama (ve/veya) eksik veri imputation uygulanır |
| Veriler |Değerleri ham verileri veya toplanan (ve/veya) imputed veri varsa toplama (ve/veya) eksik veri imputation uygulanır |
| TSpike |Bir depo TSpike algılayıcısı tarafından algılanan olup olmadığını belirtmek için ikili göstergesi |
| ZSpike |Bir depo ZSpike algılayıcısı tarafından algılanan olup olmadığını belirtmek için ikili göstergesi |
| rpscore |Kayan sayı temsil eden anomali puan çift yönlü düzeyi değişiklik |
| rpalert |çift yönlü düzeyi var. gösteren 1/0 değeri giriş duyarlılığına göre anomali değiştirme |
| tscore |Kayan sayı temsil eden anomali puan üzerinde pozitif eğilimi |
| talert |var. gösteren 1/0 giriş duyarlılığına göre pozitif eğilimi anomali değerdir |

## <a name="scorewithseasonality-api"></a>ScoreWithSeasonality API
ScoreWithSeasonality API anormallik algılamayı Mevsimlik düzenlere sahip zaman serisi çalıştırmak için kullanılır. Bu API sapmaları Mevsimlik düzenleri algılamak kullanışlıdır.  
Aşağıdaki şekilde Mevsimlik zaman serisinde algılanan anormallikleri örneği gösterilmektedir. Zaman serisi bir depo (1 siyah nokta), iki dıps (2 siyah nokta ve biri sonunda) ve tek düzey değişikliği (kırmızı nokta) sahiptir. DIP zaman serisi ve düzey değişikliği ortasında yalnızca discernable Mevsimlik bileşenleri serisinden kaldırıldıktan sonra dikkat edin.
![Mevsimselliğin API][2]

### <a name="detectors"></a>Algılayıcılar
Algılayıcılar mevsimselliğin uç noktada olanları mevsimselliğin olmayan uç noktası, ancak biraz farklı parametre adları (aşağıda listelenen) ile benzerdir.

### <a name="parameters"></a>Parametreler

Bu giriş parametreleri hakkında daha ayrıntılı bilgi aşağıdaki tabloda listelenmiştir:

| Giriş parametreleri | Açıklama | Varsayılan ayar | Tür | Geçerli aralık | Önerilen aralık |
| --- | --- | --- | --- | --- | --- |
| preprocess.aggregationInterval |Toplama aralığı toplama için saniye cinsinden zaman serisi giriş |0 (hiçbir toplama gerçekleştirilir) |tamsayı |0: toplama, > 0 aksi atla |Zaman serisi bağımlı, 1 gün için 5 dakika |
| preprocess.aggregationFunc |Belirtilen AggregationInterval veri toplamak için kullanılan işlevi |Ortalama |Numaralandırılan |Ortalama, Topla, uzunluğu |Yok |
| preprocess.replaceMissing |Eksik veri impute için kullanılan değerleri |lkv (bilinen son değer) |Numaralandırılan |sıfır, lkv, ortalama |Yok |
| detectors.historyWindow |Anomali puan hesaplama için kullanılan geçmişinde (veri noktası sayısı) |500 |tamsayı |10-2000 |Zaman serisi bağımlı |
| detectors.spikesdips | Algılanmayacağını yalnızca ani, yalnızca dıps veya her ikisi |Her ikisi |Numaralandırılan |Her ikisi de, ani, Dıps |Her ikisi |
| bileveldetector.sensitivity |Çift yönlü düzeyi duyarlılık algılayıcısı değiştirin. |3.25 |Çift |None |3,25 5 (daha düşük değerler daha hassas anlamına gelir) |
| postrenddetector.sensitivity |Pozitif eğilimi algılayıcısı duyarlılık. |3.25 |Çift |None |3,25 5 (daha düşük değerler daha hassas anlamına gelir) |
| negtrenddetector.sensitivity |Negatif eğilimi algılayıcısı duyarlılık. |3.25 |Çift |None |3,25 5 (daha düşük değerler daha hassas anlamına gelir) |
| tspikedetector.sensitivity |Duyarlılık TSpike algılayıcısı |3 |tamsayı |1-10 |3-5 (daha düşük değerler daha hassas anlamına gelir) |
| zspikedetector.sensitivity |Duyarlılık ZSpike algılayıcısı |3 |tamsayı |1-10 |3-5 (daha düşük değerler daha hassas anlamına gelir) |
| seasonality.Enable |Mevsimselliğin analiz gerçekleştirilmesi olup |TRUE |Boole değeri |TRUE, false |Zaman serisi bağımlı |
| seasonality.numSeasonality |Algılanacak düzenli döngüleri sayısı |1 |tamsayı |1, 2 |1-2 |
| seasonality.Transform |Mevsimlik olup olmadığını (ve) eğilimi bileşenleri anomali algılama uygulamadan önce kaldırılması |deseason |Numaralandırılan |None, deseason, deseasontrend |Yok |
| postprocess.tailRows |Çıkış sonuçlarında tutulacak en son veri noktası sayısı |0 |tamsayı |(tüm veri noktaları tutun) 0 veya sonuçlarında tutmak için noktası sayısını belirtin |Yok |

### <a name="output"></a>Çıktı
API tüm algılayıcılar zaman serisi verileriniz üzerinde çalışır ve anomali puanlarını ve her nokta için ikili depo göstergeleri zamanında döndürür. Aşağıdaki tabloda API'sinden çıkışları listeler. 

| Çıkışları | Açıklama |
| --- | --- |
| Zaman |Ham verileri veya toplanan (ve/veya) imputed verilerden zaman damgaları, toplama (ve/veya) eksik veri imputation uygulanır |
| OriginalData |Değerleri ham verileri veya toplanan (ve/veya) imputed veri varsa toplama (ve/veya) eksik veri imputation uygulanır |
| ProcessedData |Aşağıdakilerden birini: <ul><li>Önemli mevsimselliğin algılandı ve seçeneğe deseason Mevsimlik değişiklikler zaman serisi ayarlandı;</li><li>Mevsimlik değişiklikler ayarlanmış ve önemli mevsimselliğin algıladıysa zaman serisi ve deseasontrend seçeneği seçili değişimleri giderilmiş</li><li>Aksi takdirde, bu OriginalData aynıdır</li> |
| TSpike |Bir depo TSpike algılayıcısı tarafından algılanan olup olmadığını belirtmek için ikili göstergesi |
| ZSpike |Bir depo ZSpike algılayıcısı tarafından algılanan olup olmadığını belirtmek için ikili göstergesi |
| BiLevelChangeScore |Kayan sayı temsil eden anomali puan düzeyi değişiklik |
| BiLevelChangeAlert |Giriş duyarlılığına göre bir düzey değişikliği anomali olduğunu gösteren 1/0 değeri var. |
| PosTrendScore |Kayan sayı temsil eden anomali puan üzerinde pozitif eğilimi |
| PosTrendAlert |var. gösteren 1/0 giriş duyarlılığına göre pozitif eğilimi anomali değerdir |
| NegTrendScore |Kayan sayı temsil eden anomali puana negatif eğilimi üzerinde |
| NegTrendAlert |var. gösteren 1/0 giriş duyarlılığına göre negatif eğilimi anomali değerdir |

[1]: ./media/apps-anomaly-detection-api/anomaly-detection-score.png
[2]: ./media/apps-anomaly-detection-api/anomaly-detection-seasonal.png

