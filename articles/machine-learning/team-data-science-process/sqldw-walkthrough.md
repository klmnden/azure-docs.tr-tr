---
title: "Eylem takım veri bilimi işleminde: SQL Data Warehouse kullanarak | Microsoft Docs"
description: "Gelişmiş analizler işlemi ve eylem teknoloji"
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 88ba8e28-0bd7-49fe-8320-5dfa83b65724
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/24/2017
ms.author: bradsev;weig
ms.openlocfilehash: 73517a8d58700e987ce80889dadf8791e53170a3
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2017
---
# <a name="the-team-data-science-process-in-action-using-sql-data-warehouse"></a>Eylem takım veri bilimi işleminde: SQL Data Warehouse kullanma
Bu öğreticide, biz, oluşturma ve dağıtma SQL veri ambarı (SQL DW) kullanarak bir makine öğrenimi modeline aracılığıyla genel kullanıma açık bir veri kümesi için--yol [NYC ücreti dönüşleri](http://www.andresmh.com/nyctaxitrips/) veri kümesi. Oluşturulan ikili sınıflandırma modeli bir ipucu seyahat için ödeme ve çok sınıflı sınıflandırma ve regresyon modeli Ayrıca, dağıtım Ücretli ipucu tutarlarının tahmin açıklanan olup olmadığını tahmin eder.

Yordamdan sonraki [takım veri bilimi işlem (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) iş akışı. Bir veri bilimi ortamı kurulumu nasıl gösteriyoruz modeline özellikleri nasıl verileri SQL DW yüklemek ve nasıl mühendislik ve verileri araştırmak için SQL DW veya IPython dizüstü bilgisayar kullanın. Derleme ve Azure Machine Learning ile bir model dağıtma sonra gösteriyoruz.

## <a name="dataset"></a>NYC ücreti dönüşleri veri kümesi
Yaklaşık 20 GB sıkıştırılmış CSV dosyaları (~ 48 GB sıkıştırılmamış) NYC ücreti seyahat veri oluşur, her seyahat için ücretli 173 milyondan fazla tek tek dönüşleri ve fares kaydetme. Her seyahat kayıt alma ve teslim konumları ve kez, anonim korsan (sürücü) lisans numarası ve medallion (ücreti'nın benzersiz kimliği) numarasını içerir. Veri 2013 yıl içinde tüm dönüşleri kapsayan ve aşağıdaki iki veri kümelerinin her ay sağlanır:

1. **Trip_data.csv** dosyası yolcu, toplama ve dropoff noktaları, seyahat süresi ve seyahat Uzunluk sayısı gibi seyahat ayrıntılarını içerir. Birkaç örnek kayıt şunlardır:
   
        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
2. **Trip_fare.csv** dosyası ödeme türü, ücreti tutarı, ek ücret ve vergileri, ipuçları ve tolls, gibi her seyahat için ödenen ücreti ve Ücretli toplam miktarı ayrıntılarını içerir. Birkaç örnek kayıt şunlardır:
   
        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

**Benzersiz anahtar** seyahat katılmak için kullanılan\_veri ve seyahat\_ücreti şu üç alanlardan oluşur:

* medallion,
* korsan saldırılarına\_lisans ve
* Toplama\_datetime.

## <a name="mltasks"></a>Üç tür tahmin görevleri adres
Biz göre üç tahmin sorunları formüle *İpucu\_tutar* görevleri modelleme üç tür göstermek için:

1. **İkili sınıflandırma**: tahmin etmek için olsun veya olmasın bir ipucu Ücretli bir seyahat, yani bir *İpucu\_tutar* büyük $0 pozitif bir örnektir küçük, ancak bir *İpucu\_tutar* 0 / negatif bir örnektir.
2. **Çok sınıflı sınıflandırma**: seyahat için ücretli ipucu aralığını tahmin etmek için. Biz bölmek *İpucu\_tutar* beş depo veya sınıfları:
   
        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20
3. **Regresyon görev**: seyahat için ücretli ipucu miktarı tahmin etmek için.  

## <a name="setup"></a>Gelişmiş analiz için Azure veri bilimi ortamını ayarlama
Azure veri bilimi ortamınızı ayarlamak için aşağıdaki adımları izleyin.

**Kendi Azure blob storage hesabı oluşturma**

* Kendi Azure blob depolama sağlamak için Azure blob depolama içinde veya için mümkün olduğunca yakın bir coğrafi konum seçin **Orta Güney ABD**, NYC ücreti verilerinin depolandığı olduğu. Veri kendi depolama hesabınızdaki bir kapsayıcıya ortak blob depolama kapsayıcısından AzCopy kullanarak kopyalanacak. Yakınsa Azure blob depolama alanınızın Orta Güney ABD için bu görevi (4. adım) o kadar hızlı tamamlanır.
* Kendi Azure depolama hesabı oluşturmak için en özetlenen adımları izleyin [Azure storage hesapları hakkında](../../storage/common/storage-create-storage-account.md). Bu kılavuzda daha sonra gerekli şekilde notları aşağıdaki depolama hesabının kimlik bilgilerini değerlerinde yaptığınızdan emin olun.
  
  * **Depolama hesabı adı**
  * **Depolama hesabı anahtarı**
  * **Kapsayıcı adı** (hangi Azure blob storage'da depolanan verilerin istediğiniz)

**Azure SQL DW örneğinizi sağlayın.**
Adresindeki izleyin [SQL Data Warehouse oluşturma](../../sql-data-warehouse/sql-data-warehouse-get-started-provision.md) bir SQL Data Warehouse örneğine sağlayacak. Sonraki adımlarda kullanılacak olan aşağıdaki SQL Data Warehouse kimlik bilgileri üzerindeki gösterimler yaptığınızdan emin olun.

* **Sunucu adı**: <server Name>. database.windows.net
* **SQLDW (veritabanı) adı**
* **Kullanıcı Adı**
* **Parola**

**Visual Studio ve SQL Server veri araçları yükleyin.** Yönergeler için bkz: [yükleme Visual Studio 2015 ve/veya SQL Data Warehouse için SSDT (SQL Server veri araçları)](../../sql-data-warehouse/sql-data-warehouse-install-visual-studio.md).

**Visual Studio ile Azure SQL DW bağlayın.** 1. ve 2. adımları yönergeler için bkz [Visual Studio ile Azure SQL Data Warehouse Bağlan](../../sql-data-warehouse/sql-data-warehouse-connect-overview.md).

> [!NOTE]
> Aşağıdaki SQL sorgusunu oluşturduğunuz (yerine connect konu 3. adımında sağlanan sorgu) SQL veri ambarı veritabanı için çalışan **bir ana anahtar oluşturma**.
> 
> 

    BEGIN TRY
           --Try to create the master key
        CREATE MASTER KEY
    END TRY
    BEGIN CATCH
           --If the master key exists, do nothing
    END CATCH;

**Azure aboneliğiniz altında bir Azure Machine Learning çalışma alanı oluşturun.** Yönergeler için bkz: [bir Azure Machine Learning çalışma alanı oluşturma](../studio/create-workspace.md).

## <a name="getdata"></a>SQL Data Warehouse'a veri yükleme
Bir Windows PowerShell komut konsolunu açın. Aşağıdaki PowerShell çalıştırma SQL örneği yüklemek için komut komut sizinle Github'da parametresiyle belirttiğiniz yerel bir dizine hepimizin dosyalarını *- DestDir*. Parametre değerini değiştirebilir *- DestDir* herhangi bir yerel dizine. Varsa *- DestDir* mevcut olmadığından PowerShell betiği tarafından oluşturulur.

> [!NOTE]
> İçin gerekebilecek **yönetici olarak çalıştır** , aşağıdaki PowerShell Betiği yürütülürken, *DestDir* dizinin Yöneticisi ayrıcalığı oluşturmak veya yazma için yeterlidir.
> 
> 

    $source = "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/Download_Scripts_SQLDW_Walkthrough.ps1"
    $ps1_dest = "$pwd\Download_Scripts_SQLDW_Walkthrough.ps1"
    $wc = New-Object System.Net.WebClient
    $wc.DownloadFile($source, $ps1_dest)
    .\Download_Scripts_SQLDW_Walkthrough.ps1 –DestDir 'C:\tempSQLDW'

Geçerli çalışma dizini değişikliklerini başarılı yürütme sonrasında *- DestDir*. Ekran görebilmek için aşağıdaki gibi:

![][19]

İçinde *- DestDir*, Yönetici modunda aşağıdaki PowerShell betiğini yürütün:

    ./SQLDW_Data_Import.ps1

PowerShell komut dosyası ilk kez çalıştığında Azure SQL DW ve Azure blob depolama hesabı bilgileri giriş istenir. Bu PowerShell betik tamamlandığında, ilk olarak, kimlik bilgileri çalıştıran, giriş mevcut çalışma dizininde bir yapılandırma dosyasına SQLDW.conf yazılan. Bu PowerShell komut dosyası gelecekteki yürütülmesi tüm gerekli parametreleri bu yapılandırma dosyasından okuma seçeneğine sahiptir. Bazı parametreler değiştirmeniz gerekiyorsa, bu yapılandırma dosyasını silmek ve parametre değerlerini istendiğinde giriş yapma giriş ekranında istemi bağlı parametreleri veya, SQLDW.confdosyasındadüzenleyerekparametredeğerlerinideğiştirmekiçinseçebilirsiniz*- DestDir* dizini.

> [!NOTE]
> Şema adı çakışıyor, Azure SQL DW parametreleri doğrudan SQLDW.conf dosyasından okurken zaten o önlemek için 3 basamaklı bir rastgele sayı şema adı SQLDW.conf dosyasından her çalışma için varsayılan şema adı olarak eklenir. PowerShell komut dosyası için bir şema ad isteyebilir: kullanıcı kümeleri adı belirtilebilir.
> 
> 

Bu **PowerShell Betiği** dosya aşağıdaki görevleri tamamlar:

* **İndirir ve AzCopy yükler**, AzCopy zaten yüklü değilse
  
        $AzCopy_path = SearchAzCopy
        if ($AzCopy_path -eq $null){
               Write-Host "AzCopy.exe is not found in C:\Program Files*. Now, start installing AzCopy..." -ForegroundColor "Yellow"
            InstallAzCopy
            $AzCopy_path = SearchAzCopy
        }
            $env_path = $env:Path
            for ($i=0; $i -lt $AzCopy_path.count; $i++){
                if ($AzCopy_path.count -eq 1){
                    $AzCopy_path_i = $AzCopy_path
                } else {
                    $AzCopy_path_i = $AzCopy_path[$i]
                }
                if ($env_path -notlike '*' +$AzCopy_path_i+'*'){
                    Write-Host $AzCopy_path_i 'not in system path, add it...'
                    [Environment]::SetEnvironmentVariable("Path", "$AzCopy_path_i;$env_path", "Machine")
                    $env:Path = [System.Environment]::GetEnvironmentVariable("Path","Machine")
                    $env_path = $env:Path
                }
* **Verileri özel blob depolama hesabınıza kopyalar** AzCopy ile ortak blobda
  
        Write-Host "AzCopy is copying data from public blob to yo storage account. It may take a while..." -ForegroundColor "Yellow"
        $start_time = Get-Date
        AzCopy.exe /Source:$Source /Dest:$DestURL /DestKey:$StorageAccountKey /S
        $end_time = Get-Date
        $time_span = $end_time - $start_time
        $total_seconds = [math]::Round($time_span.TotalSeconds,2)
        Write-Host "AzCopy finished copying data. Please check your storage account to verify." -ForegroundColor "Yellow"
        Write-Host "This step (copying data from public blob to your storage account) takes $total_seconds seconds." -ForegroundColor "Green"
* **Polybase, Azure SQL DW (LoadDataToSQLDW.sql yürüterek) kullanarak verileri yükler** aşağıdaki komutlarla özel blob depolama hesabınızdan.
  
  * Bir şema oluşturun
    
          EXEC (''CREATE SCHEMA {schemaname};'');
  * Bir veritabanı kapsamlı kimlik bilgisi oluşturma
    
          CREATE DATABASE SCOPED CREDENTIAL {KeyAlias}
          WITH IDENTITY = ''asbkey'' ,
          Secret = ''{StorageAccountKey}''
  * Bir Azure depolama blob için bir dış veri kaynağı oluşturun
    
          CREATE EXTERNAL DATA SOURCE {nyctaxi_trip_storage}
          WITH
          (
              TYPE = HADOOP,
              LOCATION =''wasbs://{ContainerName}@{StorageAccountName}.blob.core.windows.net'',
              CREDENTIAL = {KeyAlias}
          )
          ;
    
          CREATE EXTERNAL DATA SOURCE {nyctaxi_fare_storage}
          WITH
          (
              TYPE = HADOOP,
              LOCATION =''wasbs://{ContainerName}@{StorageAccountName}.blob.core.windows.net'',
              CREDENTIAL = {KeyAlias}
          )
          ;
  * Bir csv dosyası için bir dış dosya biçimi oluşturun. Veri sıkıştırılmamış ve alanlar dikey çizgi karakteriyle ayrılır.
    
          CREATE EXTERNAL FILE FORMAT {csv_file_format}
          WITH
          (   
              FORMAT_TYPE = DELIMITEDTEXT,
              FORMAT_OPTIONS  
              (
                  FIELD_TERMINATOR ='','',
                  USE_TYPE_DEFAULT = TRUE
              )
          )
          ;
  * Dış ücreti ve seyahat tabloları NYC ücreti veri kümesi için Azure blob depolama alanına oluşturun.
    
          CREATE EXTERNAL TABLE {external_nyctaxi_fare}
          (
              medallion varchar(50) not null,
              hack_license varchar(50) not null,
              vendor_id char(3),
              pickup_datetime datetime not null,
              payment_type char(3),
              fare_amount float,
              surcharge float,
              mta_tax float,
              tip_amount float,
              tolls_amount float,
              total_amount float
          )
          with (
              LOCATION    = ''/nyctaxifare/'',
              DATA_SOURCE = {nyctaxi_fare_storage},
              FILE_FORMAT = {csv_file_format},
              REJECT_TYPE = VALUE,
              REJECT_VALUE = 12     
          )  

            CREATE EXTERNAL TABLE {external_nyctaxi_trip}
            (
                   medallion varchar(50) not null,
                   hack_license varchar(50)  not null,
                   vendor_id char(3),
                   rate_code char(3),
                   store_and_fwd_flag char(3),
                   pickup_datetime datetime  not null,
                   dropoff_datetime datetime,
                   passenger_count int,
                   trip_time_in_secs bigint,
                   trip_distance float,
                   pickup_longitude varchar(30),
                   pickup_latitude varchar(30),
                   dropoff_longitude varchar(30),
                   dropoff_latitude varchar(30)
            )
            with (
                LOCATION    = ''/nyctaxitrip/'',
                DATA_SOURCE = {nyctaxi_trip_storage},
                FILE_FORMAT = {csv_file_format},
                REJECT_TYPE = VALUE,
                REJECT_VALUE = 12         
            )

    - SQL veri ambarı için Azure blob depolama alanındaki dış tablodan veri yükleme

            CREATE TABLE {schemaname}.{nyctaxi_fare}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            SELECT *
            FROM   {external_nyctaxi_fare}
            ;

            CREATE TABLE {schemaname}.{nyctaxi_trip}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            SELECT *
            FROM   {external_nyctaxi_trip}
            ;

    - Örnek veri tablosu (NYCTaxi_Sample) oluşturma ve veri için SQL sorguları seyahat ve ücreti tablolarda seçerek ekleyin. (Bu kılavuzun bazı adımlar gerekir bu örnek tabloyu kullanın.)

            CREATE TABLE {schemaname}.{nyctaxi_sample}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            (
                SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount, f.total_amount, f.tip_amount,
                tipped = CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END,
                tip_class = CASE
                        WHEN (tip_amount = 0) THEN 0
                        WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
                        WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
                        WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
                        ELSE 4
                    END
                FROM {schemaname}.{nyctaxi_trip} t, {schemaname}.{nyctaxi_fare} f
                WHERE datepart("mi",t.pickup_datetime) = 1
                AND t.medallion = f.medallion
                AND   t.hack_license = f.hack_license
                AND   t.pickup_datetime = f.pickup_datetime
                AND   pickup_longitude <> ''0''
                AND   dropoff_longitude <> ''0''
            )
            ;

Yükleme süreleri depolama hesaplarınızı coğrafi konumunu etkiler.

> [!NOTE]
> Özel blob storage hesabı coğrafi konumu bağlı olarak, özel depolama hesabınıza bir ortak blob üzerinden veri kopyalama işlemi yaklaşık 15 dakika alabilir ya da daha uzun bir süre ve verileri için Azure storage hesabınızdan yükleme işlemi SQL DW 20 dakika sürebilir veya daha uzun.  
> 
> 

Yinelenen kaynak ve hedef dosya varsa, hangi karar vermeniz gerekir.

> [!NOTE]
> Kopyalanacak .csv dosyaları özel blob storage hesabınıza ortak blob depolama özel blob depolama hesabınız zaten var, AzCopy bunları üzerine yazmak isteyip istemediğinizi sorar. Bunları üzerine yazmak istemiyorsanız giriş  **n**  istendiğinde. Üzerine yazmak istiyorsanız **tüm** birini giriş **bir** istendiğinde. Ayrıca giriş **y** .csv dosyalarını tek tek üzerine yazmak için.
> 
> 

![#21 Çiz][21]

Kendi verilerinizi kullanabilirsiniz. Verileriniz şirket içi makinenizdeki gerçek hayatta uygulamanızda ise, AzCopy şirket içi verileri, özel Azure blob depolama alanına yüklemek için kullanmaya devam edebilirsiniz. Yalnızca değiştirmeye ihtiyaç **kaynak** konumu, `$Source = "http://getgoing.blob.core.windows.net/public/nyctaxidataset"`, verilerinizi içeren yerel dizine PowerShell komut dosyasının AzCopy komutu.

> [!TIP]
> Verilerinizi gerçek hayatta uygulamanızda zaten, özel Azure blob depolama alanına ise, PowerShell Betiği olarak AzCopy adımı atlayıp doğrudan Azure SQL DW karşıya veri yükleme. Bu ek düzenlemeler verilerinizi biçimine uyarlamak için komut dosyası gerektirir.
> 
> 

Bu üç dosyayı hemen sonra çalıştı hazır; böylece bu Powershell betiğini de Azure SQL DW bilgileri veri araştırması örnek dosyaları SQLDW_Explorations.sql, SQLDW_Explorations.ipynb ve SQLDW_Explorations_Scripts.py takılan PowerShell Betiği tamamlar.

Başarılı bir yürütme sonrasında ekran görürsünüz aşağıdaki gibi:

![][20]

## <a name="dbexplore"></a>Veri keşfi ve Azure SQL Data Warehouse özellik Mühendisliği
Bu bölümde, biz veri keşfi ve özellik nesil SQL sorgularını kullanarak doğrudan Azure SQL DW karşı çalıştırarak gerçekleştirebilirsiniz **Visual Studio veri Araçları**. Bu bölümde kullanılan tüm SQL sorguları adlı örnek komut dosyasında bulunabilir *SQLDW_Explorations.sql*. Bu dosya, PowerShell betiği tarafından yerel dizininize zaten yüklendi. Buradan da alabilir [GitHub](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/SQLDW_Explorations.sql). Ancak GitHub dosyasında prize takılı Azure SQL DW bilgileri yok.

SQL DW oturum açma adı ve parola ile Visual Studio kullanarak, Azure SQL DW bağlanmak ve açık **SQL Nesne Gezgini** veritabanı ve tablolar alındı onaylamak için. Alma *SQLDW_Explorations.sql* dosya.

> [!NOTE]
> Bir paralel veri ambarı (PDW) sorgu Düzenleyicisi'ni açmak için kullanmak **yeni sorgu** , PDW seçildiğinde komut **SQL Nesne Gezgini**. Standart SQL sorgu Düzenleyicisi'ni PDW tarafından desteklenmiyor.
> 
> 

Veri türü şunlardır araştırması ve özellik oluşturma görevleri gerçekleştirilen Bu bölümde:

* Veri dağıtımları değişen zaman pencereleri birkaç alanların keşfedin.
* Veri Kalitesi boylam ve enlem alanlarının araştırın.
* Temel çok sınıflı ve ikili sınıflandırma etiketleri oluşturmak **İpucu\_tutar**.
* Özellikler oluşturmak ve işlem/seyahat uzaklıklar karşılaştırın.
* İki tablo birleştirme ve modelleri oluşturmak için kullanılan rastgele bir örnek ayıklayın.

### <a name="data-import-verification"></a>Veri alma doğrulama
Bu sorgular hızlı bir satır sayısı doğrulaması sağlamak ve daha önce Polybase'nın Paralel toplu kullanarak doldurulmuş tablolardaki sütunların alma,

    -- Report number of rows in table <nyctaxi_trip> without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_trip>')

    -- Report number of columns in table <nyctaxi_trip>
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = '<nyctaxi_trip>' AND table_schema = '<schemaname>'

**Çıkış:** 173,179,759 satırları ve sütunları 14 almanız gerekir.

### <a name="exploration-trip-distribution-by-medallion"></a>Araştırması: Medallion seyahat dağıtım
Bu örnekte sorgu, belirtilen bir süre içinde 100'den fazla dönüşleri tamamlandı medallions (ücreti sayılar) tanımlar. Bölümleme düzeni söylediğinizde beri sorgu bölümlenmiş tabloda erişimden yararlanabileceğini **toplama\_datetime**. Tam veri kümesini sorgulama bölümlenmiş tablosu kullanın ve/veya dizin tarama ayrıca olun.

    SELECT medallion, COUNT(*)
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

**Çıkış:** sorgu 13,369 medallions (taksiler) ve bunları tarafından 2013'te tamamlandı seyahat sayısını belirtme satırları içeren bir tablo döndürmelidir. Son sütun tamamlandı dönüş sayısını içerir.

### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>Keşfetme: dağıtım medallion ve hack_license tarafından seyahat
Bu örnek medallions (ücreti sayılar) tanımlar ve hack_license (sürücüler) tamamlanan birden fazla 100 dönüşleri belirtilen süre içinde numaralandırır.

    SELECT medallion, hack_license, COUNT(*)
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

**Çıkış:** sorgu 100 2013'te dönüşleri olduğunu daha tamamladınız 13,369 araba/sürücü kimlikleri belirtme 13,369 satırları içeren bir tablo döndürmelidir. Son sütun tamamlandı dönüş sayısını içerir.

### <a name="data-quality-assessment-verify-records-with-incorrect-longitude-andor-latitude"></a>Veri Kalitesi değerlendirmesi: yanlış boylam ve/veya enlem ile kayıtlarını doğrulama
Boylam ve/veya enlem alanların herhangi birini ya da geçersiz bir değer içeriyorsa, bu örnek araştırır (radian derece -90 ile 90 arasında olmalıdır), ya da sahip (0, 0) koordinatları.

    SELECT COUNT(*) FROM <schemaname>.<nyctaxi_trip>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

**Çıkış:** sorgu geçersiz boylam ve/veya enlem alanınız 837,467 dönüşleri döndürür.

### <a name="exploration-tipped-vs-not-tipped-trips-distribution"></a>Keşfetme: eğimli değil Eğimli dönüşleri dağıtım
Bu örnek belirtilen bir süre (veya tam veri kümesinin burada ayarlanır gibi tam yıl kapsayan varsa) Eğimli değil sayı Eğimli dönüş sayısı bulur. Bu dağıtım için ikili sınıflandırma modelleme daha sonra kullanılmak üzere ikili etiket dağıtım yansıtır.

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM <schemaname>.<nyctaxi_fare>
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

**Çıkış:** sorgu şu ipucu sıklıklarını 2013: 90,447,622 Eğimli yıl ve 82,264,709 için not Eğimli döndürmelidir.

### <a name="exploration-tip-classrange-distribution"></a>Keşfetme: İpucu sınıfı/range dağıtım
Bu örnek, belirli bir süre içinde (veya tam yıl kapsayan varsa tam veri kümesi) dağıtım ipucu aralıklarının hesaplar. Daha sonra çok sınıflı sınıflandırma model için kullanılacak etiket sınıfları dağıtımını budur.

    SELECT tip_class, COUNT(*) AS tip_freq FROM (
        SELECT CASE
            WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tip_class

**Çıktı:**

| tip_class | tip_freq |
| --- | --- |
| 1 |82230915 |
| 2 |6198803 |
| 3 |1932223 |
| 0 |82264625 |
| 4 |85765 |

### <a name="exploration-compute-and-compare-trip-distance"></a>Keşfetme: İşlem ve seyahat uzaklığı karşılaştırma
Bu örnek alma ve teslim boylam dönüştürür ve SQL Coğrafya enlem noktaları, SQL Coğrafya noktaları fark kullanarak seyahat uzaklığı hesaplar ve sonuçları karşılaştırma için rastgele bir örneğini döndürür. Bu örnek yalnızca daha önce ele alınan veri kalitesi değerlendirme sorgusu kullanılarak geçerli koordinatları sonuçları sınırlar.

    /****** Object:  UserDefinedFunction [dbo].[fnCalculateDistance] ******/
    SET ANSI_NULLS ON
    GO

    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type IN ('FN', 'IF') AND name = 'fnCalculateDistance')
      DROP FUNCTION fnCalculateDistance
    GO

    -- User-defined function to calculate the direct distance  in mile between two geographical coordinates.
    CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)

    RETURNS float
    AS
    BEGIN
          DECLARE @distance decimal(28, 10)
          -- Convert to radians
          SET @Lat1 = @Lat1 / 57.2958
          SET @Long1 = @Long1 / 57.2958
          SET @Lat2 = @Lat2 / 57.2958
          SET @Long2 = @Long2 / 57.2958
          -- Calculate distance
          SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
          --Convert to miles
          IF @distance <> 0
          BEGIN
            SET @distance = 3958.75 * ATAN(SQRT(1 - POWER(@distance, 2)) / @distance);
          END
          RETURN @distance
    END
    GO

    SELECT pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS DirectDistance
    FROM <schemaname>.<nyctaxi_trip>
    WHERE datepart("mi",pickup_datetime)=1
    AND CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND pickup_longitude != '0' AND dropoff_longitude != '0'

### <a name="feature-engineering-using-sql-functions"></a>Özellik Mühendisliği SQL işlevlerini kullanma
Bazı durumlarda SQL işlevleri özellik Mühendisliği için verimli bir seçenek olabilir. Bu kılavuzda, toplama ve dropoff konumları arasında doğrudan uzaklığı hesaplamak için bir SQL işlevi tanımlı. Aşağıdaki SQL komut dosyaları çalıştırabilirsiniz **Visual Studio veri Araçları**.

Distance işlevi tanımlar SQL komut dosyası aşağıda verilmiştir.

    SET ANSI_NULLS ON
    GO

    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type IN ('FN', 'IF') AND name = 'fnCalculateDistance')
      DROP FUNCTION fnCalculateDistance
    GO

    -- User-defined function calculate the direct distance between two geographical coordinates.
    CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)

    RETURNS float
    AS
    BEGIN
          DECLARE @distance decimal(28, 10)
          -- Convert to radians
          SET @Lat1 = @Lat1 / 57.2958
          SET @Long1 = @Long1 / 57.2958
          SET @Lat2 = @Lat2 / 57.2958
          SET @Long2 = @Long2 / 57.2958
          -- Calculate distance
          SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
          --Convert to miles
          IF @distance <> 0
          BEGIN
            SET @distance = 3958.75 * ATAN(SQRT(1 - POWER(@distance, 2)) / @distance);
          END
          RETURN @distance
    END
    GO

SQL sorgusunda özellikleri oluşturmak için bu işlevi çağırmak için örnek aşağıda verilmiştir:

    -- Sample query to call the function to create features
    SELECT pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS DirectDistance
    FROM <schemaname>.<nyctaxi_trip>
    WHERE datepart("mi",pickup_datetime)=1
    AND CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND pickup_longitude != '0' AND dropoff_longitude != '0'

**Çıkış:** bu sorgu ile alımı ve dropoff latitudes (2,803,538 satırlar) içeren bir tablo oluşturur ve longitudes ve karşılık gelen doğrudan mil cinsinden. İlk 3 satır için sonuçları şunlardır:

|  | pickup_latitude | pickup_longitude | dropoff_latitude | dropoff_longitude | DirectDistance |
| --- | --- | --- | --- | --- | --- |
| 1 |40.731804 |-74.001083 |40.736622 |-73.988953 |.7169601222 |
| 2 |40.715794 |-74,010635 |40.725338 |-74.00399 |.7448343721 |
| 3 |40.761456 |-73.999886 |40.766544 |-73.988228 |0.7037227967 |

### <a name="prepare-data-for-model-building"></a>Model oluşturmanın için verileri hazırlama
Aşağıdaki sorgu birleştirmeler **nyctaxi\_seyahat** ve **nyctaxi\_ücreti** tabloları, ikili sınıflandırma etiketini oluşturur **Eğimli**, birden çok sınıf sınıflandırma etiketini **İpucu\_sınıfı**ve bir örnek tam birleştirilmiş kümesinden ayıklar. Örnekleme toplama zamana dayalı dönüşleri kümesini alarak yapılır.  Bu sorgu kopyalanır, ardından doğrudan yapıştırılan [Azure Machine Learning Studio](https://studio.azureml.net) [veri içeri aktarma] [ import-data] SQL veritabanı örneğinde gelen doğrudan veri alımı için Modülü Azure. Sorgu yanlış kayıtlarıyla dışlar (0, 0) koordinatları.

    SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,     f.total_amount, f.tip_amount,
        CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped,
        CASE WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM <schemaname>.<nyctaxi_trip> t, <schemaname>.<nyctaxi_fare> f
    WHERE datepart("mi",t.pickup_datetime) = 1
    AND   t.medallion = f.medallion
    AND   t.hack_license = f.hack_license
    AND   t.pickup_datetime = f.pickup_datetime
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'

Azure Machine Learning ile devam etmeye hazır olduğunuzda ya da görebilirsiniz:  

1. Ayıklamak ve veri örneği ve kopyala-yapıştır doğrudan sorgu için en son SQL sorgu Kaydet bir [veri içeri aktarma] [ import-data] Azure Machine Learning modülünde veya
2. Yeni bir SQL DW tabloda oluşturma modeli için kullanmayı düşünüyorsanız örneklenen ve tasarlanan verilerini kalıcı hale getirmek ve yeni tabloda kullanmak [veri içeri aktarma] [ import-data] Azure Machine Learning modülünde. PowerShell Betiği önceki adımda bunu sizin için yapmıştır. Bu tabloda veri içeri aktarma modülü doğrudan okuyabilir.

## <a name="ipnb"></a>Veri keşfi ve özellik Mühendisliği IPython Not
Bu bölümde, veri keşfi ve her iki Python kullanarak özellik oluşturma gerçekleştiririz ve SQL DW SQL sorguları daha önce oluşturduğunuz. Adlandırılmış bir örnek IPython not defteri **SQLDW_Explorations.ipynb** ve Python komut dosyası **SQLDW_Explorations_Scripts.py** yerel dizininize indirilir. Ayrıca üzerinde kullanılabilir [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/SQLDW). Bu iki dosyayı Python komut dosyalarında aynıdır. Bir IPython dizüstü sunucusu olmayan olasılığına Python komut dosyası olanak sağlanır. Bu iki örnek dosyaları altında tasarlanmıştır Python **Python 2.7**.

Gerekli Azure SQL DW bilgileri IPython not defteri ve Python komut dosyası yerel makinenize yüklenen örnek PowerShell betiği tarafından daha önce takıldığını. Bunlar herhangi bir değişiklik yapılmadan çalıştırılabilir.

Bir AzureML çalışma alanı zaten ayarladıysanız, doğrudan IPython Not Örnek AzureML IPython Not Defteri hizmetine yüklemek ve onu çalıştıran başlatın. AzureML IPython Not Defteri hizmetine yüklemek için adımlar şunlardır:

1. AzureML alanınıza oturum, en üstünde "Studio'nun"'i tıklatın ve web sayfasının sol tarafındaki "dizüstü bilgisayarlar"'i tıklatın.
   
    ![#22 Çiz][22]
2. Web sayfasının sol alt köşesindeki "Yeni"'yi tıklatın ve seçin "Python 2". Ardından, dizüstü bilgisayar için bir ad sağlayın ve yeni boş IPython not defteri oluşturmak için onay işaretine tıklayın.
   
    ![#23 Çiz][23]
3. Yeni IPython not defteri sol üst köşesindeki "Jupyter" simgesine tıklayın.
   
    ![#24 Çiz][24]
4. Sürükleme ve bırakma IPython Not örnek için **ağaç** sayfasında AzureML IPython dizüstü hizmet ve tıklatın **karşıya**. Ardından, IPython Not Örnek AzureML IPython dizüstü hizmeti yüklenecek.
   
    ![#25 Çiz][25]

Örnek çalıştırmak için dosya, aşağıdaki Python paketlerini gerekli IPython Not Defteri veya Python komut dosyası. AzureML IPython dizüstü hizmeti kullanıyorsanız, bu paketleri önceden yüklenmiş olmuştur.

    - pandas
    - numpy
    - matplotlib
    - pyodbc
    - PyTables

Gelişmiş analitik çözümler ile büyük veri üzerinde AzureML oluştururken önerilen sırası aşağıda verilmiştir:

* Veri küçük bir örnek, bir bellek içi veri çerçeveye okuyun.
* Bazı Görselleştirme ve explorations örneklenen verileri kullanarak gerçekleştirin.
* Örneklenen verileri kullanarak özellik Mühendisliği ile deneyin.
* Büyük veri keşfi, veri işleme ve özellik Mühendisliği, Python SQL sorguları doğrudan SQL DW vermek için kullanın.
* Azure Machine Learning modeli oluşturma için uygun olması için örnek boyutu karar verin.

Aşağıdakilere birkaç veri keşfi, veri Görselleştirme ve örnekler mühendislik özelliği verilebilir. Daha fazla veri explorations örnek IPython dizüstü bilgisayar ve örnek Python komut dosyasında bulunabilir.

### <a name="initialize-database-credentials"></a>Veritabanı kimlik bilgileri başlatma
Aşağıdaki değişkenler, veritabanı bağlantı ayarlarınızı başlatın:

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database driver>

### <a name="create-database-connection"></a>Veritabanı bağlantısı oluşturma
Veritabanı bağlantısı oluşturur bağlantı dizesi değil.

    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

### <a name="report-number-of-rows-and-columns-in-table-nyctaxitrip"></a>Satırları ve sütunları < nyctaxi_trip > tablosundaki rapor sayısı
    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_trip>')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('<nyctaxi_trip>') AND table_schema = ('<schemaname>')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

* Toplam satır sayısı 173179759 =  
* Toplam sütun sayısı = 14

### <a name="report-number-of-rows-and-columns-in-table-nyctaxifare"></a>Satırları ve sütunları < nyctaxi_fare > tablosundaki rapor sayısı
    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_fare>')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('<nyctaxi_fare>') AND table_schema = ('<schemaname>')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

* Toplam satır sayısı 173179759 =  
* Toplam sütun sayısı = 11

### <a name="read-in-a-small-data-sample-from-the-sql-data-warehouse-database"></a>Okuma-de SQL veri ambarı veritabanından küçük veri örneği
    t0 = time.time()

    query = '''
        SELECT TOP 10000 t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax,
            f.tolls_amount, f.total_amount, f.tip_amount
        FROM <schemaname>.<nyctaxi_trip> t, <schemaname>.<nyctaxi_fare> f
        WHERE datepart("mi",t.pickup_datetime) = 1
        AND   t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
    '''

    df1 = pd.read_sql(query, conn)

    t1 = time.time()
    print 'Time to read the sample table is %f seconds' % (t1-t0)

    print 'Number of rows and columns retrieved = (%d, %d)' % (df1.shape[0], df1.shape[1])

Örnek Tablo 14.096495 saniyedir okuma süresi.  
Satır ve sütunların sayısı, alınan = (1000 21).

### <a name="descriptive-statistics"></a>Açıklayıcı istatistikleri
Şimdi örneklenen verileri araştırmak hazırsınız. Bazı açıklayıcı istatistiklerini bakarak ile başlangıç **seyahat\_uzaklığı** (veya seçtiğiniz belirtmek için herhangi bir alan).

    df1['trip_distance'].describe()

### <a name="visualization-box-plot-example"></a>Görselleştirme: Kutusu çizim örneği
Sonraki quantiles görselleştirmek seyahat uzaklığı Kutu Çizimi ele.

    df1.boxplot(column='trip_distance',return_type='dict')

![#1 Çiz][1]

### <a name="visualization-distribution-plot-example"></a>Görselleştirme: Dağıtım çizim örneği
Dağıtım ve örneklenen seyahat uzaklıklar için bir histogram görselleştirmek çizimleri.

    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![#2 Çiz][2]

### <a name="visualization-bar-and-line-plots"></a>Görselleştirme: Çubuk ve çizgi çizer
Bu örnekte, beş depo içine seyahat uzaklığı bin ve binning sonuçları görselleştirebilirsiniz.

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

Yukarıdaki depo dağıtım çubuğundaki çizmek veya çizim ile satır:

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![#3 Çiz][3]

ve

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![#4 Çiz][4]

### <a name="visualization-scatterplot-examples"></a>Görselleştirme: Scatterplot örnekleri
Dağılım çizim arasında gösteriyoruz **seyahat\_zaman\_içinde\_saniye** ve **seyahat\_uzaklığı** herhangi bağıntı olup olmadığını görmek için

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![#6 Çiz][6]

Benzer şekilde biz arasındaki ilişkiyi kontrol edebilirsiniz **oranı\_kod** ve **seyahat\_uzaklığı**.

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![#8 Çiz][8]

### <a name="data-exploration-on-sampled-data-using-sql-queries-in-ipython-notebook"></a>Veri keşfi IPython not defterinde SQL sorgularını kullanarak örneklenen verileri
Bu bölümde, yukarıda oluşturduğumuz yeni tablodaki kalıcı örneklenen verileri kullanarak veri dağıtımları keşfedin. Not benzer explorations özgün tabloları kullanılarak gerçekleştirilebilir.

#### <a name="exploration-report-number-of-rows-and-columns-in-the-sampled-table"></a>Araştırması: Satır ve örneklenen tablodaki sütun sayısını raporu
    nrows = pd.read_sql('''SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_sample>')''', conn)
    print 'Number of rows in sample = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''SELECT count(*) FROM information_schema.columns WHERE table_name = ('<nyctaxi_sample>') AND table_schema = '<schemaname>'''', conn)
    print 'Number of columns in sample = %d' % ncols.iloc[0,0]

#### <a name="exploration-tippednot-tripped-distribution"></a>Keşfetme: Eğimli olmayan dağıtım dönüş
    query = '''
        SELECT tipped, count(*) AS tip_freq
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY tipped
        '''

    pd.read_sql(query, conn)

#### <a name="exploration-tip-class-distribution"></a>Keşfetme: İpucu sınıfı dağıtım
    query = '''
        SELECT tip_class, count(*) AS tip_freq
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY tip_class
    '''

    tip_class_dist = pd.read_sql(query, conn)

#### <a name="exploration-plot-the-tip-distribution-by-class"></a>Araştırması: sınıfı tarafından ipucu dağıtım çizim
    tip_class_dist['tip_freq'].plot(kind='bar')

![#26 Çiz][26]

#### <a name="exploration-daily-distribution-of-trips"></a>Araştırması: Dönüş günlük dağıtımını
    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-per-medallion"></a>Keşfetme: medallion başına dağıtım seyahat
    query = '''
        SELECT medallion,count(*) AS c
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-by-medallion-and-hack-license"></a>Keşfetme: dağıtım medallion ve korsan lisansla seyahat
    query = '''select medallion, hack_license,count(*) from <schemaname>.<nyctaxi_sample> group by medallion, hack_license'''
    pd.read_sql(query,conn)


#### <a name="exploration-trip-time-distribution"></a>Keşfetme: Seyahat süresi dağılımı
    query = '''select trip_time_in_secs, count(*) from <schemaname>.<nyctaxi_sample> group by trip_time_in_secs order by count(*) desc'''
    pd.read_sql(query,conn)

#### <a name="exploration-trip-distance-distribution"></a>Keşfetme: Seyahat uzaklığı dağıtım
    query = '''select floor(trip_distance/5)*5 as tripbin, count(*) from <schemaname>.<nyctaxi_sample> group by floor(trip_distance/5)*5 order by count(*) desc'''
    pd.read_sql(query,conn)

#### <a name="exploration-payment-type-distribution"></a>Keşfetme: Ödeme türü dağıtımı
    query = '''select payment_type,count(*) from <schemaname>.<nyctaxi_sample> group by payment_type'''
    pd.read_sql(query,conn)

#### <a name="verify-the-final-form-of-the-featurized-table"></a>Son formun featurized tablosunun doğrulayın
    query = '''SELECT TOP 100 * FROM <schemaname>.<nyctaxi_sample>'''
    pd.read_sql(query,conn)

## <a name="mlmodel"></a>Azure Machine Learning modellerini Derleme
Biz şimdi modeli yapı ve model dağıtımda devam etmeye hazır [Azure Machine Learning](https://studio.azureml.net). Verileri daha önce yani tanımlanan tahmin sorunları hiçbirinde kullanılmak üzere hazırdır:

1. **İkili sınıflandırma**: tahmin etmek için olsun veya olmasın bir ipucu Ücretli bir seyahat için.
2. **Çok sınıflı sınıflandırma**: aralık, önceden tanımlanmış sınıfları göre Ücretli ucunun tahmin etmek için.
3. **Regresyon görev**: seyahat için ücretli ipucu miktarı tahmin etmek için.  

Modelleme alıştırma başlamak için oturum açın, **Azure Machine Learning** çalışma. Machine learning çalışma alanı henüz oluşturmadıysanız, bkz: [Azure ML çalışma alanı oluşturma](../studio/create-workspace.md).

1. Azure Machine Learning ile çalışmaya başlamak için bkz: [Azure Machine Learning Studio nedir?](../studio/what-is-ml-studio.md)
2. Oturum [Azure Machine Learning Studio](https://studio.azureml.net).
3. Studio giriş sayfası modülleri başvurusu ve diğer kaynaklara bol miktarda bilgi, videoları, öğreticiler, bağlantılar sağlar. Azure Machine Learning hakkında daha fazla bilgi için başvurun [Azure Machine Learning Belge Merkezi](https://azure.microsoft.com/documentation/services/machine-learning/).

Tipik eğitim denemenizi aşağıdaki adımlardan oluşur:

1. Oluşturma bir **+ yeni** deneyin.
2. Verileri Azure ML alın.
3. Önceden işlemek, dönüştürme ve gerektiği gibi verileri işlemek.
4. Özellikler gerektiği şekilde oluşturun.
5. Verileri eğitim/doğrulama/test kümelerine bölme (veya ayrı veri kümeleri her).
6. Bir veya daha fazla makine öğrenimi algoritmalarını öğrenme sorunu çözmek için bağlı olarak seçin. Örneğin, ikili Sınıflandırma, çok sınıflı Sınıflandırma, regresyon.
7. Eğitim veri kümesini kullanarak bir veya daha fazla modelleri eğitme.
8. Eğitilmiş modellerini kullanarak doğrulama dataset puan.
9. Öğrenme sorun için ilgili ölçümlerini hesaplamak için modellere değerlendirin.
10. İnce modellere ayarlamak ve dağıtmak için en iyi modeli seçin.

Bu alıştırmada, biz varsa zaten incelediniz ve SQL veri ambarı verileri mühendislik ve Azure ML alımı için örnek boyutu karar. Bir veya daha fazla tahmin modelleri oluşturmak için gereken adımlar şunlardır:

1. Azure ML kullanarak veri almak [veri içeri aktarma] [ import-data] modülü, kullanılabilir **veri giriş ve çıkış** bölümü. Daha fazla bilgi için bkz: [veri içeri aktarma] [ import-data] modül başvuru sayfası.
   
    ![Verileri Azure ML İçeri Aktar][17]
2. Seçin **Azure SQL veritabanı** olarak **veri kaynağı** içinde **özellikleri** paneli.
3. Veritabanı DNS adı **veritabanı sunucusu adı** alan. Biçimi:`tcp:<your_virtual_machine_DNS_name>,1433`
4. Girin **veritabanı adı** karşılık gelen alandaki.
5. Girin *SQL kullanıcı adı* içinde **Server kullanıcı hesabı adı**ve *parola* içinde **Server kullanıcı hesabı parolasını**.
6. Denetleme **herhangi bir sunucu sertifikayı kabul** seçeneği.
7. İçinde **veritabanı sorgusu** metin alanı düzenlemek, gerekli veritabanı alanları (etiketler gibi hesaplanan alanları dahil) ayıklayan sorguyu yapıştırın ve aşağı istenen örnek boyutu için veri örnekleri.

Verileri doğrudan SQL veri ambarı veritabanından okunurken bir ikili sınıflandırma deneme, aşağıdaki şekilde örneğidir (şema adı ve tablo adlarının, örneklerde kullanılan tablo adları nyctaxi_trip ve nyctaxi_fare değiştirmek unutmayın). Benzer denemeler, çok sınıflı sınıflandırma ve regresyon sorunları için oluşturulabilir.

![Azure ML eğitimi][10]

> [!IMPORTANT]
> Modelleme verileri ayıklama ve sorgu örnekler örnekleme önceki bölümlerde sağlanan **üç modelleme alıştırmalar için tüm etiketleri sorguda dahil edilen**. Her modelleme alıştırmalarda önemli bir (gerekli) adım **hariç** diğer iki sorunlar ve diğer gereksiz etiketlerini **hedef sızıntıları**. Örneğin, ikili Sınıflandırma, etiket kullanırken **Eğimli** ve alanları dışarıda **İpucu\_sınıfı**, **İpucu\_tutar**ve **toplam\_tutar**. İpucu kapsıyor beri ikinci hedef sızıntıları olan Ücretli.
> 
> Tüm gereksiz sütunları hariç tutma veya sızıntıları hedeflemek için kullanabilir [Select Columns in Dataset sütun] [ select-columns] modülü veya [Düzenle meta veri][edit-metadata]. Daha fazla bilgi için bkz: [Select Columns in Dataset sütun] [ select-columns] ve [Düzenle meta veri] [ edit-metadata] başvuru sayfaları.
> 
> 

## <a name="mldeploy"></a>Azure Machine Learning modellerini dağıtma
Modeliniz hazır olduğunda, kolayca bunu deneme doğrudan bir web hizmeti olarak dağıtabilirsiniz. Azure ML web hizmetleri dağıtma hakkında daha fazla bilgi için bkz: [bir Azure Machine Learning web hizmetini dağıtma](../studio/publish-a-machine-learning-web-service.md).

Yeni bir web hizmeti dağıtmak için aktarmanız gerekir:

1. Puanlama bir deneme oluşturma.
2. Web hizmeti dağıtın.

Gelen Puanlama bir deneme oluşturmak için bir **tamamlandı** deneme eğitim, tıklatın **oluşturma PUANLAMA DENEMELER** alt eylem çubuğunda.

![Azure Puanlama][18]

Azure Machine Learning eğitim denemenizi bileşenlerini temel alan bir Puanlama deneme oluşturmayı deneyecek. Özellikle, çıkarır:

1. Eğitilen modeli kaydedin ve model eğitim modüllerini kaldırın.
2. Bir mantıksal tanımlamak **giriş bağlantı noktası** beklenen giriş veri şeması temsil etmek için.
3. Bir mantıksal tanımlamak **çıkış bağlantı noktasına** beklenen web hizmeti çıkış şeması temsil etmek için.

Puanlama deneme oluşturulduğunda, gözden geçirin ve gerektiği gibi ayarlayın olun. Hizmeti çağrıldığında bunlar kullanılabilir olmayacak şekilde girdi veri kümesi ve/veya sorgu etiket alanları hariç biriyle değiştirin tipik bir ayarlanmasıdır. Ayrıca bazı kayıtları için giriş veri kümesi ve/veya sorgu boyutunu azaltmak için iyi bir uygulama olur giriş şemasını belirtmek yeterli. Çıkış bağlantı noktasına için tüm giriş alanları çıkarmak ve yalnızca eklemek için ortak olan **skoru etiketleri** ve **skoru olasılıklar** çıkış kullanarak [Select Columns in Dataset sütun] [ select-columns] modülü.

Deneme Puanlama bir örneği aşağıdaki şekilde sağlanır. Hazır olduğunuzda dağıtmak tıklatın **yayımlama WEB hizmeti** alt eylem çubuğunda düğmesi.

![Azure ML yayımlama][11]

## <a name="summary"></a>Özet
Ne Biz bu gözden geçirme öğreticide yaptığınızdan olduðunu üzere büyük ortak sahip bir veri kümesi, çalışan bir Azure veri bilimi ortamı için takım veri bilimi işlemini, tüm veri alım model eğitim ve ardından için aracılığıyla alma oluşturduğunuz bir Azure Machine Learning web hizmeti dağıtımı.

### <a name="license-information"></a>Lisans bilgileri
Bu örnek gözden geçirme ve kendi eşlik eden komut dosyaları ve IPython notebook(s) paylaşıldığı Microsoft tarafından MIT lisansı altında. Lütfen LICENSE.txt dosyayı daha fazla ayrıntı için örnek kod github'da dizininde iade edin.

## <a name="references"></a>Başvurular
• [Andrés Monroy NYC ücreti dönüşleri indirme sayfası](http://www.andresmh.com/nyctaxitrips/)  
• [FOILing NYC ait ücreti seyahat veri Chris Whong tarafından](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• [NYC ücreti ve Limousine devreye sokmak araştırma ve istatistikleri](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)

[1]: ./media/sqldw-walkthrough/sql-walkthrough_26_1.png
[2]: ./media/sqldw-walkthrough/sql-walkthrough_28_1.png
[3]: ./media/sqldw-walkthrough/sql-walkthrough_35_1.png
[4]: ./media/sqldw-walkthrough/sql-walkthrough_36_1.png
[5]: ./media/sqldw-walkthrough/sql-walkthrough_39_1.png
[6]: ./media/sqldw-walkthrough/sql-walkthrough_42_1.png
[7]: ./media/sqldw-walkthrough/sql-walkthrough_44_1.png
[8]: ./media/sqldw-walkthrough/sql-walkthrough_46_1.png
[9]: ./media/sqldw-walkthrough/sql-walkthrough_71_1.png
[10]: ./media/sqldw-walkthrough/azuremltrain.png
[11]: ./media/sqldw-walkthrough/azuremlpublish.png
[12]: ./media/sqldw-walkthrough/ssmsconnect.png
[13]: ./media/sqldw-walkthrough/executescript.png
[14]: ./media/sqldw-walkthrough/sqlserverproperties.png
[15]: ./media/sqldw-walkthrough/sqldefaultdirs.png
[16]: ./media/sqldw-walkthrough/bulkimport.png
[17]: ./media/sqldw-walkthrough/amlreader.png
[18]: ./media/sqldw-walkthrough/amlscoring.png
[19]: ./media/sqldw-walkthrough/ps_download_scripts.png
[20]: ./media/sqldw-walkthrough/ps_load_data.png
[21]: ./media/sqldw-walkthrough/azcopy-overwrite.png
[22]: ./media/sqldw-walkthrough/ipnb-service-aml-1.png
[23]: ./media/sqldw-walkthrough/ipnb-service-aml-2.png
[24]: ./media/sqldw-walkthrough/ipnb-service-aml-3.png
[25]: ./media/sqldw-walkthrough/ipnb-service-aml-4.png
[26]: ./media/sqldw-walkthrough/tip_class_hist_1.png


<!-- Module References -->
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
