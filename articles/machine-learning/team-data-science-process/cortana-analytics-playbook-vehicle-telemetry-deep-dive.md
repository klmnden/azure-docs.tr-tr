---
title: Yakından araç durumu ve sürüş tahmin etmek nasıl yararlanabileceğini - Team Data Science Process
description: Her araç durumu gerçek zamanlı ve Tahmine dayalı Öngörüler elde etme ve sürüş alışkanlıkları için bir çözüm aşamalarını detayına gidin.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.component: team-data-science-process
ms.topic: article
ms.date: 03/14/2018
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: b8bd0286a2fa800dd2d5b8ac9c4df1a29809f7f9
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53142045"
---
# <a name="vehicle-telemetry-analytics-solution-playbook-deep-dive-into-the-solution"></a>Araç Telemetri analizi çözüm kitabı: çözümün içine yakından

Her çözüm mimarisinde gösterilen aşamalardan bu makalede tatbikatları aşağı. Yönergeler ve özelleştirme için işaretçiler dahil edilir. 

Bu çözüm özeti açıklamasını gözden geçirmek için bkz. [araç Telemetri analizi çözüm kitabı](cortana-analytics-playbook-vehicle-telemetry.md).


## <a name="data-sources"></a>Veri kaynakları
Çözüm iki farklı veri kaynaklarını kullanmaktadır:

* Simülasyon araç sinyaller ve tanılama veri kümesi
* Araç Kataloğu

Bir araç telematik simülatörü, aşağıdaki ekran görüntüsünde gösterildiği gibi bu çözüm, bir parçası olarak dahil edilir. Tanılama bilgileri ve araç durumu ve sürüş düzeni belirli bir noktada sürede karşılık gelen sinyalleri yayar.  Araç katalog araç kimlik numaraları (VINs) modeller için eşlenen başvuru veri kümesi içerir. Not: Araç telematik simülatörü Visual Studio çözümünü veri kümesi artık kullanılamıyor. 

![Araç telematik simülatörü](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig1-vehicle-telematics-simulator.png)


Bu JSON ile biçimlendirilmiş bir veri kümesi, aşağıdaki şema içeriyor.

| Sütun | Açıklama | Değerler |
| --- | --- | --- |
| TOPLAMIDIR |Rastgele oluşturulmuş Toplamıdır |10.000 rastgele oluşturulmuş VINs ana listesinden elde |
| Dış sıcaklığı |Burada araç önünü açıyor dış sıcaklığı |0 ile 100 rastgele oluşturulmuş sayı |
| Altyapısı sıcaklık |Araç altyapısı Sıcaklığın |0 rastgele oluşturulmuş bir sayıya 500 |
| Hız |Hangi aracın yönlendirdiğini altyapısı hız |0 ile 100 rastgele oluşturulmuş sayı |
| Yakıt |Araç yakıt düzeyi |Rastgele oluşturulmuş sayı 0 ile 100 (yakıt düzeyi yüzdesini gösterir) |
| EngineOil |Araç altyapısı Petrol düzeyi |Rastgele oluşturulmuş sayı 0 ile 100 (altyapısı Petrol düzeyi yüzdesini gösterir) |
| Lastik baskısı |Araç, Lastik baskısı |Rastgele oluşturulmuş sayı 0 ile 50 (Lastik baskısı düzeyi yüzdesini gösterir) |
| Kilometre Sayacı |Araç, Kilometre Sayacı okuma |Rastgele oluşturulmuş sayıya 200.000 0 |
| Accelerator_pedal_position |Araç Hızlandırıcı pedal konumu |Rastgele oluşturulmuş sayı 0 ile 100 (Hızlandırıcı düzeyi yüzdesini gösterir) |
| Parking_brake_status |Araç veya yerleşmiş durumdayken gösterir |TRUE veya False |
| Headlamp_status |Ön ışık üzerinde olup olmadığını gösterir |TRUE veya False |
| Brake_pedal_status |Hızınızı pedal veya basıldığını bildirir |TRUE veya False |
| Transmission_gear_position |Araç aktarım dişli konumu |Durumları: ilk olarak, üçüncü, dördüncü, beşinci, altıncı seventh, sekizinci ikinci |
| Ignition_status |Araç çalışıyor veya durduruldu olduğunu gösterir |TRUE veya False |
| Windshield_wiper_status |Ön wiper veya açık olup olmadığını gösterir |TRUE veya False |
| ABS |ABS veya bağlı olup olmadığını gösterir |TRUE veya False |
| Zaman damgası |Veri noktasının oluşturulduğu zaman damgası |Tarih |
| Şehir |Araç konumu |Bu çözümdeki dört şehirler: Bellevue, Redmond, Sammamish, Seattle |

Araç modeli başvuru veri kümesi için modelleri VINs eşler. 

| TOPLAMIDIR | Model |
| --- | --- |
| FHL3O1SA4IEHB4WU1 |Sedan |
| 8J0U8XCPRGW4Z3NQE |Hibrit |
| WORG68Z2PLTNZDBI7 |Aile saloon |
| JTHMYHQTEPP4WBMRN |Sedan |
| W9FTHG27LZN1YWO0Y |Hibrit |
| MHTP9N792PHK08WJM |Aile saloon |
| EI4QXI2AXVQQING4I |Sedan |
| 5KKR2VB4WHQH97PF8 |Hibrit |
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
| M4TV6IEALD5QDS3IR |Hibrit |
| VHRA1Y2TGTA84F00H |Aile saloon |
| R0JAUHT1L1R3BIKI0 |Sedan |
| 9230C202Z60XX84AU |Hibrit |
| T8DNDN5UDCWL7M72H |Aile saloon |
| 4WPYRUZII5YV7YA42 |Sedan |
| D1ZVY26UV2BFGHZNO |Hibrit |
| XUF99EW9OIQOMV7Q7 |Aile saloon |
| 8OMCL3LGI7XNCC21U |Dönüştürülebilir |
| ……. | |

## <a name="ingestion"></a>Alma
Azure Event Hubs, Azure Stream Analytics ve Azure Data Factory birleşimlerini vehicle sinyalleri, tanılama olaylarını almak için kullanılır ve gerçek zamanlı ve toplu analiz. Tüm bu bileşenlerin oluşturulur ve çözüm dağıtımının bir parçası yapılandırılmış. 

### <a name="real-time-analysis"></a>Gerçek zamanlı analiz
Araç telematik simülatörü tarafından oluşturulan olaylar, olay hub'ına olay hub'ı SDK'sını kullanarak yayımlanır.  

![Olay hub'ı Panosu](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig4-vehicle-telematics-event-hub-dashboard.png) 

Stream Analytics işi, bu olayları olay hub'ı alır ve gerçek zamanlı olarak araç durumu çözümlemek için verileri işler.

![Stream Analytics işi verileri işleme](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig5-vehicle-telematics-stream-analytics-job-processing-data.png) 


Stream Analytics işi:

* Olay hub'ından veri alır.
* ' % S'araç Toplamıdır karşılık gelen modeline eşlemek için başvuru verileriyle birleştirme gerçekleştirir. 
* Bunları zengin toplu analiz için Azure Blob depolamaya devam ettirir. 

Aşağıdaki Stream Analytics sorgusu, Blob depolama alanına verileri kalıcı hale getirmek için kullanılır: 

![Stream Analytics işi sorgusu için veri alma](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig6-vehicle-telematics-stream-analytics-job-query-for-data-ingestion.png) 


### <a name="batch-analysis"></a>Batch analizi
Ek bir birim simülasyon araç sinyaller ve tanılama veri kümesi, ayrıca daha ayrıntılı toplu analizler için oluşturulur. Bu ek bir birim, toplu işlem için bir iyi temsili veri hacmi emin olmak için gereklidir. Bu amaçla PrepareSampleDataPipeline Data Factory iş akışında bir yılın değerinde simülasyon araç sinyaller ve tanılama veri kümesi oluşturmak için kullanılır. Data Factory özel bir .NET etkinliği gereksinimlerinize göre özelleştirme için Visual Studio çözümü indirmek için Git [Data Factory özel etkinlik](https://go.microsoft.com/fwlink/?LinkId=717077) Web sayfası. 

Bu iş akışı, toplu işlem için hazır örnek veriler gösterilmektedir.

![Örnek verileri toplu işleme iş akışı için hazır](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig7-vehicle-telematics-prepare-sample-data-for-batch-processing.png) 


Özel bir Data Factory .NET etkinliğini işlem hattı oluşur.

![PrepareSampleDataPipeline etkinliği](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig8-vehicle-telematics-prepare-sample-data-pipeline.png) 

İşlem hattı başarılı bir şekilde yürütür ve RawCarEventsTable veri kümesi "Hazır" olarak işaretlenmiş sonra bir yılın değerinde simülasyon araç sinyaller ve Tanılama verileri oluşturulur. Aşağıdaki klasör ve dosya depolama hesabınızda connectedcar kapsayıcısı altında oluşturulan görürsünüz:

![PrepareSampleDataPipeline çıkış](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig9-vehicle-telematics-prepare-sample-data-pipeline-output.png) 

## <a name="partition-the-data-set"></a>Bölüm veri kümesi
Veri hazırlama adımında, ham yarı yapılandırılmış araç sinyaller ve tanılama veri kümesi yıl/ay biçimine bölümlenir. Bu bölümleme daha etkili bir sorgulama ve ölçeklenebilir, uzun vadeli depolama hatası üzerinden etkinleştirerek yükseltir. İlk blob hesabı dolduğunda, örneğin, bu üzerinden sonraki hesabına hataları. 

>[!NOTE] 
>Bu adımda çözümü yalnızca toplu işleme için geçerlidir.

Giriş ve çıkış veri yönetimi:

* **Çıktı verilerini** (etiketli PartitionedCarEventsTable) uzun bir süre için temel / "rawest" form verilerinin müşterinin veri gölü'nde olarak tutulur. 
* **Veri girişi** çıktı verilerini tam uygunlukta girişine sahip olduğundan bu işlem hattı için genellikle atılır. (Daha iyi kullanmak için bölümlenmiş) depolanır.

Bölüm araba olay iş akışı.

![Bölüm araba olay iş akışı](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig10-vehicle-telematics-partition-car-events-workflow.png)


Ham verileri PartitionCarEventsPipeline içinde bir Azure HDInsight Hive etkinliği'ni kullanarak aşağıdaki ekran görüntüsünde gösterildiği gibi bölümlenir. Veri hazırlama adımı bir yıl için oluşturulan örnek veriler yıl/ay temelinde bölümlenir. Bölümler, her bir yılın ayı için (12 bölümün toplamı) vehicle sinyaller ve tanılama verilerini oluşturmak için kullanılır. 

![PartitionCarEventsPipeline etkinliği](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig11-vehicle-telematics-partition-car-events-pipeline.png)


**PartitionConnectedCarEvents Hive betiği**

Hive betiği partitioncarevents.hql bölümleme için kullanılır. Bu işlem, indirilen ZIP dosyasının \demo\src\connectedcar\scripts klasöründe bulunur. 
    
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

İşlem hattı başarıyla yürütüldükten sonra depolama hesabınızda connectedcar kapsayıcısı altında oluşturulan aşağıdaki bölümlere bakın:

![Bölümlenmiş çıkış](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig12-vehicle-telematics-partitioned-output.png)

Daha kolay yönetilebilir ve batch zengin içgörüler edinmek için daha fazla işleme için hazır veriler artık iyileştirilmiştir. 

## <a name="data-analysis"></a>Veri çözümlemesi
Bu bölümde, Stream Analytics, Azure Machine Learning, Data Factory ve HDInsight için Gelişmiş analiz üzerinde araç durumu ve sürüş alışkanlıkları zengin bir araya getirilebileceğini öğrenin bakın.

### <a name="machine-learning"></a>Makine öğrenimi
Buradaki amaç, bakım ya da aşağıdaki varsayımları temel alarak, belirli bir sistem durumu istatistiklerini göre geri çağırma gerektiren taşıtlardan tahmin etmektir:

* Aşağıdaki üç koşulun true ise, Araçlar bakım bakım gerektirir:
  
  * Lastik baskısı düşüktür.
  * Altyapısı Petrol düşük düzeydir.
  * Altyapısı sıcaklık yüksektir.

* Aşağıdaki koşullardan biri doğru ise, Araçlar bir güvenlik sorunu sahip olabileceğiniz ve geri çağırma gerektirir:
  
  * Altyapısı sıcaklık yüksektir, ancak dış sıcaklığı düşüktür.
  * Altyapısı sıcaklık düşüktür, ancak dış sıcaklığı yüksektir.

Önceki gereksinimlerine bağlı olarak, iki ayrı modeli anomalileri algılayın. Araç bakım algılama için bir modeldir ve araç geri çağırma algılaması için bir modeldir. Her iki modelleri, yerleşik asıl bileşende analiz (PCA) algoritması anomali algılama için kullanılır. 

#### <a name="maintenance-detection-model"></a>**Bakım algılama modeli**

Varsa üç göstergelerini--Lastik baskısı, altyapısı Petrol veya altyapısı sıcaklık--kendi ilgili koşulunu karşılayan, bir anomali algılama modeli bakım raporlar. Sonuç olarak, bu üç değişkenin olması dikkate alınması gereken modeli oluşturmaya gerekir. Machine learning'de denemede **kümesindeki sütunları seçme** modülü, bu üç değişkenin ayıklamak için kullanılır. Ardından, PCA tabanlı anomali algılama modülü anomali algılama modeli oluşturmak için kullanılır. 

PCA machine learning'de özellik seçimi, Sınıflandırma ve anomali algılama için uygulanabilir kurulan bir tekniktir. PCA büyük olasılıkla bağıntılı değişkenleri asıl bileşenleri olarak adlandırılan değerleri kümesine içermesi birtakım dönüştürür. Özellikler ve anomalileri daha kolay tanımlamak için küçük boyutlu bir alanı proje verilerini modelleme PCA tabanlı anahtar fikrini sağlamaktır.

Anomali algılayıcısı, algılama modeline her yeni giriş için kendi projeksiyon üzerinde eigenvectors ilk hesaplar. Ardından, normalleştirilmiş reconstruction hata hesaplar. Bu anomali puanı normalleştirilmiş hatadır: hata: arttıkça, fazla anormal örneği. 

Üç boyutlu boşluk nokta koordinatları Lastik baskısı, altyapısı Petrol ve altyapısı sıcaklık tarafından tanımlanan bakım algılama sorunu, her bir kayıt olarak kabul edilir. Bu anormal durumları yakalamak için üç boyutlu boşluk özgün veri iki boyutlu bir alanı proje için PCA kullanılır. Bu nedenle, bileşen PCA içinde kullanmak için parametre sayısı iki olarak ayarlanır. Bu parametre, PCA tabanlı anomali algılama uygulama önemli bir rol oynar. Proje verilerini PCA kullandıktan sonra bu anomalileri daha kolay tanımlanır.

#### <a name="recall-anomaly-detection-model"></a>**Anomali algılama modelini geri çağırma**

Geri çağırma anomali algılama modelinde **kümesindeki sütunları seçme** ve PCA tabanlı anomali algılama modülleri benzer şekilde kullanılır. Özellikle, üç değişkenin--altyapısı sıcaklık, dış ısı ve hız--ayıklanır ilk kullanarak **kümesindeki sütunları seçme** modülü. Hızı değişkeni de altyapısı sıcaklık genellikle hızını eşleştiğinden dahildir. Ardından, PCA tabanlı anomali algılama modülü, üç boyutlu veriler iki boyutlu bir alanı proje için kullanılır. Geri çağırma ölçütleri karşılanır. Araç altyapısı sıcaklık ve dış sıcaklığı olumsuz son derece bağıntılı olan geri çağırma gerektirir. PCA gerçekleştirildikten sonra PCA tabanlı anomali algılama algoritması anomalileri yakalamak için kullanılır. 

Her iki modeli eğitimindeki PCA tabanlı anomali algılama modeli eğitmek için normal veri girdi verisi olarak kullanılır. (Normal veri bakım ya da geri çağırma gerektirmez.) Puanlama denemesinde eğitilen anomali algılama modelini vehicle bakım ya da geri çağırma gerekli olup olmadığını algılamak için kullanılır. 

### <a name="real-time-analysis"></a>Gerçek zamanlı analiz
Aşağıdaki Stream Analytics SQL sorgusunu, tüm önemli vehicle parametreleri ortalamasını almak için kullanılır. Bu parametreleri vehicle hızı, yakıt düzeyi, altyapısı sıcaklık, Kilometre Sayacı okuma, Lastik baskısı, altyapısı Petrol düzeyi ve diğerleri içerir. Ortalamalar, anomalileri algılayın, uyarı vermek ve belirli bir bölgede çalıştırılan Araçlar genel sistem durumu koşullarını belirlemek için kullanılır. Ortalamalar ardından demografik bilgileri için birbiriyle ilişkilendirilir. 

![Stream Analytics sorgu için gerçek zamanlı işleme](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig13-vehicle-telematics-stream-analytics-query-for-real-time-processing.png)

Üç saniye atlayan pencere üzerinde ortalama hesaplanır. Bir atlayan pencere, çakışmayan ve bitişik zaman aralıkları gerekli olduğu için kullanılır. 

Stream Analytics Pencereleme yetenekleri hakkında daha fazla bilgi için bkz. [Pencereleme (Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835019.aspx).

#### <a name="real-time-prediction"></a>**Gerçek zamanlı öngörü**

Bir uygulamayı gerçek zamanlı machine learning modeli hazır hale getirmek için çözümün parçası olarak dahil edilir. RealTimeDashboardApp uygulama oluşturulur ve çözüm dağıtımının bir parçası yapılandırılmış. Uygulama:

* Stream Analytics olayları içindeki bir desenle sürekli olarak yayımladığı olay hub'ı örneği için dinler.

    ![Stream Analytics sorgu verileri yayımlama](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig14-vehicle-telematics-stream-analytics-query-for-publishing.png) 

* Olayları alır. Bu uygulamanın aldığı her olay için: 
   
   * Veriler bir machine learning Puanlama istek yanıt (RRS) uç noktası kullanılarak işlenir. RRS uç nokta, dağıtımın bir parçası otomatik olarak yayımlanır.
   * RRS çıkış push API kullanarak Power BI veri kümesine yayımlanır.

Bu düzen, ayrıca bir satır iş kolu uygulaması gerçek zamanlı analiz akışıyla tümleştirmek istediğiniz senaryolar için geçerlidir. Bu senaryolar, uyarılar, bildirimler ve mesajlaşma içerir.

Not: veri RealtimeDashboardApp Visual Studio çözümü artık kullanılabilir değil.

#### <a name="execute-the-real-time-dashboard-application"></a>**Gerçek zamanlı Pano uygulamayı yürütme**
1. RealtimeDashboardApp ayıklayın ve yerel olarak kaydedin.

    ![RealTimeDashboardApp klasörü](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig16-vehicle-telematics-realtimedashboardapp-folder.png) 

2. Uygulama RealtimeDashboardApp.exe yürütün.

3. Geçerli Power BI kimlik bilgilerinizi girin ve seçin **oturum**.  

    ![Oturum açma gerçek zamanlı Pano uygulama penceresi](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig17a-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png) 
    
4. **Kabul Et**’i seçin.

    ![Gerçek zamanlı Pano uygulama son oturum açma penceresi](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig17b-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png) 

>[!NOTE] 
>Power BI veri kümesi temizlemek isterseniz, RealtimeDashboardApp "flushdata" parametresiyle yürütün. 

    RealtimeDashboardApp.exe -flushdata


### <a name="batch-analysis"></a>Batch analizi
Buradaki amaç, Contoso Motors Azure işlem büyük verilerden yararlanmasına imkan nasıl kullanacağını göstermektir. Bu verileri zengin içgörüler sürüş desenleri, kullanım davranışı ve araç durumu ortaya çıkarır. Bu bilgileri mümkün hale getirir:

* Müşteri deneyimini iyileştirmek ve alışkanlıkları ve fuel-efficient sürüş davranışları artırmaya Öngörüler sunarak ucuz yapın.
* İş kararlarını yöneten ve sınıfının en iyisi ürünleri ve hizmetleri sağlamak için müşterilerin ve sürüş düzenlerini proaktif bir şekilde öğrenin.

Bu çözümde aşağıdaki ölçümleri hedeflenen:

* **Agresif davranışının şekillendirilmesinde**: modelleri, konumları, sürüş koşullar ve yılın agresif sürüş desenleriyle ilgili Öngörüler elde etmek için zaman eğilimi tanımlar. Contoso motorlar bu bilgileri pazarlama kampanyaları için yeni kişiselleştirilmiş özellikler ve kullanım tabanlı sigorta tanıtmak için kullanabilir.
* **Fuel-Efficient sürüş davranışı**: modelleri, konumları, sürüş koşullar ve yılın fuel-efficient sürüş desenleriyle ilgili Öngörüler elde etmek için zaman eğilimi tanımlar. Contoso motorlar bu Öngörüler pazarlama kampanyaları için yeni özellikler ve uygun maliyetli ve ortam dostu sürüş alışkanlıkları sürücülerini proaktif raporlama tanıtmak için kullanabilirsiniz.
* **Modelleri geri çağırma**: anomali algılama machine learning denemesinden faaliyete geçirmeye yönelik geri çekme gerektiren modelleri tanımlar.

Bu ölçüm her ayrıntılarına bakalım.

#### <a name="aggressive-driving-behavior-patterns"></a>**Agresif sürüş davranış kalıplarını**

Bölümlenmiş vehicle sinyaller ve tanılama verilerini AggresiveDrivingPatternPipeline aşağıdaki iş akışında gösterildiği gibi işlenir. Hive modelleri, konum, araç, sürüş koşullar ve agresif sürüş desenleri göstermesi diğer parametreleri belirlemek için kullanılır.

![Agresif sürüş deseni iş akışı](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig18-vehicle-telematics-aggressive-driving-pattern.png) 

***Agresif sürüş deseni Hive sorgusu***

Hive betiği aggresivedriving.hql agresif sürüş koşul desenleri analiz etmek için kullanılır. Bu işlem, indirilen ZIP dosyasının \demo\src\connectedcar\scripts klasöründe bulunur. 

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


Betik, yüksek hızda desenleri braking üzerinde temel reckless/agresif sürüş davranışları algılamak için bir araç 's iletim dişli konumunu, hızınızı pedal durumu ve hızlı birleşimi kullanır. 

İşlem hattı başarıyla yürütüldükten sonra depolama hesabınızda connectedcar kapsayıcısı altında oluşturulan aşağıdaki bölümlere bakın:

![AggressiveDrivingPatternPipeline çıkış](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19-vehicle-telematics-aggressive-driving-pattern-output.png) 


#### <a name="fuel-efficient-driving-behavior-patterns"></a>**Fuel-Efficient sürüş davranış kalıplarını**

Bölümlenmiş vehicle sinyaller ve tanılama verilerini FuelEfficientDrivingPatternPipeline aşağıdaki iş akışında gösterildiği gibi işlenir. Hive modelleri, konum, araç, sürüş koşullar ve fuel-efficient sürüş desenleri göstermesi diğer özelliklerini belirlemek için kullanılır.

![Fuel-Efficient sürüş desenleri](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19-vehicle-telematics-fuel-efficient-driving-pattern.png) 

***Sürüş Fuel-Efficient deseni Hive sorgusu***

Hive betiği fuelefficientdriving.hql fuel-efficient sürüş koşul desenleri analiz etmek için kullanılır. Bu işlem, indirilen ZIP dosyasının \demo\src\connectedcar\scripts klasöründe bulunur. 

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


Araç'ın iletim dişli konumu bileşimi betikte, hızınızı pedal durumu, hız ve Hızlandırıcı braking, hızlandırma üzerinde temel fuel-efficient sürüş davranışları algılamak için konumun pedal ve desenleri hızlandırın. 

İşlem hattı başarıyla yürütüldükten sonra depolama hesabınızda connectedcar kapsayıcısı altında oluşturulan aşağıdaki bölümlere bakın:

![FuelEfficientDrivingPatternPipeline çıkış](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig20-vehicle-telematics-fuel-efficient-driving-pattern-output.png) 

**Model tahminlerini geri çağırma**

Machine learning deneme sağlanabilir ve çözüm dağıtımının parçası olarak bir web hizmeti olarak yayımlandı. Toplu işlem Puanlama uç noktası bu iş akışında kullanılır. Data factory bağlı hizmet olarak kayıtlı ve etkinlik data factory toplu işlem Puanlama kullanarak kullanıma hazır hale getirdiniz.

![Machine learning uç noktası](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig21-vehicle-telematics-machine-learning-endpoint.png) 

Kayıtlı bağlantılı hizmete DetectAnomalyPipeline anomali algılama modeli kullanarak verileri puanlamak için kullanılır. 

![Machine learning toplu iş Puanlama etkinliği, data factory'de](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig22-vehicle-telematics-aml-batch-scoring.png)  

Böylece, toplu işlem Puanlama web hizmeti ile kullanıma hazır hale getirdiniz birkaç adım veri hazırlama için bu işlem hattı gerçekleştirilir. 

![Geri çağırma tahmin için DetectAnomalyPipeline](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig23-vehicle-telematics-pipeline-predicting-recalls.png)  

***Anomali algılama Hive sorgusu***

Puanlama tamamlandıktan sonra HDInsight faaliyet işler ve model anomalileri sınıflandırılmış verileri toplar. Model 0.60 ya da daha yüksek bir olasılık puanı kullanır.

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


İşlem hattı başarıyla yürütüldükten sonra depolama hesabınızda connectedcar kapsayıcısı altında oluşturulan aşağıdaki bölümlere bakın:

![DetectAnomalyPipeline çıkış](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig24-vehicle-telematics-detect-anamoly-pipeline-output.png) 

## <a name="publish"></a>Yayımlama

### <a name="real-time-analysis"></a>Gerçek zamanlı analiz
Stream Analytics işinde birine çıkış olay hub'ı örneği için olayları yayımlar. 

![Stream Analytics işi çıktı olay hub'ı örneği için yayımlanan](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig25-vehicle-telematics-stream-analytics-job-publishes-output-event-hub.png)

Aşağıdaki Stream Analytics sorgusu, çıkış event hub örneğine yeniden yayımlamak için kullanılır:

![Stream Analytics sorgu çıkış event hub örneğine yeniden yayımlamak için](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig26-vehicle-telematics-stream-analytics-query-publish-output-event-hub.png)

Bu akışı çözümde yer alan RealTimeDashboardApp tarafından kullanılır. Bu uygulama, machine learning için gerçek zamanlı Puanlama istek-yanıt web hizmeti kullanır. Bir Power BI veri kümesine tüketim için sonuç verileri yayımlar. 

### <a name="batch-analysis"></a>Batch analizi
Toplu ve gerçek zamanlı işleme sonuçları tüketim için Azure SQL Database tablolarını yayımlanır. SQL server, veritabanı ve tabloları Kurulum betiğini bir parçası olarak otomatik olarak oluşturulur. 

Toplu işleme sonuçları veri reyonu iş akışına kopyalanır.

![Toplu veri reyonu akışına kopyalanan sonuçlarını işleme](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig27-vehicle-telematics-batch-processing-results-copy-to-data-mart.png)

Stream Analytics işi veri reyonuna yayımlanır.

![Yayımlanan veri reyonuna Stream Analytics işi](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig28-vehicle-telematics-stream-analytics-job-publishes-to-data-mart.png)

Veri reyonu Stream Analytics işinde ayarıdır.

![Stream Analytics işinde veri reyonu ayarı](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig29-vehicle-telematics-data-mart-setting-in-stream-analytics-job.png)

## <a name="consume"></a>Tüketme
Power BI bu çözüme gerçek zamanlı verileri ve Tahmine dayalı analiz görselleştirmeleri için zengin bir Pano sunar. 

Son panoyu şu örnekteki gibi görünür:

![Power BI Panosu](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig30-vehicle-telematics-powerbi-dashboard.png)

## <a name="summary"></a>Özet
Bu belgede ayrıntılı araştırıp araç Telemetri analizi çözüm içerir. Lambda mimari yapısı için kullanılan gerçek zamanlı ve toplu analiz tahminler ve Eylemler ile. Bu düzen çok çeşitli sıcak yol (gerçek zamanlı) gerektiren kullanım örnekleri için geçerlidir ve Durgun yol (toplu) analizi. 

### <a name="references"></a>Başvurular

* [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/)
* [Azure Data Factory](https://docs.microsoft.com/rest/api/datafactory/)
* [Akış alımı için Azure Event Hubs SDK](../../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
* [Azure Data Factory veri taşıma özellikleri](../../data-factory/copy-activity-overview.md)
* [Azure Data Factory .NET etkinliği](../../data-factory/transform-data-using-dotnet-custom-activity.md)
* [Azure Data Factory .NET etkinliği örnek verileri hazırlamak için kullanılan Visual Studio çözümü](https://go.microsoft.com/fwlink/?LinkId=717077) 

## <a name="next-steps"></a>Sonraki Adımlar

Power BI raporları ve panolar Bu çözüm için yapılandırma konusunda bilgi için bkz: [araç Telemetri analizi çözüm şablonu Power BI Panosu kurulum yönergeleri](cortana-analytics-playbook-vehicle-telemetry-powerbi.md).
