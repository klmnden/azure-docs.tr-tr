---
title: Apache Hadoop Hive ile HDInsight - Azure içinde Curl kullanma
description: Curl kullanarak HDInsight için Apache Pig işleri uzaktan göndermek öğrenin.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: hrasheed
ms.openlocfilehash: 334d7b886aa4e2130a12f0c8a7919986fdac55d1
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67508129"
---
# <a name="run-apache-hive-queries-with-apache-hadoop-in-hdinsight-using-rest"></a>REST kullanarak HDInsight, Apache Hadoop ile Apache Hive sorguları çalıştırma

[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

Azure HDInsight kümesinde Apache Hadoop ile Apache Hive sorguları çalıştırma için WebHCat REST API'sini kullanmayı öğrenin.

## <a name="prerequisites"></a>Önkoşullar

* HDInsight üzerinde Apache Hadoop kümesi. Bkz: [Linux'ta HDInsight kullanmaya başlama](./apache-hadoop-linux-tutorial-get-started.md).

* Bir REST istemcisi. Bu belgeyi kullanan [Invoke-WebRequest](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/invoke-webrequest) Windows PowerShell ve [Curl](https://curl.haxx.se/) üzerinde [Bash](https://docs.microsoft.com/windows/wsl/install-win10).

* Bash kullanıyorsanız, aynı zamanda jq, bir komut satırı JSON işlemci gerekir.  Bkz: [ https://stedolan.github.io/jq/ ](https://stedolan.github.io/jq/).

## <a name="base-uri-for-rest-api"></a>Taban URI için Rest API

Temel Tekdüzen Kaynak Tanımlayıcısı (URI) HDInsight REST API için olan `https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME`burada `CLUSTERNAME` kümenizin adıdır.  Küme adları içinde bir URI'leri **büyük/küçük harfe**.  While küme adı tam etki alanı adı (FQDN) bölümünde URI'ın (`CLUSTERNAME.azurehdinsight.net`) duyarlı olan diğer örnekleri urı'sindeki büyük/küçük harfe duyarlıdır.

## <a name="authentication"></a>Kimlik Doğrulaması

WebHCat ile cURL veya başka bir REST iletişimini kullanırken HDInsight küme yöneticisinin kullanıcı adını ve parolasını sağlayarak isteklerin kimliğini doğrulaması gerekir. REST API’sinin güvenliği [temel kimlik doğrulaması](https://en.wikipedia.org/wiki/Basic_access_authentication) ile sağlanır. Kimlik bilgilerinizin sunucuya güvenli bir şekilde gönderilir emin olmak için her zaman güvenli HTTP (HTTPS) kullanarak istekleri olun.

### <a name="setup-preserve-credentials"></a>Kurulum (kimlik bilgileri Koru)
Her örnek için bunları yeniden girildi önlemek için kimlik bilgilerinizi korur.  Küme adı, ayrı bir adımla korunur.

**BİR. Bash**  
Değiştirerek aşağıdaki betiği düzenleyin `PASSWORD` gerçek parolanızla.  Ardından komutu girin.

```bash
export password='PASSWORD'
```  

**B. PowerShell** aşağıdaki kod yürütün ve açılır pencerede kimlik bilgilerinizi girin:

```powershell
$creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"
```

### <a name="identify-correctly-cased-cluster-name"></a>Büyük küçük harfleri doğru küme adı belirleyin
Küme adı gerçek büyük küçük harfleri, küme nasıl oluşturulduğuna bağlı olarak, beklenenden farklı olabilir.  Buradaki adımları gerçek büyük/küçük harf Göster ve sonraki tüm örnekleri için bir değişkende depolayın.

Değiştirmek için aşağıdaki betiklerini Düzenle `CLUSTERNAME` ile kümenizin adıdır. Ardından komutu girin. (Küme adı FQDN için büyük küçük harfe duyarlı değildir.)

```bash
export clusterName=$(curl -u admin:$password -sS -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters" | jq -r '.items[].Clusters.cluster_name')
echo $clusterName
```  

```powershell
# Identify properly cased cluster name
$resp = Invoke-WebRequest -Uri "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters" `
    -Credential $creds -UseBasicParsing
$clusterName = (ConvertFrom-Json $resp.Content).items.Clusters.cluster_name;

# Show cluster name
$clusterName
```

## <a id="curl"></a>Bir Hive sorgusu çalıştırma

1. HDInsight kümenize bağlanabildiğinizi doğrulamak için aşağıdaki komutlardan birini kullanın:

    ```bash
    curl -u admin:$password -G https://$clusterName.azurehdinsight.net/templeton/v1/status
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

1. URL'nin başına `https://$CLUSTERNAME.azurehdinsight.net/templeton/v1`, tüm istekler için aynıdır. Yol `/status`, istek WebHCat (Ayrıca templeton olarak da bilinir) durumuna döndürmek için sunucu gösterir. Ayrıca, aşağıdaki komutu kullanarak Hive sürümünü isteyebilirsiniz:

    ```bash
    curl -u admin:$password -G https://$clusterName.azurehdinsight.net/templeton/v1/version/hive
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/templeton/v1/version/hive" `
       -Credential $creds `
       -UseBasicParsing
    $resp.Content
    ```

    Bu istek, aşağıdaki metne benzer bir yanıt döndürür:

    ```json
    {"module":"hive","version":"1.2.1000.2.6.5.3008-11"}
    ```

1. Adlı bir tablo oluşturmak için aşağıdakileri kullanın **log4jLogs**:

    ```bash
    jobid=$(curl -s -u admin:$password -d user.name=admin -d execute="DROP+TABLE+log4jLogs;CREATE+EXTERNAL+TABLE+log4jLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+ROW+FORMAT+DELIMITED+FIELDS+TERMINATED+BY+' '+STORED+AS+TEXTFILE+LOCATION+'/example/data/';SELECT+t4+AS+sev,COUNT(*)+AS+count+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log'+GROUP+BY+t4;" -d statusdir="/example/rest" https://$clusterName.azurehdinsight.net/templeton/v1/hive | jq -r .id)
    echo $jobid
    ```

    ```powershell
    $reqParams = @{"user.name"="admin";"execute"="DROP TABLE log4jLogs;CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED BY ' ' STORED AS TEXTFILE LOCATION '/example/data/;SELECT t4 AS sev,COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' GROUP BY t4;";"statusdir"="/example/rest"}
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
   * `SELECT` -Tüm satırların sayımını seçer Burada sütun **t4** değeri içeren **[Hata]** . Bu bildirimi bir değeri döndürür **3** olarak bu değeri içeren üç satır.

     > [!NOTE]  
     > HiveQL ifadelerini arasındaki boşluklar değiştirilir bildirimi `+` Curl ile kullanılan karakter. Sınırlayıcı gibi bir alanı içeren tırnak içine alınmış değerler değil yerine `+`.

      Bu komut, iş durumunu denetlemek için kullanılan bir iş kimliği döndürür.

1. İşin durumunu denetlemek için aşağıdaki komutu kullanın:

    ```bash
    curl -u admin:$password -d user.name=admin -G https://$clusterName.azurehdinsight.net/templeton/v1/jobs/$jobid | jq .status.state
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

1. İş durumu için değiştiğinde **başarılı**, Azure Blob depolama alanından iş sonuçlarını alabilirsiniz. `statusdir` Sorguyla geçirilen parametre içerir; bu durumda, çıkış dosyasının konumu `/example/rest`. Bu adres çıktısında depolar `example/curl` kümeleri varsayılan depolama alanı içindeki dizin.

    Liste ve kullanarak bu dosyaları indirmek [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli). Azure depolama ile Azure CLI kullanma ile ilgili daha fazla bilgi için bkz: [kullanımı Azure CLI ile Azure depolama](https://docs.microsoft.com/azure/storage/storage-azure-cli#create-and-manage-blobs) belge.

## <a id="nextsteps"></a>Sonraki adımlar

HDInsight ile Hive hakkında genel bilgi için:

* [HDInsight üzerinde Apache Hadoop ile Apache Hive'ı kullanma](hdinsight-use-hive.md)

Diğer yollar hakkında daha fazla bilgi için HDInsight üzerinde Hadoop ile çalışabilirsiniz:

* [HDInsight üzerinde Apache Hadoop ile Apache Pig kullanma](hdinsight-use-pig.md)
* [HDInsight üzerinde Apache Hadoop ile MapReduce kullanma](hdinsight-use-mapreduce.md)

Bu belgede kullanılan REST API hakkında daha fazla bilgi için bkz. [WebHCat başvuru](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference) belge.