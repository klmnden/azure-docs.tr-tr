---
title: "Apache Spark, Kafka - Azure Hdınsight akış | Microsoft Docs"
description: "İçine veya dışına Apache DStreams kullanarak Kafka Spark Apache Spark veri akışı kullanmayı öğrenin. Bu örnekte, Hdınsight'ta Spark gelen Jupyter Not Defteri kullanarak veri akışı."
keywords: "kafka örnek, kafka zookeeper spark kafka, kafka örnek Spark Akış akış"
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: dd8f53c1-bdee-4921-b683-3be4c46c2039
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/06/2017
ms.author: larryfr
ms.openlocfilehash: 788ba828d1380b17913cabf18827c1abcc83c725
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="apache-spark-streaming-dstream-example-with-kafka-preview-on-hdinsight"></a>Apache Spark Hdınsight üzerinde Kafka (Önizleme) ile (DStream) örnek akış

Spark Apache Spark veri akışı içine veya dışına DStreams kullanarak Hdınsight üzerinde Apache Kafka nasıl kullanılacağını öğrenin. Bu örnek, Spark kümesinde çalışan bir Jupyter not defteri kullanır.
> [!NOTE]
> Bu belgede yer alan adımlar, hem hdınsight'ta Spark ve Hdınsight kümesinde bir Kafka içeren bir Azure kaynak grubu oluşturun. Bu kümeleri, hem bir Azure sanal Kafka ile doğrudan iletişim kurmak Spark kümesi sağlayan ağ içinde bulunan küme ' dir.
>
> Bu belgedeki adımları tamamladığınızda, aşırı ücretlerden kaçınmak için kümelerini Sil unutmayın.

## <a name="create-the-clusters"></a>Kümeleri oluşturma

Hdınsight üzerinde Apache Kafka erişim genel internet üzerinden Kafka aracıların sağlamaz. İçin Kafka ettiği herhangi bir şey Kafka kümedeki düğümlerin aynı Azure sanal ağ içinde olmalıdır. Bu örnekte, bir Azure sanal ağında Kafka ve Spark kümeleri bulunur. Aşağıdaki diyagramda, iletişim kümeleri arasında nasıl aktığını gösterir:

![Bir Azure sanal ağı Spark ve Kafka kümelerde diyagramı](./media/hdinsight-apache-spark-with-kafka/spark-kafka-vnet.png)

> [!NOTE]
> Kafka kendi sanal ağ içinde iletişimi sınırlı olsa da, SSH ve Ambari gibi kümedeki diğer hizmetlerin internet üzerinden erişilebilir. Hdınsight ile kullanılabilen ortak bağlantı noktaları hakkında daha fazla bilgi için bkz: [bağlantı noktaları ve Hdınsight tarafından kullanılan URI](hdinsight-hadoop-port-settings-for-services.md).

Azure sanal ağı, Kafka, oluşturabilir ve el ile Spark kümeleri olsa da, bir Azure Resource Manager şablonunu kullanmak daha kolaydır. Azure sanal ağı, Kafka, dağıtmak ve Spark kümeleri Azure aboneliğiniz için aşağıdaki adımları kullanın.

1. Azure'da oturum açın ve Azure portalında şablon açmak için aşağıdaki düğmesini kullanın.
    
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-spark-cluster-in-vnet-v2.1.json" target="_blank"><img src="./media/hdinsight-apache-spark-with-kafka/deploy-to-azure.png" alt="Deploy to Azure"></a>
    
    Azure Resource Manager şablonu bulunur **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-spark-cluster-in-vnet-v2.1.json**.

    > [!WARNING]
    > HDInsight üzerinde Kafka'yı kullanabilmeniz için kümenizin en az üç çalışan düğümü içermesi gerekir. Bu şablon, üç alt düğümleri içeren bir Kafka küme oluşturur.

    Bu şablon, Kafka hem Spark Hdınsight 3.6 küme oluşturur.

2. Üzerinde girişleri doldurmak için aşağıdaki bilgileri kullanın **özel dağıtım** bölümü:
   
    ![Hdınsight özel dağıtım](./media/hdinsight-apache-spark-with-kafka/parameters.png)
   
    * **Kaynak grubu**: bir grup oluşturun veya varolan bir tanesini seçin. Bu grup, Hdınsight kümesi içerir.

    * **Konum**: coğrafi olarak yakın bir konum seçin.

    * **Temel küme adı**: Bu değer Spark temel adı olarak kullanılır ve Kafka kümeleri. Örneğin, **hdı** bir Spark spark hdi__ adlı Küme ve adlı bir Kafka kümesi oluşturur **kafka hdı**.

    * **Oturum açma kullanıcı adı küme**: Spark ve Kafka kümeleri için yönetici kullanıcı adı.

    * **Oturum açma parolası küme**: Spark ve Kafka kümeleri için yönetici kullanıcı parolası.

    * **SSH kullanıcı adı**: için Spark ve Kafka kümeleri oluşturmak için SSH kullanıcı.

    * **SSH parolası**: Spark ve Kafka kümelerinin SSH kullanıcısının parolası.

3. Okuma **hüküm ve koşullar**ve ardından **hüküm ve koşulları yukarıda belirtildiği ediyorum**.

4. Son olarak, denetleme **panoya Sabitle** ve ardından **satın alma**. Kümeleri oluşturmak için yaklaşık 20 dakika sürer.

Kaynakları oluşturduktan sonra bir Özet sayfası görüntülenir.

![Kaynak grubu vnet ve kümeler için Özet](./media/hdinsight-apache-spark-with-kafka/groupblade.png)

> [!IMPORTANT]
> Hdınsight kümeleri adlarının bildirimi **spark BASENAME** ve **kafka BASENAME**, BASENAME şablona verdiğiniz adı olduğu. Kümeye bağlanırken bu adları daha sonraki adımlarda kullanın.

## <a name="use-the-notebooks"></a>Not defterlerini kullanma

Bu belgede açıklanan örnek kodunu şu adresten edinilebilir [https://github.com/Azure-Samples/hdinsight-spark-scala-kafka](https://github.com/Azure-Samples/hdinsight-spark-scala-kafka).

Bu örnek tamamlamak için adımları `README.md`.

## <a name="delete-the-cluster"></a>Küme silme

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Bu belgede yer alan adımlar her iki kümeleri aynı Azure kaynak grubu oluşturmak için Azure portalında kaynak grubunu silebilirsiniz. Grup silindiğinde, bu belge, Azure sanal ağ ve depolama hesabı kümeleri tarafından kullanılan uygulayarak oluşturduğunuz tüm kaynakları kaldırır.

## <a name="next-steps"></a>Sonraki adımlar

Bu örnekte, Spark okumak ve yazmak için Kafka için nasıl kullanılacağı hakkında bilgi edindiniz. Kafka ile çalışmak için diğer yollarını keşfetmek için aşağıdaki bağlantıları kullanın:

* [Hdınsight üzerinde Apache Kafka kullanmaya başlama](hdinsight-apache-kafka-get-started.md)
* [MirrorMaker kullanarak HDInsight üzerinde Kafka kopyası oluşturma](hdinsight-apache-kafka-mirroring.md)
* [Apache Storm’u HDInsight üzerinde Kafka ile kullanma](hdinsight-apache-storm-with-kafka.md)

