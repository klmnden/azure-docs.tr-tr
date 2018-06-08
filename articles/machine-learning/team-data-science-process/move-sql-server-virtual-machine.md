---
title: Bir Azure sanal makinede SQL Server'a veri taşıma | Microsoft Docs
description: Verileri düz dosyalardan veya bir şirket içi SQL Server'dan Azure VM'de SQL Server'a taşıyın.
services: machine-learning
documentationcenter: ''
author: deguhath
manager: jhubbard
editor: cgronlun
ms.assetid: 2c9ef1d3-4f5c-4b1f-bf06-223646c8af06
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/04/2017
ms.author: deguhath
ms.openlocfilehash: 229e2c07a3e8d83fc01dc5f1542fd250cb4678f7
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34838985"
---
# <a name="move-data-to-sql-server-on-an-azure-virtual-machine"></a>Bir Azure sanal makinesinde SQL Server’a veri taşıma
Bu konu, verileri düz dosyalar (CSV veya TSV biçimleri) veya bir şirket içi SQL Server'dan SQL Server'a bir Azure sanal makinesi üzerinde taşıma seçeneklerini özetler. Verileri buluta taşımak için bu görevleri takım veri bilimi işleminin bir parçasıdır.

Machine Learning için bir Azure SQL veritabanına veri taşıma seçeneklerini özetlenmektedir bir konuya bakın [veri taşıma bir Azure SQL veritabanına Azure Machine Learning için](move-sql-azure.md).

**Menü** burada veri depolanabilir ve takım veri bilimi işlem (TDSP) sırasında işlenen diğer hedef ortamlara veri alma nasıl açıklayan konulara bağlantılar aşağıda.

[!INCLUDE [cap-ingest-data-selector](../../../includes/cap-ingest-data-selector.md)]

Bir Azure sanal makinede SQL Server verilerini taşıma seçenekleri aşağıdaki tabloda özetlenmiştir.

| <b>KAYNAK</b> | <b>Hedef: SQL Server üzerinde Azure VM</b> |
| --- | --- |
| <b>Düz dosya</b> |1. <a href="#insert-tables-bcp">Komut satırı toplu kopyalama yardımcı programı (BCP) </a><br> 2. <a href="#insert-tables-bulkquery">Toplu ekleme SQL sorgusu </a><br> 3. <a href="#sql-builtin-utilities">SQL Server'da yerleşik grafik yardımcı programları</a> |
| <b>Şirket içi SQL Server</b> |1. <a href="#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard">Microsoft Azure VM Sihirbazı için bir SQL Server veritabanı dağıtma</a><br> 2. <a href="#export-flat-file">Düz bir dosyaya aktarın </a><br> 3. <a href="#sql-migration">SQL veritabanı Geçiş Sihirbazı </a> <br> 4. <a href="#sql-backup">Yukarı geri veritabanı ve geri yükleme </a><br> |

Bu belge SQL komutlarını SQL Server Management Studio veya Visual Studio veritabanı Gezgini'nden yürütülür varsaydığını unutmayın.

> [!TIP]
> Alternatif olarak, kullandığınız [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) oluşturmak ve verileri Azure üzerinde bir SQL Server VM taşınır ardışık zamanlamak için. Daha fazla bilgi için bkz: [Azure Data Factory (kopyalama etkinliği) ile veri kopyalama](../../data-factory/v1/data-factory-data-movement-activities.md).
>
>

## <a name="prereqs"></a>Önkoşullar
Bu öğretici, sahip olduğunuz varsayılmaktadır:

* Bir **Azure aboneliği**. Aboneliğiniz yoksa [ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
* Bir **Azure depolama hesabı**. Bu öğreticide verileri depolamak için bir Azure depolama hesabı kullanır. Azure depolama hesabınız yoksa [Depolama hesabı oluşturma](../../storage/common/storage-create-storage-account.md#create-a-storage-account) makalesine bakın. Depolama hesabı oluşturduktan sonra depolama erişmek için kullanılan hesap anahtarı edinmeniz gerekir. Bkz: [depolama erişim tuşlarınızı yönetme](../../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys).
* Sağlanan **Azure VM'de SQL Server**. Yönergeler için bkz: [IPython dizüstü sunucusu gelişmiş analizler için bir Azure SQL Server sanal makinesini yap](../data-science-virtual-machine/setup-sql-server-virtual-machine.md).
* Yüklenmiş ve yapılandırılmış **Azure PowerShell** yerel olarak. Yönergeler için bkz: [Azure PowerShell'i yükleme ve yapılandırma nasıl](/powershell/azure/overview).

## <a name="filesource_to_sqlonazurevm"></a> Azure VM'de SQL Server için bir düz dosya kaynaktan veri taşımayı
Verilerinizi (bir satır/sütun biçiminde düzenlenmiş) düz bir dosya ise, bu SQL Server VM Azure üzerinde aşağıdaki yöntemleri taşınabilir:

1. [Komut satırı toplu kopyalama yardımcı programı (BCP)](#insert-tables-bcp)
2. [Toplu ekleme SQL sorgusu ](#insert-tables-bulkquery)
3. [SQL Server (içeri/dışarı aktarma, SSIS) grafik yerleşik yardımcı programları](#sql-builtin-utilities)

### <a name="insert-tables-bcp"></a>Komut satırı toplu kopyalama yardımcı programı (BCP)
BCP komut satırı yardımcı programı ile SQL Server yüklü olduğundan ve verileri taşımak için hızlı yollarından biri. Bu, tüm üç SQL Server çeşitleri (şirket içi SQL Server, SQL Azure ve SQL Server VM Azure üzerinde) arasında çalışır.

> [!NOTE]
> **Burada verilerim için BCP olması gerekiyor mu?**  
> Gerekli olmamasına karşın, hedef SQL Server ile aynı makinede bulunan kaynak verilerini içeren dosyaları daha hızlı aktarımları (ağ hızını vs yerel disk g/ç hızı) sağlar. Makineye verileri içeren düz dosyalar taşıyabilirsiniz çeşitli dosya kopyalama kullanarak SQL Server yüklü olduğu gibi araçları [AZCopy](../../storage/common/storage-use-azcopy.md), [Azure Storage Gezgini](http://storageexplorer.com/) veya windows Uzak Masaüstü kopyalayıp yapıştırın Protokolü (RDP).
>
>

1. Veritabanı ve tablo hedef SQL Server veritabanında oluşturduğunuzdan emin olun. Bu kullanılarak nasıl bir örneği burada verilmiştir `Create Database` ve `Create Table` komutlar:

        CREATE DATABASE <database_name>

        CREATE TABLE <tablename>
        (
            <columnname1> <datatype> <constraint>,
            <columnname2> <datatype> <constraint>,
            <columnname3> <datatype> <constraint>
        )
2. Bcp yüklü olduğu makinenin komut satırından aşağıdaki komutu gönderdikten tarafından tablo için şema açıklayan biçim dosyası oluşturur.

    `bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n`
3. Bcp komut aşağıdaki gibi kullanarak veritabanına veri ekleyin. SQL Server aynı makinede yüklü varsayılarak bu komut satırı ile çalışması gerekir:

    `bcp dbname..tablename in datafilename.tsv -f exportformatfilename.xml -S servername\sqlinstancename -U username -P password -b block_size_to_move_in_single_attemp -t \t -r \n`

> **BCP ekler en iyi duruma getirme** Lütfen aşağıdaki makaleye bakın ['En iyi duruma getirme toplu içeri aktarma yönergeleri'](https://technet.microsoft.com/library/ms177445%28v=sql.105%29.aspx) böyle ekler en iyi duruma getirme.
>
>

### <a name="insert-tables-bulkquery-parallel"></a>Ekler için daha hızlı veri taşıma parallelizing
Taşıdığınız veri büyükse, şeyler paralel bir PowerShell Betiği olarak aynı anda birden çok BCP komutları yürüterek hızlandırma.

> [!NOTE]
> **Büyük veri alım** veri yükleme büyük ve çok büyük veri kümeleri için en iyi duruma getirmek için birden çok dosya grupları ve bölüm tabloları kullanarak, mantıksal ve fiziksel veritabanı tablolarını bölüm. Oluşturma ve bölüm tablolara veri yükleme hakkında daha fazla bilgi için bkz: [paralel yük SQL bölüm tabloları](parallel-load-sql-partitioned-tables.md).
>
>

Aşağıdaki örnek PowerShell komut dosyası bcp kullanarak paralel eklemeleri gösterilmektedir:

    $NO_OF_PARALLEL_JOBS=2

     Set-ExecutionPolicy RemoteSigned #set execution policy for the script to execute
     # Define what each job does
       $ScriptBlock = {
           param($partitionnumber)

           #Explictly using SQL username password
           bcp database..tablename in datafile_path.csv -F 2 -f format_file_path.xml -U username@servername -S tcp:servername -P password -b block_size_to_move_in_single_attempt -t "," -r \n -o path_to_outputfile.$partitionnumber.txt

            #Trusted connection w.o username password (if you are using windows auth and are signed in with that credentials)
            #bcp database..tablename in datafile_path.csv -o path_to_outputfile.$partitionnumber.txt -h "TABLOCK" -F 2 -f format_file_path.xml  -T -b block_size_to_move_in_single_attempt -t "," -r \n
      }


    # Background processing of all partitions
    for ($i=1; $i -le $NO_OF_PARALLEL_JOBS; $i++)
    {
      Write-Debug "Submit loading partition # $i"
      Start-Job $ScriptBlock -Arg $i      
    }


    # Wait for it all to complete
    While (Get-Job -State "Running")
    {
      Start-Sleep 10
      Get-Job
    }

    # Getting the information back from the jobs
    Get-Job | Receive-Job
    Set-ExecutionPolicy Restricted #reset the execution policy


### <a name="insert-tables-bulkquery"></a>Toplu ekleme SQL sorgusu
[Toplu ekleme SQL sorgusu](https://msdn.microsoft.com/library/ms188365) satır/sütun göre dosyaları veritabanından veri almak için kullanılan (desteklenen türlerden ele alınmaktadır[toplu dışarı veya içeri aktarma (SQL Server) için verileri hazırlama](https://msdn.microsoft.com/library/ms188609)) konu.

Bulk INSERT için bazı örnek komutları şunlardır aşağıdaki şekilde şunlardır:  

1. Verilerinizi çözümlemek ve SQL Server veritabanı tarihleri gibi herhangi bir özel alan için aynı biçimde varsayar emin olmak için almadan önce tüm özel seçenekleri ayarlayın. (Verileriniz yıl ay gün biçimine tarih içeriyorsa) tarih biçimi yıl ay-günlük ayarlamak nasıl bir örneği aşağıda verilmiştir:

        SET DATEFORMAT ymd;    
2. Toplu içeri aktarma deyimlerini kullanarak veri içeri aktarın:

        BULK INSERT <tablename>
        FROM    
        '<datafilename>'
        WITH
        (
        FirstRow=2,
        FIELDTERMINATOR =',', --this should be column separator in your data
        ROWTERMINATOR ='\n'   --this should be the row separator in your data
        )

### <a name="sql-builtin-utilities"></a>SQL Server'da yerleşik yardımcı programları
SQL Server VM azure'da bir düz dosyadan veri almak için SQL Server tümleştirmeler Services (SSIS) kullanabilirsiniz.
SSIS iki studio ortamlarında kullanılabilir. Ayrıntılar için bkz [Integration Services (SSIS) ve Studio ortamları](https://technet.microsoft.com/library/ms140028.aspx):

* SQL Server veri araçları hakkında daha fazla bilgi için bkz: [Microsoft SQL Server veri araçları](https://msdn.microsoft.com/data/tools.aspx)  
* İçeri/Dışarı Aktarma Sihirbazı hakkında daha fazla bilgi için bkz: [SQL Server içeri ve Dışarı Aktarma Sihirbazı](https://msdn.microsoft.com/library/ms141209.aspx)

## <a name="sqlonprem_to_sqlonazurevm"></a>Şirket içi SQL Server'dan veri taşımayı Azure VM'de SQL Server
Aşağıdaki geçiş stratejilerini de kullanabilirsiniz:

1. [Microsoft Azure VM Sihirbazı için bir SQL Server veritabanı dağıtma](#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard)
2. [Düz dosya dışarı aktarma](#export-flat-file)
3. [SQL veritabanı Geçiş Sihirbazı](#sql-migration)
4. [Yukarı geri veritabanı ve geri yükleme](#sql-backup)

Biz bunların her biri aşağıda açıklanmıştır:

### <a name="deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard"></a>Microsoft Azure VM Sihirbazı için bir SQL Server veritabanı dağıtma
**Microsoft Azure VM Sihirbazı için bir SQL Server veritabanı dağıtmak** verileri bir şirket içi SQL Server örneğini SQL Server için bir Azure VM taşımak için bir basit ve önerilen yoldur. Ayrıntılı adımlar yanı sıra tartışma diğer alternatifleri için bkz: [Azure VM'de SQL Server veritabanı geçirilecek](../../virtual-machines/windows/sql/virtual-machines-windows-migrate-sql.md).

### <a name="export-flat-file"></a>Düz dosya dışarı aktarma
Bir şirket içi SQL Server verileri dışa açıklandığı gibi toplu için çeşitli yöntemler kullanılabilir [toplu içeri ve dışarı aktarma veri (SQL Server)](https://msdn.microsoft.com/library/ms175937.aspx) konu. Bu belge toplu kopyalama programı (BCP) örnek olarak ele alınacaktır. Veri düz bir dosyaya dışarı aktarıldıktan sonra başka bir SQL server kullanarak toplu içeri aktarılabilir.

1. Verileri şirket içi SQL Server'dan bcp yardımcı programını kullanarak bir dosyaya aktarın.

    `bcp dbname..tablename out datafile.tsv -S    servername\sqlinstancename -T -t \t -t \n -c`
2. Azure kullanarak SQL Server VM üzerinde veritabanı ve tablo oluşturma `create database` ve `create table` tablo şemasını dışarı adım 1.
3. Verilerin dışa ve içe olmasıyla tablo şemasını tanımlayan bir biçim dosyası oluşturun. Biçim dosyası ayrıntılarını açıklanmıştır [(SQL Server) bir biçim dosyası oluşturma](https://msdn.microsoft.com/library/ms191516.aspx).

    Biçim dosyası oluşturma SQL Server makine BCP çalışırken

        bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n

    Biçim dosyası oluşturma çalışırken BCP uzaktan karşı bir SQL Server

        bcp dbname..tablename format nul -c -x -f  exportformatfilename.xml  -U username@servername.database.windows.net -S tcp:servername -P password  --t \t -r \n
4. Bölümünde açıklanan yöntemlerden birini kullanın [dosya kaynağı verilerini taşıma](#filesource_to_sqlonazurevm) verileri bir SQL Server'a düz dosyalarda taşımak için.

### <a name="sql-migration"></a>SQL veritabanı Geçiş Sihirbazı
[SQL Server veritabanı Geçiş Sihirbazı](http://sqlazuremw.codeplex.com/) iki SQL server örnekleri arasında verileri taşımak için kullanımı kolay bir yol sağlar. Kaynak ve hedef tablolar arasında veri şeması eşlemek için sütun türleri ve diğer çeşitli işlevlerin seçin kullanıcının sağlar. Kapak altında toplu kopyalama (BCP) kullanır. SQL veritabanı Geçiş Sihirbazı Hoş Geldiniz ekranının ekran görüntüsü aşağıda gösterilmiştir.  

![SQL Server Yükseltme Sihirbazı][2]

### <a name="sql-backup"></a>Yukarı geri veritabanı ve geri yükleme
SQL Server destekler:

1. [Veritabanı geri yukarı ve işlevlerin geri yüklenmesi](https://msdn.microsoft.com/library/ms187048.aspx) (bir yerel dosya veya bacpac her ikisini birden verme blob'a) ve [veri katmanı uygulamaları](https://msdn.microsoft.com/library/ee210546.aspx) (bacpac kullanarak).
2. Doğrudan kopyalanan veritabanı veya var olan bir SQL Azure veritabanı kopyasına sahip Azure üzerinde SQL Server Vm'lerinin oluşturma yeteneği. Daha fazla ayrıntı için bkz: [kopya Veritabanı Sihirbazı'nı](https://msdn.microsoft.com/library/ms188664.aspx).

Bir ekran görüntüsünü veritabanı geri yukarı/geri yükleme seçenekleri SQL Server Management Studio'dan aşağıda gösterilmiştir.

![SQL Server içeri aktarma aracı][1]

## <a name="resources"></a>Kaynaklar
[Azure VM'de SQL Server veritabanı geçir](../../virtual-machines/windows/sql/virtual-machines-windows-migrate-sql.md)

[Azure Sanal Makineler’de SQL Server’a genel bakış](../../virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md)

[1]: ./media/move-sql-server-virtual-machine/sqlserver_builtin_utilities.png
[2]: ./media/move-sql-server-virtual-machine/database_migration_wizard.png
