---
title: Azure Data Lake - Team Data Science Process'i ile ölçeklenebilir veri bilimi
description: Bir veri kümesi üzerinde veri keşfi ve ikili sınıflandırma görevleri yapmak için Azure Data Lake kullanma
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
ms.openlocfilehash: cc37109eda2690b4407f9cd0c92851b7c0e3f915
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60400091"
---
# <a name="scalable-data-science-with-azure-data-lake-an-end-to-end-walkthrough"></a>Azure Data Lake ile ölçeklenebilir veri bilimi: Uçtan uca kılavuz
Bu yönerge, Azure Data Lake veri keşfi ve NYC taksi seyahat örneği ikili sınıflandırma görevleri ve masrafları ipucu tarafından bir taksi Ücretli olup olmadığını tahmin etmek için veri kümesi için nasıl kullanılacağını gösterir. Bu adımlarında size kılavuzluk eder [Team Data Science Process](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/), uçtan uca, eğitim modeli için veri alma ve sonra model yayımlayan bir web hizmeti dağıtımı için.

### <a name="azure-data-lake-analytics"></a>Azure Data Lake Analytics
[Microsoft Azure Data Lake](https://azure.microsoft.com/solutions/data-lake/) veri bilimcileri kolaylaştırmak için gereken tüm özellikleri herhangi bir boyut, Şekil ve hızlı veri depolama ve veri işleme, Gelişmiş analiz ve makine öğrenimi ile yüksek modelleme yürütmek için sahip uygun maliyetli ölçeklenebilirlik.   Yalnızca veri gerçekten işlenirken bir iş başına temelinde ödeme yaparsınız. Azure Data Lake Analytics U-SQL içerir, SQL'in bildirim temelli doğasını ölçeklenebilir sağlamak için C# etkileyici gücüyle karışan bir dilde sorgu özelliği dağıtılmış. Okuma sırasında şema uygulayarak yapılandırılmamış verileri işlemek, özel mantığı ve kullanıcı tanımlı işlevler (UDF'ler) ekleme sağlar ve uygun ölçekte yürütmek nasıl üzerinde ayrıntılı denetim etkinleştirmek için gereken extensibility içerir. U-SQL arkasında tasarımı felsefesi hakkında daha fazla bilgi için bkz: [Visual Studio blog gönderisi](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).

Data Lake Analytics ayrıca Cortana Analytics Suite’in de önemli bir parçasıdır ve Azure SQL Veri Ambarı, Power BI ve Veri Fabrikası ile birlikte çalışır. Bu, bir bulut büyük veri ve Gelişmiş analiz platformu sağlar.

Bu kılavuz Önkoşullar ve veri bilimi işlem görevlerini tamamlamak için gereken kaynakları nasıl yükleneceğini açıklayan tarafından başlar. U-SQL'yi kullanarak veri işleme adımlarını özetler ve Python ve Hive kullanmaya nasıl yapıldığını göstererek sonuçlandıktan sonra Azure Machine Learning Studio'da Tahmine dayalı modelleri oluşturma ve dağıtma için.

### <a name="u-sql-and-visual-studio"></a>U-SQL ve Visual Studio
Bu izlenecek yolda, veri kümesini işlemek için U-SQL betikleri düzenlemek için Visual Studio kullanmanızı önerir. U-SQL betikleri ayrı bir dosyada sağlanan ve burada açıklanmıştır. İşlem almak için keşfetmek ve örnekleme verileri içerir. Ayrıca, Azure portalından bir U-SQL komut dosyası iş çalıştırılmasına nasıl gösterir. Hive tabloları oluşturma ve Azure Machine Learning Studio'da bir ikili sınıflandırma modelinde dağıtımını kolaylaştırmak için ilişkili bir HDInsight kümesinde veri için oluşturulur.

### <a name="python"></a>Python
Bu izlenecek yol, oluşturun ve Azure Machine Learning Studio ile Python kullanarak Tahmine dayalı model dağıtma gösterilmektedir bir bölüm de içerir. Jupyter Not Defteri, bu işlem adımları için Python betikleri ile sağlar. Not defteri için bazı ek özellik Mühendisliği adımları ve modelleri oluşturma sınıflı sınıflandırma ve regresyon Burada özetlenen ikili sınıflandırma modelinde yanı sıra modelleme gibi kod içerir. Diğer ipucu özelliklerini temel alarak bahşiş miktarını tahmin etmek için regresyon görevdir.

### <a name="azure-machine-learning"></a>Azure Machine Learning
Azure Machine Learning Studio'da Tahmine dayalı modelleri oluşturma ve dağıtma için kullanılır. Bu yapılır iki yaklaşım kullanma: ilk Python betikleri ve ardından HDInsight (Hadoop) kümesinde Hive tablolarını.

### <a name="scripts"></a>Betikler
Bu izlenecek yolda yalnızca asıl adımları özetlenmiştir. Tam indirebileceğiniz **U-SQL betiği** ve **Jupyter not defteri** gelen [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough).

## <a name="prerequisites"></a>Önkoşullar
Bu konu başlıkları başlamadan önce aşağıdakilere sahip olmanız gerekir:

* Azure aboneliği. Zaten bir yoksa, bkz. [alma Azure ücretsiz deneme sürümü](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* [Önerilen] Visual Studio 2013 veya üzeri. Zaten yüklü bu sürümlerinden birini yoksa, ücretsiz bir Community sürümü indirebilirsiniz [Visual Studio Community](https://www.visualstudio.com/vs/community/).

> [!NOTE]
> Visual Studio yerine, Azure Data Lake sorguları göndermek için Azure portalını kullanabilirsiniz. Böylece hem Visual Studio ile nasıl yapıldığını öğrenmek ve bölümüne portalında yönergeler verilmiştir **U-SQL ile verileri**.
>
>


## <a name="prepare-data-science-environment-for-azure-data-lake"></a>Azure Data Lake için veri bilimi ortamını hazırlama
Bu izlenecek yol için veri bilimi ortamını hazırlamak için aşağıdaki kaynakları oluşturun:

* Azure Data Lake Store (ADLS)
* Azure Data Lake Analytics'i (ADLA)
* Azure Blob Depolama hesabı
* Azure Machine Learning Studio hesabı
* (Önerilen) Visual Studio için Azure Data Lake araçları

Bu bölümde, bu kaynakların her biri oluşturma hakkında yönergeler sağlar. Hive tablolarını yerine Python ve Azure Machine Learning ile kullanmayı seçerseniz bir model oluşturmak için de HDInsight (Hadoop) kümesi sağlamak ihtiyacınız. Bu alternatif bir yordam seçeneği 2 bölümünde açıklanan.


> [!NOTE]
> **Azure Data Lake Store** ya da ayrı olarak oluşturulabilir veya oluşturduğunuzda **Azure Data Lake Analytics** varsayılan depolama alanı olarak. Bu kaynakların her biri ayrı olarak oluşturmak için yönergeler başvurulan, ancak Data Lake storage hesabını ayrı ayrı oluşturulup.
>
>

### <a name="create-an-azure-data-lake-store"></a>Bir Azure Data Lake Store oluşturun


Gelen bir ADLS oluşturma [Azure portalında](https://portal.azure.com). Ayrıntılar için bkz [Azure portalını kullanarak Data Lake Store ile HDInsight kümesi oluşturma](../../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md). Küme AAD kimlik ayarlamak mutlaka **DataSource** dikey **isteğe bağlı yapılandırma** dikey açıklanan vardır.

 ![3](./media/data-lake-walkthrough/3-create-ADLS.PNG)

### <a name="create-an-azure-data-lake-analytics-account"></a>Bir Azure Data Lake Analytics hesabı oluşturma
Bir ADLA hesabı oluşturma [Azure portalında](https://portal.azure.com). Ayrıntılar için bkz [öğretici: Azure Data Lake Azure portalını kullanarak Analytics ile çalışmaya başlama](../../data-lake-analytics/data-lake-analytics-get-started-portal.md).

 ![4](./media/data-lake-walkthrough/4-create-ADLA-new.PNG)

### <a name="create-an-azure-blob-storage-account"></a>Bir Azure Blob Depolama hesabı oluşturma
Bir Azure Blob Depolama hesabındaki oluşturma [Azure portalında](https://portal.azure.com). Ayrıntılar için bkz: bir depolama hesabı bölümünde oluşturma [Azure depolama hesapları hakkında](../../storage/common/storage-create-storage-account.md).

 ![5](./media/data-lake-walkthrough/5-Create-Azure-Blob.PNG)

### <a name="set-up-an-azure-machine-learning-studio-account"></a>Bir Azure Machine Learning Studio hesap ayarlama
Yukarı/Azure Machine Learning Studio'dan içine oturum [Azure Machine Learning studio](https://azure.microsoft.com/services/machine-learning/) sayfası. Tıklayarak **hemen başlayın** düğmesine tıklayın ve ardından bir "Ücretsiz çalışma alanı" veya "Standart çalışma" seçin. Artık olduğunuz hazır Azure Machine Learning Studio'da denemeleri oluşturmak için.

### <a name="install-azure-data-lake-tools-recommended"></a>Azure Data Lake araçları [önerilen] yükleyin
Azure Data Lake araçları Visual Studio'dan sürümünüz için yükleme [Visual Studio için Azure Data Lake Araçları](https://www.microsoft.com/download/details.aspx?id=49504).

 ![6](./media/data-lake-walkthrough/6-install-ADL-tools-VS.PNG)

Yükleme başarıyla tamamlandıktan sonra Visual Studio'yu açın. Data Lake menüsünün üstünde sekmesinde görmeniz gerekir. Azure kaynaklarınızı, Azure hesabınızda oturum açın, sol bölmede bulunan görüntülenmesi gerekir.

 ![7](./media/data-lake-walkthrough/7-install-ADL-tools-VS-done.PNG)

## <a name="the-nyc-taxi-trips-dataset"></a>NYC taksi Gelişlerin veri kümesi
Burada kullanılan veri kümesi genel kullanıma açık bir veri kümesi--olan [NYC taksi Gelişlerin dataset](https://www.andresmh.com/nyctaxitrips/). Yaklaşık 20 GB sıkıştırılmış CSV dosyalar (sıkıştırmadan ~ 48 GB), NYC taksi seyahat verilerini oluşuyorsa, 173 milyondan fazla bireysel gelişlerin ve fares kaydetmek için her bir seyahat Ücretli. Her bir seyahat kayıt, toplama ve dropoff konumları ve süreleri, anonim hack (sürücü) lisans numarası ve medallion (taksi'nın benzersiz Tanımlayıcı) numarası içerir. Veriler tüm dönüş 2013 yılında kapsar ve aşağıdaki iki veri kümesi için her ay sağlanır:

'trip_data' CSV Yolcuların, toplama ve dropoff noktaları, seyahat süresini ve seyahat uzunluğu sayısı gibi seyahat ayrıntıları içerir. Birkaç örnek kayıt şunlardır:

       medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
       89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
       0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
       0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
       DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
       DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868



'trip_fare' CSV ödeme türü, taksi tutar, ek ücret ve vergiler, ipuçları ve Ücretli geçişler, gibi her seyahat için ücretli taksi ve ödenen toplam tutarı ayrıntılarını içerir. Birkaç örnek kayıt şunlardır:

       medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
       89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
       0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
       0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
       DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
       DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Seyahat katılmak için benzersiz anahtar\_veri ve seyahat\_taksi, aşağıdaki üç alanı oluşur: medallion hack\_lisans ve alma\_datetime. Genel Azure depolama blobundan ham CSV dosyalarından erişilebilir. Bu birleştirme için U-SQL betiği bulunduğu [birleştirme seyahat ve taksi tabloları](#join) bölümü.

## <a name="process-data-with-u-sql"></a>U-SQL ile verileri işleme
Bu bölümde gösterilen veri işleme görevlerini başlayan kümeniz, kalite denetimi, araştırma ve örnekleme verileri içerir. Seyahat ve taksi tabloların nasıl birleştirileceğini da gösterilir. Son bölümde, Azure portalından bir U-SQL komut dosyası iş çalıştırma gösterilmektedir. Her bir alt bağlantılar aşağıda verilmiştir:

* [Veri alımı: Genel blobdan veri okuma](#ingest)
* [Veri Kalitesi denetimleri](#quality)
* [Veri keşfi](#explore)
* [Seyahat ve taksi tabloları birleştirme](#join)
* [Veri örnekleme](#sample)
* [U-SQL işleri çalıştırma](#run)

U-SQL betikleri ayrı bir dosyada sağlanan ve burada açıklanmıştır. Tam indirebileceğiniz **U-SQL betikleri** gelen [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough).

U-SQL, Visual Studio'yu Aç çalıştırabileceği **Dosya--> Yeni Proje-->**, seçin **U-SQL projesi**, ad ve bir klasöre kaydedin.

![8](./media/data-lake-walkthrough/8-create-USQL-project.PNG)

> [!NOTE]
> U-SQL Visual Studio yerine yürütmek için Azure portalı kullanmak mümkündür. Portal'daki Azure Data Lake Analytics kaynağa gidin ve doğrudan olarak aşağıdaki şekilde Resimli sorguları gönderin:
>
>

![9](./media/data-lake-walkthrough/9-portal-submit-job.PNG)

### <a name="ingest"></a>Veri alımı: Genel blobdan veri okuma

Azure BLOB verilerin konumu olarak başvurulan **wasb://container\_adı\@blob\_depolama\_hesabı\_name.blob.core.windows.net/blob_name**ve alınabilir **Extractors.Csv()**. Yerine kendi kapsayıcı adı ve depolama hesabı adı kapsayıcısı için aşağıdaki komut\_adı\@blob\_depolama\_hesabı\_wasb adres adı. Dosya adları aynı biçimde olduğundan kullanmak mümkün mü **seyahat\_veri\_\{\*\}.csv** tüm 12 seyahat dosyaları okumak için.

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

İlk satırı üst bilgileri olduğundan, üst bilgileri kaldırın ve uygun parçalara sütun türleri değiştirmek gerekir. Yapabilirsiniz veya Azure Data Lake depolamanın kullanılması için işlenen verileri kaydetme **swebhdfs://data_lake_storage_name.azuredatalakestorage.net/folder_name/file_name**_ veya Azure Blob Depolama hesabı kullanarak **wasb: / / container_name\@blob_storage_account_name.blob.core.windows.net/blob_name**.

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

Benzer şekilde, taksi veri kümelerinde okuyabilirsiniz. Azure Data Lake Store sağ tıklayın, tercih edebilirsiniz verilerinizi bakmak **Azure portalında Veri Gezgini-->** veya **dosya Gezgini** Visual Studio içinden.

 ![10](./media/data-lake-walkthrough/10-data-in-ADL-VS.PNG)

 ![11](./media/data-lake-walkthrough/11-data-in-ADL.PNG)

### <a name="quality"></a>Veri Kalitesi denetimleri
Veri kalite denetimlerinden, seyahat ve taksi tabloları okunduktan sonra aşağıdaki şekilde gerçekleştirebilirsiniz. Sonuçta elde edilen CSV dosyaları Azure Blob Depolama veya Azure Data Lake Store için çıkış olabilir.

Medallions sayısını ve medallions benzersiz numarası bulun:

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

100'den fazla gelişlerin olan bu medallions bulun:

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

Geçersiz kayıtları pickup_longitude açısından bulun:

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
Bazı veri araştırması ile verileri daha iyi anlamak için aşağıdaki betikler yapın.

Eğimli ve eğimli gelişlerin dağıtımını bulun:

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

Sonlandırma değerleri ile ipucu miktarını dağıtımını bulun: 0, 5, 10 ve 20 ABD Doları.

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

Seyahat mesafenin temel istatistikleri bulun:

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

Seyahat uzaklık yüzdebirliklerini bulun:

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


### <a name="join"></a>Seyahat ve taksi tabloları birleştirme
Seyahat ve taksi tabloları medallion, hack_license ve pickup_time katılabilir.

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


Her düzeyde yolcular sayısı için kayıtları, ortalama ipucu tutar, varyans ipucu tutar, Eğimli dönüş yüzdesi sayısını hesaplayın.

    // contingency table
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

Ardından stratified örnekleme tarafından değişken ikili tip_class yapın:

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
U-SQL betikleri düzenlemeyi tamamladığınızda, Azure Data Lake Analytics hesabınızı kullanarak sunucuya gönderebilirsiniz. Tıklayın **Data Lake**, **işi Gönder**seçin, **Analytics hesabı**, seçin **paralellik**, tıklatıp **Gönder**  düğmesi.

 ![12](./media/data-lake-walkthrough/12-submit-USQL.PNG)

Projenin başarıyla derlendiğini, işinizin durumunu izlemek için Visual Studio'da görüntülenir. İş çalışmayı tamamladıktan sonra hatta iş yürütme işlemi yeniden ve iş verimliliğinizi artırmak için performans sorunu adımları bulun. Ayrıca, U-SQL işlerinizin durumunu denetlemek için Azure Portalı'na gidebilirsiniz.

 ![13](./media/data-lake-walkthrough/13-USQL-running-v2.PNG)

 ![14](./media/data-lake-walkthrough/14-USQL-jobs-portal.PNG)

Artık çıktı dosyaları veya Azure Blob Depolama, hem de Azure portalında kontrol edebilirsiniz. Sonraki adımda bizim modelleme stratified örnek verileri kullanın.

 ![15](./media/data-lake-walkthrough/15-U-SQL-output-csv.PNG)

 ![16](./media/data-lake-walkthrough/16-U-SQL-output-csv-portal.PNG)

## <a name="build-and-deploy-models-in-azure-machine-learning"></a>Azure Machine Learning modellerini Derleme ve dağıtma
İki seçenek için veri oluşturmak için Azure Machine Learning içine çekmek kullanılabilir ve

* İlk seçenek, kullandığınız bir Azure Blob için yazılan örneklenen verileri (içinde **veri örnekleme** Yukarıdaki adımı) ve Azure Machine learning'in modellerini Derleme ve dağıtma için Python'ı kullanın.
* İkinci seçenek bir Hive sorgusu kullanarak doğrudan Azure Data Lake verileri sorgulamak. Bu seçenek, yeni bir HDInsight kümesi oluşturma veya var olan bir HDInsight kümesine Azure Data Lake Storage NY taksi verileri nerede Hive tablolarını işaret kullanmak gerektirir.  Bu seçeneklerin, aşağıdaki bölümlerde ele alınmıştır.

## <a name="option-1-use-python-to-build-and-deploy-machine-learning-models"></a>1. seçenek: Machine learning modellerini Derleme ve dağıtma için Python kullanma
Python kullanarak makine öğrenme modellerini Derleme ve dağıtma için yerel makinenizde veya Azure Machine Learning Studio'da Jupyter not defteri oluşturun. Jupyter not defteri sağlanan [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough) keşfedin, veri, özellik Mühendisliği, modelleme ve dağıtım görselleştirmek için tam kodu içerir. Bu makalede, yalnızca modelleme ve dağıtım ele alınmaktadır.

### <a name="import-python-libraries"></a>Python kitaplıkları içeri aktarma
Örneği çalıştırmak için Jupyter Not Defteri veya Python, paketleri gereken aşağıdaki Python betik dosyası. Azure Machine Learning Not Defteri hizmeti kullanıyorsanız, bu paketlerin önceden yüklü olmuştur.

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


### <a name="read-in-the-data-from-blob"></a>Blobdan veri okuma
* Bağlantı Dizesi

        CONTAINERNAME = 'test1'
        STORAGEACCOUNTNAME = 'XXXXXXXXX'
        STORAGEACCOUNTKEY = 'YYYYYYYYYYYYYYYYYYYYYYYYYYYY'
        BLOBNAME = 'demo_ex_9_stratified_1_1000_copy.csv'
        blob_service = BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
* İçinde metin olarak okuyun.

        t1 = time.time()
        data = blob_service.get_blob_to_text(CONTAINERNAME,BLOBNAME).split("\n")
        t2 = time.time()
        print(("It takes %s seconds to read in "+BLOBNAME) % (t2 - t1))

  ![17](./media/data-lake-walkthrough/17-python_readin_csv.PNG)
* Sütun adları ekleyebilir ve ayrı sütunlar

        colnames = ['medallion','hack_license','vendor_id','rate_code','store_and_fwd_flag','pickup_datetime','dropoff_datetime',
        'passenger_count','trip_time_in_secs','trip_distance','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude',
        'payment_type', 'fare_amount', 'surcharge', 'mta_tax', 'tolls_amount',  'total_amount', 'tip_amount', 'tipped', 'tip_class', 'rownum']
        df1 = pd.DataFrame([sub.split(",") for sub in data], columns = colnames)
* Bazı sütunlar sayısal değiştirme

        cols_2_float = ['trip_time_in_secs','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude',
        'fare_amount', 'surcharge','mta_tax','tolls_amount','total_amount','tip_amount', 'passenger_count','trip_distance'
        ,'tipped','tip_class','rownum']
        for col in cols_2_float:
            df1[col] = df1[col].astype(float)

### <a name="build-machine-learning-models"></a>Makine öğrenimi modelleri oluşturma
Burada bir seyahat olmadığını Eğimli tahmin etmek için bir ikili sınıflandırma modeli oluşturun. Jupyter Not Defteri, diğer iki modeli bulabilirsiniz: sınıflı sınıflandırma ve regresyon modellerini.

* İlk scikit kullanılabilir işlevsiz değişkenler oluşturmanız gerekir-modelleri bilgi edinin

        df1_payment_type_dummy = pd.get_dummies(df1['payment_type'], prefix='payment_type_dummy')
        df1_vendor_id_dummy = pd.get_dummies(df1['vendor_id'], prefix='vendor_id_dummy')
* Modelleme için veri çerçevesi oluşturun

        cols_to_keep = ['tipped', 'trip_distance', 'passenger_count']
        data = df1[cols_to_keep].join([df1_payment_type_dummy,df1_vendor_id_dummy])

        X = data.iloc[:,1:]
        Y = data.tipped
* Eğitim ve 60-40 bölünmüş test

        X_train, X_test, Y_train, Y_test = train_test_split(X, Y, test_size=0.4, random_state=0)
* Eğitim kümesindeki Lojistik regresyon

        model = LogisticRegression()
        logit_fit = model.fit(X_train, Y_train)
        print ('Coefficients: \n', logit_fit.coef_)
        Y_train_pred = logit_fit.predict(X_train)

       ![c1](./media/data-lake-walkthrough/c1-py-logit-coefficient.PNG)
* Sınama veri kümesi Puanlama

        Y_test_pred = logit_fit.predict(X_test)
* Değerlendirme ölçümlerini Hesapla

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

### <a name="build-web-service-api-and-consume-it-in-python"></a>Web hizmeti API'si oluşturma ve Python kullanma
Makine öğrenimi modelinde, oluşturulduktan sonra kullanıma hazır hale getirmek istediğiniz. İkili Lojistik modeli kullanılan bir örnek burada. Scikit emin olun.-yerel makinenize sürümünde 0.15.1 olduğunu öğrenin. Azure Machine Learning studio kullanıyorsanız, bu konuda endişelenmeniz gerekmez.

* Azure Machine Learning studio ayarlarından çalışma kimlik bilgilerinizi bulun. Azure Machine Learning Studio'da tıklayın **ayarları** --> **adı** --> **yetkilendirme belirteçleri**.

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
* Web hizmeti kimlik bilgilerini alma

        url = predictNYCTAXI.service.url
        api_key =  predictNYCTAXI.service.api_key

        print url
        print api_key

        @services.service(url, api_key)
        @services.types(trip_distance = float, passenger_count = float, payment_type_dummy_CRD = float, payment_type_dummy_CSH=float,payment_type_dummy_DIS = float, payment_type_dummy_NOC = float, payment_type_dummy_UNK = float, vendor_id_dummy_CMT = float, vendor_id_dummy_VTS = float)
        @services.returns(float)
        def NYCTAXIPredictor(trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH,payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS ):
            pass
* Web hizmeti API'sini çağırın. Önceki adım 5-10 saniye beklemeniz gerekir.

        NYCTAXIPredictor(1,2,1,0,0,0,0,0,1)

       ![c4](./media/data-lake-walkthrough/c4-call-API.PNG)

## <a name="option-2-create-and-deploy-models-directly-in-azure-machine-learning"></a>2. seçenek: Oluşturma ve Azure Machine Learning modellerini doğrudan dağıtma
Azure Machine Learning Studio, doğrudan Azure Data Lake Store verileri okuyabilir ve modelleri oluşturup sonra kullanılabilir. Bu yaklaşım Azure Data Lake Store işaret eden bir Hive tablosu kullanır. Bu ayrı bir Azure HDInsight kümesi, sağlanması, gerektirir üzerinde Hive tablosu oluşturulur. Aşağıdaki bölümlerde bunun nasıl yapılacağı gösterilmektedir.

### <a name="create-an-hdinsight-linux-cluster"></a>Bir HDInsight Linux kümesi oluşturma
Bir HDInsight kümesi (Linux) oluşturma [Azure portalında](https://portal.azure.com). Ayrıntılar için bkz **Azure Data Lake Store erişimi olan bir HDInsight kümesi oluşturma** konusundaki [Azure portalını kullanarak Data Lake Store ile HDInsight kümesi oluşturma](../../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

 ![18](./media/data-lake-walkthrough/18-create_HDI_cluster.PNG)

### <a name="create-hive-table-in-hdinsight"></a>HDInsight Hive tablosu oluşturma
Şimdi Azure Machine Learning Studio'da, önceki adımda Azure Data Lake Store içinde depolanan verileri kullanarak HDInsight kümesinde kullanılmak üzere Hive tabloları oluşturun. Oluşturulan HDInsight kümesine gidin. Tıklayın **ayarları** --> **özellikleri** --> **küme AAD kimlik** --> **ADLS erişim**, Azure Data Lake Store hesabınızı okuma listesinde eklendiğinden emin olun, yazma ve yürütme hakları.

 ![19](./media/data-lake-walkthrough/19-HDI-cluster-add-ADLS.PNG)

Ardından **Pano** yanındaki **ayarları** düğme ve bir pencere açılır. Tıklayın **Hive görünümü** ve sayfanın sağ alt köşesinde görmelisiniz **sorgu Düzenleyicisi**.

 ![20](./media/data-lake-walkthrough/20-HDI-dashboard.PNG)

 ![21](./media/data-lake-walkthrough/21-Hive-Query-Editor-v2.PNG)

Bir tablo oluşturmak için aşağıdaki Hive komut yapıştırın. Bu şekilde Azure Data Lake Store başvurusu veri kaynağı konumu şudur: **adl://data_lake_store_name.azuredatalakestore.net:443/klasör_adı/file_name**.

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


Sorgu çalışmayı tamamladığında bu benzer bir sonuç görmeniz gerekir:

 ![22](./media/data-lake-walkthrough/22-Hive-Query-results.PNG)

### <a name="build-and-deploy-models-in-azure-machine-learning-studio"></a>Azure Machine Learning Studio'da modelleri oluşturma ve dağıtma
Derleme ve dağıtma ile Azure Machine Learning ipucu Ücretli olup olmadığını tahmin eden bir model artık hazırsınız. Bu ikili sınıflandırma içinde kullanılmaya hazır stratified örnek veriler (veya ipucu) sorun. (Tip_class) sınıflı sınıflandırma ve regresyon (tip_amount) kullanarak Tahmine dayalı modelleri de oluşturulabilen ve Azure Machine Learning Studio ile dağıtılan, ancak burada, yalnızca ikili sınıflandırma modelinde durumu işlemek nasıl gösterilir.

1. Azure Machine Learning studio kullanarak veri alma **verileri içeri aktarma** modülü, kullanılabilir **veri giriş ve çıkış** bölümü. Daha fazla bilgi için [verileri içeri aktarma modülü](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) başvuru sayfası.
2. Seçin **Hive sorgusu** olarak **veri kaynağı** içinde **özellikleri** paneli.
3. Aşağıdaki Hive betiği yapıştırın **Hive veritabanı sorgusu** Düzenleyicisi

        select * from nyc_stratified_sample;
4. URI, HDInsight kümesi (Bu Azure portalında bulunabilir), Hadoop kimlik bilgilerini, çıktı verilerini Azure depolama hesabı adı/anahtar/kapsayıcı adı ve konumu girin.

   ![23](./media/data-lake-walkthrough/23-reader-module-v3.PNG)

Hive tablosundaki verileri aşağıdaki şekilde gösterilen bir ikili sınıflandırma deneme okuma örneği:

 ![24](./media/data-lake-walkthrough/24-AML-exp.PNG)

Denemeyi oluşturulduktan sonra tıklayın **Web hizmetinin ayarı** --> **Tahmine dayalı Web hizmeti**

 ![25](./media/data-lake-walkthrough/25-AML-exp-deploy.PNG)

Otomatik olarak oluşturulan çalıştırmak sona erdiğinde deneme, Puanlama, tıklayın **Web hizmetini dağıtma**

 ![26](./media/data-lake-walkthrough/26-AML-exp-deploy-web.PNG)

Web hizmeti panosundaki kısa bir süre içinde görüntüler:

 ![27](./media/data-lake-walkthrough/27-AML-web-api.PNG)

## <a name="summary"></a>Özet
Bu izlenecek yolu tamamlayarak Azure Data lake'te ölçeklenebilir uçtan uca çözümler oluşturmak için bir veri bilimi ortamını oluşturdunuz. Bu ortam, veri alım modeli eğitimi aracılığıyla ve dağıtım modelinin bir web hizmeti olarak gelen Data Science Process kurallı adımları boyunca alma büyük bir genel veri kümesini, analiz etmek için kullanıldı. U-SQL işlemek için keşfetmek ve veri örnekleme için kullanıldı. Python ve Hive ile Azure Machine Learning Studio'da Tahmine dayalı modelleri oluşturma ve dağıtma için kullanıldı.

## <a name="whats-next"></a>Sırada ne var?
Öğrenme yolu [Team Data Science işlem (TDSP)](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/) Gelişmiş analiz işlemin her adımından açıklayan konulara bağlantılar sağlar. Bir dizi üzerinde listelenen izlenecek yollar mevcuttur [Team Data Science Process Kılavuzu](walkthroughs.md) kaynaklarını ve Hizmetleri, çeşitli Tahmine dayalı analiz senaryolarında kullanmak nasıl başlanacağını gösteren sayfa:

* [Team Data Science Process'in çalışması: SQL veri ambarı kullanma](sqldw-walkthrough.md)
* [Team Data Science Process'in çalışması: HDInsight Hadoop kümelerini kullanma](hive-walkthrough.md)
* [Team Data Science Process: SQL Server kullanma](sql-walkthrough.md)
* [Azure HDInsight üzerinde Spark'ı kullanarak Data Science Process genel bakış](spark-overview.md)
