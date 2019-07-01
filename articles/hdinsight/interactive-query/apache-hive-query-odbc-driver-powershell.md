---
title: Sorgu Apache Hive ODBC sürücüsünü ve PowerShell - Azure HDInsight ile
description: Azure HDInsight kümelerinde Apache Hive sorgu için Microsoft Hive ODBC sürücüsünü ve PowerShell kullanın.
keywords: Hive, hive odbc powershell
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: tutorial
ms.date: 06/27/2019
ms.author: hrasheed
ms.openlocfilehash: b02c865e953861b5ac396538fdd0f0623b0e5428
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67486105"
---
# <a name="tutorial-query-apache-hive-with-odbc-and-powershell"></a>Öğretici: ODBC ve PowerShell ile Apache Hive sorgusu

Microsoft ODBC sürücüsü, Apache Hive gibi veri kaynakları, farklı tür ile etkileşim kurmak için esnek bir yol sağlar. ODBC sürücüleri kullanan Hive kümenize bir bağlantıyı açmak için seçtiğiniz bir query geçirmek komut dosyası dilleri içinde PowerShell gibi kod yazın ve sonuçları görüntüler.

Bu öğreticide, aşağıdaki görevleri gerçekleştirin:

> [!div class="checklist"]
> * İndirme ve Microsoft Hive ODBC sürücüsünü yükleme
> * Kümenize bağlı bir Apache Hive ODBC veri kaynağı oluşturma
> * PowerShell kullanarak kümenize sorgu örneği bilgileri

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce aşağıdaki öğelere sahip olmanız gerekir:

* HDInsight üzerinde etkileşimli sorgu kümesi. Oluşturmak için bkz: [Azure HDInsight ile çalışmaya başlama](../hdinsight-hadoop-provision-linux-clusters.md). Seçin **etkileşimli sorgu** küme türü olarak.

## <a name="install-microsoft-hive-odbc-driver"></a>Microsoft Hive ODBC sürücüsünü yükleme

İndirme ve yükleme [Microsoft Hive ODBC sürücüsünü](https://go.microsoft.com/fwlink/?LinkID=286698).

## <a name="create-apache-hive-odbc-data-source"></a>Apache Hive ODBC veri kaynağı oluşturma

Aşağıdaki adımlar Apache Hive ODBC veri kaynağını oluşturma işlemini gösterir.

1. Windows gidin **Başlat** > **Windows Yönetim Araçları** > **ODBC veri kaynakları (32-bit)/(64-bit)** .  Bir **ODBC Veri Kaynağı Yöneticisi** penceresi açılır.

    ![Veri Kaynağı Yöneticisi OBDC](./media/apache-hive-query-odbc-driver-powershell/hive-odbc-driver-dsn-setup.png "ODBC Veri Kaynağı Yöneticisi kullanan bir DSN'ye yapılandırın")

1. Gelen **Kullanıcı DSN** sekmesinde **Ekle** açmak için **yeni veri kaynağı oluştur** penceresi.

1. Seçin **Microsoft Hive ODBC sürücüsünü**ve ardından **son** açmak için **Microsoft Hive ODBC sürücüsü DSN Kurulum** penceresi.

1. Aşağıdaki değerleri yazın veya seçin:

   | Özellik | Açıklama |
   | --- | --- |
   |  Data Source Name |Veri kaynağınız için bir ad verin |
   |  Konaklarında |`CLUSTERNAME.azurehdinsight.net` yazın. Örneğin, `myHDICluster.azurehdinsight.net` |
   |  Port |**443** yazın.|
   |  Database |Kullanım **varsayılan**. |
   |  Mechanism |Seçin **Windows Azure HDInsight hizmeti** |
   |  Kullanıcı adı |HDInsight küme HTTP kullanıcısı kullanıcı adı girin. Varsayılan kullanıcı adı **admin** şeklindedir. |
   |  Parola |HDInsight küme kullanıcı parolasını girin. Onay kutusunu işaretleyin **Parolayı Kaydet (şifrelenmiş)** .|

1. İsteğe bağlı: Seçin **Gelişmiş Seçenekler**.  

   | Parametre | Açıklama |
   | --- | --- |
   |  Yerel sorgu kullanın |Seçildiğinde, TSQL HiveQL dönüştürmek ODBC sürücüsü denemez. Yalnızca %100 saf HiveQL ifadelerini gönderme emin değilseniz bu seçeneği kullanın. SQL Server veya Azure SQL veritabanına bağlanırken işaretli bırakmanız gerekir. |
   |  Blok başına getirilen satırlar |Çok sayıda kayıt getirme işlemi sırasında bu parametresi ayarlama işleminde en iyi performans sağlamak için gerekebilir. |
   |  Varsayılan sütun uzunluğu dize, ikili sütun uzunluğu, ondalık sütun ölçeği |Veri türü uzunlukları ve Precision bilgisayarlar, veriler nasıl döndürülür etkileyebilir. Bunlar, yanlış bilgi kaybı duyarlık ve kesilmesi nedeniyle döndürülecek neden. |

    ![Gelişmiş Seçenekleri](./media/apache-hive-query-odbc-driver-powershell/odbc-data-source-advanced-options.png "DSN Gelişmiş yapılandırma seçenekleri")

1. Seçin **Test** veri kaynağı test etmek için. Veri kaynağının doğru şekilde yapılandırıldığında, test sonuçlarını gösterir **başarı**.  

1. Seçin **Tamam** Test penceresini kapatın.  

1. Seçin **Tamam** kapatmak için **Microsoft Hive ODBC sürücüsü DSN Kurulum** penceresi.  

1. Seçin **Tamam** kapatmak için **ODBC Veri Kaynağı Yöneticisi** penceresi.  

## <a name="query-data-with-powershell"></a>PowerShell ile verileri Sorgulama

Aşağıdaki PowerShell betiğini bir Hive küme sorgulamak için bu ODBC bir işlevdir.

```powershell
function Get-ODBC-Data {

   param(
   [string]$query=$(throw 'query is required.'),
   [string]$dsn,  
   [PSCredential] $cred = (Get-Credential)  
   )

   $conn = New-Object System.Data.Odbc.OdbcConnection
   $uname = $cred.UserName

   $pswd = (New-Object System.Net.NetworkCredential -ArgumentList "", $cred.Password).Password
   $conn.ConnectionString = "DSN=$dsn;Uid=$uname;Pwd=$pswd;"
   $conn.open()
   $cmd = New-object System.Data.Odbc.OdbcCommand($query,$conn)

   $ds = New-Object system.Data.DataSet

   (New-Object system.Data.odbc.odbcDataAdapter($cmd)).fill($ds) #| out-null
   $conn.close()
   $ds.Tables
}
```

Aşağıdaki kod parçacığı, öğreticinin başında oluşturduğunuz etkileşimli sorgu kümesi üzerinde bir sorgu yürütmek için yukarıdaki işlevi kullanır. Değiştirin `DATASOURCENAME` ile **veri kaynağı adı** üzerinde belirtilen **Microsoft Hive ODBC sürücüsü DSN Kurulum** ekran. Kimlik bilgileri istendiğinde kullanıcı adını ve altında girdiğiniz parolayı girin **küme oturum açma kullanıcı** ve **küme oturum açma parolası** kümeyi oluşturduğunuzda.

```powershell

$dsn = "DATASOURCENAME"

$query = "select count(distinct clientid) AS total_clients from hivesampletable"

Get-ODBC-Data -query $query -dsn $dsn
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, HDInsight kümesi ve depolama hesabını silin. Bunu yapmak için kümenin oluşturulduğu kaynak grubunu seçin ve **Sil**.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Microsoft Hive ODBC sürücüsünü ve PowerShell, Azure HDInsight etkileşimli sorgu kümenizden verileri almak için nasıl kullanılacağını öğrendiniz.

> [!div class="nextstepaction"]
> [Excel için Apache Hive ODBC kullanarak bağlanma](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md)
