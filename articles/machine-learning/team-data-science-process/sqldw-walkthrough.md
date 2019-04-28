---
title: Derleme ve SQL veri ambarı - Team Data Science Process kullanarak bir model dağıtma
description: Oluşturun ve makine öğrenme modeli SQL veri ambarı ile genel kullanıma açık bir veri kümesini kullanarak dağıtın.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/24/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: e27c4462e7137145917d1284bfb6f8838e8a090b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61045484"
---
# <a name="the-team-data-science-process-in-action-using-sql-data-warehouse"></a>Team Data Science Process'in çalışması: SQL veri ambarı kullanma
Bu öğreticide, derlemeye ve dağıtmaya SQL veri ambarı'nı (SQL DW) kullanarak makine öğrenme modeli aracılığıyla genel kullanıma açık bir veri kümesi için--inceleyeceğiz [NYC taksi Gelişlerin](https://www.andresmh.com/nyctaxitrips/) veri kümesi. Oluşturulan ikili sınıflandırma modelinde, bir ipucu bir seyahat için ödeme yapılır ve çok sınıflı sınıflandırma ve regresyon modellerini ayrıca dağıtım Ücretli ipucu tutarlarının tahmin açıklanan olup olmadığını tahmin eder.

Yordamdan sonraki [Team Data Science işlem (TDSP)](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/) iş akışı. Bir veri bilimi ortamını ayarlama göstereceğiz SQL DW'ye veri yükleme ve nasıl mühendisi ve verileri araştırmak için SQL DW veya Ipython Notebook kullanma modeline sahiptir. Ardından nasıl oluşturacağınızı ve Azure Machine Learning ile model dağıtma göstereceğiz.

## <a name="dataset"></a>NYC taksi Gelişlerin veri kümesi
Yaklaşık 20 GB sıkıştırılmış CSV dosyalar (sıkıştırmadan ~ 48 GB), NYC taksi seyahat verilerini oluşuyorsa, 173 milyondan fazla bireysel gelişlerin ve fares kaydetmek için her bir seyahat Ücretli. Her bir seyahat kaydı alma ve dropoff konumları ve süreleri, anonimleştirilmiş hack (sürücü) lisans numarası ve medallion (taksi'nın benzersiz tanımlayıcı) sayısını içerir. Veriler tüm dönüş 2013 yılında kapsar ve aşağıdaki iki veri kümesi için her ay sağlanır:

1. **Trip_data.csv** dosyası Yolcuların, toplama ve dropoff noktaları, seyahat süresini ve seyahat uzunluğu sayısı gibi seyahat ayrıntıları içerir. Birkaç örnek kayıt şunlardır:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
2. **Trip_fare.csv** dosyası her bir gidiş dönüş için ödeme türü, taksi tutar, ek ücret ve vergiler, ipuçları ve Ücretli geçişler, gibi Ücretli taksi ve ödenen toplam tutarı ayrıntılarını içerir. Birkaç örnek kayıt şunlardır:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

**Benzersiz anahtar** seyahat katılmak için kullanılan\_veri ve seyahat\_taksi, aşağıdaki üç alanı oluşur:

* medallion,
* HACK\_lisans ve
* Toplama\_datetime.

## <a name="mltasks"></a>Üç tür tahmin görevleri adresi
Şu formüle göre üç tahmin sorunları *İpucu\_tutarı* görevleri modelleme üç tür göstermek için:

1. **İkili sınıflandırma**: Tahmin etmek için olup olmadığını bir ipucu Ücretli bir seyahat için yani bir *İpucu\_tutarı* büyük pozitif bir örnek 0 TL'dir daha açıkken bir *İpucu\_tutarı* 0 veya negatif bir örnektir.
2. **Sınıflı sınıflandırma**: İpucu aralığını tahmin etmek için seyahat Ücretli. Biz bölmek *İpucu\_tutarı* beş depo veya sınıflar:

        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20
3. **Regresyon görev**: İpucu miktarı tahmin etmek için bir seyahat Ücretli.

## <a name="setup"></a>Gelişmiş analiz için Azure veri bilimi ortamı ayarlama
Azure veri bilimi ortamı oluşturmanız için bu adımları izleyin.

**Kendi Azure blob depolama hesabı oluşturma**

* Kendi Azure blob depolama sağladığınızda, Azure blob depolama içinde veya için mümkün olduğunca yakın bir coğrafi konum seçin **Orta Güney ABD**, NYC taksi verilerinin nerede depolanacağını olduğu. Kendi depolama hesabında bir kapsayıcı için ortak bir blob depolama kapsayıcısından AzCopy kullanarak veri kopyalanır. Daha yakından Azure blob depolama alanınızın Güney Orta ABD için bu görevi (adım 4) daha hızlı tamamlanır.
* Kendi Azure depolama hesabı oluşturmak için ana hatlarıyla belirtilen adımları izleyin. [Azure depolama hesapları hakkında](../../storage/common/storage-create-storage-account.md). Bu kılavuzda daha sonra gerekli şekilde aşağıdaki depolama hesabı kimlik değerleri hakkında notlar alın emin olun.

  * **Depolama hesabı adı**
  * **Depolama hesabı anahtarı**
  * **Kapsayıcı adı** (Bu Azure blob depolamada depolanan verilerin istediğiniz)

**Azure SQL DW örneğinizin sağlayın.**
Adresindeki belgelere izleyin [SQL veri ambarı oluşturma](../../sql-data-warehouse/sql-data-warehouse-get-started-provision.md) bir SQL Data Warehouse örneğine sağlamak için. Sonraki adımlarda kullanılacak olan aşağıdaki SQL veri ambarı kimlik bilgileri üzerinde gösterimler yaptığınızdan emin olun.

* **Sunucu adı**: \<sunucu adı >. database.windows.net
* **SQLDW (veritabanı) adı**
* **Kullanıcı Adı**
* **Parola**

**Visual Studio ve SQL Server veri Araçları'nı yükleyin.** Yönergeler için [yükleme Visual Studio 2015 ve/veya SQL veri ambarı için SSDT (SQL Server veri araçları)](../../sql-data-warehouse/sql-data-warehouse-install-visual-studio.md).

**Visual Studio ile Azure SQL DW bağlanın.** Adım 1 ve 2. yönergeler için bkz [Visual Studio ile Azure SQL Data Warehouse'a bağlanma](../../sql-data-warehouse/sql-data-warehouse-connect-overview.md).

> [!NOTE]
> Aşağıdaki SQL sorgusunu, oluşturduğunuz (yerine connect konu, 3. adımında sağlanan sorgu), SQL veri ambarı veritabanı için çalıştırmak **create master key**.
>
>

    BEGIN TRY
           --Try to create the master key
        CREATE MASTER KEY
    END TRY
    BEGIN CATCH
           --If the master key exists, do nothing
    END CATCH;

**Azure aboneliğinizde bir Azure Machine Learning çalışma alanı oluşturun.** Yönergeler için [bir Azure Machine Learning çalışma alanı oluşturma](../studio/create-workspace.md).

## <a name="getdata"></a>SQL Data Warehouse'a veri yükleme
Bir Windows PowerShell komutu konsolunu açın. Aşağıdaki PowerShell komutunu çalıştırın SQL örneği yükleme komutlarını komut biz sizinle paylaştığı Github'da parametresiyle belirtirsiniz yerel bir dizin dosyalarını *- DestDir*. Parametre değerini değiştirebilirsiniz *- DestDir* herhangi bir yerel dizine. Varsa *- DestDir* yok, PowerShell betiği tarafından oluşturulur.

> [!NOTE]
> Gerekebilir **yönetici olarak çalıştır** aşağıdaki PowerShell Betiği, yürütülürken, *DestDir* dizinin Yöneticisi ayrıcalığı oluşturmak veya ona yazmak için yeterlidir.
>
>

    $source = "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/Download_Scripts_SQLDW_Walkthrough.ps1"
    $ps1_dest = "$pwd\Download_Scripts_SQLDW_Walkthrough.ps1"
    $wc = New-Object System.Net.WebClient
    $wc.DownloadFile($source, $ps1_dest)
    .\Download_Scripts_SQLDW_Walkthrough.ps1 –DestDir 'C:\tempSQLDW'

Geçerli çalışma dizininize değişikliklerini başarılı yürütme sonrasında *- DestDir*. Ekran görebilmek için aşağıdaki gibi:

![Geçerli çalışma dizini değişiklikleri][19]

İçinde *- DestDir*, aşağıdaki PowerShell betiğini Yönetici modunda çalıştırın:

    ./SQLDW_Data_Import.ps1

PowerShell komut dosyası ilk kez çalıştığında, giriş, Azure SQL DW ve Azure blob depolama hesabınıza bilgileri istenir. Bu PowerShell Betiği tamamlandıktan sonra ilk kez kimlik bilgilerini çalıştıran, giriş SQLDW.conf mevcut çalışma dizininde bir yapılandırma dosyasına yazılmış. Bu PowerShell Betiği gelecekteki yürütülmesi tüm gerekli parametreleri bu yapılandırma dosyasından okumak için seçeneği vardır. Bazı parametreler değiştirmeniz gerekiyorsa, bu yapılandırma dosyasını silme ve parametrelerin değerleri istendiği gibi giriş yapma giriş ekranında istemi bağlı parametreleri veya, SQLDW.confdosyayıdüzenleyerekparametredeğerlerinideğiştirmekiçinseçebilirsiniz *- DestDir* dizin.

> [!NOTE]
> Parametreler doğrudan SQLDW.conf dosyasından okurken, Azure SQL DW mevcut olanla Şema ad çakışmalarını önlemek için 3 haneli rastgele bir sayı için şema adı SQLDW.conf dosyasından her çalıştırma için varsayılan şema adı olarak eklenir. PowerShell Betiği, bir şema adı isteyebilir: kullanıcı kararımıza adı belirtilebilir.
>
>

Bu **PowerShell Betiği** dosyası aşağıdaki görevleri tamamlar:

* **İndirir ve yükler AzCopy**, AzCopy zaten yüklü değilse

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
* **Verilerini özel olarak blob depolama hesabınıza kopyalar** AzCopy ile ortak blobdan

        Write-Host "AzCopy is copying data from public blob to yo storage account. It may take a while..." -ForegroundColor "Yellow"
        $start_time = Get-Date
        AzCopy.exe /Source:$Source /Dest:$DestURL /DestKey:$StorageAccountKey /S
        $end_time = Get-Date
        $time_span = $end_time - $start_time
        $total_seconds = [math]::Round($time_span.TotalSeconds,2)
        Write-Host "AzCopy finished copying data. Please check your storage account to verify." -ForegroundColor "Yellow"
        Write-Host "This step (copying data from public blob to your storage account) takes $total_seconds seconds." -ForegroundColor "Green"
* **Polybase kullanarak Azure SQL DW'ye (LoadDataToSQLDW.sql yürüterek) verileri yükler** özel blob depolama hesabınızda aşağıdaki komutlarla.

  * Bir şema oluşturun

          EXEC (''CREATE SCHEMA {schemaname};'');
  * Veritabanı kapsamlı kimlik bilgileri oluşturma

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
  * Csv dosyaları için bir dış dosya biçimine oluşturun. Veri sıkıştırılmamış ve alanlar dikey çizgi karakteriyle ayrılır.

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
  * Dış taksi ve NYC taksi veri kümesi için seyahat tablolar Azure blob depolama alanında oluşturun.

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

    - Azure blob depolama alanındaki dış tablodan SQL Data Warehouse'a veri yükleme

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

    - Bir örnek veri tablosu (NYCTaxi_Sample) oluşturma ve verileri için seyahat ve taksi tablolarını SQL sorguları seçeneğini ekleyin. (Bu kılavuzun bazı adımlar gerekir bu örnek tabloyu kullanın.)

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

Depolama hesaplarınızı coğrafi konumunu, yükleme süreleri etkiler.

> [!NOTE]
> Coğrafi konumunu özel blob depolama hesabınıza bağlı olarak, özel depolama hesabınıza ortak BLOB'dan veri kopyalama işlemi yaklaşık 15 dakika alabilir ya da daha uzun bir süre ve işleme için Azure depolama hesabınızdan veri yükleme SQL DW, 20 dakika sürebilir veya daha uzun.
>
>

Yinelenen kaynak ve hedef dosya varsa, hangi karar vermeniz gerekir.

> [!NOTE]
> Kopyalanacak .csv dosyalarını özel blob depolama hesabınıza ortak blob depolama, özel bir blob depolama hesabınızda zaten var, AzCopy üzerine yazmak isteyip istemediğinizi sorar. Üzerine yazmak istemiyorsanız giriş **n** istendiğinde. Üzerine yazmak istiyorsanız **tüm** , giriş **bir** istendiğinde. Ayrıca giriş **y** ayrı ayrı .csv dosyalarını üzerine yazmak için.
>
>

![AzCopy çıktısı][21]

Kendi verilerinizi kullanabilirsiniz. Verilerinizi şirket içi makinenizde, gerçek hayatta uygulamanızda varsa, yine de şirket içi verileri, özel bir Azure blob depolama alanına yüklemek için AzCopy kullanabilirsiniz. Yalnızca değiştirmek gereken **kaynak** konumu `$Source = "http://getgoing.blob.core.windows.net/public/nyctaxidataset"`, verilerinizi içeren yerel bir dizin için PowerShell betik dosyasının AzCopy komutunda.

> [!TIP]
> Verilerinizi gerçek hayatta uygulamanızda zaten özel Azure blob depolamanızda ise, PowerShell betiğinde AzCopy adımı atlayın ve doğrudan Azure SQL DW'ye veri karşıya yükleyin. Bu ek düzenlemeler verilerinizi biçimine uygun hale getirmek için komut dosyası gerektirir.
>
>

Bu üç dosyayı anında sonra çalıştığınız hazır olacak şekilde bu Powershell betiği ayrıca Azure SQL DW bilgilerinde veri araştırma örnek dosyalarına SQLDW_Explorations.sql SQLDW_Explorations.ipynb ve SQLDW_Explorations_Scripts.py yararlanmanıza imkan sağlar PowerShell betiğini tamamlar.

Başarılı yürütme sonrasında bir ekran görürsünüz. aşağıdaki gibi:

![Başarılı betik yürütme çıktısı][20]

## <a name="dbexplore"></a>Veri keşfi ve özellik Mühendisliği, Azure SQL veri ambarı
Bu bölümde, veri keşfi ve özellik oluşturma kullanarak doğrudan Azure SQL DW karşı SQL sorgu çalıştırarak gerçekleştiririz **Visual Studio veri Araçları**. Bu bölümde kullanılan tüm SQL sorguları adlı örnek betikte bulunabilir *SQLDW_Explorations.sql*. Bu dosya yerel dizininize PowerShell betiği tarafından zaten yüklendi. Buradan ayrıca alabilirsiniz [GitHub](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/SQLDW_Explorations.sql). Ancak GitHub dosyasında prize takılı Azure SQL DW bilgilerine sahip değil.

SQL DW oturum açma adı ve parola ile Visual Studio kullanarak, Azure SQL DW bağlanmak ve açık **SQL Nesne Gezgini** tablo ve veritabanı aktarıldı onaylamak için. Alma *SQLDW_Explorations.sql* dosya.

> [!NOTE]
> Bir paralel veri ambarı'nı (PDW) sorgu Düzenleyicisi'ni açmak için **yeni sorgu** , PDW içinde seçili durumdayken komutunun **SQL Nesne Gezgini**. Standart SQL sorgu Düzenleyicisi'ni PDW tarafından desteklenmiyor.
>
>

Veri türü şunlardır keşfi ve özellik oluşturma görevleri gerçekleştirdiği Bu bölümde:

* Değişen zaman pencereleri bazı alanların veri dağıtımları keşfedin.
* Veri Kalitesi boylam ve enlem alanlarının araştırın.
* İkili ve çok sınıflı sınıflandırma etiketlerine göre oluşturmak **İpucu\_tutarı**.
* Özellikleri oluşturma ve işlem/seyahat uzaklıkları karşılaştırın.
* İki tabloyu birleştirmek ve modelleri oluşturmak için kullanılan rastgele bir örnek ayıklayın.

### <a name="data-import-verification"></a>Veri içeri aktarma doğrulama
Bu sorguların hızlı bir satır sayısı doğrulaması sağlamak ve daha önce Polybase'nın Paralel toplu kullanarak doldurulmuş tablolardaki sütunlara içeri aktarma

    -- Report number of rows in table <nyctaxi_trip> without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_trip>')

    -- Report number of columns in table <nyctaxi_trip>
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = '<nyctaxi_trip>' AND table_schema = '<schemaname>'

**Çıkış:** 173,179,759 satırları ve sütunları 14 almanız gerekir.

### <a name="exploration-trip-distribution-by-medallion"></a>Keşfetme: Seyahat dağıtım medallion tarafından
Bu örnekte sorgu, belirtilen süre içinde 100'den fazla gelişlerin tamamlandı medallions (taksi numaraları) tanımlar. Bölümleme düzeni koşuluna bağlıdır beri sorgu bölümlenmiş tabloda erişimden avantaj elde edecektir **toplama\_datetime**. Tam bir veri kümesinin sorgulanmasını ayrıca bölümlenmiş tablosunu kullanmak ve/veya dizin tarama yapar.

    SELECT medallion, COUNT(*)
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

**Çıkış:** Sorgu 13,369 medallions (taksiler) ve 2013'te tarafından tamamladığınız seyahat sayısını belirten bir satır içeren bir tablo döndürmelidir. Son sütun tamamlandı gelişlerin sayısını içerir.

### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>Keşfetme: Seyahat dağıtım medallion ve hack_license
Bu örnekte medallions (taksi numaraları) tanımlar ve hack_license (sürücüler) tamamlanan birden fazla 100 gelişlerin belirtilen bir süre içinde numaralandırır.

    SELECT medallion, hack_license, COUNT(*)
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

**Çıkış:** Sorgu 13,369 araba/DRIVER 2013'te, daha fazla 100 gelişlerin tamamladınız kimlikleri belirtme 13,369 satırları içeren tablo döndürmelidir. Son sütun tamamlandı gelişlerin sayısını içerir.

### <a name="data-quality-assessment-verify-records-with-incorrect-longitude-andor-latitude"></a>Veri Kalitesi değerlendirmesi: Yanlış boylam ve/veya enlem kayıtlarla doğrulayın
Herhangi bir boylam ve/veya enlem alanı ya da geçersiz bir değer içeriyorsa, bu örnekte araştırır (radian derece -90 ile 90 arasında olmalıdır), veya (0, 0) koordinatları.

    SELECT COUNT(*) FROM <schemaname>.<nyctaxi_trip>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

**Çıkış:** Sorgu geçersiz bir boylam ve/veya enlem alanlara sahip 837,467 gelişlerin döndürür.

### <a name="exploration-tipped-vs-not-tipped-trips-distribution"></a>Keşfetme: Eğimli değil Eğimli gelişlerin dağıtım
Bu örnek, belirli bir süre içinde (veya buraya ayarlanır gibi tam yıl kapsayan, tam veri kümesi) Eğimli değil, sayı Eğimli dönüş sayısı bulur. Bu dağıtım için ikili sınıflandırma modelleme daha sonra kullanılmak üzere ikili etiket dağılımı yansıtır.

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM <schemaname>.<nyctaxi_fare>
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

**Çıkış:** Sorgu aşağıdaki ipucu sıklıklardan 2013 yılı döndürmesi gerekir: 90,447,622 Eğimli ve 82,264,709 değil-Eğimli.

### <a name="exploration-tip-classrange-distribution"></a>Keşfetme: Sınıf/aralığı dağıtım İpucu
Bu örnek, belirli bir süre içinde (veya kapsayan tam yıl, tam veri kümesi) dağıtım ipucu aralıklarının hesaplar. Daha sonra çok sınıflı sınıflandırma model için kullanılacak etiket sınıfların dağıtımıdır.

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

**Çıkış:**

| tip_class | tip_freq |
| --- | --- |
| 1 |82230915 |
| 2 |6198803 |
| 3 |1932223 |
| 0 |82264625 |
| 4 |85765 |

### <a name="exploration-compute-and-compare-trip-distance"></a>Keşfetme: İşlem ve seyahat uzaklık karşılaştırın
Bu örnek dönüştürür alma ve dropoff boylam ve enlem SQL coğrafi konum için işaret, SQL Coğrafya noktaları fark kullanarak seyahat uzaklığı hesaplar ve sonuçları karşılaştırma için rastgele oluşturulmuş bir örnek döndürür. Bu örnek yalnızca daha önce veri kalitesi değerlendirme sorgusu kullanarak geçerli koordinat sonuçları sınırlar.

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

### <a name="feature-engineering-using-sql-functions"></a>SQL işlevleri kullanarak özellik Mühendisliği
Bazı durumlarda SQL işlevleri, özellik Mühendisliği için verimli bir seçenek olabilir. Bu kılavuzda, toplama ve dropoff konumlar arasında doğrudan uzaklığı hesaplamak için bir SQL işlev tanımladık. Aşağıdaki SQL komut dosyalarını çalıştırabileceğiniz **Visual Studio veri Araçları**.

Distance işlevi tanımlayan SQL komut dosyası aşağıda verilmiştir.

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

Özellikler, SQL sorgunuzun oluşturmak için bu işlevi çağırmak için bir örnek aşağıda verilmiştir:

    -- Sample query to call the function to create features
    SELECT pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS DirectDistance
    FROM <schemaname>.<nyctaxi_trip>
    WHERE datepart("mi",pickup_datetime)=1
    AND CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND pickup_longitude != '0' AND dropoff_longitude != '0'

**Çıkış:** Bu sorgu, toplama ve dropoff latitudes longitudes ve karşılık gelen doğrudan uzaklıkları mil içindeki (2,803,538 satırları) içeren bir tablo oluşturur. İlk 3 satır için sonuçları şunlardır:

|  | pickup_latitude | pickup_longitude | dropoff_latitude | dropoff_longitude | DirectDistance |
| --- | --- | --- | --- | --- | --- |
| 1 |40.731804 |-74.001083 |40.736622 |-73.988953 |.7169601222 |
| 2 |40.715794 |-74,010635 |40.725338 |-74.00399 |.7448343721 |
| 3 |40.761456 |-73.999886 |40.766544 |-73.988228 |0.7037227967 |

### <a name="prepare-data-for-model-building"></a>Veri hazırlama için model oluşturma
Aşağıdaki Sorguda birleştirme **nyctaxi\_seyahat** ve **nyctaxi\_taksi** tablolar, bir ikili sınıflandırma etiketi oluşturur **Eğimli**, çok sınıflı sınıflandırma etiketi **İpucu\_sınıfı**ve bir örnek tam Birleşmiş kümesinden ayıklar. Örnekleme, toplama zamana dayalı gelişlerin kümesini alarak gerçekleştirilir.  Bu sorgu kopyalanır, ardından doğrudan yapıştırılan [Azure Machine Learning Studio](https://studio.azureml.net) [verileri içeri aktarma] [ import-data] için SQL veritabanı örneğinde doğrudan veri alma Modülü Azure. Sorgu yanlış kayıtlarla dışlar (0, 0) koordinatları.

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

Azure Machine Learning için devam etmeye hazır olduğunuzda, şunlardan birini yapabilirsiniz:

1. Son SQL sorgusunun ayıklayın ve örnek veriler ve doğrudan sorgu kopyala-yapıştır kaydetmek bir [verileri içeri aktarma] [ import-data] modülü Azure Machine learning'de veya
2. Model içinde yeni bir SQL DW tablo oluşturmak için kullanmayı planladığınız örneklenen ve mühendislik uygulanan verileri kalıcı hale getirmenize ve yeni tabloya kullanma [verileri içeri aktarma] [ import-data] Azure Machine learning'de modülü. PowerShell Betiği önceki adımda bunu sizin için yapmıştır. Bu tablodaki verileri içeri aktarma modülü doğrudan okuyabilir.

## <a name="ipnb"></a>Veri keşfi ve özellik Mühendisliği, Ipython notebook
Bu bölümde, bir veri keşfi ve her iki Python kullanarak özellik nesil gerçekleştiririz ve SQL DW SQL sorgularını daha önce oluşturduğunuz. Adlandırılmış bir örnek Ipython notebook **SQLDW_Explorations.ipynb** ve Python betik dosyası **SQLDW_Explorations_Scripts.py** yerel dizininize yüklendi. Bunlar ayrıca kullanılabilir [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/SQLDW). Bu iki dosyayı Python betiklerini aynıdır. Python betik dosyası Ipython not defteri olmaması durumunda size sağlanır. Bu iki örnek Python dosyaları altında tasarlanmıştır **Python 2.7**.

Gerekli Azure SQL DW bilgileri Ipython Notebook ve Python betik dosyasını yerel makinenize indirilen örnek PowerShell betiği tarafından daha önce takıldığını. Bunlar herhangi bir değişiklik yapılmadan yürütülebilir.

AzureML bir çalışma alanı zaten ayarladıysanız, doğrudan örnek Ipython Notebook AzureML Ipython Notebook hizmetine yükleyebilir ve onu çalıştırmaya başlayın. AzureML Ipython Notebook hizmetine yüklemek için aşağıdaki adımları şunlardır:

1. AzureML çalışma alanınıza oturum açın, üst kısmında "Studio" tıklayın ve web sayfasının sol tarafındaki "dizüstü bilgisayarlar" tıklayın.

    ![Studio Not DEFTERLERİNİ tıklatın][22]
2. "Yeni" web sayfasının sol alt köşedeki tıklayın ve "Python 2". Ardından, not defteri için bir ad sağlayın ve yeni boş Ipython not defteri oluşturmak için onay işaretine tıklayın.

    ![YENİ'ye tıklayın, sonra Python 2'yi seçin][23]
3. Yeni Ipython not defteri sol üst köşesindeki "Jupyter" simgesine tıklayın.

    ![Jupyter simgesine tıklayın][24]
4. Sürükle ve bırak örnek Ipython Notebook için **ağaç** sayfasında AzureML Ipython Notebook hizmeti ve tıklatın **karşıya**. Ardından, Ipython Notebook örnek AzureML Ipython Notebook hizmete yüklenir.

    ![Yükle'yi tıklatın][25]

Örneği çalıştırmak için Ipython Notebook veya Python, paketleri gereken aşağıdaki Python betik dosyası. AzureML Ipython Notebook hizmeti kullanıyorsanız, bu paketlerin önceden yüklü olmuştur.

    - pandas
    - numpy
    - matplotlib
    - pyodbc
    - PyTables

Önerilen diziyi Gelişmiş analitik çözümleri ile büyük veri üzerinde AzureML oluştururken aşağıdaki gibidir:

* Verilerin küçük bir örnek, bir bellek içi veri çerçevesine okuyun.
* Bazı görselleştirmeler ve araştırmaları örneklenen verileri kullanarak gerçekleştirin.
* Örnek verileri kullanarak özellik Mühendisliği ile denemeler yapın.
* Büyük veri keşfi, veri işleme ve özellik Mühendisliği için SQL DW karşı doğrudan SQL sorguları göndermek amacıyla Python'ı kullanın.
* Azure Machine Learning modeli oluşturma için uygun olması için örnek boyutunu karar verin.

Aşağıdakilere birkaç veri keşfi, veri Görselleştirme ve örnekler mühendislik özelliği olan. Daha fazla veri araştırmaları, Ipython Notebook örnek ve örnek Python betik dosyası bulunamadı.

### <a name="initialize-database-credentials"></a>Veritabanı kimlik bilgileri Başlat
Veritabanı bağlantı ayarlarınızı aşağıdaki değişkenleri başlatın:

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database driver>

### <a name="create-database-connection"></a>Veritabanı bağlantısı oluşturma
Veritabanı bağlantısı oluşturur. bağlantı dizesi aşağıda verilmiştir.

    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

### <a name="report-number-of-rows-and-columns-in-table-nyctaxitrip"></a>< Nyctaxi_trip > Tablo içindeki satırları ve sütunları rapor sayısı
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

### <a name="report-number-of-rows-and-columns-in-table-nyctaxifare"></a>< Nyctaxi_fare > Tablo içindeki satırları ve sütunları rapor sayısı
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
* Toplam sütun sayısı 11 =

### <a name="read-in-a-small-data-sample-from-the-sql-data-warehouse-database"></a>Okuma-içinde bir küçük veri örneği SQL veri ambarı veritabanından
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
Satır ve sütun sayısı, alınan = (1000 21).

### <a name="descriptive-statistics"></a>Açıklayıcı istatistikleri
Örneklenen verileri araştırmak hazırsınız. Bazı açıklayıcı istatistiklerini bakarak ile Başlat **seyahat\_uzaklık** (veya seçtiğiniz belirtmek üzere diğer tüm alanlar).

    df1['trip_distance'].describe()

### <a name="visualization-box-plot-example"></a>Görselleştirme: Kutu Diagram örneği
Sonraki Kutu Çizimi quantiles görselleştirmek seyahat uzaklığı için atacağız.

    df1.boxplot(column='trip_distance',return_type='dict')

![Kutusu çizim çıkış][1]

### <a name="visualization-distribution-plot-example"></a>Görselleştirme: Dağıtım Diagram örneği
Dağıtım ve örneklenen seyahat uzaklıkları histogramını görselleştirin çizimleri.

    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![Dağıtım çizim çıkış][2]

### <a name="visualization-bar-and-line-plots"></a>Görselleştirme: Çubuk ve çizgi çizimler
Bu örnekte, beş depo seyahat uzaklığı bin ve Gruplama sonuçları görselleştirebilirsiniz.

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

Yukarıdaki çubuk bin dağıtım çizim veya çizim ile satır:

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![Çubuk grafik çıkış][3]

ve

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![Satırı çizim çıkışı][4]

### <a name="visualization-scatterplot-examples"></a>Görselleştirme: Dağılım Grafiği örnekleri
Dağılım grafiğinde noktalara arasında göstereceğiz **seyahat\_zaman\_içinde\_saniye** ve **seyahat\_uzaklık** herhangi bir ilişki olup olmadığını görmek için

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![Dağılım Grafiği çıkış saati uzaklığı arasındaki ilişki][6]

Benzer şekilde biz arasındaki ilişkiyi kontrol edebilirsiniz **oranı\_kod** ve **seyahat\_uzaklık**.

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![Dağılım Grafiği çıktısını kod ve uzaklık arasındaki ilişki][8]

### <a name="data-exploration-on-sampled-data-using-sql-queries-in-ipython-notebook"></a>Veri keşfi Ipython notebook SQL sorgularını kullanarak örneklenen verileri
Bu bölümde, yukarıda oluşturduğumuz yeni tablodaki kalıcı örnek verileri kullanarak veri dağıtımları inceleyeceğiz. Not benzer araştırmaları özgün tabloda kullanılarak gerçekleştirilebilir.

#### <a name="exploration-report-number-of-rows-and-columns-in-the-sampled-table"></a>Keşfetme: Örneklenen tablo içindeki satırları ve sütunları rapor sayısı
    nrows = pd.read_sql('''SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_sample>')''', conn)
    print 'Number of rows in sample = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''SELECT count(*) FROM information_schema.columns WHERE table_name = ('<nyctaxi_sample>') AND table_schema = '<schemaname>'''', conn)
    print 'Number of columns in sample = %d' % ncols.iloc[0,0]

#### <a name="exploration-tippednot-tripped-distribution"></a>Keşfetme: Dağıtım dönüş Eğimli değil
    query = '''
        SELECT tipped, count(*) AS tip_freq
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY tipped
        '''

    pd.read_sql(query, conn)

#### <a name="exploration-tip-class-distribution"></a>Keşfetme: İpucu dağıtımıdır
    query = '''
        SELECT tip_class, count(*) AS tip_freq
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY tip_class
    '''

    tip_class_dist = pd.read_sql(query, conn)

#### <a name="exploration-plot-the-tip-distribution-by-class"></a>Keşfetme: Sınıfı tarafından ipucu dağıtım Çiz
    tip_class_dist['tip_freq'].plot(kind='bar')

![#26 Çiz][26]

#### <a name="exploration-daily-distribution-of-trips"></a>Keşfetme: Dönüş günlük dağıtımı
    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-per-medallion"></a>Keşfetme: Seyahat dağıtım medallion başına
    query = '''
        SELECT medallion,count(*) AS c
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-by-medallion-and-hack-license"></a>Keşfetme: Seyahat dağıtım medallion ve hack lisans tarafından
    query = '''select medallion, hack_license,count(*) from <schemaname>.<nyctaxi_sample> group by medallion, hack_license'''
    pd.read_sql(query,conn)


#### <a name="exploration-trip-time-distribution"></a>Keşfetme: Seyahat süresi dağılımı
    query = '''select trip_time_in_secs, count(*) from <schemaname>.<nyctaxi_sample> group by trip_time_in_secs order by count(*) desc'''
    pd.read_sql(query,conn)

#### <a name="exploration-trip-distance-distribution"></a>Keşfetme: Seyahat uzaklık dağıtım
    query = '''select floor(trip_distance/5)*5 as tripbin, count(*) from <schemaname>.<nyctaxi_sample> group by floor(trip_distance/5)*5 order by count(*) desc'''
    pd.read_sql(query,conn)

#### <a name="exploration-payment-type-distribution"></a>Keşfetme: Ödeme türü dağılımı
    query = '''select payment_type,count(*) from <schemaname>.<nyctaxi_sample> group by payment_type'''
    pd.read_sql(query,conn)

#### <a name="verify-the-final-form-of-the-featurized-table"></a>Özellikleri tespit tablonun son biçimini doğrulayın
    query = '''SELECT TOP 100 * FROM <schemaname>.<nyctaxi_sample>'''
    pd.read_sql(query,conn)

## <a name="mlmodel"></a>Azure Machine Learning modelleri oluşturma
Model yapı ve model dağıtımı, devam etmeye hazır sunmaktayız [Azure Machine Learning](https://studio.azureml.net). Verileri daha önce yani tanımlanan tahmin sorunları hiçbirinde kullanılmaya hazır:

1. **İkili sınıflandırma**: Tahmin etmek için olup olmadığını bir ipucu için bir seyahat ödenmiş.
2. **Sınıflı sınıflandırma**: İpucu aralığını tahmin etmek için önceden tanımlanmış sınıfları göre Ücretli.
3. **Regresyon görev**: İpucu miktarı tahmin etmek için bir seyahat Ücretli.

Modelleme alıştırma başlamak için oturum açın, **Azure Machine Learning** çalışma. Bir machine learning çalışma alanını henüz oluşturmadıysanız bkz [bir Azure Machine Learning studio çalışma alanı oluşturma](../studio/create-workspace.md).

1. Azure Machine Learning ile çalışmaya başlamak için bkz: [Azure Machine Learning Studio nedir?](../studio/what-is-ml-studio.md)
2. Oturum [Azure Machine Learning Studio'yu](https://studio.azureml.net).
3. Studio giriş sayfası, modül başvurusu ve diğer kaynaklar için çok sayıda bilgi, videolar, öğreticiler, bağlantılar sağlar. Azure Machine Learning hakkında daha fazla bilgi için başvurun [Azure Machine Learning Belge Merkezi](https://azure.microsoft.com/documentation/services/machine-learning/).

Tipik eğitim denemesini aşağıdaki adımlardan oluşur:

1. Oluşturma bir **+ yeni** denemeler yapın.
2. Azure Machine Learning Studio'ya veri alın.
3. Ön işlem, dönüştürme ve gerektiği gibi verileri işlemek.
4. Özellikler, gerektiği gibi oluşturun.
5. Verileri eğitim/doğrulama/test kümelerine bölme (veya ayrı veri kümelerinde her biri için).
6. Bir veya daha fazla makine öğrenimi algoritmaları öğrenme problemi çözmek için bağlı olarak seçin. Örneğin, ikili Sınıflandırma, çok sınıflı Sınıflandırma, regresyon.
7. Eğitim veri kümesi kullanarak bir veya daha fazla modeller eğitin.
8. Eğitilen model kullanarak doğrulama dataset puan.
9. İlgili ölçümleri öğrenme problemi için işlem modellere değerlendirin.
10. İnce modellere ayarlayın ve dağıtmak için en iyi modeli seçin.

Bu alıştırmada biz zaten incelediniz ve verileri SQL veri ambarı'nda mühendislik ve Azure Machine Learning Studio'da almak için örnek boyutuna karar. Bir veya daha fazla öngörü modelleri oluşturmak için bir yordam şöyledir:

1. Azure Machine Learning studio kullanarak veri alma [verileri içeri aktarma] [ import-data] modülü, kullanılabilir **veri giriş ve çıkış** bölümü. Daha fazla bilgi için [verileri içeri aktarma] [ import-data] modül başvuru sayfası.

    ![Verileri Azure ML İçeri Aktar][17]
2. Seçin **Azure SQL veritabanı** olarak **veri kaynağı** içinde **özellikleri** paneli.
3. Veritabanı bir DNS adı girmeniz **veritabanı sunucu adı** alan. Biçim: `tcp:<your_virtual_machine_DNS_name>,1433`
4. Girin **veritabanı adı** karşılık gelen alandaki.
5. Girin *SQL kullanıcı adı* içinde **Server kullanıcı hesabı adı**ve *parola* içinde **Server kullanıcı hesabı parolası**.
7. İçinde **veritabanı sorgusu** metin alanını düzenleme, gerekli veritabanı alanları (etiketler gibi tüm hesaplanan alanları dahil) ayıklayan sorguyu yapıştırın ve aşağı istenen örnek boyutu için verileri örnekler.

Aşağıdaki çizimde, doğrudan SQL veri ambarı veritabanından veriler okunurken bir ikili sınıflandırma deneme örnektir (şema adı ve, bu kılavuzda kullanılan tablo adlarının ve tablo adları nyctaxi_trip nyctaxi_fare değiştirmeyi unutmayın). Benzer denemeleri, çok sınıflı sınıflandırma ve regresyon sorunları için oluşturulabilir.

![Azure ML Train][10]

> [!IMPORTANT]
> Modelleme verileri ayıklama ve sorgu örnekleri örnekleme önceki kısımlarında sağlanan **üç modelleme alıştırmalar için tüm etiketleri sorguda bulunan**. Her model alıştırmalarda önemli bir (gerekli) adım **hariç** diğer iki sorunları ve diğer gereksiz etiketlerini **hedef sızıntılarını**. Örneğin, ikili Sınıflandırma, etiket kullanırken **Eğimli** ve alanları Dışla **İpucu\_sınıfı**, **İpucu\_tutarı**ve **toplam\_tutarı**. Bunlar tip kapsıyor ikinci hedef sızıntılarını, çünkü Ücretli.
>
> Tüm gereksiz sütunları hariç tutma veya sızıntıları hedeflemek için kullanabilir [kümesindeki sütunları seçme] [ select-columns] modülü veya [meta verileri Düzenle][edit-metadata]. Daha fazla bilgi için [kümesindeki sütunları seçme] [ select-columns] ve [meta verileri Düzenle] [ edit-metadata] başvuru sayfalarına.
>
>

## <a name="mldeploy"></a>Azure Machine Learning modelleri dağıtma
Modeliniz hazır olduğunda, kolayca, denemeyi doğrudan bir web hizmeti olarak dağıtabilirsiniz. Azure ML web hizmetleri dağıtma hakkında daha fazla bilgi için bkz. [bir Azure Machine Learning web hizmetini dağıtma](../studio/publish-a-machine-learning-web-service.md).

Yeni bir web hizmeti dağıtmak için yapmanız:

1. Puanlama deneme oluşturun.
2. Web hizmeti dağıtın.

Puanlama bir deneyden oluşturmak için bir **tamamlandı** deneme eğitim, tıklayın **PUANLAMA DENEMELER oluşturma** alt eylem çubuğunda.

![Azure Puanlama][18]

Azure Machine Learning eğitim denemesini bileşenlerine göre bir Puanlama deneme oluşturmaya çalışır. Özellikle de yapar:

1. Eğitilen modeli kaydedin ve model eğitim modülleri kaldırın.
2. Bir mantıksal tanımlamak **giriş bağlantı noktasına** beklenen giriş verileri şemasını temsil etmek için.
3. Bir mantıksal tanımlamak **çıkış bağlantı noktasına** beklenen web hizmeti çıkış şeması temsil etmek için.

Puanlama denemeyi oluşturulduğunda gözden geçirin ve gerektiği gibi yapın. Tipik bir düzeltme hizmeti çağrıldığında bu kullanılabilir olmayacak şekilde giriş veri kümesi ve/veya sorgu Bu etiket alanları dışlar biri ile değiştirmektir. Ayrıca bazı kayıtları için giriş veri kümesi ve/veya sorgu boyutunu azaltmak için iyi bir uygulama olan giriş şemasını göstermek yeterli. Çıkış bağlantı noktasına için tüm giriş alanları Dışla ve yalnızca dahil etmek için ortak olan **Puanlanmış etiketler** ve **Puanlanmış olasılıklar** çıkış kullanarak [kümesindekisütunlarıseçme] [ select-columns] modülü.

Bir örnek denemeyi Puanlama, aşağıdaki şekilde sağlanır. Hazır olduğunuzda dağıtmak tıklayın **WEB hizmeti yayımlama** alt eylem çubuğunda düğme.

![Azure ML yayımlama][11]

## <a name="summary"></a>Özet
Ne Bu izlenecek yol öğreticide uyguladığımız özeti için büyük genel kümesiyle çalışan bir Azure veri bilimi ortamını Team Data Science Process model eğitiminin ve ardından için veri alım gelen tüm aracılığıyla alma oluşturdunuz bir Azure Machine Learning web hizmeti dağıtımı.

### <a name="license-information"></a>Lisans bilgileri
Bu örnek gözden geçirme ve kendi eşlik eden betikleri ve Ipython notebook(s) paylaşılır Microsoft tarafından MIT lisansı altında. LICENSE.txt dosyasına örnek kod dizini github'da daha fazla ayrıntı için lütfen denetleyin.

## <a name="references"></a>Başvurular
• [Andrés Monroy NYC taksi Gelişlerin indirme sayfasına](https://www.andresmh.com/nyctaxitrips/) • [FOILing NYC'ın taksi seyahat verilerini Chris Whong tarafından](https://chriswhong.com/open-data/foil_nyc_taxi/) • [NYC taksi ve Limousine komisyon araştırma ve istatistikleri](https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page)

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
