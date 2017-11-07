---
title: "Derleme ve bir Azure VM üzerinde SQL Server kullanarak bir makine öğrenimi modeline dağıtma | Microsoft Docs"
description: "Gelişmiş analizler işlemi ve eylem teknoloji"
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 6066b083-262c-4453-a712-a5c05acc3df8
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: fashah;bradsev
ms.openlocfilehash: d42377a55b1decc0918932b3ecc13cf575f934a9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="the-team-data-science-process-in-action-using-sql-server"></a>Eylem takım veri bilimi işleminde: SQL Server kullanma
Bu öğreticide, oluşturma ve SQL Server ve genel kullanıma açık bir veri kümesini kullanarak bir makine öğrenimi modeline dağıtma işleminde size kılavuzluk-- [NYC ücreti dönüşleri](http://www.andresmh.com/nyctaxitrips/) veri kümesi. Standart veri bilimi akışı yordamdan sonraki: alma ve verileri, öğrenme, kolaylaştırmak sonra yapı ve model dağıtmak için mühendislik özellikleri keşfedin.

## <a name="dataset"></a>NYC ücreti veri kümesi tanımı dönüşleri
NYC ücreti seyahat veri yaklaşık 20 GB sıkıştırılmış CSV dosyaları (~ 48 GB sıkıştırılmamış), her seyahat için ücretli 173 milyondan fazla tek tek dönüşleri ve fares. Her seyahat kayıt alma ve teslim konumu ve zaman, anonim korsan (sürücü) lisans numarası ve medallion (ücreti'nın benzersiz kimliği) numarasını içerir. Veri 2013 yıl içinde tüm dönüşleri kapsayan ve aşağıdaki iki veri kümelerinin her ay sağlanır:

1. 'trip_data' CSV yolcu, toplama ve dropoff noktaları, seyahat süresi ve seyahat Uzunluk sayısı gibi seyahat ayrıntıları içerir. Birkaç örnek kayıt şunlardır:
   
        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
2. 'trip_fare' CSV ödeme türü, ücreti tutarı, ek ücret ve vergileri, ipuçları ve tolls, gibi her seyahat için ödenen ücreti ve Ücretli toplam miktarı ayrıntılarını içerir. Birkaç örnek kayıt şunlardır:
   
        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Seyahat katılmak için benzersiz anahtar\_veri ve seyahat\_ücreti alanlarını oluşur: medallion, korsan\_lisans ve alma\_datetime.

## <a name="mltasks"></a>Tahmin görev örnekleri
Temel üç tahmin sorunları formüle *İpucu\_tutar*, yani:

1. İkili sınıflandırma: bir ipucu bir seyahat, yani ödenmiş olup olmadığına bakılmaksızın tahmin bir *İpucu\_tutar* büyük $0 pozitif bir örnektir küçük, ancak bir *İpucu\_tutar* 0 / negatif bir örnektir.
2. Çok sınıflı sınıflandırma: seyahat için ücretli ipucu aralığını tahmin etmek için. Biz bölmek *İpucu\_tutar* beş depo veya sınıfları:
   
        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20
3. Regresyon görevi: seyahat için ücretli ipucu miktarı tahmin etmek için.  

## <a name="setup"></a>Gelişmiş analiz için Azure yedekleme ayarı veri bilimi ortamı
Gelen gördüğünüz [ortamınıza planlama](plan-your-environment.md) Kılavuzu, Azure NYC ücreti dönüşleri kümesinde çalışmak için birkaç seçenek vardır:

* Azure BLOB'ları verilerde sonra Azure Machine Learning modeli ile çalışma
* SQL Server veritabanı sonra Azure Machine Learning modelinde içine veri yükleme

Bu öğreticide biz verilerin veri keşfi, özellik, bir SQL Server paralel toplu içeri gösterilmektedir mühendislik ve aşağı örnekleme SQL Server Management Studio'yu kullanarak yanı sıra IPython Not Defteri kullanarak. [Örnek komutlar](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) ve [IPython not defterlerini](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) Github'da paylaşılır. Azure BLOB'ları verilerle çalışmak için bir IPython dizüstü bilgisayar da aynı konumda mevcuttur.

Azure veri bilimi ortamınızı ayarlamak için:

1. [Depolama hesabı oluşturma](../../storage/common/storage-create-storage-account.md)
2. [Bir Azure Machine Learning çalışma alanı oluşturma](../studio/create-workspace.md)
3. [Bir veri bilimi sanal makine sağlama](../data-science-virtual-machine/setup-sql-server-virtual-machine.md), bir SQL sunucusu ve bir IPython dizüstü sunucusu sağlar.
   
   > [!NOTE]
   > Örnek komut dosyaları ve IPython not defterlerini veri bilimi sanal makinenize Kurulum işlemi sırasında yüklenir. VM yükleme sonrası betik tamamlandığında örnekleri VM Belge Kitaplığı'nda olur:  
   > 
   > * Örnek komut dosyaları:`C:\Users\<user_name>\Documents\Data Science Scripts`  
   > * Örnek IPython dizüstü bilgisayarlar:`C:\Users\<user_name>\Documents\IPython Notebooks\DataScienceSamples`  
   >   Burada `<user_name>` VM Windows oturum açma adıdır. Örnek klasörlere başvurur **örnek betikler** ve **örnek IPython not defterlerini**.
   > 
   > 

Veri kümesi boyutu, veri kaynağı konumu ve seçili Azure hedef ortam bağlı olarak, bu senaryo benzer [senaryo \#5: yerel dosyaları büyük bir veri kümesini hedef Azure VM'deki SQL Server](plan-sample-scenarios.md#largelocaltodb).

## <a name="getdata"></a>Ortak kaynaktan veri al
Almak için [NYC ücreti dönüşleri](http://www.andresmh.com/nyctaxitrips/) dataset ortak konumundan kullandığınız açıklanan yöntemlerden herhangi birini [veri taşımak için ve Azure Blob depolama biriminden](move-azure-blob.md) , yeni bir sanal makineye verileri kopyalamak için.

AzCopy kullanarak verileri kopyalamak için:

1. Sanal makine (VM) oturum açın
2. Sanal makinenin veri diski yeni bir dizin oluşturun (Not: bir veri diski olarak VM ile birlikte gelen geçici Disk kullanmayın).
3. Bir komut istemi penceresinde aşağıdaki Azcopy komutu (2)'de oluşturduğunuz veri klasörünüz < path_to_data_folder > yerine satırı çalıştırın:
   
        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S
   
    AzCopy tamamlandığında 24 toplam CSV dosyaları sıkıştırılmış (seyahat için 12\_veri ve seyahat için 12\_ücreti) veri klasörde olmalıdır.
4. İndirilen dosyaları ayıklayın. Sıkıştırılması kaldırılan dosyaların bulunduğu klasöre dikkat edin. Bu klasör için olarak anılacaktır < yol\_için\_veri\_dosyaları\>.

## <a name="dbload"></a>SQL Server veritabanına veri toplu İçeri Aktar
Yükleme ve aktarma büyük miktarlarda verinin bir SQL veritabanı ve sonraki sorgulara performansını kullanılarak geliştirilebilir *bölümlenmiş tabloları ve görünümleri*. Bu bölümde, biz bölümünde açıklanan yönergeleri izler [paralel toplu veri içeri aktarma kullanarak SQL bölüm tabloları](parallel-load-sql-partitioned-tables.md) yeni bir veritabanı oluşturmak ve verileri paralel bölümlenmiş tablolara yüklemek için.

1. VM'nize oturum açtığınızda Başlat **SQL Server Management Studio**.
2. Windows kimlik doğrulaması kullanarak bağlanın.
   
    ![SSMS bağlanma][12]
3. Eğer henüz SQL Server kimlik doğrulama modu değiştirildi ve yeni bir SQL oturum açma kullanıcı oluşturulan adlı betik dosyasını açma **değiştirme\_auth.sql** içinde **örnek betikler** klasör. Varsayılan kullanıcı adı ve parolasını değiştirin. Tıklatın **! Yürütme** komut dosyasını çalıştırmak için araç çubuğundaki.
   
    ![Komut dosyası yürütme][13]
4. Doğrulayın ve/veya veritabanlarını yeni oluşturulmuş bir veri diski depolanacak emin olmak için SQL Server varsayılan veritabanı ve günlük klasörleri değiştirin. Datawarehousing yükler için en iyi duruma getirilmiş SQL Server VM görüntüsü veri ve günlük disklerle önceden yapılandırılmıştır. VM'yi bir veri diski içermeyen ve yeni sanal sabit diskler VM Kurulum işlemi sırasında eklediyseniz varsayılan klasörler aşağıdaki gibi değiştirin:
   
   * Sol bölmede SQL Server adını sağ tıklatıp **özellikleri**.
     
       ![SQL Server özellikleri][14]
   * Seçin **veritabanı ayarlarını** gelen **sayfa Seç** sol listesi.
   * Doğrulayın ve/veya değiştirme **veritabanı varsayılan konumları** için **veri diski** tercih ettiğiniz konumları. Yeni veritabanları varsayılan konumu ayarlarını oluşturduysanız bulunduğu budur.
     
       ![SQL veritabanı Varsayılanları][15]  
5. Yeni bir veritabanı ve bölümlenmiş tabloları tutmak için dosya grupları kümesi oluşturmak için örnek komut dosyasını açın **oluşturma\_db\_default.sql**. Komut dosyası adlı yeni bir veritabanı oluşturacak **TaxiNYC** ve varsayılan veri konumu 12 grupları. Her dosya seyahat bir aylık tutacak\_veri ve seyahat\_masrafları veri. İsterseniz veritabanı adını değiştirin. Tıklatın **! Yürütme** komut dosyasını çalıştırmak için.
6. Ardından, iki bölüm tabloları seyahat için bir tane oluşturun\_veri ve seyahat için başka bir\_ücreti. Örnek komut dosyasını açın **oluşturma\_bölümlenmiş\_table.sql**, hangi olur:
   
   * Veri aya göre ayırmak için bir bölüm işlev oluşturun.
   * Her ayın verileri farklı bir dosya grubuna eşlemek için bir bölüm düzeni oluşturun.
   * Bölüm düzeni eşlenen iki bölümlenmiş tabloları oluşturma: **nyctaxi\_seyahat** seyahat tutacak\_veri ve **nyctaxi\_ücreti** seyahat tutacak\_masrafları veri.
     
     Tıklatın **! Yürütme** komut dosyasını çalıştırın ve bölümlenmiş tablolar oluşturun.
7. İçinde **örnek betikler** klasörü, paralel toplu alır, SQL Server tablolarını verilerinin göstermek için sağlanan iki örnek PowerShell komut dosyaları vardır.
   
   * **BCP\_paralel\_generic.ps1** bir tabloya paralel Toplu içe aktarma verilere genel bir komut dosyasıdır. Giriş ve hedef değişkenleri komut dosyasındaki yorum satırlarda belirtildiği şekilde ayarlamak için bu komut dosyasını değiştirin.
   * **BCP\_paralel\_nyctaxi.ps1** genel komut dosyası, önceden yapılandırılmış bir sürümüdür ve çok NYC ücreti dönüşleri veriler için her iki tabloyu yüklemek için kullanılabilir.  
8. Sağ **bcp\_paralel\_nyctaxi.ps1** betik adı ve tıklatın **Düzenle** PowerShell'de açmak için. Hazır değişkenleri gözden geçirin ve seçili veritabanı adı, giriş veri klasörü, hedef günlük klasörü ve örnek biçim dosyalarını yollara göre değiştirin **nyctaxi_trip.xml** ve **nyctaxi\_fare.xml** (sağlanan **örnek betikler** klasörü).
   
    ![Toplu Veri Al][16]
   
    Kimlik doğrulama modu da seçebilirsiniz, varsayılan Windows kimlik doğrulaması. Çalıştırmak için araç çubuğundaki yeşil ok simgesine tıklayın. Komut dosyası 24 toplu içeri aktarma işlemleri paralel 12 bölümlenmiş her tablo için başlatır. Veri alma ilerlemesini izlemek için yukarıdaki kümesi olarak SQL Server varsayılan veri klasörü açarak.
9. PowerShell komut dosyası başlangıç ve bitiş saatleri raporlar. Tüm içeri aktarmalar tam toplu, bitiş saati bildirilir. Hedef günlük klasöründe herhangi bir hata bildirdi Toplu Almalar başka bir deyişle, başarılı olduğunu doğrulamak için hedef günlük klasörü denetleyin.
10. Veritabanınızı artık araştırması, özellik Mühendisliği ve istediğiniz gibi diğer işlemler için hazırdır. Tabloları göre bölümlenir beri **toplama\_datetime** alan, içeren sorguları **toplama\_datetime** koşulları **burada** yan tümcesi, bölüm düzeni yararlanacaktır.
11. İçinde **SQL Server Management Studio**, sağlanan örnek komut dosyasını keşfedin **örnek\_queries.sql**. Herhangi bir örnek sorguları çalıştırmak için sorgu satırlarını vurgulayın ve ardından tıklatın **! Yürütme** araç.
12. NYC ücreti dönüşleri veriler iki ayrı tablolarda yüklenir. Birleştirme işlemleri artırmak için tablolar dizini oluşturmak için önerilir. Örnek komut dosyasını **oluşturma\_bölümlenmiş\_index.sql** bölümlenmiş dizinlerini bileşik birleştirme anahtarı oluşturur **medallion, korsan\_lisans ve alma\_datetime**.

## <a name="dbexplore"></a>Veri keşfi ve SQL Server'daki özelliği mühendislik
Bu bölümde, veri keşfi ve özellik nesil SQL sorguları doğrudan çalıştırarak gerçekleştiririz **SQL Server Management Studio** SQL Server veritabanı kullanılarak oluşturulan önceki. Adlandırılmış bir örnek komut dosyası **örnek\_queries.sql** sağlanan **örnek betikler** klasör. Varsayılandan farklı ise, veritabanı adını değiştirmek için komut dosyasını değiştirin: **TaxiNYC**.

Bu alıştırmada, şunları yapacağız:

* Bağlanmak **SQL Server Management Studio** ya da Windows kimlik doğrulaması veya SQL kimlik doğrulaması ve SQL oturum açma adı ve parola kullanarak.
* Veri dağıtımları değişen zaman pencereleri birkaç alanların keşfedin.
* Veri Kalitesi boylam ve enlem alanlarının araştırın.
* Temel çok sınıflı ve ikili sınıflandırma etiketleri oluşturmak **İpucu\_tutar**.
* Özellikler oluşturmak ve işlem/seyahat uzaklıklar karşılaştırın.
* İki tablo birleştirme ve modelleri oluşturmak için kullanılan rastgele bir örnek ayıklayın.

Azure Machine Learning ile devam etmeye hazır olduğunuzda ya da görebilirsiniz:  

1. Ayıklamak ve veri örneği ve kopyala-yapıştır doğrudan sorgu için en son SQL sorgu Kaydet bir [veri içeri aktarma] [ import-data] Azure Machine Learning modülünde veya
2. Örneklenen kalıcı tasarlanan veri modeli içindeki yeni bir veritabanı oluşturmak için kullanmayı düşünüyorsanız tablo yeni tabloda kullanılacağını ve [veri içeri aktarma] [ import-data] Azure Machine Learning modülünde.

Bu bölümde biz ayıklamak ve veri örneği için en son sorgu kaydeder. İkinci yöntem örneklerde gösterildiği [veri keşfi ve özellik Mühendisliği IPython Not](#ipnb) bölümü.

Satır ve sütun sayısı, daha önce paralel toplu içeri kullanarak doldurulmuş tablolardaki hızlı doğrulanması için

    -- Report number of rows in table nyctaxi_trip without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('nyctaxi_trip')

    -- Report number of columns in table nyctaxi_trip
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = 'nyctaxi_trip'

#### <a name="exploration-trip-distribution-by-medallion"></a>Araştırması: Medallion seyahat dağıtım
Bu örnek, belirli bir süre içinde ile 100'den fazla dönüşleri medallion (ücreti sayılar) tanımlar. Bölümleme düzeni söylediğinizde beri sorgu bölümlenmiş tabloda erişimden yararlanabileceğini **toplama\_datetime**. Tam veri kümesini sorgulama bölümlenmiş tablosu kullanın ve/veya dizin tarama ayrıca olun.

    SELECT medallion, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

#### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>Keşfetme: dağıtım medallion ve hack_license tarafından seyahat
    SELECT medallion, hack_license, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

#### <a name="data-quality-assessment-verify-records-with-incorrect-longitude-andor-latitude"></a>Veri Kalitesi değerlendirmesi: yanlış boylam ve/veya enlem ile kayıtlarını doğrulama
Boylam ve/veya enlem alanların herhangi birini ya da geçersiz bir değer içeriyorsa, bu örnek araştırır (radian derece -90 ile 90 arasında olmalıdır), ya da sahip (0, 0) koordinatları.

    SELECT COUNT(*) FROM nyctaxi_trip
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

#### <a name="exploration-tipped-vs-not-tipped-trips-distribution"></a>Keşfetme: Eğimli vs. Değil Eğimli dönüşleri dağıtım
Bu örnek karşılaştırması belirli bir zaman aralığı (veya tam yıl kapsayan varsa tam veri kümesi) Eğimli değil Eğimli dönüş sayısı bulur. Bu dağıtım için ikili sınıflandırma modelleme daha sonra kullanılmak üzere ikili etiket dağıtım yansıtır.

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM nyctaxi_fare
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

#### <a name="exploration-tip-classrange-distribution"></a>Araştırması: Sınıf/Range dağıtım İpucu
Bu örnek, belirli bir süre içinde (veya tam yıl kapsayan varsa tam veri kümesi) dağıtım ipucu aralıklarının hesaplar. Daha sonra çok sınıflı sınıflandırma model için kullanılacak etiket sınıfları dağıtımını budur.

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

#### <a name="exploration-compute-and-compare-trip-distance"></a>Keşfi: İşlem ve seyahat uzaklığı Karşılaştır
Bu örnek alma ve teslim boylam dönüştürür ve SQL Coğrafya enlem noktaları, SQL Coğrafya noktaları fark kullanarak seyahat uzaklığı hesaplar ve sonuçları karşılaştırma için rastgele bir örneğini döndürür. Bu örnek yalnızca daha önce ele alınan veri kalitesi değerlendirme sorgusu kullanılarak geçerli koordinatları sonuçları sınırlar.

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

#### <a name="feature-engineering-in-sql-queries"></a>Özellik Mühendisliği SQL sorguları
Etiket oluşturma ve Coğrafya dönüştürme araştırması sorguları sayım bölümü kaldırarak etiketleri/özellikleri oluşturmak için de kullanılabilir. Ek özellik Mühendisliği SQL örnekleri verilmiştir [veri keşfi ve özellik Mühendisliği IPython Not](#ipnb) bölümü. Özelliği, veri kümesinin tamamına veya büyük bir alt kümesini doğrudan SQL Server veritabanı örneğinde çalışan SQL sorgularını kullanarak nesil sorguları çalıştırmak için daha verimli olur. İçinde sorguları yürütülebilecek **SQL Server Management Studio**, IPython Not Defteri veya herhangi bir geliştirme aracı / veritabanı yerel olarak veya uzaktan erişebileceğiniz ortamında.

#### <a name="preparing-data-for-model-building"></a>Model oluşturmanın için verileri hazırlama
Aşağıdaki sorgu birleştirmeler **nyctaxi\_seyahat** ve **nyctaxi\_ücreti** tabloları, ikili sınıflandırma etiketini oluşturur **Eğimli**, birden çok sınıf sınıflandırma etiketini **İpucu\_sınıfı**ve tam birleştirilmiş kümesinden %1 rasgele örnek ayıklar. Bu sorgu kopyalanır, ardından doğrudan yapıştırılan [Azure Machine Learning Studio](https://studio.azureml.net) [veri içeri aktarma] [ import-data] azure'da SQL Server veritabanı örneğinden doğrudan veri alımı için modülü. Sorgu yanlış kayıtlarıyla dışlar (0, 0) koordinatları.

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


## <a name="ipnb"></a>Veri keşfi ve özellik Mühendisliği IPython Not
Bu bölümde, biz veri keşfi ve daha önce oluşturulan SQL Server veritabanı Python ve SQL sorguları kullanarak özellik oluşturma işlemini gerçekleştirecek. Adlandırılmış bir örnek IPython not defteri **machine-Learning-data-science-process-sql-story.ipynb** sağlanan **örnek IPython not defterlerini** klasör. Bu dizüstü bilgisayar da kullanılabilir [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks).

Büyük verilerle çalışırken, önerilen sırası aşağıda verilmiştir:

* Veri küçük bir örnek, bir bellek içi veri çerçeveye okuyun.
* Bazı Görselleştirme ve explorations örneklenen verileri kullanarak gerçekleştirin.
* Örneklenen verileri kullanarak özellik Mühendisliği ile deneyin.
* Büyük veri keşfi, veri işleme ve özellik Mühendisliği, Python Azure VM'de SQL Server veritabanında karşı doğrudan SQL sorguları göndermek için kullanın.
* Azure Machine Learning modeli oluşturma için kullanılacak örnek boyutunu karar verin.

Azure Machine Learning ile devam etmeye hazır olduğunuzda, ya da görebilirsiniz:  

1. Ayıklamak ve veri örneği ve kopyala-yapıştır doğrudan sorgu için en son SQL sorgu Kaydet bir [veri içeri aktarma] [ import-data] Azure Machine Learning modülünde. Bu yöntem örneklerde gösterildiği [Azure Machine Learning yapı modellerinde](#mlmodel) bölümü.    
2. Yeni bir veritabanı tablosunda derleme modeli için kullanmayı düşünüyorsanız örneklenen ve tasarlanan verilerini kalıcı sonra yeni tabloda kullanın [veri içeri aktarma] [ import-data] modülü.

Birkaç veri keşfi, veri Görselleştirme ve örnekler mühendislik özelliği verilmiştir. Örnek SQL IPython not defterinde daha fazla örnek için bkz: **örnek IPython not defterlerini** klasör.

#### <a name="initialize-database-credentials"></a>Veritabanı kimlik bilgileri başlatma
Aşağıdaki değişkenler, veritabanı bağlantı ayarlarınızı başlatın:

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database server>

#### <a name="create-database-connection"></a>Veritabanı bağlantısı oluşturma
    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

#### <a name="report-number-of-rows-and-columns-in-table-nyctaxitrip"></a>Tablo nyctaxi_trip sayfasındaki satır ve sütunları rapor sayısı
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

#### <a name="read-in-a-small-data-sample-from-the-sql-server-database"></a>Okuma-içinde SQL Server veritabanından küçük veri örneği
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
Satır ve sütunların sayısı, alınan = (84952, 21)

#### <a name="descriptive-statistics"></a>Açıklayıcı istatistikleri
Şimdi, örneklenen verileri araştırmak hazırsınız. Açıklayıcı istatistiklerini bakarak ile başlangıç **seyahat\_uzaklığı** (veya diğer) alan:

    df1['trip_distance'].describe()

#### <a name="visualization-box-plot-example"></a>Görselleştirme: Kutusu çizim örneği
Sonraki quantiles görselleştirmek seyahat uzaklığı Kutu Çizimi ele

    df1.boxplot(column='trip_distance',return_type='dict')

![#1 Çiz][1]

#### <a name="visualization-distribution-plot-example"></a>Görselleştirme: Dağıtım çizim örneği
    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![#2 Çiz][2]

#### <a name="visualization-bar-and-line-plots"></a>Görselleştirme: Çubuğu ve satır çizimleri
Bu örnekte, beş depo içine seyahat uzaklığı bin ve binning sonuçları görselleştirebilirsiniz.

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

Biz çubuğundaki yukarıdaki depo dağıtım çizmek veya aşağıdaki şekilde çizim satır

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![#3 Çiz][3]

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![#4 Çiz][4]

#### <a name="visualization-scatterplot-example"></a>Görselleştirme: Scatterplot örneği
Dağılım çizim arasında gösteriyoruz **seyahat\_zaman\_içinde\_saniye** ve **seyahat\_uzaklığı** herhangi bağıntı olup olmadığını görmek için

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![#6 Çiz][6]

Benzer şekilde biz arasındaki ilişkiyi kontrol edebilirsiniz **oranı\_kod** ve **seyahat\_uzaklığı**.

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![#8 Çiz][8]

### <a name="sub-sampling-the-data-in-sql"></a>SQL veri alt örnekleme
Veri modeli oluşturmaya için hazırlarken [Azure Machine Learning Studio](https://studio.azureml.net), üzerinde ya da karar verebilirsiniz **doğrudan veri içeri aktarma modül kullanmak için SQL sorgusu** veya içinde kullanabilen yeni bir tabloda tasarlanan ve örneklenen verileri kalıcı [veri içeri aktarma] [ import-data] basit bir modülüyle **seçin * FROM <,\_yeni\_tablo\_adı >**.

Bu bölümde örneklenen ve tasarlanan verileri tutmak için yeni bir tablo oluşturacağız. Model oluşturmanın için doğrudan bir SQL sorgusu örneği sağlanan [veri keşfi ve özellik Mühendisliği SQL Server'daki](#dbexplore) bölümü.

#### <a name="create-a-sample-table-and-populate-with-1-of-the-joined-tables-drop-table-first-if-it-exists"></a>Bir örnek oluşturmak tablo ve birleştirilmiş tablolar, %1 ile doldurabilirsiniz. Varsa Tablo ilk bırakın.
Bu bölümde, biz tabloları birleştirme **nyctaxi\_seyahat** ve **nyctaxi\_ücreti**%1 rasgele örnek ayıklamak ve yeni bir tablo adı örneklenen verileri kalır **nyctaxi\_bir\_yüzde**:

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

### <a name="data-exploration-using-sql-queries-in-ipython-notebook"></a>Veri keşfi IPython not defterinde SQL sorgularını kullanma
Bu bölümde, yukarıda oluşturduğumuz yeni tablodaki kalıcı %1 örneklenen verileri kullanarak veri dağıtımları keşfedin. Benzer explorations isteğe bağlı olarak kullanarak özgün tabloları kullanılarak gerçekleştirilebilir Not **TABLESAMPLE** örnek veya belirli bir zaman dönemi using sonuçlarını sınırlayarak araştırması sınırlamak için **toplama\_datetime** de gösterildiği gibi bölümler [veri keşfi ve özellik Mühendisliği SQL Server'daki](#dbexplore) bölümü.

#### <a name="exploration-daily-distribution-of-trips"></a>Araştırması: Dönüş günlük dağıtımını
    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-per-medallion"></a>Keşfetme: medallion başına dağıtım seyahat
    query = '''
        SELECT medallion,count(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

### <a name="feature-generation-using-sql-queries-in-ipython-notebook"></a>IPython not defterinde SQL sorgularını kullanarak özelliği oluşturma
Bu bölümde yeni etiketler oluşturacağız ve özellikleri, doğrudan SQL sorgularını kullanarak %1 örnek tabloda işletim önceki bölümde oluşturduğumuz.

#### <a name="label-generation-generate-class-labels"></a>Etiket oluşturma: Sınıf etiketleri oluşturmak
Aşağıdaki örnekte, iki model için kullanılacak etiket oluşturur:

1. İkili sınıfı etiketleri **Eğimli** (bir ipucu verilir tahmin etmeye)
2. Çok sınıflı etiketleri **İpucu\_sınıfı** (tahmin etmeye ipucu depo veya aralık)
   
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

#### <a name="feature-engineering-count-features-for-categorical-columns"></a>Özellik Mühendisliği: Kategorik sütunlar sayısı özellikleri
Bu örnek veri kendi yineleme sayısı ile her kategori değiştirerek kategorik alan bir sayısal alana dönüştürür.

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

#### <a name="feature-engineering-bin-features-for-numerical-columns"></a>Özellik Mühendisliği: Sayısal sütunlar Bin özellikleri
Bu örnek sürekli bir sayısal alana hazır kategori aralıkları, yani, sayısal alana kategorik alan dönüştürme dönüştürür.

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

#### <a name="feature-engineering-extract-location-features-from-decimal-latitudelongitude"></a>Özellik Mühendisliği: Ondalık enlem/boylam konumu özelliklerinden Extract
Bu örnek bir enlem ve/veya boylam alan ondalık gösterimini farklı bir ayrıntı düzeyi birden çok bölgede alanlarına gibi keser ülke, şehir, şehir, engelleme, vb.. Yeni coğrafi alanlar gerçek konumlara eşlenmemiş unutmayın. Eşleme geocode konumları hakkında daha fazla bilgi için bkz: [Bing Haritalar REST Hizmetleri](https://msdn.microsoft.com/library/ff701710.aspx).

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

#### <a name="verify-the-final-form-of-the-featurized-table"></a>Son formun featurized tablosunun doğrulayın
    query = '''SELECT TOP 100 * FROM nyctaxi_one_percent'''
    pd.read_sql(query,conn)

Biz şimdi modeli yapı ve model dağıtımda devam etmeye hazır [Azure Machine Learning](https://studio.azureml.net). Verileri herhangi bir önceki sürümlerinde, yani tanımlanan tahmin sorunları için hazırdır:

1. İkili sınıflandırma: tahmin etmek için olsun veya olmasın bir ipucu Ücretli bir seyahat için.
2. Çok sınıflı sınıflandırma: aralık, önceden tanımlanmış sınıfları göre Ücretli ucunun tahmin etmek için.
3. Regresyon görevi: seyahat için ücretli ipucu miktarı tahmin etmek için.  

## <a name="mlmodel"></a>Azure Machine Learning oluşturma modelleri
Modelleme alıştırma başlamak için Azure Machine Learning çalışma alanına oturum açın. Machine learning çalışma alanı henüz oluşturmadıysanız, bkz: [bir Azure Machine Learning çalışma alanı oluşturma](../studio/create-workspace.md).

1. Azure Machine Learning ile çalışmaya başlamak için bkz: [Azure Machine Learning Studio nedir?](../studio/what-is-ml-studio.md)
2. Oturum [Azure Machine Learning Studio](https://studio.azureml.net).
3. Studio giriş sayfası modülleri başvurusu ve diğer kaynaklara bol miktarda bilgi, videoları, öğreticiler, bağlantılar sağlar. Azure Machine Learning hakkında daha fazla bilgi için başvurun [Azure Machine Learning Belge Merkezi](https://azure.microsoft.com/documentation/services/machine-learning/).

Tipik eğitim denemenizi aşağıdakilerden oluşur:

1. Oluşturma bir **+ yeni** deneyin.
2. Azure Machine Learning için verileri alın.
3. Önceden işlemek, dönüştürme ve gerektiği gibi verileri işlemek.
4. Özellikler gerektiği şekilde oluşturun.
5. Verileri eğitim/doğrulama/test kümelerine bölme (veya ayrı veri kümeleri her).
6. Bir veya daha fazla makine öğrenimi algoritmalarını öğrenme sorunu çözmek için bağlı olarak seçin. Örneğin, ikili Sınıflandırma, çok sınıflı Sınıflandırma, regresyon.
7. Eğitim veri kümesini kullanarak bir veya daha fazla modelleri eğitme.
8. Eğitilmiş modellerini kullanarak doğrulama dataset puan.
9. Öğrenme sorun için ilgili ölçümlerini hesaplamak için modellere değerlendirin.
10. İnce modellere ayarlamak ve dağıtmak için en iyi modeli seçin.

Bu alıştırmada, biz varsa zaten incelediniz ve SQL Server verileri mühendislik ve Azure Machine Learning alımı için örnek boyutu karar. Bir veya daha fazla karar tahmin modelleri oluşturmak için:

1. Azure Machine Learning kullanarak verileri [veri içeri aktarma] [ import-data] modülü, kullanılabilir **veri giriş ve çıkış** bölümü. Daha fazla bilgi için bkz: [veri içeri aktarma] [ import-data] modül başvuru sayfası.
   
    ![Azure Machine Learning Veri Al][17]
2. Seçin **Azure SQL veritabanı** olarak **veri kaynağı** içinde **özellikleri** paneli.
3. Veritabanı DNS adı **veritabanı sunucusu adı** alan. Biçimi:`tcp:<your_virtual_machine_DNS_name>,1433`
4. Girin **veritabanı adı** karşılık gelen alandaki.
5. Girin **SQL kullanıcı adı** içinde ** sunucu kullanıcı aqccount adını ve parolayı **Server kullanıcı hesabı parolasını**.
6. Denetleme **herhangi bir sunucu sertifikayı kabul** seçeneği.
7. İçinde **veritabanı sorgusu** metin alanı düzenlemek, gerekli veritabanı alanları (etiketler gibi hesaplanan alanları dahil) ayıklayan sorguyu yapıştırın ve aşağı istenen örnek boyutu için veri örnekleri.

Aşağıdaki şekilde doğrudan SQL Server veritabanından veri okunurken bir ikili sınıflandırma deneme örnektir. Benzer denemeler, çok sınıflı sınıflandırma ve regresyon sorunları için oluşturulabilir.

![Azure Machine Learning eğitimi][10]

> [!IMPORTANT]
> Modelleme verileri ayıklama ve sorgu örnekler örnekleme önceki bölümlerde sağlanan **üç modelleme alıştırmalar için tüm etiketleri sorguda dahil edilen**. Her modelleme alıştırmalarda önemli bir (gerekli) adım **hariç** diğer iki sorunlar ve diğer gereksiz etiketlerini **hedef sızıntıları**. İçin örn., ikili Sınıflandırma, kullanırken etiketi **Eğimli** ve alanları dışarıda **İpucu\_sınıfı**, **İpucu\_tutar**, ve **toplam\_tutar**. İpucu kapsıyor beri ikinci hedef sızıntıları olan Ücretli.
> 
> Gereksiz sütunları hariç tutmak ve/veya sızıntıları hedeflemek için kullanabilir [Select Columns in Dataset sütun] [ select-columns] modülü veya [Düzenle meta veri][edit-metadata]. Daha fazla bilgi için bkz: [Select Columns in Dataset sütun] [ select-columns] ve [Düzenle meta veri] [ edit-metadata] başvuru sayfaları.
> 
> 

## <a name="mldeploy"></a>Azure Machine Learning modellerini dağıtma
Modeliniz hazır olduğunda, kolayca bunu deneme doğrudan bir web hizmeti olarak dağıtabilirsiniz. Azure Machine Learning web hizmetleri dağıtma hakkında daha fazla bilgi için bkz: [bir Azure Machine Learning web hizmetini dağıtma](../studio/publish-a-machine-learning-web-service.md).

Yeni bir web hizmeti dağıtmak için aktarmanız gerekir:

1. Puanlama bir deneme oluşturma.
2. Web hizmeti dağıtın.

Gelen Puanlama bir deneme oluşturmak için bir **tamamlandı** deneme eğitim, tıklatın **oluşturma PUANLAMA DENEMELER** alt eylem çubuğunda.

![Azure Puanlama][18]

Azure Machine Learning eğitim denemenizi bileşenlerini temel alan bir Puanlama deneme oluşturmayı deneyecek. Özellikle, çıkarır:

1. Eğitilen modeli kaydedin ve model eğitim modüllerini kaldırın.
2. Bir mantıksal tanımlamak **giriş bağlantı noktası** beklenen giriş veri şeması temsil etmek için.
3. Bir mantıksal tanımlamak **çıkış bağlantı noktasına** beklenen web hizmeti çıkış şeması temsil etmek için.

Puanlama deneme oluşturulduğunda, gözden geçirin ve gerektiği gibi ayarlayın. Hizmeti çağrıldığında bunlar kullanılabilir olmayacak şekilde girdi veri kümesi ve/veya sorgu etiket alanları hariç biriyle değiştirin tipik bir ayarlanmasıdır. Ayrıca bazı kayıtları için giriş veri kümesi ve/veya sorgu boyutunu azaltmak için iyi bir uygulama olur giriş şemasını belirtmek yeterli. Çıkış bağlantı noktasına için tüm giriş alanları çıkarmak ve yalnızca eklemek için ortak olan **skoru etiketleri** ve **skoru olasılıklar** çıkış kullanarak [Select Columns in Dataset sütun] [ select-columns] modülü.

Deneme Puanlama bir örneği aşağıdaki şekilde gösterilmiştir. Hazır olduğunuzda dağıtmak tıklatın **yayımlama WEB hizmeti** alt eylem çubuğunda düğmesi.

![Azure Machine Learning yayımlama][11]

, Bu gözden geçirme öğreticide olduðunu için tüm veri alım model eğitimi ve bir Azure Machine Learning web hizmeti dağıtma büyük ortak kümesinden ile çalışan bir Azure veri bilimi ortam oluşturdunuz.

### <a name="license-information"></a>Lisans bilgileri
Bu örnek gözden geçirme ve kendi eşlik eden komut dosyaları ve IPython notebook(s) paylaşıldığı Microsoft tarafından MIT lisansı altında. Lütfen LICENSE.txt dosyayı daha fazla ayrıntı için örnek kod github'da dizininde iade edin.

### <a name="references"></a>Başvurular
• [Andrés Monroy NYC ücreti dönüşleri indirme sayfası](http://www.andresmh.com/nyctaxitrips/)  
• [FOILing NYC ait ücreti seyahat veri Chris Whong tarafından](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• [NYC ücreti ve Limousine devreye sokmak araştırma ve istatistikleri](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)

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
