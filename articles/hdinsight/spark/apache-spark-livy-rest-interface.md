---
title: Azure Hdınsight'ta Spark işlerini küme göndermek için Livy Spark kullanın | Microsoft Docs
description: Apache Spark REST API uzaktan Azure Hdınsight kümesi için Spark iş göndermek için nasıl kullanılacağını öğrenin.
keywords: Apache spark rest API, livy spark
services: hdinsight
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 2817b779-1594-486b-8759-489379ca907d
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: conceptual
ms.date: 12/11/2017
ms.author: nitinme
ms.openlocfilehash: 29cf245a03b38be4f5396a3c83c966a27cf038f3
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
ms.locfileid: "31517786"
---
# <a name="use-apache-spark-rest-api-to-submit-remote-jobs-to-an-hdinsight-spark-cluster"></a>Hdınsight Spark kümesinde işleri uzaktan göndermek için Apache Spark REST API kullanın

Livy, Azure Hdınsight Spark kümesinde işleri uzaktan göndermek için kullanılan Apache Spark REST API kullanmayı öğrenin. Ayrıntılı belgeler için bkz: [ http://livy.incubator.apache.org/ ](http://livy.incubator.apache.org/).

Livy etkileşimli Spark Kabukları çalıştırmak veya Spark çalıştırılmak üzere toplu iş göndermek için kullanabilirsiniz. Bu makalede, toplu iş göndermek Livy kullanarak alınmaktadır. Bu makalede parçacıkları cURL Livy Spark uç REST API çağrıları yapmak için kullanın.

**Ön koşullar:**

* Hdınsight'ta bir Apache Spark kümesi. Yönergeler için bkz: [Azure Hdınsight'ta Apache Spark oluşturmak kümeleri](apache-spark-jupyter-spark-sql.md).

* [cURL](http://curl.haxx.se/). Bu makalede, Hdınsight Spark kümesinde karşı REST API çağrılarının nasıl yapılacağını göstermek üzere cURL kullanılmıştır.

## <a name="submit-a-livy-spark-batch-job"></a>Livy Spark toplu iş gönderme
Toplu iş göndermeden önce küme depolama kümesi ile ilişkili uygulama jar yüklemeniz gerekir. Kullanabileceğiniz [ **AzCopy**](../../storage/common/storage-use-azcopy.md), bunu yapmak için bir komut satırı yardımcı programı. Verileri yüklemek için kullanabileceğiniz çeşitli istemciler vardır. Onları hakkında daha fazla bilgiyi [hdınsight'ta Hadoop işleri için verileri karşıya yükleme](../hdinsight-upload-data.md).

    curl -k --user "<hdinsight user>:<user password>" -v -H <content-type> -X POST -d '{ "file":"<path to application jar>", "className":"<classname in jar>" }' 'https://<spark_cluster_name>.azurehdinsight.net/livy/batches'

**Örnekler**:

* Jar dosyasını küme storage (WASB) ise
  
        curl -k --user "admin:mypassword1!" -v -H 'Content-Type: application/json' -X POST -d '{ "file":"wasb://mycontainer@mystorageaccount.blob.core.windows.net/data/SparkSimpleTest.jar", "className":"com.microsoft.spark.test.SimpleFile" }' "https://mysparkcluster.azurehdinsight.net/livy/batches"
* Girdi dosyası bir parçası olarak jar filename ve classname geçirmek istiyorsanız (Bu örnekte, girdi.txt)
  
        curl -k  --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

## <a name="get-information-on-livy-spark-batches-running-on-the-cluster"></a>Küme üzerinde çalışan Livy Spark toplu işlemler hakkında bilgi edinin
    curl -k --user "<hdinsight user>:<user password>" -v -X GET "https://<spark_cluster_name>.azurehdinsight.net/livy/batches"

**Örnekler**:

* Tüm küme üzerinde çalışan Livy Spark toplu almak istiyorsanız:
  
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"
* Belirli bir yığın belirli bir toplu işin almak istiyorsanız
  
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"

## <a name="delete-a-livy-spark-batch-job"></a>Livy Spark toplu işi silme
    curl -k --user "<hdinsight user>:<user password>" -v -X DELETE "https://<spark_cluster_name>.azurehdinsight.net/livy/batches/{batchId}"

**Örnek**:

    curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"

## <a name="livy-spark-and-high-availability"></a>Livy Spark ve yüksek kullanılabilirlik
Livy yüksek kullanılabilirlik için Spark kümesinde çalışan işleri sağlar. Aşağıda birkaç örnek verilmiştir.

* Uzaktan bir Spark kümesi için bir işi gönderdikten sonra Livy hizmeti devre dışı kalırsa, iş arka planda çalışmaya devam eder. Livy yedekleme olduğunda geri raporları ve iş durumunu geri yükler.
* Hdınsight için Jupyter not defterleri Livy tarafından arka güç sağlar. Bir not defteri Spark işi çalışıyor ve Livy hizmeti yeniden, not defteri kod hücreleri çalışmaya devam eder. 

## <a name="show-me-an-example"></a>Örnek göster
Bu bölümde, toplu iş gönderme, işin ilerleme durumunu izlemek ve bunu silmek için Livy Spark kullanmaya örneklere bakın. Bu örnekte kullanırız makalesinde geliştirilen bir uygulamadır [tek başına Scala uygulama ve Hdınsight Spark küme üzerinde çalışacak şekilde Oluştur](apache-spark-create-standalone-application.md). Adımlar Burada, varsayın:

* Kümeyle ilişkili depolama hesabı uygulama jar önceden kopyaladığınız.
* CuRL, şu adımları çalıştığınız bilgisayarda yüklü olması.

Aşağıdaki adımları gerçekleştirin:

1. Bize Livy Spark kümesinde çalıştığını ilk doğrulayın. Biz toplu çalışan listesini alarak bunu yapabilirsiniz. Bir işi Livy kullanarak ilk kez çalıştırıyorsanız, çıktı sıfır döndürmelidir.
   
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"
   
    Aşağıdaki kod parçacığında benzer bir çıktı almalısınız:
   
        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:47:53 GMT
        < Content-Length: 34
        <
        {"from":0,"total":0,"sessions":[]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact
   
    Çıktı son satırında nasıl diyor fark **toplam: 0**, hiçbir çalışan toplu önerir.

2. Bize toplu iş şimdi gönderin. Aşağıdaki kod parçacığında jar adını ve sınıf adını parametre olarak geçirmek için bir giriş dosyası (girdi.txt) kullanır. Bu adımları bir Windows bilgisayardan çalıştırılıyorsa, bir giriş dosyası kullanarak önerilen yaklaşımdır.
   
        curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"
   
    Parametreler dosyasında **girdi.txt** şu şekilde tanımlanır:
   
        { "file":"wasb:///example/jars/SparkSimpleApp.jar", "className":"com.microsoft.spark.example.WasbIOTest" }
   
    Aşağıdaki kod parçacığına benzer bir çıktı görmeniz gerekir:
   
        < HTTP/1.1 201 Created
        < Content-Type: application/json; charset=UTF-8
        < Location: /0
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:51:30 GMT
        < Content-Length: 36
        <
        {"id":0,"state":"starting","log":[]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact
   
    Çıktının son satırında nasıl diyor fark **durumu: Başlangıç**. Ayrıca diyor, **kodu: 0**. Burada, **0** toplu kimliğidir.

3. Şimdi toplu iş kimliğini kullanarak bu belirli toplu işlem durumunu Al
   
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/0"
   
    Aşağıdaki kod parçacığına benzer bir çıktı görmeniz gerekir:
   
        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:54:42 GMT
        < Content-Length: 509
        <
        {"id":0,"state":"success","log":["\t diagnostics: N/A","\t ApplicationMaster host: 10.0.0.4","\t ApplicationMaster RPC port: 0","\t queue: default","\t start time: 1448063505350","\t final status: SUCCEEDED","\t tracking URL: http://hn0-myspar.lpel1gnnvxne3gwzqkfq5u5uzh.jx.internal.cloudapp.net:8088/proxy/application_1447984474852_0002/","\t user: root","15/11/20 23:52:47 INFO Utils: Shutdown hook called","15/11/20 23:52:47 INFO Utils: Deleting directory /tmp/spark-b72cd2bf-280b-4c57-8ceb-9e3e69ac7d0c"]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact
   
    Şimdi çıktısında **durumu: başarılı**, hangi öneren işi başarıyla tamamlandı.

4. İsterseniz, toplu artık silebilirsiniz.
   
        curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/0"
   
    Aşağıdaki kod parçacığına benzer bir çıktı görmeniz gerekir:
   
        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Sat, 21 Nov 2015 18:51:54 GMT
        < Content-Length: 17
        <
        {"msg":"deleted"}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact
   
    Çıktının son satırında toplu başarıyla silindiğini gösterir. Bir iş çalışırken, silme, işi sonlandırır. Başarıyla veya aksi halde, tamamlanmış bir iş silerseniz iş bilgilerini tamamen siler.

## <a name="using-livy-spark-on-hdinsight-35-clusters"></a>Livy Spark Hdınsight 3.5 kümelerinde kullanma

Hdınsight 3.5 küme, varsayılan olarak, yerel dosya yolları erişim örnek veri dosyalarını veya Kavanoz kullanımını devre dışı bırakır. Kullanmanızı öneririz `wasb://` yerine Kavanoz erişmek veya veri örneği için dosyalarının yolu kümeden. Yerel yol kullanmak istiyorsanız, Ambari yapılandırma uygun şekilde güncelleştirmeniz gerekir. Bunu yapmak için:

1. Küme için Ambari portalına gidin. Ambari Web kullanıcı arabirimini https:// konumunda Hdınsight kümenize kullanılabilir**CLUSTERNAME**. azurehdidnsight.net, burada CLUSTERNAME kümenizin adıdır.

2. Sol gezinti bölmesinden tıklatın **Livy**ve ardından **yapılandırmalar**.

3. Altında **livy varsayılan** özellik adı eklemek `livy.file.local-dir-whitelist` ve değeri ayarlayın **"/"** tam dosya sistemi erişmesine izin vermek istiyorsanız. Belirli bir dizin yalnızca erişmesine izin vermek istiyorsanız, bu dizinin yolunu değeri olarak sağlayın.

## <a name="submitting-livy-jobs-for-a-cluster-within-an-azure-virtual-network"></a>Bir küme içinde bir Azure sanal ağı Livy işleri gönderme

Bir Azure sanal ağ içindeki bir Hdınsight Spark kümesi bağlarsanız kümede Livy için doğrudan bağlanabilir. Livy uç noktası için URL böyle bir durumda olduğundan `http://<IP address of the headnode>:8998/batches`. Burada, **8998** Livy küme headnode üzerinde çalıştığı bağlantı noktasıdır. Hizmetlere genel olmayan bağlantı noktalarına erişme hakkında daha fazla bilgi için bkz: [hdınsight'ta Hadoop Hizmetleri tarafından kullanılan bağlantı noktaları](../hdinsight-hadoop-port-settings-for-services.md).

## <a name="troubleshooting"></a>Sorun giderme

Livy kullanarak Spark kümeleri için uzak iş gönderme için içine çalışabilir bazı sorunlar şunlardır.

### <a name="using-an-external-jar-from-the-additional-storage-is-not-supported"></a>Ek depolama biriminden bir dış jar kullanılması desteklenmez

**Sorun:** Livy Spark işinizi kümesi ile ilişkili ek depolama hesabından bir dış jar başvuruyorsa, işi başarısız olur.

**Çözüm:** kullanmak istediğiniz jar Hdınsight kümesi ile ilişkili varsayılan depolama alanında kullanılabilir olduğundan emin olun.





## <a name="next-step"></a>Sonraki adım

* [Livy REST API belgeleri](http://livy.incubator.apache.org/docs/latest/rest-api.html)
* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](apache-spark-job-debugging.md)

