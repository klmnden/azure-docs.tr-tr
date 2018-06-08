---
title: 'Azure Data Lake ile ölçeklenebilir veri bilimi: bir uçtan uca kılavuz | Microsoft Docs'
description: Azure Data Lake bir veri kümesi üzerinde veri keşfi ve ikili sınıflandırma görevleri gerçekleştirmek için nasıl kullanılacağını.
services: machine-learning
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: 91a8207f-1e57-4570-b7fc-7c5fa858ffeb
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2017
ms.author: deguhath
ms.openlocfilehash: 62ca89ffe7507c2dc0a0f1a86750fb2bb996a5bd
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34836979"
---
# <a name="scalable-data-science-with-azure-data-lake-an-end-to-end-walkthrough"></a>Azure Data Lake ile ölçeklenebilir veri bilimi: bir uçtan uca gözden geçirme
Bu kılavuzda Azure Data Lake veri keşfi ve ikili sınıflandırma görevleri NYC ücreti seyahat örneği üzerinde yapın ve bir ipucu tarafından ücreti ödenen olsun veya olmasın tahmin etmek için veri kümesi masrafları için nasıl kullanılacağını gösterir. Adımlarda size yol gösterir [takım veri bilimi işlemi](http://aka.ms/datascienceprocess), uçtan uca, eğitim modeli için veri alma ve sonra model yayımlayan bir web hizmeti dağıtımına.

### <a name="azure-data-lake-analytics"></a>Azure Data Lake Analytics
[Microsoft Azure Data Lake](https://azure.microsoft.com/solutions/data-lake/) veri bilimcilerine kolaylaştırmak için gerekli tüm özellikleri boyutu, farklı şekillerdeki ve hızını verileri depolamak ve veri işleme, gelişmiş analizler ve machine learning ile yüksek modelleme yürütmek için sahip düşük maliyetli bir şekilde ölçeklenebilirlik.   Yalnızca veri gerçekte işlenirken bir iş başına temelinde ücret ödersiniz. Bildirim temelli SQL yapısını ölçeklenebilir sağlamak için gücüyle C# ile karıştırır bir dil sorgu yetenek dağıtılmış, Azure Data Lake Analytics U-SQL içerir. Yapılandırılmamış veriler üzerinde okuma şema uygulayarak işlem, özel mantık ve kullanıcı tanımlı işlevler (UDF'ler) ekleme olanak tanır ve ölçekte yürütmek nasıl üzerinden hassas denetimini etkinleştirmek için genişletilebilirlik içerir. U-SQL arkasında tasarımı felsefesi hakkında daha fazla bilgi için bkz: [Visual Studio blog gönderisi](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).

Data Lake Analytics ayrıca Cortana Analytics Suite’in de önemli bir parçasıdır ve Azure SQL Veri Ambarı, Power BI ve Veri Fabrikası ile birlikte çalışır. Bu, tam bulut büyük veri ve Gelişmiş analizi platformu sunar.

Bu kılavuz Önkoşullar ve veri bilimi işlem görevleri tamamlamak için gereken kaynakları nasıl yükleneceğini açıklayan tarafından başlar. U-SQL'yi kullanarak veri işleme adımlarını özetler ve Python ve Hive kullanmaya nasıl yapıldığını göstererek sonucuna sonra oluşturmak ve Tahmine dayalı modelleri dağıtmak için Azure Machine Learning Studio'da. 

### <a name="u-sql-and-visual-studio"></a>U-SQL ve Visual Studio
Bu kılavuzda, veri kümesi işlemek için U-SQL betikleri düzenlemek için Visual Studio kullanarak önerir. U-SQL betikleri ayrı bir dosyaya sağlanan ve burada açıklanmıştır. Alma, keşfetme ve veri örnekleme işlemi içerir. Ayrıca, Azure portalından bir U-SQL Script işini çalıştırmak nasıl gösterir. Hive tablolarını verileri yapı ve Azure Machine Learning Studio'da bir ikili sınıflandırma modeli dağıtımını kolaylaştırmak için ilişkili bir Hdınsight kümesi içinde oluşturulur.  

### <a name="python"></a>Python
Bu izlenecek yol da derleme ve Python ile Azure Machine Learning Studio kullanarak Tahmine dayalı bir model dağıtma gösterilmektedir bir bölüm içerir. Jupyter Not Defteri, bu işlem adımları için Python komut dosyaları sağlar. Not Defteri bazı ek özellik Mühendisliği adımları ve modelleri oluşturma çok sınıflı sınıflandırma ve Burada özetlenen ikili sınıflandırma modeli yanı sıra modelleme regresyon gibi kodunu içerir. Regresyon diğer ipucu özelliklerini temel alarak ipucu miktarı tahmin etmek için bir görevdir. 

### <a name="azure-machine-learning"></a>Azure Machine Learning
Azure Machine Learning Studio oluşturmak ve Tahmine dayalı modelleri dağıtmak için kullanılır. Bu yapılır iki yaklaşım kullanarak: ilk Python komut dosyaları ve ardından ile Hive tablolarını Hdınsight (Hadoop) kümesinde.

### <a name="scripts"></a>Betikler
Bu kılavuzda yalnızca asıl adımları özetlenmiştir. Tam indirebilirsiniz **U-SQL betiği** ve **Jupyter not defteri** gelen [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough).

## <a name="prerequisites"></a>Önkoşullar
Bu konularda başlamadan önce aşağıdakilere sahip olmanız gerekir:

* Azure aboneliği. Zaten bir yoksa, bkz: [alma Azure ücretsiz deneme sürümü](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* [Önerilen] Visual Studio 2013 veya üzeri. Zaten yüklü bu sürümlerinden birini yoksa, ücretsiz bir Community sürümü indirebilirsiniz [Visual Studio Community](https://www.visualstudio.com/vs/community/).

> [!NOTE]
> Visual Studio yerine Azure Data Lake sorguları göndermek için Azure portalını kullanabilirsiniz. Başlıklı portalında ve böylece her ikisi de Visual Studio ile nasıl üzerinde yönergeler verilmiştir **işlem U-SQL verilerle**. 
> 
> 


## <a name="prepare-data-science-environment-for-azure-data-lake"></a>Azure Data Lake için veri bilimi ortamını hazırlayın
Bu kılavuz için veri bilimi ortamını hazırlamak için aşağıdaki kaynakları oluşturun:

* Azure Data Lake deposu (ADLS) 
* Azure Data Lake Analytics (ADLA)
* Azure Blob storage hesabı
* Azure Machine Learning Studio hesabı
* (Önerilen) Visual Studio için Azure Data Lake araçları

Bu bölümde, bu kaynakların her biri oluşturma hakkında yönergeler sağlar. Hive tablolarını yerine Python, Azure Machine Learning ile kullanmayı seçerseniz bir model oluşturmak için ayrıca bir Hdınsight (Hadoop) kümesi sağlamak gerekir. Bu alternatif yordam seçeneği 2 bölümde açıklanmaktadır.


> [!NOTE]
> **Azure Data Lake Store** ya da ayrı olarak oluşturulabilir veya oluştururken **Azure Data Lake Analytics** varsayılan depolama. Bu kaynakların her biri ayrı olarak oluşturmak için yönergeler başvurulan ancak Data Lake storage hesabını ayrı olarak oluşturulmamış.
>
> 

### <a name="create-an-azure-data-lake-store"></a>Bir Azure Data Lake deposu oluşturma


Gelen bir ADLS oluşturma [Azure portal](http://portal.azure.com). Ayrıntılar için bkz [Azure portalını kullanarak Data Lake Store ile bir Hdınsight kümesi oluşturmayı](../../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md). Küme AAD kimlik ayarladığınızdan emin olun **DataSource** dikey **isteğe bağlı yapılandırma** dikey açıklanan vardır. 

 ![3](./media/data-lake-walkthrough/3-create-ADLS.PNG)

### <a name="create-an-azure-data-lake-analytics-account"></a>Bir Azure Data Lake Analytics hesabı oluşturma
Bir ADLA hesabından oluşturma [Azure portal](http://portal.azure.com). Ayrıntılar için bkz [Öğreticisi: Azure Data Lake Azure portalını kullanarak Analytics ile çalışmaya başlama](../../data-lake-analytics/data-lake-analytics-get-started-portal.md). 

 ![4](./media/data-lake-walkthrough/4-create-ADLA-new.PNG)

### <a name="create-an-azure-blob-storage-account"></a>Bir Azure Blob storage hesabı oluşturma
Bir Azure Blob Depolama hesabından oluşturma [Azure portal](http://portal.azure.com). Ayrıntılar için bir depolama hesabı bölümünde bkz [Azure storage hesapları hakkında](../../storage/common/storage-create-storage-account.md).

 ![5](./media/data-lake-walkthrough/5-Create-Azure-Blob.PNG)

### <a name="set-up-an-azure-machine-learning-studio-account"></a>Bir Azure Machine Learning Studio hesap ayarlama
Oturum/Azure Machine Learning Studio'dan içine [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) sayfası. Tıklayın **hemen kullanmaya başlayın** düğmesine tıklayın ve ardından "Ücretsiz çalışma" veya "Standart çalışma"'i seçin. Şimdi olduğunuz hazır Azure ML Studio'da denemeler oluşturmak için.  

### <a name="install-azure-data-lake-tools-recommended"></a>[Önerilen] Azure Data Lake Araçları'nı yükleme
Visual Studio'dan sürümünüz için Azure Data Lake araçları yükleme [Visual Studio için Azure Data Lake Araçları](https://www.microsoft.com/download/details.aspx?id=49504).

 ![6](./media/data-lake-walkthrough/6-install-ADL-tools-VS.PNG)

Yükleme başarıyla tamamlandıktan sonra Visual Studio'yu açın. Data Lake menüsünün üstünde sekmesinde görmeniz gerekir. Azure hesabınızda oturum açtığınızda Azure kaynaklarınızı sol panelinde görüntülenmesi gerekir.

 ![7](./media/data-lake-walkthrough/7-install-ADL-tools-VS-done.PNG)

## <a name="the-nyc-taxi-trips-dataset"></a>NYC ücreti dönüşleri veri kümesi
Burada kullanılan veri kümesi genel kullanıma açık bir veri kümesi--olan [NYC ücreti dönüşleri dataset](http://www.andresmh.com/nyctaxitrips/). Yaklaşık 20 GB sıkıştırılmış CSV dosyaları (~ 48 GB sıkıştırılmamış) NYC ücreti seyahat veri oluşur, her seyahat için ücretli 173 milyondan fazla tek tek dönüşleri ve fares kaydetme. Her seyahat kayıt alma ve teslim konumları ve saatleri, anonim korsan (sürücü) lisans numarası ve medallion (ücreti'nın benzersiz kimliği) numarasını içerir. Veri 2013 yıl içinde tüm dönüşleri kapsayan ve aşağıdaki iki veri kümelerinin her ay sağlanır:

'trip_data' CSV yolcu, toplama ve dropoff noktaları, seyahat süresi ve seyahat Uzunluk sayısı gibi seyahat ayrıntıları içerir. Birkaç örnek kayıt şunlardır:

       medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
       89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
       0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
       0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
       DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
       DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868



'trip_fare' CSV ödeme türü, ücreti tutarı, ek ücret ve vergileri, ipuçları ve tolls, gibi her seyahat için ödenen ücreti ve Ücretli toplam miktarı ayrıntılarını içerir. Birkaç örnek kayıt şunlardır:

       medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
       89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
       0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
       0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
       DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
       DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Seyahat katılmak için benzersiz anahtar\_veri ve seyahat\_ücreti şu üç alanlardan oluşur: medallion, korsan\_lisans ve alma\_datetime. Ham CSV dosyaları ortak Azure storage blobundan erişilebilir. Bu katılımı U-SQL betiği bulunduğu [katılma seyahat ve ücreti tabloları](#join) bölümü.

## <a name="process-data-with-u-sql"></a>U-SQL ile verileri işleme
Bu bölümde gösterilen veri işleme görevlerini alma, kalite denetimi, keşfetme ve veri örnekleme içerir. Seyahat ve ücreti tabloları nasıl de gösterilir. Son bölümü Azure portalından bir U-SQL komut dosyası işi çalıştırma gösterir. Aşağıda, her alt bağlantıları verilmiştir:

* [Veri alımı: ortak blob verileri okuma](#ingest)
* [Veri Kalitesi denetimleri](#quality)
* [Veri keşfi](#explore)
* [Seyahat ve ücreti tabloları birleştirme](#join)
* [Veri örnekleme](#sample)
* [U-SQL işleri çalıştırma](#run)

U-SQL betikleri ayrı bir dosyaya sağlanan ve burada açıklanmıştır. Tam indirebilirsiniz **U-SQL betikleri** gelen [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough).

U-SQL, açık Visual Studio yürütmek için tıklatın **Dosya--> Yeni Proje-->**, seçin **U-SQL projesi**, ad ve bir klasöre kaydedin.

![8](./media/data-lake-walkthrough/8-create-USQL-project.PNG)

> [!NOTE]
> U-SQL Visual Studio yerine yürütmek için Azure Portalı'nı kullanmak da mümkündür. Portalındaki Azure Data Lake Analytics kaynağa gidin ve sorgular doğrudan olarak aşağıdaki şekilde Resimli gönder:
> 
> 

![9](./media/data-lake-walkthrough/9-portal-submit-job.PNG)

### <a name="ingest"></a>Veri alımı: ortak blob verileri okuyun
Azure blob verileri konumu olarak başvurulur **wasb://container_name@blob_storage_account_name.blob.core.windows.net/blob_name** ve kullanarak ayıklanabilir **Extractors.Csv()**. Kendi kapsayıcı adı ve depolama hesabı adı için aşağıdaki komut yerine container_name@blob_storage_account_name wasb adresi. Dosya adları aynı biçimde olduğundan, kullanmak mümkün **seyahat\_veri_ {\*\}.csv** tüm 12 seyahat dosyaları okumak için. 

    ///Read in Trip data
    @trip0 =
        EXTRACT 
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count string,
        trip_time_in_secs string,
        trip_distance string,
        pickup_longitude string,
        pickup_latitude string,
        dropoff_longitude string,
        dropoff_latitude string
    // This is reading 12 trip data from blob
    FROM "wasb://container_name@blob_storage_account_name.blob.core.windows.net/nyctaxitrip/trip_data_{*}.csv"
    USING Extractors.Csv();

İlk satırın üstbilgi olduğundan üstbilgileri kaldırın ve uygun parçalara sütun türleri değiştirmeniz gerekir. İşlenen verilerin Azure Data Lake Storage kullanmaya Kaydet'i seçebilir ya da **swebhdfs://data_lake_storage_name.azuredatalakestorage.net/folder_name/file_name**_ veya Azure Blob Depolama hesabı kullanarak  **wasb://container_name@blob_storage_account_name.blob.core.windows.net/blob_name**. 

    // change data types
    @trip =
        SELECT 
        medallion,
        hack_license,
        vendor_id,
        rate_code,
        store_and_fwd_flag,
        DateTime.Parse(pickup_datetime) AS pickup_datetime,
        DateTime.Parse(dropoff_datetime) AS dropoff_datetime,
        Int32.Parse(passenger_count) AS passenger_count,
        Double.Parse(trip_time_in_secs) AS trip_time_in_secs,
        Double.Parse(trip_distance) AS trip_distance,
        (pickup_longitude==string.Empty ? 0: float.Parse(pickup_longitude)) AS pickup_longitude,
        (pickup_latitude==string.Empty ? 0: float.Parse(pickup_latitude)) AS pickup_latitude,
        (dropoff_longitude==string.Empty ? 0: float.Parse(dropoff_longitude)) AS dropoff_longitude,
        (dropoff_latitude==string.Empty ? 0: float.Parse(dropoff_latitude)) AS dropoff_latitude
    FROM @trip0
    WHERE medallion != "medallion";

    ////output data to ADL
    OUTPUT @trip   
    TO "swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_trip.csv"
    USING Outputters.Csv(); 

    ////Output data to blob
    OUTPUT @trip   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_trip.csv"
    USING Outputters.Csv();  

Benzer şekilde ücreti veri kümelerinde okuyabilir. Sağ Azure Data Lake Store, verilerinize bakmak seçebilir **Azure portal--> Veri Gezgini** veya **dosya Gezgini** Visual Studio içinde. 

 ![10](./media/data-lake-walkthrough/10-data-in-ADL-VS.PNG)

 ![11](./media/data-lake-walkthrough/11-data-in-ADL.PNG)

### <a name="quality"></a>Veri Kalitesi denetimleri
Veri Kalitesi denetimleri, seyahat ve ücreti tabloları okunduktan sonra aşağıdaki şekilde yapılabilir. Sonuçta elde edilen CSV dosyaları çıktısını Azure Blob storage veya Azure Data Lake Store olabilir. 

Medallions sayısı ve medallions benzersiz sayısını bulun:

    ///check the number of medallions and unique number of medallions
    @trip2 =
        SELECT
        medallion,
        vendor_id,
        pickup_datetime.Month AS pickup_month
        FROM @trip;

    @ex_1 =
        SELECT
        pickup_month, 
        COUNT(medallion) AS cnt_medallion,
        COUNT(DISTINCT(medallion)) AS unique_medallion
        FROM @trip2
        GROUP BY pickup_month;
        OUTPUT @ex_1   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_1.csv"
    USING Outputters.Csv(); 

100'den fazla dönüşleri vardı bu medallions bulun:

    ///find those medallions that had more than 100 trips
    @ex_2 =
        SELECT medallion,
               COUNT(medallion) AS cnt_medallion
        FROM @trip2
        //where pickup_datetime >= "2013-01-01t00:00:00.0000000" and pickup_datetime <= "2013-04-01t00:00:00.0000000"
        GROUP BY medallion
        HAVING COUNT(medallion) > 100;
        OUTPUT @ex_2   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_2.csv"
    USING Outputters.Csv(); 

Geçersiz kayıtların pickup_longitude bakımından bulun:

    ///find those invalid records in terms of pickup_longitude
    @ex_3 =
        SELECT COUNT(medallion) AS cnt_invalid_pickup_longitude
        FROM @trip
        WHERE
        pickup_longitude <- 90 OR pickup_longitude > 90;
        OUTPUT @ex_3   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_3.csv"
    USING Outputters.Csv(); 

Eksik değerleri için bazı değişkenler bulun:

    //check missing values
    @res =
        SELECT *,
               (medallion == null? 1 : 0) AS missing_medallion
        FROM @trip;

    @trip_summary6 =
        SELECT 
            vendor_id,
        SUM(missing_medallion) AS medallion_empty, 
        COUNT(medallion) AS medallion_total,
        COUNT(DISTINCT(medallion)) AS medallion_total_unique  
        FROM @res
        GROUP BY vendor_id;
    OUTPUT @trip_summary6
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_16.csv"
    USING Outputters.Csv();



### <a name="explore"></a>Veri keşfi
Bazı veri keşfi verilerin daha iyi anlamak için aşağıdaki komut dosyalarıyla yapın.

Eğimli ve eğimli olmayan dönüşleri dağıtımını bulun:

    ///tipped vs. not tipped distribution
    @tip_or_not =
        SELECT *,
               (tip_amount > 0 ? 1: 0) AS tipped
        FROM @fare;

    @ex_4 =
        SELECT tipped,
               COUNT(*) AS tip_freq
        FROM @tip_or_not
        GROUP BY tipped;
        OUTPUT @ex_4   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_4.csv"
    USING Outputters.Csv(); 

İpucu tutar sonlandırma değerlerle dağıtımını bulun: 0, 5, 10 ile 20 dolar.

    //tip class/range distribution
    @tip_class =
        SELECT *,
               (tip_amount >20? 4: (tip_amount >10? 3:(tip_amount >5 ? 2:(tip_amount > 0 ? 1: 0)))) AS tip_class
        FROM @fare;
    @ex_5 =
        SELECT tip_class,
               COUNT(*) AS tip_freq
        FROM @tip_class
        GROUP BY tip_class;
        OUTPUT @ex_5   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_5.csv"
    USING Outputters.Csv(); 

Seyahat uzaklığı temel istatistiklerinin bulun:

    // find basic statistics for trip_distance
    @trip_summary4 =
        SELECT 
            vendor_id,
            COUNT(*) AS cnt_row,
            MIN(trip_distance) AS min_trip_distance,
            MAX(trip_distance) AS max_trip_distance,
            AVG(trip_distance) AS avg_trip_distance 
        FROM @trip
        GROUP BY vendor_id;
    OUTPUT @trip_summary4
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_14.csv"
    USING Outputters.Csv();

Seyahat uzaklığı, yüzdebirlik değeri Bul:

    // find percentiles of trip_distance
    @trip_summary3 =
        SELECT DISTINCT vendor_id AS vendor,
                        PERCENTILE_DISC(0.25) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc,
                        PERCENTILE_DISC(0.5) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc,
                        PERCENTILE_DISC(0.75) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc
        FROM @trip;
       // group by vendor_id;
    OUTPUT @trip_summary3
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_13.csv"
    USING Outputters.Csv(); 


### <a name="join"></a>Seyahat ve ücreti tabloları birleştirme
Seyahat ve ücreti tabloları medallion, hack_license ve pickup_time tarafından katılabilir.

    //join trip and fare table

    @model_data_full =
    SELECT t.*, 
    f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,  f.total_amount, f.tip_amount,
    (f.tip_amount > 0 ? 1: 0) AS tipped,
    (f.tip_amount >20? 4: (f.tip_amount >10? 3:(f.tip_amount >5 ? 2:(f.tip_amount > 0 ? 1: 0)))) AS tip_class
    FROM @trip AS t JOIN  @fare AS f
    ON   (t.medallion == f.medallion AND t.hack_license == f.hack_license AND t.pickup_datetime == f.pickup_datetime)
    WHERE   (pickup_longitude != 0 AND dropoff_longitude != 0 );

    //// output to blob
    OUTPUT @model_data_full   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_7_full_data.csv"
    USING Outputters.Csv(); 

    ////output data to ADL
    OUTPUT @model_data_full   
    TO "swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_ex_7_full_data.csv"
    USING Outputters.Csv(); 


Her yolcu sayısı düzeyi için kayıtları, ortalama ipucu tutar, ipucu tutar varyansını, Eğimli dönüşleri yüzdesi sayısını hesaplayın.

    // contigency table
    @trip_summary8 =
        SELECT passenger_count,
               COUNT(*) AS cnt,
               AVG(tip_amount) AS avg_tip_amount,
               VAR(tip_amount) AS var_tip_amount,
               SUM(tipped) AS cnt_tipped,
               (float)SUM(tipped)/COUNT(*) AS pct_tipped
        FROM @model_data_full
        GROUP BY passenger_count;
        OUTPUT @trip_summary8
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_17.csv"
    USING Outputters.Csv();


### <a name="sample"></a>Veri örnekleme
İlk olarak, rastgele verilerin %0,1 birleştirilmiş tablosundan seçin:

    //random select 1/1000 data for modeling purpose
    @addrownumberres_randomsample =
    SELECT *,
            ROW_NUMBER() OVER() AS rownum
    FROM @model_data_full;

    @model_data_random_sample_1_1000 =
    SELECT *
    FROM @addrownumberres_randomsample
    WHERE rownum % 1000 == 0;

    OUTPUT @model_data_random_sample_1_1000   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_7_random_1_1000.csv"
    USING Outputters.Csv(); 

Ardından stratified örnekleme tarafından ikili değişken tip_class yapın:

    //stratified random select 1/1000 data for modeling purpose
    @addrownumberres_stratifiedsample =
    SELECT *,
            ROW_NUMBER() OVER(PARTITION BY tip_class) AS rownum
    FROM @model_data_full;

    @model_data_stratified_sample_1_1000 =
    SELECT *
    FROM @addrownumberres_stratifiedsample
    WHERE rownum % 1000 == 0;
    //// output to blob
    OUTPUT @model_data_stratified_sample_1_1000   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_9_stratified_1_1000.csv"
    USING Outputters.Csv(); 
    ////output data to ADL
    OUTPUT @model_data_stratified_sample_1_1000   
    TO "swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_ex_9_stratified_1_1000.csv"
    USING Outputters.Csv(); 


### <a name="run"></a>U-SQL işleri çalıştırma
U-SQL betikleri düzenlemeyi tamamladığınızda, Azure Data Lake Analytics hesabınızı kullanarak sunucuya gönderebilirsiniz. Tıklatın **Data Lake**, **işi Gönder**seçin, **Analytics hesabı**, seçin **paralellik**, tıklatıp **Gönder**  düğmesi.  

 ![12](./media/data-lake-walkthrough/12-submit-USQL.PNG)

İş başarıyla derlendiğini durumlarda, işin durumunu izlemek için Visual Studio'da görüntülenir. Çalışan iş tamamlandıktan sonra bile iş yürütme işlemi yeniden yürütme ve iş verimliliğinizi artırmak için performans sorunu adımları bulun. U-SQL işlerinizin durumunu denetlemek için Azure portalına de gidebilirsiniz.

 ![13](./media/data-lake-walkthrough/13-USQL-running-v2.PNG)

 ![14](./media/data-lake-walkthrough/14-USQL-jobs-portal.PNG)

Şimdi, Azure Blob Depolama veya Azure portal çıktı dosyaları kontrol edebilirsiniz. Sonraki adımda bizim modelleme için stratified örnek verileri kullanın.

 ![15](./media/data-lake-walkthrough/15-U-SQL-output-csv.PNG)

 ![16](./media/data-lake-walkthrough/16-U-SQL-output-csv-portal.PNG)

## <a name="build-and-deploy-models-in-azure-machine-learning"></a>Derleme ve Azure Machine Learning modellerini dağıtma
İki seçenek, verileri oluşturmak için Azure Machine Learning çekmek kullanılabilir ve 

* İlk seçenek, bir Azure Blob yazılmış örneklenen verileri kullan (içinde **veri örnekleme** Yukarıdaki adımı) ve Python derleme ve Azure Machine Learning modelinden dağıtmak için kullanın. 
* İkinci seçenek, doğrudan bir Hive sorgusu kullanarak Azure Data Lake verilerde sorgu. Bu seçenek, yeni bir Hdınsight kümesi oluşturma veya mevcut bir Hdınsight kümesine burada Azure Data Lake Storage NY ücreti verileri Hive tablolarını işaret kullanın gerektirir.  Her iki Bu seçenekler aşağıdaki bölümlerde ele alınmıştır. 

## <a name="option-1-use-python-to-build-and-deploy-machine-learning-models"></a>Seçenek 1: learning modellerini oluşturmak ve dağıtmak için kullanım Python makine
Derleme ve makine öğrenimi modellerini Python kullanarak dağıtmak için yerel makinenizde veya Azure Machine Learning Studio'da Jupyter not defteri oluşturun. Jupyter not defteri sağlanan [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough) keşfedin, verileri, özellik Mühendisliği, model ve dağıtım görselleştirmek için tam kodunu içerir. Bu makalede, yalnızca modelleme ve dağıtım ele alınmıştır. 

### <a name="import-python-libraries"></a>Python kitaplıkları içeri aktarma
Örnek çalıştırmak için dosya, aşağıdaki Python paketlerini gerekli Jupyter Not Defteri veya Python komut dosyası. AzureML Not Defteri hizmeti kullanıyorsanız, bu paketleri önceden yüklenmiş olmuştur.

    import pandas as pd
    from pandas import Series, DataFrame
    import numpy as np
    import matplotlib.pyplot as plt
    from time import time
    import pyodbc
    import os
    from azure.storage.blob import BlobService
    import tables
    import time
    import zipfile
    import random
    import sklearn
    from sklearn.linear_model import LogisticRegression
    from sklearn.cross_validation import train_test_split
    from sklearn import metrics
    from __future__ import division
    from sklearn import linear_model
    from azureml import services


### <a name="read-in-the-data-from-blob"></a>Blob verilerini okuma
* Bağlantı Dizesi   
  
        CONTAINERNAME = 'test1'
        STORAGEACCOUNTNAME = 'XXXXXXXXX'
        STORAGEACCOUNTKEY = 'YYYYYYYYYYYYYYYYYYYYYYYYYYYY'
        BLOBNAME = 'demo_ex_9_stratified_1_1000_copy.csv'
        blob_service = BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
* Metin olarak okuyun
  
        t1 = time.time()
        data = blob_service.get_blob_to_text(CONTAINERNAME,BLOBNAME).split("\n")
        t2 = time.time()
        print(("It takes %s seconds to read in "+BLOBNAME) % (t2 - t1))
  
  ![17](./media/data-lake-walkthrough/17-python_readin_csv.PNG)    
* Sütun adları eklemek ve ayrı sütunlar
  
        colnames = ['medallion','hack_license','vendor_id','rate_code','store_and_fwd_flag','pickup_datetime','dropoff_datetime',
        'passenger_count','trip_time_in_secs','trip_distance','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude',
        'payment_type', 'fare_amount', 'surcharge', 'mta_tax', 'tolls_amount',  'total_amount', 'tip_amount', 'tipped', 'tip_class', 'rownum']
        df1 = pd.DataFrame([sub.split(",") for sub in data], columns = colnames)
* Bazı sütunları sayısal değiştirme
  
        cols_2_float = ['trip_time_in_secs','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude',
        'fare_amount', 'surcharge','mta_tax','tolls_amount','total_amount','tip_amount', 'passenger_count','trip_distance'
        ,'tipped','tip_class','rownum']
        for col in cols_2_float:
            df1[col] = df1[col].astype(float)

### <a name="build-machine-learning-models"></a>Machine learning modellerini Derleme
Burada bir seyahat veya eğimli tahmin etmek için bir ikili sınıflandırma modeli oluşturun. Jupyter Not Defteri, diğer iki modeli bulabilirsiniz: çok sınıflı sınıflandırma ve regresyon modeli.

* İlk scikit içinde kullanılabilir kukla değişkenleri oluşturmak üzere ihtiyacınız-modelleri öğrenin
  
        df1_payment_type_dummy = pd.get_dummies(df1['payment_type'], prefix='payment_type_dummy')
        df1_vendor_id_dummy = pd.get_dummies(df1['vendor_id'], prefix='vendor_id_dummy')
* Modelleme için veri çerçeve oluşturma
  
        cols_to_keep = ['tipped', 'trip_distance', 'passenger_count']
        data = df1[cols_to_keep].join([df1_payment_type_dummy,df1_vendor_id_dummy])
  
        X = data.iloc[:,1:]
        Y = data.tipped
* Eğitim ve 60-40 bölünmüş test etme
  
        X_train, X_test, Y_train, Y_test = train_test_split(X, Y, test_size=0.4, random_state=0)
* Eğitim kümesindeki Lojistik regresyon
  
        model = LogisticRegression()
        logit_fit = model.fit(X_train, Y_train)
        print ('Coefficients: \n', logit_fit.coef_)
        Y_train_pred = logit_fit.predict(X_train)
  
       ![c1](./media/data-lake-walkthrough/c1-py-logit-coefficient.PNG)
* Sınama veri kümesi puan
  
        Y_test_pred = logit_fit.predict(X_test)
* Değerlendirme ölçümleri Hesapla
  
        fpr_train, tpr_train, thresholds_train = metrics.roc_curve(Y_train, Y_train_pred)
        print fpr_train, tpr_train, thresholds_train
  
        fpr_test, tpr_test, thresholds_test = metrics.roc_curve(Y_test, Y_test_pred) 
        print fpr_test, tpr_test, thresholds_test
  
        #AUC
        print metrics.auc(fpr_train,tpr_train)
        print metrics.auc(fpr_test,tpr_test)
  
        #Confusion Matrix
        print metrics.confusion_matrix(Y_train,Y_train_pred)
        print metrics.confusion_matrix(Y_test,Y_test_pred)
  
       ![c2](./media/data-lake-walkthrough/c2-py-logit-evaluation.PNG)

### <a name="build-web-service-api-and-consume-it-in-python"></a>Web hizmeti API'si derleme ve Python içinde kullanma
Makine, oluşturulduktan sonra model öğrenimi faaliyete istiyorsunuz. İkili Lojistik modeli kullanılan burada bir örnek olarak. Scikit emin olun-yerel makinenize sürümünde 0.15.1 olduğunu öğrenin. Azure ML studio hizmeti kullanıyorsanız bu hakkında endişelenmeniz gerekmez.

* Çalışma alanı kimlik bilgileriniz Azure ML studio ayarlarından bulun. Azure Machine Learning Studio'da tıklatın **ayarları** --> **adı** --> **yetkilendirme belirteçleri**. 
  
    ![c3](./media/data-lake-walkthrough/c3-workspace-id.PNG)

        workspaceid = 'xxxxxxxxxxxxxxxxxxxxxxxxxxx'
        auth_token = 'xxxxxxxxxxxxxxxxxxxxxxxxxxx'

* Web hizmeti oluşturma
  
        @services.publish(workspaceid, auth_token) 
        @services.types(trip_distance = float, passenger_count = float, payment_type_dummy_CRD = float, payment_type_dummy_CSH=float, payment_type_dummy_DIS = float, payment_type_dummy_NOC = float, payment_type_dummy_UNK = float, vendor_id_dummy_CMT = float, vendor_id_dummy_VTS = float)
        @services.returns(int) #0, or 1
        def predictNYCTAXI(trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH,payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS ):
            inputArray = [trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH, payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS]
            return logit_fit.predict(inputArray)
* Web hizmeti kimlik bilgileri alma
  
        url = predictNYCTAXI.service.url
        api_key =  predictNYCTAXI.service.api_key
  
        print url
        print api_key
  
        @services.service(url, api_key)
        @services.types(trip_distance = float, passenger_count = float, payment_type_dummy_CRD = float, payment_type_dummy_CSH=float,payment_type_dummy_DIS = float, payment_type_dummy_NOC = float, payment_type_dummy_UNK = float, vendor_id_dummy_CMT = float, vendor_id_dummy_VTS = float)
        @services.returns(float)
        def NYCTAXIPredictor(trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH,payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS ):
            pass
* Web hizmeti API'sine çağırın. Önceki adım 5-10 saniye beklemeniz gerekir.
  
        NYCTAXIPredictor(1,2,1,0,0,0,0,0,1)
  
       ![c4](./media/data-lake-walkthrough/c4-call-API.PNG)

## <a name="option-2-create-and-deploy-models-directly-in-azure-machine-learning"></a>Seçenek 2: Oluşturma ve Azure Machine Learning modellerini doğrudan dağıtma
Azure Machine Learning Studio verileri doğrudan Azure Data Lake Deposu'ndan veri okuyabilir ve sonra oluşturmak ve modelleri dağıtmak için kullanılır. Bu yaklaşım Azure Data Lake Store işaret eden bir Hive tablosu kullanır. Bu ayrı bir Azure Hdınsight kümesi olduğunu sağlanması, gerektirir Hive tablosu oluşturulduğu üzerinde. Aşağıdaki bölümlerde bunun nasıl yapılacağı gösterilmektedir. 

### <a name="create-an-hdinsight-linux-cluster"></a>Hdınsight Linux kümesi oluşturma
Hdınsight kümesi (Linux) oluşturmak [Azure portal](http://portal.azure.com). Ayrıntılar için bkz **Azure Data Lake Store erişimi olan bir Hdınsight kümesi oluşturmayı** bölümüne [Azure portalını kullanarak Data Lake Store ile bir Hdınsight kümesi oluşturmayı](../../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

 ![18](./media/data-lake-walkthrough/18-create_HDI_cluster.PNG)

### <a name="create-hive-table-in-hdinsight"></a>Hdınsight'ta Hive tablosu oluşturma
Şimdi önceki adımda Azure Data Lake Store içinde depolanan verileri kullanarak Hdınsight kümesinde Azure Machine Learning Studio'da kullanılacak Hive tabloları oluşturma. Oluşturulan Hdınsight kümesi gidin. Tıklatın **ayarları** --> **özellikleri** --> **küme AAD kimlik** --> **ADLS erişimini**, Azure Data Lake Store hesabınızı okuma listesinde eklendiğinden emin olun, yazma ve yürütme hakları. 

 ![19](./media/data-lake-walkthrough/19-HDI-cluster-add-ADLS.PNG)

Ardından **Pano** yanına **ayarları** düğmesi ve bir pencere açılır. Tıklatın **Hive görünümü** ve sayfanın sağ üst köşedeki görmelisiniz **sorgu Düzenleyicisi'ni**.

 ![20](./media/data-lake-walkthrough/20-HDI-dashboard.PNG)

 ![21](./media/data-lake-walkthrough/21-Hive-Query-Editor-v2.PNG)

Bir tablo oluşturmak için aşağıdaki Hive komut dosyasında yapıştırın. Bu şekilde Azure Data Lake Store Başvurusu'nda veri kaynağı konumu şudur: **adl://data_lake_store_name.azuredatalakestore.net:443/klasör_adı/dosya_adı**.

    CREATE EXTERNAL TABLE nyc_stratified_sample
    (
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count string,
        trip_time_in_secs string,
        trip_distance string,
        pickup_longitude string,
        pickup_latitude string,
        dropoff_longitude string,
        dropoff_latitude string,
      payment_type string,
      fare_amount string,
      surcharge string,
      mta_tax string,
      tolls_amount string,
      total_amount string,
      tip_amount string,
      tipped string,
      tip_class string,
      rownum string
      )
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    LOCATION 'adl://data_lake_storage_name.azuredatalakestore.net:443/nyctaxi_folder/demo_ex_9_stratified_1_1000_copy.csv';


Sorgu çalışması sona erdiğinde, şöyle sonuçları görmeniz gerekir:

 ![22](./media/data-lake-walkthrough/22-Hive-Query-results.PNG)

### <a name="build-and-deploy-models-in-azure-machine-learning-studio"></a>Derleme ve Azure Machine Learning Studio'da modelleri dağıtma
Şimdi oluşturmak ve bir ipucu Azure Machine Learning ile Ücretli olsun veya olmasın tahmin modeli dağıtmak hazır olursunuz. Stratified örnek verileri bu ikili sınıflandırma kullanıma hazır (veya ipucu) sorun. Çok sınıflı sınıflandırma (tip_class) ve regresyon (tip_amount) kullanarak Tahmine dayalı modelleri Ayrıca oluşturulmuş ve Azure Machine Learning Studio ile dağıtılır, ancak burada yalnızca ikili sınıflandırma modelini kullanarak durumda nasıl ele alınacağını gösterilen.

1. Azure ML kullanarak veri almak **veri içeri aktarma** modülü, kullanılabilir **veri giriş ve çıkış** bölümü. Daha fazla bilgi için bkz: [veri içeri aktarma modülü](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) başvuru sayfası.
2. Seçin **Hive sorgusu** olarak **veri kaynağı** içinde **özellikleri** paneli.
3. Aşağıdaki Hive betiğini yapıştırın **Hive veritabanı sorgusu** Düzenleyicisi
   
        select * from nyc_stratified_sample;
4. URI, Hdınsight kümesi (Bu Azure portalında bulunabilir), Hadoop kimlik bilgileri, çıktı verilerini Azure depolama hesabı anahtarı/ad/kapsayıcı adı ve konumu girin.
   
   ![23](./media/data-lake-walkthrough/23-reader-module-v3.PNG)  

Hive tablosundaki verileri aşağıdaki şekilde gösterilen bir ikili sınıflandırma deneme okuma örneği:

 ![24](./media/data-lake-walkthrough/24-AML-exp.PNG)

Denemeyi oluşturulduktan sonra tıklatın **Web hizmetinin ayarı** --> **Tahmine dayalı Web hizmeti**

 ![25](./media/data-lake-walkthrough/25-AML-exp-deploy.PNG)

Otomatik olarak oluşturulan çalışması sona erdiğinde, denemeyi Puanlama, tıklatın **Web hizmeti Dağıt**

 ![26](./media/data-lake-walkthrough/26-AML-exp-deploy-web.PNG)

Web hizmeti Pano kısa süre içinde görüntüler:

 ![27](./media/data-lake-walkthrough/27-AML-web-api.PNG)

## <a name="summary"></a>Özet
Bu kılavuzu izleyerek Azure Data Lake içinde ölçeklenebilir uçtan uca çözümler oluşturmak için bir veri bilimi ortamı oluşturdunuz. Bu ortam, veri bilimi işleminden veri alım model eğitim üzerinden ve sonra bir web hizmeti olarak modeli dağıtımına kurallı adımları boyunca alan bir büyük bir genel veri çözümlemek için kullanıldı. U-SQL işlemek, araştırın ve veri örneği için kullanıldı. Python ve Hive Azure Machine Learning Studio oluşturmak ve Tahmine dayalı modelleri dağıtmak için kullanılmıştır.

## <a name="whats-next"></a>Sırada ne var?
Öğrenme yolunu [takım veri bilimi işlem (TDSP)](http://aka.ms/datascienceprocess) gelişmiş analizler işlemdeki her adım açıklayan konulara bağlantılar sağlar. İzlenecek yollar üzerinde dökümü dizi bulunmaktadır [takım veri bilimi süreci gözden geçirmeleri](walkthroughs.md) kaynaklarını ve Hizmetleri çeşitli Tahmine dayalı analiz senaryolarda kullanmak nasıl çalışmaya başlanacağını gösteren sayfa:

* [Eylem takım veri bilimi işleminde: SQL Data Warehouse kullanma](sqldw-walkthrough.md)
* [Eylem takım veri bilimi işleminde: Hdınsight Hadoop kümeleri kullanma](hive-walkthrough.md)
* [Takım veri bilimi işlem: SQL Server kullanma](sql-walkthrough.md)
* [Azure Hdınsight'ta Spark kullanarak veri bilimi işlemine genel bakış](spark-overview.md)
