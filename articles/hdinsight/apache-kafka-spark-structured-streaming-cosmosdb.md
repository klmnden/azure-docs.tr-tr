---
title: Apache Spark yapılandırılmış Kafka Azure Cosmos DB - Azure Hdınsight akış | Microsoft Docs
description: Apache Spark yapılandırılmış akış Apache Kafka veri okumak ve Azure Cosmos Veritabanına depolamak için nasıl kullanılacağını öğrenin. Bu örnekte, Hdınsight'ta Spark gelen Jupyter Not Defteri kullanarak veri akışı.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: ''
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: larryfr
ms.openlocfilehash: 63c536f1a8bdcfbbbd97b904f15ccf83043659e0
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="use-spark-structured-streaming-with-kafka-and-azure-cosmos-db"></a>Yapılandırılmış Spark Kafka ve Azure Cosmos DB ile akış kullanın

Azure hdınsight'ta Apache Kafka verileri okuyabilir ve ardından verilerin Azure Cosmos Veritabanına depolamak için Spark yapılandırılmış akış kullanmayı öğrenin.

Azure Cosmos DB Genel dağıtılmış, birden çok model veritabanıdır. Bu örnek SQL API'yi veritabanı modeli kullanır. Daha fazla bilgi için bkz: ['na Hoş Geldiniz Azure Cosmos DB](../cosmos-db/introduction.md) belge.

Yapılandırılmış Spark akış Spark SQL yerleşik bir akış işleme altyapısıdır. Akış hesaplamalar express için toplu hesaplama aynı statik verileri sağlar. Yapılandırılmış akışı hakkında daha fazla bilgi için bkz: [yapılandırılmış akış Programlama Kılavuzu [alfa]](http://spark.apache.org/docs/2.1.0/structured-streaming-programming-guide.html) Apache.org.

> [!IMPORTANT]
> Bu örnek, Hdınsight 3.6 üzerinde Spark 2.2 kullanılır.
>
> Bu belgede yer alan adımlar, hem hdınsight'ta Spark ve Hdınsight kümesinde bir Kafka içeren bir Azure kaynak grubu oluşturun. Bu kümeleri, hem bir Azure sanal Kafka ile doğrudan iletişim kurmak Spark kümesi sağlayan ağ içinde bulunan küme ' dir.
>
> Bu belgedeki adımları tamamladığınızda, aşırı ücretlerden kaçınmak için kümelerini Sil unutmayın.

## <a name="create-the-clusters"></a>Kümeleri oluşturma

Hdınsight üzerinde Apache Kafka erişim genel internet üzerinden Kafka aracıların sağlamaz. İçin Kafka ettiği herhangi bir şey Kafka kümedeki düğümlerin aynı Azure sanal ağ içinde olmalıdır. Bu örnekte, bir Azure sanal ağında Kafka ve Spark kümeleri bulunur. Aşağıdaki diyagramda, iletişim kümeleri arasında nasıl aktığını gösterir:

![Bir Azure sanal ağı Spark ve Kafka kümelerde diyagramı](./media/hdinsight-apache-spark-with-kafka/spark-kafka-vnet.png)

> [!NOTE]
> Sanal ağ içinde iletişimi Kafka hizmet sınırlıdır. Kümedeki SSH ve Ambari, gibi diğer hizmetlerin internet üzerinden erişilebilir. Hdınsight ile kullanılabilen ortak bağlantı noktaları hakkında daha fazla bilgi için bkz: [bağlantı noktaları ve Hdınsight tarafından kullanılan URI](hdinsight-hadoop-port-settings-for-services.md).

Azure sanal ağı, Kafka, oluşturabilir ve el ile Spark kümeleri olsa da, bir Azure Resource Manager şablonunu kullanmak daha kolaydır. Azure sanal ağı, Kafka, dağıtmak ve Spark kümeleri Azure aboneliğiniz için aşağıdaki adımları kullanın.

1. Azure'da oturum açın ve Azure portalında şablon açmak için aşağıdaki düğmesini kullanın.
    
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fhdinsight-spark-scala-kafka-cosmosdb%2Fmaster%2Fazuredeploy.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
    </a>

    Azure Resource Manager şablonu bu proje için GitHub deposunda bulunur ([https://github.com/Azure-Samples/hdinsight-spark-scala-kafka-cosmosdb](https://github.com/Azure-Samples/hdinsight-spark-scala-kafka-cosmosdb)).

    Bu şablon, aşağıdaki kaynaklara oluşturur:

    * Hdınsight 3.6 kümede Kafka.

    * Bir Spark Hdınsight 3.6 kümede.

    * Bir Azure sanal Hdınsight kümeleri içeren ağ.

        > [!NOTE]
        > Şablon tarafından oluşturulan sanal ağ 10.0.0.0/16 adres alanı kullanır.

    * Bir Azure Cosmos DB SQL API'si veritabanı.

    > [!IMPORTANT]
    > Bu örnekte kullanılan yapılandırılmış akış dizüstü Spark Hdınsight 3.6 gerektirir. Hdınsight'ta Spark önceki bir sürümünü kullanıyorsanız, dizüstü bilgisayar kullanırken hataları alırsınız.

2. Üzerinde girişleri doldurmak için aşağıdaki bilgileri kullanın **özel dağıtım** bölümü:
   
    ![Hdınsight özel dağıtım](./media/apache-kafka-spark-structured-streaming-cosmosdb/parameters.png)

    * **Abonelik**: Azure aboneliğinizi seçin.
   
    * **Kaynak grubu**: bir grup oluşturun veya varolan bir tanesini seçin. Bu grup, Hdınsight kümesi içerir.

    * **Konum**: coğrafi olarak yakın bir konum seçin.

    * **Cosmos DB hesap adı**: Bu değer Cosmos DB hesabın adı olarak kullanılır.

    * **Temel küme adı**: Bu değer Spark temel adı olarak kullanılır ve Kafka kümeleri. Örneğin, **myhdi** adlı bir Spark kümesi oluşturur __spark myhdi__ ve adlı Kafka küme **kafka myhdi**.

    * **Sürüm küme**: Hdınsight küme sürümü.

        > [!IMPORTANT]
        > Bu örnek, Hdınsight 3.6 sınanır ve diğer küme türü ile çalışmayabilir.

    * **Oturum açma kullanıcı adı küme**: Spark ve Kafka kümeleri için yönetici kullanıcı adı.

    * **Oturum açma parolası küme**: Spark ve Kafka kümeleri için yönetici kullanıcı parolası.

    * **SSH kullanıcı adı**: için Spark ve Kafka kümeleri oluşturmak için SSH kullanıcı.

    * **SSH parolası**: Spark ve Kafka kümelerinin SSH kullanıcısının parolası.

3. Okuma **hüküm ve koşullar**ve ardından **hüküm ve koşulları yukarıda belirtildiği ediyorum**.

4. Son olarak, denetleme **panoya Sabitle** ve ardından **satın alma**. Kümeleri oluşturmak için yaklaşık 20 dakika sürer.

> [!IMPORTANT]
> Kümeler, sanal ağ ve Cosmos DB hesap oluşturma 45 dakika kadar sürebilir.

## <a name="create-the-cosmos-db-database-and-collection"></a>Cosmos DB veritabanınızı ve koleksiyonunuzu oluşturun

Bu belgede kullanılan proje Cosmos DB'de verileri depolar. Kod çalıştırmadan önce ilk olarak oluşturmalısınız bir _veritabanı_ ve _koleksiyonu_ Cosmos DB Örneğinizde. Belge endpoint almanız gerekir ve _anahtar_ Cosmos DB isteklerine kimlik doğrulaması için kullanılır. 

Yapmanın bir yolu bu kullanmaktır [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest). Aşağıdaki komut dosyası adlı bir veritabanı oluşturacak `kafkadata` ve adlı bir koleksiyon `kafkacollection`. Ardından, birincil anahtar döndürür.

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

Belge uç noktası ve birincil anahtar bilgilerini aşağıdakine benzer:

```text
# endpoint
"https://mycosmosaccount.documents.azure.com:443/"
# key
"YqPXw3RP7TsJoBF5imkYR0QNA02IrreNAlkrUMkL8EW94YHs41bktBhIgWq4pqj6HCGYijQKMRkCTsSaKUO2pw=="
```

> [!IMPORTANT]
> Jupyter not defterlerinde gerektiğinde uç noktasını ve anahtar değerlerinin kaydedin.

## <a name="get-the-kafka-brokers"></a>Aracıların Kafka Al

Bu örnekteki kod Kafka kümedeki Kafka Aracısı ana bağlanır. İki Kafka Aracısı ana adresleri bulmak için aşağıdaki PowerShell veya Bash örneği kullanın:

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
> Bash örnek bekliyor `$CLUSTERNAME` Kafka küme adı içeriyor.
>
> Bu örnekte [jq](https://stedolan.github.io/jq/) JSON belgesini dışında verileri ayrıştırmak için yardımcı programı.

```bash
curl -u admin -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER" | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2
```

İstendiğinde, küme oturum açma (Yönetici) hesabı için parolayı girin

Çıktı aşağıdaki metne benzer:

`wn0-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092,wn1-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092`

Bu belge aşağıdaki bölümlerde kullanılmak üzere bu bilgileri kaydedin.

## <a name="get-the-notebooks"></a>Not defterlerini Al

Bu belgede açıklanan örnek kodunu şu adresten edinilebilir [ https://github.com/Azure-Samples/hdinsight-spark-scala-kafka-cosmosdb ](https://github.com/Azure-Samples/hdinsight-spark-scala-kafka-cosmosdb).

## <a name="upload-the-notebooks"></a>Not defterlerini karşıya yükle

Proje defterlerinden, Spark Hdınsight kümesinde için karşıya yüklemek için aşağıdaki adımları kullanın:

1. Web tarayıcınız, Spark kümesinde Jupyter not defteri bağlayın. Aşağıdaki URL ile değiştirin `CLUSTERNAME` adıyla, __Spark__ küme:

        https://CLUSTERNAME.azurehdinsight.net/jupyter

    İstendiğinde, küme oturum açma (Yönetici) ve küme oluştururken kullanılan parolayı girin.

2. Sayfanın üst sağ taraftan kullanmak __karşıya__ karşıya yüklemek için düğmeyi __akış-ücreti-data-için-kafka.ipynb__ kümeye dosya. Seçin __açık__ başlatmak için.

3. Bul __akış-ücreti-data-için-kafka.ipynb__ not defterlerini ve select listesi girişi __karşıya__ yanında düğmesi.

4. Yüklemek için 1-3 arasındaki adımları yineleyin __Stream-data-from-Kafka-to-Cosmos-DB.ipynb__ dizüstü bilgisayar.

## <a name="load-taxi-data-into-kafka"></a>Kafka yük ücreti verileri

Karşıya yüklenen dosyaların sonra seçeneğini __akış-ücreti-data-için-kafka.ipynb__ girişi not defterini açın. Veriler Kafka yüklemek için not defterindeki adımları izleyin.

## <a name="process-taxi-data-using-spark-structured-streaming"></a>Spark yapılandırılmış akış kullanarak işlem ücreti verileri

Jupyter not defteri giriş sayfadan seçin __Stream-data-from-Kafka-to-Cosmos-DB.ipynb__ girişi. Not defteri için veri akışı Kafka ve Azure Cosmos yapılandırılmış Spark akış kullanarak DB içine adımları.

## <a name="next-steps"></a>Sonraki adımlar

Spark yapılandırılmış akış kullanmayı öğrendiniz, Spark, Kafka ve Azure Cosmos DB ile çalışma hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

* [(DStream) ile Kafka Spark akış kullanmayı](hdinsight-apache-spark-with-kafka.md).
* [Jupyter not defteri ve hdınsight'ta Spark ile Başlat](spark/apache-spark-jupyter-spark-sql.md)
* [Hoş Geldiniz Azure Cosmos DB](../cosmos-db/introduction.md)
