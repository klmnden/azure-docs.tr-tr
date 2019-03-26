---
title: Apache Hadoop Hive ile HDInsight - Azure içinde Curl kullanma
description: Curl kullanarak HDInsight için Apache Pig işleri uzaktan göndermek öğrenin.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/23/2018
ms.author: hrasheed
ms.openlocfilehash: 00fd697b42c7d93cb04392e91deea23133cf398a
ms.sourcegitcommit: 280d9348b53b16e068cf8615a15b958fccad366a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58407248"
---
# <a name="run-apache-hive-queries-with-apache-hadoop-in-hdinsight-using-rest"></a>REST kullanarak HDInsight, Apache Hadoop ile Apache Hive sorguları çalıştırma

[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

Azure HDInsight kümesinde Apache Hadoop ile Apache Hive sorguları çalıştırma için WebHCat REST API'sini kullanmayı öğrenin.

## <a name="prerequisites"></a>Önkoşullar

* Linux tabanlı Hadoop HDInsight kümesi sürüm 3.4 üzerindeki.

  > [!IMPORTANT]  
  > Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Bir REST istemcisi. Bu belge Windows PowerShell'i kullanır ve [Curl](https://curl.haxx.se/) örnekler.

    > [!NOTE]  
    > Azure PowerShell, HDInsight üzerinde Hive ile çalışmak için adanmış cmdlet'leri sağlar. Daha fazla bilgi için [Azure PowerShell ile Hive kullanma Apache](apache-hadoop-use-hive-powershell.md) belge.

Bu belge de Windows PowerShell kullanır ve [Jq](https://stedolan.github.io/jq/) JSON verilerini REST isteklerinden döndürülen işlem.

## <a id="curl"></a>Bir Hive sorgusu çalıştırma

> [!NOTE]  
> WebHCat ile cURL veya başka bir REST iletişimini kullanırken HDInsight küme yöneticisinin kullanıcı adını ve parolasını sağlayarak isteklerin kimliğini doğrulaması gerekir.
>
> REST API’sinin güvenliği [temel kimlik doğrulaması](https://en.wikipedia.org/wiki/Basic_access_authentication) ile sağlanır. Kimlik bilgilerinizin sunucuya güvenli bir şekilde gönderilir emin olmak için her zaman güvenli HTTP (HTTPS) kullanarak istekleri olun.

1. Bu belgedeki betikler tarafından kullanılan Küme oturum açma ayarlamak için aşağıdaki komutlardan birini kullanın:

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

3. HDInsight kümenize bağlanabildiğinizi doğrulamak için aşağıdaki komutlardan birini kullanın:

    ```bash
    curl -u $LOGIN -G https://$CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```
    
    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/templeton/v1/status" `
       -Credential $creds `
       -UseBasicParsing
    $resp.Content
    ```

    Aşağıdaki metne benzer bir yanıt alırsınız:

    ```json
    {"status":"ok","version":"v1"}
    ```

    Bu komutta kullanılan parametreler aşağıdaki gibidir:

    * `-u` -Kullanıcı adı ve istek kimliğini doğrulamak için kullanılan parola.
    * `-G` -Bu isteği bir alma işlemi olduğunu gösterir.

   URL'nin başına `https://$CLUSTERNAME.azurehdinsight.net/templeton/v1`, tüm istekler için aynıdır. Yol `/status`, istek WebHCat (Ayrıca templeton olarak da bilinir) durumuna döndürmek için sunucu gösterir. Ayrıca, aşağıdaki komutu kullanarak Hive sürümünü isteyebilirsiniz:

    ```bash
    curl -u $LOGIN -G https://$CLUSTERNAME.azurehdinsight.net/templeton/v1/version/hive
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/templeton/v1/version/hive" `
       -Credential $creds `
       -UseBasicParsing
    $resp.Content
    ```

    Bu istek, aşağıdaki metne benzer bir yanıt döndürür:

    ```json
        {"module":"hive","version":"0.13.0.2.1.6.0-2103"}
    ```

4. Adlı bir tablo oluşturmak için aşağıdakileri kullanın **log4jLogs**:

    ```bash
    JOBID=`curl -s -u $LOGIN -d user.name=$LOGIN -d execute="set+hive.execution.engine=tez;DROP+TABLE+log4jLogs;CREATE+EXTERNAL+TABLE+log4jLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+ROW+FORMAT+DELIMITED+FIELDS+TERMINATED+BY+' '+STORED+AS+TEXTFILE+LOCATION+'/example/data/';SELECT+t4+AS+sev,COUNT(*)+AS+count+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log'+GROUP+BY+t4;" -d statusdir="/example/rest" https://$CLUSTERNAME.azurehdinsight.net/templeton/v1/hive | jq .id`
    echo $JOBID
    ```

    ```powershell
    $reqParams = @{"user.name"="admin";"execute"="set hive.execution.engine=tez;DROP TABLE log4jLogs;CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED BY ' ' STORED AS TEXTFILE LOCATION '/example/data/;SELECT t4 AS sev,COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' GROUP BY t4;";"statusdir"="/example/rest"}
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/templeton/v1/hive" `
       -Credential $creds `
       -Body $reqParams `
       -Method POST `
       -UseBasicParsing
    $jobID = (ConvertFrom-Json $resp.Content).id
    $jobID
    ```

    Bu istek verileri isteğin parçası olarak REST API'sine gönderir. POST yöntemini kullanır. Aşağıdaki veri değerleri ile istek gönderilir:

     * `user.name` -Komutu çalıştırmadan kullanıcı.
     * `execute` -Yürütülecek HiveQL ifadelerini.
     * `statusdir` -Bu görev için durum yazılan dizin.

   Bu deyimler, aşağıdaki eylemleri gerçekleştirin:
   
   * `DROP TABLE` -Tablo zaten var, silinir.
   * `CREATE EXTERNAL TABLE` -Hive 'dış' yeni bir tablo oluşturur. Dış tablolar yalnızca tablo tanımı kovanında depolayın. Verileri özgün konumunda bırakılır.

     > [!NOTE]  
     > Dış tablolar, temel alınan veriler dış bir kaynak tarafından güncelleştirilmesi beklediğiniz kullanılmalıdır. Örneğin, bir otomatik veri karşıya yükleme işlemi veya başka bir MapReduce işlem.
     >
     > Bir dış tablo bırakılırken mu **değil** verileri, yalnızca tablo tanımını silin.

   * `ROW FORMAT` -Nasıl veri biçimlendirilir. Her günlük alanlar boşlukla ayrılır.
   * `STORED AS TEXTFILE LOCATION` -Verilerin depolandığı (örnek/veri dizini) ve metin olarak depolanır.
   * `SELECT` -Tüm satırların sayımını seçer Burada sütun **t4** değeri içeren **[Hata]**. Bu bildirimi bir değeri döndürür **3** olarak bu değeri içeren üç satır.

     > [!NOTE]  
     > HiveQL ifadelerini arasındaki boşluklar değiştirilir bildirimi `+` Curl ile kullanılan karakter. Sınırlayıcı gibi bir alanı içeren tırnak içine alınmış değerler değil yerine `+`.

      Bu komut, iş durumunu denetlemek için kullanılan bir iş kimliği döndürür.

5. İşin durumunu denetlemek için aşağıdaki komutu kullanın:

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

    İş bitip bitmediğini durumudur **başarılı**.

6. İş durumu için değiştiğinde **başarılı**, Azure Blob depolama alanından iş sonuçlarını alabilirsiniz. `statusdir` Sorguyla geçirilen parametre içerir; bu durumda, çıkış dosyasının konumu `/example/rest`. Bu adres çıktısında depolar `example/curl` kümeleri varsayılan depolama alanı içindeki dizin.

    Liste ve kullanarak bu dosyaları indirmek [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli). Azure depolama ile Azure CLI kullanma ile ilgili daha fazla bilgi için bkz: [kullanımı Azure CLI ile Azure depolama](https://docs.microsoft.com/azure/storage/storage-azure-cli#create-and-manage-blobs) belge.

## <a id="nextsteps"></a>Sonraki adımlar

HDInsight ile Hive hakkında genel bilgi için:

* [HDInsight üzerinde Apache Hadoop ile Apache Hive'ı kullanma](hdinsight-use-hive.md)

Diğer yollar hakkında daha fazla bilgi için HDInsight üzerinde Hadoop ile çalışabilirsiniz:

* [HDInsight üzerinde Apache Hadoop ile Apache Pig kullanma](hdinsight-use-pig.md)
* [HDInsight üzerinde Apache Hadoop ile MapReduce kullanma](hdinsight-use-mapreduce.md)

Tez Hive'ı kullanıyorsanız, hata ayıklama bilgileri için aşağıdaki belgelere bakın:

* [Linux tabanlı HDInsight üzerinde Apache Ambari Tez görünümünü kullanın](../hdinsight-debug-ambari-tez-view.md)

Bu belgede kullanılan REST API hakkında daha fazla bilgi için bkz. [WebHCat başvuru](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference) belge.

[azure-purchase-options]: https://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: https://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: https://azure.microsoft.com/pricing/free-trial/

[apache-tez]: https://tez.apache.org
[apache-hive]: https://hive.apache.org/
[apache-log4j]: https://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: https://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md




[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]:submit-apache-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[powershell-here-strings]: https://technet.microsoft.com/library/ee692792.aspx


