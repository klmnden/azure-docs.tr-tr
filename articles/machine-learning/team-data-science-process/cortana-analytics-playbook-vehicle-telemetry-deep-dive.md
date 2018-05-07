---
title: Derin Dalış araç sağlık ve yürüten tahmin etmek nasıl içine alışkanlıklarınıza - Azure | Microsoft Docs
description: Araç sistem durumu ve yürüten gerçek zamanlı ve Tahmine dayalı Öngörüler elde etmek için Cortana Intelligence yeteneklerini kullanabilir alışkanlıklarınıza.
services: machine-learning
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: d8866fa6-aba6-40e5-b3b3-33057393c1a8
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2018
ms.author: deguhath
ms.openlocfilehash: 10fe87757a6da8a64e4fbd7fb624fef3e666714c
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="vehicle-telemetry-analytics-solution-playbook-deep-dive-into-the-solution"></a>Araç Telemetri analiz çözümü playbook: ayrıntılı çözüme daha yakından inceleyin
Bu playbook bölümlerini bu menü bağlantılar: 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../../includes/cap-vehicle-telemetry-playbook-selector.md)]

Bu belge kümede ayrıntısına gider her çözüm mimarisinde gösterilen aşamaları. Yönergeler ve özelleştirme işaretçileri dahil edilir. 

## <a name="data-sources"></a>Veri kaynakları
Çözüm iki farklı veri kaynakları kullanır:

* Benzetimli araç sinyalleri ve tanılama veri kümesi
* Araç Kataloğu

Aşağıdaki ekran görüntüsünde gösterildiği gibi bir araç telematik simulator Bu çözüm, bir parçası olarak dahil edilir. Tanılama bilgileri ve araç durumunu ve belirli bir noktada yönlendirmeli düzeni zamanında karşılık gelen sinyalleri yayar. Araç telematik Simulator Visual Studio çözümü gereksinimlerinize göre özelleştirmeler indirmek için Git [araç telematik simulator](http://go.microsoft.com/fwlink/?LinkId=717075) Web sayfası. Araç katalog araç tanımlama numaraları (VINs) modellerine eşleyen bir başvuru veri kümesi içerir.

![Araç telematik simulator](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig1-vehicle-telematics-simulator.png)


Bu JSON biçimli veri kümesinin aşağıdaki şema içerir.

| Sütun | Açıklama | Değerler |
| --- | --- | --- |
| TOPLAMIDIR |Rastgele oluşturulmuş Toplamıdır |10.000 rastgele oluşturulmuş VINs ana listesinden elde |
| Dış sıcaklığı |Aracın burada yürüten dış sıcaklığı |0 ile 100 rastgele oluşturulmuş sayı |
| Altyapısı sıcaklık |Aracın altyapısı sıcaklığını |0'dan rastgele oluşturulmuş bir sayıya 500 |
| Hız |Aracın yönlendiren altyapısı hızı |0 ile 100 rastgele oluşturulmuş sayı |
| Yakıt |Aracın yakıt düzeyi |Rastgele oluşturulan numarası 0 ile 100'e (yakıt düzeyi yüzdesi gösterir) |
| EngineOil |Aracın altyapısı Petrol düzeyi |Rastgele oluşturulan numarası 0 ile 100'e (altyapısı Petrol düzeyi yüzdesi gösterir) |
| Lastiği baskısı |Aracın lastiği Basıncı |Rastgele oluşturulan numarası 0 ile 50 (lastiği baskısı düzeyi yüzdesi gösterir) |
| Kilometre Sayacı |Aracın Kilometre Sayacı okuma |Rastgele oluşturulmuş 0'dan numara 200.000 |
| Accelerator_pedal_position |Aracın Hızlandırıcı pedal konumu |Rastgele oluşturulan numarası 0 ile 100'e (Hızlandırıcı düzeyi yüzdesi gösterir) |
| Parking_brake_status |Aracın veya yerleşmiş durumdayken gösterir |TRUE veya False |
| Headlamp_status |Ön ışık üzerinde olup olmadığını gösterir |TRUE veya False |
| Brake_pedal_status |Fren pedal veya basılı olup olmadığını gösterir |TRUE veya False |
| Transmission_gear_position |Aracın iletim dişli konumu |Durumları: ilk olarak, üçüncü, dördüncü beşinci, altıncı seventh, sekizinci ikinci |
| Ignition_status |Aracın çalışıyor veya durdurulmuştur olup olmadığını gösterir |TRUE veya False |
| Windshield_wiper_status |Ön wiper veya açık olup olmadığını gösterir |TRUE veya False |
| ABS |ABS veya bağlı olup olmadığını gösterir |TRUE veya False |
| Zaman damgası |Veri noktası oluşturulduğunda, zaman damgası |Tarih |
| Şehir |Aracın konumu |Bu çözümdeki dört Şehir: Bellevue, Redmond, Sammamish, Seattle |

Araç model başvuru veri kümesi VINs modellerine eşler. 

| TOPLAMIDIR | Model |
| --- | --- |
| FHL3O1SA4IEHB4WU1 |Sedan |
| 8J0U8XCPRGW4Z3NQE |Karma |
| WORG68Z2PLTNZDBI7 |Aile saloon |
| JTHMYHQTEPP4WBMRN |Sedan |
| W9FTHG27LZN1YWO0Y |Karma |
| MHTP9N792PHK08WJM |Aile saloon |
| EI4QXI2AXVQQING4I |Sedan |
| 5KKR2VB4WHQH97PF8 |Karma |
| W9NSZ423XZHAONYXB |Aile saloon |
| 26WJSGHX4MA5ROHNL |Dönüştürülebilir |
| GHLUB6ONKMOSI7E77 |İstasyon wagon |
| 9C2RHVRVLMEJDBXLP |Küçük otomobil |
| BRNHVMZOUJ6EOCP32 |Küçük SUV |
| VCYVW0WUZNBTM594J |Spor araba |
| HNVCE6YFZSA5M82NY |Orta SUV |
| 4R30FOR7NUOBL05GJ |İstasyon wagon |
| WYNIIY42VKV6OQS1J |Büyük SUV |
| 8Y5QKG27QET1RBK7I |Büyük SUV |
| DF6OX2WSRA6511BVG |Coupe |
| Z2EOZWZBXAEW3E60T |Sedan |
| M4TV6IEALD5QDS3IR |Karma |
| VHRA1Y2TGTA84F00H |Aile saloon |
| R0JAUHT1L1R3BIKI0 |Sedan |
| 9230C202Z60XX84AU |Karma |
| T8DNDN5UDCWL7M72H |Aile saloon |
| 4WPYRUZII5YV7YA42 |Sedan |
| D1ZVY26UV2BFGHZNO |Karma |
| XUF99EW9OIQOMV7Q7 |Aile saloon |
| 8OMCL3LGI7XNCC21U |Dönüştürülebilir |
| ……. | |

## <a name="ingestion"></a>Alım
Azure Event Hubs, Azure akış analizi ve Azure Data Factory birleşimleri araç sinyalleri, tanılama olayları alma için kullanılır ve gerçek zamanlı ve toplu analizi. Tüm bu bileşenlerin oluşturulur ve çözüm dağıtımının bir parçası yapılandırılmış. 

### <a name="real-time-analysis"></a>Gerçek zamanlı analiz
Araç telematik simulator tarafından oluşturulan olayları event hub'ına olay hub'ı SDK kullanarak yayımlanır.  

![Olay hub'ı Panosu](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig4-vehicle-telematics-event-hub-dashboard.png) 

Stream Analytics işi, bu olayları olay hub'ı alır ve araç durumu çözümlemek için gerçek zamanlı verileri işler.

![Veriler işlenirken Analytics iş akışı](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig5-vehicle-telematics-stream-analytics-job-processing-data.png) 


Stream Analytics işi:

* Olay hub'ı verileri alır.
* Araç Toplamıdır karşılık gelen modeline eşlemek için başvuru verileri ile bir birleştirme gerçekleştirir. 
* Bunları zengin toplu analiz için Azure Blob depolama alanına devam ettirir. 

Aşağıdaki akış analizi sorgu verileri Blob depolama alanına kalıcı hale getirmek için kullanılır: 

![Akış analizi işi sorgu veri alımı için](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig6-vehicle-telematics-stream-analytics-job-query-for-data-ingestion.png) 


### <a name="batch-analysis"></a>Toplu iş analiz
Ek bir birim benzetimli araç sinyalleri ve tanılama veri kümesi için daha zengin toplu analiz de oluşturulur. Bu ek bir birim toplu işlem için iyi temsili veri birimi emin olmak için gereklidir. Bu amaçla PrepareSampleDataPipeline veri fabrikası iş akışı içinde bir yılın tutarında benzetimli araç sinyalleri ve tanılama veri kümesi oluşturmak için kullanılır. Veri Fabrikası özel .NET etkinlik gereksinimlerinize göre özelleştirmeleri için Visual Studio çözümü indirmek için Git [Data Factory özel etkinlik](http://go.microsoft.com/fwlink/?LinkId=717077) Web sayfası. 

Bu iş akışı örneği verileri toplu işleme için hazırlanmış gösterir.

![Toplu işleme iş akışı için hazırlanmış örnek veriler](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig7-vehicle-telematics-prepare-sample-data-for-batch-processing.png) 


Ardışık Düzen özel bir veri fabrikası .NET etkinliğini oluşur.

![PrepareSampleDataPipeline etkinliği](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig8-vehicle-telematics-prepare-sample-data-pipeline.png) 

Ardışık Düzen başarıyla yürütür ve RawCarEventsTable veri kümesi "Hazır" olarak işaretlenmiş sonra bir yılın tutarında benzetimli araç sinyalleri ve tanılama veri oluşturulur. Aşağıdaki klasör ve dosya depolama hesabınızda connectedcar kapsayıcısı altında oluşturulan bakın:

![PrepareSampleDataPipeline çıkış](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig9-vehicle-telematics-prepare-sample-data-pipeline-output.png) 

## <a name="partition-the-data-set"></a>Bölüm veri kümesi
Veri hazırlama adımında ham yarı yapılandırılmış araç sinyalleri ve tanılama veri kümesi yıl/ay biçimine bölümlenir. Bu bölümleme daha verimli şekilde sorgulamak ve ölçeklenebilir uzun vadeli depolama hatası üzerinden etkinleştirerek yükseltir. İlk blob hesabı dolduğunda, örneğin, bunu üzerinden sonraki hesabına hataları. 

>[!NOTE] 
>Bu adım çözümdeki yalnızca toplu işleme için geçerlidir.

Giriş ve çıkış veri yönetimi:

* **Çıktı verilerini** (etiketli PartitionedCarEventsTable) uzun bir süre için Müşteri'nin veri gölü veri temel / "rawest" form olarak tutulur. 
* **Giriş verileri** çıktı verilerini tam uygunluğunu girişine sahip olduğu için bu ardışık düzen genellikle atılır. (Daha sonra kullanmak için bölümlenmiş) depolanır.

Bölüm araba olayları iş akışı.

![Bölüm araba olayları iş akışı](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig10-vehicle-telematics-partition-car-events-workflow.png)


Ham verileri PartitionCarEventsPipeline içinde bir Azure Hdınsight Hive etkinliğini kullanarak aşağıdaki ekran görüntüsünde gösterildiği gibi bölümlenmiş. Veri hazırlama adımında bir yıl için oluşturulan örnek verileri yıl/AYA göre bölümlenmiş. Bölümler araç sinyalleri ve tanılama verilerini her ay yılın (toplam 12 bölümlerinin) oluşturmak için kullanılır. 

![PartitionCarEventsPipeline etkinliği](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig11-vehicle-telematics-partition-car-events-pipeline.png)


**PartitionConnectedCarEvents Hive betiği**

Hive betiği partitioncarevents.hql bölümleme için kullanılır. İndirilen ZIP dosyasının \demo\src\connectedcar\scripts klasöründe bulunur. 
    
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

Ardışık Düzen başarıyla yürütüldükten sonra depolama hesabınızda connectedcar kapsayıcısı altında oluşturulur şu bölümlerde bakın:

![Bölümlenmiş çıktı](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig12-vehicle-telematics-partitioned-output.png)

Veriler artık, daha kolay yönetilebilir ve zengin toplu Öngörüler kazanmak için daha fazla işleme için hazır duruma getirilmiştir. 

## <a name="data-analysis"></a>Veri çözümlemesi
Bu bölümde, akış analizi, Azure Machine Learning, veri fabrikası ve Hdınsight araç sistem durumunu gelişmiş analizler ve alışkanlık yürüten zengin için birleştirme bakın.

### <a name="machine-learning"></a>Makine öğrenimi
Burada amaç, Bakım veya aşağıdaki varsayımları temel alarak, belirli bir sistem durumu istatistikleri temel geri çağırma gerektiren taşıtlardan tahmin etmektir:

* Aşağıdaki üç koşullardan biri doğruysa taşıtlardan bakım bakım gerektirir:
  
  * Lastiği baskısı düşüktür.
  * Altyapısı Petrol düşük düzeydir.
  * Altyapısı sıcaklık yüksektir.

* Aşağıdaki koşullardan biri doğruysa taşıtlardan bir güvenlik sorunu sahip ve geri çağırma gerektirir:
  
  * Altyapısı sıcaklık yüksekse, ancak dış sıcaklığı düşüktür.
  * Altyapısı sıcaklık düşüktür, ancak dış sıcaklığı yüksektir.

Önceki gereksinimlerine bağlı olarak, iki ayrı modelleri anormallikleri algıla. Bir model araç bakım algılama için ve araç geri çağırma algılaması için bir model şeklindedir. Her iki modellerinde yerleşik asıl bileşen analiz (PCA) algoritması anomali algılama için kullanılır. 

#### <a name="maintenance-detection-model"></a>**Bakım algılama modeli**

Varsa, üç göstergeleri--lastiği baskısı, altyapısı Petrol veya altyapısı sıcaklık--ilgili koşulu karşılayan, bakım algılama modelini bir anomali raporları. Sonuç olarak, bu üç değişkenleri olması dikkate almanız model oluşturmaya gerekir. Machine learning'de denemede **Select Columns in Dataset sütun** modülü, bu üç değişkenleri ayıklamak için kullanılır. Ardından, PCA tabanlı anomali algılama modülü anomali algılama modelini oluşturmak için kullanılır. 

PCA özellik seçimi, Sınıflandırma ve anomali algılama uygulanan machine learning'de kurulmuş bir tekniktir. PCA büyük olasılıkla bağıntılı değişkenleri asıl bileşenleri adı verilen değerleri kümesine içermesi kümesi dönüştürür. Anahtar PCA tabanlı model özellikleri ve anormallikleri daha kolay tanımlamak için küçük boyutlu bir alanı üzerine proje verilere olur.

İçin yeni her giriş algılama modeline anomali algılayıcısı ilk eigenvectors üzerinde kendi projeksiyon hesaplar. Ardından, normalleştirilmiş yeniden yapılandırma hata hesaplar. Bu anomali puan normalleştirilmiş hatadır: yüksek hata, daha anormal örneği. 

Üç boyutlu boşluk nokta koordinatları lastiği baskısı, altyapısı Petrol ve altyapısı sıcaklık tarafından tanımlanan bakım algılama sorunu her kayıt olarak kabul edilir. Bu anormallikleri yakalamak için PCA iki boyutlu bir alanı üç boyutlu alanı özgün verileri proje için kullanılır. Bu nedenle, bileşenleri içinde PCA kullanmak için parametre sayısı iki olarak ayarlanır. Bu parametre, PCA tabanlı anomali algılama uygulama önemli bir rol oynar. Proje verilerini PCA kullandıktan sonra bu anormallikleri daha kolay tanımlanır.

#### <a name="recall-anomaly-detection-model"></a>**Anomali algılama modelini geri çağırma**

Geri çağırma anomali algılama modelinde **Select Columns in Dataset sütun** ve PCA tabanlı anomali algılama modülleri benzer şekilde kullanılır. Özellikle, üç değişkenleri--altyapısı sıcaklık, dış sıcaklık ve hız--ayıklanır ilk kullanarak **Select Columns in Dataset sütun** modülü. Hızı değişkeni de altyapısı sıcaklık hızı için genellikle eşleştiğinden bulunmaktadır. Ardından, PCA tabanlı anomali algılama modülü iki boyutlu bir alanı üç boyutlu alanı verilerden proje için kullanılır. Geri çağırma ölçütleri karşılanır. Aracın altyapısı sıcaklık ve dış sıcaklığı yüksek oranda olumsuz bağıntılı geri çağırma gerektirir. PCA gerçekleştirildikten sonra PCA tabanlı anomali algılama algoritması anormallikleri yakalamak için kullanılır. 

Her iki modeli eğitimindeki normal veri PCA tabanlı anomali algılama modelini eğitmek için girdi verisi olarak kullanılır. (Normal veri Bakım veya geri çağırma gerektirmez.) Puanlama denemesinde eğitilen anomali algılama modelini araç Bakım veya geri çağırma gerekli olup olmadığını algılamak için kullanılır. 

### <a name="real-time-analysis"></a>Gerçek zamanlı analiz
Aşağıdaki akış analizi SQL sorgusunu tüm önemli araç parametreleri ortalamasını almak için kullanılır. Bu parametreler araç hızı, yakıt düzeyi, altyapısı sıcaklık, Kilometre Sayacı okuma, lastiği baskısı, altyapısı Petrol düzeyinin ve diğerleri içerir. Ortalamalar anormallikleri algılamak, uyarıları vermek ve belirli bir bölgede işletilen taşıtlardan genel sistem durumu koşulları belirlemek için kullanılır. Ortalamalar sonra demografisine için bağıntılı olan. 

![Stream Analytics sorgu için gerçek zamanlı işleme](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig13-vehicle-telematics-stream-analytics-query-for-real-time-processing.png)

Tüm ortalamalar üç saniyelik atlayan pencere hesaplanır. Atlayan pencere çakışmayan ve bitişik zaman aralıkları gerektiği için kullanılır. 

Stream Analytics Pencereleme özellikleri hakkında daha fazla bilgi için bkz: [Pencereleme (Azure akış analizi)](https://msdn.microsoft.com/library/azure/dn835019.aspx).

#### <a name="real-time-prediction"></a>**Gerçek zamanlı tahmin**

Bir uygulamayı gerçek zamanlı machine learning modelini faaliyete çözümün parçası olarak dahil edilir. RealTimeDashboardApp uygulama oluşturulur ve çözüm dağıtımının bir parçası yapılandırılmış. Uygulama:

* Stream Analytics olayları bir modelinde sürekli yayımladığı bir olay hub'ı örneğine dinler.

    ![Stream Analytics sorgu veri yayımlamak için](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig14-vehicle-telematics-stream-analytics-query-for-publishing.png) 

* Olaylarını alır. Bu uygulamayı alır her olay için: 
   
   * Veriler, machine learning Puanlama istek-yanıt (RRS) uç noktası kullanılarak işlenir. RRS endpoint dağıtımının bir parçası otomatik olarak yayımlanır.
   * RRS çıkış API'leri gönderme yöntemini kullanarak bir Power BI veri kümesine yayımlanır.

Bu deseni, ayrıca iş kolu satır uygulama gerçek zamanlı analiz akış ile tümleştirmek istediğiniz senaryolar için geçerlidir. Bu senaryolar, uyarılar, bildirimler ve mesajlaşma içerir.

Özelleştirmeler RealtimeDashboardApp Visual Studio çözümü indirmek için bkz: [RealtimeDashboardApp indirme](http://go.microsoft.com/fwlink/?LinkId=717078) Web sayfası. 

#### <a name="execute-the-real-time-dashboard-application"></a>**Gerçek zamanlı Pano uygulamanın yürütme**
1. RealtimeDashboardApp ayıklamak ve yerel olarak kaydedin.

    ![RealTimeDashboardApp klasörü](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig16-vehicle-telematics-realtimedashboardapp-folder.png) 

2. Uygulama RealtimeDashboardApp.exe yürütün.

3. Geçerli Power BI kimlik bilgilerinizi girin ve seçin **oturum**.  

    ![Oturum açma gerçek zamanlı Pano uygulama penceresi](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig17a-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png) 
    
4. Seçin **kabul**.

    ![Gerçek zamanlı Pano uygulama son oturum açma penceresi](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig17b-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png) 

>[!NOTE] 
>Power BI veri kümesi temizlemek isterseniz, "flushdata" parametresiyle RealtimeDashboardApp yürütün. 

    RealtimeDashboardApp.exe -flushdata


### <a name="batch-analysis"></a>Toplu iş analiz
Burada amaç, Contoso Motors büyük veri harekete geçirmek Azure işlem özellikleri nasıl kullanacağını göstermektir. Bu veri yönlendirmeli modelleri, kullanım davranışı ve araç durumu zengin Öngörüler ortaya çıkarır. Bu bilgileri mümkün hale getirir:

* Müşteri deneyimini geliştirmek ve alışkanlık ve fuel-efficient yönlendirmeli davranışları yürüten üzerinde Öngörüler sağlayarak daha ucuzdur yapın.
* Proaktif olarak iş kararları yönetir ve en iyi sınıf ürünleri ve hizmetleri sağlamak için müşteriler ve bunların yönlendirmeli desenler hakkında bilgi edinin.

Bu çözümde aşağıdaki ölçümleri hedeflenir:

* **Davranış yürüten etkin**: modelleri, konumları, yönlendirmeli koşullar, saat ve agresif yönlendirmeli modeli serisidir yılın eğilimi tanımlar. Contoso Motors bu Öngörüler pazarlama kampanyaları için kişiselleştirilmiş yeni özellikler ve kullanım tabanlı sigorta tanıtmak için kullanabilirsiniz.
* **Fuel-Efficient yönlendirmeli davranışı**: modelleri, konumları, yönlendirmeli koşullar, saat ve fuel-efficient yönlendirmeli modeli serisidir yılın eğilimi tanımlar. Contoso Motors bu Öngörüler pazarlama kampanyaları için yeni özellikler ve uygun maliyetli ve ortam kolay yönlendirmeli alışkanlıklarınıza sürücülerini öngörülü raporlama tanıtmak için kullanabilirsiniz.
* **Modelleri geri çağırma**: anomali algılama makine öğrenimi denemesinin faaliyete geçirmeye yönelik geri çekme gerektiren modelleri tanımlar.

Her bir bu ölçümleri ayrıntılara göz atalım.

#### <a name="aggressive-driving-behavior-patterns"></a>**Agresif yönlendirmeli davranışı desenleri**

Bölümlenmiş araç sinyalleri ve tanılama veri AggresiveDrivingPatternPipeline aşağıdaki iş akışında gösterildiği gibi işlenir. Hive modelleri, konum, araç, yönlendirmeli koşullar ve agresif yönlendirmeli desenleri sergilemesine diğer parametreleri belirlemek için kullanılır.

![Agresif yönlendirmeli düzeni iş akışı](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig18-vehicle-telematics-aggressive-driving-pattern.png) 

***Agresif yönlendirmeli düzeni Hive sorgusu***

Hive betiği aggresivedriving.hql agresif yönlendirmeli koşul modelleri analiz etmek için kullanılır. İndirilen ZIP dosyasının \demo\src\connectedcar\scripts klasöründe bulunur. 

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


Komut dosyası en yüksek hızda desenleri braking göre reckless/agresif yönlendirmeli davranışlarını algılamak için bir araç 's iletim dişli konum, Fren pedal durumu ve hız birleşimini kullanır. 

Ardışık Düzen başarıyla yürütüldükten sonra depolama hesabınızda connectedcar kapsayıcısı altında oluşturulur şu bölümlerde bakın:

![AggressiveDrivingPatternPipeline çıkış](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19-vehicle-telematics-aggressive-driving-pattern-output.png) 


#### <a name="fuel-efficient-driving-behavior-patterns"></a>**Fuel-Efficient yönlendirmeli davranışı desenleri**

Bölümlenmiş araç sinyalleri ve tanılama veri FuelEfficientDrivingPatternPipeline aşağıdaki iş akışında gösterildiği gibi işlenir. Hive modelleri, konum, araç, yönlendirmeli koşullar ve fuel-efficient yönlendirmeli desenleri sergilemesine diğer özelliklerini belirlemek için kullanılır.

![Fuel-Efficient yönlendirmeli desenleri](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19-vehicle-telematics-fuel-efficient-driving-pattern.png) 

***Fuel-Efficient yönlendirmeli düzeni Hive sorgusu***

Hive betiği fuelefficientdriving.hql fuel-efficient yönlendirmeli koşul modelleri analiz etmek için kullanılır. İndirilen ZIP dosyasının \demo\src\connectedcar\scripts klasöründe bulunur. 

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


Komut dosyasını bir araç 's iletim dişli konumu bileşimini kullanır, Fren pedal durumu, hızlı ve Hızlandırıcı braking, hızlandırmasını, temel fuel-efficient yönlendirmeli davranışlarını algılamak için konumunu pedal ve desenler hızı. 

Ardışık Düzen başarıyla yürütüldükten sonra depolama hesabınızda connectedcar kapsayıcısı altında oluşturulur şu bölümlerde bakın:

![FuelEfficientDrivingPatternPipeline çıkış](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig20-vehicle-telematics-fuel-efficient-driving-pattern-output.png) 

**Model tahminleri geri çağırma**

Deneme öğrenme makine sağlandığında ve çözüm dağıtımının bir parçası olarak bir web hizmeti olarak yayımlanır. Toplu Puanlama uç noktası bu iş akışındaki kullanılır. Bu data factory bağlantılı hizmet olarak kayıtlı olup Puanlama etkinliği veri fabrikası toplu kullanarak kullanıma hazır hale getirilmiş.

![Machine learning uç noktası](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig21-vehicle-telematics-machine-learning-endpoint.png) 

Kayıtlı bağlantılı hizmet DetectAnomalyPipeline içinde anomali algılama modelini kullanarak verileri Puanlama amacıyla kullanılır. 

![Machine learning toplu veri fabrikasında Puanlama etkinliği](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig22-vehicle-telematics-aml-batch-scoring.png)  

Web hizmeti Puanlama toplu işlemle operationalized böylece bu ardışık düzendeki veri hazırlığı için birkaç adım gerçekleştirilir. 

![Geri çağırma tahmin DetectAnomalyPipeline](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig23-vehicle-telematics-pipeline-predicting-recalls.png)  

***Anomali algılama Hive sorgusu***

Puanlama tamamlandıktan sonra bir Hdınsight etkinliği işler ve anormallikleri modelin kategorize verileri toplar. Model 0.60 veya daha yüksek bir olasılık puan kullanır.

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


Ardışık Düzen başarıyla yürütüldükten sonra depolama hesabınızda connectedcar kapsayıcısı altında oluşturulur şu bölümlerde bakın:

![DetectAnomalyPipeline çıkış](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig24-vehicle-telematics-detect-anamoly-pipeline-output.png) 

## <a name="publish"></a>Yayımlama

### <a name="real-time-analysis"></a>Gerçek zamanlı analiz
Stream Analytics işi sorgularda birini olayları çıkış olay hub'ı örneğine yayımlar. 

![Akış analizi işine çıkış olay hub'ı örneğine yayımlanan](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig25-vehicle-telematics-stream-analytics-job-publishes-output-event-hub.png)

Aşağıdaki akış analizi sorgu çıkış olay hub'ı örneğine yayımlamak için kullanılır:

![Stream Analytics sorgu çıkış olay hub'ı örneğine yayımlamak için](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig26-vehicle-telematics-stream-analytics-query-publish-output-event-hub.png)

Bu akış olayların çözümde bulunan RealTimeDashboardApp tarafından kullanılır. Bu uygulama, machine learning gerçek zamanlı Puanlama istek-yanıt web hizmetini kullanır. Sonuç veri tüketim için bir Power BI veri kümesi yayımlar. 

### <a name="batch-analysis"></a>Toplu iş analiz
Toplu işlem ve gerçek zamanlı işleme sonuçları Azure SQL veritabanı tabloları tüketimi için yayımlanır. SQL server, veritabanı ve tablo Kurulum komut dosyasının bir parçası otomatik olarak oluşturulur. 

Toplu işleme sonuçlarını veri reyonu akışına kopyalanır.

![Toplu veri reyonu akışına kopyalanan sonuçlarını işleme](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig27-vehicle-telematics-batch-processing-results-copy-to-data-mart.png)

Stream Analytics işi veri reyonuna yayımlanır.

![Akış analizi işi veri reyonuna yayımlanan](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig28-vehicle-telematics-stream-analytics-job-publishes-to-data-mart.png)

Veri reyonu Stream Analytics işinde ayardır.

![Veri reyonu ayarında Stream Analytics işi](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig29-vehicle-telematics-data-mart-setting-in-stream-analytics-job.png)

## <a name="consume"></a>Tüketme
Power BI Bu çözüm gerçek zamanlı veri ve Tahmine dayalı analiz görselleştirmeleri için zengin bir Pano sağlar. 

Son Panosu, aşağıdaki örnekte olduğu gibi görünür:

![Power BI Panosu](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig30-vehicle-telematics-powerbi-dashboard.png)

## <a name="summary"></a>Özet
Bu belgede ayrıntılı ayrıntıya araç Telemetri analizi çözümün içerir. Lambda mimarisi düzeni için kullanılan gerçek zamanlı ve toplu analytics Öngörüler ve Eylemler ile. Çeşitli etkin yolunuzda (gerçek zamanlı) gerektiren kullanım durumları için bu deseni uygular ve yolunuzda (toplu) analytics. 

### <a name="references"></a>Başvurular

* [Araç telematik Simulator Visual Studio çözümü](http://go.microsoft.com/fwlink/?LinkId=717075) 
* [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/)
* [Azure Data Factory](https://azure.microsoft.com/documentation/learning-paths/data-factory/)
* [Akış alımı için Azure olay hub'ları SDK](../../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
* [Azure Data Factory veri taşıma özellikleri](../../data-factory/v1/data-factory-data-movement-activities.md)
* [Azure veri fabrikası .NET etkinliği](../../data-factory/v1/data-factory-use-custom-activities.md)
* [Azure veri fabrikası .NET etkinlik örnek verileri hazırlamak için kullanılan Visual Studio çözümü](http://go.microsoft.com/fwlink/?LinkId=717077) 
