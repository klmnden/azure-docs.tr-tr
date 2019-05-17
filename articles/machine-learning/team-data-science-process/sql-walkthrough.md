---
title: Derleme ve bir SQL Server VM - Team Data Science Process model dağıtma
description: Oluşturun ve makine öğrenme modeli herkese bir veri kümesi ile Azure VM'de SQL Server'ı kullanarak dağıtın.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/29/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 578f7a01c22bd5aafd4e4ac08c9f5ab78e340a34
ms.sourcegitcommit: 6ea7f0a6e9add35547c77eef26f34d2504796565
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65606521"
---
# <a name="the-team-data-science-process-in-action-using-sql-server"></a>Team Data Science Process'in çalışması: SQL Server'ı kullanma
Bu öğreticide, oluşturma ve SQL Server ve genel kullanıma açık bir veri kümesini kullanarak makine öğrenme modeli dağıtma sürecinde size yol-- [NYC taksi Gelişlerin](https://www.andresmh.com/nyctaxitrips/) veri kümesi. Standart veri bilimi iş akışı yordamdan sonraki: alma ve verileri, mühendislik işlevlerini, öğrenme süreçlerini kolaylaştırmasına sonra yapı ve model dağıtma keşfedin.

## <a name="dataset"></a>Veri kümesi tanımı NYC taksi Geçiren
NYC taksi seyahat verilerini yaklaşık 20 GB sıkıştırılmış CSV dosyalar (sıkıştırmadan ~ 48 GB), her seyahat için ücretli 173 milyondan fazla bireysel gelişlerin ve fares. Her bir seyahat kaydı alma ve dropoff konumu zaman, anonimleştirilmiş hack (sürücü) lisans numarası ve medallion (taksi'nın benzersiz tanımlayıcı) numarasını içerir. Veriler tüm dönüş 2013 yılında kapsar ve aşağıdaki iki veri kümesi için her ay sağlanır:

1. 'trip_data' CSV Yolcuların, toplama ve dropoff noktaları, seyahat süresini ve seyahat uzunluğu sayısı gibi seyahat ayrıntıları içerir. Birkaç örnek kayıt şunlardır:
   
        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
2. 'trip_fare' CSV ödeme türü, taksi tutar, ek ücret ve vergiler, ipuçları ve Ücretli geçişler, gibi her seyahat için ücretli taksi ve ödenen toplam tutarı ayrıntılarını içerir. Birkaç örnek kayıt şunlardır:
   
        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Seyahat katılmak için benzersiz anahtar\_veri ve seyahat\_taksi alanlarını oluşur: medallion hack\_lisans ve alma\_datetime.

## <a name="mltasks"></a>Tahmin görev örnekleri
Biz formüle göre üç tahmin sorunları *İpucu\_tutarı*, yani:

1. İkili sınıflandırma: İpucu için bir seyahat, yani ödenmiş olup olmadığını tahmin bir *İpucu\_tutarı* büyük pozitif bir örnek 0 TL'dir daha açıkken bir *İpucu\_tutarı* 0 veya negatif bir örnektir.
2. Sınıflı sınıflandırma: İpucu aralığını tahmin etmek için seyahat Ücretli. Biz bölmek *İpucu\_tutarı* beş depo veya sınıflar:
   
        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20
3. Regresyon. Görev: İpucu miktarı tahmin etmek için bir seyahat Ücretli.  

## <a name="setup"></a>Azure yedekleme ayarı veri bilimi ortamını Gelişmiş analiz
Gelen gördüğünüz gibi [ortamınızı planlama](plan-your-environment.md) Kılavuzu, Azure NYC taksi Gelişlerin veri kümesi ile çalışmak için birkaç seçenek vardır:

* Daha sonra Azure Machine learning'de model verileri Azure BLOB'ları ile çalışma
* Verileri bir SQL Server veritabanı sonra Azure Machine learning'de model yüklemek

Bu öğreticide verileri bir veri keşfi, özellik, bir SQL Server paralel toplu olarak içeri aktarma biz gösterilecektir mühendislik ve örnekleme aşağı SQL Server Management Studio kullanarak hem de Ipython Notebook kullanma. [Örnek betikleri](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) ve [Ipython not defterleri](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) Github'da paylaşılır. Bir örnek Ipython notebook'ın Azure bloblarındaki verilerden çalışmak için de aynı konumda kullanılabilir.

Azure veri bilimi ortamı oluşturmanız için:

1. [Depolama hesabı oluşturma](../../storage/common/storage-quickstart-create-account.md)
2. [Bir Azure Machine Learning çalışma alanı oluşturma](../studio/create-workspace.md)
3. [Bir veri bilimi sanal makine sağlama](../data-science-virtual-machine/setup-sql-server-virtual-machine.md), bir SQL sunucusu ve bir IPython dizüstü sunucusu sağlar.
   
   > [!NOTE]
   > Örnek betikler ve Ipython not defterleri Kurulum işlemi sırasında veri bilimi sanal makinenize indirilir. Örnekler, VM yükleme sonrası betik tamamlandığında, sanal makinenizin Belge Kitaplığı'nda şöyle olacaktır:  
   > 
   > * Örnek komutlar: `C:\Users\<user_name>\Documents\Data Science Scripts`  
   > * Örnek Ipython not defterleri: `C:\Users\<user_name>\Documents\IPython Notebooks\DataScienceSamples`  
   >   Burada `<user_name>` VM'NİZİN Windows oturum açma adıdır. Örnek klasörler olarak anılacaktır **örnek betikler** ve **örnek Ipython not defterleri**.
   > 
   > 

Veri kümesi boyutu, veri kaynağı konumu ve seçili Azure hedef ortama bağlı olarak, bu senaryo benzer [senaryo \#5: Büyük veri kümesinde bir yerel dosyalar, hedef Azure VM'de SQL Server](plan-sample-scenarios.md#largelocaltodb).

## <a name="getdata"></a>Genel kaynaktan veri alma
Alınacak [NYC taksi Gelişlerin](https://www.andresmh.com/nyctaxitrips/) veri kümesi genel konumundan kullanabilirsiniz açıklanan yöntemlerden herhangi birini [için ve Azure Blob Depolama'dan veri taşıma](move-azure-blob.md) verileri yeni sanal makinenize kopyalamak için.

AzCopy kullanarak verileri kopyalamak için:

1. Sanal makinenize (VM) oturum açın
2. Sanal makinenin veri disk yeni bir dizin oluşturma (Not: VM veri diski olarak ile birlikte gelen geçici Disk kullanmayın).
3. Bir komut istemi penceresinde aşağıdaki Azcopy komutunu (2)'de oluşturulan veri klasörünüz < path_to_data_folder > yerine satırı çalıştırın:
   
        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S
   
    AzCopy tamamlandığında 24 toplam CSV dosyaları sıkıştırılmış (gidiş dönüş için 12\_veri ve gidiş dönüş için 12\_taksi) veri klasöründe olmalıdır.
4. İndirilen dosyaları ayıklayın. Sıkıştırılması kaldırılan dosyaların bulunduğu klasör unutmayın. Bu klasör için olarak anılacaktır < yolu\_için\_veri\_dosyaları\>.

## <a name="dbload"></a>SQL Server veritabanına toplu verileri içeri aktar
Kullanarak yükleme ve aktarma büyük miktarda verileri bir SQL veritabanı ve sonraki sorgu performansı artırılabilir *bölümlenmiş tabloları ve görünümleri*. Bu bölümde, biz bölümünde açıklanan yönergeleri izler [paralel toplu veri alma kullanarak SQL bölüm tablolarından](parallel-load-sql-partitioned-tables.md) yeni bir veritabanı oluşturmak ve bölümlenmiş tabloları paralel verileri yüklemek için.

1. Sanal makinenizde oturum açmış durumdayken, başlangıç **SQL Server Management Studio**.
2. Windows kimlik doğrulaması kullanarak bağlanın.
   
    ![SSMS bağlanma][12]
3. Eğer henüz SQL Server kimlik doğrulama modu değiştirildi ve yeni bir SQL oturum açma kullanıcı oluşturulan adlı bir betik dosyasını açma **değiştirme\_auth.sql** içinde **örnek betikler** klasör. Varsayılan kullanıcı adı ve parolasını değiştirin. Tıklayın **! Yürütme** betiği çalıştırmak için araç.
   
    ![Komut dosyası yürütme][13]
4. Doğrulayın ve/veya veritabanlarını yeni oluşturulmuş bir veri diski depolanmış emin olmak için SQL Server varsayılan veritabanı ve günlük klasörleri değiştirme. Datawarehousing yükler için en iyi duruma getirilmiş SQL Server VM görüntüsü, veri ve günlük diskleri ile önceden yapılandırılmıştır. Sanal makinenizin veri diski dahil edilmemiştir ve yeni sanal sabit diskler VM Kurulum işlemi sırasında eklenen, varsayılan klasörler gibi değiştirin:
   
   * Sol bölmede SQL Server adını sağ tıklatıp **özellikleri**.
     
       ![SQL Server özellikleri][14]
   * Seçin **veritabanı ayarlarını** gelen **sayfa Seç** sol listesi.
   * Doğrulayın ve/veya değiştirmek **veritabanı varsayılan konumları** için **veri diski** tercih ettiğiniz konumları. Yeni veritabanları ile varsayılan konumu ayarları oluşturduysanız bulunduğu budur.
     
       ![SQL veritabanı Varsayılanları][15]  
5. Yeni bir veritabanı ve dosya grupları bölümlenmiş tabloları tutmak için bir dizi oluşturmak için örnek betiği açın **oluşturma\_db\_default.sql**. Betik adlı yeni bir veritabanı oluşturur **TaxiNYC** ve varsayılan veri konumda 12 dosya grupları. Her dosya grubu bir seyahat aylık tutar\_veri ve seyahat\_masrafları veri. Veritabanı adı, isterseniz değiştirin. Tıklayın **! Yürütme** betiği çalıştırmak için.
6. Ardından, bir gidiş dönüş için iki bölüm tablolarını oluşturmayı\_veri ve başka bir gidiş dönüş için\_taksi. Örnek betiği açın **oluşturma\_bölümlenmiş\_table.sql**, hangi olur:
   
   * Verileri aya göre bölmek için bölümleme işlevi oluşturun.
   * Her aya ait verileri farklı bir dosya grubuna eşlemek için bir bölüm düzeni oluşturun.
   * Bölüm düzeni için eşlenen iki bölümlenmiş tablolar oluşturun: **nyctaxi\_seyahat** seyahat tutacak\_veri ve **nyctaxi\_taksi** Seyahattutar\_masrafları veri.
     
     Tıklayın **! Yürütme** betiği çalıştırın ve bölümlenmiş tabloları oluşturun.
7. İçinde **örnek betikler** klasöründe paralel toplu içeri aktarır SQL Server tablolarına veri göstermek için sağlanan iki örnek PowerShell komut dosyası vardır.
   
   * **BCP\_paralel\_generic.ps1** paralel toplu veri tabloya aktarmak için genel bir betiktir. Komut dosyasındaki yorum satırlarında belirtildiği gibi giriş ve hedef değişkenlerini ayarlamak için bu betiği değiştirin.
   * **BCP\_paralel\_nyctaxi.ps1** genel komut dosyası, önceden yapılandırılmış bir sürümüdür ve her iki tablonun NYC taksi Gelişlerin veri yüklemek için kullanılabilir.  
8. Sağ **bcp\_paralel\_nyctaxi.ps1** betik adı ve tıklatın **Düzenle** PowerShell'de açmak için. Önceden ayarlanmış değişkenleri inceleyip seçili veritabanı adı, giriş veri klasörü, hedef günlük klasörü ve örnek biçim dosyalara olan yolları göre değiştirebilirsiniz **nyctaxi_trip.xml** ve **nyctaxi\_fare.xml** (sağlanan **örnek betikler** klasörü).
   
    ![Toplu veri içeri aktar][16]
   
    Kimlik doğrulama modu da seçebilirsiniz, varsayılan Windows kimlik doğrulaması. Çalıştırmak için araç çubuğundaki yeşil oka tıklayın. Betik 24 toplu içeri aktarma işlemlerinde paralel 12 bölümlenmiş her tablo için başlatılır. Veri içeri aktarma ilerlemesi kümesi olarak yukarıdaki SQL Server varsayılan veri klasörü açarak izleyebilirsiniz.
9. PowerShell Betiği, başlangıç ve bitiş saatleri bildirir. Tüm toplu içeri aktarmalar tamamlandı, bitiş saati bildirilir. Hedef günlük klasöründe hiçbir hata bildirilmemiş toplu içeri aktarmalar başka bir deyişle, başarılı doğrulamak için hedef günlük klasörü denetleyin.
10. Veritabanı keşfi, özellik Mühendisliği ve istediğiniz gibi diğer işlemler için artık hazırdır. Tabloları göre bölümlenir beri **toplama\_datetime** alan içeren sorgular **toplama\_datetime** koşulları **burada** yan tümcesi bölüm düzeni yararlanacaktır.
11. İçinde **SQL Server Management Studio**, sağlanan örnek betiği keşfedin **örnek\_queries.sql**. Herhangi bir örnek sorguları çalıştırmak için sorgu satırlarını vurgulayın ardından **! Yürütme** araç.
12. NYC taksi dönüş verileri iki ayrı tablolarda yüklenir. Birleştirme işlemleri iyileştirmek için dizin tabloları için önerilir. Örnek betik **oluşturma\_bölümlenmiş\_index.sql** bölümlenmiş dizinleri bileşik birleştirme anahtarı oluşturur **medallion hack\_lisans ve alma\_ DateTime**.

## <a name="dbexplore"></a>Veri keşfi ve SQL Server özellik Mühendisliği
Bu bölümde, biz veri keşfi ve özellik nesil doğrudan SQL sorguları çalıştırarak gerçekleştirir **SQL Server Management Studio** daha önce SQL Server veritabanı kullanılarak oluşturulmuş. Adlandırılmış bir örnek betiği **örnek\_queries.sql** sağlanan **örnek betikler** klasör. Varsayılandan farklı olması durumunda veritabanı adını değiştirmek için komut dosyasını değiştirin: **TaxiNYC**.

Bu alıştırmada yapacağız:

* Bağlanma **SQL Server Management Studio** ya da Windows kimlik doğrulaması veya SQL kimlik doğrulaması ve SQL oturum açma adı ve parola kullanarak.
* Değişen zaman pencereleri bazı alanların veri dağıtımları keşfedin.
* Veri Kalitesi boylam ve enlem alanlarının araştırın.
* İkili ve çok sınıflı sınıflandırma etiketlerine göre oluşturmak **İpucu\_tutarı**.
* Özellikleri oluşturma ve işlem/seyahat uzaklıkları karşılaştırın.
* İki tabloyu birleştirmek ve modelleri oluşturmak için kullanılan rastgele bir örnek ayıklayın.

Azure Machine Learning için devam etmeye hazır olduğunuzda, şunlardan birini yapabilirsiniz:  

1. Son SQL sorgusunun ayıklayın ve örnek veriler ve doğrudan sorgu kopyala-yapıştır kaydetmek bir [verileri içeri aktarma] [ import-data] modülü Azure Machine learning'de veya
2. Örneklenen kalıcı hale getirmek ve modeli yeni bir veritabanı oluşturmak için kullanmayı planladığınız tasarlanan veri tablosu ve yeni tablodaki kullanın [verileri içeri aktarma] [ import-data] Azure Machine learning'de modülü.

Bu bölümde biz ayıklamak ve veri örneği için en son sorgu kaydeder. İkinci yöntem gösterilmiştir [veri keşfi ve özellik Mühendisliği, Ipython Notebook](#ipnb) bölümü.

Daha önce paralel toplu olarak içeri aktarma kullanarak doldurulmuş tablo içindeki satırları ve sütunları sayısını hızlı bir doğrulama için

    -- Report number of rows in table nyctaxi_trip without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('nyctaxi_trip')

    -- Report number of columns in table nyctaxi_trip
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = 'nyctaxi_trip'

#### <a name="exploration-trip-distribution-by-medallion"></a>Keşfetme: Seyahat dağıtım medallion tarafından
Bu örnek, belirli bir süre içinde 100'den fazla gelişlerin medallion (taksi numaraları) tanımlar. Bölümleme düzeni koşuluna bağlıdır beri sorgu bölümlenmiş tabloda erişimden avantaj elde edecektir **toplama\_datetime**. Tam bir veri kümesinin sorgulanmasını ayrıca bölümlenmiş tablosunu kullanmak ve/veya dizin tarama yapar.

    SELECT medallion, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

#### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>Keşfetme: Seyahat dağıtım medallion ve hack_license
    SELECT medallion, hack_license, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

#### <a name="data-quality-assessment-verify-records-with-incorrect-longitude-andor-latitude"></a>Veri Kalitesi değerlendirmesi: Yanlış boylam ve/veya enlem kayıtlarla doğrulayın
Herhangi bir boylam ve/veya enlem alanı ya da geçersiz bir değer içeriyorsa, bu örnekte araştırır (radian derece -90 ile 90 arasında olmalıdır), veya (0, 0) koordinatları.

    SELECT COUNT(*) FROM nyctaxi_trip
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

#### <a name="exploration-tipped-vs-not-tipped-trips-distribution"></a>Keşfetme: Eğimli vs. Eğimli Gelişlerin dağıtım yok
Bu örnek karşılaştırması belirli bir zaman dönemi (veya kapsayan tam yıl, tam veri kümesi) Eğimli değil Eğimli dönüş sayısı bulur. Bu dağıtım için ikili sınıflandırma modelleme daha sonra kullanılmak üzere ikili etiket dağılımı yansıtır.

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM nyctaxi_fare
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

#### <a name="exploration-tip-classrange-distribution"></a>Keşfetme: Sınıf/aralığı dağıtım İpucu
Bu örnek, belirli bir süre içinde (veya kapsayan tam yıl, tam veri kümesi) dağıtım ipucu aralıklarının hesaplar. Daha sonra çok sınıflı sınıflandırma model için kullanılacak etiket sınıfların dağıtımıdır.

    SELECT tip_class, COUNT(*) AS tip_freq FROM (
        SELECT CASE
            WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tip_class

#### <a name="exploration-compute-and-compare-trip-distance"></a>Keşfetme: İşlem ve seyahat uzaklık karşılaştırın
Bu örnek dönüştürür alma ve dropoff boylam ve enlem SQL coğrafi konum için işaret, SQL Coğrafya noktaları fark kullanarak seyahat uzaklığı hesaplar ve sonuçları karşılaştırma için rastgele oluşturulmuş bir örnek döndürür. Bu örnek yalnızca daha önce veri kalitesi değerlendirme sorgusu kullanarak geçerli koordinat sonuçları sınırlar.

    SELECT
    pickup_location=geography::STPointFromText('POINT(' + pickup_longitude + ' ' + pickup_latitude + ')', 4326)
    ,dropoff_location=geography::STPointFromText('POINT(' + dropoff_longitude + ' ' + dropoff_latitude + ')', 4326)
    ,trip_distance
    ,computedist=round(geography::STPointFromText('POINT(' + pickup_longitude + ' ' + pickup_latitude + ')', 4326).STDistance(geography::STPointFromText('POINT(' + dropoff_longitude + ' ' + dropoff_latitude + ')', 4326))/1000, 2)
    FROM nyctaxi_trip
    tablesample(0.01 percent)
    WHERE CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND   CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'

#### <a name="feature-engineering-in-sql-queries"></a>SQL sorguları, özellik Mühendisliği
Etiket oluşturma ve geography dönüştürme araştırma sorgular sayım bölümü kaldırarak etiketleri/özellikler oluşturmak için de kullanılabilir. Ek özellik Mühendisliği SQL örnekleri verilmiştir [veri keşfi ve özellik Mühendisliği, Ipython Notebook](#ipnb) bölümü. Özellik tam veri kümesi veya doğrudan SQL Server veritabanı örneği üzerinde çalışan SQL sorgularını kullanarak büyük bir alt kümesi üzerinde üretimi sorguları çalıştırmak için daha verimlidir. İçinde sorguları yürütülebilecek **SQL Server Management Studio**, Ipython Notebook veya herhangi bir geliştirme aracı / veritabanı yerel olarak veya uzaktan erişebileceğiniz ortamında.

#### <a name="preparing-data-for-model-building"></a>Veri modeli oluşturma için hazırlanıyor
Aşağıdaki Sorguda birleştirme **nyctaxi\_seyahat** ve **nyctaxi\_taksi** tablolar, bir ikili sınıflandırma etiketi oluşturur **Eğimli**, çok sınıflı sınıflandırma etiketi **İpucu\_sınıfı**ve tam Birleşmiş kümesinden %1 rastgele örnek ayıklar. Bu sorgu kopyalanır, ardından doğrudan yapıştırılan [Azure Machine Learning Studio](https://studio.azureml.net) [verileri içeri aktarma] [ import-data] için SQL Server veritabanını doğrudan veri alma Modülü Azure örneği. Sorgu yanlış kayıtlarla dışlar (0, 0) koordinatları.

    SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,     f.total_amount, f.tip_amount,
        CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped,
        CASE WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM nyctaxi_trip t, nyctaxi_fare f
    TABLESAMPLE (1 percent)
    WHERE t.medallion = f.medallion
    AND   t.hack_license = f.hack_license
    AND   t.pickup_datetime = f.pickup_datetime
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'


## <a name="ipnb"></a>Veri keşfi ve özellik Mühendisliği, Ipython Notebook
Bu bölümde, biz veri keşfi ve daha önce oluşturduğunuz SQL Server veritabanında hem Python hem de SQL sorgularını kullanarak özellik nesil gerçekleştirir. Adlandırılmış bir örnek Ipython notebook **machine-Learning-data-science-process-sql-story.ipynb** sağlanan **örnek Ipython not defterleri** klasör. Bu not defteri de kullanılabilir [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks).

Büyük verilerle çalışırken Önerilen diziyi aşağıda verilmiştir:

* Verilerin küçük bir örnek, bir bellek içi veri çerçevesine okuyun.
* Bazı görselleştirmeler ve araştırmaları örneklenen verileri kullanarak gerçekleştirin.
* Örnek verileri kullanarak özellik Mühendisliği ile denemeler yapın.
* Büyük veri keşfi, veri işleme ve özellik Mühendisliği için doğrudan Azure VM'de SQL Server veritabanında SQL sorguları göndermek amacıyla Python'ı kullanın.
* Azure Machine Learning modeli yapı için kullanılacak örnek boyutunu karar verin.

Azure Machine Learning için devam etmeye hazır olduğunuzda, şunlardan birini yapabilirsiniz:  

1. Son SQL sorgusunun ayıklayın ve örnek veriler ve doğrudan sorgu kopyala-yapıştır kaydetmek bir [verileri içeri aktarma] [ import-data] Azure Machine learning'de modülü. Bu yöntem gösterilmiştir [yapı modeller Azure Machine learning'de](#mlmodel) bölümü.    
2. Model içinde yeni bir veritabanı tablosu oluşturmak için kullanmayı planladığınız örneklenen ve mühendislik uygulanan verileri kalıcı hale getirmenize ve ardından yeni tabloda kullanın [verileri içeri aktarma] [ import-data] modülü.

Birkaç veri keşfi, veri Görselleştirme ve örnekler mühendislik özelliğini aşağıda verilmiştir. Daha fazla örnek için bkz: örnek SQL Ipython not defteri **örnek Ipython not defterleri** klasör.

#### <a name="initialize-database-credentials"></a>Veritabanı kimlik bilgileri Başlat
Veritabanı bağlantı ayarlarınızı aşağıdaki değişkenleri başlatın:

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database server>

#### <a name="create-database-connection"></a>Veritabanı bağlantısı oluşturma
    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

#### <a name="report-number-of-rows-and-columns-in-table-nyctaxitrip"></a>Tablo nyctaxi_trip içindeki satırları ve sütunları rapor sayısı
    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('nyctaxi_trip')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('nyctaxi_trip')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

* Toplam satır sayısı 173179759 =  
* Toplam sütun sayısı = 14

#### <a name="read-in-a-small-data-sample-from-the-sql-server-database"></a>Okuma-içinde SQL Server veritabanından bir küçük veri örneği
    t0 = time.time()

    query = '''
        SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax,
            f.tolls_amount, f.total_amount, f.tip_amount
        FROM nyctaxi_trip t, nyctaxi_fare f
        TABLESAMPLE (0.05 PERCENT)
        WHERE t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
    '''

    df1 = pd.read_sql(query, conn)

    t1 = time.time()
    print 'Time to read the sample table is %f seconds' % (t1-t0)

    print 'Number of rows and columns retrieved = (%d, %d)' % (df1.shape[0], df1.shape[1])

Örnek tablo 6.492000 saniyedir okuma süresi  
Satır ve sütun sayısı, alınan = (84952, 21)

#### <a name="descriptive-statistics"></a>Açıklayıcı istatistikleri
Artık, örneklenen verileri araştırmak hazırsınız. Açıklayıcı istatistiklerini bakarak ile Başlat **seyahat\_uzaklık** (veya diğer) alanı:

    df1['trip_distance'].describe()

#### <a name="visualization-box-plot-example"></a>Görselleştirme: Kutu Diagram örneği
Sonraki Kutu Çizimi quantiles görselleştirmek seyahat uzaklığı için şu konuları

    df1.boxplot(column='trip_distance',return_type='dict')

![#1 Çiz][1]

#### <a name="visualization-distribution-plot-example"></a>Görselleştirme: Dağıtım Diagram örneği
    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![#2 Çiz][2]

#### <a name="visualization-bar-and-line-plots"></a>Görselleştirme: Çubuk ve çizgi çizimler
Bu örnekte, beş depo seyahat uzaklığı bin ve Gruplama sonuçları görselleştirebilirsiniz.

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

Biz de yukarıdaki bin dağıtım çubuğundaki çizim veya aşağıda gösterildiği gibi çizim satır

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![#3 Çiz][3]

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![#4 Çiz][4]

#### <a name="visualization-scatterplot-example"></a>Görselleştirme: Dağılım Grafiği örneği
Dağılım grafiğinde noktalara arasında göstereceğiz **seyahat\_zaman\_içinde\_saniye** ve **seyahat\_uzaklık** herhangi bir ilişki olup olmadığını görmek için

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![#6 Çiz][6]

Benzer şekilde biz arasındaki ilişkiyi kontrol edebilirsiniz **oranı\_kod** ve **seyahat\_uzaklık**.

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![#8 Çiz][8]

### <a name="sub-sampling-the-data-in-sql"></a>SQL verileri alt örnekleme
Veri modeli içinde oluşturma için hazırlanırken [Azure Machine Learning Studio](https://studio.azureml.net), üzerinde ya da karar verebilirsiniz **doğrudan verileri içeri aktarma modülü içinde kullanmak için SQL sorgu** veya tasarlanmış ve örneklenen verileri yeni bir kalıcı içinde kullanabileceğinizi tablo [verileri içeri aktarma] [ import-data] basit bir modülle **seçin * FROM <,\_yeni\_tablo\_adı >** .

Bu bölümde örnek ve mühendislik uygulanan verileri tutmak için yeni bir tablo oluşturacağız. Örnek model oluşturmak için doğrudan bir SQL sorgusunun sağlanan [veri keşfi ve özellik Mühendisliği SQL Server'da](#dbexplore) bölümü.

#### <a name="create-a-sample-table-and-populate-with-1-of-the-joined-tables-drop-table-first-if-it-exists"></a>Bir örnek oluşturun %1 ile birleştirilmiş tablolar doldurma ve tablo. Varsa Tablo ilk bırakın.
Bu bölümde, biz tabloları birleştirme **nyctaxi\_seyahat** ve **nyctaxi\_taksi**, %1 rastgele örnek ayıklayın ve yeni bir tablo adı örneklenen verileri kalıcı hale  **nyctaxi\_bir\_yüzde**:

    cursor = conn.cursor()

    drop_table_if_exists = '''
        IF OBJECT_ID('nyctaxi_one_percent', 'U') IS NOT NULL DROP TABLE nyctaxi_one_percent
    '''

    nyctaxi_one_percent_insert = '''
        SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount, f.total_amount, f.tip_amount
        INTO nyctaxi_one_percent
        FROM nyctaxi_trip t, nyctaxi_fare f
        TABLESAMPLE (1 PERCENT)
        WHERE t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
        AND   pickup_longitude <> '0' AND dropoff_longitude <> '0'
    '''

    cursor.execute(drop_table_if_exists)
    cursor.execute(nyctaxi_one_percent_insert)
    cursor.commit()

### <a name="data-exploration-using-sql-queries-in-ipython-notebook"></a>Veri keşfi Ipython Notebook SQL sorgularını kullanma
Bu bölümde, yukarıda oluşturduğumuz yeni tablodaki kalıcı %1 örnek verileri kullanarak veri dağıtımları inceleyeceğiz. Benzer araştırmaları kullanarak isteğe bağlı olarak, özgün tabloda kullanılarak gerçekleştirilebilir Not **TABLESAMPLE** örnek veya belirli bir zaman dönemi using sonuçlarını sınırlayarak araştırma sınırlamak için **toplama\_datetime** de gösterildiği gibi bölümler [veri keşfi ve özellik Mühendisliği SQL Server'da](#dbexplore) bölümü.

#### <a name="exploration-daily-distribution-of-trips"></a>Keşfetme: Dönüş günlük dağıtımı
    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-per-medallion"></a>Keşfetme: Seyahat dağıtım medallion başına
    query = '''
        SELECT medallion,count(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

### <a name="feature-generation-using-sql-queries-in-ipython-notebook"></a>SQL sorgularını Ipython Not Defteri kullanarak özellik oluşturma
Bu bölümde yeni etiketler oluşturacağız ve özellikler, doğrudan SQL sorguları kullanarak %1 örnek tablo üzerinde çalışan önceki bölümde oluşturduğumuz.

#### <a name="label-generation-generate-class-labels"></a>Etiket oluşturma: Sınıf etiketleri oluştur
Aşağıdaki örnekte, biz iki model için kullanılacak etiketleri oluşturun:

1. İkili sınıfı etiketleri **Eğimli** (bir ipucu verilip verilmeyeceğini tahmin)
2. Çok sınıflı etiketleri **İpucu\_sınıfı** (ipucu depo veya aralığını tahmin)
   
        nyctaxi_one_percent_add_col = '''
            ALTER TABLE nyctaxi_one_percent ADD tipped bit, tip_class int
        '''
   
        cursor.execute(nyctaxi_one_percent_add_col)
        cursor.commit()
   
        nyctaxi_one_percent_update_col = '''
            UPDATE nyctaxi_one_percent
            SET
               tipped = CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END,
               tip_class = CASE WHEN (tip_amount = 0) THEN 0
                                WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
                                WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
                                WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
                                ELSE 4
                            END
        '''
   
        cursor.execute(nyctaxi_one_percent_update_col)
        cursor.commit()

#### <a name="feature-engineering-count-features-for-categorical-columns"></a>Özellik Mühendisliği: Kategorik sütunlar için sayısı özellikleri
Bu örnek, bir kategorik alan veri alt yineleme sayısı ile her kategori değiştirerek bir sayısal alana dönüştürür.

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent ADD cmt_count int, vts_count int
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        WITH B AS
        (
            SELECT medallion, hack_license,
                SUM(CASE WHEN vendor_id = 'cmt' THEN 1 ELSE 0 END) AS cmt_count,
                SUM(CASE WHEN vendor_id = 'vts' THEN 1 ELSE 0 END) AS vts_count
            FROM nyctaxi_one_percent
            GROUP BY medallion, hack_license
        )

        UPDATE nyctaxi_one_percent
        SET nyctaxi_one_percent.cmt_count = B.cmt_count,
            nyctaxi_one_percent.vts_count = B.vts_count
        FROM nyctaxi_one_percent A INNER JOIN B
        ON A.medallion = B.medallion AND A.hack_license = B.hack_license
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="feature-engineering-bin-features-for-numerical-columns"></a>Özellik Mühendisliği: Sayısal sütunlara bin özellikleri
Bu örnekte önceden oluşturulmuş kategori aralıkları, yani, sayısal alana bir kategorik alanı dönüştürme sürekli bir sayısal alana dönüştürür.

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent ADD trip_time_bin int
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        WITH B(medallion,hack_license,pickup_datetime,trip_time_in_secs, BinNumber ) AS
        (
            SELECT medallion,hack_license,pickup_datetime,trip_time_in_secs,
            NTILE(5) OVER (ORDER BY trip_time_in_secs) AS BinNumber from nyctaxi_one_percent
        )

        UPDATE nyctaxi_one_percent
        SET trip_time_bin = B.BinNumber
        FROM nyctaxi_one_percent A INNER JOIN B
        ON A.medallion = B.medallion
        AND A.hack_license = B.hack_license
        AND A.pickup_datetime = B.pickup_datetime
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="feature-engineering-extract-location-features-from-decimal-latitudelongitude"></a>Özellik Mühendisliği: Ondalık enlem/boylam konumu özellikleri ayıklayın
Bu örnek bir enlem ve/veya boylam alan ondalık gösterimini farklı ayrıntı düzeyi, birden çok bölgeye alanlarına gibi keser ülke/bölge, şehir, şehir, blok, vb. Yeni coğrafi alanları gerçek konumlara eşlenmedi unutmayın. Eşleme coğrafi konumları hakkında daha fazla bilgi için bkz: [Bing Haritalar REST Hizmetleri](https://msdn.microsoft.com/library/ff701710.aspx).

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent
        ADD l1 varchar(6), l2 varchar(3), l3 varchar(3), l4 varchar(3),
            l5 varchar(3), l6 varchar(3), l7 varchar(3)
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        UPDATE nyctaxi_one_percent
        SET l1=round(pickup_longitude,0)
            , l2 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 1 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),1,1) ELSE '0' END     
            , l3 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 2 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),2,1) ELSE '0' END     
            , l4 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 3 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),3,1) ELSE '0' END     
            , l5 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 4 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),4,1) ELSE '0' END     
            , l6 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 5 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),5,1) ELSE '0' END     
            , l7 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 6 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),6,1) ELSE '0' END
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="verify-the-final-form-of-the-featurized-table"></a>Özellikleri tespit tablonun son biçimini doğrulayın
    query = '''SELECT TOP 100 * FROM nyctaxi_one_percent'''
    pd.read_sql(query,conn)

Model yapı ve model dağıtımı, devam etmeye hazır sunmaktayız [Azure Machine Learning](https://studio.azureml.net). Veri herhangi biri daha önce yani tanımlanan tahmin sorunları için hazırdır:

1. İkili sınıflandırma: Tahmin etmek için olup olmadığını bir ipucu için bir seyahat ödenmiş.
2. Sınıflı sınıflandırma: İpucu aralığını tahmin etmek için önceden tanımlanmış sınıfları göre Ücretli.
3. Regresyon. Görev: İpucu miktarı tahmin etmek için bir seyahat Ücretli.  

## <a name="mlmodel"></a>Azure Machine Learning modelleri oluşturma
Modelleme alıştırma başlamak için Azure Machine Learning çalışma alanına oturum açın. Machine learning çalışma alanı henüz oluşturmadıysanız, bkz: [bir Azure Machine Learning çalışma alanı oluşturma](../studio/create-workspace.md).

1. Azure Machine Learning ile çalışmaya başlamak için bkz: [Azure Machine Learning Studio nedir?](../studio/what-is-ml-studio.md)
2. Oturum [Azure Machine Learning Studio'yu](https://studio.azureml.net).
3. Studio giriş sayfası, modül başvurusu ve diğer kaynaklar için çok sayıda bilgi, videolar, öğreticiler, bağlantılar sağlar. Azure Machine Learning hakkında daha fazla bilgi için başvurun [Azure Machine Learning Belge Merkezi](https://azure.microsoft.com/documentation/services/machine-learning/).

Tipik eğitim denemesini aşağıdakilerden oluşur:

1. Oluşturma bir **+ yeni** denemeler yapın.
2. Azure Machine Learning için verileri elde edersiniz.
3. Ön işlem, dönüştürme ve gerektiği gibi verileri işlemek.
4. Özellikler, gerektiği gibi oluşturun.
5. Verileri eğitim/doğrulama/test kümelerine bölme (veya ayrı veri kümelerinde her biri için).
6. Bir veya daha fazla makine öğrenimi algoritmaları öğrenme problemi çözmek için bağlı olarak seçin. Örneğin, ikili Sınıflandırma, çok sınıflı Sınıflandırma, regresyon.
7. Eğitim veri kümesi kullanarak bir veya daha fazla modeller eğitin.
8. Eğitilen model kullanarak doğrulama dataset puan.
9. İlgili ölçümleri öğrenme problemi için işlem modellere değerlendirin.
10. İnce modellere ayarlayın ve dağıtmak için en iyi modeli seçin.

Bu alıştırmada biz zaten incelediniz ve SQL Server'daki verileri mühendislik ve örnek boyutuna, Azure Machine Learning'de alma karar verdi. Bir veya daha fazla verdik tahmin modellerini derlemek için:

1. Azure Machine Learning kullanarak verileri [verileri içeri aktarma] [ import-data] modülü, kullanılabilir **veri giriş ve çıkış** bölümü. Daha fazla bilgi için [verileri içeri aktarma] [ import-data] modül başvuru sayfası.
   
    ![Azure Machine Learning verileri içeri aktar][17]
2. Seçin **Azure SQL veritabanı** olarak **veri kaynağı** içinde **özellikleri** paneli.
3. Veritabanı bir DNS adı girmeniz **veritabanı sunucu adı** alan. Biçim: `tcp:<your_virtual_machine_DNS_name>,1433`
4. Girin **veritabanı adı** karşılık gelen alandaki.
5. Girin **SQL kullanıcı adı** içinde **Server kullanıcı hesabı adı**ve **parola** içinde **Server kullanıcı hesabı parolası**.
7. İçinde **veritabanı sorgusu** metin alanını düzenleme, gerekli veritabanı alanları (etiketler gibi tüm hesaplanan alanları dahil) ayıklayan sorguyu yapıştırın ve aşağı istenen örnek boyutu için verileri örnekler.

Aşağıdaki çizimde SQL Server veritabanından doğrudan verileri okuma bir ikili sınıflandırma deneme örneğidir. Benzer denemeleri, çok sınıflı sınıflandırma ve regresyon sorunları için oluşturulabilir.

![Azure Machine Learning eğitimi][10]

> [!IMPORTANT]
> Modelleme verileri ayıklama ve sorgu örnekleri örnekleme önceki kısımlarında sağlanan **üç modelleme alıştırmalar için tüm etiketleri sorguda bulunan**. Her model alıştırmalarda önemli bir (gerekli) adım **hariç** diğer iki sorunları ve diğer gereksiz etiketlerini **hedef sızıntılarını**. Örneğin, ikili sınıflandırma kullanırken kullanmak etiketi **Eğimli** ve alanları Dışla **İpucu\_sınıfı**, **İpucu\_tutar**ve **toplam\_tutarı**. Bunlar tip kapsıyor ikinci hedef sızıntılarını, çünkü Ücretli.
> 
> Gereksiz sütunların dışarıda ve/veya sızıntıları hedeflemek için kullanabilir [kümesindeki sütunları seçme] [ select-columns] modülü veya [meta verileri Düzenle][edit-metadata]. Daha fazla bilgi için [kümesindeki sütunları seçme] [ select-columns] ve [meta verileri Düzenle] [ edit-metadata] başvuru sayfalarına.
> 
> 

## <a name="mldeploy"></a>Azure Machine learning'de model dağıtma
Modeliniz hazır olduğunda, kolayca, denemeyi doğrudan bir web hizmeti olarak dağıtabilirsiniz. Azure Machine Learning web hizmetleri dağıtma hakkında daha fazla bilgi için bkz: [bir Azure Machine Learning web hizmetini dağıtma](../studio/publish-a-machine-learning-web-service.md).

Yeni bir web hizmeti dağıtmak için yapmanız:

1. Puanlama deneme oluşturun.
2. Web hizmeti dağıtın.

Puanlama bir deneyden oluşturmak için bir **tamamlandı** deneme eğitim, tıklayın **PUANLAMA DENEMELER oluşturma** alt eylem çubuğunda.

![Azure Puanlama][18]

Azure Machine Learning eğitim denemesini bileşenlerine göre bir Puanlama deneme oluşturmaya çalışır. Özellikle de yapar:

1. Eğitilen modeli kaydedin ve model eğitim modülleri kaldırın.
2. Bir mantıksal tanımlamak **giriş bağlantı noktasına** beklenen giriş verileri şemasını temsil etmek için.
3. Bir mantıksal tanımlamak **çıkış bağlantı noktasına** beklenen web hizmeti çıkış şeması temsil etmek için.

Puanlama denemeyi oluşturulduğunda gözden geçirin ve gerektiği gibi ayarlayın. Tipik bir düzeltme hizmeti çağrıldığında bu kullanılabilir olmayacak şekilde giriş veri kümesi ve/veya sorgu Bu etiket alanları dışlar biri ile değiştirmektir. Ayrıca bazı kayıtları için giriş veri kümesi ve/veya sorgu boyutunu azaltmak için iyi bir uygulama olan giriş şemasını göstermek yeterli. Çıkış bağlantı noktasına için tüm giriş alanları Dışla ve yalnızca dahil etmek için ortak olan **Puanlanmış etiketler** ve **Puanlanmış olasılıklar** çıkış kullanarak [kümesindekisütunlarıseçme] [ select-columns] modülü.

Deneme Puanlama bir örnek, aşağıdaki çizimde gösterilmektedir. Hazır olduğunuzda dağıtmak tıklayın **WEB hizmeti yayımlama** alt eylem çubuğunda düğme.

![Azure Machine Learning yayımlama][11]

Bu izlenecek yol öğreticide bilgilerin üzerinden geçelim için model eğitimi ve bir Azure Machine Learning web hizmeti dağıtımı için tüm veri alma büyük genel kümesinden ile birlikte çalışarak bir Azure veri bilimi ortamını oluşturdunuz.

### <a name="license-information"></a>Lisans bilgileri
Bu örnek gözden geçirme ve kendi eşlik eden betikleri ve Ipython notebook(s) paylaşılır Microsoft tarafından MIT lisansı altında. LICENSE.txt dosyasına örnek kod dizini github'da daha fazla ayrıntı için lütfen denetleyin.

### <a name="references"></a>Başvurular
• [Andrés Monroy NYC taksi Gelişlerin indirme sayfası](https://www.andresmh.com/nyctaxitrips/)  
• [FOILing NYC'ın taksi seyahat verilerini Chris Whong tarafından](https://chriswhong.com/open-data/foil_nyc_taxi/)   
• [NYC taksi ve Limousine komisyon araştırma ve istatistikleri](https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page)

[1]: ./media/sql-walkthrough/sql-walkthrough_26_1.png
[2]: ./media/sql-walkthrough/sql-walkthrough_28_1.png
[3]: ./media/sql-walkthrough/sql-walkthrough_35_1.png
[4]: ./media/sql-walkthrough/sql-walkthrough_36_1.png
[5]: ./media/sql-walkthrough/sql-walkthrough_39_1.png
[6]: ./media/sql-walkthrough/sql-walkthrough_42_1.png
[7]: ./media/sql-walkthrough/sql-walkthrough_44_1.png
[8]: ./media/sql-walkthrough/sql-walkthrough_46_1.png
[9]: ./media/sql-walkthrough/sql-walkthrough_71_1.png
[10]: ./media/sql-walkthrough/azuremltrain.png
[11]: ./media/sql-walkthrough/azuremlpublish.png
[12]: ./media/sql-walkthrough/ssmsconnect.png
[13]: ./media/sql-walkthrough/executescript.png
[14]: ./media/sql-walkthrough/sqlserverproperties.png
[15]: ./media/sql-walkthrough/sqldefaultdirs.png
[16]: ./media/sql-walkthrough/bulkimport.png
[17]: ./media/sql-walkthrough/amlreader.png
[18]: ./media/sql-walkthrough/amlscoring.png


<!-- Module References -->
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
