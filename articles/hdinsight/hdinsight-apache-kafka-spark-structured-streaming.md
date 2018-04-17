---
title: Kafka ile Apache Spark Yapılandırılmış Akışı - Azure HDInsight | Microsoft Docs
description: Apache Spark akışını (DStream) kullanarak Apache Kafka içine veya dışına veri almayı öğrenin. Bu örnekte, HDInsight üzerinde Spark’tan bir Jupyter not defterini kullanarak verilerinizi akışla aktaracaksınız.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: cgronlun
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: ''
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/04/2018
ms.author: larryfr
ms.openlocfilehash: 49c13bbea537d7de60ecf509bc28675191c0b34d
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="use-spark-structured-streaming-with-kafka-on-hdinsight"></a>Spark Yapılandırılmış Akışını HDInsight üzerinde Kafka ile kullanma

Azure HDInsight üzerinde Apache Kafka’dan veri okumak için Spark Yapılandırılmış Akışının nasıl kullanılacağını öğrenin.

Spark yapılandırılmış akışı, Spark SQL üzerinde yerleşik bir akış işleme altyapısıdır. Bu altyapıyı kullanarak, statik veriler üzerinde toplu hesaplamayla aynı şekilde akış hesaplamalarını ifade edebilirsiniz. Yapılandırılmış Akış hakkında daha fazla bilgi için Apache.org sitesindeki [Yapılandırılmış Akış Programlama Kılavuzu [Alfa]](http://spark.apache.org/docs/2.2.0/structured-streaming-programming-guide.html) bölümüne bakın.

> [!IMPORTANT]
> Bu örnek, HDInsight 3.6 üzerinde Spark 2.2 kullanır.
>
> Bu belgede yer alan adımlar hem HDInsight üzerinde Spark hem de HDInsight kümesinde Kafka içeren bir Azure kaynak grubu oluşturur. Bu kümelerin her ikisi de Spark kümesinin Kafka kümesiyle doğrudan iletişim kurmasına olanak tanıyan bir Azure Sanal Ağı içinde bulunur.
>
> Bu belgedeki adımları tamamladığınızda, aşırı ücretlerden kaçınmak için kümeleri silmeyi unutmayın.

## <a name="create-the-clusters"></a>Kümeleri oluşturma

HDInsight üzerinde Apache Kafka, genel internet üzerinden Kafka aracılarına erişim sağlamaz. Kafka kullanan her özellik aynı Azure sanal ağı içinde olmalıdır. Bu öğreticide hem Kafka hem de Spark kümeleri aynı Azure sanal ağı içinde yer alır. 

Aşağıdaki diyagramda Spark ile Kafka arasındaki iletişimin nasıl aktığı gösterilmektedir:

![Bir Azure sanal ağında Spark ve Kafka kümeleri diyagramı](./media/hdinsight-apache-spark-with-kafka/spark-kafka-vnet.png)

> [!NOTE]
> Kafka hizmeti, sanal ağ içindeki iletişimle sınırlıdır. SSH ve Ambari gibi küme üzerindeki diğer hizmetlere internet üzerinden erişilebilir. HDInsight üzerinde kullanılabilir olan genel bağlantı noktaları hakkında daha fazla bilgi için bkz. [HDInsight Tarafından Kullanılan Bağlantı Noktaları ve URI’ler](hdinsight-hadoop-port-settings-for-services.md).

Kolaylık olması için, aşağıdaki adımlarda bir sanal ağ içinde Kafka ve Spark kümeleri oluşturmak için üzere Azure Resource Manager şablonu kullanılmıştır.

1. Aşağıdaki düğmeyi kullanarak Azure'da oturum açın ve şablonu Azure portalında açın.
    
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fhdinsight-spark-kafka-structured-streaming%2Fmaster%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apache-spark-with-kafka/deploy-to-azure.png" alt="Deploy to Azure"></a>
    
    Azure Resource Manager şablonu **https://raw.githubusercontent.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming/master/azuredeploy.json** sayfasında bulunur.

    Bu şablon aşağıdaki kaynakları oluşturur:

    * HDInsight 3.6 kümesi üzerinde bir Kafka.
    * HDInsight 3.6 kümesi üzerinde bir Spark 2.2.0.
    * HDInsight kümeleri içeren bir Azure Sanal Ağı.

    > [!IMPORTANT]
    > Bu örnekte kullanılan yapılandırılmış akış not defteri, HDInsight 3.6 üzerinde Spark gerektirir. HDInsight üzerinde Spark’ın daha önceki bir sürümünü kullanıyorsanız, not defterini kullanırken hatalarla karşılaşırsınız.

2. **Özelleştirilmiş şablon** bölümündeki girişleri doldurmak için aşağıdaki bilgileri kullanın:

    | Ayar | Değer |
    | --- | --- |
    | Abonelik | Azure aboneliğiniz |
    | Kaynak grubu | Kaynakları içeren kaynak grubu. |
    | Konum | İçinde kaynakların oluşturulduğu Azure bölgesi. |
    | Spark Kümesi Adı | Spark kümesinin adı. |
    | Kafka Kümesi Adı | Kafka kümesinin adı. |
    | Küme Oturum Açma Kullanıcı Adı | Kümeler için yönetici kullanıcı adı. |
    | Küme Oturum Açma Parolası | Kümeler için yönetici kullanıcı parolası. |
    | SSH Kullanıcı Adı | Kümeler için oluşturulacak SSH kullanıcısı. |
    | SSH Parolası | SSH kullanıcısı için parola. |
   
    ![Özelleştirilmiş şablonun ekran görüntüsü](./media/hdinsight-apache-kafka-spark-structured-streaming/spark-kafka-template.png)

4. Son olarak, **Panoya sabitle**’yi işaretleyin ve **Satın Al**’ı seçin. 

> [!NOTE]
> Kümelerin oluşturulması 20 dakikaya kadar sürebilir.

## <a name="get-the-notebook"></a>Not defterini alma

Bu belgede açıklanan örneğin kodu [https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming](https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming) sayfasından edinilebilir.

## <a name="upload-the-notebooks"></a>Not defterlerini karşıya yükleme

Not defterini projeden HDInsight üzerinde Spark kümenize yüklemek için aşağıdaki adımları kullanın:

1. Web tarayıcınızdan Spark kümeniz üzerindeki Jupyter not defterine bağlanın. Aşağıdaki URL’de `CLUSTERNAME` değerini __Spark__ kümenizin adıyla değiştirin:

        https://CLUSTERNAME.azurehdinsight.net/jupyter

    Sorulduğunda, kümeyi oluştururken kullanılan küme kullanıcı adı (yönetici) ve parolasını girin.

2. Sayfanın sağ üst kısmındaki __Karşıya Yükle__ düğmesini kullanarak __spark-structured-streaming-kafka.ipynb__ dosyasını kümeye yükleyin. Karşıya yüklemeyi başlatmak için __Aç__’ı seçin.

    ![Karşıya yükleme düğmesini kullanarak not defterini seçme ve karşıya yükleme](./media/hdinsight-apache-kafka-spark-structured-streaming/upload-button.png)

    ![KafkaStreaming.ipynb dosyasını seçme](./media/hdinsight-apache-kafka-spark-structured-streaming/select-notebook.png)

3. Not defterleri listesinde __spark-structured-streaming-kafka.ipynb__ girişini bulun ve yanındaki __Karşıya Yükle__ düğmesini seçin.

    ![Not defterini karşıya yüklemek için KafkaStreaming.ipynb girişinin karşıya yükleme düğmesini kullanın](./media/hdinsight-apache-kafka-spark-structured-streaming/upload-notebook.png)


## <a name="use-the-notebook"></a>Not defterini kullanma

Dosyalar karşıya yüklendikten sonra __spark-structured-streaming-kafka.ipynb__ girişini seçerek not defterini açın. Spark yapılandırılmış akışını HDInsight üzerinde Kafka ile birlikte kullanma hakkında bilgi almak için not defterindeki yönergeleri izleyin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğretici ile oluşturulan kaynakları temizlemek için kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, ilişkili HDInsight kümesini ve kaynak grubuyla ilişkili diğer tüm kaynakları da siler.

Azure portalını kullanarak kaynak grubunu kaldırmak için:

1. Azure portalında sol taraftaki menüyü genişleterek hizmet menüsünü açın ve sonra __Kaynak Grupları__'nı seçerek kaynak gruplarınızın listesini görüntüleyin.
2. Silinecek kaynak grubunu bulun ve sonra listenin sağ tarafındaki __Daha fazla__ düğmesine (...) sağ tıklayın.
3. __Kaynak grubunu sil__'i seçip onaylayın.

> [!WARNING]
> HDInsight kümesi faturalandırması küme oluşturulduğunda başlar ve küme silindiğinde sona erer. Fatura dakikalara eşit olarak dağıtıldığından, kullanılmayan kümelerinizi mutlaka silmelisiniz.
> 
> HDInsight üzerinde Kafka kümesinin silinmesi Kafka’da depolanmış tüm verileri siler.

## <a name="next-steps"></a>Sonraki adımlar

Artık Spark Yapılandırılmış Akışını kullanmayı öğrendiğinize göre aşağıdaki belgelere bakarak Spark ve Kafka ile çalışma hakkında daha fazla bilgi edinebilirsiniz:

* [Kafka ile Spark akışı (DStream) kullanma](hdinsight-apache-spark-with-kafka.md).
* [Jupyter Not Defteri ve HDInsight üzerinde Spark ile Başlama](spark/apache-spark-jupyter-spark-sql.md)