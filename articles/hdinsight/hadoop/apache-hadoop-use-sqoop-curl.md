---
title: Curl hdınsight'ta - Azure ile Hadoop Sqoop kullanma | Microsoft Docs
description: Sqoop Curl kullanarak hdınsight'a uzaktan göndermek öğrenin.
services: hdinsight
documentationcenter: ''
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 39798321-78ca-428c-bcfe-322e49af4059
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: jgao
ms.openlocfilehash: a83b87f1ed052c6d21d337eb37bc560efbf118ba
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34202154"
---
# <a name="run-sqoop-jobs-with-hadoop-in-hdinsight-with-curl"></a>Curl ile hdınsight'ta Hadoop ile Sqoop işleri çalıştırma
[!INCLUDE [sqoop-selector](../../../includes/hdinsight-selector-use-sqoop.md)]

Hdınsight Hadoop kümesinde Sqoop işlerini çalıştırmak için Curl kullanmayı öğrenin.

Curl çalıştırmak, izlemek ve Sqoop işleri sonuçları almak için ham HTTP isteklerini kullanarak Hdınsight ile nasıl etkileşim kurabileceğine göstermek için kullanılır. Bu, WebHCat REST (önceki adıyla Templeton da bilinir), Hdınsight küme tarafından sağlanan API'sini kullanarak çalışır.

## <a name="prerequisites"></a>Önkoşullar
Bu makaledeki adımları tamamlamak için aşağıdakiler gerekir:

* Tam [hdınsight'ta Hadoop ile Sqoop kullanma](hdinsight-use-sqoop.md#create-cluster-and-sql-database) bir Hdınsight kümesi ve bir Azure SQL veritabanı bir ortamda yapılandırmak için.
* [Curl](http://curl.haxx.se/). Curl ya da bir Hdınsight kümesine veri aktarmak için bir araçtır.
* [jq](http://stedolan.github.io/jq/). Jq yardımcı programı, REST isteklerinden döndürülen JSON verileri işlemek için kullanılır.

## <a name="submit-sqoop-jobs-by-using-curl"></a>Curl kullanarak Sqoop işleri gönderme
> [!NOTE]
> Curl’ü veya WebHCat ile başka bir REST iletişimini kullanırken HDInsight küme yöneticisinin kullanıcı adı ve parolasını sağlayarak isteklerin kimliğini doğrulamanız gerekir. Ayrıca, sunucuya istek göndermek için kullanılan Tekdüzen Kaynak Tanımlayıcısı’nın (URI) bir parçası olarak küme adını kullanmanız gerekir.
> 
> Bu bölümdeki komutlar için **USERNAME** ifadesini küme kimliğini doğrulayacak kullanıcı ile, **PASSWORD** ifadesini ise kullanıcı hesabının parolası ile değiştirin. **CLUSTERNAME** değerini kümenizin adıyla değiştirin.
> 
> REST API’sinin güvenliği [temel kimlik doğrulaması](http://en.wikipedia.org/wiki/Basic_access_authentication) ile sağlanır. Kimlik bilgilerinizin sunucuya güvenli bir şekilde gönderilmesi için istekleri her zaman Güvenli HTTP (HTTPS) kullanarak yapmanız gerekir.
> 
> 

1. HDInsight kümenize bağlanabildiğinizi doğrulamak için bir komut satırında aşağıdaki komutu kullanın:

    ```bash   
    curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    Aşağıdakine benzer bir yanıt almanız gerekir:

    ```json   
    {"status":"ok","version":"v1"}
    ```
   
    Bu komutta kullanılan parametreler aşağıdaki gibidir:
   
   * **-u** - İstek kimliğini doğrulamak için kullanılan kullanıcı adı ve parola.
   * **-G** - Bunun bir GET isteği olduğunu belirtir.
     
     URL'nin başına **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, tüm istekler için aynıdır. Yol **/status**, sunucu için istek WebHCat (Templeton olarak da bilinir) durumuna döndürmek için olduğunu belirtir. 
2. Sqoop işi göndermek için aşağıdakileri kullanın:

    ```bash
    curl -u USERNAME:PASSWORD -d user.name=USERNAME -d command="export --connect jdbc:sqlserver://SQLDATABASESERVERNAME.database.windows.net;user=USERNAME@SQLDATABASESERVERNAME;password=PASSWORD;database=SQLDATABASENAME --table log4jlogs --export-dir /example/data/sample.log --input-fields-terminated-by \0x20 -m 1" -d statusdir="wasb:///example/data/sqoop/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/sqoop
    ```

    Bu komutta kullanılan parametreler aşağıdaki gibidir:

    * **-d** - bu yana `-G` kullanılmaz, istek varsayılan olarak, POST yöntemi. `-d` istekle birlikte gönderilen veri değerleri belirtir.

        * **User.Name** -komutu çalıştıran kullanıcının.

        * **komut** -Sqoop komutun yürütülmesi için.

        * **statusdir** -bu iş için durumu yazılır dizin.

    Bu komut, iş durumunu denetlemek için kullanılan bir iş kimliği döndürür.

        ```json
        {"id":"job_1415651640909_0026"}
        ```

3. İş durumunu denetlemek için aşağıdaki komutu kullanın. Değiştir **JOBID** önceki adımda döndürülen değer. Örneğin, dönüş değeri `{"id":"job_1415651640909_0026"}`, ardından **JOBID** olacaktır `job_1415651640909_0026`.

    ```bash
    curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
    ```

    İş tamamlandı, durum olacaktır **başarılı**.
   
   > [!NOTE]
   > Bu Curl isteği iş hakkındaki bilgileri ile bir JavaScript nesne gösterimi (JSON) belgesini döndürür; jq yalnızca durum değeri almak için kullanılır.
   > 
   > 
4. İş durumu olarak değiştirildi sonra **başarılı**, Azure Blob depolama alanından iş sonuçlarını alabilirsiniz. `statusdir` Sorguyla geçirilen parametre içeren çıkış dosyasının; bu durumda, konumu **wasb: / / / örnek/data/sqoop/curl**. Bu adres işinde çıktısını depolar **örnek/data/sqoop/curl** Hdınsight küme tarafından kullanılan varsayılan depolama kapsayıcısı üzerinde dizin.
   
    Stderr ve stdout BLOB'lar erişmek için Azure portalını kullanabilirsiniz.  Log4jlogs tabloya karşıya verileri denetlemek için Microsoft SQL Server Management Studio'yu da kullanabilirsiniz.

## <a name="limitations"></a>Sınırlamalar
* Toplu export - ile Linux tabanlı Hdınsight, Microsoft SQL Server veya Azure SQL veritabanı için verileri dışa aktarmak için kullanılan Sqoop bağlayıcı toplu eklemeler şu anda desteklemiyor.
* -Kullanırken, Linux tabanlı Hdınsight ile toplu işleme `-batch` eklemeleri gerçekleştirirken geçiş, Sqoop, INSERT işlemlerine toplu işleme yerine birden çok eklemeleri gerçekleştirir.

## <a name="summary"></a>Özet
Bu belgede gösterildiği gibi çalıştırın, izlemek ve Hdınsight kümenize Sqoop işleri sonuçlarını görüntülemek için ham bir HTTP isteği'ni kullanabilirsiniz.

Bu makalede kullanılan REST arabirimi hakkında daha fazla bilgi için bkz: <a href="https://sqoop.apache.org/docs/1.99.3/RESTAPI.html" target="_blank">Sqoop REST API Kılavuzu</a>.

## <a name="next-steps"></a>Sonraki adımlar
Hdınsight ile Hive hakkında genel bilgi için:

* [Hdınsight'ta Hadoop ile Sqoop kullanma](hdinsight-use-sqoop.md)

Diğer yöntemler hakkında bilgi için hdınsight'ta Hadoop ile çalışabilirsiniz:

* [Hdınsight'ta Hadoop ile Hive kullanma](hdinsight-use-hive.md)
* [Hdınsight'ta Hadoop ile pig kullanma](hdinsight-use-pig.md)
* [Hdınsight'ta Hadoop ile MapReduce kullanma](hdinsight-use-mapreduce.md)

Diğer Hdınsight için ilgili makaleler curl:
 
* [Azure REST API'sini kullanarak Hadoop kümeleri oluşturma](../hdinsight-hadoop-create-linux-clusters-curl-rest.md)
* [REST kullanarak hdınsight'ta Hadoop ile Hive sorguları çalıştırma](apache-hadoop-use-hive-curl.md)
* [REST kullanarak Hdınsight'ta Hadoop ile MapReduce işleri çalıştırma](apache-hadoop-use-mapreduce-curl.md)
* [CURL kullanarak Hdınsight'ta Hadoop ile pig işleri çalıştırma](apache-hadoop-use-pig-curl.md)



