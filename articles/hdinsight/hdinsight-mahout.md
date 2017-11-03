---
title: "PowerShell - Azure Hdınsight'ta Mahout kullanarak öneri oluşturmak | Microsoft Docs"
description: "İstemci üzerinde çalışan bir PowerShell komut dosyasından learning kitaplığı Apache Mahout makine Hdınsight (Hadoop) ile film önerileri oluşturma için nasıl kullanılacağını öğrenin."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 07b57208-32aa-4e59-900a-6c934fa1b7a7
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: larryfr
ms.openlocfilehash: 934de9ca2df48b29ef7a56d5729d59d77875ea7b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="generate-movie-recommendations-by-using-apache-mahout-with-hadoop-in-hdinsight-powershell"></a>(PowerShell) hdınsight'ta Hadoop ile Apache Mahout kullanarak film önerileri oluşturma

[!INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

Nasıl kullanacağınızı öğrenin [Apache Mahout](http://mahout.apache.org) machine learning kitaplığı Azure Hdınsight'ın Film önerileri oluşturma ile. Bu belge örnekte Mahout işlerini çalıştırmak için Azure PowerShell'i kullanır.

## <a name="prerequisites"></a>Ön koşullar

* Linux tabanlı Hdınsight kümesi. Bir oluşturma hakkında daha fazla bilgi için bkz: [Hdınsight'ta Linux tabanlı Hadoop ile çalışmaya başlamak][getstarted].

> [!IMPORTANT]
> Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* [Azure PowerShell](/powershell/azure/overview)

## <a name="recommendations"></a>Azure PowerShell kullanarak önerileri oluşturma

> [!WARNING]
> Bu bölümde iş Azure PowerShell kullanarak çalışır. Mahout ile sağlanan sınıfların çoğu Azure PowerShell ile şu anda çalışmıyor. Azure PowerShell ile çalışmaz sınıfları listesi için bkz: [sorun giderme](#troubleshooting) bölümü.
>
> Hdınsight ve çalışma Mahout örnekler küme üzerinde doğrudan bağlanmak için SSH kullanarak bir örnek için bkz: [Mahout ve Hdınsight (SSH) kullanarak film önerileri oluşturma](hdinsight-hadoop-mahout-linux-mac.md).

Mahout tarafından sağlanan işlevleri bir öneri altyapısı biridir. Bu altyapı biçiminde verilerini kabul eden `userID`, `itemId`, ve `prefValue` (kullanıcılar tercih öğesi için). Mahout veri önerileri yapmak için kullanılan benzer öğe Tercihler kullanıcılarla belirlemek için kullanır.

Aşağıdaki örnek öneri işleminin nasıl çalıştığı, Basitleştirilmiş bir gözden geçirme verilmiştir:

* **Ortak oluşumu**: Can, Alice ve Bob tüm beğendiğinizi *yıldız çatışmaları*, *geri Empire düşer*, ve *Jedi dönüşünü*. Bu filmler herhangi biri de gibi kullanıcıların diğer iki ister mahout belirler.

* **Ortak oluşumu**: Bob ve Alice de beğendiğinizi *hayali İstilası*, *klonlar saldırı*, ve *Sith Revenge*. Önceki üç filmler de ilişkilendirilmiş kullanıcılar bu filmler ister mahout belirler.

* **Benzerlik öneri**: çünkü Joe beğendiğinizi ilk üç filmler, Mahout o beğendiğinizi benzer Tercihler başkalarıyla filmler görünür, ancak Joe olmayan izlenen (beğendiğinizi/derecelendirilmiş). Bu durumda, Mahout önerir *hayali İstilası*, *klonlar saldırı*, ve *Sith Revenge*.

### <a name="understanding-the-data"></a>Veri anlama

[GroupLens araştırma] [ movielens] Mahout ile uyumlu bir biçimde film derecelendirme veri sağlar. Bu verilerin varsayılan depolama konumunda kümenize için kullanılabilir `/HdiSamples//HdiSamples/MahoutMovieData`.

İki dosya vardır `moviedb.txt` (filmler hakkındaki bilgiler) ve `user-ratings.txt`. `user-ratings.txt` Dosyası Çözümleme sırasında kullanılır. `moviedb.txt` Dosya çözümleme sonuçlarını görüntülerken, kullanımı kolay metin sağlamak için kullanılır.

Kullanıcı-ratings.txt bulunan verileri yapısını sahip `userID`, `movieID`, `userRating`, ve `timestamp`, söyleyen bize nasıl yüksek oranda her kullanıcı bir filmi derecelendirilir. Verileri bir örneği burada verilmiştir:

    196    242    3    881250949
    186    302    3    891717742
    22    377    1    878887116
    244    51    2    880606923
    166    346    1    886397596

### <a name="run-the-job"></a>İşi çalıştır

Film verilerle Mahout öneri altyapısı kullanan bir iş çalıştırmak için aşağıdaki Windows PowerShell betiğini kullanın:

> [!NOTE]
> Bu dosya Hdınsight kümenize bağlanmak ve işlerini çalıştırmak için kullanılan bilgileri ister. İşlerini tamamlayıp çıktı.txt dosyasını karşıdan yüklemek birkaç dakika sürebilir.

[!code-powershell[main](../../powershell_scripts/hdinsight/mahout/use-mahout.ps1?range=5-98)]

> [!NOTE]
> Mahout işleri iş işlenirken oluşturulan geçici verileri kaldırmayın. `--tempDir` Parametresi geçici dosyalar belirli bir dizine yalıtmak için örnek proje belirtilen.

Mahout iş STDOUT çıktı döndürmez. Bunun yerine, belirtilen çıkış dizinine depolar **bölümü r 00000**. Bu dosyaya betiğini indirir **çýktý.txt** istasyonunuzda geçerli dizin.

Aşağıdaki metni, bu dosyanın içeriğini örneğidir:

    1    [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
    2    [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
    3    [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
    4    [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

İlk sütun `userID`. İçinde yer alan değerler ' [' ve ']' olan `movieId`:`recommendationScore`.

Komut dosyası ayrıca indirmeleri `moviedb.txt` ve `user-ratings.txt` daha okunabilir olması için çıktı biçimlendirmek için gereken dosyalar.

### <a name="view-the-output"></a>Çıktısını görüntüleyin

Oluşturulan çıktı bir uygulamada kullanmak için Tamam olabilir, ancak kullanıcı dostu değil. `moviedb.txt` Sunucudan çözümlemek için kullanılan `movieId` film adı. Film adları ile ilgili öneriler görüntülemek için aşağıdaki PowerShell betiğini kullanın:

[!code-powershell[main](../../powershell_scripts/hdinsight/mahout/use-mahout.ps1?range=106-180)]

Öneriler kullanımı kolay bir biçimde görüntülemek için aşağıdaki komutu kullanın: 

```powershell
.\show-recommendation.ps1 -userId 4 -userDataFile .\user-ratings.txt -movieFile .\moviedb.txt -recommendationFile .\output.txt
```

Çıktı aşağıdaki metne benzer:

    Reading movies descriptions
    Reading rated movies
    Reading recommendations
    Rated movies
    ---------------------------
    Movie                                    Rating
    -----                                    ------
    Devil's Own, The (1997)                  1
    Alien: Resurrection (1997)               3
    187 (1997)                               2
    (lines ommitted)

    ---------------------------
    Recommended movies
    ---------------------------

    Movie                                    Score
    -----                                    -----
    Good Will Hunting (1997)                 4.6504064
    Swingers (1996)                          4.6862745
    Wings of the Dove, The (1997)            4.6666665
    People vs. Larry Flynt, The (1996)       4.834559
    Everyone Says I Love You (1996)          4.707071
    Secrets & Lies (1996)                    4.818182
    That Thing You Do! (1996)                4.75
    Grosse Pointe Blank (1997)               4.8235292
    Donnie Brasco (1997)                     4.6792455
    Lone Star (1996)                         4.7099237

## <a name="troubleshooting"></a>Sorun giderme

### <a name="cannot-overwrite-files"></a>Dosyaları üzerine yazılamıyor

Temizlemeden işleme sırasında oluşturulan geçici dosyaları mahout işleri yapın. Ayrıca, işleri var olan çıkış dosyasının üzerine yazmaz.

Mahout işleri çalıştırma esnasında oluşacak hataları önlemek için çalıştırmaları arasında geçici ve çıktı dosyaları silin. Bu belgedeki önceki komut dosyaları tarafından oluşturulan dosyaları kaldırmak için aşağıdaki PowerShell betiğini kullanın:

```powershell
# Login to your Azure subscription
# Is there an active Azure subscription?
$sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Add-AzureRmAccount
}

# Get cluster info
$clusterName = Read-Host -Prompt "Enter the HDInsight cluster name"
$creds=Get-Credential -Message "Enter the login for the cluster"

#Get the cluster info so we can get the resource group, storage, etc.
$clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
$resourceGroup = $clusterInfo.ResourceGroup
$storageAccountName = $clusterInfo.DefaultStorageAccount.split('.')[0]
$container = $clusterInfo.DefaultStorageContainer
$storageAccountKey = (Get-AzureRmStorageAccountKey `
    -Name $storageAccountName `
-ResourceGroupName $resourceGroup)[0].Value

#Create a storage context and upload the file
$context = New-AzureStorageContext `
    -StorageAccountName $storageAccountName `
    -StorageAccountKey $storageAccountKey

#Azure PowerShell can't delete blobs using wildcard,
#so have to get a list and delete one at a time
# Start with the output
$blobs = Get-AzureStorageBlob -Container $container -Context $context -Prefix "example/out"
foreach($blob in $blobs)
{
    Remove-AzureStorageBlob -Blob $blob.Name -Container $container -context $context
}
# Next the temp files
$blobs = Get-AzureStorageBlob -Container $container -Context $context -Prefix "example/temp"
foreach($blob in $blobs)
{
    Remove-AzureStorageBlob -Blob $blob.Name -Container $container -context $context
}
```

### <a name="nopowershell"></a>Azure PowerShell ile çalışmaz sınıfları

Aşağıdaki sınıfları kullanan mahout işleri Windows Powershell'den kullanıldığında çeşitli hata iletileri döndürün:

* org.apache.mahout.utils.clustering.ClusterDumper
* org.apache.mahout.utils.SequenceFileDumper
* org.apache.mahout.utils.vectors.lucene.Driver
* org.apache.mahout.utils.vectors.arff.Driver
* org.apache.mahout.text.WikipediaToSequenceFile
* org.apache.mahout.clustering.streaming.tools.ResplitSequenceFiles
* org.apache.mahout.clustering.streaming.tools.ClusterQualitySummarizer
* org.apache.mahout.classifier.sgd.TrainLogistic
* org.apache.mahout.classifier.sgd.RunLogistic
* org.apache.mahout.classifier.sgd.TrainAdaptiveLogistic
* org.apache.mahout.classifier.sgd.ValidateAdaptiveLogistic
* org.apache.mahout.classifier.sgd.RunAdaptiveLogistic
* org.apache.mahout.classifier.sequencelearning.hmm.BaumWelchTrainer
* org.apache.mahout.classifier.sequencelearning.hmm.ViterbiEvaluator
* org.apache.mahout.classifier.sequencelearning.hmm.RandomSequenceGenerator
* org.apache.mahout.classifier.df.tools.Describe

Bu sınıfları kullanan işlerini çalıştırmak için SSH kullanarak Hdınsight kümesine bağlanma ve komut satırından işleri çalıştırın. Mahout işlerini çalıştırmak için SSH kullanarak bir örnek için bkz: [Mahout ve Hdınsight (SSH) kullanarak film önerileri oluşturma](hdinsight-hadoop-mahout-linux-mac.md).

## <a name="next-steps"></a>Sonraki adımlar

Mahout kullanmayı öğrendiniz, Hdınsight'ta veri ile çalışmanın diğer yolları Bul:

* [Hdınsight ile hive](hdinsight-use-hive.md)
* [Hdınsight ile pig](hdinsight-use-pig.md)
* [Hdınsight ile MapReduce](hdinsight-use-mapreduce.md)

[build]: http://mahout.apache.org/developers/buildingmahout.html
[aps]: /powershell/azureps-cmdlets-docs
[movielens]: http://grouplens.org/datasets/movielens/
[100k]: http://files.grouplens.org/datasets/movielens/ml-100k.zip
[getstarted]: hdinsight-hadoop-linux-tutorial-get-started.md
[upload]: hdinsight-upload-data.md
[ml]: http://en.wikipedia.org/wiki/Machine_learning
[forest]: http://en.wikipedia.org/wiki/Random_forest
[enableremote]: ./media/hdinsight-mahout/enableremote.png
[connect]: ./media/hdinsight-mahout/connect.png
[hadoopcli]: ./media/hdinsight-mahout/hadoopcli.png
[tools]: https://github.com/Blackmist/hdinsight-tools
