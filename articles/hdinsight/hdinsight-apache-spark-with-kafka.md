---
title: Apache Spark ile Apache Kafka - Azure HDInsight akış
description: Apache Spark akışı verilerini içine veya dışına DStreams kullanarak Apache Kafka kullanmayı öğrenin. Bu örnekte, HDInsight üzerinde Spark’tan bir Jupyter not defterini kullanarak verilerinizi akışla aktaracaksınız.
keywords: Örnek kafka, kafka zookeeper, spark akış kafka, spark, kafka örnek akış
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: hrasheed
ms.openlocfilehash: e0c39ae5f5c23ae0715ef1eee38b6dd34704538a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64690956"
---
# <a name="apache-spark-streaming-dstream-example-with-apache-kafka-on-hdinsight"></a>Apache Spark akışını (DStream) HDInsight üzerinde Apache Kafka örneğiyle

Nasıl kullanacağınızı öğrenin [Apache Spark](https://spark.apache.org/) içinde veya dışında veri akışı [Apache Kafka](https://kafka.apache.org/) HDInsight kullanma [DStreams](https://spark.apache.org/docs/latest/api/java/org/apache/spark/streaming/dstream/DStream.html). Bu örnekte bir [Jupyter not defteri](https://jupyter.org/) kullanarak Spark kümesi üzerinde çalışır.

> [!NOTE]  
> Bu belgede yer alan adımlar hem HDInsight üzerinde Spark hem de HDInsight kümesinde Kafka içeren bir Azure kaynak grubu oluşturur. Bu kümelerin her ikisi de Spark kümesinin Kafka kümesiyle doğrudan iletişim kurmasına olanak tanıyan bir Azure Sanal Ağı içinde bulunur.
>
> Bu belgedeki adımları tamamladığınızda, aşırı ücretlerden kaçınmak için kümeleri silmeyi unutmayın.

> [!IMPORTANT]  
> Bu örnek, daha eski bir Spark akış teknolojisinin DStreams kullanır. Yeni Spark özelliklerini akış kullanan bir örnek için bkz: [Spark yapılandırılmış akışı ile Apache Kafka](hdinsight-apache-kafka-spark-structured-streaming.md) belge.

## <a name="create-the-clusters"></a>Kümeleri oluşturma

HDInsight üzerinde Apache Kafka, genel internet üzerinden Kafka aracılarına erişim sağlamaz. Kafka için ile konuşuyor ve kendisinden herhangi bir şey Kafka kümesindeki düğümler aynı Azure sanal ağ olması gerekir. Bu örnekte, Kafka ve Spark kümeleri, bir Azure sanal ağında yer alır. Aşağıdaki diyagramda, nasıl kümeleri iletişimin akış gösterilmektedir:

![Bir Azure sanal ağında Spark ve Kafka kümeleri diyagramı](./media/hdinsight-apache-spark-with-kafka/spark-kafka-vnet.png)

> [!NOTE]  
> Kafka kendi sanal ağ içindeki iletişimle sınırlıdır ancak SSH ve Ambari gibi küme üzerindeki diğer hizmetler internet üzerinden erişilebilir. HDInsight üzerinde kullanılabilir olan genel bağlantı noktaları hakkında daha fazla bilgi için bkz. [HDInsight Tarafından Kullanılan Bağlantı Noktaları ve URI’ler](hdinsight-hadoop-port-settings-for-services.md).

Bir Azure sanal ağı, Kafka, oluşturabileceğiniz ve el ile Spark kümeleri, ancak bir Azure Resource Manager şablonu kullanmak daha kolaydır. Bir Azure sanal ağı, Kafka, dağıtma ve Spark kümeleri, Azure aboneliğiniz için aşağıdaki adımları kullanın.

1. Aşağıdaki düğmeyi kullanarak Azure'da oturum açın ve şablonu Azure portalında açın.
    
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-spark-cluster-in-vnet-v4.1.json" target="_blank"><img src="./media/hdinsight-apache-spark-with-kafka/deploy-to-azure.png" alt="Deploy to Azure"></a>
    
    Azure Resource Manager şablonu **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-spark-cluster-in-vnet-v4.1.json** sayfasında bulunur.

    > [!WARNING]  
    > HDInsight üzerinde Kafka'yı kullanabilmeniz için kümenizin en az üç çalışan düğümü içermesi gerekir. Bu şablon, üç çalışan düğümü içeren bir Kafka kümesi oluşturur.

    Bu şablon, Kafka ve Spark için bir HDInsight 3.6 kümesi oluşturur.

2. Girişleri doldurmak için aşağıdaki bilgileri kullanın **özel dağıtım** bölümü:
   
    ![HDInsight özel dağıtım](./media/hdinsight-apache-spark-with-kafka/parameters.png)
   
    * **Kaynak grubu**: Bir grup oluşturun veya var olanı seçin. Bu grup, HDInsight kümesi içerir.

    * **Konum**: Coğrafi olarak yakın bir konum seçin.

    * **Temel küme adı**: Bu değer, Spark ve Kafka kümeleri için temel adı olarak kullanılır. Örneğin, girme **hdistreaming** adlı bir Spark kümesi oluşturulur __spark hdistreaming__ ve adlı bir Kafka kümesi **kafka hdistreaming**.

    * **Küme oturum açma kullanıcı adı**: Spark ve Kafka kümeleri için yönetici kullanıcı adı.

    * **Küme oturum açma parolası**: Spark ve Kafka kümeleri için yönetici kullanıcı parolası.

    * **SSH kullanıcı adı**: Spark ve Kafka kümeler için oluşturulacak SSH kullanıcısı.

    * **SSH parolası**: Spark ve Kafka kümeleri için SSH kullanıcı parolası.

3. **Hüküm ve Koşullar**’ı okuyun ve ardından **Yukarıda belirtilen hüküm ve koşulları kabul ediyorum**’u seçin.

4. Son olarak, seçin **satın alma**. Kümelerin oluşturulması yaklaşık 20 dakika sürer.

Kaynak oluşturulduktan sonra Özet sayfasında görünür.

![Kaynak grubu sanal ağ ve küme için Özet](./media/hdinsight-apache-spark-with-kafka/groupblade.png)

> [!IMPORTANT]  
> HDInsight kümeleri adları olan **spark BASENAME** ve **kafka BASENAME**, BASENAME şablona verdiğiniz adı olduğu yer. Kümeye bağlanırken bu adlar daha sonraki adımlarda kullanın.

## <a name="use-the-notebooks"></a>Not defterlerini kullanma

Bu belgede açıklanan örneğin kodu [https://github.com/Azure-Samples/hdinsight-spark-scala-kafka](https://github.com/Azure-Samples/hdinsight-spark-scala-kafka) sayfasından edinilebilir.

Bu örneği tamamlamak için adımları izleyin. `README.md`.

## <a name="delete-the-cluster"></a>Küme silme

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Bu belgedeki adımlarda, aynı Azure kaynak grubu içinde her iki küme oluşturma olduğundan, Azure portalında kaynak grubunu silebilirsiniz. Grup silindiğinde, bu belge, Azure sanal ağ ve küme tarafından kullanılan depolama hesabı'nı izleyerek oluşturulan tüm kaynakları kaldırır.

## <a name="next-steps"></a>Sonraki adımlar

Bu örnekte, okuma ve yazma için Kafka için Spark kullanmayı öğrendiniz. Kafka ile çalışmak için diğer yollarını bulmak için aşağıdaki bağlantıları kullanın:

* [HDInsight üzerinde Apache Kafka ile çalışmaya başlama](kafka/apache-kafka-get-started.md)
* [MirrorMaker HDInsight üzerinde Apache Kafka'nın bir çoğaltma oluşturmak için kullanın](kafka/apache-kafka-mirroring.md)
* [Apache Storm'u HDInsight üzerinde Apache Kafka ile kullanma](hdinsight-apache-storm-with-kafka.md)

