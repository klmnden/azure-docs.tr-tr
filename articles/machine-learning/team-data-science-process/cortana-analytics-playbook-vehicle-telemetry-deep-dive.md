---
title: "Derin Dalış içine araç durumu tahmin etmek ve alışkanlık - Azure yürüten | Microsoft Docs"
description: "Araç sistem durumu ve yürüten gerçek zamanlı ve Tahmine dayalı Öngörüler elde etmek için Cortana Intelligence yeteneklerini kullanabilir alışkanlıklarınıza."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: d8866fa6-aba6-40e5-b3b3-33057393c1a8
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: 4050fdc2056df395bbcc37e3783f61eebd90f80a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="vehicle-telemetry-analytics-solution-playbook-deep-dive-into-the-solution"></a>Araç telemetri analizi çözüm kitabı: çözümün ayrıntılarına inme
Bu **menü** bu playbook bölümlerine bağlantılar: 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../../includes/cap-vehicle-telemetry-playbook-selector.md)]

Bu bölümde kümede ayrıntısına gider her yönergeleri ve özelleştirme işaretçileri ile çözüm mimarisinde gösterilen aşamaları. 

## <a name="data-sources"></a>Veri Kaynakları
Çözüm iki farklı veri kaynakları kullanır:

* **Benzetimli araç sinyalleri ve tanılama veri kümesi** ve 
* **araç Kataloğu**

Bir araç telematik simulator bu çözümün bir parçası olarak dahil edilir. Tanılama bilgisi yayar ve araç durumunu ve belirli bir noktada yönlendirmeli düzeni zamanında karşılık gelen işaret eder. Tıklatın [araç telematik Simulator](http://go.microsoft.com/fwlink/?LinkId=717075) indirmek için **araç telematik Simulator Visual Studio çözümü** gereksinimlerinize göre özelleştirmeler. Araç katalog Toplamıdır modeli eşlemesi ile başvuru dataset içerir.

![Araç telematik simulator](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig1-vehicle-telematics-simulator.png)

*Şekil 1 – araç telematik Simulator*

Aşağıdaki şema içeren bir JSON olarak biçimlendirilmiş bir veri kümesi budur.

| Sütun | Açıklama | Değerler |
| --- | --- | --- |
| TOPLAMIDIR |Rastgele oluşturulan araç kimlik numarası |Bu ana 10.000 rastgele oluşturulmuş araç kimlik numaraları listesinden elde edilir. |
| Dış sıcaklığı |Aracın burada yürüten dış sıcaklığı |0-100 arasında rastgele oluşturulmuş bir sayıya |
| Altyapısı sıcaklık |Aracın altyapısı sıcaklığını |0-500 arasında rastgele oluşturulmuş sayı |
| Hız |Aracın yönlendiren altyapısı hızı |0-100 arasında rastgele oluşturulmuş bir sayıya |
| Yakıt |Aracın yakıt düzeyi |Rastgele oluşturulmuş bir sayıya 0-100 (yakıt düzeyi yüzdesi gösterir) |
| EngineOil |Aracın altyapısı Petrol düzeyi |Rastgele oluşturulmuş bir sayıya 0-100 (altyapısı Petrol düzeyi yüzdesi gösterir) |
| Lastiği baskısı |Aracın lastiği Basıncı |Rastgele oluşturulan numarası 0-50'den (lastiği baskısı düzeyi yüzdesi gösterir) |
| Kilometre Sayacı |Aracın Kilometre Sayacı okuma |0-200000 arasında rastgele oluşturulmuş sayı |
| Accelerator_pedal_position |Aracın Hızlandırıcı pedal konumu |Rastgele oluşturulmuş bir sayıya 0-100 (Hızlandırıcı düzeyi yüzdesi gösterir) |
| Parking_brake_status |Aracın veya yerleşmiş durumdayken gösterir |TRUE veya False |
| Headlamp_status |Burada ön ışık üzerinde olup olmadığını gösterir |TRUE veya False |
| Brake_pedal_status |Fren pedal veya basılı olup olmadığını gösterir |TRUE veya False |
| Transmission_gear_position |Aracın iletim dişli konumu |Durumları: ilk olarak, üçüncü, dördüncü beşinci, altıncı seventh, sekizinci ikinci |
| Ignition_status |Aracın çalışıyor veya durdurulmuştur olup olmadığını gösterir |TRUE veya False |
| Windshield_wiper_status |Ön wiper veya açık olup olmadığını gösterir |TRUE veya False |
| ABS |ABS veya bağlı olup olmadığını gösterir |TRUE veya False |
| zaman damgası |Veri noktası oluşturulduğunda, zaman damgası |Tarih |
| Şehir |Aracın konumu |Bu çözümdeki 4 Şehir: Bellevue, Redmond, Sammamish, Seattle |

Araç model başvuru dataset Toplamıdır modeli eşleme içerir. 

| TOPLAMIDIR | modeli |
| --- | --- |
| FHL3O1SA4IEHB4WU1 |Sedan |
| 8J0U8XCPRGW4Z3NQE |Karma |
| WORG68Z2PLTNZDBI7 |Aile Saloon |
| JTHMYHQTEPP4WBMRN |Sedan |
| W9FTHG27LZN1YWO0Y |Karma |
| MHTP9N792PHK08WJM |Aile Saloon |
| EI4QXI2AXVQQING4I |Sedan |
| 5KKR2VB4WHQH97PF8 |Karma |
| W9NSZ423XZHAONYXB |Aile Saloon |
| 26WJSGHX4MA5ROHNL |Dönüştürülebilir |
| GHLUB6ONKMOSI7E77 |İstasyon Wagon |
| 9C2RHVRVLMEJDBXLP |Küçük otomobil |
| BRNHVMZOUJ6EOCP32 |Küçük SUV |
| VCYVW0WUZNBTM594J |Spor araba |
| HNVCE6YFZSA5M82NY |Orta SUV |
| 4R30FOR7NUOBL05GJ |İstasyon Wagon |
| WYNIIY42VKV6OQS1J |Büyük SUV |
| 8Y5QKG27QET1RBK7I |Büyük SUV |
| DF6OX2WSRA6511BVG |Coupe |
| Z2EOZWZBXAEW3E60T |Sedan |
| M4TV6IEALD5QDS3IR |Karma |
| VHRA1Y2TGTA84F00H |Aile Saloon |
| R0JAUHT1L1R3BIKI0 |Sedan |
| 9230C202Z60XX84AU |Karma |
| T8DNDN5UDCWL7M72H |Aile Saloon |
| 4WPYRUZII5YV7YA42 |Sedan |
| D1ZVY26UV2BFGHZNO |Karma |
| XUF99EW9OIQOMV7Q7 |Aile Saloon |
| 8OMCL3LGI7XNCC21U |Dönüştürülebilir |
| ……. | |

### <a name="references"></a>Başvurular
[Araç telematik Simulator Visual Studio çözümü](http://go.microsoft.com/fwlink/?LinkId=717075) 

[Azure Event hub'ı](https://azure.microsoft.com/services/event-hubs/)

[Azure Data Factory](https://azure.microsoft.com/documentation/learning-paths/data-factory/)

## <a name="ingestion"></a>Alım
Azure Event Hubs, Stream Analytics ve Data Factory birleşimleri araç sinyalleri, tanılama olayları alma için kullanılabilir ve gerçek zamanlı ve toplu analizi. Tüm bu bileşenlerin oluşturulur ve çözüm dağıtımının bir parçası yapılandırılmış. 

### <a name="real-time-analysis"></a>Gerçek zamanlı analiz
Araç telematik Simulator tarafından oluşturulan olayları olay Olay Hub SDK'sını kullanarak Hub'ına yayımlanır. Stream Analytics işi, bu olayları olay hub'ı alır ve araç durumu çözümlemek için gerçek zamanlı verileri işler. 

![Olay hub'ı Panosu](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig4-vehicle-telematics-event-hub-dashboard.png) 

*Şekil 4 - Event Hub Panosu*

![Veriler işlenirken Analytics iş akışı](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig5-vehicle-telematics-stream-analytics-job-processing-data.png) 

*Şekil 5 - Stream Analytics işi veri işleme*

Stream Analytics işi;

* Olay hub'ı verileri alır 
* araç Toplamıdır karşılık gelen modeline eşlemek için başvuru verileri ile bir birleştirme gerçekleştirir 
* bunları zengin toplu analiz için Azure blob depolama alanına devam ettirir. 

Aşağıdaki akış analizi sorgu verileri Azure blob depolama alanına kalıcı hale getirmek için kullanılır. 

![Akış analizi işi sorgu veri alımı için](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig6-vehicle-telematics-stream-analytics-job-query-for-data-ingestion.png) 

*Şekil 6 - Stream Analytics işi veri alımı için sorgu*

### <a name="batch-analysis"></a>Toplu iş analiz
Biz de benzetimli araç sinyalleri ve tanılama veri kümesi daha zengin toplu analizi için ek bir birim oluşturduğunu. Bu toplu işlem için iyi temsili veri birimi emin olmak için gereklidir. Bu amaç için "PrepareSampleDataPipeline" adlı işlem hattını Azure Data Factory iş akışında bir yılın tutarında benzetimli araç sinyalleri ve tanılama veri kümesi oluşturmak için kullanıyoruz. Tıklatın [Data Factory özel etkinlik](http://go.microsoft.com/fwlink/?LinkId=717077) Data Factory özel DotNet etkinlik gereksinimlerinize göre özelleştirmeleri için Visual Studio çözümü karşıdan yüklemek için. 

![Toplu iş akışı işleme için örnek verileri hazırlama](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig7-vehicle-telematics-prepare-sample-data-for-batch-processing.png) 

*Şekil 7 - toplu işleme iş akışı için örnek verileri hazırlama*

Ardışık Düzen özel bir ADF .net oluşan etkinlik, burada göster:

![PrepareSampleDataPipeline etkinliği](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig8-vehicle-telematics-prepare-sample-data-pipeline.png) 

*Şekil 8 - PrepareSampleDataPipeline*

Ardışık Düzen başarıyla yürütür ve "RawCarEventsTable" veri kümesi "Hazır", yıllık değerinde benzetimli araç sinyaller ve tanılama işaretlenmiş sonra veri üretilir. Aşağıdaki klasör ve dosya depolama hesabınızda "connectedcar" kapsayıcısı altında oluşturulan bakın:

![PrepareSampleDataPipeline çıkış](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig9-vehicle-telematics-prepare-sample-data-pipeline-output.png) 

*Şekil 9 - PrepareSampleDataPipeline çıkış*

### <a name="references"></a>Başvurular
[Akış alımı için Azure olay hub'ı SDK](../../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

[Azure Data Factory veri taşıma özellikleri](../../data-factory/v1/data-factory-data-movement-activities.md)
[Azure veri fabrikası DotNet etkinliği](../../data-factory/v1/data-factory-use-custom-activities.md)

[Örnek verileri hazırlama için azure veri fabrikası DotNet etkinlik visual studio çözümü](http://go.microsoft.com/fwlink/?LinkId=717077) 

## <a name="partition-the-dataset"></a>Bölüm veri kümesi
Ham yarı yapılandırılmış araç sinyalleri ve tanılama veri kümesi yıl/ay biçimine veri hazırlık adımı bölümlenir. Bu bölümleme daha verimli şekilde sorgulamak ve ölçeklenebilir uzun vadeli depolama ilk hesap dolduğunda diğerine hatası-üzerinden bir blob hesabından etkinleştirerek yükseltir. 

>[!NOTE] 
>Bu adımda çözümü, yalnızca toplu işleme için geçerlidir.

Giriş ve çıkış veri veri yönetimi:

* **Çıktı verilerini** (etiketli *PartitionedCarEventsTable*) "Data Lake" Müşteri'nin veri temel / "rawest" form olarak uzun bir süre için tutulması sağlamaktır. 
* **Giriş verileri** bu ardışık düzen genellikle atılan gibi çıktı veri girişi için tam uygunluğa sahiptir - yalnızca (sonraki kullanım için daha iyi bölümlenmiş) depolanır.

![Bölüm araba olayları iş akışı](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig10-vehicle-telematics-partition-car-events-workflow.png)

*Şekil 10 – bölüm araba olayları iş akışı*

Hdınsight Hive etkinliği "PartitionCarEventsPipeline içinde" kullanarak ham verileri bölümlenen. 1. adımda bir yıl için oluşturulan örnek verileri yıl/AYA göre bölümlenmiş. Bölümler araç sinyalleri ve tanılama verilerini her ay yılın (toplam 12 bölümler) oluşturmak için kullanılır. 

![PartitionCarEventsPipeline etkinliği](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig11-vehicle-telematics-partition-car-events-pipeline.png)

*Şekil 11 - PartitionCarEventsPipeline*

***PartitionConnectedCarEvents Hive betiği***

"Partitioncarevents.hql" adlı aşağıdaki Hive betiğini bölümleme için kullanılır ve indirilen ZIP "\demo\src\connectedcar\scripts" klasöründe bulunur. 
    
    SET hive.exec.dynamic.partition=true;
    SET hive.exec.dynamic.partition.mode = nonstrict;
    set hive.cli.print.header=true;

    DROP TABLE IF EXISTS RawCarEvents; 
    CREATE EXTERNAL TABLE RawCarEvents 
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:RAWINPUT}'; 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents 
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string
    ) partitioned by (YearNo int, MonthNo int) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDOUTPUT}';

    DROP TABLE IF EXISTS Stage_RawCarEvents; 
    CREATE TABLE IF NOT EXISTS Stage_RawCarEvents 
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string,
                YearNo                             int,
                MonthNo                         int) 
    ROW FORMAT delimited fields terminated by ',' LINES TERMINATED BY '10';

    INSERT OVERWRITE TABLE Stage_RawCarEvents
    SELECT
        vin,            
        model,
        timestamp,
        outsidetemperature,
        enginetemperature,
        speed,
        fuel,
        engineoil,
        tirepressure,
        odometer,
        city,
        accelerator_pedal_position,
        parking_brake_status,
        headlamp_status,
        brake_pedal_status,
        transmission_gear_position,
        ignition_status,
        windshield_wiper_status,
        abs,
        gendate,
        Year(gendate),
        Month(gendate)

    FROM RawCarEvents WHERE Year(gendate) = ${hiveconf:Year} AND Month(gendate) = ${hiveconf:Month}; 

    INSERT OVERWRITE TABLE PartitionedCarEvents PARTITION(YearNo, MonthNo) 
    SELECT
        vin,            
        model,
        timestamp,
        outsidetemperature,
        enginetemperature,
        speed,
        fuel,
        engineoil,
        tirepressure,
        odometer,
        city,
        accelerator_pedal_position,
        parking_brake_status,
        headlamp_status,
        brake_pedal_status,
        transmission_gear_position,
        ignition_status,
        windshield_wiper_status,
        abs,
        gendate,
        YearNo,
        MonthNo
    FROM Stage_RawCarEvents WHERE YearNo = ${hiveconf:Year} AND MonthNo = ${hiveconf:Month};

Ardışık Düzen başarılı bir şekilde yürütüldükten sonra depolama hesabınızda "connectedcar" kapsayıcısı altında oluşturulur şu bölümlerde bakın.

![Bölümlenmiş çıktı](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig12-vehicle-telematics-partitioned-output.png)

*Şekil 12 - bölümlenmiş çıktı*

Veriler artık iyileştirilmiş, daha yönetilebilir ve zengin toplu Öngörüler kazanmak için daha fazla işleme için hazırdır. 

## <a name="data-analysis"></a>Veri analizi
Bu bölümde, Azure akış analizi, Azure Machine Learning, Azure Data Factory ve Azure Hdınsight araç sistem durumunu gelişmiş analizler ve alışkanlık yürüten zengin için birleştirme bakın. Burada üç alt bölümleri şunlardır:

1. **Machine Learning**: Bu alt bölümde bakım bakım gerektiren Araçlar ve güvenlik sorunları nedeniyle geri çekme gerektiren taşıtlardan tahmin etmek için bu çözümde kullanılan anomali algılama denemeler hakkında bilgi içerir.
2. **Gerçek zamanlı analiz**: Bu alt bölümde Stream Analytics sorgu dili kullanarak ve özel bir uygulama kullanarak gerçek zamanlı machine learning deneme faaliyete geçirmeye yönelik gerçek zamanlı analiz ilgili bilgiler içerir.
3. **Toplu analiz**: Bu alt bölümde Azure Hdınsight ve Azure Machine Learning Azure Data Factory tarafından kullanıma hazır hale getirilmiş kullanarak toplu veri işleme ve dönüştürme ile ilgili bilgileri içerir.

### <a name="machine-learning"></a>Machine Learning
Amacımız burada Bakım veya belirli sistem durumu istatistiklerine bağlı geri çağırma gerektiren taşıtlardan tahmin etmektir. Aşağıdaki varsayımlar vermiyoruz

* Aşağıdaki üç koşullardan biri geçerliyse taşıtlardan gerektiren **bakım Bakım**:
  
  * Lastiği baskısı düşük
  * Altyapısı Petrol düzeyinin düşük
  * Altyapısı sıcaklık yüksek
* Aşağıdaki koşullardan biri geçerliyse taşıtlardan olabilir bir **güvenlik sorunu** ve gerektiren **geri çağırma**:
  
  * Altyapısı sıcaklık yüksek olduğu, ancak dış sıcaklığı düşük
  * Altyapısı sıcaklık düşük olduğu, ancak dış sıcaklığı yüksek

Önceki gereksinimlerine bağlı olarak, daha fazla bilgi, araç bakım algılaması için diğeri için araç geri çağırma algılama algılamak için iki ayrı modelleri oluşturduk. Her iki bu modellerinde yerleşik asıl bileşen analiz (PCA) algoritması anomali algılama için kullanılır. 

**Bakım algılama modeli**

Üç göstergeleri - lastiği baskısı, altyapısı Petrol veya altyapısı sıcaklık - birini ilgili koşul uymazsa, bakım algılama modelini bir anomali bildirir. Sonuç olarak, biz yalnızca model oluşturmanın bu üç değişkenleri dikkate almanız gerekir. Azure Machine learning'de bizim denemesinde önce kullandığımız bir **Select Columns in Dataset sütun** bu üç değişkenleri ayıklamak için modülü. Sonraki PCA tabanlı anomali algılama modülü anomali algılama modelini oluşturmak için kullanırız. 

Asıl bileşen analiz (PCA), özellik seçimi, Sınıflandırma ve anomali algılama uygulanan machine learning'de kurulmuş bir tekniktir. PCA durum büyük olasılıkla bağıntılı değişkenleri asıl bileşenleri adı verilen değerleri kümesine içeren bir dizi dönüştürür. PCA tabanlı modelleme anahtar fikrini küçük boyutlu bir alanı üzerine proje verileri, böylelikle daha kolay özellikleri ve anormallikleri tanımlanabilir.

Her yeni giriş algılama modeline anomali algılayıcısı ilk eigenvectors üzerinde kendi projeksiyon hesaplar ve normalleştirilmiş yeniden yapılandırma hata hesaplar. Anomali puanı bu normalleştirilmiş hatasıdır. Yüksek hata, daha anormal örneğidir. 

3 boyutlu boşluk nokta koordinatları lastiği baskısı, altyapısı Petrol ve altyapısı sıcaklık tarafından tanımlanan bakım algılama sorunu her kayıt kabul edilebilir. Bu anormallikleri yakalamak için biz 3 boyutlu alanı özgün verileri PCA kullanarak 2 boyutlu boşluk yansıtabilirsiniz. Bu nedenle, parametresi 2 PCA içinde kullanmak için bileşenlerin sayısını ayarlayın. Bu parametre, PCA tabanlı anomali algılama uygulama önemli bir rol oynar. PCA kullanarak planlanması verileri sonra Biz bu anormallikleri daha kolay tanımlayabilirsiniz.

**Geri çağırma anomali algılama modelini** Select Columns in Dataset ve PCA tabanlı anomali sütun kullanırız geri çağırma anomali algılama modelinde algılama modüllerini benzer şekilde. Özellikle, biz ilk üç değişkenleri - sıcaklık ve hız dışında altyapısı sıcaklık - kullanarak ayıklamak **Select Columns in Dataset sütun** modülü. Altyapısı sıcaklık hızı için genellikle bağıntılı bu yana biz de hızı değişken içerir. Sonraki PCA tabanlı anomali algılama modülü 2 boyutlu boşluk 3 boyutlu alanı verilerden proje için kullanırız. Geri çağırma ölçütlerini karşılamadı ve bu nedenle altyapısı sıcaklık ve dış sıcaklığı yüksek oranda olumsuz bağıntılı olduğunda araç geri çağırma gerektirir. PCA tabanlı anomali algılama algoritması kullanan, biz anormallikleri PCA gerçekleştirildikten sonra yakalayabilirsiniz. 

Her iki modeli eğitimindeki biz Bakım veya geri çağırma PCA tabanlı anomali algılama modelini eğitmek için girdi verisi olarak gerektirmez normal veri kullanmanız gerekir. Puanlama denemesinde eğitilen anomali algılama modelini araç Bakım veya geri çağırma gerektirip gerektirmediğini algılamak için kullanırız. 

### <a name="real-time-analysis"></a>Gerçek zamanlı analiz
Aşağıdaki akış analizi SQL sorgusunu araç hızı, yakıt düzeyi, altyapısı sıcaklık, Kilometre Sayacı okuma, lastiği baskısı, altyapısı Petrol düzeyinin ve diğerleri gibi tüm önemli araç parametreleri ortalamasını almak için kullanılır. Ortalamalar anormallikleri algılayıp, uyarılar, sorun belirli bölgede işletilen taşıtlardan genel sistem durumu koşulları belirlemek ve demografisine için ilişkilendirmek için kullanılır. 

![Stream Analytics sorgu için gerçek zamanlı işleme](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig13-vehicle-telematics-stream-analytics-query-for-real-time-processing.png)

*Şekil 13 – Stream Analytics sorgu için gerçek zamanlı işleme*

Tüm ortalamalar 3 saniyelik TumblingWindow hesaplanır. Biz çakışmayan ve bitişik zaman aralıkları gerektiren bu yana TubmlingWindow bu durumda kullanıyoruz. 

Azure Stream Analytics tüm "Pencereleme" özellikleri hakkında daha fazla bilgi edinmek için tıklayın [Pencereleme (Azure akış analizi)](https://msdn.microsoft.com/library/azure/dn835019.aspx).

**Gerçek zamanlı tahmin**

Bir uygulamayı gerçek zamanlı machine learning modelini faaliyete çözümün parçası olarak dahil edilir. "RealTimeDashboardApp" olarak adlandırılan bu uygulama oluşturulur ve çözüm dağıtımının bir parçası yapılandırılmış. Uygulama aşağıdakileri gerçekleştirir:

1. Olay hub'ı örneği burada Stream Analytics olayları bir modelinde sürekli yayımlama dinler. ![Verileri yayımlamak için stream Analytics sorgu](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig14-vehicle-telematics-stream-analytics-query-for-publishing.png) *Şekil 14 – bir çıkış veri yayımlamak için Stream Analytics sorgu olay hub'ı örneği* 
2. Bu uygulamayı alır her olay için: 
   
   * Machine Learning istek-yanıt Puanlama (RR) uç noktası kullanarak verileri işler. RRS endpoint dağıtımının bir parçası otomatik olarak yayımlanır.
   * RRS çıkış push API'lerini kullanarak bir Power BI veri kümesine yayımlanır.

Bu deseni, bir iş kolu (LoB) uygulaması uyarılar, bildirimler ve mesajlaşma gibi senaryolar için gerçek zamanlı analiz akış ile tümleştirmek istediğiniz senaryoları için de geçerlidir.

Tıklatın [RealtimeDashboardApp indirme](http://go.microsoft.com/fwlink/?LinkId=717078) özelleştirmeler RealtimeDashboardApp Visual Studio çözümü karşıdan yüklemek için. 

**Gerçek zamanlı Pano uygulamasını çalıştırmak için**
1. Ayıklayın ve yerel olarak Kaydet ![RealtimeDashboardApp klasörü](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig16-vehicle-telematics-realtimedashboardapp-folder.png) *Şekil 16 – RealtimeDashboardApp klasörü*  
2. Uygulamanın RealtimeDashboardApp.exe yürütme
3. Geçerli Power BI kimlik bilgilerini sağlayın, oturum açın ve Kabul Et'i tıklatın ![Gerçek zamanlı Pano uygulama oturum açma Power BI](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig17a-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png) ![Gerçek zamanlı Pano uygulaması son oturum açma Power BI](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig17b-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png) 

*Şekil 17 – RealtimeDashboardApp: Oturum açma Power BI*

>[!NOTE] 
>Power BI veri kümesi temizlemek isterseniz, "flushdata" parametresiyle RealtimeDashboardApp yürütün: 

    RealtimeDashboardApp.exe -flushdata


### <a name="batch-analysis"></a>Toplu iş analiz
Burada amaç, Contoso Motors düzeni, kullanım davranışı ve araç durumunu yürüten üzerinde zengin serisidir büyük veri harekete geçirmek Azure işlem özellikleri nasıl kullanacağını göstermektir. Bu, mümkün kılar:

* Müşteri deneyimini geliştirmek ve alışkanlık ve yakıt verimli yönlendirmeli davranışları yürüten üzerinde Öngörüler sağlayarak daha ucuzdur yapın
* Müşteriler ve bunların yönlendirmeli patters iş kararları yönetir ve en iyi sınıf ürünler ve hizmetler sağlamak için proaktif olarak öğrenin

Bu çözümde, biz aşağıdaki ölçümleri hedeflediğiniz:

1. **Davranış yürüten etkin**: modelleri, konumları, yönlendirmeli koşullar, saat ve agresif yönlendirmeli modeli Öngörüler elde etmek için yılın eğilimi tanımlar. Contoso Motors bu Öngörüler kişiselleştirilmiş yeni özellikler ve kullanım tabanlı sigorta yürüten pazarlama kampanyaları için kullanabilirsiniz.
2. **Yakıt Al verimli yönlendirmeli davranışı**: modelleri, konumları, yönlendirmeli koşullar, saat ve yakıt verimli yönlendirmeli modeli Öngörüler elde etmek için yılın eğilimi tanımlar. Contoso Motors bu Öngörüler, pazarlama kampanyalarının için yeni özellikler yürüten kullanabilirsiniz ve sürücülerini öngörülü raporlama etkili ve ortam kolay yönlendirmeli alışkanlıklarınıza maliyeti. 
3. **Modelleri geri çağırma**: anomali algılama makine öğrenimi denemesinin faaliyete geçirmeye yönelik geri çekme gerektiren modelleri tanımlar

Her bir bu ölçümleri ayrıntılara göz atalım,

**Agresif yönlendirmeli düzeni**

Bölümlenmiş araç sinyalleri ve Tanılama verileri ", agresif yürüten sergiler, modelleri, konum, araç, yönlendirmeli koşullar ve diğer parametreleri belirlemek için Hive kullanarak AggresiveDrivingPatternPipeline" adlı ardışık düzeninde işlenir Desen.

![Agresif yönlendirmeli düzeni iş akışı](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig18-vehicle-telematics-aggressive-driving-pattern.png) 
*Şekil 18 – agresif yönlendirmeli düzeni iş akışı*


***Agresif yönlendirmeli düzeni Hive sorgusu***

"Aggresivedriving.hql agresif yönlendirmeli koşul düzeni çözümlemek için kullanılan" adlandırılmış Hive betiğini indirilen ZIP "\demo\src\connectedcar\scripts" klasör bulunur. 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDINPUT}';

    DROP TABLE IF EXISTS CarEventsAggresive; 
    CREATE EXTERNAL TABLE CarEventsAggresive
    (
                   vin                         string, 
                model                        string,
                timestamp                    string,
                city                        string,
                speed                          string,
                transmission_gear_position    string,
                brake_pedal_status            string,
                Year                        string,
                Month                        string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:AGGRESIVEOUTPUT}';



    INSERT OVERWRITE TABLE CarEventsAggresive
    select
    vin,
    model,
    timestamp,
    city,
    speed,
    transmission_gear_position,
    brake_pedal_status,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from PartitionedCarEvents
    where transmission_gear_position IN ('fourth', 'fifth', 'sixth', 'seventh', 'eight') AND brake_pedal_status = '1' AND speed >= '50'


En yüksek hızda düzeni braking göre reckless/agresif yönlendirmeli davranışlarını algılamak için araç'ın iletim dişli konumu, Fren pedal durumu ve hız birleşimini kullanır. 

Ardışık Düzen başarılı bir şekilde yürütüldükten sonra depolama hesabınızda "connectedcar" kapsayıcısı altında oluşturulur şu bölümlerde bakın.

![AggressiveDrivingPatternPipeline çıkış](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19-vehicle-telematics-aggressive-driving-pattern-output.png) 

*Şekil 19 – AggressiveDrivingPatternPipeline çıkış*

**Yakıt verimli yönlendirmeli düzeni**

Bölümlenmiş araç sinyalleri ve tanılama veri "FuelEfficientDrivingPatternPipeline" adlı ardışık düzeninde işlenir. Hive modelleri, konum, araç, yönlendirmeli koşullar ve yakıt verimli yönlendirmeli düzeni sergilemesine diğer özelliklerini belirlemek için kullanılır.

![Fuel-Efficient yönlendirmeli düzeni](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19-vehicle-telematics-fuel-efficient-driving-pattern.png) 

*Şekil 20 – Fuel-efficient yönlendirmeli düzeni iş akışı*

***Yakıt verimli yönlendirmeli düzeni Hive sorgusu***

"Fuelefficientdriving.hql agresif yönlendirmeli koşul düzeni çözümlemek için kullanılan" adlandırılmış Hive betiğini indirilen ZIP "\demo\src\connectedcar\scripts" klasör bulunur. 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDINPUT}';

    DROP TABLE IF EXISTS FuelEfficientDriving; 
    CREATE EXTERNAL TABLE FuelEfficientDriving
    (
                   vin                         string, 
                model                        string,
                   city                        string,
                speed                          string,
                transmission_gear_position    string,                
                brake_pedal_status            string,            
                accelerator_pedal_position    string,                             
                Year                        string,
                Month                        string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:FUELEFFICIENTOUTPUT}';



    INSERT OVERWRITE TABLE FuelEfficientDriving
    select
    vin,
    model,
    city,
    speed,
    transmission_gear_position,
    brake_pedal_status,
    accelerator_pedal_position,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from PartitionedCarEvents
    where transmission_gear_position IN ('fourth', 'fifth', 'sixth', 'seventh', 'eight') AND parking_brake_status = '0' AND brake_pedal_status = '0' AND speed <= '60' AND accelerator_pedal_position >= '50'


Araç'ın iletim dişli konumu bileşimini kullanır, Fren pedal durumu, hızlı ve Hızlandırıcı braking, hızlandırmasını, temel yakıt verimli yönlendirmeli davranışlarını algılamak için konumunu pedal ve desenler hızı. 

Ardışık Düzen başarılı bir şekilde yürütüldükten sonra depolama hesabınızda "connectedcar" kapsayıcısı altında oluşturulur şu bölümlerde bakın.

![FuelEfficientDrivingPatternPipeline çıkış](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig20-vehicle-telematics-fuel-efficient-driving-pattern-output.png) 

*Şekil 21 – FuelEfficientDrivingPatternPipeline çıkış*

**Tahminleri geri çağırma**

Deneme öğrenme makine sağlandığında ve çözüm dağıtımının bir parçası olarak bir web hizmeti olarak yayımlanır. Data factory bağlantılı hizmet olarak kayıtlı ve veri fabrikası toplu iş Puanlama etkinliği kullanarak kullanıma hazır hale getirilmiş bu iş akışında, toplu Puanlama uç noktası yararlanır.

![Machine Learning uç noktası](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig21-vehicle-telematics-machine-learning-endpoint.png) 

*Veri fabrikasında bağlı hizmet olarak Şekil 22 – Machine learning uç noktası kayıtlı*

Kayıtlı bağlantılı hizmet DetectAnomalyPipeline anomali algılama modelini kullanarak veri Puanlama amacıyla kullanılır. 

![Machine Learning toplu veri fabrikasında Puanlama etkinliği](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig22-vehicle-telematics-aml-batch-scoring.png) 

*Şekil 23 – veri fabrikasında Azure Machine Learning toplu Puanlama etkinliği* 

Web hizmeti Puanlama toplu işlemle operationalized böylece bu ardışık düzendeki veri hazırlığı için gerçekleştirilen birkaç adım vardır. 

![Geri çekme gerektiren taşıtlardan tahmin etmeye yönelik DetectAnomalyPipeline](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig23-vehicle-telematics-pipeline-predicting-recalls.png) 

*Şekil 24 – geri çekme gerektiren taşıtlardan tahmin etmeye yönelik DetectAnomalyPipeline* 

***Anomali algılama Hive sorgusu***

Puanlama tamamlandığında, bir Hdınsight etkinliği işlemek ve anormallikleri 0.60 olasılık puanı modeli tarafından olarak kategorilere ayrılmış veya daha yüksek veri toplamak için kullanılır.

    DROP TABLE IF EXISTS CarEventsAnomaly; 
    CREATE EXTERNAL TABLE CarEventsAnomaly 
    (
                vin                            string,
                model                        string,
                gendate                        string,
                outsidetemperature            string,
                enginetemperature            string,
                speed                        string,
                fuel                        string,
                engineoil                    string,
                tirepressure                string,
                odometer                    string,
                city                        string,
                accelerator_pedal_position    string,
                parking_brake_status        string,
                headlamp_status                string,
                brake_pedal_status            string,
                transmission_gear_position    string,
                ignition_status                string,
                windshield_wiper_status        string,
                abs                          string,
                maintenanceLabel            string,
                maintenanceProbability        string,
                RecallLabel                    string,
                RecallProbability            string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:ANOMALYOUTPUT}';

    DROP TABLE IF EXISTS RecallModel; 
    CREATE EXTERNAL TABLE RecallModel 
    (

                vin                            string,
                model                        string,
                city                        string,
                outsidetemperature            string,
                enginetemperature            string,
                speed                        string,
                Year                        string,
                Month                        string                

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:RECALLMODELOUTPUT}';

    INSERT OVERWRITE TABLE RecallModel
    select
    vin,
    model,
    city,
    outsidetemperature,
    enginetemperature,
    speed,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from CarEventsAnomaly
    where RecallLabel = '1' AND RecallProbability >= '0.60'


Ardışık Düzen başarılı bir şekilde yürütüldükten sonra depolama hesabınızda "connectedcar" kapsayıcısı altında oluşturulur şu bölümlerde bakın.

![DetectAnomalyPipeline çıkış](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig24-vehicle-telematics-detect-anamoly-pipeline-output.png) 

*Şekil 25 – DetectAnomalyPipeline çıkış*

## <a name="publish"></a>Yayımlama

### <a name="real-time-analysis"></a>Gerçek zamanlı analiz
Bir çıkış akış analizi işi sorgularda birini yayımlar olayların Event Hub örneği. 

![Akış analizi işi yayımlar için çıktı olay hub'ı örneği](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig25-vehicle-telematics-stream-analytics-job-publishes-output-event-hub.png)

*Şekil 26 – Stream Analytics işi yayımlar için çıktı olay hub'ı örneği*

![Stream Analytics sorgu çıkışı yayımlamak için olay hub'ı örneği](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig26-vehicle-telematics-stream-analytics-query-publish-output-event-hub.png)

*Şekil 27 – çıkışı yayımlamak için Stream Analytics sorgu olay hub'ı örneği*

Bu akış olayların çözümdeki RealTimeDashboardApp tarafından kullanılır. Bu uygulamayı gerçek zamanlı Puanlama için makine öğrenme istek-yanıt web hizmetini kullanır ve Power BI veri kümesi tüketimi için sonuç verileri yayımlar. 

### <a name="batch-analysis"></a>Toplu iş analiz
Toplu işlem ve gerçek zamanlı işleme sonuçlarını tüketimi için Azure SQL veritabanı tabloları yayımlanır. Azure SQL Server, veritabanı ve tablo Kurulum komut dosyasının bir parçası otomatik olarak oluşturulur. 

![Veri reyonu akışına toplu işleme sonuçlarını kopyalayın](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig27-vehicle-telematics-batch-processing-results-copy-to-data-mart.png)

*Şekil 28 – toplu veri reyonu akışına sonuçları kopyalama işleme*

![Akış analizi işi veri reyonuna yayımlar](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig28-vehicle-telematics-stream-analytics-job-publishes-to-data-mart.png)

*Şekil 29 – Stream Analytics işi veri reyonuna yayımlar*

![Veri reyonu ayarında Stream Analytics işi](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig29-vehicle-telematics-data-mart-setting-in-stream-analytics-job.png)

*Şekil 30 – Data mart Stream Analytics işinde ayarlama*

## <a name="consume"></a>Tüketme
Power BI Bu çözüm gerçek zamanlı veri ve Tahmine dayalı analiz görselleştirmeleri için zengin bir Pano sağlar. 

Power BI raporları ve panoyu ayarlama hakkında ayrıntılı yönergeler için buraya tıklayın. Son Pano şöyle görünür:

![Power BI Panosu](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig30-vehicle-telematics-powerbi-dashboard.png)

*Şekil 31 - Power BI Panosu*

## <a name="summary"></a>Özet
Bu belgede ayrıntılı ayrıntıya araç Telemetri analizi çözümün içerir. Bu bir lambda mimarisi desenini paylaşan gerçek zamanlı ve toplu analytics Öngörüler ve Eylemler ile. Çeşitli etkin yolunuzda (gerçek zamanlı) gerektiren kullanım durumları için bu deseni uygular ve yolunuzda (toplu) analytics. 

