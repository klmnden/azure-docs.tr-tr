---
title: Yapılandırılmış Apache Kafka'dan Azure Cosmos DB'ye - Azure HDInsight ve akış Apache Spark
description: Apache Kafka'dan veri okumak ve ardından Azure Cosmos DB'ye depolamak için Apache Spark yapılandırılmış akış'ı kullanmayı öğrenin. Bu örnekte, HDInsight üzerinde Spark’tan bir Jupyter not defterini kullanarak verilerinizi akışla aktaracaksınız.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: hrasheed
ms.openlocfilehash: c2f3d882ac01427ea017d4f9b81edc3c64cc932d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64728724"
---
# <a name="use-apache-spark-structured-streaming-with-apache-kafka-and-azure-cosmos-db"></a>Apache Spark yapılandırılmış akış Apache Kafka ve Azure Cosmos DB ile kullanma

Nasıl kullanacağınızı öğrenin [Apache Spark](https://spark.apache.org/) [yapılandırılmış akış](https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html) verileri okumak için [Apache Kafka](https://kafka.apache.org/) Azure HDInsight, ardından store ve Azure Cosmos DB verileri.

[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) Global olarak dağıtılmış, çok modelli bir veritabanıdır. Bu örnek, bir SQL API'si veritabanı modeli kullanır. Daha fazla bilgi için [Azure Cosmos DB'ye Hoş Geldiniz](../cosmos-db/introduction.md) belge.

Spark yapılandırılmış akışı, Spark SQL üzerinde yerleşik bir akış işleme altyapısıdır. Bu altyapıyı kullanarak, statik veriler üzerinde toplu hesaplamayla aynı şekilde akış hesaplamalarını ifade edebilirsiniz. Yapılandırılmış akış hakkında daha fazla bilgi için bkz: [yapılandırılmış akış Programlama Kılavuzu](https://spark.apache.org/docs/2.2.0/structured-streaming-programming-guide.html) Apache.org.

> [!IMPORTANT]  
> Bu örnekte, HDInsight 3.6 üzerinde Spark 2.2 kullanılır.
>
> Bu belgede yer alan adımlar hem HDInsight üzerinde Spark hem de HDInsight kümesinde Kafka içeren bir Azure kaynak grubu oluşturur. Bu kümelerin her ikisi de Spark kümesinin Kafka kümesiyle doğrudan iletişim kurmasına olanak tanıyan bir Azure Sanal Ağı içinde bulunur.
>
> Bu belgedeki adımları tamamladığınızda, aşırı ücretlerden kaçınmak için kümeleri silmeyi unutmayın.

## <a name="create-the-clusters"></a>Kümeleri oluşturma

HDInsight üzerinde Apache Kafka, genel internet üzerinden Kafka aracılarına erişim sağlamaz. Kafka için ile konuşuyor ve kendisinden herhangi bir şey Kafka kümesindeki düğümler aynı Azure sanal ağ olması gerekir. Bu örnekte, Kafka ve Spark kümeleri, bir Azure sanal ağında yer alır. Aşağıdaki diyagramda, nasıl kümeleri iletişimin akış gösterilmektedir:

![Bir Azure sanal ağında Spark ve Kafka kümeleri diyagramı](./media/hdinsight-apache-spark-with-kafka/spark-kafka-vnet.png)

> [!NOTE]  
> Kafka hizmeti, sanal ağ içindeki iletişimle sınırlıdır. SSH ve Ambari gibi küme üzerindeki diğer hizmetlere internet üzerinden erişilebilir. HDInsight üzerinde kullanılabilir olan genel bağlantı noktaları hakkında daha fazla bilgi için bkz. [HDInsight Tarafından Kullanılan Bağlantı Noktaları ve URI’ler](hdinsight-hadoop-port-settings-for-services.md).

Bir Azure sanal ağı, Kafka, oluşturabileceğiniz ve el ile Spark kümeleri, ancak bir Azure Resource Manager şablonu kullanmak daha kolaydır. Bir Azure sanal ağı, Kafka, dağıtma ve Spark kümeleri, Azure aboneliğiniz için aşağıdaki adımları kullanın.

1. Aşağıdaki düğmeyi kullanarak Azure'da oturum açın ve şablonu Azure portalında açın.
    
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fhdinsight-spark-scala-kafka-cosmosdb%2Fmaster%2Fazuredeploy.json" target="_blank">
    <img src="https://azuredeploy.net/deploybutton.png"/>
    </a>

    Azure Resource Manager şablonu, bu proje için GitHub deposunda bulunur ([https://github.com/Azure-Samples/hdinsight-spark-scala-kafka-cosmosdb](https://github.com/Azure-Samples/hdinsight-spark-scala-kafka-cosmosdb)).

    Bu şablon aşağıdaki kaynakları oluşturur:

   * HDInsight 3.6 kümesi üzerinde bir Kafka.

   * HDInsight 3.6 kümesi üzerinde bir Spark.

   * HDInsight kümeleri içeren bir Azure Sanal Ağı.

       > [!NOTE]  
       > Şablon tarafından oluşturulan sanal ağ 10.0.0.0/16 adres alanı kullanır.

   * Bir Azure Cosmos DB SQL API veritabanı.

     > [!IMPORTANT]  
     > Bu örnekte kullanılan yapılandırılmış akış not defteri, HDInsight 3.6 üzerinde Spark gerektirir. HDInsight üzerinde Spark’ın daha önceki bir sürümünü kullanıyorsanız, not defterini kullanırken hatalarla karşılaşırsınız.

2. Girişleri doldurmak için aşağıdaki bilgileri kullanın **özel dağıtım** bölümü:
   
    ![HDInsight özel dağıtım](./media/apache-kafka-spark-structured-streaming-cosmosdb/parameters.png)

    * **Abonelik**: Azure aboneliğinizi seçin.
   
    * **Kaynak grubu**: Bir grup oluşturun veya var olanı seçin. Bu grup, HDInsight kümesi içerir.

    * **Konum**: Coğrafi olarak yakın bir konum seçin.

    * **Cosmos DB hesap adı**: Bu değer, Cosmos DB hesabı adı olarak kullanılır.

    * **Temel küme adı**: Bu değer, Spark ve Kafka kümeleri için temel adı olarak kullanılır. Örneğin, girme **myhdi** adlı bir Spark kümesi oluşturulur __spark myhdi__ ve adlı bir Kafka kümesi **kafka myhdi**.

    * **Küme sürümü**: HDInsight küme sürümü.

        > [!IMPORTANT]  
        > Bu örnekte, HDInsight 3.6 ile birlikte test edilir ve diğer küme türleri ile çalışmayabilir.

    * **Küme oturum açma kullanıcı adı**: Spark ve Kafka kümeleri için yönetici kullanıcı adı.

    * **Küme oturum açma parolası**: Spark ve Kafka kümeleri için yönetici kullanıcı parolası.

    * **SSH kullanıcı adı**: Spark ve Kafka kümeler için oluşturulacak SSH kullanıcısı.

    * **SSH parolası**: Spark ve Kafka kümeleri için SSH kullanıcı parolası.

3. **Hüküm ve Koşullar**’ı okuyun ve ardından **Yukarıda belirtilen hüküm ve koşulları kabul ediyorum**’u seçin.

4. Son olarak, seçin **satın alma**. Kümelerin oluşturulması yaklaşık 20 dakika sürer.

> [!IMPORTANT]  
> Bu kümeler, sanal ağ ve Cosmos DB hesabı oluşturma 45 dakika kadar sürebilir.

## <a name="create-the-cosmos-db-database-and-collection"></a>Cosmos DB veritabanı ve koleksiyon oluşturma

Bu belgede kullanılan proje verilerini Cosmos DB içinde depolar. Önce oluşturmanız gerekir kodu çalıştırmadan önce bir _veritabanı_ ve _koleksiyon_ Cosmos DB Örneğinizdeki. Koncový bod dokumentu ayrıca Al gerekir ve _anahtarı_ Cosmos DB'ye isteklerinin kimliğini doğrulamak için kullanılır. 

Yapmanın bir yolu bu kullanmaktır [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest). Aşağıdaki komut dosyası adlı bir veritabanı oluşturur `kafkadata` ve adlı bir koleksiyon `kafkacollection`. Ardından, birincil anahtarını döndürür.

```azurecli
#!/bin/bash

# Replace 'myresourcegroup' with the name of your resource group
resourceGroupName='myresourcegroup'
# Replace 'mycosmosaccount' with the name of your Cosmos DB account name
name='mycosmosaccount'

# WARNING: If you change the databaseName or collectionName
#          then you must update the values in the Jupyter notebook
databaseName='kafkadata'
collectionName='kafkacollection'

# Create the database
az cosmosdb database create --name $name --db-name $databaseName --resource-group $resourceGroupName
# Create the collection
az cosmosdb collection create --collection-name $collectionName --name $name --db-name $databaseName --resource-group $resourceGroupName

# Get the endpoint
az cosmosdb show --name $name --resource-group $resourceGroupName --query documentEndpoint

# Get the primary key
az cosmosdb list-keys --name $name --resource-group $resourceGroupName --query primaryMasterKey
```

Koncový bod dokumentu ve birincil anahtar bilgilerine aşağıdaki metne benzer:

```text
# endpoint
"https://mycosmosaccount.documents.azure.com:443/"
# key
"YqPXw3RP7TsJoBF5imkYR0QNA02IrreNAlkrUMkL8EW94YHs41bktBhIgWq4pqj6HCGYijQKMRkCTsSaKUO2pw=="
```

> [!IMPORTANT]  
> Jupyter not defterlerinde gerektiğinde uç noktasını ve anahtarı değerleri kaydedin.

## <a name="get-the-apache-kafka-brokers"></a>Apache Kafka aracılarına Al

Bu örnekteki kod Kafka kümesinin Kafka Aracısı ana bilgisayarlara bağlanır. İki Kafka aracı konak adreslerini bulmak için aşağıdaki PowerShell veya Bash örneği kullanın:

```powershell
$creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"
$clusterName = Read-Host -Prompt "Enter the Kafka cluster name"
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/KAFKA/components/KAFKA_BROKER" `
    -Credential $creds `
    -UseBasicParsing
$respObj = ConvertFrom-Json $resp.Content
$brokerHosts = $respObj.host_components.HostRoles.host_name[0..1]
($brokerHosts -join ":9092,") + ":9092"
```

> [!NOTE]  
> Bash örnek bekliyor `$CLUSTERNAME` değerini Kafka kümesinin adı içeriyor.
>
> Bu örnekte [jq](https://stedolan.github.io/jq/) yardımcı programını, verileri JSON belgesi ayrıştırılamadı.

```bash
curl -u admin -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER" | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2
```

İstendiğinde, küme oturum açma (Yönetici) hesabı için parolayı girin.

Çıktı aşağıdaki metne benzer:

`wn0-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092,wn1-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092`

Aşağıdaki bölümlerde bu belgenin kullanıldığından bu bilgileri kaydedin.

## <a name="get-the-notebooks"></a>Not defterlerini Al

Bu belgede açıklanan örneğin kodu [https://github.com/Azure-Samples/hdinsight-spark-scala-kafka-cosmosdb](https://github.com/Azure-Samples/hdinsight-spark-scala-kafka-cosmosdb) sayfasından edinilebilir.

## <a name="upload-the-notebooks"></a>Not defterlerini karşıya yükleme

HDInsight kümesi üzerinde Spark projesinden not defterlerini karşıya yüklemek için aşağıdaki adımları kullanın:

1. Web tarayıcınızdan Spark kümeniz üzerindeki Jupyter not defterine bağlanın. Aşağıdaki URL’de `CLUSTERNAME` değerini __Spark__ kümenizin adıyla değiştirin:

        https://CLUSTERNAME.azurehdinsight.net/jupyter

    Sorulduğunda, kümeyi oluştururken kullanılan küme kullanıcı adı (yönetici) ve parolasını girin.

2. Sayfanın üst sağ taraftan kullanın __karşıya__ karşıya yükleme düğmesini __Stream-taksi-data-için-kafka.ipynb__ dosyasını kümeye. Karşıya yüklemeyi başlatmak için __Aç__’ı seçin.

3. Bulma __Stream-taksi-data-için-kafka.ipynb__ Not defterlerinin ve select listesindeki girdinin __karşıya__ yanında düğmesi.

4. Yüklemek için 1-3 arasındaki adımları yineleyin __Stream-data-from-Kafka-to-Cosmos-DB.ipynb__ dizüstü bilgisayar.

## <a name="load-taxi-data-into-kafka"></a>Kafka taksi verileri yükleme

Dosyalar karşıya yüklendikten sonra seçin __Stream-taksi-data-için-kafka.ipynb__ giriş not defterini açın. Not defterini Kafka verileri yüklemek için adımları izleyin.

## <a name="process-taxi-data-using-spark-structured-streaming"></a>Spark yapılandırılmış akış'ı kullanarak taksi verilerini işleme

Gelen [Jupyter not defteri](https://jupyter.org/) giriş sayfası, select __Stream-data-from-Kafka-to-Cosmos-DB.ipynb__ girişi. Not defterinde adımları, Kafka ve Spark yapılandırılmış akış'ı kullanarak Azure Cosmos DB içine veri akışı için izleyin.

## <a name="next-steps"></a>Sonraki adımlar

Apache Spark yapılandırılmış akışını kullanmayı öğrendiniz, Apache Spark, Apache Kafka ve Azure Cosmos DB ile çalışma hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

* [Apache Spark (DStream) akışı ile Apache Kafka kullanma](hdinsight-apache-spark-with-kafka.md).
* [Jupyter not defteri ve HDInsight üzerinde Apache Spark kullanmaya başlayın](spark/apache-spark-jupyter-spark-sql.md)
* [Azure Cosmos DB'ye Hoş Geldiniz](../cosmos-db/introduction.md)
