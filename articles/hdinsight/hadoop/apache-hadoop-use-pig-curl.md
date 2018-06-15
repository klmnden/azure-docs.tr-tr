---
title: Hadoop Pig hdınsight'ta - Azure REST ile kullanma | Microsoft Docs
description: Azure Hdınsight Hadoop kümesinde Pig Latin işlerini çalıştırmak için REST kullanmayı öğrenin.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.assetid: ed5e10d1-4f47-459c-a0d6-7ff967b468c4
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/10/2018
ms.author: larryfr
ms.openlocfilehash: 4883794261116abf4925e7e4e9a8df14626c7a71
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31403517"
---
# <a name="run-pig-jobs-with-hadoop-on-hdinsight-by-using-rest"></a>Pig işleri, REST kullanarak Hdınsight'ta Hadoop ile çalıştırın

[!INCLUDE [pig-selector](../../../includes/hdinsight-selector-use-pig.md)]

Azure Hdınsight kümesi için REST istekleri yaparak pig Latin işleri çalıştırmayı öğrenin. Curl WebHCat REST API kullanarak Hdınsight ile nasıl etkileşim göstermek için kullanılır.

> [!NOTE]
> Zaten Linux tabanlı Hadoop sunucuları kullandıysanız, ancak yeni Hdınsight için, bkz: [Linux tabanlı Hdınsight ipuçları](../hdinsight-hadoop-linux-information.md).

## <a id="prereq"></a>Önkoşullar

* Azure Hdınsight (Hadoop hdınsight) kümesi (Linux tabanlı veya Windows tabanlı)

  > [!IMPORTANT]
  > Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

* [Curl](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

## <a id="curl"></a>Curl kullanarak pig işleri çalıştırma

> [!NOTE]
> REST API aracılığıyla güvenli [temel erişimi kimlik doğrulaması](http://en.wikipedia.org/wiki/Basic_access_authentication). Her zaman istekleri kimlik bilgilerinizin sunucuya güvenli bir şekilde gönderildiğinden emin olmak için Güvenli HTTP (HTTPS) kullanarak yapın.
>
> Bu bölümdeki komutlar kullanırken, Değiştir `USERNAME` küme kimliğini ve değiştirmek için kullanıcıyla `PASSWORD` kullanıcı hesabı için parola ile. `CLUSTERNAME` değerini kümenizin adıyla değiştirin.
>


1. HDInsight kümenize bağlanabildiğinizi doğrulamak için bir komut satırında aşağıdaki komutu kullanın:

    ```bash
    curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    Aşağıdaki JSON yanıt almanız gerekir:

        {"status":"ok","version":"v1"}

    Bu komutta kullanılan parametreler aşağıdaki gibidir:

    * **-u**: kullanıcı adı ve istek kimliğini doğrulamak için kullanılan parola
    * **-G**: Bu isteği bir GET isteği olduğunu gösterir

     URL'nin başına **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, tüm istekler için aynıdır. Yol **/status**, sunucu için istek WebHCat (Templeton olarak da bilinir) durumuna döndürmek için olduğunu belirtir.

2. Küme için Pig Latin işi göndermek için aşağıdaki kodu kullanın:

    ```bash
    curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="LOGS=LOAD+'/example/data/sample.log';LEVELS=foreach+LOGS+generate+REGEX_EXTRACT($0,'(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)',1)+as+LOGLEVEL;FILTEREDLEVELS=FILTER+LEVELS+by+LOGLEVEL+is+not+null;GROUPEDLEVELS=GROUP+FILTEREDLEVELS+by+LOGLEVEL;FREQUENCIES=foreach+GROUPEDLEVELS+generate+group+as+LOGLEVEL,COUNT(FILTEREDLEVELS.LOGLEVEL)+as+count;RESULT=order+FREQUENCIES+by+COUNT+desc;DUMP+RESULT;" -d statusdir="/example/pigcurl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/pig
    ```

    Bu komutta kullanılan parametreler aşağıdaki gibidir:

    * **-d**: çünkü `-G` kullanılmaz, istek varsayılan olarak, POST yöntemi. `-d` istekle birlikte gönderilen veri değerleri belirtir.

    * **User.Name**: komutu çalıştıran kullanıcının
    * **yürütme**: yürütmek için Pig Latin deyimleri
    * **statusdir**: Bu iş için durumu yazılır dizini

    > [!NOTE]
    > Pig Latin deyimlerinde alanları değiştirilir bildirimi `+` karakter Curl ile kullanıldığında.

    Bu komut, örneğin, iş durumunu denetlemek için kullanılan bir iş kimliği döndürmesi gerekir:

        {"id":"job_1415651640909_0026"}

3. İş durumunu denetlemek için aşağıdaki komutu kullanın.

     ```bash
    curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
    ```

     Değiştir `JOBID` önceki adımda döndürülen değer. Örneğin, dönüş değeri `{"id":"job_1415651640909_0026"}`, ardından `JOBID` olan `job_1415651640909_0026`.

    İş tamamlandı durumu varsa, **başarılı**.

    > [!NOTE]
    > Bu Curl istek JavaScript nesne gösterimi (JSON) belge ile iş hakkındaki bilgileri döndürür ve jq yalnızca durum değeri almak için kullanılır.

## <a id="results"></a>Sonuçları Görüntüle

İş durumunu değiştiği için **başarılı**, iş sonuçları alabilirsiniz. `statusdir` Sorguyla geçirilen parametre içeren çıkış dosyasının; bu durumda, konumu `/example/pigcurl`.

Hdınsight Azure Storage veya Azure Data Lake Store varsayılan veri deposu olarak kullanabilirsiniz. Hangisinin bağlı olarak, kullandığınız veri almanın çeşitli yolları vardır. Daha fazla bilgi için depolama bölümüne bakın [Linux tabanlı Hdınsight bilgi](../hdinsight-hadoop-linux-information.md#hdfs-azure-storage-and-data-lake-store) belge.

## <a id="summary"></a>Özet

Bu belgede gösterildiği gibi çalıştırın, izlemek ve Hdınsight kümenize Pig işleri sonuçlarını görüntülemek için ham bir HTTP isteği'ni kullanabilirsiniz.

Bu makalede kullanılan REST arabirimi hakkında daha fazla bilgi için bkz: [WebHCat başvuru](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).

## <a id="nextsteps"></a>Sonraki adımlar

Hdınsight Pig hakkında genel bilgi için:

* [Hdınsight'ta Hadoop ile pig kullanma](hdinsight-use-pig.md)

Diğer yolları hakkında bilgi için hdınsight'ta Hadoop ile çalışabilirsiniz:

* [Hdınsight'ta Hadoop ile Hive kullanma](hdinsight-use-hive.md)
* [Hdınsight'ta Hadoop ile MapReduce kullanma](hdinsight-use-mapreduce.md)
