---
title: "Azure Cosmos DB: Spark ve Apache TinkerPop Gremlin kullanarak grafik analiz gerçekleştirme | Microsoft Docs"
description: "Bu makalede ayarlama ve Azure Cosmos veritabanı Spark ve TinkerPop SparkGraphComputer grafik analizi ve Paralel hesaplama çalıştırmak için yönergeler sunar."
services: cosmosdb
documentationcenter: 
author: khdang
manager: shireest
editor: 
ms.assetid: 89ea62bb-c620-46d5-baa0-eefd9888557c
ms.service: cosmos-db
ms.custom: quick start connect
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: gremlin
ms.topic: article
ms.date: 09/08/2017
ms.author: khdang
ms.openlocfilehash: d2ea692c5c353586cc2b653e57eca7ccb8c9c7ce
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="azure-cosmos-db-perform-graph-analytics-by-using-spark-and-apache-tinkerpop-gremlin"></a>Azure Cosmos DB: Spark ve Apache TinkerPop Gremlin kullanarak grafik analiz gerçekleştirme

[Azure Cosmos DB](introduction.md) Microsoft Genel dağıtılmış, birden çok model veritabanı hizmetidir. Oluşturma ve belge, anahtar/değer ve grafik veritabanları, her biri genel dağıtım ve yatay ölçek özellikleri Azure Cosmos DB en yararlı sorgulayabilirsiniz. Azure Cosmos DB destekleyen kullanın (OLTP) grafik iş yükleri çevrimiçi işlem [Apache TinkerPop Gremlin](graph-introduction.md).

[Spark](http://spark.apache.org/) üzerinde genel amaçlı çevrimiçi analitik işlem (OLAP) veri işleme odaklanan bir Apache Software Foundation projesidir. Spark Hadoop MapReduce modeline benzer bir karma içinde bellek/disk tabanlı dağıtılmış bilgi işlem modelidir. Apache Spark bulutta kullanarak dağıtabilirsiniz [Azure Hdınsight](https://azure.microsoft.com/services/hdinsight/apache-spark/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

Azure Cosmos DB ve Spark birleştirerek Gremlin kullandığınızda OLTP ve OLAP iş yüklerinin gerçekleştirebilirsiniz. Bu hızlı başlangıç makale bir Azure Hdınsight Spark kümesinde Azure Cosmos DB karşı Gremlin sorguları çalıştırmak gösterilmiştir.

## <a name="prerequisites"></a>Ön koşullar

Bu örneği çalıştırmadan önce aşağıdaki önkoşullara sahip olmanız gerekir:
* Azure Hdınsight Spark kümesinde 2.0
* JDK 1.8 + (JDK yoksa çalıştırmak `apt-get install default-jdk`.)
* Maven (Maven yoksa, çalıştırmak `apt-get install maven`.)
* Bir Azure aboneliği ([!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)])

Bir Azure Hdınsight Spark kümesi hakkında daha fazla bilgi için bkz: [sağlama Hdınsight kümeleri](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md).

## <a name="create-an-azure-cosmos-db-database-account"></a>Bir Azure Cosmos DB veritabanı hesabı oluşturma

İlk olarak, aşağıdakileri yaparak grafik API'si ile bir veritabanı hesabı oluşturun:

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <a name="add-a-collection"></a>Koleksiyon ekleme

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-graph.md)]

## <a name="get-apache-tinkerpop"></a>Apache TinkerPop Al

Aşağıdakileri yaparak Apache TinkerPop alın:

1. Hdınsight küme ana düğümünün uzak `ssh tinkerpop3-cosmosdb-demo-ssh.azurehdinsight.net`.

2. TinkerPop3 kaynak kodunu kopyalama, yerel olarak oluşturun ve Maven önbelleğine yükleyin.

    ```bash
    git clone https://github.com/apache/tinkerpop.git
    cd tinkerpop
    mvn clean install
    ```

3. Spark-Gremlin eklentisini yükleme 

    a. Eklenti yüklemesi üzüm tarafından işlenir. Eklenti indirebilirsiniz şekilde üzüm ve bağımlılıklarını depoları bilgileri doldurun. 

      Konumunda mevcut değilse üzümlü yapılandırma dosyası oluşturma `~/.groovy/grapeConfig.xml`. Aşağıdaki ayarları kullanın:

    ```xml
    <ivysettings>
    <settings defaultResolver="downloadGrapes"/>
    <resolvers>
        <chain name="downloadGrapes">
        <filesystem name="cachedGrapes">
            <ivy pattern="${user.home}/.groovy/grapes/[organisation]/[module]/ivy-[revision].xml"/>
            <artifact pattern="${user.home}/.groovy/grapes/[organisation]/[module]/[type]s/[artifact]-[revision].[ext]"/>
        </filesystem>
        <ibiblio name="codehaus" root="http://repository.codehaus.org/" m2compatible="true"/>
        <ibiblio name="central" root="http://central.maven.org/maven2/" m2compatible="true"/>
        <ibiblio name="jitpack" root="https://jitpack.io" m2compatible="true"/>
        <ibiblio name="java.net2" root="http://download.java.net/maven/2/" m2compatible="true"/>
        <ibiblio name="apache-snapshots" root="http://repository.apache.org/snapshots/" m2compatible="true"/>
        <ibiblio name="local" root="file:${user.home}/.m2/repository/" m2compatible="true"/>
        </chain>
    </resolvers>
    </ivysettings>
    ``` 

    b. Gremlin konsolunu başlatın `bin/gremlin.sh`.
        
    c. Spark-Gremlin eklenti sürümüyle yükleyin önceki adımlarda oluşturduğunuz 3.3.0-SNAPSHOT:

    ```bash
    $ bin/gremlin.sh

            \,,,/
            (o o)
    -----oOOo-(3)-oOOo-----
    plugin activated: tinkerpop.server
    plugin activated: tinkerpop.utilities
    plugin activated: tinkerpop.tinkergraph
    gremlin> :install org.apache.tinkerpop spark-gremlin 3.3.0-SNAPSHOT
    ==>loaded: [org.apache.tinkerpop, spark-gremlin, 3.3.0-SNAPSHOT] - restart the console to use [tinkerpop.spark]
    gremlin> :q
    $ bin/gremlin.sh

            \,,,/
            (o o)
    -----oOOo-(3)-oOOo-----
    plugin activated: tinkerpop.server
    plugin activated: tinkerpop.utilities
    plugin activated: tinkerpop.tinkergraph
    gremlin> :plugin use tinkerpop.spark
    ==>tinkerpop.spark activated
    ```

4. Denetleyin olup olmadığını `Hadoop-Gremlin` ile etkinleştirilen `:plugin list`. Devre dışı bırak Spark-Gremlin ile eklenti engelleyebilir çünkü bu eklenti `:plugin unuse tinkerpop.hadoop`.

## <a name="prepare-tinkerpop3-dependencies"></a>TinkerPop3 bağımlılıkları hazırlama

Önceki adımda TinkerPop3 yapılandırıldığında, işlem de tüm jar bağımlılıkları Spark ve Hadoop için hedef dizinde çekilen. HDI ile önceden yüklenmiş ve ek bağımlılıklar yalnızca gerektiğinde isteme Kavanoz kullanın.

1. Gremlin konsol hedef dizininde gidin `tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone`. 

2. Tüm Kavanoz altında taşıma `ext/` için `lib/`: `find ext/ -name '*.jar' -exec mv {} lib/ \;`.

3. Tüm jar kitaplıkları altında Kaldır `lib/` aşağıdaki listede olmayan:

    ```bash
    # TinkerPop3
    gremlin-console-3.3.0-SNAPSHOT.jar
    gremlin-core-3.3.0-SNAPSHOT.jar       
    gremlin-groovy-3.3.0-SNAPSHOT.jar     
    gremlin-shaded-3.3.0-SNAPSHOT.jar     
    hadoop-gremlin-3.3.0-SNAPSHOT.jar     
    spark-gremlin-3.3.0-SNAPSHOT.jar      
    tinkergraph-gremlin-3.3.0-SNAPSHOT.jar

    # Gremlin depedencies
    asm-3.2.jar                                
    avro-1.7.4.jar                             
    caffeine-2.3.1.jar                         
    cglib-2.2.1-v20090111.jar                  
    gbench-0.4.3-groovy-2.4.jar                
    gprof-0.3.1-groovy-2.4.jar                 
    groovy-2.4.9-indy.jar                      
    groovy-2.4.9.jar                           
    groovy-console-2.4.9.jar                   
    groovy-groovysh-2.4.9-indy.jar             
    groovy-json-2.4.9-indy.jar                 
    groovy-jsr223-2.4.9-indy.jar               
    groovy-sql-2.4.9-indy.jar                  
    groovy-swing-2.4.9.jar                     
    groovy-templates-2.4.9.jar                 
    groovy-xml-2.4.9.jar                       
    hadoop-yarn-server-nodemanager-2.7.2.jar   
    hppc-0.7.1.jar                             
    javatuples-1.2.jar                         
    jaxb-impl-2.2.3-1.jar                      
    jbcrypt-0.4.jar                            
    jcabi-log-0.14.jar                         
    jcabi-manifests-1.1.jar                    
    jersey-core-1.9.jar                        
    jersey-guice-1.9.jar                       
    jersey-json-1.9.jar                        
    jettison-1.1.jar                           
    scalatest_2.11-2.2.6.jar                   
    servlet-api-2.5.jar                        
    snakeyaml-1.15.jar                         
    unused-1.0.0.jar                           
    xml-apis-1.3.04.jar                        
    ```

## <a name="get-the-azure-cosmos-db-spark-connector"></a>Azure Cosmos DB Spark Bağlayıcısı'nı al

1. Azure Cosmos DB Spark bağlayıcı almak `azure-cosmosdb-spark-0.0.3-SNAPSHOT.jar` ve Cosmos DB Java SDK'sı `azure-documentdb-1.12.0.jar` gelen [Azure Cosmos DB Spark Bağlayıcısı github'da](https://github.com/Azure/azure-cosmosdb-spark/tree/master/releases/azure-cosmosdb-spark-0.0.3_2.0.2_2.11).

2. Alternatif olarak, onu yerel olarak oluşturabilir. Spark Gremlin en son sürümünü 1.6.1 Spark ile oluşturulmuş ve Azure Cosmos DB Spark bağlayıcıda şu anda kullanılan, Spark ile 2.0.2, uyumlu olmadığından son TinkerPop3 kodu derleyin ve Kavanoz el ile yükleyin. Şunları yapın:

    a. Azure Cosmos DB Spark Bağlayıcısı'nı kopyalayın.

    b. (Önceki adımlarda zaten yapılır) TinkerPop3 oluşturun. Tüm TinkerPop 3.3.0-SNAPSHOT Kavanoz yerel olarak yükleyin.

    ```bash
    mvn install:install-file -Dfile="gremlin-core-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=gremlin-core -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar
    mvn install:install-file -Dfile="gremlin-groovy-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=gremlin-groovy -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    mvn install:install-file -Dfile="gremlin-shaded-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=gremlin-shaded -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    mvn install:install-file -Dfile="hadoop-gremlin-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=hadoop-gremlin -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    mvn install:install-file -Dfile="spark-gremlin-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=spark-gremlin -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    mvn install:install-file -Dfile="tinkergraph-gremlin-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=tinkergraph-gremlin -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    ```

    c. Güncelleştirme `tinkerpop.version` `azure-documentdb-spark/pom.xml` için `3.3.0-SNAPSHOT`.
    
    d. Maven ile oluşturun. Gerekli Kavanoz yerleştirilir `target` ve `target/alternateLocation`.

    ```bash
    git clone https://github.com/Azure/azure-cosmosdb-spark.git
    cd azure-documentdb-spark
    mvn clean package
    ```

3. Yukarıda açıklanan Kavanoz yerel bir dizine kopyalayın ~ / azure-documentdb-spark:

    ```bash
    $ azure-documentdb-spark:
    mkdir ~/azure-documentdb-spark
    cp target/azure-documentdb-spark-0.0.3-SNAPSHOT.jar ~/azure-documentdb-spark
    cp target/alternateLocation/azure-documentdb-1.10.0.jar ~/azure-documentdb-spark
    ```

## <a name="distribute-the-dependencies-to-the-spark-worker-nodes"></a>Spark çalışan düğümlerine bağımlılıkları Dağıt 

1. Grafik verileri dönüştürme TinkerPop3 üzerinde bağımlı olduğundan, tüm Spark çalışan düğümleri için ilgili bağımlılıkları dağıtmanız gerekir.

2. Yukarıda açıklanan Gremlin bağımlılıkları, CosmosDB Spark bağlayıcı jar ve CosmosDB Java SDK'sı aşağıdakileri yaparak çalışan düğümlerine kopyalayın:

    a. İçine tüm Kavanoz kopyalama `~/azure-documentdb-spark`.

    ```bash
    $ /home/sshuser/tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone:
    cp lib/* ~/azure-documentdb-spark
    ```

    b. Ambari panosunda içinde bulabileceğiniz tüm Spark çalışan düğümlerinin listesine al `Spark2 Clients` listesinde `Spark2` bölümü.

    c. Bu dizine her düğümlerin kopyalayın.

    ```bash
    scp -r ~/azure-documentdb-spark sshuser@wn0-cosmos:/home/sshuser
    scp -r ~/azure-documentdb-spark sshuser@wn1-cosmos:/home/sshuser
    ...
    ```
    
## <a name="set-up-the-environment-variables"></a>Ortam değişkenlerini ayarlama

1. Spark kümesi HDP sürümü bulunamıyor. Dizin adı altında olan `/usr/hdp/` (örneğin, 2.5.4.2-7).

2. Tüm düğümler için hdp.Version ayarlayın. Ambari panosunda Git **YARN bölüm** > **yapılandırmalar** > **Gelişmiş**, ve ardından aşağıdakileri yapın: 
 
    a. İçinde `Custom yarn-site`, yeni bir özellik eklemek `hdp.version` ana düğüm HDP sürümünde değerine sahip. 
     
    b. Yapılandırmaları kaydedin. Göz ardı edebilirsiniz uyarıları vardır. 
     
    c. Bildirim simgelerini göstermek gibi YARN ve Oozie hizmetlerini yeniden başlatın.

3. Ana düğümde aşağıdaki ortam değişkenlerini ayarlama (değerleri uygun şekilde değiştirin):

    ```bash
    export HADOOP_GREMLIN_LIBS=/home/sshuser/tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone/ext/spark-gremlin/lib
    export CLASSPATH=$CLASSPATH:$HADOOP_CONF_DIR:/usr/hdp/current/spark2-client/jars/*:/home/sshuser/azure-documentdb-spark/*
    export HDP_VERSION=2.5.4.2-7
    export HADOOP_HOME=${HADOOP_HOME:-/usr/hdp/current/hadoop-client}
    ```

## <a name="prepare-the-graph-configuration"></a>Grafik yapılandırmasını hazırla

1. Azure Cosmos DB bağlantı parametrelerini ve Spark ayarlarına sahip bir yapılandırma dosyası oluşturun ve bunu adresindeki koyun `tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone/conf/hadoop/gremlin-spark.properties`.

    ```
    gremlin.graph=org.apache.tinkerpop.gremlin.hadoop.structure.HadoopGraph
    gremlin.hadoop.jarsInDistributedCache=true
    gremlin.hadoop.defaultGraphComputer=org.apache.tinkerpop.gremlin.spark.process.computer.SparkGraphComputer

    gremlin.hadoop.graphReader=com.microsoft.azure.documentdb.spark.gremlin.DocumentDBInputRDD
    gremlin.hadoop.graphWriter=com.microsoft.azure.documentdb.spark.gremlin.DocumentDBOutputRDD

    ####################################
    # SparkGraphComputer Configuration #
    ####################################
    spark.master=yarn
    spark.executor.memory=3g
    spark.executor.instances=6
    spark.serializer=org.apache.spark.serializer.KryoSerializer
    spark.kryo.registrator=org.apache.tinkerpop.gremlin.spark.structure.io.gryo.GryoRegistrator
    gremlin.spark.persistContext=true

    # Classpath for the driver and executors
    spark.driver.extraClassPath=/usr/hdp/current/spark2-client/jars/*:/home/sshuser/azure-documentdb-spark/*
    spark.executor.extraClassPath=/usr/hdp/current/spark2-client/jars/*:/home/sshuser/azure-documentdb-spark/*
    
    ######################################
    # DocumentDB Spark connector         #
    ######################################
    spark.documentdb.connectionMode=Gateway
    spark.documentdb.schema_samplingratio=1.0
    spark.documentdb.Endpoint=https://FILLIN.documents.azure.com:443/
    spark.documentdb.Masterkey=FILLIN
    spark.documentdb.Database=FILLIN
    spark.documentdb.Collection=FILLIN
    spark.documentdb.preferredRegions=FILLIN
    ```

2. Güncelleştirme `spark.driver.extraClassPath` ve `spark.executor.extraClassPath` önceki adımda, bu durumda dağıtılmış Kavanoz dizininin içerecek şekilde `/home/sshuser/azure-documentdb-spark/*`.

3. Azure Cosmos DB için aşağıdaki ayrıntıları girin:

    ```
    spark.documentdb.Endpoint=https://FILLIN.documents.azure.com:443/
    spark.documentdb.Masterkey=FILLIN
    spark.documentdb.Database=FILLIN
    spark.documentdb.Collection=FILLIN
    # Optional
    #spark.documentdb.preferredRegions=West\ US;West\ US\ 2
    ```
   
## <a name="load-the-tinkerpop-graph-and-save-it-to-azure-cosmos-db"></a>TinkerPop grafik yükleyin ve Azure Cosmos DB kaydedin
Azure Cosmos Veritabanına, bu örnek bir grafik kalıcı hale getirmek nasıl yapılacağını göstermek üzere kullandığı TinkerPop TinkerPop modern grafik önceden tanımlanmış. Grafik Kryo biçiminde depolanır ve TinkerPop depoya sağlanır.

1. Gremlin YARN modunda çalıştırdığınız için grafik verileri Hadoop dosya sistemi içinde kullanılabilir olmanız gerekir. Bir dizin oluşturun ve yerel grafik dosyası içine kopyalamak için aşağıdaki komutları kullanın. 

    ```bash
    $ tinkerpop:
    hadoop fs -mkdir /graphData
    hadoop fs -copyFromLocal ~/tinkerpop/data/tinkerpop-modern.kryo /graphData/tinkerpop-modern.kryo
    ```

2. Geçici güncelleştirme `gremlin-spark.properties` kullanmak üzere bir dosya `GryoInputFormat` Grafiği okunamıyor. Ayrıca belirtmek `inputLocation` dizin size, aşağıdaki gibi oluşturun:

    ```
    gremlin.hadoop.graphReader=org.apache.tinkerpop.gremlin.hadoop.structure.io.gryo.GryoInputFormat
    gremlin.hadoop.inputLocation=/graphData/tinkerpop-modern.kryo
    ```

3. Gremlin konsolunu başlatın ve ardından veri yapılandırılmış Azure Cosmos DB koleksiyona kalıcı hale getirmek için aşağıdaki hesaplama adımları oluşturun:  

    a. Grafik oluşturma `graph = GraphFactory.open("conf/hadoop/gremlin-spark.properties")`.

    b. Yazma için SparkGraphComputer kullanmak `graph.compute(SparkGraphComputer.class).result(GraphComputer.ResultGraph.NEW).persist(GraphComputer.Persist.EDGES).program(TraversalVertexProgram.build().traversal(graph.traversal().withComputer(Computer.compute(SparkGraphComputer.class)),"gremlin-groovy","g.V()").create(graph)).submit().get()`.

    ```bash
    gremlin> graph = GraphFactory.open("conf/hadoop/gremlin-spark.properties")
    ==>hadoopgraph[gryoinputformat->documentdboutputrdd]
    gremlin> hg = graph.
                compute(SparkGraphComputer.class).
                result(GraphComputer.ResultGraph.NEW).
                persist(GraphComputer.Persist.EDGES).
                program(TraversalVertexProgram.build().
                    traversal(graph.traversal().withComputer(Computer.compute(SparkGraphComputer.class)), "gremlin-groovy", "g.V()").
                    create(graph)).
                submit().
                get() 
    ==>result[hadoopgraph[documentdbinputrdd->documentdboutputrdd],memory[size:1]]
    ```

4. Veri Gezgini'nden verileri Azure Cosmos Veritabanına kalıcı olduğunu doğrulayabilirsiniz.

## <a name="load-the-graph-from-azure-cosmos-db-and-run-gremlin-queries"></a>Grafik Azure Cosmos DB'den yükleme ve Gremlin sorgular çalıştırın

1. Grafik yüklemek için düzenleme `gremlin-spark.properties` ayarlamak için `graphReader` için `DocumentDBInputRDD`:

    ```
    gremlin.hadoop.graphReader=com.microsoft.azure.documentdb.spark.gremlin.DocumentDBInputRDD
    ```

2. Grafik yükleme, verileri çapraz geçiş ve Gremlin sorguları ile aşağıdakileri yaparak çalıştırın:

    a. Gremlin Konsolu'ndan `bin/gremlin.sh`.

    b. Grafik yapılandırmayla oluşturmak `graph = GraphFactory.open('conf/hadoop/gremlin-spark.properties')`.

    c. Grafik geçişi ile SparkGraphComputer oluşturma `g = graph.traversal().withComputer(SparkGraphComputer)`.

    d. Aşağıdaki Gremlin grafik sorgular çalıştırın:

    ```bash
    gremlin> graph = GraphFactory.open("conf/hadoop/gremlin-spark.properties")
    ==>hadoopgraph[documentdbinputrdd->documentdboutputrdd]
    gremlin> g = graph.traversal().withComputer(SparkGraphComputer)
    ==>graphtraversalsource[hadoopgraph[documentdbinputrdd->documentdboutputrdd], sparkgraphcomputer]
    gremlin> g.V().count()
    ==>6
    gremlin> g.E().count()
    ==>6
    gremlin> g.V(1).out().values('name')
    ==>josh
    ==>vadas
    ==>lop
    gremlin> g.V().hasLabel('person').coalesce(values('nickname'), values('name'))
    ==>josh
    ==>peter
    ==>vadas
    ==>marko
    gremlin> g.V().hasLabel('person').
            choose(values('name')).
                option('marko', values('age')).
                option('josh', values('name')).
                option('vadas', valueMap()).
                option('peter', label())
    ==>josh
    ==>person
    ==>[name:[vadas],age:[27]]
    ==>29
    ```

> [!NOTE]
> Daha ayrıntılı günlük kaydı için günlük düzeyde kümesi `conf/log4j-console.properties` daha fazla ayrıntı düzeyi.
>

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıç makalede, Azure Cosmos DB ve Spark birleştirerek grafikleri ile çalışmaya nasıl öğrendiniz.

> [!div class="nextstepaction"]
