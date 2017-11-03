---
title: "Derleme ve Azure VM'de SQL Server içine tablolar verilerin hızlı paralel alma için en iyi duruma getirme | Microsoft Docs"
description: "SQL Bölüm Tabloları Kullanarak Paralel Toplu Veri Alma"
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: ff90fdb0-5bc7-49e8-aee7-678b54f901c8
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: bradsev
ms.openlocfilehash: 899f20b3642612386f2513c9c8649cd845be826e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="parallel-bulk-data-import-using-sql-partition-tables"></a>SQL Bölüm Tabloları Kullanarak Paralel Toplu Veri Alma
Bu belge, yapı verilerinin bir SQL Server veritabanına hızlı paralel Toplu içe aktarma için bölümlenmiş tabloları açıklar. Büyük veri yükleme/aktarım için bir SQL veritabanı için sonraki sorgular ve SQL DB için veri alma kullanılarak geliştirilebilir *bölümlenmiş tabloları ve görünümleri*. 

## <a name="create-a-new-database-and-a-set-of-filegroups"></a>Yeni bir veritabanı ve dosya grupları kümesi oluşturma
* [Yeni bir veritabanı oluşturmak](https://technet.microsoft.com/library/ms176061.aspx), zaten yoksa.
* Veritabanı dosya grupları bölümlenmiş fiziksel dosyaları tutacak veritabanına ekleyin. Bu, yapılabilir [CREATE DATABASE](https://technet.microsoft.com/library/ms176061.aspx) yeni veya [ALTER DATABASE](https://msdn.microsoft.com/library/bb522682.aspx) veritabanı zaten varsa.
* Bir veya daha fazla her veritabanı dosya (gerektiğinde) ekleyin.
  
  > [!NOTE]
  > Bu bölüm veri tutan hedef dosya grubunda ve dosya verilerini depolanacağı fiziksel veritabanı dosya adı belirtin.
  > 
  > 

Aşağıdaki örnekte, üç birincil dışındaki dosya grupları ve günlük grupları, her bir fiziksel dosya içeren yeni bir veritabanı oluşturur. Veritabanı dosyaları, SQL Server örneğinde yapılandırılan varsayılan SQL Server veri klasörü oluşturulur. Varsayılan dosya konumları hakkında daha fazla bilgi için bkz: [varsayılan ve SQL Server örnekleri adlı dosya konumlarını](https://msdn.microsoft.com/library/ms143547.aspx).

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
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name_2>.ndf'' , SIZE = 4096KB , FILEGROWTH = 1024KB ), 
         FILEGROUP [filegroup_3] 
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name>.ndf'' , SIZE = 102400KB , FILEGROWTH = 10240KB ), 
         LOG ON 
        ( NAME = ''LogFileGroup'', FILENAME = ''' + @data_path + '<log_file_name>.ldf'' , SIZE = 1024KB , FILEGROWTH = 10%)
    ')

## <a name="create-a-partitioned-table"></a>Bölümlenmiş bir tablo oluştur
Önceki adımda oluşturulan veritabanı dosya gruplarını eşlenmiş veri şeması göre bölümlenmiş tabloları oluşturun. Veriler toplu bölümlenmiş tabloları içeri olduğunda, kayıtları arasında bir bölüm düzeni göre dosya gruplarını aşağıda açıklandığı gibi dağıtılacaktır.

**Bir bölüm tablosu oluşturmak için aktarmanız gerekir:**

* [Bölümleme işlevi oluşturma](https://msdn.microsoft.com/library/ms187802.aspx) değerleri/sınırları her tek tek bölüm tablosunda örneğin dahil edilecek, aya göre bölümleri sınırlamak için aralığını tanımlar (bazı\_datetime\_alan) yıl 2013:
  
        CREATE PARTITION FUNCTION <DatetimeFieldPFN>(<datetime_field>)  
        AS RANGE RIGHT FOR VALUES (
            '20130201', '20130301', '20130401',
            '20130501', '20130601', '20130701', '20130801',
            '20130901', '20131001', '20131101', '20131201' )
* [Bir bölüm düzeni oluşturma](https://msdn.microsoft.com/library/ms179854.aspx) hangi eşlemeleri her bölüm aralığı bölüm işlevinde fiziksel bir dosya grubuna örn:
  
        CREATE PARTITION SCHEME <DatetimeFieldPScheme> AS  
        PARTITION <DatetimeFieldPFN> TO (
        <filegroup_1>, <filegroup_2>, <filegroup_3>, <filegroup_4>,
        <filegroup_5>, <filegroup_6>, <filegroup_7>, <filegroup_8>,
        <filegroup_9>, <filegroup_10>, <filegroup_11>, <filegroup_12> )
  
  Her bölüm işlevi/düzeni göre aralıklardaki yürürlükte doğrulamak için aşağıdaki sorguyu çalıştırın:
  
        SELECT psch.name as PartitionScheme,
            prng.value AS ParitionValue,
            prng.boundary_id AS BoundaryID
        FROM sys.partition_functions AS pfun
        INNER JOIN sys.partition_schemes psch ON pfun.function_id = psch.function_id
        INNER JOIN sys.partition_range_values prng ON prng.function_id=pfun.function_id
        WHERE pfun.name = <DatetimeFieldPFN>
* [Bölümlenmiş bir tablo oluşturmak](https://msdn.microsoft.com/library/ms174979.aspx)(s), veri şemasına göre ve tablo örneğin bölümlemek için kullanılan bölüm düzeni ve kısıtlama alanı belirtin:
  
        CREATE TABLE <table_name> ( [include schema definition here] )
        ON <TablePScheme>(<partition_field>)

Daha fazla bilgi için bkz: [bölümlenmiş tablolar oluşturmak ve dizinleri](https://msdn.microsoft.com/library/ms188730.aspx).

## <a name="bulk-import-the-data-for-each-individual-partition-table"></a>Toplu her tek tek bölüm tablosu için veri alma
* BCP, toplu ekleme ya da diğer yöntemleri gibi kullanabilir [SQL Server Yükseltme Sihirbazı](http://sqlazuremw.codeplex.com/). Sağlanan örnek BCP yöntemini kullanır.
* [Veritabanı alter](https://msdn.microsoft.com/library/bb522682.aspx) günlüğü, örneğin yükünü en aza indirmek için toplu_günlüklü için işlem günlüğü düzenini değiştirmek için:
  
        ALTER DATABASE <database_name> SET RECOVERY BULK_LOGGED
* Veri yükleme hızlandırmak için toplu içeri aktarma işlemleri paralel başlatın. SQL Server veritabanlarına büyük veri alma toplu hızlandırılması ipuçları için bkz [1 TB 1 saatten az yük](http://blogs.msdn.com/b/sqlcat/archive/2006/05/19/602142.aspx).

Aşağıdaki PowerShell betiğini paralel veri BCP kullanarak yükleme örneğidir.

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


## <a name="create-indexes-to-optimize-joins-and-query-performance"></a>Birleştirme ve sorgu performansı en iyi duruma getirmek için dizinleri oluşturma
* Birden çok tablodan veri modelleme için ayıklanır varsa, dizinleri birleştirme performansını artırmak için birleştirme anahtarları oluşturun.
* [Dizin oluşturma](https://technet.microsoft.com/library/ms188783.aspx) (kümelenmiş veya kümelenmemiş) her bölüm için aynı dosya grubu için örneğin hedefleme:
  
        CREATE CLUSTERED INDEX <table_idx> ON <table_name>( [include index columns here] )
        ON <TablePScheme>(<partition)field>)
  Veya,
  
        CREATE INDEX <table_idx> ON <table_name>( [include index columns here] )
        ON <TablePScheme>(<partition)field>)
  
  > [!NOTE]
  > Toplu veri almadan önce dizin oluşturmak tercih edebilirsiniz. Toplu içe aktarmadan önce dizin oluşturma, veri yükleme yavaşlatır.
  > 
  > 

## <a name="advanced-analytics-process-and-technology-in-action-example"></a>Gelişmiş analizler işlemi ve eylem örnekte teknolojisi
Sahip ortak bir veri kümesi Cortana Analytics işlemini kullanarak uçtan uca kılavuz örnek için bkz: [Cortana Analytics eylem işleminde: SQL Server'ı kullanarak](sql-walkthrough.md).

