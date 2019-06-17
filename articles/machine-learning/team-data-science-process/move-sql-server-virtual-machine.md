---
title: Bir SQL Server sanal makinesi - Team Data Science Process için veri taşıma
description: Verileri düz dosyalara veya bir şirket içi SQL Server'dan Azure sanal makinesinde SQL Server'a taşıyın.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/04/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 47a77def43a9577e5a3506899da47db2f684b495
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61429527"
---
# <a name="move-data-to-sql-server-on-an-azure-virtual-machine"></a>Bir Azure sanal makinesinde SQL Server’a veri taşıma

Bu makalede, verileri düz dosyaları (CSV veya TSV biçimleri) veya bir şirket içi SQL Server'dan SQL Server için Azure sanal makinesinde taşımak için seçenekler özetlenmektedir. Verileri buluta taşımak için bu görevleri Team Data Science Process bir parçasıdır.

Machine Learning için Azure SQL veritabanı'na veri taşımak için seçenekler özetlenmektedir bir konuya bakın [veri taşıma için bir Azure SQL veritabanı için Azure Machine Learning](move-sql-azure.md).

Aşağıdaki tabloda, verileri bir Azure sanal makinesinde SQL Server'a taşımak için seçenekler özetlenmektedir.

| <b>KAYNAK</b> | <b>HEDEF: Azure vm'lerde SQL Server</b> |
| --- | --- |
| <b>Düz dosya</b> |1. <a href="#insert-tables-bcp">Komut satırı toplu kopyalama (BCP) yardımcı programı </a><br> 2. <a href="#insert-tables-bulkquery">Toplu ekleme SQL sorgusu </a><br> 3. <a href="#sql-builtin-utilities">SQL Server'da yerleşik grafik yardımcı programları</a> |
| <b>Şirket içi SQL Server</b> |1. <a href="#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard">Bir Microsoft Azure VM Sihirbazı için bir SQL Server veritabanı dağıtma</a><br> 2. <a href="#export-flat-file">Düz dosyaya </a><br> 3. <a href="#sql-migration">SQL veritabanı Geçiş Sihirbazı </a> <br> 4. <a href="#sql-backup">Yedekleme geri veritabanı ve geri yükleme </a><br> |

Bu belge SQL komutlarını SQL Server Management Studio veya Visual Studio veritabanı Gezgini yürütülür varsaydığını unutmayın.

> [!TIP]
> Alternatif olarak, kullandığınız [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) oluşturmak ve verileri Azure'da bir SQL Server VM taşıyacaktır bir işlem hattı zamanlamak için. Daha fazla bilgi için [Azure Data Factory (kopyalama etkinliği) ile veri kopyalama](../../data-factory/copy-activity-overview.md).
>
>

## <a name="prereqs"></a>Önkoşullar
Bu öğreticide, sahip olduğunuz varsayılır:

* Bir **Azure aboneliği**. Aboneliğiniz yoksa [ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
* Bir **Azure depolama hesabı**. Bu öğreticide verilerin depolanması için bir Azure depolama hesabı kullanır. Azure depolama hesabınız yoksa [Depolama hesabı oluşturma](../../storage/common/storage-quickstart-create-account.md) makalesine bakın. Depolama hesabı oluşturduktan sonra depolamaya erişmek için kullanılan hesap anahtarını edinmeniz gerekir. Bkz: [depolama erişim anahtarlarınızı yönetme](../../storage/common/storage-account-manage.md#access-keys).
* Sağlanan **Azure VM'deki SQL Server'a**. Yönergeler için [Gelişmiş analiz için Ipython Notebook olarak bir Azure SQL Server sanal makinesini ayarlama](../data-science-virtual-machine/setup-sql-server-virtual-machine.md).
* Yüklenmiş ve yapılandırılmış **Azure PowerShell** yerel olarak. Yönergeler için [Azure PowerShell'i yükleme ve yapılandırma işlemini](/powershell/azure/overview).

## <a name="filesource_to_sqlonazurevm"></a> Verileri bir düz dosya kaynaktan bir Azure sanal makinesinde SQL Server'a taşıma
Verilerinizi (bir satır/sütun biçimde düzenlenmesi) düz bir dosya ise, bu SQL Server VM'si için Azure'da aşağıdaki yöntemleri taşınabilir:

1. [Komut satırı toplu kopyalama (BCP) yardımcı programı](#insert-tables-bcp)
2. [Toplu ekleme SQL sorgusu](#insert-tables-bulkquery)
3. [SQL Server'ın (içeri/dışarı aktarma, SSIS) grafik yerleşik yardımcı programları](#sql-builtin-utilities)

### <a name="insert-tables-bcp"></a>Komut satırı toplu kopyalama (BCP) yardımcı programı
BCP ile SQL Server yüklü bir komut satırı yardımcı programıdır ve verilerini taşımak için hızlı yollarından biridir. Bu, tüm üç SQL Server çeşitleri (şirket içi SQL Server, SQL Azure ve Azure üzerinde SQL Server VM) üzerinde çalışır.

> [!NOTE]
> **Verilerim için BCP olduğu?**  
> Gerekli olmamasına karşın, hedef SQL Server ile aynı makinede bulunan kaynak verileri içeren dosyalarını daha hızlı aktarımları için (ağ hızı vs yerel disk g/ç hızı) sağlar. Makine verilerini içeren düz dosyaları taşıyabilirsiniz çeşitli dosya kopyalama kullanarak SQL Server yüklü olduğu gibi araçlar [AZCopy](../../storage/common/storage-use-azcopy.md), [Azure Depolama Gezgini](https://storageexplorer.com/) veya Uzak Masaüstü aracılığıyla windows kopyala/yapıştır Protokolü (RDP).
>
>

1. Hedef SQL Server veritabanında veritabanı ve tabloları oluşturduğunuzdan emin olun. Bunu yapmak nasıl bir örnek aşağıdadır `Create Database` ve `Create Table` komutları:

```sql
CREATE DATABASE <database_name>

CREATE TABLE <tablename>
(
    <columnname1> <datatype> <constraint>,
    <columnname2> <datatype> <constraint>,
    <columnname3> <datatype> <constraint>
)
```

1. Bcp, yüklü olduğu makinenin komut satırından aşağıdaki komutu gönderdikten tarafından tablo şemasını açıklayan biçim dosyası oluşturur.

    `bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n`
1. Bcp komut aşağıdaki gibi kullanarak veritabanına veri ekleyin. SQL Server aynı makinede yüklü varsayılarak bu komut satırından çalışması gerekir:

    `bcp dbname..tablename in datafilename.tsv -f exportformatfilename.xml -S servername\sqlinstancename -U username -P password -b block_size_to_move_in_single_attempt -t \t -r \n`

> **BCP ekler en iyi duruma getirme** aşağıdaki makaleye başvurun ['İçin en iyi duruma getirme toplu olarak içeri aktarma yönergeleri'](https://technet.microsoft.com/library/ms177445%28v=sql.105%29.aspx) böyle ekler en iyi duruma getirme.
>
>

### <a name="insert-tables-bulkquery-parallel"></a>Eklemeleri paralelleştirmek için daha hızlı veri taşıma
Taşınan veriler büyükse, şeyler paralel bir PowerShell Betiği olarak aynı anda birden çok BCP komutları yürüterek düşebileceğimizi.

> [!NOTE]
> **Büyük veri alımı** büyük ve çok büyük veri kümeleri için veri yükleme iyileştirmek için birden çok dosya grupları kullanarak, mantıksal ve fiziksel veritabanı tabloyu bölümlemek ve tablo bölümleme. Oluşturma ve bölüm tablolara veri yükleme hakkında daha fazla bilgi için bkz. [paralel yük SQL bölüm tablolarından](parallel-load-sql-partitioned-tables.md).
>
>

Aşağıdaki örnek PowerShell Betiği bcp kullanarak paralel eklemeleri gösterir:

```powershell
$NO_OF_PARALLEL_JOBS=2

Set-ExecutionPolicy RemoteSigned #set execution policy for the script to execute
# Define what each job does
$ScriptBlock = {
    param($partitionnumber)

    #Explicitly using SQL username password
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
```

### <a name="insert-tables-bulkquery"></a>Toplu ekleme SQL sorgusu
[Toplu ekleme SQL sorgusu](https://msdn.microsoft.com/library/ms188365) tabanlı satır/sütun dosyaları veritabanından veri almak için kullanılabilir (desteklenen türler ele alınmaktadır[toplu dışarı veya içeri aktarma (SQL Server) için veri hazırlama](https://msdn.microsoft.com/library/ms188609)) konu.

Toplu ekleme için bazı örnek komutlar şunlardır aşağıda gösterildiği gibi şunlardır:  

1. Verilerinizi analiz edin ve SQL Server veritabanı herhangi bir özel alanın tarihler gibi aynı biçimde varsayar emin olmak için içeri aktarmadan önce tüm özel seçenekleri belirleyin. (Verilerinizi yıl ay gün biçimine tarihi içeriyorsa) tarih biçimini yıl ay-günlük ayarlamak nasıl bir örnek aşağıda verilmiştir:

```sql
SET DATEFORMAT ymd;
```
1. Toplu içeri aktarma deyimlerini kullanarak verileri içeri aktarın:

```sql
BULK INSERT <tablename>
FROM
'<datafilename>'
WITH
(
    FirstRow = 2,
    FIELDTERMINATOR = ',', --this should be column separator in your data
    ROWTERMINATOR = '\n'   --this should be the row separator in your data
)
```

### <a name="sql-builtin-utilities"></a>SQL Server'da yerleşik yardımcı programları
Azure'da bir düz dosyadan SQL Server VM veri aktarmak için SQL Server tümleştirmeler Services (SSIS) kullanabilirsiniz.
SSIS iki studio ortamlarında kullanılabilir. Ayrıntılar için bkz [Integration Services (SSIS) ve Studio ortamları](https://technet.microsoft.com/library/ms140028.aspx):

* SQL Server veri araçları hakkında daha fazla bilgi için bkz: [Microsoft SQL Server veri araçları](https://msdn.microsoft.com/data/tools.aspx)  
* İçeri/Dışarı Aktarma Sihirbazı hakkında daha fazla bilgi için bkz: [SQL Server içeri ve Dışarı Aktarma Sihirbazı](https://msdn.microsoft.com/library/ms141209.aspx)

## <a name="sqlonprem_to_sqlonazurevm"></a>Verileri şirket içi SQL Server'dan bir Azure sanal makinesinde SQL Server'a taşıma
Aşağıdaki geçiş stratejileri de kullanabilirsiniz:

1. [Bir Microsoft Azure VM Sihirbazı için bir SQL Server veritabanı dağıtma](#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard)
2. [Düz dosyaya](#export-flat-file)
3. [SQL veritabanı Geçiş Sihirbazı](#sql-migration)
4. [Yedekleme geri veritabanı ve geri yükleme](#sql-backup)

Biz bunların her biri aşağıda açıklanmıştır:

### <a name="deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard"></a>Bir Microsoft Azure VM Sihirbazı için bir SQL Server veritabanı dağıtma
**Bir Microsoft Azure VM Sihirbazı için bir SQL Server veritabanı dağıtma** verileri bir şirket içi SQL Server örneğinden bir Azure sanal makinesinde SQL Server'a taşımak için bir basit ve önerilen yoludur. Ayrıntılı adımlar yanı sıra başka alternatifler hakkında ayrıntılı bilgi, bkz. [bir veritabanını Azure VM'deki SQL Server'a geçirme](../../virtual-machines/windows/sql/virtual-machines-windows-migrate-sql.md).

### <a name="export-flat-file"></a>Düz dosyaya
Açıklandığı gibi bir şirket içi SQL Server'dan dışarı aktarma verileri toplu olarak için çeşitli yöntemler kullanılabilir [toplu olarak içeri ve dışarı veri (SQL Server)](https://msdn.microsoft.com/library/ms175937.aspx) konu. Bu belge, örnek olarak toplu kopyalama programı (BCP) kapsar. Veri düz bir dosyaya dışarı aktarıldıktan sonra başka bir SQL server kullanarak toplu içeri aktarılabilir.

1. Verileri şirket içi SQL Server'dan bcp yardımcı programını kullanarak aşağıdaki gibi bir dosyaya aktarın.

    `bcp dbname..tablename out datafile.tsv -S    servername\sqlinstancename -T -t \t -t \n -c`
2. Veritabanı ve tablo kullanarak Azure üzerinde SQL Server VM oluşturma `create database` ve `create table` tablo şemasını dışarı adım 1.
3. Dışarı/içeri aktarılan verilerin tablo şemasını tanımlamak için bir biçim dosyası oluşturun. Biçim dosyası ayrıntılarını açıklanmıştır [bir biçim dosyası (SQL Server) oluşturma](https://msdn.microsoft.com/library/ms191516.aspx).

    Biçim dosyası oluşturma SQL Server makine BCP çalışırken

        bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n

    Biçim dosyası oluşturma çalışırken BCP uzaktan karşı bir SQL Server

        bcp dbname..tablename format nul -c -x -f  exportformatfilename.xml  -U username@servername.database.windows.net -S tcp:servername -P password  --t \t -r \n
4. Bölümünde açıklanan yöntemlerden dilediğinizi [dosya kaynağı verilerini taşıma](#filesource_to_sqlonazurevm) verileri düz dosyaları bir SQL Server'a taşımak için.

### <a name="sql-migration"></a>SQL veritabanı Geçiş Sihirbazı
[SQL Server veritabanı Geçiş Sihirbazı](https://sqlazuremw.codeplex.com/) iki SQL server örnekleri arasında veri taşımak için kullanışlı bir yol sağlar. Bu sütun türlerini ve diğer çeşitli İşlevler'i seçin, kaynakları ve hedef tablolar arasında veri şemasını harita izin verir. Bu işlem arka planda altında toplu kopyalama (BCP) kullanır. SQL veritabanı Geçiş Sihirbazı Hoş Geldiniz ekranının ekran görüntüsü aşağıda gösterilmiştir.  

![SQL Server Yükseltme Sihirbazı][2]

### <a name="sql-backup"></a>Yedekleme geri veritabanı ve geri yükleme
SQL Server'ı destekler:

1. [Arka veritabanı yedeklemek ve geri yükleme işlevselliğinin](https://msdn.microsoft.com/library/ms187048.aspx) (her ikisi de bir yerel dosya veya bacpac vermek için blob) ve [veri katmanı uygulamaları](https://msdn.microsoft.com/library/ee210546.aspx) (bacpac kullanarak).
2. Doğrudan kopyalanmış veritabanı veya varolan bir SQL Azure veritabanına kopyalama Azure'da SQL Server Vm'leri oluşturma olanağı. Daha fazla ayrıntı için [veritabanı kopyalama Sihirbazı'nı](https://msdn.microsoft.com/library/ms188664.aspx).

Veritabanı geri görüntüsü yukarı/geri yükleme SQL Server Management Studio seçenekleri aşağıda gösterilmiştir.

![SQL Server içeri aktarma aracı][1]

## <a name="resources"></a>Kaynaklar
[Bir veritabanını Azure VM'deki SQL Server'a geçirme](../../virtual-machines/windows/sql/virtual-machines-windows-migrate-sql.md)

[Azure Sanal Makineler’de SQL Server’a genel bakış](../../virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md)

[1]: ./media/move-sql-server-virtual-machine/sqlserver_builtin_utilities.png
[2]: ./media/move-sql-server-virtual-machine/database_migration_wizard.png
