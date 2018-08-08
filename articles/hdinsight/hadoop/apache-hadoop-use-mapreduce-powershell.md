---
title: -Azure HDInsight Hadoop ile MapReduce ve PowerShell kullanma
description: PowerShell uzaktan Hadoop ile MapReduce işleri HDInsight üzerinde çalıştırmak için kullanmayı öğrenin.
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/09/2018
ms.author: jasonh
ms.openlocfilehash: cab6fc652fa11db7dd1e9e9ae7f0a1a634dca3b0
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39591829"
---
# <a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-powershell"></a>MapReduce işleri PowerShell kullanarak HDInsight üzerinde Hadoop ile çalıştırın.

[!INCLUDE [mapreduce-selector](../../../includes/hdinsight-selector-use-mapreduce.md)]

Bu belgede bir MapReduce işi içinde bir Hadoop HDInsight kümesinde çalıştırmak için Azure PowerShell kullanarak bir örnek sağlar.

## <a id="prereq"></a>Önkoşullar

* **Bir Azure HDInsight (Hadoop HDInsight üzerinde) kümesi**

  > [!IMPORTANT]
  > Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

* **Azure PowerShell içeren bir iş istasyonu**.

## <a id="powershell"></a>Bir MapReduce işi çalıştırın

Azure PowerShell sağlar *cmdlet'leri* uzaktan üzerinde HDInsight MapReduce işleri çalıştırmanıza izin verir. Dahili olarak, PowerShell, REST çağrıları yapan [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (eski adıyla templeton da denir) HDInsight kümesinde çalışıyor.

MapReduce işleri çalıştıran bir uzak HDInsight kümesinde aşağıdaki cmdlet'ler kullanılır.

* **Connect-AzureRmAccount**: Azure PowerShell'in Azure aboneliğinize kimliğini doğrular.

* **Yeni AzureRmHDInsightMapReduceJobDefinition**: yeni bir oluşturur *iş tanımı* belirtilen MapReduce bilgileri kullanarak.

* **Başlangıç AzureRmHDInsightJob**: iş tanımı için HDInsight gönderir ve bir iş başlatılır. A *iş* nesne döndürülür.

* **Bekleme AzureRmHDInsightJob**: İş nesnesinde işinin durumunu denetlemek için kullanır. Bekleme süresi aşılırsa veya iş tamamlanana kadar bekler.

* **Get-AzureRmHDInsightJobOutput**: tanımlı işlemin çıktısını almak için kullanılır.

Aşağıdaki adımlarda HDInsight kümenizdeki bir işi çalıştırmak için bu cmdlet'leri kullanma gösterilmektedir.

1. Bir Düzenleyicisi'ni kullanarak aşağıdaki kodun da Kaydet **mapreducejob.ps1**.

    [!code-powershell[main](../../../powershell_scripts/hdinsight/use-mapreduce/use-mapreduce.ps1?range=5-69)]

2. Yeni bir **Azure PowerShell** komut istemi. Dizinleri konumuna **mapreducejob.ps1** dosya ve betiği çalıştırmak için aşağıdaki komutu kullanın:

        .\mapreducejob.ps1

    Betiği çalıştırmak için HDInsight kümesi ve küme oturum açma adı sorulur. Ayrıca, Azure aboneliğinizin kimlik doğrulaması istenebilir.

3. İş tamamlandığında aşağıdaki metne benzer bir çıktı alırsınız:

        Cluster         : CLUSTERNAME
        ExitCode        : 0
        Name            : wordcount
        PercentComplete : map 100% reduce 100%
        Query           :
        State           : Completed
        StatusDirectory : f1ed2028-afe8-402f-a24b-13cc17858097
        SubmissionTime  : 12/5/2014 8:34:09 PM
        JobId           : job_1415949758166_0071

    Bu çıkış, işin başarıyla tamamlandığını gösterir.

    > [!NOTE]
    > Varsa **ExitCode** değer 0'dan bkz [sorun giderme](#troubleshooting).

    Bu örnek ayrıca indirilen dosyaları depolayan bir **çýktý.txt** betiğini çalıştırdığınız dizindeki dosya.

### <a name="view-output"></a>Görünüm çıkış

Sözcükleri ve sayıları iş tarafından üretilen görmek için **çýktý.txt** dosyasını bir metin düzenleyicisinde.

> [!NOTE]
> Bir MapReduce işi'nın çıktı dosyalarını sabittir. Bu nedenle, bu örnek yeniden, çıkış dosyasının adını değiştirmek gerekir.

## <a id="troubleshooting"></a>Sorun giderme

İş tamamlandığında hiçbir bilgi döndürülürse, iş hataları görüntüleyin. Bu işi hata bilgilerini görüntülemek için aşağıdaki komutu sonuna ekleyin **mapreducejob.ps1** dosyasını kaydedin ve yeniden çalıştırın.

```powershell
# Print the output of the WordCount job.
Write-Host "Display the standard output ..." -ForegroundColor Green
Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $wordCountJob.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

Bu cmdlet iş çalışırken STDERR yazılmıştır bilgileri döndürür.

## <a id="summary"></a>Özet

Gördüğünüz gibi Azure PowerShell, bir HDInsight kümesinde MapReduce işleri çalıştırma, iş durumunu izlemek ve çıktısını almak için kolay bir yol sağlar.

## <a id="nextsteps"></a>Sonraki adımlar

HDInsight MapReduce işleri hakkında genel bilgi için:

* [HDInsight üzerinde Hadoop MapReduce kullanma](hdinsight-use-mapreduce.md)

Diğer yollar hakkında daha fazla bilgi için HDInsight üzerinde Hadoop ile çalışabilirsiniz:

* [HDInsight üzerinde Hadoop ile Hive kullanma](hdinsight-use-hive.md)
* [HDInsight üzerinde Hadoop ile Pig kullanma](hdinsight-use-pig.md)
