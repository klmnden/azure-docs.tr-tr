---
title: Hadoop - Azure Hdınsight ile MapReduce ve PowerShell kullanma | Microsoft Docs
description: PowerShell uzaktan MapReduce işleri Hdınsight'ta Hadoop ile çalıştırmak için nasıl kullanılacağını öğrenin.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 21b56d32-1785-4d44-8ae8-94467c12cfba
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 05/09/2018
ms.author: larryfr
ms.openlocfilehash: 7416d064f89515feb04523ca6d4ea73f37c14e38
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-powershell"></a>PowerShell kullanarak Hdınsight'ta Hadoop ile MapReduce işleri çalıştırma

[!INCLUDE [mapreduce-selector](../../../includes/hdinsight-selector-use-mapreduce.md)]

Bu belge, Hdınsight kümesinde bir Hadoop MapReduce işi çalıştırmak için Azure PowerShell kullanarak bir örnek sağlar.

## <a id="prereq"></a>Önkoşullar

* **Azure Hdınsight (Hadoop hdınsight) kümesi**

  > [!IMPORTANT]
  > Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

* **Azure PowerShell içeren bir iş istasyonu**.

## <a id="powershell"></a>Bir MapReduce işi çalıştırma

Azure PowerShell sağlar *cmdlet'leri* Hdınsight'ta MapReduce işleri uzaktan çalıştırma izin verir. Dahili olarak, PowerShell KALAN çağrılar [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (Templeton adıysa) Hdınsight kümesinde çalışan.

Aşağıdaki cmdlet, MapReduce işleri uzaktan Hdınsight kümesinde çalıştırılırken kullanılır.

* **Connect-AzureRmAccount**: Azure PowerShell'i Azure aboneliğinize kimliğini doğrular.

* **AzureRmHDInsightMapReduceJobDefinition yeni**: yeni bir *iş tanımı* belirtilen MapReduce bilgileri kullanarak.

* **Başlangıç AzureRmHDInsightJob**: iş tanımı için Hdınsight gönderir ve işini başlatır. A *iş* nesne döndürülür.

* **Bekleme AzureRmHDInsightJob**: iş nesnesi işinin durumunu denetlemek için kullanır. İş tamamlandığında veya bekleme süresi aşılırsa kadar bekler.

* **Get-AzureRmHDInsightJobOutput**: işlemin çıktısını almak için kullanılır.

Aşağıdaki adımlarda bu cmdlet'leri Hdınsight kümenizdeki bir işi çalıştırmak için nasıl kullanılacağı gösterilmektedir.

1. Bir düzenleyici kullanarak aşağıdaki kodu olarak Kaydet **mapreducejob.ps1**.

    [!code-powershell[main](../../../powershell_scripts/hdinsight/use-mapreduce/use-mapreduce.ps1?range=5-69)]

2. Yeni bir **Azure PowerShell** komut istemi. Dizin konumuna değiştirme **mapreducejob.ps1** dosya sonra komut dosyasını çalıştırmak için aşağıdaki komutu kullanın:

        .\mapreducejob.ps1

    Komut dosyasını çalıştırdığınızda, Hdınsight kümesi ve küme oturum açma adı istenir. Ayrıca, Azure aboneliğinizin kimlik doğrulaması istenebilir.

3. İş tamamlandığında, aşağıdakine benzer bir çıktı alırsınız:

        Cluster         : CLUSTERNAME
        ExitCode        : 0
        Name            : wordcount
        PercentComplete : map 100% reduce 100%
        Query           :
        State           : Completed
        StatusDirectory : f1ed2028-afe8-402f-a24b-13cc17858097
        SubmissionTime  : 12/5/2014 8:34:09 PM
        JobId           : job_1415949758166_0071

    Bu çıktı, iş başarıyla tamamlandığını gösterir.

    > [!NOTE]
    > Varsa **ExitCode** bir değer 0'dan bkz [sorun giderme](#troubleshooting).

    Bu örnek ayrıca indirilen dosyaları depolayan bir **çýktý.txt** komut dosyasını çalıştırmak dizindeki dosyayı.

### <a name="view-output"></a>Görünüm çıktı

Sözcükler ve iş tarafından üretilen sayılar görmek için açın **çýktý.txt** dosyasını bir metin düzenleyicisinde.

> [!NOTE]
> Bir MapReduce işi çıktı dosyalarını değişmez. Bu nedenle, bu örnek çalıştırırsanız, çıktı dosyası adını değiştirmeniz gerekir.

## <a id="troubleshooting"></a>Sorun giderme

İş tamamlandığında hiçbir bilgi döndürülürse, iş hataları görüntüleyin. Bu işi için hata bilgilerini görüntülemek için aşağıdaki komutu sonuna ekleyin **mapreducejob.ps1** dosya, dosyayı kaydedin ve yeniden çalıştırın.

```powershell
# Print the output of the WordCount job.
Write-Host "Display the standard output ..." -ForegroundColor Green
Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $wordCountJob.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

Bu cmdlet iş çalışırken STDERR yazıldı bilgileri döndürür.

## <a id="summary"></a>Özet

Gördüğünüz gibi Azure PowerShell bir Hdınsight kümesine MapReduce işleri çalıştırma, iş durumunu izleyebilir ve çıkış almak için kolay bir yol sağlar.

## <a id="nextsteps"></a>Sonraki adımlar

Hdınsight'ta MapReduce işleri hakkında genel bilgi için:

* [Hdınsight Hadoop MapReduce kullanın](hdinsight-use-mapreduce.md)

Diğer yolları hakkında bilgi için hdınsight'ta Hadoop ile çalışabilirsiniz:

* [Hdınsight'ta Hadoop ile Hive kullanma](hdinsight-use-hive.md)
* [Hdınsight'ta Hadoop ile pig kullanma](hdinsight-use-pig.md)
