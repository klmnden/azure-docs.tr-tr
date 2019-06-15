---
title: SQL bölüm tablolarından - Team Data Science Process içinde paralel toplu veri alma
description: Hızlı paralel toplu veri bir SQL Server veritabanına içeri aktarmak için bölümlenmiş tabloları oluşturun.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/09/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 253f73cc58292778d88417b693c157fcbd7d92bd
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61428315"
---
# <a name="build-and-optimize-tables-for-fast-parallel-import-of-data-into-a-sql-server-on-an-azure-vm"></a>Derleme ve bir Azure VM'deki SQL Server'a hızlı paralel içeri aktarılacak veri tabloları iyileştirin

Bu makalede, derleme hızlı paralel toplu veri bir SQL Server veritabanına içeri aktarmak için bölümlenmiş tabloları açıklar. Büyük veri yükleme/aktarım için bir SQL veritabanı, SQL DB ve sonraki sorgular için veri alma kullanılarak geliştirilebilir *bölümlenmiş tabloları ve görünümleri*. 

## <a name="create-a-new-database-and-a-set-of-filegroups"></a>Yeni bir veritabanı ve dosya grupları kümesi oluşturma
* [Yeni veritabanı oluştur](https://technet.microsoft.com/library/ms176061.aspx), zaten yoksa.
* Veritabanı dosya grupları bölümlenmiş fiziksel dosyalarını tutan veritabanına eklenir. 
* Bu ile yapılabilir [CREATE DATABASE](https://technet.microsoft.com/library/ms176061.aspx) yeni veya [ALTER DATABASE](https://msdn.microsoft.com/library/bb522682.aspx) veritabanı zaten varsa.
* Bir veya daha fazla dosya (gereken şekilde) için her veritabanı dosya ekleyin.
  
  > [!NOTE]
  > Bu bölüm için veri içeren hedef dosya grubunda ve dosya verilerinin depolandığı fiziksel veritabanı dosya adı belirtin.
  > 
  > 

Aşağıdaki örnek, üç birincil dışındaki dosya grupları ve günlük grupları, her bir fiziksel dosya içeren yeni bir veritabanı oluşturur. Veritabanı dosyaları, yapılandırılan SQL Server örneğinde varsayılan SQL Server veri klasöründe oluşturulur. Varsayılan dosya konumları hakkında daha fazla bilgi için bkz. [varsayılan ve adlandırılmış örnekleri, SQL Server için dosya konumlarını](https://msdn.microsoft.com/library/ms143547.aspx).

    DECLARE @data_path nvarchar(256);
    SET @data_path = (SELECT SUBSTRING(physical_name, 1, CHARINDEX(N'master.mdf', LOWER(physical_name)) - 1)
      FROM master.sys.master_files
      WHERE database_id = 1 AND file_id = 1);

    EXECUTE ('
        CREATE DATABASE <database_name>
         ON  PRIMARY 
        ( NAME = ''Primary'', FILENAME = ''' + @data_path + '<primary_file_name>.mdf'', SIZE = 4096KB , FILEGROWTH = 1024KB ), 
         FILEGROUP [filegroup_1] 
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name_1>.ndf'' , SIZE = 4096KB , FILEGROWTH = 1024KB ), 
         FILEGROUP [filegroup_2] 
        ( NAME = ''FileGroup2'', FILENAME = ''' + @data_path + '<file_name_2>.ndf'' , SIZE = 4096KB , FILEGROWTH = 1024KB ), 
         FILEGROUP [filegroup_3] 
        ( NAME = ''FileGroup3'', FILENAME = ''' + @data_path + '<file_name_3>.ndf'' , SIZE = 102400KB , FILEGROWTH = 10240KB ) 
         LOG ON 
        ( NAME = ''LogFileGroup'', FILENAME = ''' + @data_path + '<log_file_name>.ldf'' , SIZE = 1024KB , FILEGROWTH = 10%)
    ')

## <a name="create-a-partitioned-table"></a>Bölümlenmiş bir tablo oluşturma
Önceki adımda oluşturduğunuz veritabanı dosya grupları için eşlenmiş veri şemasını göre bölümlenmiş tabloları oluşturmak için öncelikle bir bölüm işlevini ve düzeni oluşturmanız gerekir. Veriler için bölümlenmiş tabloları içeri toplu olduğunda, kayıtları arasında bir bölüm düzeni göre dosya gruplarını aşağıda açıklandığı şekilde dağıtılır.

### <a name="1-create-a-partition-function"></a>1. Bölümleme işlevi oluşturma
[Bölümleme işlevi oluşturma](https://msdn.microsoft.com/library/ms187802.aspx) aralığı değerleri/sınırları her tek bölüm tablosunda örneğin dahil edilecek, aya göre bölümlere sınırlamak için bu işlevi tanımlar (bazı\_datetime\_alan) 2013 yılında:
  
        CREATE PARTITION FUNCTION <DatetimeFieldPFN>(<datetime_field>)  
        AS RANGE RIGHT FOR VALUES (
            '20130201', '20130301', '20130401',
            '20130501', '20130601', '20130701', '20130801',
            '20130901', '20131001', '20131101', '20131201' )

### <a name="2-create-a-partition-scheme"></a>2. Bir bölüm düzeni oluşturma
[Bir bölüm düzeni oluşturma](https://msdn.microsoft.com/library/ms179854.aspx). Bu düzen, her bölüm aralığı bölüm işlevindeki örneğin fiziksel bir dosya grubu eşler:
  
        CREATE PARTITION SCHEME <DatetimeFieldPScheme> AS  
        PARTITION <DatetimeFieldPFN> TO (
        <filegroup_1>, <filegroup_2>, <filegroup_3>, <filegroup_4>,
        <filegroup_5>, <filegroup_6>, <filegroup_7>, <filegroup_8>,
        <filegroup_9>, <filegroup_10>, <filegroup_11>, <filegroup_12> )
  
  İşlevi/düzeni göre her bir bölümdeki geçerli aralıklar doğrulamak için aşağıdaki sorguyu çalıştırın:
  
        SELECT psch.name as PartitionScheme,
            prng.value AS PartitionValue,
            prng.boundary_id AS BoundaryID
        FROM sys.partition_functions AS pfun
        INNER JOIN sys.partition_schemes psch ON pfun.function_id = psch.function_id
        INNER JOIN sys.partition_range_values prng ON prng.function_id=pfun.function_id
        WHERE pfun.name = <DatetimeFieldPFN>

### <a name="3-create-a-partition-table"></a>3. Bölüm tablosu oluşturma
[Bölümlenmiş bir tablo oluşturma](https://msdn.microsoft.com/library/ms174979.aspx)(s) için veri şemasına göre ve örneğin, tabloyu bölümlemek için kullanılan bölüm düzeni ve kısıtlama alanı belirtin:
  
        CREATE TABLE <table_name> ( [include schema definition here] )
        ON <TablePScheme>(<partition_field>)

Daha fazla bilgi için [bölümlenmiş tablolar oluşturun ve dizinler](https://msdn.microsoft.com/library/ms188730.aspx).

## <a name="bulk-import-the-data-for-each-individual-partition-table"></a>Toplu her tek bölüm tablosu için veri alma

* BCP, toplu ekleme veya diğer yöntemler gibi kullanabilir [SQL Server Yükseltme Sihirbazı](https://sqlazuremw.codeplex.com/). Sağlanan örnek BCP yöntemini kullanır.
* [Veritabanı alter](https://msdn.microsoft.com/library/bb522682.aspx) günlüğü, örneğin yükünü en aza indirmek için toplu_günlüklü için işlem günlüğü düzenini değiştirmek için:
  
        ALTER DATABASE <database_name> SET RECOVERY BULK_LOGGED
* Veri yükleme hızlandırmak için paralel toplu içeri aktarma işlemlerinde başlatın. SQL Server veritabanlarına büyük veri alma toplu hızlandırılması ipuçları için bkz [1 saatten kısa bir süre içinde 1 TB yükleme](https://blogs.msdn.com/b/sqlcat/archive/2006/05/19/602142.aspx).

Aşağıdaki PowerShell betiğini bir paralel veri BCP kullanarak yükleme örneğidir.

    # Set database name, input data directory, and output log directory
    # This example loads comma-separated input data files
    # The example assumes the partitioned data files are named as <base_file_name>_<partition_number>.csv
    # Assumes the input data files include a header line. Loading starts at line number 2.

    $dbname = "<database_name>"
    $indir  = "<path_to_data_files>"
    $logdir = "<path_to_log_directory>"

    # Select authentication mode
    $sqlauth = 0

    # For SQL authentication, set the server and user credentials
    $sqlusr = "<user@server>"
    $server = "<tcp:serverdns>"
    $pass   = "<password>"

    # Set number of partitions per table - Should match the number of input data files per table
    $numofparts = <number_of_partitions>

    # Set table name to be loaded, basename of input data files, input format file, and number of partitions
    $tbname = "<table_name>"
    $basename = "<base_input_data_filename_no_extension>"
    $fmtfile = "<full_path_to_format_file>"

    # Create log directory if it does not exist
    New-Item -ErrorAction Ignore -ItemType directory -Path $logdir

    # BCP example using Windows authentication
    $ScriptBlock1 = {
       param($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $num)
       bcp ($dbname + ".." + $tbname) in ($indir + "\" + $basename + "_" + $num + ".csv") -o ($logdir + "\" + $tbname + "_" + $num + ".txt") -h "TABLOCK" -F 2 -C "RAW" -f ($fmtfile) -T -b 2500 -t "," -r \n
    }

    # BCP example using SQL authentication
    $ScriptBlock2 = {
       param($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $num, $sqlusr, $server, $pass)
       bcp ($dbname + ".." + $tbname) in ($indir + "\" + $basename + "_" + $num + ".csv") -o ($logdir + "\" + $tbname + "_" + $num + ".txt") -h "TABLOCK" -F 2 -C "RAW" -f ($fmtfile) -U $sqlusr -S $server -P $pass -b 2500 -t "," -r \n
    }

    # Background processing of all partitions
    for ($i=1; $i -le $numofparts; $i++)
    {
       Write-Output "Submit loading trip and fare partitions # $i"
       if ($sqlauth -eq 0) {
          # Use Windows authentication
          Start-Job -ScriptBlock $ScriptBlock1 -Arg ($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $i)
       } 
       else {
          # Use SQL authentication
          Start-Job -ScriptBlock $ScriptBlock2 -Arg ($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $i, $sqlusr, $server, $pass)
       }
    }

    Get-Job

    # Optional - Wait till all jobs complete and report date and time
    date
    While (Get-Job -State "Running") { Start-Sleep 10 }
    date


## <a name="create-indexes-to-optimize-joins-and-query-performance"></a>Birleştirme ve sorgu performansını iyileştirmek için dizinleri oluşturma
* Birden çok tablodan veri modelleme için ayıklama, birleştirme performansını artırmak için join anahtarlar üzerinde dizin oluşturun.
* [Dizinler oluşturma](https://technet.microsoft.com/library/ms188783.aspx) (kümelenmiş veya kümelenmemiş) örneğin her bölüm için aynı dosya grubu hedefleme:
  
        CREATE CLUSTERED INDEX <table_idx> ON <table_name>( [include index columns here] )
        ON <TablePScheme>(<partition)field>)
  veya,
  
        CREATE INDEX <table_idx> ON <table_name>( [include index columns here] )
        ON <TablePScheme>(<partition)field>)
  
  > [!NOTE]
  > Toplu veri almadan önce bir dizin oluşturmak tercih edebilirsiniz. Toplu içe aktarmadan önce dizin oluşturmayı, veri yükleme yavaşlatır.
  > 
  > 

## <a name="advanced-analytics-process-and-technology-in-action-example"></a>Gelişmiş analitik işlemi ve teknolojisi eylem örnekte
Team Data Science Process genel bir veri kümesi ile kullanarak bir uçtan uca izlenecek örnek için bkz [Team Data Science Process'in çalışması: SQL Server'ı kullanarak](sql-walkthrough.md).

