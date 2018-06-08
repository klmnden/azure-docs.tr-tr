---
title: Azure Hdınsight Hadoop kümesi 1 TB veri kümesinde kullanarak eylem - takım veri bilimi işleminde | Microsoft Docs
description: Bir Hdınsight Hadoop kümesi oluşturmak ve bir büyük (1 TB) genel kullanıma açık veri kümesini kullanarak bir model dağıtmak için kullanabileceğiniz bir uçtan uca senaryo için takım veri bilimi işlemi kullanma
services: machine-learning,hdinsight
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: 72d958c4-3205-49b9-ad82-47998d400d2b
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: deguhath
ms.openlocfilehash: 4c368c3f06347b1164731d056a7341bdabb759b4
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "34837353"
---
# <a name="the-team-data-science-process-in-action---using-an-azure-hdinsight-hadoop-cluster-on-a-1-tb-dataset"></a>Azure Hdınsight Hadoop kümesi 1 TB veri kümesinde kullanarak eylem - takım veri bilimi işleminde

Bu kılavuz bir uçtan uca senaryoda takım veri bilimi işlemi kullanımı gösterilmiştir bir [Azure Hdınsight Hadoop kümesi](https://azure.microsoft.com/services/hdinsight/) depolamak için keşfetmek, özellik mühendislik ve aşağı genel kullanıma açık birindenörnekveriler[ Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) veri kümeleri. Bu veriler üzerinde bir ikili sınıflandırma modeli oluşturmak için Azure Machine Learning kullanır. Ayrıca, bu modeller birini bir Web hizmeti olarak yayımlamak üzere nasıl gösterir.

Bu kılavuzda sunulan görevleri gerçekleştirmek için bir IPython dizüstü kullanmak da mümkündür. Bu yaklaşım denemek ister misiniz kullanıcılar başvurun [Criteo izlenecek bir Hive ODBC bağlantısı kullanarak](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) konu.

## <a name="dataset"></a>Criteo Dataset açıklaması
Yaklaşık 370 GB sıkıştırılmış gzip TSV dosyaları (~1.3TB sıkıştırılmamış), bir tıklatın tahmin dataset verilerdir Criteo 4.3 milyardan fazla kayıtları kapsayan. 24 gün gerçekleştirilecek tarafından kullanılabilir duruma verileri tıklatın [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/). Veri bilimcilerine kolaylık olması için bize denemeniz için kullanılabilir veri sıkıştırması kaldırıldı.

Bu veri kümesi her bir kayıtta 40 sütunları içerir:

* ilk kullanıcı olup olmadığını belirten bir etiket sütunu dir bir **ekleme** (değer 1) veya bir (0 değeri) tıklatın değil
* sonraki 13 sütunları sayısal, ve
* Son 26 kategorik sütunlar:

Sütunları gizlidir ve bir dizi numaralandırılmış adları kullanın: için (için etiket sütunu) "Col1" ' Col40 "(son Kategorik bir sütun için).            

Bu veri kümesine ilişkin iki gözlem (satırlar) ilk 20 sütunlarının bir alıntı şöyledir:

    Col1    Col2    Col3    Col4    Col5    Col6    Col7    Col8    Col9    Col10    Col11    Col12    Col13    Col14    Col15            Col16            Col17            Col18            Col19        Col20

    0       40      42      2       54      3       0       0       2       16      0       1       4448    4       1acfe1ee        1b2ff61f        2e8b2631        6faef306        c6fc10d3    6fcd6dcb           
    0               24              27      5               0       2       1               3       10064           9a8cb066        7a06385f        417e6103        2170fc56        acf676aa    6fcd6dcb                      

Bu veri kümesi içinde her iki sayısal ve kategorik sütunları eksik değerleri vardır. Eksik değerleri işlemek için basit bir yöntem açıklanmıştır. Verilerin ek ayrıntılarını bunları Hive tablolara depolarken incelediniz.

**Tanımı:** *geçişli tıklatma oranı (Ctrl):* veri tıklama yüzdesidir. Bu Criteo kümesinde yaklaşık %3.3 veya 0.033 CTRL değil.

## <a name="mltasks"></a>Tahmin görev örnekleri
İki örnek tahmin sorunların bu kılavuzda ele alınmıştır:

1. **İkili sınıflandırma**: kullanıcı ekleme tıklattınız olup olmadığını tahmin:
   
   * Sınıfı 0: Hiçbir tıklatın
   * Sınıf 1:'ı tıklatın
2. **Regresyon**: kullanıcı özelliklerinden bir reklam tıklatma olasılığını tahmin eder.

## <a name="setup"></a>Veri bilimi için ayarlanmış yukarı bir Hdınsight Hadoop kümesi
**Not:** genellikle budur bir **yönetici** görev.

Üç adımda Hdınsight kümeleri ile Tahmine dayalı analiz çözümleri oluşturmak için Azure veri bilimi ortamınızı ayarlayın:

1. [Depolama hesabı oluşturma](../../storage/common/storage-create-storage-account.md): Bu depolama hesap verileri Azure Blob Storage'da depolamak için kullanılır. Hdınsight kümelerinde kullanılan veri burada depolanır.
2. [Azure Hdınsight Hadoop kümeleri için veri bilimi özelleştirme](customize-hadoop-cluster.md): Bu adım, 64-bit Anaconda Python 2.7 tüm düğümlerde yüklü olan bir Azure Hdınsight Hadoop kümesi oluşturur. Hdınsight kümesi özelleştirirken tamamlamak için (Bu konuda açıklanan) iki önemli adım vardır.
   
   * Hdınsight kümenize ile 1. adımda oluşturulduğunda oluşturulan depolama hesabı bağlamanız gerekir. Bu depolama hesabı küme içinde işlenebilecek verilerine erişmek için kullanılır.
   * Oluşturulduktan sonra küme baş düğüm için uzaktan erişim etkinleştirmeniz gerekir. Burada belirttiğiniz (oluşturulduktan konumundaki küme için belirtilen kullanılanlardan farklı) uzaktan erişim kimlik bilgilerini Hatırla: aşağıdaki yordamları tamamlamanız gerekir.
3. [Azure ML çalışma alanı oluşturma](../studio/create-workspace.md): Bu Azure Machine Learning çalışma alanı, sonra ilk veri keşfi ve Hdınsight kümesinde örnekleme aşağı makine öğrenimi modellerini oluşturmak için kullanılır.

## <a name="getdata"></a>Alma ve ortak bir kaynaktan verileri kullanma
[Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) dataset erişilebilir bağlantıyı tıklatmak, kullanım koşullarını kabul ederek ve sağlayan bir adı. Bunun nasıl göründüğünü, bir anlık görüntüsünü burada gösterilir:

![Criteo koşullarını kabul edin](./media/hive-criteo-walkthrough/hLxfI2E.png)

Tıklatın **karşıdan devam** daha fazla bilgi için veri kümesi ve onun kullanılabilirliği hakkında.

Ortak bir veri bulunduğu [Azure blob depolama](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) konumu: wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/. "wasb" Azure Blob Depolama konumunuz anlamına gelir. 

1. Bu ortak blob depolama birimindeki veri sıkıştırması açılmış veri üç alt oluşur.
   
   1. Alt *ham/sayısı/* ilk 21 günlük verilerini - gün içeren\_gün 00\_20
   2. Alt *ham/train/* verileri tek bir gününü oluşur gün\_21
   3. Alt *ham/test/* iki günlük veri oluşur gün\_22 ve günü\_23
2. Kişiler için ham gzip verilerle başlatmak istediğiniz, bu da ana klasörde bulunan *ham /* day_NN.gz, burada NN gider 00-23 olarak.

Bir erişim, keşfetmek için alternatif bir yaklaşım ve biz Hive tablolarını oluşturduğunuzda, yerel yüklemeleri gerektirmez bu verileri daha sonra bu kılavuzda açıklanan modeli.

## <a name="login"></a>Küme headnode oturum açın
Küme headnode için oturum açmak için kullandığınız [Azure portal](https://ms.portal.azure.com) küme bulunamadı. Soldaki Hdınsight elephant simgesine tıklayın ve sonra kümenizi adına çift tıklayın. Gidin **yapılandırma** sekmesinde, sayfanın BAĞLAN simgesine çift tıklayın ve istendiğinde, uzaktan erişim kimlik bilgilerinizi girin. Bu küme headnode için alır.

İşte bir tipik ilk oturum açtığında küme headnode için benzer:

![Küme için oturum açın](./media/hive-criteo-walkthrough/Yys9Vvm.png)

Sol taraftaki "Hadoop komut bizim workhorse veri keşfi için olan", satırıdır. İki yararlı URL'lere - "Hadoop Yarn durumu" ve "Hadoop adı düğümü" dikkat edin. Yarn durum URL İş ilerleme durumunu gösterir ve ad düğümü URL küme yapılandırması ayrıntılarını verir.

Şimdi ayarlanır ve ilk bölümünü gözden geçirme başlamak için hazır: Hive kullanarak ve Azure Machine Learning için verileri hazırlığı veri keşfi.

## <a name="hive-db-tables"></a> Hive veritabanı ve tablo oluşturma
Hive tablolarını bizim Criteo veri kümesi oluşturmak için açık ***Hadoop komut satırı*** baş düğümü masaüstündeki ve komutunu girerek Hive dizini girin

    cd %hive_home%\bin

> [!NOTE]
> Tüm Hive komutları bu kılavuzda Hive Kutusu'ndan Çalıştır / directory istemi. Bu alan yol sorunları otomatik olarak dikkat edin. "Hive dizin istemi" koşulları kullanabilirsiniz "Hive bin / directory istemi" ve "Hadoop komut satırı" birbirinin yerine.
> 
> [!NOTE]
> Hive sorgusu çalıştırmak için aşağıdaki komutları her zaman kullanabilirsiniz:
> 
> 

        cd %hive_home%\bin
        hive

İle Hive REPL göründükten sonra bir "hive >"oturum, yalnızca kesin ve çalıştırmak üzere sorguyu yapıştırın.

Aşağıdaki kod, bir veritabanı "criteo" oluşturur ve 4 tablolar oluşturur:

* bir *sayıları oluşturmak için tablo* gün gününde yerleşik\_gün 00\_20,
* bir *tablosu tren veri kümesi olarak kullanılması için* günü yerleşik\_21, ve
* iki *tablolar test veri kümeleri olarak kullanmak için* günü yerleşik\_22 ve günü\_23 sırasıyla.

Gün biri tatil olduğundan test veri iki farklı tablolara bölme. Amaç, model bir tatil ve tatil olmayan arasındaki farklar tıklatın aracılığıyla kurundan algılayabilir belirlemektir.

Komut dosyası [örnek&#95;hive&#95;oluşturma&#95;criteo&#95;veritabanı&#95;ve&#95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql) kolaylık sağlamak için burada görüntülenir:

    CREATE DATABASE IF NOT EXISTS criteo;
    DROP TABLE IF EXISTS criteo.criteo_count;
    CREATE TABLE criteo.criteo_count (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/count';

    DROP TABLE IF EXISTS criteo.criteo_train;
    CREATE TABLE criteo.criteo_train (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/train';

    DROP TABLE IF EXISTS criteo.criteo_test_day_22;
    CREATE TABLE criteo.criteo_test_day_22 (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/test/day_22';

    DROP TABLE IF EXISTS criteo.criteo_test_day_23;
    CREATE TABLE criteo.criteo_test_day_23 (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/test/day_23';

Bu tablolar dış olduğundan yalnızca Azure Blob Storage (wasb) konumlarına işaret edebilir.

**HERHANGİ bir Hive sorgusu çalıştırmak için iki yolu vardır:**

1. **Komut satırı Hive REPL kullanarak**: ilk "hive" komutunu ve kopyalama vermek ve komut satırı Hive REPL bir sorguyu yapıştırın sağlamaktır. Bunu yapmak için aşağıdakileri yapın:
   
        cd %hive_home%\bin
        hive
   
     Şimdi komut satırı REPL kesme ve yapıştırma sorgu yürütülür.
2. **Sorguları bir dosyaya kaydedilmesi ve komutu yürütülürken**: sorguları .hql dosyasına kaydetmek için saniyedir ([örnek&#95;hive&#95;oluşturma&#95;criteo&#95;veritabanı&#95;ve&#95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql)) ve ardından sorguyu çalıştırmak için aşağıdaki komutu yürütün:
   
        hive -f C:\temp\sample_hive_create_criteo_database_and_tables.hql

### <a name="confirm-database-and-table-creation"></a>Veritabanı ve tablo oluşturma onaylayın
Ardından, Hive Kutusu'ndan aşağıdaki komutla veritabanı oluşturma Onayla / directory istemi:

        hive -e "show databases;"

Bu sunar:

        criteo
        default
        Time taken: 1.25 seconds, Fetched: 2 row(s)

Bu, "criteo" yeni bir veritabanı oluşturulmasını doğrular.

Hangi tabloları oluşturulan görmek için yalnızca Hive Kutusu'ndan komutu Yürüt / directory istemi:

        hive -e "show tables in criteo;"

Ardından aşağıdaki çıktı görmeniz gerekir:

        criteo_count
        criteo_test_day_22
        criteo_test_day_23
        criteo_train
        Time taken: 1.437 seconds, Fetched: 4 row(s)

## <a name="exploration"></a> Veri keşfi kovanında
Şimdi bazı temel veri keşfi kovanında yapmak hazırsınız. Tren örneklerde sayısı sayım tarafından başlamak ve test veri tabloları.

### <a name="number-of-train-examples"></a>Tren örnek sayısı
İçeriğini [örnek&#95;hive&#95;sayısı&#95;eğitmek&#95;tablo&#95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_train_table_examples.hql) burada gösterilir:

        SELECT COUNT(*) FROM criteo.criteo_train;

Bu verir:

        192215183
        Time taken: 264.154 seconds, Fetched: 1 row(s)

Alternatif olarak, biri de Hive Kutusu'ndan aşağıdaki komutu verebileceği / directory istemi:

        hive -f C:\temp\sample_hive_count_criteo_train_table_examples.hql

### <a name="number-of-test-examples-in-the-two-test-datasets"></a>Test örnekleri iki test kümelerindeki sayısı
Şimdi iki sınama veri kümesi örneklerde sayısı. İçeriğini [örnek&#95;hive&#95;sayısı&#95;criteo&#95;test&#95;gün&#95;22&#95;tablo&#95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_22_table_examples.hql) şunlardır:

        SELECT COUNT(*) FROM criteo.criteo_test_day_22;

Bu verir:

        189747893
        Time taken: 267.968 seconds, Fetched: 1 row(s)

Her zamanki gibi aynı zamanda betik Hive Kutusu'ndan çağırabilir / directory istemi komutu göre veren:

        hive -f C:\temp\sample_hive_count_criteo_test_day_22_table_examples.hql

Son olarak, test örnekleri Gün bazında test veri kümesinde sayısını incelemek\_23.

Bunu yapmak için komutu yalnızca gösterilen benzer (başvurmak [örnek&#95;hive&#95;sayısı&#95;criteo&#95;test&#95;gün&#95;23&#95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_23_examples.hql)):

        SELECT COUNT(*) FROM criteo.criteo_test_day_23;

Bu sunar:

        178274637
        Time taken: 253.089 seconds, Fetched: 1 row(s)

### <a name="label-distribution-in-the-train-dataset"></a>Tren kümesindeki etiket dağıtım
Tren kümesindeki etiket dağıtım ilginizi çekecektir. Bu görmek için içeriğini göster [örnek&#95;hive&#95;criteo&#95;etiket&#95;dağıtım&#95;eğitmek&#95;table.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_label_distribution_train_table.hql):

        SELECT Col1, COUNT(*) AS CT FROM criteo.criteo_train GROUP BY Col1;

Bu etiket dağıtım verir:

        1       6292903
        0       185922280
        Time taken: 459.435 seconds, Fetched: 2 row(s)

Pozitif etiketleri yüzdesi yaklaşık % 3.3 (özgün veri kümesiyle tutarlı) olduğunu unutmayın.

### <a name="histogram-distributions-of-some-numeric-variables-in-the-train-dataset"></a>Tren kümesindeki sayısal bazı değişkenler Histogram dağıtımları
Hive'nın yerel kullanabilirsiniz "histogram\_sayısal" sayısal değişkenleri dağıtımını nasıl göründüğünü bulmak için işlevi. İçeriğini işte [örnek&#95;hive&#95;criteo&#95;histogram&#95;numeric.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_histogram_numeric.hql):

        SELECT CAST(hist.x as int) as bin_center, CAST(hist.y as bigint) as bin_height FROM
            (SELECT
            histogram_numeric(col2, 20) as col2_hist
            FROM
            criteo.criteo_train
            ) a
            LATERAL VIEW explode(col2_hist) exploded_table as hist;

Aşağıdaki verir:

        26      155878415
        2606    92753
        6755    22086
        11202   6922
        14432   4163
        17815   2488
        21072   1901
        24113   1283
        27429   1225
        30818   906
        34512   723
        38026   387
        41007   290
        43417   312
        45797   571
        49819   428
        53505   328
        56853   527
        61004   160
        65510   3446
        Time taken: 317.851 seconds, Fetched: 20 row(s)

Görünüm - YANAL normal listesi yerine SQL benzeri bir çıktı oluşturmak için Hive görevi görür birlikte Aç. Unutmayın bu tablo, ilk sütun karşılık gelen depo merkezi ve ikinci depo sıklığı.

### <a name="approximate-percentiles-of-some-numeric-variables-in-the-train-dataset"></a>Tren kümesindeki sayısal bazı değişkenlerin yaklaşık yüzdebirlik değeri
Ayrıca sayısal değişkenleriyle yaklaşık yüzdebirlik değeri hesaplama ilgilendirir. Hive yerel "yüzdelik\_yaklaşık" bunu bize yapar. İçeriğini [örnek&#95;hive&#95;criteo&#95;yaklaşık&#95;percentiles.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_approximate_percentiles.hql) şunlardır:

        SELECT MIN(Col2) AS Col2_min, PERCENTILE_APPROX(Col2, 0.1) AS Col2_01, PERCENTILE_APPROX(Col2, 0.3) AS Col2_03, PERCENTILE_APPROX(Col2, 0.5) AS Col2_median, PERCENTILE_APPROX(Col2, 0.8) AS Col2_08, MAX(Col2) AS Col2_max FROM criteo.criteo_train;

Bu verir:

        1.0     2.1418600917169246      2.1418600917169246    6.21887086390288 27.53454893115633       65535.0
        Time taken: 564.953 seconds, Fetched: 1 row(s)

Yüzdebirlik değeri dağıtımını yakından genellikle herhangi bir sayısal değişken histogram dağıtımını ilişkilidir.         

### <a name="find-number-of-unique-values-for-some-categorical-columns-in-the-train-dataset"></a>Tren kümesindeki kategorik bazı sütunları için benzersiz değerlerin sayısını bulur
Veri keşfi etmeden, bazı kategorik sütunlar için aldıkları benzersiz değerlerin sayısını bulur. Bunu yapmak için içeriğini göster [örnek&#95;hive&#95;criteo&#95;benzersiz&#95;değerleri&#95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_categoricals.hql):

        SELECT COUNT(DISTINCT(Col15)) AS num_uniques FROM criteo.criteo_train;

Bu verir:

        19011825
        Time taken: 448.116 seconds, Fetched: 1 row(s)

Col15 19 M benzersiz değerler olduğunu unutmayın! "Bir hot kodlama" gibi naïve teknikleri kullanarak bu yüksek boyutlu kategorik değişkenleri kodlamak için uygun değildir. Özellikle, güçlü, sağlam bir teknik olarak adlandırılan [ile öğrenme sayar](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) bu sorunu verimli bir şekilde tackling açıklandığı gösterilen ve.

Son olarak bazı diğer kategorik sütunlar için de benzersiz değerlerin sayısını bakın. İçeriğini [örnek&#95;hive&#95;criteo&#95;benzersiz&#95;değerleri&#95;birden çok&#95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_multiple_categoricals.hql) şunlardır:

        SELECT COUNT(DISTINCT(Col16)), COUNT(DISTINCT(Col17)),
        COUNT(DISTINCT(Col18), COUNT(DISTINCT(Col19), COUNT(DISTINCT(Col20))
        FROM criteo.criteo_train;

Bu verir:

        30935   15200   7349    20067   3
        Time taken: 1933.883 seconds, Fetched: 1 row(s)

Yeniden Col20 dışındaki tüm sütunları birçok benzersiz değerlere sahip unutmayın.

### <a name="co-occurrence-counts-of-pairs-of-categorical-variables-in-the-train-dataset"></a>Tren kümesindeki kategorik değişkenlerin çiftleri ortak oluşumu sayar

Ayrıca ilgi kategorik değişkenleri çiftlerini ortak oluşum sayısı olur. Bu kod içinde kullanma belirlenebilir [örnek&#95;hive&#95;criteo&#95;eşleştirilmiş&#95;kategorik&#95;counts.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_paired_categorical_counts.hql):

        SELECT Col15, Col16, COUNT(*) AS paired_count FROM criteo.criteo_train GROUP BY Col15, Col16 ORDER BY paired_count DESC LIMIT 15;

Ters tarafından kendi oluşum sayısı sipariş ve 15 üstünde bu durumda bakın. Bu bize sunar:

        ad98e872        cea68cd3        8964458
        ad98e872        3dbb483e        8444762
        ad98e872        43ced263        3082503
        ad98e872        420acc05        2694489
        ad98e872        ac4c5591        2559535
        ad98e872        fb1e95da        2227216
        ad98e872        8af1edc8        1794955
        ad98e872        e56937ee        1643550
        ad98e872        d1fade1c        1348719
        ad98e872        977b4431        1115528
        e5f3fd8d        a15d1051        959252
        ad98e872        dd86c04a        872975
        349b3fec        a52ef97d        821062
        e5f3fd8d        a0aaffa6        792250
        265366bf        6f5c7c41        782142
        Time taken: 560.22 seconds, Fetched: 15 row(s)

## <a name="downsample"></a> Azure Machine Learning için aşağı örnek veri kümeleri
Veri kümeleri incelediniz ve böylece Azure Machine Learning modellerini yerleşik araştırması (birleşimleri dahil), herhangi bir değişkeni için bu tür örnek veri kümelerini nasıl yapılacağı gösterilmektedir. Odak noktası sorun, geri çağırma: örnek öznitelikleri (Col2 - Col40 özellik değerleri) kümesi düşünüldüğünde Col1 0 (hiçbir tıklayın) veya 1 (tıklatın) olup olmadığını tahmin etmek.

Örnek eğitin ve test veri kümeleri için %1 özgün boyutunun aşağı için Hive'nın yerel RAND() işlevini kullanın. Sonraki komut [örnek&#95;hive&#95;criteo&#95;alt örnekleyin&#95;eğitmek&#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_train_dataset.hql) tren veri kümesi için bunu yapar:

        CREATE TABLE criteo.criteo_train_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        ---Now downsample and store in this table

        INSERT OVERWRITE TABLE criteo.criteo_train_downsample_1perc SELECT * FROM criteo.criteo_train WHERE RAND() <= 0.01;

Bu verir:

        Time taken: 12.22 seconds
        Time taken: 298.98 seconds

Komut dosyası [örnek&#95;hive&#95;criteo&#95;alt örnekleyin&#95;test&#95;gün&#95;22&#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_22_dataset.hql) test verileri için mevcut gün\_22:

        --- Now for test data (day_22)

        CREATE TABLE criteo.criteo_test_day_22_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        INSERT OVERWRITE TABLE criteo.criteo_test_day_22_downsample_1perc SELECT * FROM criteo.criteo_test_day_22 WHERE RAND() <= 0.01;

Bu verir:

        Time taken: 1.22 seconds
        Time taken: 317.66 seconds


Son olarak, komut dosyası [örnek&#95;hive&#95;criteo&#95;alt örnekleyin&#95;test&#95;gün&#95;23&#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_23_dataset.hql) test verileri için mevcut gün\_23:

        --- Finally test data day_23
        CREATE TABLE criteo.criteo_test_day_23_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 srical feature; tring)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        INSERT OVERWRITE TABLE criteo.criteo_test_day_23_downsample_1perc SELECT * FROM criteo.criteo_test_day_23 WHERE RAND() <= 0.01;

Bu verir:

        Time taken: 1.86 seconds
        Time taken: 300.02 seconds

Bu, size bizim aşağı örneklenen tren kullanın ve Azure Machine Learning modellerini oluşturmak için veri kümelerini test etmek hazır olursunuz.

Azure Machine hangi sayısı tablo ilgiliyse Learning için geçmeden önce son önemli bileşeni yoktur. Sonraki alt bölümde, count tablo biraz ayrıntılı olarak ele alınmıştır.

## <a name="count"></a> Kısa bir tartışma sayısı tablosundaki
Gördüğünüz gibi çeşitli kategorik değişkenler çok yüksek bir boyut sahiptir. Bu kılavuzda, güçlü bir teknik olarak adlandırılan [ile öğrenme sayar](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) Bu değişkenler bir verimli kodlamak için sağlam bir şekilde sunulur. Bu teknik hakkında daha fazla bilgi, sağlanan bağlantısıdır.

[!NOTE]
>Bu kılavuzda, yüksek boyutlu kategorik özellikleri compact gösterimlerini üretmek için sayısı tabloları kullanarak odak noktasıdır. Bu kategorik özellikleri kodlamak için tek yolu değildir; diğer teknikleri hakkında daha fazla bilgi için ilgi kullanıcılar kontrol edebilirsiniz [bir-hot-encoding](http://en.wikipedia.org/wiki/One-hot) ve [özellik karma](http://en.wikipedia.org/wiki/Feature_hashing).
>

Sayısı tablolar üzerinde sayısı verileri oluşturmak için klasör ham/sayıma verileri kullanın. Modelleme bölümünde baştan kategorik özellikleri için bu sayısı tablolar oluşturma kullanıcılara gösterilir veya alternatif olarak, explorations için önceden derlenmiş sayısı tablosunu kullanmak için. Hangi aşağıdaki içinde olduğunda "count tabloları önceden oluşturulmuş" denir için sağlanmış olan sayısı tabloları kullanarak anlama. Sonraki bölümde bu tablolar erişmek ayrıntılı yönergeler sağlanır.

## <a name="aml"></a> Azure Machine Learning ile bir model oluşturma
Oluşturma işlemi Azure Machine learning'de modelimizi şu adımları izler:

1. [Azure Machine Learning Hive tablolarından veri alma](#step1)
2. [Deneme oluşturma: featurize sayısı tablolar ile veri temizleme](#step2)
3. [Oluşturmak, eğitmek ve modeli Puanlama](#step3)
4. [Modeli değerlendirin](#step4)
5. [Web hizmeti olarak modeli yayımlama](#step5)

Artık Azure Machine Learning Studio'da modelleri oluşturmaya hazırsınız. Aşağı örneklenen verilerimizi kümesindeki Hive tablolarını olarak kaydedilir. Azure Machine Learning kullanma **veri içeri aktarma** bu verileri okumak için modülü. Bu küme depolama hesabına erişmek için kimlik bilgilerini izleyen içinde sağlanır.

### <a name="step1"></a> 1. adım: Azure Machine Learning veri içeri aktarma modülü kullanılarak Hive tablolarından veri almak ve bir makine öğrenimi denemesinin için seçin
Başlangıç seçerek bir **+ yeni** -> **deneme** -> **boş deneme**. Öğesinden sonra **arama** kutusunu sol, üstteki "Veri Al" arayın. Sürükleme ve bırakma **veri içeri aktarma** modülünde deneme açın tuvale (ekranın Orta bölümünü) modülü veri erişimi için kullanılacak.

Bu nedir **veri içeri aktarma** gibi görünüyor Hive tablosundan veri alınırken hata oluştu:

![Veri içeri aktarma verileri alır](./media/hive-criteo-walkthrough/i3zRaoj.png)

İçin **veri içeri aktarma** modülü, grafik olarak sağlanan parametrelerin değerlerini sıralama sağlamanız gereken değerlerin yalnızca örnekler verilmiştir. Parametresi için ayarlanmış doldurmak nasıl bazı genel rehberlik işte **veri içeri aktarma** modülü.

1. "Hive sorgusu" seçin **veri kaynağı**
2. İçinde **Hive veritabanı sorgusu** kutusunda, basit bir SELECT * FROM <,\_veritabanı\_name.your\_tablo\_adı >-yeterlidir.
3. **Hcatalog sunucusu URI**: "abc" kümeniz olduğunu sonra yalnızca budur: https://abc.azurehdinsight.net
4. **Hadoop kullanıcı hesabı adı**: küme commissioning sırasında seçilen kullanıcı adı. (Uzaktan erişim kullanıcı adı değil!)
5. **Hadoop kullanıcı hesabı parolasını**: küme commissioning sırasında seçilen kullanıcı adı parolası. (Uzaktan erişim parola eklenmez!)
6. **Çıktı verilerini konumunu**: "Azure" seçin
7. **Azure depolama hesabı adı**: kümeyle ilişkili depolama hesabı
8. **Azure depolama hesabı anahtarı**: depolama hesabı anahtar kümeyle ilişkili.
9. **Azure kapsayıcı adı**: "abc" Küme adı olduğundan sonra yalnızca "abc", genellikle budur.

Bir kez **veri içeri aktarma** sonlandığında (gördüğünüz yeşil onay modülünü) veri alma (tercih ettiğiniz bir adla) bir veri kümesi olarak bu verileri kaydedin. Bunun nasıl göründüğünü:

![Veri alma veri kaydetme](./media/hive-criteo-walkthrough/oxM73Np.png)

Çıkış bağlantı noktasına sağ **veri içeri aktarma** modülü. Bu gösteren bir **veri kümesi Kaydet** seçeneği ve bir **Görselleştir** seçeneği. **Görselleştir** seçeneği tıkladıysanız, bazı Özet istatistikleri için yararlı olan bir sağ panelde yanı sıra veri 100 satırı görüntüler. Verileri kaydetmek için seçmeniz yeterlidir **veri kümesi Kaydet** ve yönergeleri izleyin.

Veri kümeleri kullanarak kaydedilmiş veri kümesi kullanmak için bir makine öğrenme deneme seçmek için bulun **arama** aşağıdaki resimde gösterilen kutusu. Yalnızca türü adı verdiğiniz veri kümesinin kısmen erişmek ve veri kümesi ana paneline sürükleyin. Ana panelinden bırakmadan machine learning modelleme kullanmak için seçilir.

![Ana bölmesi üzerine Drage veri kümesi](./media/hive-criteo-walkthrough/cl5tpGw.png)

> [!NOTE]
> Bu, eğitme ve test veri kümeleri için yapın. Ayrıca, bu amaçla verdiğiniz tablo adları ve veritabanı adını kullanmayı unutmayın. Şekilde kullanılan değerler yalnızca çizim yaratılır * için:
> 
> 

### <a name="step2"></a> 2. adım: Azure Machine Learning ile tıklama tahmin etmek için basit bir deneme oluşturma / hiçbir tıklama
Bizim Azure ML deneme şöyle görünür:

![Machine Learning deneme](./media/hive-criteo-walkthrough/xRpVfrY.png)

Şimdi bu deneme anahtar bileşenlerinin inceleyin. Bizim kaydedilmiş tren sürükleyin ve veri kümeleri bizim deneme tuvalinin açın önce test edin.

#### <a name="clean-missing-data"></a>Eksik verileri temizleme
**Clean Missing Data** modülü mu ne adından da anlaşılacağı: kullanıcı tarafından belirtilen yolla eksik verileri temizler. Bu görmek için bu modüle bakın:

![Eksik verilerini temizle](./media/hive-criteo-walkthrough/0ycXod6.png)

Tüm eksik değerleri 0 ile değiştirmek burada seçtiniz. Modüldeki bırakmalar bakarak görülebilir diğer seçenekler de vardır.

#### <a name="feature-engineering-on-the-data"></a>Mühendislik veri özelliği
Büyük veri kategorik bazı özellikler için benzersiz değerler milyonlarca olabilir. Naïve yöntemlerden biri hot yüksek boyutlu kategorik özelliklerden temsil etmek için kodlama gibi tamamen unfeasible kullanmaktır. Bu kılavuzda, yüksek boyutlu bu kategorik değişkenleri compact temsilini oluşturmak için yerleşik Azure Machine Learning modüllerini kullanma sayısı özelliklerinin nasıl kullanılacağını gösterir. Sonuç daha küçük bir model boyutu, daha hızlı eğitim sürelerine ve başka teknikler kullanarak oldukça karşılaştırılabilir performans ölçümleri ' dir.

##### <a name="building-counting-transforms"></a>Dönüşümler sayım oluşturma
Count özellikleri oluşturmak için kullanmak **yapı sayım dönüştürme** Azure Machine Learning kullanılabilir modül. Modül şöyle görünür:

![Sayım dönüştürme modülü oluşturmak](./media/hive-criteo-walkthrough/e0eqKtZ.png)
![yapı sayım dönüştürme Modülü](./media/hive-criteo-walkthrough/OdDN0vw.png)

> [!IMPORTANT] 
> İçinde **saymak sütunları** kutusuna, sayar çubuğunda gerçekleştirmek istediğiniz bu sütunları girin. Genellikle, bunlar (belirtildiği gibi) yüksek boyutlu kategorik sütun. Criteo dataset 26 kategorik sütun olduğunu unutmayın: Col40 Col15 gelen. Burada, hepsinde saymak ve bunların dizinlerini (15 gösterildiği gibi virgülle ayırarak 40 ')'ten verin.
> 

Modül MapReduce modunda kullanmak için (büyük veri kümeleri için uygundur), bir Hdınsight Hadoop kümesi erişmeniz (özellik araştırması için kullanılan bu amaç için yeniden kullanılabilir) ve kimlik bilgileri. Önceki rakamları görünüm gibi (yerine kendi kullanım durumu için ilgili olanla çizim için sağlanan değerler) doldurulan değerleri gösterilmektedir.

![Modülü parametreleri](./media/hive-criteo-walkthrough/05IqySf.png)

Yukarıdaki şekil giriş blob konumuna girmek nasıl gösterir. Bu konumda sayısı tabloları oluşturma için ayrılmış veri yok.

Bu modül çalışması sona erdiğinde, dönüştürme için daha sonra modülü sağ tıklayıp seçerek Kaydet **dönüştürme Kaydet** seçeneği:

!["Dönüştürme Kaydet" seçeneğini](./media/hive-criteo-walkthrough/IcVgvHR.png)

Yukarıda gösterilen bizim deneme mimarisinde, veri kümesi "ytransform2" tam olarak kaydedilmiş sayısı Dönüştür karşılık gelir. Bu deneme geri kalanı için onu okuyucu kullandığınız varsayılır bir **yapı sayım dönüştürme** modülü sayıları oluşturmak ve sonra kullanabilirsiniz sayısı oluşturmak için bu sayıları için bazı veriler, eğitme ve test veri kümelerinin özellikleri.

##### <a name="choosing-what-count-features-to-include-as-part-of-the-train-and-test-datasets"></a>Hangi sayısı eğitin ve test veri kümeleri dahil edilecek özellikleri seçme
Bir kez sayısı dönüştürme hazır kullanıcı kendi tren içerir ve veri kümeleri kullanarak test etmek için hangi özelliklerin seçebilir **sayısı tablo parametreleri değiştirmek** modülü. Eksiksiz olması için bu modülü burada gösterilir. Ancak ilgi Basitlik, aslında bunu bizim deneme kullanmayın.

![Count tablo parametreleri değiştirin](./media/hive-criteo-walkthrough/PfCHkVg.png)

Bu durumda, görüldüğü gibi büyük olasılıkla günlük kullanılacak olan ve geri alma sütun göz ardı edilir. Çöp Kutusu eşik gibi parametreleri düzgünleştirme ve tüm Laplacian gürültü veya kullanıp kullanmayacağınızı eklemek için kaç tane sözde önceki örnekler de ayarlayabilirsiniz. Bunların tümü Gelişmiş Özellikler ve bu varsayılan değerleri özellik oluşturma bu tür için yeni olan kullanıcılar için iyi bir başlangıç noktası olduğunu kaydedilmelidir.

##### <a name="data-transformation-before-generating-the-count-features"></a>Count özellikleri oluşturmadan veri dönüştürme
Odağı artık bir önemli bizim tren dönüştürme hakkında gelin ve test gerçekte sayısı özellikleri oluşturma önce verileri. İki olduğunu unutmayın **R betiği yürütün** sayısı dönüştürme için verilerimizi uygulanmadan önce kullanılan modülleri.

![R betiği modülleri yürütme](./media/hive-criteo-walkthrough/aF59wbc.png)

İlk R betiği şöyledir:

![İlk R betiği](./media/hive-criteo-walkthrough/3hkIoMx.png)

Bu R betiği bizim sütun adları "Col1" "Col40" için yeniden adlandırır. Bu biçim adını sayısı dönüştürme bekliyor olmasıdır.

Pozitif ve negatif sınıflar arasında dağıtım ikinci R betiği dengeleyen (sırasıyla 1 ve 0 sınıfları) aşağı örnekleme negatif sınıfı tarafından. R betiği buraya bunun nasıl yapılacağı gösterilmektedir:

![İkinci R betiği](./media/hive-criteo-walkthrough/91wvcwN.png)

Bu basit bir R betiği içinde "pos\_neg\_oranı" pozitif ve negatif sınıflar arasında bir denge miktarını ayarlamak için kullanılır. Bu sınıf dağıtım olduğu sınıflandırma sorunları (Bu durumda, %3.3 pozitif ve negatif sınıf %96.7 olduğunuz geri çağırma) eğri için iyileştirme sınıfı dengesizliği genellikle performans avantajı olduğundan yapmak önemlidir.

##### <a name="applying-the-count-transformation-on-our-data"></a>Verilerimizi üzerinde sayısı Dönüşüm Uygulama
Son olarak, kullanabileceğiniz **uygulamak dönüştürme** bizim tren üzerinde sayısı Dönüşümler Uygulama ve veri kümelerini test etmek için modül. Bu modül bir giriş olarak kaydedilmiş sayısı dönüştürme ve tren veya test veri kümeleri diğer giriş olarak alır ve sayısı özelliklere sahip verileri döndürür. Burada gösterilir:

![Dönüştürme modülü Uygula](./media/hive-criteo-walkthrough/xnQvsYf.png)

##### <a name="an-excerpt-of-what-the-count-features-look-like"></a>Bir alıntı gibi özellikleri bakın sayısı
Count özellikleri örneğimizde nasıl göründüğünü görmek için eğitici. Bu bir alıntı şöyledir:

![Count özellikleri](./media/hive-criteo-walkthrough/FO1nNfw.png)

Bu alıntı sayılan sütunlar için sayılarını elde ve büyük olasılıkla tüm ilgili backoffs yanı sıra oturum olduğunu gösterir.

Şimdi bu dönüştürülmüş veri kümelerini kullanarak bir Azure Machine Learning modeli oluşturmaya hazırsınız. Sonraki bölüm bu nasıl yapılabilir gösterir.

### <a name="step3"></a> 3. adım: Oluşturmak, eğitmek ve modeli Puanlama

#### <a name="choice-of-learner"></a>Öğrenen seçimi
İlk olarak, bir öğrenen seçmeniz gerekir. İki sınıflı artırılmış karar ağacı bizim öğrenen kullanın. Bu öğrenen varsayılan seçenekleri şunlardır:

![İki-Class Boosted karar ağacı parametreleri](./media/hive-criteo-walkthrough/bH3ST2z.png)

Deneme için varsayılan değerleri seçin. Varsayılanları genellikle anlamlı Not ve performans üzerinde hızlı temelleri almak için iyi bir yoludur. Bir taban çizgisi olduktan sonra seçerseniz parametreleri yerleştirmez performansı artırabilir.

#### <a name="train-the-model"></a>Modeli eğitme
Eğitim için basitçe çağırma bir **Train Model** modülü. Bu iki girişleri iki-Class Boosted karar ağacı öğrenen ve tren kümemize'dır. Bu aşağıda gösterilmiştir:

![Train Model Modülü](./media/hive-criteo-walkthrough/2bZDZTy.png)

#### <a name="score-the-model"></a>Modeli puanlama
Bir modeli eğittikten sonra test veri kümesinde Puanlama ve kendi performansını değerlendirmek için hazır olursunuz. Kullanarak bunu **Score Model** modülü ile birlikte aşağıdaki şekilde gösterilen bir **Evaluate Model** Modülü:

![Score Model (Model Puanlama) modülü](./media/hive-criteo-walkthrough/fydcv6u.png)

### <a name="step4"></a> 4. adım: modeli değerlendirin
Son olarak, model performans çözümlemeniz gerekir. Genellikle, iki sınıfı (ikili) sınıflandırma sorunu için iyi AUC ölçüsüdür. Bu görselleştirmek için takma **Score Model** modülüne bir **Evaluate Model** için bu modülü. Tıklatarak **Görselleştir** üzerinde **Evaluate Model** modülü aşağıdakine benzer bir grafik verir:

![Modül BDT modelini değerlendir](./media/hive-criteo-walkthrough/0Tl0cdg.png)

İkili (veya iki sınıf) sınıflandırma sorunları, tahmin doğruluğunu iyi bir ölçü olduğundan alanı altında eğri (AUC). Aşağıdaki bölümde test kümemize bu modeli kullanarak bizim sonuçlarını gösterir. Bu almak için çıkış bağlantı noktasına sağ **Evaluate Model** modülü ve ardından **Görselleştir**.

![Evaluate Model modülü Görselleştirme](./media/hive-criteo-walkthrough/IRfc7fH.png)

### <a name="step5"></a> 5. adım: bir Web hizmeti olarak modeli yayımlama
Web hizmetleri fuss en az olarak bir Azure Machine Learning modeli yayımlama özelliğine, yaygın olarak kullanılabilir hale getirme için değerli bir özelliktir. Bu yapıldığında, herkesin tahminleri için ihtiyaç duydukları ve web hizmeti, bu Öngörüler döndürülecek modelini kullanır. giriş verilerle web hizmeti çağrıları yapabilirsiniz.

Bunu yapmak için önce bizim eğitilen model eğitilen Model nesnesi olarak kaydedin. Bu sağ tıklayarak yapılır **Train Model** modülü ve kullanarak **eğitilen modelini Farklı Kaydet** seçeneği.

Ardından, giriş oluşturun ve çıkış bağlantı noktasına bizim web hizmeti için:

* bir giriş bağlantı noktası veri tahminleri için gereken verileri aynı biçiminde alır.
* bir çıkış bağlantı noktasına skoru etiketleri ve ilişkili olasılıklar döndürür.

#### <a name="select-a-few-rows-of-data-for-the-input-port"></a>Giriş bağlantı noktası için veri birkaç satır seçin
Kullanmak uygun olan bir **geçerli SQL dönüştürme** modülü giriş bağlantı noktası verileri olarak hizmet vermek için yalnızca 10 satır seçin. Yalnızca bu burada gösterilen SQL sorgusunu kullanarak, giriş bağlantı noktası için veri satırı seçin:

![Giriş bağlantı noktası verileri](./media/hive-criteo-walkthrough/XqVtSxu.png)

#### <a name="web-service"></a>Web hizmeti
Şimdi web hizmetimizi yayımlamak için kullanılan küçük bir denemeyi çalıştırmak hazırsınız.

#### <a name="generate-input-data-for-webservice"></a>Web hizmeti için giriş verileri oluştur
Sıfırıncı adım olarak sayısı tablo büyük olduğundan, birkaç satırlık bir test verileri alın ve çıktı verilerini buradan sayısı özelliklerle oluşturur. Bu bizim webservice için giriş veri biçimi olarak hizmet verebilir. Bu aşağıda gösterilmiştir:

![BDT giriş verileri oluşturma](./media/hive-criteo-walkthrough/OEJMmst.png)

> [!NOTE]
> Giriş veri biçimi için ÇIKTISINI kullanın **sayısı Featurizer** modülü. Bu deneme çalıştıran bittikten sonra çıkışı Kaydet **sayısı Featurizer** modülü bir veri kümesi olarak. Bu veri kümesi webservice giriş verileri için kullanılır.
> 
> 

#### <a name="scoring-experiment-for-publishing-webservice"></a>Deneme için Web Yayımlama hizmeti Puanlama
İlk olarak, bu nasıl göründüğünü gösterilir. Temel yapısı bir **Score Model** bizim eğitilen model nesnesi ve birkaç satırlık kullanarak önceki adımlarda oluşturulan giriş verilerini kabul eden modül **sayısı Featurizer** modülü. Scored etiketleri ve puanı olasılıklar proje için "Sütun Dataset içinde seçin" kullanın.

![Veri kümesinde sütun seçme](./media/hive-criteo-walkthrough/kRHrIbe.png)

Bildirim nasıl **Select Columns in Dataset sütun** modülü, 'verileri bir veri kümesinden filtreleme' için kullanılabilir. İçeriği burada gösterilir:

![Select Columns in Dataset modülü sütun ile filtreleme](./media/hive-criteo-walkthrough/oVUJC9K.png)

Mavi giriş ve çıkış bağlantı noktaları almak için tıklamanız yeterlidir **webservice hazırlama** sağ alt köşede. Bu deneme çalıştırılması de olanak tanır. web hizmeti yayımlamak için bize: tıklatın **yayımlama WEB hizmeti** alt right, gösterilen burada simgesine tıklayın:

![Web hizmeti yayımlama](./media/hive-criteo-walkthrough/WO0nens.png)

Webservice yayımlandığında, bu nedenle görünen bir sayfaya yönlendirilirsiniz:

![Web hizmeti Panosu](./media/hive-criteo-walkthrough/YKzxAA5.png)

Sol tarafındaki webservices'a için iki bağlantı dikkat edin:

* **İstek/yanıt** hizmeti (veya RR) tek tahminleri için tasarlanmıştır ve bu Atölye kullanılan.
* **Toplu iş yürütme** hizmeti (BES) için toplu tahminleri kullanılır ve girdi verileri Azure Blob Storage'da bulunan tahminleri yapmak için kullanılan gerektirir.

Bağlantıyı tıklatmak **istek/yanıt** bize bize sağlayan bir sayfa önceden tamamlanmış kod C#, python ve r alır Bu kod, Web hizmeti çağrıları yapmak için uygun şekilde kullanılabilir. Bu sayfada API anahtarı kimlik doğrulama için kullanılması gerektiğini unutmayın.

IPython not defterinde yeni bir hücreye üzerinden bu python kodu kopyalamak uygundur.

Python kodu doğru API anahtarı ile bir parçası değil.

![Python kodu](./media/hive-criteo-walkthrough/f8N4L4g.png)

Varsayılan API anahtarı bizim webservices'a 's API anahtarıyla yerini aldığını unutmayın. Tıklatarak **çalıştırmak** bir IPython Bu hücrede üzerinde dizüstü bilgisayar aşağıdaki yanıt verir:

![IPython yanıt](./media/hive-criteo-walkthrough/KSxmia2.png)

(JSON framework python komut) ilgili sorular örnekleri için iki test, "Scored etiketleri, Scored olasılıklar" biçiminde yanıt geri alın. Bu durumda, varsayılan değerleri önceden tamamlanmış kod (0'ların tüm sayısal sütunlar ve tüm kategorik sütunlar için "value" dizesi) sağladığını seçildi.

Azure Machine Learning kullanarak büyük ölçekli veri kümesi nasıl ele alınacağını gösteren bizim izlenecek sonlanır. Bir veri terabayt ile başlatıldı, bir tahmin modeli oluşturulan ve bulutta bir web hizmeti olarak dağıtılabilir.

