---
title: Powershell'den - Azure HDInsight Mahout kullanarak önerileri oluşturma
description: İstemcide çalışan bir PowerShell betiğinden Apache Mahout machine learning kitaplığı HDInsight (Hadoop) ile film önerileri oluşturma için nasıl kullanılacağını öğrenin.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/23/2018
ms.author: hrasheed
ms.openlocfilehash: df8e15bbde5ad60d2e5cb90ba37892c135070f41
ms.sourcegitcommit: 223604d8b6ef20a8c115ff877981ce22ada6155a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58359305"
---
# <a name="generate-movie-recommendations-by-using-apache-mahout-with-apache-hadoop-in-hdinsight-powershell"></a>HDInsight (PowerShell), Apache Hadoop ile Apache Mahout kullanarak film önerileri oluşturma

[!INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

Nasıl kullanacağınızı öğrenin [Apache Mahout](https://mahout.apache.org) makine öğrenimi kitaplığı olan Azure HDInsight'ın Film önerileri oluşturma. Bu belgede örnek Mahout işlerini çalıştırmak için Azure PowerShell kullanır.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

* Bir Linux tabanlı HDInsight kümesi. Bir oluşturma hakkında daha fazla bilgi için bkz: [HDInsight içinde Linux tabanlı Hadoop kullanmaya başlama][getstarted].

    > [!IMPORTANT]  
    > Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* [Azure PowerShell](/powershell/azure/overview)

## <a name="recommendations"></a>Azure PowerShell kullanarak önerileri oluşturma

> [!WARNING]  
> Bu bölümdeki işi, Azure PowerShell kullanarak çalışır. Mahout ile sağlanan sınıfları birçoğu Azure PowerShell ile şu anda çalışmıyor. Azure PowerShell ile çalışmayan sınıfların listesi için bkz. [sorun giderme](#troubleshooting) bölümü.
>
> HDInsight kümesi üzerinde doğrudan çalışma Mahout örnekler bağlanmak için SSH kullanarak bir örnek için bkz: [Apache Mahout ve HDInsight (SSH) kullanarak film önerileri oluşturma](hadoop/apache-hadoop-mahout-linux-mac.md).

Mahout tarafından sağlanan işlevlerden birini bir öneri altyapısıdır. Bu altyapı, veri biçimi kabul `userID`, `itemId`, ve `prefValue` (kullanıcıların tercih öğesi için). Mahout, verileri öneriler yapmak için kullanılan benzer öğe tercihleri, kullanıcılarla belirlemek için kullanır.

Aşağıdaki örnek, öneri işleminin nasıl çalıştığı, Basitleştirilmiş bir gözden geçirme:

* **Ortak oluşum**: ALi, Alice ve Bob tüm bağlanan *Star Wars*, *geri Empire durumda*, ve *Jedi dönüşü*. Ayrıca bu filmler herhangi biri gibi kullanıcılar diğer iki ister mahout belirler.

* **Ortak oluşum**: Bob ve Gamze ayrıca beğenmediğinizi *hayali İstilası*, *kopyaları saldırısını*, ve *Sith, Revenge*. Önceki üç filmler ayrıca beğenmediğinizi kullanıcılar bu filmler ister mahout belirler.

* **Benzerlik öneri**: Ali'nin ilk üç filmler beğenmediğinizi çünkü Mahout filmleri beğenmediğinizi benzer tercihleri, başkalarıyla görünür, ancak Joe (beğenmediğinizi/derecelendirilmiş) izlenen değil. Bu durumda, Mahout önerir *hayali İstilası*, *kopyaları saldırısını*, ve *Sith, Revenge*.

### <a name="understanding-the-data"></a>Verileri anlama

[GroupLens araştırma] [ movielens] Mahout ile uyumlu bir biçimde film derecelendirmesi veri sağlar. Bu verilerin varsayılan depolama alanı konumunda kümenize için kullanılabilir `/HdiSamples/HdiSamples/MahoutMovieData`.

İki dosya vardır `moviedb.txt` (filmlerle ilgili bilgiler) ve `user-ratings.txt`. `user-ratings.txt` Analiz sırasında kullanılır. `moviedb.txt` Dosyasının kullanıcı dostu metin analiz sonuçlarını görüntülerken sağlamak amacıyla kullanılır.

Kullanıcı-ratings.txt içerdiği veri yapısını sahip `userID`, `movieID`, `userRating`, ve `timestamp`, her kullanıcı bir filmi nasıl yüksek dereceli söyler. Verilerin bir örnek aşağıda verilmiştir:

    196    242    3    881250949
    186    302    3    891717742
    22     377    1    878887116
    244    51     2    880606923
    166    346    1    886397596

### <a name="run-the-job"></a>İşi çalıştırma

Mahout öneri altyapısının film verileri kullanan bir işi çalıştırmak için aşağıdaki Windows PowerShell betiğini kullanın:

> [!NOTE]  
> Bu dosya, HDInsight kümenize bağlanın ve işleri çalıştırmak için kullanılan bilgileri ister. Bu, işleri tamamlayın ve çýktý.txt dosyasını indirmeniz için birkaç dakika sürebilir.

[!code-powershell[main](../../powershell_scripts/hdinsight/mahout/use-mahout.ps1?range=5-98)]

> [!NOTE]  
> Mahout işleri iş işlenirken oluşan geçici veri kaldırmayın. `--tempDir` Geçici dosyaları belirli bir dizine yalıtmak için örnek işteki parametresi belirtildi.

Mahout iş çıktısını STDOUT'a döndürmez. Bunun yerine, belirtilen çıkış dizinde depoladığı **bölümü r 00000**. Betiği bu dosyaya indirir **çýktý.txt** istasyonunuzda geçerli dizin.

Bu dosyanın içeriği bir örneği aşağıda gösterilmiştir:

    1    [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
    2    [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
    3    [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
    4    [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

İlk sütun `userID`. Bulunan değerler ' [' ve ']' olan `movieId`:`recommendationScore`.

Betik ayrıca indirir `moviedb.txt` ve `user-ratings.txt` daha okunabilir olacak şekilde biçimlendirmek için gereken dosyaları.

### <a name="view-the-output"></a>Çıkışı görüntülemek

Oluşturulan çıktı Tamam kullanılmak üzere bir uygulama olsa bile, kullanıcı dostu değildir. `moviedb.txt` Sunucudan çözümlemek için kullanılan `movieId` film adı. Film adları ile öneriler görüntülemek için aşağıdaki PowerShell betiğini kullanın:

[!code-powershell[main](../../powershell_scripts/hdinsight/mahout/use-mahout.ps1?range=106-180)]

Öneriler kullanıcı dostu bir biçimde görüntülemek için aşağıdaki komutu kullanın: 

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

### <a name="cannot-overwrite-files"></a>Dosyaların üzerine yazılamıyor

Mahout işleri yapmak değil temizleme işlemi sırasında oluşturulan geçici dosyaları. Ayrıca, işleri çıkış varolan dosyanın üzerine yazma.

Mahout işlerini çalıştırma esnasında oluşacak hataları önlemek için çalıştırma arasında geçici ve çıkış dosyaları silin. Bu belgedeki önceki komut tarafından oluşturulan dosyaları kaldırmak için aşağıdaki PowerShell betiğini kullanın:

```powershell
# Login to your Azure subscription
# Is there an active Azure subscription?
$sub = Get-AzSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Connect-AzAccount
}

# Get cluster info
$clusterName = Read-Host -Prompt "Enter the HDInsight cluster name"
$creds=Get-Credential -Message "Enter the login for the cluster"

#Get the cluster info so we can get the resource group, storage, etc.
$clusterInfo = Get-AzHDInsightCluster -ClusterName $clusterName
$resourceGroup = $clusterInfo.ResourceGroup
$storageAccountName = $clusterInfo.DefaultStorageAccount.split('.')[0]
$container = $clusterInfo.DefaultStorageContainer
$storageAccountKey = (Get-AzStorageAccountKey `
    -Name $storageAccountName `
-ResourceGroupName $resourceGroup)[0].Value

#Create a storage context and upload the file
$context = New-AzStorageContext `
    -StorageAccountName $storageAccountName `
    -StorageAccountKey $storageAccountKey

#Azure PowerShell can't delete blobs using wildcard,
#so have to get a list and delete one at a time
# Start with the output
$blobs = Get-AzStorageBlob -Container $container -Context $context -Prefix "example/out"
foreach($blob in $blobs)
{
    Remove-AzStorageBlob -Blob $blob.Name -Container $container -context $context
}
# Next the temp files
$blobs = Get-AzStorageBlob -Container $container -Context $context -Prefix "example/temp"
foreach($blob in $blobs)
{
    Remove-AzStorageBlob -Blob $blob.Name -Container $container -context $context
}
```

### <a name="nopowershell"></a>Azure PowerShell ile çalışmayan sınıfları

Aşağıdaki sınıfları kullanan mahout işleri Windows Powershell'den kullanıldığında çeşitli hata iletileri döndürür:

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

Bu sınıfların kullanan işleri çalıştırmak için HDInsight kümesine SSH kullanarak bağlanın ve işleri komut satırından çalıştırın. Mahout işlerini çalıştırmak için SSH kullanarak bir örnek için bkz: [Apache Mahout ve HDInsight (SSH) kullanarak film önerileri oluşturma](hadoop/apache-hadoop-mahout-linux-mac.md).

## <a name="next-steps"></a>Sonraki adımlar

Apache Mahout kullanmayı öğrendiniz, HDInsight üzerinde verilerle çalışma için diğer yöntemler keşfedin:

* [Apache Hive ile HDInsight](hadoop/hdinsight-use-hive.md)
* [HDInsight ile Apache Pig](hadoop/hdinsight-use-pig.md)
* [HDInsight ile MapReduce](hadoop/hdinsight-use-mapreduce.md)

[build]: https://mahout.apache.org/developers/buildingmahout.html
[aps]: /powershell/azureps-cmdlets-docs
[movielens]: https://grouplens.org/datasets/movielens/
[100k]: https://files.grouplens.org/datasets/movielens/ml-100k.zip
[getstarted]:hadoop/apache-hadoop-linux-tutorial-get-started.md
[upload]: hdinsight-upload-data.md
[ml]: https://en.wikipedia.org/wiki/Machine_learning
[forest]: https://en.wikipedia.org/wiki/Random_forest
[enableremote]: ./media/hdinsight-mahout/enableremote.png
[connect]: ./media/hdinsight-mahout/connect.png
[hadoopcli]: ./media/hdinsight-mahout/hadoopcli.png
[tools]: https://github.com/Blackmist/hdinsight-tools
