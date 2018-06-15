---
title: Hadoop Pig hdınsight'ta - Azure PowerShell ile kullanma | Microsoft Docs
description: Azure PowerShell kullanarak hdınsight'ta Hadoop kümesi pig iş göndermek öğrenin.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.assetid: 737089c1-b494-4387-9def-7b4dac3be532
ms.service: hdinsight
ms.devlang: na
ms.topic: conceptual
ms.date: 05/09/2018
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: a3e62647ec41cfdfc7f0f7bb55474215ab435ee8
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
ms.locfileid: "33939449"
---
# <a name="use-azure-powershell-to-run-pig-jobs-with-hdinsight"></a>Hdınsight ile Pig işlerini çalıştırmak için Azure PowerShell'i kullanma

[!INCLUDE [pig-selector](../../../includes/hdinsight-selector-use-pig.md)]

Bu belge, Azure PowerShell kullanarak Hdınsight kümesinde bir Hadoop Pig işleri göndermek için bir örnek sağlar. Pig, MapReduce işleri dili (Pig Latin) kullanarak bu modeller veri dönüşümleri yazmak yerine eşleme ve İşlevler azaltmak sağlar.

> [!NOTE]
> Bu belgedeki örneklerde kullanılan Pig Latin deyimleri ne ayrıntılı bir açıklama sağlamaz. Bu örnekte kullanılan Pig Latin hakkında daha fazla bilgi için bkz: [hdınsight'ta Hadoop ile Pig kullanma](hdinsight-use-pig.md).

## <a id="prereq"></a>Önkoşullar

* **Azure Hdınsight kümesi**

  > [!IMPORTANT]
  > Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

* **Azure PowerShell içeren bir iş istasyonu**.

## <a id="powershell"></a>Pig işi çalıştırma

Azure PowerShell sağlar *cmdlet'leri* Hdınsight'ta Pig işleri uzaktan çalıştırma izin verir. Dahili olarak, PowerShell REST çağrılarını kullanır [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) Hdınsight kümesinde çalışan.

Aşağıdaki cmdlet, Pig işleri uzaktan bir Hdınsight kümesine çalıştırılırken kullanılır:

* **Connect-AzureRmAccount**: Azure PowerShell'i Azure aboneliğinize kimliğini doğrular.
* **AzureRmHDInsightPigJobDefinition yeni**: oluşturur bir *iş tanımı* belirtilen Pig Latin deyimleri kullanarak.
* **Başlangıç AzureRmHDInsightJob**: iş tanımı için Hdınsight gönderir ve işini başlatır. A *iş* nesne döndürülür.
* **Bekleme AzureRmHDInsightJob**: iş nesnesi işinin durumunu denetlemek için kullanır. İş tamamlandı ya da bekleme süresi aşıldı kadar bekler.
* **Get-AzureRmHDInsightJobOutput**: işlemin çıktısını almak için kullanılır.

Aşağıdaki adımlarda bu cmdlet'leri, Hdınsight kümesinde bir işi çalıştırmak için nasıl kullanılacağı gösterilmektedir.

1. Bir düzenleyici kullanarak aşağıdaki kodu olarak Kaydet **pigjob.ps1**.

    [!code-powershell[main](../../../powershell_scripts/hdinsight/use-pig/use-pig.ps1?range=5-51)]

1. Yeni bir Windows PowerShell komut istemi açın. Dizin konumuna değiştirme **pigjob.ps1** dosya sonra komut dosyasını çalıştırmak için aşağıdaki komutu kullanın:

        .\pigjob.ps1

    Azure aboneliğinizde oturum istenir. Ardından, HTTPs/Yönetici hesap adı ve Hdınsight kümesi için parola istenir.

2. İş tamamlandığında, bilgileri aşağıdaki metni benzer döndürmesi gerekir:

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

İş tamamlandığında hiçbir bilgi döndürülürse, hata günlüklerine bakın. Bu işi için hata bilgilerini görüntülemek için aşağıdaki komutu sonuna ekleyin **pigjob.ps1** dosya, dosyayı kaydedin ve yeniden çalıştırın.

    # Print the output of the Pig job.
    Write-Host "Display the standard error output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $pigJob.JobId `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

Bu cmdlet STDERR iş işleme sırasında yazıldı bilgiler döndürür.

## <a id="summary"></a>Özet
Gördüğünüz gibi Azure PowerShell bir Hdınsight kümesine Pig işleri çalıştırma, iş durumunu izleyebilir ve çıkış almak için kolay bir yol sağlar.

## <a id="nextsteps"></a>Sonraki adımlar
Hdınsight'ta Pig hakkında genel bilgi için:

* [Hdınsight'ta Hadoop ile pig kullanma](hdinsight-use-pig.md)

Diğer yolları hakkında bilgi için hdınsight'ta Hadoop ile çalışabilirsiniz:

* [Hdınsight'ta Hadoop ile Hive kullanma](hdinsight-use-hive.md)
* [Hdınsight'ta Hadoop ile MapReduce kullanma](hdinsight-use-mapreduce.md)
