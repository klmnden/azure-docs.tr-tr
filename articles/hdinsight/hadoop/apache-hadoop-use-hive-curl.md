---
title: Curl hdınsight'ta - Azure ile Hadoop Hive kullanma | Microsoft Docs
description: Curl kullanarak hdınsight'a Pig işleri uzaktan göndermek öğrenin.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 6ce18163-63b5-4df6-9bb6-8fcbd4db05d8
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 04/23/2018
ms.author: larryfr
ms.openlocfilehash: f602cf45165625ec252f2e29cb9b0e5ed878f3a8
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="run-hive-queries-with-hadoop-in-hdinsight-using-rest"></a>REST kullanarak hdınsight'ta Hadoop ile Hive sorguları çalıştırma

[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

Azure Hdınsight kümesinde Hadoop ile Hive sorguları çalıştırmak için WebHCat REST API kullanmayı öğrenin.

## <a name="prerequisites"></a>Önkoşullar

* Bir Linux tabanlı Hadoop Hdınsight kümesi sürüm 3.4 veya daha büyük.

  > [!IMPORTANT]
  > Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

* REST istemcisi. Bu belge Windows PowerShell kullanır ve [Curl](http://curl.haxx.se/) örnekler.

    > [!NOTE]
    > Azure PowerShell hdınsight'ta Hive ile çalışmak için adanmış cmdlet'leri sağlar. Daha fazla bilgi için bkz: [Azure PowerShell ile Hive kullanma](apache-hadoop-use-hive-powershell.md) belge.

Bu belgede ayrıca Windows PowerShell'i kullanır ve [Jq](http://stedolan.github.io/jq/) JSON verilerini döndürülen gelen REST istekleri işleyemedi.

## <a id="curl"></a>Hive sorgusu çalıştırma

> [!NOTE]
> WebHCat ile cURL veya başka bir REST iletişimini kullanırken Hdınsight küme yöneticisinin kullanıcı adını ve parolasını sağlayarak isteklerin kimliğini doğrulaması gerekir.
>
> REST API’sinin güvenliği [temel kimlik doğrulaması](http://en.wikipedia.org/wiki/Basic_access_authentication) ile sağlanır. Kimlik bilgilerinizin sunucuya güvenli bir şekilde gönderilir sağlamaya yardımcı olmak için her zaman güvenli HTTP (HTTPS) kullanarak istekleri yapın.

1. Bu belgedeki komut dosyaları tarafından kullanılan Küme oturum açma ayarlamak için aşağıdaki komutlardan birini kullanın:

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

3. Hdınsight kümenize bağlanabilirsiniz doğrulamak için aşağıdaki komutlardan birini kullanın:

    ```bash
    curl -u $LOGIN -G https://$CLUSTERNAME.azurehdinsight.net/templeton/v1/status)
    ```
    
    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/templeton/v1/status" `
       -Credential $creds `
       -UseBasicParsing
    $resp.Content
    ```

    Aşağıdakine benzer bir yanıt alırsınız:

    ```json
    {"status":"ok","version":"v1"}
    ```

    Bu komutta kullanılan parametreler aşağıdaki gibidir:

    * `-u` -Kullanıcı adı ve istek kimliğini doğrulamak için kullanılan parola.
    * `-G` -Bu isteği alma işlemi olduğunu gösterir.

   URL'nin başına `https://$CLUSTERNAME.azurehdinsight.net/templeton/v1`, tüm istekler için aynıdır. Yol `/status`, sunucu için istek WebHCat (Templeton olarak da bilinir) durumuna döndürmek için olduğunu belirtir. Ayrıca, aşağıdaki komutu kullanarak Hive sürümü isteyebilirsiniz:

    ```bash
    curl -u $LOGIN -G https://$CLUSTERNAME.azurehdinsight.net/templeton/v1/version/hive
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/templeton/v1/version/hive" `
       -Credential $creds `
       -UseBasicParsing
    $resp.Content
    ```

    Bu istek aşağıdaki metni benzer bir yanıt döndürür:

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

    Bu istek için REST API isteğinin bir parçası verileri gönderir POST yöntemini kullanır. Aşağıdaki veri değerleri istekle gönderilir:

     * `user.name` -Komutu çalıştırdıktan kullanıcı.
     * `execute` -Yürütülecek HiveQL ifadelerini.
     * `statusdir` -Bu iş için durumu yazılır dizin.

   Bu ifadeler aşağıdaki eylemleri gerçekleştirin:
   
   * `DROP TABLE` -Tablosu zaten varsa, bu silinir.
   * `CREATE EXTERNAL TABLE` -Kovanında yeni bir 'external' tablo oluşturur. Dış tablolara yalnızca tablo tanımı kovanında depolar. Veriler özgün konumda kalır.

     > [!NOTE]
     > Dış kaynak tarafından güncelleştirilecek temel alınan veri beklediğiniz dış tablolara kullanılmalıdır. Örneğin, bir otomatik veri karşıya yükleme işlemi veya başka bir MapReduce işlemi.
     >
     > Bir dış tablo bırakma mu **değil** verileri, yalnızca tablo tanımını silin.

   * `ROW FORMAT` -Nasıl veri biçimlendirilir. Her günlüğüne alanlar boşlukla ayrılır.
   * `STORED AS TEXTFILE LOCATION` -Verilerin depolandığı (örneğin/veri dizini) ve metin olarak depolanır.
   * `SELECT` -Tüm satırların sayımını seçer Burada sütun **t4** değeri içeren **[Hata]**. Bu ifade değerini döndürür **3** bu değeri içeren üç satır olarak.

     > [!NOTE]
     > HiveQL ifadelerini arasında boşluk değiştirilir bildirimi `+` karakter Curl ile kullanıldığında. Sınırlayıcı gibi bir alanı içeren tırnak içine alınmış değerler değiştirilmemişse tarafından `+`.

      Bu komut, iş durumunu denetlemek için kullanılan bir iş kimliği döndürür.

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

    İş tamamlandı durumu varsa, **başarılı**.

6. İş durumu olarak değiştirildi sonra **başarılı**, Azure Blob depolama alanından iş sonuçlarını alabilirsiniz. `statusdir` Sorguyla geçirilen parametre içeren çıkış dosyasının; bu durumda, konumu `/example/rest`. Bu adres çıktıda depolar `example/curl` kümeleri varsayılan depolama birimindeki dizin.

    Liste ve kullanarak bu dosyaları indirmek [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli). Azure Storage ile Azure CLI kullanma ile ilgili daha fazla bilgi için bkz: [Azure Storage ile Azure CLI 2.0 Kullan](https://docs.microsoft.com/azure/storage/storage-azure-cli#create-and-manage-blobs) belge.

## <a id="nextsteps"></a>Sonraki adımlar

Hdınsight ile Hive hakkında genel bilgi için:

* [Hdınsight'ta Hadoop ile Hive kullanma](hdinsight-use-hive.md)

Diğer yöntemler hakkında bilgi için hdınsight'ta Hadoop ile çalışabilirsiniz:

* [Hdınsight'ta Hadoop ile pig kullanma](hdinsight-use-pig.md)
* [Hdınsight'ta Hadoop ile MapReduce kullanma](hdinsight-use-mapreduce.md)

Tez Hive ile kullanıyorsanız, hata ayıklama bilgileri için aşağıdaki belgelere bakın:

* [Linux tabanlı Hdınsight üzerinde Ambari Tez görünümünü kullanın](../hdinsight-debug-ambari-tez-view.md)

Bu belgede kullanılan REST API hakkında daha fazla bilgi için bkz: [WebHCat başvuru](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference) belge.

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md




[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]:submit-apache-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


