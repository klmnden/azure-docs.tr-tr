---
title: "Azure Hdınsight'ta Spark kullanarak veri bilimi genel bakış | Microsoft Docs"
description: "Spark Mllib'i Araç Seti modelleme yetenekleri dağıtılmış Hdınsight ortamına önemli makine öğrenimi getirir."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: a4e1de99-a554-4240-9647-2c6d669593c8
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/15/2017
ms.author: deguhath;bradsev;gokuma
ms.openlocfilehash: d9964ace6b59fa65f0f5d4caff28a4291047c8a5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="overview-of-data-science-using-spark-on-azure-hdinsight"></a>Azure Hdınsight'ta Spark kullanarak veri bilimi genel bakış
[!INCLUDE [machine-learning-spark-modeling](../../../includes/machine-learning-spark-modeling.md)]

Bu paketi konuları Hdınsight Spark veri alımı, özellik Mühendisliği, model ve model değerlendirme gibi ortak veri bilimi görevleri tamamlamak için nasıl kullanılacağını gösterir. Kullanılan veri kümesinin 2013 NYC ücreti seyahat ve ücreti bir örnektir. Modellerinin oluşturulması Lojistik ve doğrusal regresyon, rastgele ormanları ve gradyan boosted ağaçları içerir. Konular, Azure blob storage (WASB) Bu modeller depolamak ve puanlama ve Tahmine dayalı kendi performansını değerlendirmek de gösterir. Daha gelişmiş konular ele modelleri nasıl olabilir çapraz doğrulama ve parametre hyper Süpürme kullanılarak eğitilmiş. Bu genel bakış konusunun da sağlanan izlenecek adımları tamamlamak için gereken Spark kümesi ayarlamak nasıl açıklayan konuları başvurur. 

## <a name="spark-and-mllib"></a>Spark ve Mllib'i
[Spark](http://spark.apache.org/) büyük veri analizi uygulamalarının performansını artırmak üzere bellek içi destekleyen bir açık kaynaklı bir paralel işleme altyapısıdır işleme değil. Spark işleme altyapısı hızı, kullanımı kolay, gelişmiş analizler için yerleşik olarak bulunur. Spark'ın bellek içi dağıtılmış hesaplama özellikleri onu kullanılan machine learning ve grafik hesaplamalarında yinelemeli algoritmalar için iyi bir seçimdir haline getirir. [Mllib'i](http://spark.apache.org/mllib/) algoritmik getirir Spark'ın ölçeklenebilir machine learning kitaplığı modelleme yetenekleri Bu dağıtılmış bir ortama. 

## <a name="hdinsight-spark"></a>Hdınsight Spark
[Hdınsight Spark](../../hdinsight/hdinsight-apache-spark-overview.md) açık kaynak Spark Azure barındırılan sunulması değil. İçin destek de içerir **Jupyter PySpark not defterlerini** Spark kümesinde dönüştürme, filtreleme ve Azure BLOB'ları (WASB) içinde depolanan verileri görselleştirmek için Spark SQL etkileşimli sorguları çalıştırabilirsiniz. PySpark Spark Python API'dir. Çözümleri sağlamak ve burada yüklü üzerinde Spark kümeleri Jupyter not defterleri çalıştırmanız verileri görselleştirmek için ilgili çizimleri Göster kod parçacıkları. Bu konularda modelleme adımlarda eğitmek, değerlendirmek, kaydetme ve her türde bir model kullanmasına gösterilmektedir kodu içerir. 

## <a name="setup-spark-clusters-and-jupyter-notebooks"></a>Kurulum: Spark kümeleri ve Jupyter Not Defterleri
Kurulum adımlarını ve kod bu kılavuzda bir Hdınsight Spark 1.6 kullanmak için sağlanır. Ancak Jupyter not defterleri Hdınsight Spark 1.6 ve Spark 2.0 kümeleri için sağlanır. Not defterlerini ve bağlantılarını bir açıklaması verilmiştir [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) bunları içeren GitHub deposunu için. Ayrıca, kodu buraya ve bağlantılı not defterlerini geneldir ve tüm Spark kümesi üzerinde çalışması gerekir. Hdınsight Spark kullanmıyorsanız küme kurulum ve yönetim adımlar ne burada gösterilenden biraz farklı olabilir. Kolaylık olması için Spark (Jupyter not defteri sunucunun pySpark Çekirdeği'nde çalıştırılacak) 1.6 ve Spark 2. 0'ı (Jupyter not defteri sunucunun pySpark3 Çekirdeği'nde çalıştırılacak) için Jupyter not defterleri bağlantılar şunlardır:

### <a name="spark-16-notebooks"></a>Spark 1.6 dizüstü bilgisayarlar
Bu dizüstü bilgisayarlar Jupyter not defteri sunucu pySpark Çekirdeği'nde çalıştırmak üzeresiniz.

- [pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb): modelleme ve birkaç farklı algoritmalarıyla Puanlama veri keşfi, gerçekleştirme hakkında bilgi verilmektedir.
- [pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): Not Defteri #1 ve hyperparameter ayarlama ve çapraz doğrulama kullanarak modeli geliştirme konuları içerir.
- [pySpark-machine-learning-data-science-spark-model-consumption.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-model-consumption.ipynb): Hdınsight kümelerinde Python kullanarak kaydedilmiş modeli faaliyete gösterilmektedir.

### <a name="spark-20-notebooks"></a>Spark 2.0 dizüstü bilgisayarlar
Bu dizüstü bilgisayarlar Jupyter not defteri sunucu pySpark3 Çekirdeği'nde çalıştırmak üzeresiniz.

- [Spark2.0-pySpark3-Machine-Learning-Data-Science-Spark-Advanced-Data-exploration-Modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): Bu dosyayı veri keşfi, modelleme, gerçekleştirmeyle ilgili bilgi sağlar ve Spark 2. 0'Puanlama NYC ücreti seyahat ve açıklanan ücreti veri kümesini kullanarak kümelerini [burada](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-data-science-spark-overview#the-nyc-2013-taxi-data). Bu Not hızlı bir şekilde Spark 2.0 için sağladık kod keşfetme için iyi bir başlangıç noktası olabilir. Daha ayrıntılı bir not defteri NYC ücreti verileri analiz eder, bu listedeki sonraki dizüstü bakın. Bu not defterlerini karşılaştırmak bu liste aşağıdaki notlara bakın. 
- [Spark2.0 pySpark3_NYC_Taxi_Tip_Regression.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0_pySpark3_NYC_Taxi_Tip_Regression.ipynb): Bu dosyayı nasıl wrangling verileri (işlem), Spark SQL ve dataframe modelleme ve NYC ücreti seyahat ve açıklanan ücreti veri kümesini kullanarak Puanlama araştırması gerçekleştirileceğini gösterir [burada](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-data-science-spark-overview#the-nyc-2013-taxi-data).
- [Spark2.0 pySpark3_Airline_Departure_Delay_Classification.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0_pySpark3_Airline_Departure_Delay_Classification.ipynb): Bu dosyayı nasıl wrangling verileri (işlem), Spark SQL ve dataframe modelleme ve bilinen uçak zamanında ayrılma kümesinden 2011 ve 2012 kullanarak Puanlama araştırması gerçekleştirileceğini gösterir. Bu hava durumu özellikleri modele dahil edilebilir biz havaalanı hava durumu verileri (ör. windspeed, sıcaklık, yükseklik vb.) içeren hava yolu veri kümesi modelleme önce tümleşik.

<!-- -->

> [!NOTE]
> Uçak dataset sınıflandırma algoritmalarının kullanımını daha iyi anlamak için Spark 2.0 dizüstü bilgisayarlar için eklenmiştir. Aşağıdaki bağlantılar ayrılma dataset ve hava durumu dataset zamanında hava yolu hakkında bilgi için bkz:

>- Uçak zamanında ayrılma veri: [http://www.transtats.bts.gov/ONTIME/](http://www.transtats.bts.gov/ONTIME/)

>- Havaalanı hava durumu verileri: [https://www.ncdc.noaa.gov/](https://www.ncdc.noaa.gov/) 
> 
> 

<!-- -->

<!-- -->

> [!NOTE]
NYC ücreti ve uçak uçuş gecikme veri kümeleri üzerinde Spark 2.0 not defterlerini 10 dakika veya (HDI kümesi boyutuna bağlı olarak) çalıştırmak için daha fazla sürebilir. Yukarıdaki listede ilk dizüstü veri keşfi pek çok görünüşünün gösterir, Görselleştirme ve ML model eğitim aşağı örneklenen NYC veri içinde ücreti ve ücreti dosyaları önceden birleştirilmiş kümesi ile çalıştırmak için daha az zaman alan bir Not: [Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb) bu not (2-3 dakika) son çok daha kısa sürer ve olması iyi bir başlangıç noktası hızla biz olması koşuluyla Spark 2.0 için kod keşfetme için. 

<!-- -->

Bir Spark 2.0 modeli ve puanlama için model tüketim operationalization hakkında yönergeler için bkz [Spark 1.6 belge tüketimi üzerinde](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-model-consumption.ipynb) gerekli adımlar anahat oluşturma örneği. Bu Spark 2.0 kullanmak için Python kodu dosyasıyla Değiştir [bu dosyayı](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Python/Spark2.0_ConsumeRFCV_NYCReg.py).

### <a name="prerequisites"></a>Ön koşullar
Aşağıdaki yordamlar için Spark 1.6 ilişkilidir. Spark 2.0 sürümü için açıklanan ve için daha önce bağlanan dizüstü bilgisayarları kullanın. 

1. bir Azure aboneliğinizin olması gerekir. Zaten bir yoksa, bkz: [alma Azure ücretsiz deneme sürümü](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

2., bu kılavuzu tamamlamak için bir Spark 1.6 kümesi gerekir. Oluşturmak için sağlanan yönergelere bakın [Başlarken: Azure Hdınsight'ta Apache Spark oluşturma](../../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md). Küme türü ve sürümü belirtilen gelen **küme türü seçin** menüsü. 

![Küme yapılandırma](./media/spark-overview/spark-cluster-on-portal.png)

<!-- -->

> [!NOTE]
> Python yerine Scala uçtan uca veri bilimi işlemi için görevleri tamamlamak için nasıl kullanılacağını gösteren bir konuya bakın [veri bilimi Azure üzerinde Spark Scala kullanarak](scala-walkthrough.md).
> 
> 

<!-- -->

> [!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]
> 
> 

## <a name="the-nyc-2013-taxi-data"></a>NYC 2013 ücreti verileri
NYC ücreti seyahat veri yaklaşık 20 GB sıkıştırılmış virgülle ayrılmış değerler (CSV) dosyaları (~ 48 GB sıkıştırılmamış), her seyahat için ücretli 173 milyondan fazla tek tek dönüşleri ve fares. Her seyahat kaydı yukarı çekme teslim konumu ve zaman, anonim korsan (sürücü) lisans numarası ve medallion (ücreti'nın benzersiz kimliği) numarasını içerir. Veri 2013 yıl içinde tüm dönüşleri kapsayan ve aşağıdaki iki veri kümelerinin her ay sağlanır:

1. 'Trip_data' CSV dosyaları yolcu sayısı gibi seyahat ayrıntıları içerir, almak ve dropoff noktalarını süresi ve seyahat uzunluğu seyahat. Birkaç örnek kayıt şunlardır:
   
        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
2. 'Trip_fare' CSV dosyaları gibi ödeme türü, ücreti tutarı, ek ücret ve vergileri, ipuçları ve tolls, her seyahat için ödenen ücreti ödenen toplam tutar ve ayrıntıları içerir. Birkaç örnek kayıt şunlardır:
   
        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Biz bu dosyaları % 0,1 örneği alınır ve seyahat birleştirilmiş\_veri ve seyahat\_bu kılavuz için giriş veri kümesi kullanmak için tek bir veri kümesi içine CVS dosyaları masrafları. Seyahat katılmak için benzersiz anahtar\_veri ve seyahat\_ücreti alanlarını oluşur: medallion, korsan\_lisans ve alma\_datetime. Her kayıt kümesinin NYC ücreti seyahati temsil eden aşağıdaki öznitelikleri içerir:

| Alan | Kısa açıklama |
| --- | --- |
| medallion |Anonim ücreti medallion (benzersiz ücreti kimliği) |
| hack_license |Anonim Hackney satır lisans numarası |
| vendor_id |Ücreti satıcı kimliği |
| rate_code |Ücreti NYC ücreti oranı |
| store_and_fwd_flag |Depolama ve iletme bayrağı |
| pickup_datetime |Tarih ve saat seçin |
| dropoff_datetime |Dropoff tarih ve saat |
| pickup_hour |Saati seçin |
| pickup_week |Yılın haftası seçin |
| Haftanın günü |Haftanın günü (aralık 1-7) |
| passenger_count |Ücreti seyahat yolcu sayısı |
| trip_time_in_secs |Seyahat süresini saniye cinsinden |
| trip_distance |Mili seyahat seyahat uzaklığı |
| pickup_longitude |Boylam seçin |
| pickup_latitude |Enlem seçin |
| dropoff_longitude |Dropoff boylam |
| dropoff_latitude |Dropoff enlem |
| direct_distance |Yukarı çekme arasındaki uzaklığı doğrudan ve dropoff konumları |
| payment_type |Ödeme türü (CA'lar, kredi kartı vb.) |
| fare_amount |Ücreti tutar |
| Ek ücret |Ek ücret |
| mta_tax |MTA vergi |
| tip_amount |İpucu tutar |
| tolls_amount |Tolls tutar |
| total_amount |Toplam miktarı |
| Eğimli |Eğimli (0/1 Hayır Evet veya) |
| tip_class |Sınıf İpucu (0: 0, 1: $0-5, 2: $6-10, 3: $11-20, 4: > $20) |

## <a name="execute-code-from-a-jupyter-notebook-on-the-spark-cluster"></a>Spark kümesinde Jupyter not defteri gelen kodu yürütme
Azure portalından Jupyter not defteri başlatabilirsiniz. Spark kümenizin Panonuzda bulun ve Yönetim sayfasında, kümeniz için girmek için tıklatın. Spark kümesi ile ilişkili dizüstü bilgisayar'ı açmak için **küme panolarında** -> **Jupyter not defteri** .

![Küme panolarında](./media/spark-overview/spark-jupyter-on-portal.png)

Ayrıca gözatabilirsiniz ***https://CLUSTERNAME.azurehdinsight.net/jupyter*** Jupyter not defterlerini erişmek için. Bu URL CLUSTERNAME parçası kendi küme adıyla değiştirin. Not defterlerini erişmek, yönetici hesabı için parola gerekir.

![Jupyter not defterleri Gözat](./media/spark-overview/spark-jupyter-notebook.png)

PySpark API kullanan önceden paketlenmiş not defterlerini birkaç örnekleri içeren bir dizini görmek için PySpark seçin. Bu paketi Spark konunun için kod örnekleri içeren dizüstü bilgisayarlar kullanılabilir [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark)

Dizüstü bilgisayarlar doğrudan yükleyebilirsiniz [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark) Spark kümesinde Jupyter not defteri sunucuya. Giriş sayfasında, Jupyter birini tıklatın **karşıya** ekranın sağ bölümünün düğmesinde. Dosya Gezgini'ni açar. Buraya tıklayın ve not defteri GitHub (Ham içerik) URL'sini yapıştırabilirsiniz **açık**. 

Dosya adı ile Jupyter dosya listenizdeki gördüğünüz bir **karşıya** yeniden düğmesi. Bu tıklatın **karşıya** düğmesi. Şimdi not defteri aldınız. Bu kılavuzda diğer defterlerinden karşıya yüklemek için bu adımları yineleyin.

> [!TIP]
> Tarayıcı ve seçim bağlantılar sağ **bağlantıyı Kopyala** github Ham içerik URL'sini alabilirsiniz. Jupyter karşıya dosya Gezgini iletişim kutusuna bu URL'yi yapıştırabilirsiniz.
> 
> 

Artık şunları yapabilirsiniz:

* Not Defteri tıklayarak koduna bakın.
* Her bir hücre tuşlarına basarak yürütme **SHIFT-ENTER**.
* Tüm Not tıklayarak çalıştırın **hücre** -> **çalıştırmak**.
* Sorguların otomatik görselleştirme kullanın.

> [!TIP]
> PySpark çekirdeği otomatik olarak (HiveQL) SQL sorguları çıktısını visualizes. Birkaç farklı türde görselleştirmeleri (tablo, pasta, çizgi, alan veya çubuğu) arasında kullanarak seçmek için seçeneği de verilir **türü** menü düğmelerini Not:
> 
> 

![Lojistik regresyon ROC eğrisi genel yaklaşım için](./media/spark-overview/pyspark-jupyter-autovisualization.png)

## <a name="whats-next"></a>Sırada ne var?
Hdınsight Spark kümesinde ile ayarlanır ve Jupyter not defterlerini karşıya yüklediğiniz göre üç PySpark dizüstü bilgisayarlar için karşılık gelen konuları aracılığıyla çalışmaya hazır olursunuz. Bunlar, verilerinizi keşfedin ve ardından oluşturmak ve modelleri kullanmak gösterir. Gelişmiş Veri keşfi ve modelleme Not Defteri, çapraz doğrulama, hyper-yerleştirmez, parametre içerir ve değerlendirme modeli gösterilmektedir. 

**Veri keşfi ve modelleme ile Spark:** dataset keşfetmek ve oluşturmak, Puanlama ve modelleri ile çalışarak öğrenme makine değerlendirmek [Spark Mllib'i araç seti ile veri için ikili sınıflandırma ve regresyon model oluşturma](spark-data-exploration-modeling.md) konu.

**Model tüketimi:** bu konuda oluşturulan sınıflandırma ve regresyon modeli Puanlama öğrenmek için bkz: [puanı ve Spark yerleşik makine öğrenimi modellerini değerlendirme](spark-model-consumption.md).

**Çapraz doğrulama ve hyperparameter Süpürme**: bkz [veri keşfi ve modelleme Spark ile Gelişmiş](spark-advanced-data-exploration-modeling.md) modelleri nasıl olabilir üzerinde çapraz doğrulama ve parametre hyper Süpürme kullanılarak eğitilmiş

