---
title: HDInsight - Azure içinde Curl ile Apache Sqoop'u kullanma
description: Curl kullanarak HDInsight için Apache Sqoop'u işleri uzaktan göndermek öğrenin.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: hrasheed
ms.openlocfilehash: ad716e2ef5e597424c860378e7a63d5c2de53f54
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2019
ms.locfileid: "57834566"
---
# <a name="run-apache-sqoop-jobs-with-hadoop-in-hdinsight-with-curl"></a>Curl ile HDInsight Hadoop ile Apache Sqoop işleri çalıştırma
[!INCLUDE [sqoop-selector](../../../includes/hdinsight-selector-use-sqoop.md)]

Bir Apache Hadoop kümesi HDInsight üzerinde Apache Sqoop işlerini çalıştırmak için Curl kullanmayı öğrenin.

Curl, çalıştırma, izleme ve Sqoop iş sonuçlarını almak üzere ham HTTP isteklerini kullanarak HDInsight ile nasıl etkileşim kurabileceğine göstermek için kullanılır. Bu, WebHCat REST (eski adıyla templeton da bilinir) HDInsight kümeniz tarafından sağlanan API kullanarak çalışır.

## <a name="prerequisites"></a>Önkoşullar
Bu makaledeki adımları tamamlamak için aşağıdakiler gerekir:


* Tam [HDInsight, Hadoop ile Apache Sqoop'u kullanma](hdinsight-use-sqoop.md#create-cluster-and-sql-database) bir HDInsight kümesi ve bir Azure SQL veritabanı ile bir ortamı yapılandırmak için.
* [Curl](https://curl.haxx.se/). Curl, bir HDInsight kümesine ya da veri aktarımı için kullanılan bir araçtır.
* [jq](https://stedolan.github.io/jq/). Jq yardımcı programı, REST isteklerinden döndürülen JSON verilerini işlemek için kullanılır.


## <a name="submit-apache-sqoop-jobs-by-using-curl"></a>Curl kullanarak Apache Sqoop işlerini gönderme
> [!NOTE]  
> Curl’ü veya WebHCat ile başka bir REST iletişimini kullanırken HDInsight küme yöneticisinin kullanıcı adı ve parolasını sağlayarak isteklerin kimliğini doğrulamanız gerekir. Ayrıca, sunucuya istek göndermek için kullanılan Tekdüzen Kaynak Tanımlayıcısı’nın (URI) bir parçası olarak küme adını kullanmanız gerekir.
> 
> Bu bölümdeki komutlar için **USERNAME** ifadesini küme kimliğini doğrulayacak kullanıcı ile, **PASSWORD** ifadesini ise kullanıcı hesabının parolası ile değiştirin. **CLUSTERNAME** değerini kümenizin adıyla değiştirin.
> 
> REST API’sinin güvenliği [temel kimlik doğrulaması](https://en.wikipedia.org/wiki/Basic_access_authentication) ile sağlanır. Kimlik bilgilerinizin sunucuya güvenli bir şekilde gönderilmesi için istekleri her zaman Güvenli HTTP (HTTPS) kullanarak yapmanız gerekir.
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
     
     URL'nin başına **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, tüm istekler için aynıdır. Yol **/status**, istek WebHCat (Ayrıca templeton olarak da bilinir) durumuna döndürmek için sunucu gösterir. 
2. Sqoop işi göndermek için aşağıdakileri kullanın:

    ```bash
    curl -u USERNAME:PASSWORD -d user.name=USERNAME -d command="export --connect jdbc:sqlserver://SQLDATABASESERVERNAME.database.windows.net;user=USERNAME@SQLDATABASESERVERNAME;password=PASSWORD;database=SQLDATABASENAME --table log4jlogs --export-dir /example/data/sample.log --input-fields-terminated-by \0x20 -m 1" -d statusdir="wasb:///example/data/sqoop/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/sqoop
    ```

    Bu komutta kullanılan parametreler aşağıdaki gibidir:

   * **-d** - beri `-G` kullanılmıyorsa, varsayılan POST yöntemine istek. `-d` istekle beraber gönderilen veri değerleri belirtir.

       * **User.Name** -komutu çalıştıran kullanıcı.

       * **komut** -çalıştırılacak Sqoop komutu.

       * **statusdir** -bu görev için durum yazılacağı dizin.

     Bu komut, iş durumunu denetlemek için kullanılan bir iş kimliği döndürür.

       ```json
       {"id":"job_1415651640909_0026"}
       ```

3. İşin durumunu denetlemek için aşağıdaki komutu kullanın. Değiştirin **JOBID** ile önceki adımda döndürülen değer. Örneğin, dönüş değeri `{"id":"job_1415651640909_0026"}`, ardından **JOBID** olacaktır `job_1415651640909_0026`.

    ```bash
    curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
    ```

    İş tamamlandı, durumu olacaktır **başarılı**.
   
   > [!NOTE]  
   > Bu Curl isteği işle ilgili bilgileri içeren bir JavaScript nesne gösterimi (JSON) belge döndürür; jq, yalnızca durum değeri almak için kullanılır.

4. İş durumu için değiştiğinde **başarılı**, Azure Blob depolama alanından iş sonuçlarını alabilirsiniz. `statusdir` Sorguyla geçirilen parametre içerir; bu durumda, çıkış dosyasının konumu **wasb: / / / örnek/data/sqoop/curl**. Bu adres iş çıktısı depolar **örnek/data/sqoop/curl** HDInsight kümeniz tarafından kullanılan varsayılan depolama kapsayıcısı üzerinde dizin.
   
    Stderr ve stdout bloblara erişmek için Azure portalını kullanabilirsiniz.  Microsoft SQL Server Management Studio log4jlogs tabloya karşıya yüklenen verileri denetlemek için de kullanabilirsiniz.

## <a name="limitations"></a>Sınırlamalar
* Toplu Dışarı Aktar - ile Linux tabanlı HDInsight, Microsoft SQL Server veya Azure SQL veritabanı için verileri dışarı aktarmak için kullanılan Sqoop bağlayıcısının toplu ekleme şu anda desteklemiyor.
* -Kullanırken, Linux tabanlı HDInsight ile toplu işleme `-batch` ekler gerçekleştirirken geçiş, Sqoop, birden çok eklemeleri toplu INSERT işlemler yerine gerçekleştirir.

## <a name="summary"></a>Özet
Bu belgede gösterildiği gibi ham bir HTTP isteği çalıştırma, izleme ve HDInsight kümenizi Sqoop işleri sonuçlarını görüntülemek için kullanabilirsiniz.

Bu makalede kullanılan REST arabirimi hakkında daha fazla bilgi için bkz. <a href="https://sqoop.apache.org/docs/1.99.3/RESTAPI.html" target="_blank">Apache Sqoop REST API Kılavuzu</a>.

## <a name="next-steps"></a>Sonraki adımlar
HDInsight ile Hive hakkında genel bilgi için:

* [HDInsight üzerinde Apache Hadoop ile Apache Sqoop'u kullanma](hdinsight-use-sqoop.md)

Diğer yollar hakkında daha fazla bilgi için HDInsight üzerinde Hadoop ile çalışabilirsiniz:

* [HDInsight üzerinde Apache Hadoop ile Apache Hive'ı kullanma](hdinsight-use-hive.md)
* [HDInsight üzerinde Apache Hadoop ile Apache Pig kullanma](hdinsight-use-pig.md)
* [HDInsight üzerinde Apache Hadoop ile MapReduce kullanma](hdinsight-use-mapreduce.md)

İlgili makaleler için diğer HDInsight curl:
 
* [Azure REST API'sini kullanarak Apache Hadoop kümeleri oluşturma](../hdinsight-hadoop-create-linux-clusters-curl-rest.md)
* [REST kullanarak HDInsight, Apache Hadoop ile Apache Hive sorguları çalıştırma](apache-hadoop-use-hive-curl.md)
* [REST kullanarak HDInsight üzerinde Apache Hadoop MapReduce işlerle çalışma](apache-hadoop-use-mapreduce-curl.md)
* [Apache Pig işleri cURL kullanarak HDInsight üzerinde Apache Hadoop ile çalıştırın.](apache-hadoop-use-pig-curl.md)



