---
title: Apache Hive ile HDInsight - Azure PowerShell kullanma
description: HDInsight üzerinde Apache Hadoop Hive sorguları çalıştırmak için PowerShell kullanın.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/23/2018
ms.author: hrasheed
ms.openlocfilehash: 108a3e7d899eef4ca78ae7507bf4852b861e74d5
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64722177"
---
# <a name="run-apache-hive-queries-using-powershell"></a>PowerShell kullanarak Apache Hive sorguları çalıştırma
[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

Bu belgede, HDInsight kümesinde bir Apache Hadoop Hive sorguları çalıştırmak için Azure kaynak grubu moduna Azure PowerShell kullanarak bir örnek sağlar.

> [!NOTE]  
> Bu belgede ayrıntılı açıklamasını örneklerde kullanılan HiveQL ifadelerini ne sağlamaz. Bu örnekte kullanılan HiveQL hakkında daha fazla bilgi için bkz: [HDInsight üzerinde Apache Hadoop ile Hive kullanma Apache](hdinsight-use-hive.md).

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

* Bir Linux tabanlı Apache Hadoop üzerine HDInsight kümesi sürüm 3.4.

  > [!IMPORTANT]  
  > Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Azure PowerShell ile bir istemci.

[!INCLUDE [upgrade-powershell](../../../includes/hdinsight-use-latest-powershell.md)]

## <a name="run-a-hive-query"></a>Hive sorgusu çalıştırma

Azure PowerShell sağlar *cmdlet'leri* uzaktan üzerinde HDInsight Hive sorguları çalıştırmanıza izin verir. Dahili olarak, REST çağrıları için cmdlet'leri olun [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) HDInsight kümesi üzerinde.

Bir uzak HDInsight kümesinde Hive sorgularının çalıştırılması aşağıdaki cmdlet'ler kullanılır:

* `Connect-AzAccount`: Azure PowerShell, Azure aboneliğinize kimliğini doğrular.
* `New-AzHDInsightHiveJobDefinition`: Oluşturur bir *iş tanımı* belirtilen HiveQL ifadelerini kullanarak.
* `Start-AzHDInsightJob`: HDInsight için iş tanımını gönderir ve bir iş başlatılır. A *iş* nesne döndürülür.
* `Wait-AzHDInsightJob`: İş nesnesi, iş durumunu denetlemek için kullanır. Bekleme süresi aşılırsa veya iş tamamlanana kadar bekler.
* `Get-AzHDInsightJobOutput`: İşin çıktısını almak için kullanılır.
* `Invoke-AzHDInsightHiveJob`: HiveQL ifadelerini çalıştırmak için kullanılır. Bu cmdlet blokları sorgu tamamlandıktan sonra sonuçları döndürür.
* `Use-AzHDInsightCluster`: Geçerli Küme için kullanılacak ayarlar `Invoke-AzHDInsightHiveJob` komutu.

Aşağıdaki adımlarda, HDInsight kümenizin bir işi çalıştırmak için bu cmdlet'leri kullanma gösterilmektedir:

1. Bir Düzenleyicisi'ni kullanarak aşağıdaki kodun da Kaydet `hivejob.ps1`.

    [!code-powershell[main](../../../powershell_scripts/hdinsight/use-hive/use-hive.ps1?range=5-42)]

2. Yeni bir **Azure PowerShell** komut istemi. Dizinleri konumuna `hivejob.ps1` dosya ve betiği çalıştırmak için aşağıdaki komutu kullanın:

        .\hivejob.ps1

    Komut dosyası çalıştığında, küme adınızı ve HTTPS/küme yönetici hesabı kimlik bilgilerini girmeniz istenir. Azure aboneliğinizde oturum açmak için ayrıca istenebilir.

3. İş tamamlandığında, bilgileri aşağıdaki metne benzer döndürür:

        Display the standard output...
        2012-02-03      18:35:34        SampleClass0    [ERROR] incorrect       id
        2012-02-03      18:55:54        SampleClass1    [ERROR] incorrect       id
        2012-02-03      19:25:27        SampleClass4    [ERROR] incorrect       id

4. Daha önce belirtildiği `Invoke-Hive` bir sorgu çalıştırmak ve yanıt için beklemek için kullanılabilir. Invoke-Hive nasıl çalıştığını görmek için aşağıdaki betiği kullanın:

    [!code-powershell[main](../../../powershell_scripts/hdinsight/use-hive/use-hive.ps1?range=50-71)]

    Çıkışı şu metin gibi görünür:

        2012-02-03    18:35:34    SampleClass0    [ERROR]    incorrect    id
        2012-02-03    18:55:54    SampleClass1    [ERROR]    incorrect    id
        2012-02-03    19:25:27    SampleClass4    [ERROR]    incorrect    id

   > [!NOTE]  
   > Uzun HiveQL sorgular için Azure PowerShell kullanabilirsiniz **burada dizeler** cmdlet veya HiveQL komut dosyaları. Aşağıdaki kod parçacığını nasıl kullanılacağını gösterir `Invoke-Hive` cmdlet'ini HiveQL komut dosyasını çalıştırın. Wasb için HiveQL komut dosyası karşıya yüklenmelidir: / /.
   >
   > `Invoke-AzHDInsightHiveJob -File "wasb://<ContainerName>@<StorageAccountName>/<Path>/query.hql"`
   >
   > Hakkında daha fazla bilgi için **burada dizeler**, bkz: <a href="https://technet.microsoft.com/library/ee692792.aspx" target="_blank">kullanarak Windows PowerShell burada-dizeleri</a>.

## <a name="troubleshooting"></a>Sorun giderme

İş tamamlandığında hiçbir bilgi döndürülürse, hata günlüklerini görüntüleyin. Bu işi hata bilgilerini görüntülemek için aşağıdaki sonuna ekleyin `hivejob.ps1` dosyasını kaydedin ve yeniden çalıştırın.

```powershell
# Print the output of the Hive job.
Get-AzHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

Bu cmdlet iş işleme sırasında yazılan STDERR bilgileri döndürür.

## <a name="summary"></a>Özet

Gördüğünüz gibi Azure PowerShell, bir HDInsight kümesinde Hive sorguları çalıştırma, iş durumunu izlemek ve çıktısını almak için kolay bir yol sağlar.

## <a name="next-steps"></a>Sonraki adımlar

HDInsight Hive hakkında genel bilgi için:

* [HDInsight üzerinde Apache Hadoop ile Apache Hive'ı kullanma](hdinsight-use-hive.md)

Diğer yollar hakkında daha fazla bilgi için HDInsight üzerinde Hadoop ile çalışabilirsiniz:

* [HDInsight üzerinde Apache Hadoop ile Apache Pig kullanma](hdinsight-use-pig.md)
* [HDInsight üzerinde Apache Hadoop ile MapReduce kullanma](hdinsight-use-mapreduce.md)
