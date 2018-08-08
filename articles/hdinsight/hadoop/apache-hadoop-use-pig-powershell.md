---
title: HDInsight - Azure PowerShell ile Hadoop Pig kullanma
description: Azure PowerShell kullanarak HDInsight üzerinde Hadoop kümesi için pig işleri göndermeyi öğrenin.
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/09/2018
ms.author: jasonh
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: d8a729177328b2f6f4e7e75f133b91ddb4db5a61
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39597601"
---
# <a name="use-azure-powershell-to-run-pig-jobs-with-hdinsight"></a>HDInsight ile Pig işleri çalıştırmak için Azure PowerShell'i kullanma

[!INCLUDE [pig-selector](../../../includes/hdinsight-selector-use-pig.md)]

Bu belgede, HDInsight kümesinde bir Hadoop için Pig işleri göndermek için Azure PowerShell kullanarak bir örnek sağlar. Pig, MapReduce işleri bir dil (Pig Latin) kullanarak söz konusu model veri dönüşümleri yazmaya yerine eşleyin ve işlevleri azaltılmasına olanak tanır.

> [!NOTE]
> Bu belgede ayrıntılı açıklamasını örneklerde kullanılan Pig Latin açıklamaları neler sağlamaz. Bu örnekte kullanılan Pig Latin hakkında daha fazla bilgi için bkz. [HDInsight üzerinde Hadoop ile Pig kullanma](hdinsight-use-pig.md).

## <a id="prereq"></a>Önkoşullar

* **Bir Azure HDInsight kümesi**

  > [!IMPORTANT]
  > Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

* **Azure PowerShell içeren bir iş istasyonu**.

## <a id="powershell"></a>Pig işi çalıştırma

Azure PowerShell sağlar *cmdlet'leri* uzaktan üzerinde HDInsight Pig işleri çalıştırmanıza izin verir. Dahili olarak, PowerShell, REST çağrılarını kullanır [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) HDInsight kümesinde çalışıyor.

Pig işleri çalıştıran bir uzak HDInsight kümesinde aşağıdaki cmdlet'ler kullanılır:

* **Connect-AzureRmAccount**: Azure PowerShell, Azure aboneliğiniz için kimlik doğrulaması yapar.
* **Yeni AzureRmHDInsightPigJobDefinition**: oluşturur bir *iş tanımı* belirtilen Pig Latin açıklamaları kullanarak.
* **Başlangıç AzureRmHDInsightJob**: iş tanımı için HDInsight gönderir ve bir iş başlatılır. A *iş* nesne döndürülür.
* **Bekleme AzureRmHDInsightJob**: İş nesnesinde işinin durumunu denetlemek için kullanır. İşi tamamlandı ya da bekleme zamanı aşıldı kadar bekler.
* **Get-AzureRmHDInsightJobOutput**: tanımlı işlemin çıktısını almak için kullanılır.

Aşağıdaki adımlarda, HDInsight kümesinde bir işi çalıştırmak için bu cmdlet'leri kullanmaya nasıl ekleyebileceğiniz gösterilmektedir.

1. Bir Düzenleyicisi'ni kullanarak aşağıdaki kodun da Kaydet **pigjob.ps1**.

    [!code-powershell[main](../../../powershell_scripts/hdinsight/use-pig/use-pig.ps1?range=5-51)]

1. Yeni bir Windows PowerShell komut istemi açın. Dizinleri konumuna **pigjob.ps1** dosya ve betiği çalıştırmak için aşağıdaki komutu kullanın:

        .\pigjob.ps1

    Azure aboneliğinizde oturum açmak için istenir. Ardından, HTTPs/yönetici hesabı adını ve HDInsight kümesi için parola istenir.

2. İş tamamlandığında, bilgileri aşağıdaki metne benzer döndürmesi gerekir:

        Start the Pig job ...
        Wait for the Pig job to complete ...
        Display the standard output ...
        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

## <a id="troubleshooting"></a>Sorun giderme

İş tamamlandığında hiçbir bilgi döndürülürse, hata günlüklerini görüntüleyin. Bu işi hata bilgilerini görüntülemek için aşağıdaki komutu sonuna ekleyin **pigjob.ps1** dosyasını kaydedin ve yeniden çalıştırın.

    # Print the output of the Pig job.
    Write-Host "Display the standard error output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $pigJob.JobId `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

Bu cmdlet iş işlenirken STDERR yazılmıştır bilgileri döndürür.

## <a id="summary"></a>Özet
Gördüğünüz gibi Azure PowerShell, bir HDInsight kümesinde Pig işleri çalıştırma, iş durumunu izlemek ve çıktısını almak için kolay bir yol sağlar.

## <a id="nextsteps"></a>Sonraki adımlar
HDInsight, Pig hakkında genel bilgi için:

* [HDInsight üzerinde Hadoop ile Pig kullanma](hdinsight-use-pig.md)

Diğer yollar hakkında daha fazla bilgi için HDInsight üzerinde Hadoop ile çalışabilirsiniz:

* [HDInsight üzerinde Hadoop ile Hive kullanma](hdinsight-use-hive.md)
* [HDInsight üzerinde Hadoop ile MapReduce kullanma](hdinsight-use-mapreduce.md)
