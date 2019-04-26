---
title: Azure HDInsight - Team Data Science Process Spark kullanan veri bilimi
description: Önemli makine öğrenimi modelleme özellikleri dağıtılmış HDInsight ortamına Spark MLlib Araç Seti sunar.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/13/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 932587afcffcb3b1a259a02a98c648e938e99931
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60256371"
---
# <a name="overview-of-data-science-using-spark-on-azure-hdinsight"></a>Azure HDInsight üzerinde Spark kullanan veri bilimi genel bakış

Bu paketi konuları veri alımı, özellik Mühendisliği, modelleme ve model değerlendirme gibi genel veri bilimi görevlerini tamamlamak için HDInsight Spark kullanmayı gösterir. Kullanılan veri kümesinin 2013 NYC taksi seyahat ve taksi bir örnektir. Mantıksal ve doğrusal regresyon, rastgele ormanları ve gradyan artırmalı ağaçları modellerinin içerir. Konular, Azure blob depolama (WASB) şu modelleri depolamak ve puan ve Tahmine dayalı performanslarını değerlendirmek de gösterir. Daha gelişmiş konular ele modelleri nasıl olabileceğini çapraz doğrulama ve hiper parametreli Süpürme kullanarak eğitilir. Bu genel bakış konusu, ayrıca sağlanan izlenecek adımları tamamlamak için gereken bir Spark kümesi oluşturma nasıl açıklayan konulara başvuruyor.

## <a name="spark-and-mllib"></a>Spark ve MLlib
[Spark](https://spark.apache.org/) büyük veri analizi uygulamalarının performansını artırmak üzere bellek içi destekleyen açık kaynaklı bir paralel işleme altyapısıdır işleme olduğu. Spark işleme altyapısı hız, kullanım kolaylığı ve Gelişmiş analiz için oluşturulmuştur. Spark'ın dağıtılmış bellek içi hesaplama özellikleri onu kullanılan makine öğrenimi ve grafik hesaplamalarında yinelemeli algoritmalar için iyi bir seçim haline getirir. [MLlib](https://spark.apache.org/mllib/) algoritmik getiren Spark'ın ölçeklenebilir makine öğrenimi kitaplığı modelleme dağıtılmış bu ortam özellikleri.

## <a name="hdinsight-spark"></a>HDInsight Spark
[HDInsight Spark](../../hdinsight/spark/apache-spark-overview.md) Azure barındırılan açık kaynaklı Spark'ın bir tekliftir. İçin destek de içerir **Jupyter PySpark not defterleri** Spark kümesinde dönüştürme, filtreleme ve Azure BLOB'ları (WASB) içinde depolanan verileri görselleştiren Spark SQL etkileşimli sorguları çalıştırabilirsiniz. PySpark Spark için Python API'dir. Çözümler ve burada yüklü üzerinde Spark kümeleri Jupyter not defterlerini çalıştırmak verileri görselleştirmek için ilgili çizimleri Göster kod parçacıkları. Bu konu başlıklarındaki modelleme adımları eğitmek, değerlendirmek, kaydetme ve her bir türü modelin kullanma işlemi gösterilmektedir kodunu içerir.

## <a name="setup-spark-clusters-and-jupyter-notebooks"></a>Kurulum: Spark kümelerine ve Jupyter Not Defterleri
Bu izlenecek yolda, kurulum adımları ve kod kullanarak bir HDInsight Spark 1.6 için sağlanır. Ancak Jupyter not defterleri, kümeler, HDInsight Spark 1.6 hem de Spark 2.0 için sağlanır. Not defterlerini ve bağlantıları onlara bir açıklamasını sağlanan [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) bunları içeren GitHub deposu. Ayrıca, kodu buraya bağlı not defterlerinde geneldir ve herhangi bir Spark kümesi üzerinde çalışması gerekir. HDInsight Spark kullanmıyorsanız, küme kurulum ve yönetim adımları ne burada gösterilenden biraz farklı olabilir. Kolaylık olması için Jupyter not defterlerini Spark 1.6'ı (Jupyter Notebook sunucusu pySpark Çekirdeği'nde çalışacak şekilde) ve (Jupyter Notebook sunucusu pySpark3 çekirdek içinde çalışacak şekilde) Spark 2.0 için bağlantılar şunlardır:

### <a name="spark-16-notebooks"></a>Spark 1.6 Not Defterleri
Bu not defterlerini Jupyter notebook sunucusu pySpark Çekirdeği'nde çalıştırılması üzeresiniz.

- [pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb): Veri keşfi, modelleme ve birkaç farklı algoritma ile Puanlama gerçekleştirme hakkında bilgi sağlar.
- [pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): Not Defteri #1 ve hiper parametre ayarı ve çapraz doğrulama kullanarak model geliştirme konuları içerir.
- [pySpark-machine-learning-data-science-spark-model-consumption.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-model-consumption.ipynb): Python kullanarak HDInsight kümelerinde kaydedilmiş bir modeli kullanıma hazır hale getirme işlemi gösterilmektedir.

### <a name="spark-20-notebooks"></a>Spark 2.0 Not Defterleri
Bu not defterlerini Jupyter notebook sunucusu pySpark3 Çekirdeği'nde çalıştırılması üzeresiniz.

- [Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): Bu dosya, veri keşfi, modelleme, gerçekleştirme konusunda bilgi sağlar ve Spark 2.0 Puanlama NYC taksi seyahat ve açıklanan taksi verileri kümesi kullanarak kümelerini [burada](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-spark-overview#the-nyc-2013-taxi-data). Bu not defteri hızla Spark 2.0 için sağladık Kodu Keşfetme için iyi bir başlangıç noktası olabilir. Daha ayrıntılı bir not defteri NYC taksi verileri analiz eder, bu listedeki sonraki not bakın. Bu not defterlerini karşılaştırma bu listeye aşağıdaki notlara bakın.
- [Spark2.0-pySpark3_NYC_Taxi_Tip_Regression.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0_pySpark3_NYC_Taxi_Tip_Regression.ipynb): Bu dosya denetimi veri (işlem), Spark SQL ve veri çerçevesi model ve puanlama NYC taksi seyahat ve açıklanan taksi verileri kümesi kullanarak keşif gerçekleştirmeyi gösterir [burada](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-spark-overview#the-nyc-2013-taxi-data).
- [Spark2.0-pySpark3_Airline_Departure_Delay_Classification.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0_pySpark3_Airline_Departure_Delay_Classification.ipynb): Bu dosya, denetimi veri (işlem), Spark SQL ve veri keşfi, modelleme ve iyi bilinen Havayolu zamanında kalkış veri kümesinden 2011 ve 2012 kullanarak Puanlama yapma işlemi açıklanır. Bu hava durumu özellikleri modele dahil edilebilecek şekilde (örneğin, windspeed, sıcaklık, yükseklik vb.) havaalanı hava durumu verilerini Havayolu kümesiyle modelleme önce tümleştirdik.

<!-- -->

> [!NOTE]
> Havayolu veri kümesini sınıflandırma algoritmalarının kullanımını daha iyi anlamak için Spark 2.0 not defterleri için eklendi. Aşağıdaki bağlantıları kalkış veri kümesi ve hava durumu dataset zamanında Havayolu hakkında bilgi için bkz.
> 
> - Havayolu zamanında kalkış verileri: [https://www.transtats.bts.gov/ONTIME/](https://www.transtats.bts.gov/ONTIME/)
> 
> - Havaalanı hava durumu verileri: [https://www.ncdc.noaa.gov/](https://www.ncdc.noaa.gov/)

<!-- -->

<!-- -->

> [!NOTE]
> Spark 2.0 not defterleri ile ilgili NYC taksi ve Havayolu uçuş gecikme veri kümeleri, 10 dakika veya (HDI kümenizin boyutuna bağlı olarak) çalıştırmak için daha fazla sürebilir. Yukarıdaki listede ilk not defterini veri keşfi, Görselleştirme ve ML model eğitiminin birçok yönden alt örneklenen NYC veri taksi ve taksi dosyalarını önceden birleştirilmiş silinmiş kümesi ile çalıştırmak için daha az zaman alan bir not defteri gösterir: [Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb). Bu Not Defteri (2-3 dakika) tamamlanması daha kısa bir zaman alır ve iyi bir kolayca Spark 2.0 için sağladık Kodu Keşfetme noktası başlatılıyor olabilir.

<!-- -->

Spark 2.0 model ve puanlama için tüketim modeli kullanıma hazır hale getirme konusunda kılavuz bilgiler için bkz. [Spark 1.6 belge tüketimi](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-model-consumption.ipynb) gerekli adımları anahat oluşturma örneği. Bu, Spark 2.0 kullanmak için Python kodu dosyayla değiştirin [bu dosyayı](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Python/Spark2.0_ConsumeRFCV_NYCReg.py).

### <a name="prerequisites"></a>Önkoşullar

Spark 1.6 için aşağıdaki yordamları ilgilidir. Spark 2.0 sürümüne için açıklanan ve için daha önce bağlı not defterlerini kullanma.

1. Bir Azure aboneliğiniz olmalıdır. Zaten bir yoksa, bkz. [alma Azure ücretsiz deneme sürümü](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

2. Bu izlenecek yolu tamamlamak için bir Spark 1.6 kümesine ihtiyacınız vardır. Bölümündeki yönergeleri oluşturmak için bkz [Başlarken: Azure HDInsight üzerinde Apache Spark oluşturma](../../hdinsight/spark/apache-spark-jupyter-spark-sql.md). Küme türü ve sürümü gelen belirtildiğinden **küme türü seçin** menüsü.

![Küme yapılandırma](./media/spark-overview/spark-cluster-on-portal.png)

<!-- -->

> [!NOTE]
> Python yerine Scala uçtan uca veri bilimi işlemi için görevleri tamamlamak için nasıl kullanılacağını gösteren bir konu için bkz. [azure'da Spark Scala kullanarak veri bilimi](scala-walkthrough.md).
>
>

<!-- -->

> [!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]
>
>

## <a name="the-nyc-2013-taxi-data"></a>NYC 2013 taksi verileri
NYC taksi seyahat verilerini yaklaşık 20 GB sıkıştırılmış virgülle ayrılmış değerler (CSV) dosyaları (~ 48 sıkıştırılmamış GB), her seyahat için ücretli 173 milyondan fazla bireysel gelişlerin ve fares. Her bir seyahat kayıt gönderildikten dropoff konumu ve zaman, anonimleştirilmiş hack (sürücü) lisans numarası ve medallion (taksi'nın benzersiz tanımlayıcı) numarasını içerir. Veriler tüm dönüş 2013 yılında kapsar ve aşağıdaki iki veri kümesi için her ay sağlanır:

1. 'Trip_data' CSV dosyaları öğrenilip Yolcuların Sayısı gibi seyahat ayrıntıları içerir ve süresi ve seyahat uzunluğu dropoff işaret geçirmek. Birkaç örnek kayıt şunlardır:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
2. 'Trip_fare' CSV dosyaları için ödeme türü, taksi tutar, ek ücret ve vergiler, ipuçları ve Ücretli geçişler, gibi her seyahat Ücretli taksi ve ödenen toplam tutarı ayrıntılarını içerir. Birkaç örnek kayıt şunlardır:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Biz bu dosyaların bir %0,1 örneği ve seyahat katılmış\_veri ve seyahat\_masrafları CVS dosyaları halinde bu kılavuz için giriş veri kümesi olarak kullanmak için tek bir veri kümesi. Seyahat katılmak için benzersiz anahtar\_veri ve seyahat\_taksi alanlarını oluşur: medallion hack\_lisans ve alma\_datetime. Her kayıt veri kümesinin bir NYC taksi seyahatini temsil eden aşağıdaki öznitelikleri içerir:

| Alan | Kısa açıklama |
| --- | --- |
| medallion |Anonimleştirilmiş taksi medallion (benzersiz taksi kimliği) |
| hack_license |Anonimleştirilmiş Hackney satır başı Ehliyet numarası |
| vendor_id |Taksi satıcı kimliği |
| rate_code |Taksi NYC taksi oranı |
| store_and_fwd_flag |Store ve iletme bayrağı |
| pickup_datetime |Tarih ve saati seçin |
| dropoff_datetime |Dropoff tarih ve saat |
| pickup_hour |Saati seçin |
| pickup_week |Haftanın günü seçin |
| Haftanın günü |Haftanın günü (aralık 1-7) |
| passenger_count |Taksi seyahat içinde Yolcuların Sayısı |
| trip_time_in_secs |Seyahat süresini saniye cinsinden |
| trip_distance |Mili imkansız seyahat uzaklık |
| pickup_longitude |Boylam seçin |
| pickup_latitude |Enlem seçin |
| dropoff_longitude |Dropoff boylam |
| dropoff_latitude |Dropoff enlem |
| direct_distance |Doğrudan Seçim arasındaki uzaklık yukarı ve dropoff konumları |
| payment_type |Ödeme türü (nakit, kredi kartı vb.) |
| fare_amount |Taksi tutar |
| Ek maliyeti |Ek maliyeti |
| mta_tax |MTA vergi |
| tip_amount |İpucu tutar |
| tolls_amount |Ücretli geçişler tutar |
| total_amount |Toplam tutar |
| Eğimli |Eğimli (0/1 Hayır için Evet veya) |
| tip_class |Sınıf İpucu (0: 0, 1: $0-5, 2: $6-10, 3: $11-20, 4: > $20) |

## <a name="execute-code-from-a-jupyter-notebook-on-the-spark-cluster"></a>Spark kümesinde Jupyter not defteri gelen kod yürütün
Jupyter not defterine Azure portalından başlatabilirsiniz. Spark kümenizde panonuzu bulun ve kümeniz için yönetim sayfası girmek için tıklatın. Not Defteri kullanarak Spark kümesi ile ilişkili'ni açmak için **küme panoları** -> **Jupyter not defteri** .

![Küme panoları](./media/spark-overview/spark-jupyter-on-portal.png)

De göz atabilirsiniz ***https://CLUSTERNAME.azurehdinsight.net/jupyter*** Jupyter not defterlerini erişmek için. CLUSTERNAME bu URL'nin bir parçası, kendi kümenizin adıyla değiştirin. Not defterlerini erişmek, yönetici hesabı için parola gerekir.

![Jupyter not defterleri Gözat](./media/spark-overview/spark-jupyter-notebook.png)

PySpark API kullanan önceden paketlenmiş not defterlerini birkaç örneklerini içeren bir dizine görmek için PySpark seçin. Not defterlerini Spark konunun bu paket için kod örnekleri içeren kullanılabilir [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark)

Doğrudan not defterlerini karşıya yüklediğiniz [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark) Spark kümeniz üzerindeki Jupyter notebook sunucusu için. Giriş sayfasında, Jupyter, tıklayın **karşıya** ekranın sağ tarafında düğmesi. Dosya Gezgini'ni açar. Not Defteri ve tıklayın (ham içeriği) GitHub URL'sini buraya yapıştırın **açık**.

Dosya adı ile Jupyter dosya listenizdeki gördüğünüz bir **karşıya** düğmesini tekrar. Burayı tıklatın **karşıya** düğmesi. Şimdi bir not defteri aldınız. Bu izlenecek yolda diğer defterlerinden karşıya yüklemek için bu adımları yineleyin.

> [!TIP]
> Seçin ve tarayıcı bağlantılara sağ tıklayabilirsiniz **bağlantıyı Kopyala** GitHub Ham içerik URL'sini alabilirsiniz. Bu URL'yi Jupyter karşıya dosya Gezgini iletişim kutusuna yapıştırabilirsiniz.
> 
> 

Artık şunları yapabilirsiniz:

* Not defterini tıklayarak koduna bakın.
* Her hücre tuşlarına basarak yürütme **SHIFT girin**.
* Tıklayarak tüm not defterlerini çalıştırmak **hücre** -> **çalıştırma**.
* Sorgu otomatik görselleştirme kullanın.

> [!TIP]
> PySpark Çekirdeği (HiveQL) SQL sorguları çıktısını otomatik olarak görselleştirir. Kullanılarak birkaç farklı türde (tablo, pasta, çizgi, alan veya çubuk) görselleştirmeler arasında tercih yapma seçeneğine verilir **türü** düğmeleri Not:
>
>

![Genel bir yaklaşım için Lojistik regresyon ROC eğrisi](./media/spark-overview/pyspark-jupyter-autovisualization.png)

## <a name="whats-next"></a>Sırada ne var?
Bir HDInsight Spark kümesi ile ayarlanır ve Jupyter not defterlerini karşıya yüklediğiniz göre bu üç PySpark not defterleri için karşılık gelen konuları ile çalışmaya hazır olursunuz. Bunlar, nasıl oluşturarak verilerinizi araştırmanıza ve nasıl oluşturulup tüketim modelleri görüntüleyin. Gelişmiş Veri keşfi ve modelleme Not Defteri, çapraz doğrulama, hyper-Süpürme saldırısı yapılabilir, parametre eklemek ve değerlendirme modeli gösterilmektedir.

**Veri keşfi ve modelleme ile Spark:** Veri kümesini araştırmak ve oluşturma, Puanlama ve makine öğrenimi modellerini aracılığıyla değerlendirmek [Spark MLlib araç seti ile veriler için ikili sınıflandırma ve regresyon modellerini oluşturma](spark-data-exploration-modeling.md) konu.

**Model tüketimi:** Bu konu başlığında oluşturduğunuz sınıflandırma ve regresyon modellerini puanlamanıza öğrenmek için bkz. [puanı ve Spark'a yerleşik machine learning modellerini değerlendirme](spark-model-consumption.md).

**Çapraz doğrulama ve hiper parametre Süpürme**: Bkz: [Gelişmiş Veri keşfi ve modelleme Spark ile](spark-advanced-data-exploration-modeling.md) modelleri nasıl olabileceğini üzerinde çapraz doğrulama ve hiper parametreli Süpürme kullanarak eğitim

