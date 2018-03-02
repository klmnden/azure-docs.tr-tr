---
title: "Kullanım MapReduce ve hdınsight'ta - Azure Hadoop ile Curl | Microsoft Docs"
description: "Uzaktan MapReduce işleri Curl kullanarak Hdınsight'ta Hadoop ile çalıştırma hakkında bilgi edinin."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: bc6daf37-fcdc-467a-a8a8-6fb2f0f773d1
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/27/2018
ms.author: larryfr
ms.openlocfilehash: e48e9f833db86f01d944133c8a32d2c6b27b7b48
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-rest"></a>REST kullanarak Hdınsight'ta Hadoop ile MapReduce işleri çalıştırma

Hdınsight kümesinde bir Hadoop MapReduce işleri çalıştırmayı WebHCat REST API kullanmayı öğrenin. Curl MapReduce işleri çalıştırmak için ham HTTP isteklerini kullanarak Hdınsight ile nasıl etkileşim kurabileceğine göstermek için kullanılır.

> [!NOTE]
> Linux tabanlı Hadoop sunucuları kullanarak bilginiz, ancak yeni Hdınsight için, bkz: [hdınsight'ta Linux tabanlı Hadoop hakkında bilmeniz gerekenleri](../hdinsight-hadoop-linux-information.md) belge.


## <a id="prereq"></a>Önkoşullar

* Hdınsight kümesi Hadoop'ta
* Windows PowerShell veya [Curl](http://curl.haxx.se/) ve [jq](http://stedolan.github.io/jq/)

## <a id="curl"></a>Bir MapReduce işi çalıştırma

> [!NOTE]
> WebHCat ile Curl veya başka bir REST iletişimini kullanırken Hdınsight küme yönetici kullanıcı adını ve parolasını sağlayarak isteklerin kimliğini doğrulaması gerekir. Sunucuya istek göndermek için kullanılan URI'ın bir parçası olarak küme adını kullanmanız gerekir.
>
> REST API kullanarak güvenli [temel erişimi kimlik doğrulaması](http://en.wikipedia.org/wiki/Basic_access_authentication). Ayrıca, kimlik bilgilerinizin sunucuya güvenli bir şekilde gönderildiğinden emin olmak için HTTPS kullanarak istekleri her zaman yapmanız gerekir.

1. Bu belgedeki komut dosyaları tarafından kullanılan Küme oturum açma ayarlamak için followig komutlarından birini kullanın:

    ```bash
    read -p "Enter your cluster login account name: " LOGIN
    ```

    ```powershell
    $creds = Get-Credential -UserName admin -Message "Enter the cluster login name and password"
    ```

2. Küme adı ayarlamak için aşağıdaki komutlardan birini kullanın:

    ```bash
    read -p "Enter the HDInsight cluster name: " CLUSTERNAME
    ```

    ```powershell
    $clusterName = Read-Host -Prompt "Enter the HDInsight cluster name"
    ```

3. HDInsight kümenize bağlanabildiğinizi doğrulamak için bir komut satırında aşağıdaki komutu kullanın:

    ```bash
    curl -u $LOGIN -G https://$CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clustername.azurehdinsight.net/templeton/v1/status" `
        -Credential $creds `
        -UseBasicParsing
    $resp.Content
    ```

    Aşağıdaki JSON benzer bir yanıt alırsınız:

        {"status":"ok","version":"v1"}

    Bu komutta kullanılan parametreler aşağıdaki gibidir:

   * **-u**: kullanıcı adı ve parola istek kimliğini doğrulamak için kullanılan gösterir
   * **-G**: Bu işlem bir GET isteği olduğunu gösterir

   URI başlangıcını **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, tüm istekler için aynıdır.

4. Bir MapReduce işi göndermek için aşağıdaki komutu kullanın:

    ```bash
    JOBID=`curl -u $LOGIN -d user.name=$LOGIN -d jar=/example/jars/hadoop-mapreduce-examples.jar -d class=wordcount -d arg=/example/data/gutenberg/davinci.txt -d arg=/example/data/output https://$CLUSTERNAME.azurehdinsight.net/templeton/v1/mapreduce/jar | jq .id`
    echo $JOBID
    ```

    ```powershell
    $reqParams = @{}
    $reqParams."user.name" = "admin"
    $reqParams.jar = "/example/jars/hadoop-mapreduce-examples.jar"
    $reqParams.class = "wordcount"
    $reqParams.arg = @()
    $reqParams.arg += "/example/data/gutenberg/davinci.txt"
    $reqparams.arg += "/example/data/output"
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/templeton/v1/mapreduce/jar" `
       -Credential $creds `
       -Body $reqParams `
       -Method POST `
       -UseBasicParsing
    $jobID = (ConvertFrom-Json $resp.Content).id
    $jobID
    ```

    Bu istek bir sınıftan jar dosyasındaki bir MapReduce işi başlatır WebHCat (/ mapreduce/jar) URI sonuna söyler. Bu komutta kullanılan parametreler aşağıdaki gibidir:

   * **-d**: `-G` isteği için POST yöntemini Varsayılanları şekilde, kullanılmaz. `-d` istekle birlikte gönderilen veri değerleri belirtir.
    * **User.Name**: komutu çalıştıran kullanıcının
    * **jar**: olmasını sınıfı içeren jar dosyasını konumunu çalıştı
    * **sınıf**: MapReduce mantığı içeren sınıfı
    * **arg**: MapReduce işi için geçirilecek bağımsız değişkenler. Bu durumda, giriş metin dosyasını ve çıktısı için kullanılacak dizini

   Bu komut, iş durumunu denetlemek için kullanılan bir iş kimliği döndürmesi gerekir:

       job_1415651640909_0026

5. İş durumunu denetlemek için aşağıdaki komutu kullanın:

    ```bash
    curl -G -u $LOGIN -d user.name=$LOGIN https://$CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/$JOBID | jq .status.state
    ```

    ```powershell
    $reqParams=@{"user.name"="admin"}
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/templeton/v1/jobs/$jobID" `
       -Credential $creds `
       -Body $reqParams `
       -UseBasicParsing
    # ConvertFrom-JSON can't handle duplicate names with different case
    # So change one to prevent the error
    $fixDup=$resp.Content.Replace("jobID","job_ID")
    (ConvertFrom-Json $fixDup).status.state
    ```

    İş tamamlandıktan durumu döndürülür `SUCCEEDED`.

   > [!NOTE]
   > Bu Curl isteği bir JSON belgesi işle ilgili bilgilerle döndürür. Jq yalnızca durum değeri almak için kullanılır.

6. İş durumunu değiştiği için `SUCCEEDED`, Azure Blob depolama alanından iş sonuçlarını alabilirsiniz. `statusdir` Sorguyla geçirilen parametre çıkış dosyasının konumunu içerir. Bu örnekte, konumdur `/example/curl`. Bu adres iş çıktısı adresinde kümeleri varsayılan depolama depolar `/example/curl`.

Liste ve kullanarak bu dosyaları indirmek [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli). Azure CLI bloblarından ile çalışma hakkında daha fazla bilgi için bkz: [Azure Storage ile Azure CLI 2.0 kullanan](../../storage/common/storage-azure-cli.md#create-and-manage-blobs) belge.

## <a id="nextsteps"></a>Sonraki adımlar

Hdınsight'ta MapReduce işleri hakkında genel bilgi için:

* [Hdınsight'ta Hadoop ile MapReduce kullanma](hdinsight-use-mapreduce.md)

Diğer yolları hakkında bilgi için hdınsight'ta Hadoop ile çalışabilirsiniz:

* [Hdınsight'ta Hadoop ile Hive kullanma](hdinsight-use-hive.md)
* [Hdınsight'ta Hadoop ile pig kullanma](hdinsight-use-pig.md)

Bu makalede kullanılan REST arabirimi hakkında daha fazla bilgi için bkz: [WebHCat başvuru](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).