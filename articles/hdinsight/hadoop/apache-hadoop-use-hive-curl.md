---
title: "Curl hdınsight'ta - Azure ile Hadoop Hive kullanma | Microsoft Docs"
description: "Curl kullanarak hdınsight'a Pig işleri uzaktan göndermek öğrenin."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 6ce18163-63b5-4df6-9bb6-8fcbd4db05d8
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/04/2017
ms.author: larryfr
ms.openlocfilehash: b451a80934a19f8a38ab9e8ace358674827aefa0
ms.sourcegitcommit: 828cd4b47fbd7d7d620fbb93a592559256f9d234
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2018
---
# <a name="run-hive-queries-with-hadoop-in-hdinsight-using-rest"></a>REST kullanarak hdınsight'ta Hadoop ile Hive sorguları çalıştırma

[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

Azure Hdınsight kümesinde Hadoop ile Hive sorguları çalıştırmak için WebHCat REST API kullanmayı öğrenin.

[Curl](http://curl.haxx.se/) ham HTTP isteklerini kullanarak Hdınsight ile nasıl etkileşim kurabileceğine göstermek için kullanılır. [Jq](http://stedolan.github.io/jq/) yardımcı programı, REST isteklerinden döndürülen JSON verileri işlemek için kullanılır.

> [!NOTE]
> Zaten Linux tabanlı Hadoop sunucuları kullandıysanız, ancak yeni Hdınsight için, bkz: [Linux tabanlı hdınsight'ta Hadoop hakkında bilmeniz gerekenleri](../hdinsight-hadoop-linux-information.md) belge.

## <a id="curl"></a>Hive sorgularını çalıştırma

> [!NOTE]
> WebHCat ile cURL veya başka bir REST iletişimini kullanırken Hdınsight küme yöneticisinin kullanıcı adını ve parolasını sağlayarak isteklerin kimliğini doğrulaması gerekir.
>
> Bu bölümdeki komutlar için değiştirme **yönetici** küme kimlik doğrulaması için kullanıcı. **CLUSTERNAME** değerini kümenizin adıyla değiştirin. İstendiğinde, kullanıcı hesabı için parolayı girin.
>
> REST API’sinin güvenliği [temel kimlik doğrulaması](http://en.wikipedia.org/wiki/Basic_access_authentication) ile sağlanır. Kimlik bilgilerinizin sunucuya güvenli bir şekilde gönderilir sağlamaya yardımcı olmak için her zaman güvenli HTTP (HTTPS) kullanarak istekleri yapın.

1. HDInsight kümenize bağlanabildiğinizi doğrulamak için bir komut satırında aşağıdaki komutu kullanın:

    ```bash
    curl -u admin -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    Aşağıdakine benzer bir yanıt alırsınız:

    ```json
    {"status":"ok","version":"v1"}
    ```

    Bu komutta kullanılan parametreler aşağıdaki gibidir:

    * **-u** - İstek kimliğini doğrulamak için kullanılan kullanıcı adı ve parola.
    * **-G** -bu isteği alma işlemi olduğunu gösterir.

   URL'nin başına **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, tüm istekler için aynıdır. Yol **/status**, sunucu için istek WebHCat (Templeton olarak da bilinir) durumuna döndürmek için olduğunu belirtir. Ayrıca, aşağıdaki komutu kullanarak Hive sürümü isteyebilirsiniz:

    ```bash
    curl -u admin -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/version/hive
    ```

    Bu istek aşağıdaki metni benzer bir yanıt döndürür:

    ```json
        {"module":"hive","version":"0.13.0.2.1.6.0-2103"}
    ```

2. Adlı bir tablo oluşturmak için aşağıdakileri kullanın **log4jLogs**:

    ```bash
    curl -u admin -d user.name=admin -d execute="set+hive.execution.engine=tez;DROP+TABLE+log4jLogs;CREATE+EXTERNAL+TABLE+log4jLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+ROW+FORMAT+DELIMITED+FIELDS+TERMINATED+BY+' '+STORED+AS+TEXTFILE+LOCATION+'/example/data/';SELECT+t4+AS+sev,COUNT(*)+AS+count+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log'+GROUP+BY+t4;" -d statusdir="/example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/hive
    ```

    Bu istekle kullanılan aşağıdaki Parametreler:

   * **-d** - bu yana `-G` kullanılmaz, istek varsayılan olarak, POST yöntemi. `-d`istekle birlikte gönderilen veri değerleri belirtir.

     * **User.Name** -komutu çalıştıran kullanıcının.
     * **yürütme** -HiveQL ifadelerini yürütülecek.
     * **statusdir** -bu iş için durumu yazılır dizin.

   Bu ifadeler aşağıdaki eylemleri gerçekleştirin:
   
   * **DROP TABLE** -tablosu zaten silinmiş.
   * **Dış tablo oluştur** -kovanında yeni bir 'external' tablo oluşturur. Dış tablolara yalnızca tablo tanımı kovanında depolar. Veriler özgün konumda kalır.

     > [!NOTE]
     > Dış kaynak tarafından güncelleştirilecek temel alınan veri beklediğiniz dış tablolara kullanılmalıdır. Örneğin, bir otomatik veri karşıya yükleme işlemi veya başka bir MapReduce işlemi.
     >
     > Bir dış tablo bırakma mu **değil** verileri, yalnızca tablo tanımını silin.

   * **SATIR biçimi** - verileri biçimlendirilmiş nasıl. Her günlüğüne alanlar boşlukla ayrılır.
   * **AS TEXTFILE konumu DEPOLANAN** - verilerinin depolandığı (örneğin/veri dizini) ve metin olarak depolanır.
   * **SEÇİN** -tüm satırların sayımını seçer Burada sütun **t4** değeri içeren **[Hata]**. Bu ifade değerini döndürür **3** bu değeri içeren üç satır olarak.

     > [!NOTE]
     > HiveQL ifadelerini arasında boşluk değiştirilir bildirimi `+` karakter Curl ile kullanıldığında. Sınırlayıcı gibi bir alanı içeren tırnak içine alınmış değerler değiştirilmemişse tarafından `+`.

   * **'% 25.log' INPUT__FILE__NAME gibi** -bu deyim yalnızca biten dosyaları kullanmak üzere aramayı sınırlar. günlük.

     > [!NOTE]
     > `%25` Gerçek koşul %, URL kodlanmış form olduğundan `like '%.log'`. % URL'lerde özel karakter olarak değerlendirildiğinden URL kodlanmış, olması gerekir.

   Bu komut, iş durumunu denetlemek için kullanılan bir iş kimliği döndürür.

    ```json
       {"id":"job_1415651640909_0026"}
    ```

3. İş durumunu denetlemek için aşağıdaki komutu kullanın:

    ```bash
    curl -G -u admin -d user.name=admin https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
    ```

    Değiştir **JOBID** önceki adımda döndürülen değer. Örneğin, dönüş değeri `{"id":"job_1415651640909_0026"}`, ardından **JOBID** olacaktır `job_1415651640909_0026`.

    İş tamamlandı durumu varsa, **başarılı**.

   > [!NOTE]
   > Bu Curl istek JavaScript nesne gösterimi (JSON) belge ile iş hakkındaki bilgileri döndürür. Jq yalnızca durum değeri almak için kullanılır.

4. İş durumu olarak değiştirildi sonra **başarılı**, Azure Blob depolama alanından iş sonuçlarını alabilirsiniz. `statusdir` Sorguyla geçirilen parametre içeren çıkış dosyasının; bu durumda, konumu **/örnek/curl**. Bu adres çıktıda depolar **örnek/curl** kümeleri varsayılan depolama birimindeki dizin.

    Liste ve kullanarak bu dosyaları indirmek [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli). Azure Storage ile Azure CLI kullanma ile ilgili daha fazla bilgi için bkz: [Azure Storage ile Azure CLI 2.0 Kullan](https://docs.microsoft.com/azure/storage/storage-azure-cli#create-and-manage-blobs) belge.

5. Adlı yeni bir 'iç' tablo oluşturmak için aşağıdaki deyimleri kullanın **günlüklerini**:

    ```bash
    curl -u admin -d user.name=admin -d execute="set+hive.execution.engine=tez;CREATE+TABLE+IF+NOT+EXISTS+errorLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+STORED+AS+ORC;INSERT+OVERWRITE+TABLE+errorLogs+SELECT+t1,t2,t3,t4,t5,t6,t7+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log';SELECT+*+from+errorLogs;" -d statusdir="/example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/hive
    ```

    Bu ifadeler aşağıdaki eylemleri gerçekleştirin:

   * **Tablo IF değil var oluşturmak** -zaten yoksa, bir tablo oluşturur. Bu bildirimi Hive veri ambarında depolanan bir iç tablosu oluşturur. Bu tablo Hive tarafından yönetilir.

     > [!NOTE]
     > Dış tablolara, bir iç tablosu bırakarak temel alınan veri de siler.

   * **AS ORC DEPOLANAN** -en iyi duruma getirilmiş satır sütunlu (ORC) biçiminde verileri depolar. ORC Hive verilerini depolamak için yüksek oranda en iyi duruma getirilmiş ve verimli bir biçimidir.
   * **INSERT ÜZERİNE... SEÇİN** -satırları seçer **log4jLogs** içeren tablo **[Hata]**, verileri ekler **günlüklerini** tablo.
   * **SEÇİN** -tüm satırları yeni seçer **günlüklerini** tablo.

6. İş durumunu denetlemek için döndürülen iş Kimliğini kullanın. Başarılı olduktan sonra Azure CLI açıklandığı gibi önceden indirmek ve sonuçları görüntülemek için kullanın. Çıktı da içeren üç satırları içermelidir **[Hata]**.

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


