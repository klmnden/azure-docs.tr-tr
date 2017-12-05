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
ms.date: 12/04/2017
ms.author: larryfr
ms.openlocfilehash: dd3e5904ee21ee74da5adaa65abd7865a82c8b36
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2017
---
# <a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-rest"></a>REST kullanarak Hdınsight'ta Hadoop ile MapReduce işleri çalıştırma

Hdınsight kümesinde bir Hadoop MapReduce işleri çalıştırmayı WebHCat REST API kullanmayı öğrenin. Curl MapReduce işleri çalıştırmak için ham HTTP isteklerini kullanarak Hdınsight ile nasıl etkileşim kurabileceğine göstermek için kullanılır.

> [!NOTE]
> Linux tabanlı Hadoop sunucuları kullanarak bilginiz, ancak yeni Hdınsight için, bkz: [hdınsight'ta Linux tabanlı Hadoop hakkında bilmeniz gerekenleri](../hdinsight-hadoop-linux-information.md) belge.


## <a id="prereq"></a>Önkoşullar

* Hdınsight kümesi Hadoop'ta
* [Curl](http://curl.haxx.se/)
* [jq](http://stedolan.github.io/jq/)

## <a id="curl"></a>Curl kullanarak MapReduce işleri çalıştırma

> [!NOTE]
> WebHCat ile Curl veya başka bir REST iletişimini kullanırken Hdınsight küme yönetici kullanıcı adını ve parolasını sağlayarak isteklerin kimliğini doğrulaması gerekir. Sunucuya istek göndermek için kullanılan URI'ın bir parçası olarak küme adını kullanmanız gerekir.
>
> Bu bölümdeki komutlar için değiştirme **yönetici** küme kimlik doğrulaması için kullanıcı. **CLUSTERNAME** değerini kümenizin adıyla değiştirin. İstendiğinde, kullanıcı hesabı için parola sağlayın.
>
> REST API kullanarak güvenli [temel erişimi kimlik doğrulaması](http://en.wikipedia.org/wiki/Basic_access_authentication). Ayrıca, kimlik bilgilerinizin sunucuya güvenli bir şekilde gönderildiğinden emin olmak için HTTPS kullanarak istekleri her zaman yapmanız gerekir.


1. HDInsight kümenize bağlanabildiğinizi doğrulamak için bir komut satırında aşağıdaki komutu kullanın:

    ```bash
    curl -u admin -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    Aşağıdaki JSON benzer bir yanıt alırsınız:

        {"status":"ok","version":"v1"}

    Bu komutta kullanılan parametreler aşağıdaki gibidir:

   * **-u**: kullanıcı adı ve parola istek kimliğini doğrulamak için kullanılan gösterir
   * **-G**: Bu işlem bir GET isteği olduğunu gösterir

   URI başlangıcını **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, tüm istekler için aynıdır.

2. Bir MapReduce işi göndermek için aşağıdaki komutu kullanın:

    ```bash
    curl -u admin -d user.name=admin -d jar=/example/jars/hadoop-mapreduce-examples.jar -d class=wordcount -d arg=/example/data/gutenberg/davinci.txt -d arg=/example/data/CurlOut https://CLUSTERNAME.azurehdinsight.net/templeton/v1/mapreduce/jar
    ```

    Bu istek bir sınıftan jar dosyasındaki bir MapReduce işi başlatır WebHCat (/ mapreduce/jar) URI sonuna söyler. Bu komutta kullanılan parametreler aşağıdaki gibidir:

   * **-d**: `-G` isteği için POST yöntemini Varsayılanları şekilde, kullanılmaz. `-d`istekle birlikte gönderilen veri değerleri belirtir.
    * **User.Name**: komutu çalıştıran kullanıcının
    * **jar**: olmasını sınıfı içeren jar dosyasını konumunu çalıştı
    * **sınıf**: MapReduce mantığı içeren sınıfı
    * **arg**: MapReduce işi için geçirilecek bağımsız değişkenler. Bu durumda, giriş metin dosyasını ve çıktısı için kullanılacak dizini

   Bu komut, iş durumunu denetlemek için kullanılan bir iş kimliği döndürmesi gerekir:

       {"id":"job_1415651640909_0026"}

3. İş durumunu denetlemek için aşağıdaki komutu kullanın:

    ```bash
    curl -G -u admin -d user.name=admin https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
    ```

    Değiştir **JOBID** önceki adımda döndürülen değer. Örneğin, dönüş değeri `{"id":"job_1415651640909_0026"}`, JOBID şu şekilde olacaktır `job_1415651640909_0026`.

    İş tamamlandıktan durumu döndürülür `SUCCEEDED`.

   > [!NOTE]
   > Bu Curl isteği bir JSON belgesi işle ilgili bilgilerle döndürür. Jq yalnızca durum değeri almak için kullanılır.

4. İş durumunu değiştiği için `SUCCEEDED`, Azure Blob depolama alanından iş sonuçlarını alabilirsiniz. `statusdir` Sorguyla geçirilen parametre çıkış dosyasının konumunu içerir. Bu örnekte, konumdur `/example/curl`. Bu adres iş çıktısı adresinde kümeleri varsayılan depolama depolar `/example/curl`.

Liste ve kullanarak bu dosyaları indirmek [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli). Azure CLI bloblarından ile çalışma hakkında daha fazla bilgi için bkz: [Azure Storage ile Azure CLI 2.0 kullanan](../../storage/common/storage-azure-cli.md#create-and-manage-blobs) belge.

## <a id="nextsteps"></a>Sonraki adımlar

Hdınsight'ta MapReduce işleri hakkında genel bilgi için:

* [Hdınsight'ta Hadoop ile MapReduce kullanma](hdinsight-use-mapreduce.md)

Diğer yolları hakkında bilgi için hdınsight'ta Hadoop ile çalışabilirsiniz:

* [Hdınsight'ta Hadoop ile Hive kullanma](hdinsight-use-hive.md)
* [Hdınsight'ta Hadoop ile pig kullanma](hdinsight-use-pig.md)

Bu makalede kullanılan REST arabirimi hakkında daha fazla bilgi için bkz: [WebHCat başvuru](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).